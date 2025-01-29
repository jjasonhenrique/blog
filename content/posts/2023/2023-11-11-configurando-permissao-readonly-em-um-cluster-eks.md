---
title: "Configurando permissão somente leitura em um cluster EKS"
author: Jason
date: 2023-11-11
tags: [ "cloud", "AWS" ]
type: post
weight: 18
showTableOfContents: true
---
O objetivo desse post é demonstrar como fazer a configuração para que
uma determinada role da AWS tenha permissão de somente leitura dentro de
um cluster EKS usando o kubectl que é um utilitário para administrar o
Kubernetes através de linha de comando.

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Para conseguir seguir com essa demosntração será necessário os seguintes
pré requisitos abaixo:

-   Conta na AWS.
-   Um cluster eks já criado. Para fazer a criação de um cluster EKS
    você pode usar esse
    [artigo](https://jjasonhenrique.me/2023/01/28/usando-eksctl-para-criacao-e-gerenciamento-de-um-cluster-eks/)
    que falei sobre o eksctl.
-   Kubectl instalado. Mais informações nesse
    [link](https://kubernetes.io/pt-br/docs/tasks/tools/install-kubectl-linux/).
-   AWS CLI instalado. Mais informações nesse
    [link](https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html).

**Lembrando que essa demonstração terá custo na AWS.**

## Criação de user e role AWS {#criação-de-user-e-role-aws .wp-block-heading .has-x-large-font-size}

Primeiramente devemos criar um usuário iam aws sem nenhuma permissão
conforme esse
[link](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_users_create.html)
lembrando que esse usuário deve ter acesso via CLI.

Antes da criação da role é necessário a criação de uma policy que tenha
apenas permissão de list e read no EKS conforme abaixo:

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "eks:ListNodegroups",
                "eks:ListEksAnywhereSubscriptions",
                "eks:DescribeFargateProfile",
                "eks:DescribeAddonConfiguration",
                "eks:ListTagsForResource",
                "eks:ListAddons",
                "eks:DescribeEksAnywhereSubscription",
                "eks:DescribeAddon",
                "eks:ListFargateProfiles",
                "eks:DescribeNodegroup",
                "eks:DescribeIdentityProviderConfig",
                "eks:ListUpdates",
                "eks:DescribeUpdate",
                "eks:AccessKubernetesApi",
                "eks:DescribeCluster",
                "eks:ListClusters",
                "eks:DescribeAddonVersions",
                "eks:ListIdentityProviderConfigs"
            ],
            "Resource": "*"
        }
    ]
}
```

Agora vamos criar uma role com a policy acima anexada. Outro ponto de
atenção é que devemos criar uma relação de confiança para que o usuário
iam criado anteriormente possa fazer o assume role. Abaixo segue como
deverá ser configurado essa relação de confiança:

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::id-conta:user/jason"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}
```

Onde **id-conta** é a a informação da sua conta.

Para mais informações sobre essas configurações acessar esse
[link](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_roles_create_for-user.html).

## Alteração arquivo aws-auth {#alteração-arquivo-aws-auth .wp-block-heading .has-x-large-font-size}

O primeiro passo que devemos fazer é a inclusão da role AWS no configmap
aws-auth dentro do cluster EKS. Para isso vamos usar uma role com
permissões administrativas. Para isso vamos executar o seguinte comando
para edição desse configmap:

``` wp-block-code
eksctl create iamidentitymapping --cluster nome-cluster --region=us-east-1 \
    --arn arn:aws:iam::id-conta:role/eks --username {{SessionName}} --group system:masters
```

Para verificacao podemos executar o comando abaixo:

``` wp-block-code
eksctl get iamidentitymapping --cluster nome-cluster --region=us-east-1
```

Essa configuração também pode ser feita manualmente conforme informado
nesse
[link](https://docs.aws.amazon.com/pt_br/eks/latest/userguide/add-user-role.html).

## Configuração de Profile AWS {#configuração-de-profile-aws .wp-block-heading .has-x-large-font-size}

Nesse passo vamos fazer a configuração das credenciais do usuário iam,
criação de um novo profile referenciando a role criada e acertar a
configuração do kubeconfig.

Primeiramente vamos configurar o usuário iam criado usando o comando
abaixo onde será solicitado o access key, secret access key, region e
output format:

``` wp-block-code
aws configure
```

Essas informações pode ser consultadas acessando o arquivo
.aws/credentials. Será exibido uma saída similar a abaixo:

``` wp-block-code
[default]
aws_access_key_id = XXXXXX
aws_secret_access_key = XXXXXX
```

Depois vamos configurar um profile no arquivo .aws/config

``` wp-block-code
[profile eks]
role_arn = arn:aws:iam::id-conta:role/eks
source_profile = default
```

Onde **source_profile** deverá ser informado o nome cadastrado no
arquivo .aws/credentials

Agora vamos acertar o kubeconfig referenciado o profile criado. A seção
de users do arquivo kubeconfig deve ser incluido o argumento --profile
eks conforme abaixo:

``` wp-block-code
users:
- name: eksctl@cluster-demoeksctl.us-east-1.eksctl.io
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - eks
      - get-token
      - --output
      - json
      - --cluster-name
      - cluster-demoeksctl
      - --region
      - us-east-1
      - --profile
      - eks
      command: aws
      env:
      - name: AWS_STS_REGIONAL_ENDPOINTS
        value: regional
      provideClusterInfo: false
```

## Configuração de permissão somente leitura no Kubernetes {#configuração-de-permissão-somente-leitura-no-kubernetes .wp-block-heading .has-x-large-font-size}

Após a configuração do item anterior foi verificado que mesmo usando um
role AWS com perfil apenas de leitura ainda era possível alteração de
recursos usando kubectl.

Foi necessário a criação de um clusterrole, clusterrolebinding e um
group com permissões de list, get e watch em todos os recursos do
kubernetes conforme abaxio:

``` wp-block-code
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: developers-readonly
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["get", "list", "watch"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: developers-readonly
subjects:
- kind: Group
  name: developers-readonly
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: developers-readonly
  apiGroup: rbac.authorization.k8s.io
```

Aplicar o conteúdo acima criando um arquivo chamado readonly.yaml e
executar o comando abaixo:

``` wp-block-code
kubectl apply -f readonly.yaml
```

Após isso se faz necessário a remoção da entrada abaixo no configmap
aws-auth:

``` wp-block-code
eksctl delete iamidentitymapping --cluster nome-cluster --region=us-east-1 --arn arn:aws:iam::id-conta:role/eks --all
```

Com um usuário com permissão administrativa verificar se a role AWS foi
removida:

``` wp-block-code
eksctl get iamidentitymapping --cluster nome-cluster --region=us-east-1
```

Depois adicionar a role AWS com o Group criado anteriormente junto com
clusterrole e clusterrolebinding executando o comando abaixo:

``` wp-block-code
eksctl create iamidentitymapping --cluster nome-cluster --region=us-east-1 --arn arn:aws:iam::id-conta:role/eks --username {{SessionName}} --group developers-readonly
```

## Validação de acesso {#validação-de-acesso .wp-block-heading .has-x-large-font-size}

Agora para validar se a role AWS está realmente com acesso somente
leitura tentamos criar um secret no cluster usando o comando abaixo:

``` wp-block-code
kubectl create secret generic teste --from-literal=user=jason
```

Deve ser apresentado o seguinte erro conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=1024"
class="wp-image-508" data-attachment-id="508"
data-permalink="https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/screenshot-from-2023-11-11-16-51-37/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png"
data-orig-size="1529,93" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-11-11-16-51-37"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-51-37.png 1529w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="62" />
</figure>

Depois tentamos listar todos os pods executando no cluster e obtivemos
sucesso conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=778"
class="wp-image-510" data-attachment-id="510"
data-permalink="https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/screenshot-from-2023-11-11-16-53-04/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png"
data-orig-size="778,170" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-11-11-16-53-04"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=778"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png 778w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-11-16-53-04.png?w=768 768w"
sizes="(max-width: 778px) 100vw, 778px" width="778" height="170" />
</figure>

Com isso garantimos que o acesso está somente leitura aos objectos do
Kubernetes.

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objectivo desse artigo foi demonstar como conceder permissão somente
leitura aos recursos do Kubernetes vinculando com uma role AWS.

Outro ponto que para conceder permissão somente leitura podemos
implementar uma solução de interface gráfica que mostre todos os
recursos do cluster EKS como por o Kubernetes Dashboard.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a79dd6394c}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-418" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-418" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-418" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-418-679a79dd641a5 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=418&origin=jjasonhenrique.wordpress.com&obj_id=205418077-418-679a79dd641a5&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-418-679a79dd641a5" data-title="Curtir ou reblogar"}
::: {.likes-widget-placeholder .post-likes-widget-placeholder style="height: 55px;"}
[Curtir]{.button} [Carregando...]{.loading}
:::

[]{.sd-text-color}[]{.sd-link-color}
::::
::::::::
::::::::::

:::::::::: wp-block-template-part
::::::::: {.wp-block-group .is-layout-constrained .wp-block-group-is-layout-constrained}
:::::::: {.wp-block-group .post-meta .is-layout-flex .wp-block-group-is-layout-flex}
:::: {.is-style-post-author-icon .wp-block-post-author style="font-size:var(--wp--custom--font-sizes--x-small);"}
::: wp-block-post-author__content
Jason Henrique
:::
::::

::: {.is-style-post-date-icon .wp-block-post-date style="font-size:var(--wp--custom--font-sizes--x-small);"}
[11 de novembro de
2023](https://jjasonhenrique.me/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[EKS](https://jjasonhenrique.me/category/eks/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}
:::
::::::::
:::::::::
::::::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

::::::::::::: {.wp-block-group .is-layout-constrained .wp-container-core-group-is-layout-6 .wp-block-group-is-layout-constrained style="padding-top:30px;padding-right:20px;padding-bottom:0px;padding-left:20px"}
::::::: {.wp-block-columns .alignwide .next-prev-links .is-layout-flex .wp-container-core-columns-is-layout-1 .wp-block-columns-is-layout-flex}
:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-previous .wp-block-post-navigation-link}
[←]{.wp-block-post-navigation-link__arrow-previous .is-arrow-arrow
aria-hidden="true"}[Conhecendo a
ferramenta Trivy](https://jjasonhenrique.me/2023/11/05/conhecendo-a-ferramenta-trivy/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Criando um cluster local de kubernetes com
o kind](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/11/11/configurando-permissao-readonly-em-um-cluster-eks/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

::: verbum-form-meta
:::
