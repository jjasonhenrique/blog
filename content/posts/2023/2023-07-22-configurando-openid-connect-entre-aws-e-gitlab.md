---
title: "Configurando OpenID Connect entre AWS e GitLab"
author: Jason
date: 2023-07-22
tags: [ "cloud", "AWS" ]
type: post
weight: 21
showTableOfContents: true
---
O objetivo desse post é demonstrar como configurar o OpenID Connect
entre a sua conta AWS e sua conta do GitLab para executar uma pipeline
usando uma role da sua conta AWS.

Primeiramente o OpenID Connect (OIDC) permite você recupere tokens de
curta duração para se comunicar com seu Cloud Provider nesse caso vamos
utilizar a AWS mas também pode ser utilizado para Azure e GCP.

A grande vantagem dessa integração entre AWS e GitLab usando OpenID
Connect é a segurança já que é gerado um token de curta duração e não
será necessário a criação de um **Access Key** e de um **Secret Access
Key**. Será utilizado o conceito de role da AWS e essa role será
executada apenas em um determinado repositório do GitLab.

**Pré-Requisitos**

Para prosseguir com esse tutorial é necessário ter os seguintes
pré-requisitos:

-   Conta na AWS;
-   Conta no GitLab SaaS utilizando esse
    [lin](https://about.gitlab.com/){target="_blank"
    rel="noreferrer noopener"}[k](https://about.gitlab.com/);

**Configuração OIDC**

No primeiro passo vamos configurar o OIDC na AWS mas especificamente no
IAM. Você deve ir no serviço **IAM** e ir na opção **Identity
providers** deve mostrar a tela abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=1024"
class="wp-image-341" data-attachment-id="341"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-21/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png"
data-orig-size="1920,482" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-21" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-21.png 1920w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="257" />
</figure>

Clicando na opção **Add provider** será informado a tela abaixo onde
vamos iniciar a configuração do OIDC:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=937"
class="wp-image-344" data-attachment-id="344"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-23/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png"
data-orig-size="937,782" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-23" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=937"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png 937w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-23.png?w=768 768w"
sizes="(max-width: 937px) 100vw, 937px" width="937" height="782" />
</figure>

Deve ser selecionado a opção OpenID Connect. Nos campos **Provider URL**
e **Audience** devem ser configurado a URL <https://gitlab.com> conforme
imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=848"
class="wp-image-345" data-attachment-id="345"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-24/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png"
data-orig-size="848,767" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-24" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=848"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png 848w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-24.png?w=768 768w"
sizes="(max-width: 848px) 100vw, 848px" width="848" height="767" />
</figure>

Você deve clicar na opção **Get thumbprint** onde será gerado um
certificado thumbprint que irá validar a CA do OIDC Provider no nosso
caso do GitLab e depois clicar na opção **Add provider** e com isso
finalizamos a configuração do OIDC. Será criado um provider conforme
imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=1024"
class="wp-image-347" data-attachment-id="347"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-25/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png"
data-orig-size="1686,357" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-25" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=1020 1020w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-25.png 1686w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="216" />
</figure>

**Criação Role AWS**

No segundo passo vamos criar uma role na AWS indo na opção **IAM** e
depois **Roles** (Funções em Português)e depois na opção **Create Role**
deverá mostrar uma tela similar a abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=1024"
class="wp-image-349" data-attachment-id="349"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-26/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png"
data-orig-size="1855,602" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-26" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-26.png 1855w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="332" />
</figure>

Deverá selecionar a opção **Web Identity** e selecionar o **Identity
provider** e o **Audience** criado no passo anterior conforme abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=1024"
class="wp-image-351" data-attachment-id="351"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-27/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png"
data-orig-size="1881,603" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-27" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-27.png 1881w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="328" />
</figure>

Depois vamos para o step 2 onde vamos selecionar as permissões dessa
role. Nesse caso vamos usar a politica
[AmazonS3ReadOnlyAccess](https://us-east-1.console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess){rel="noreferrer noopener"
target="_blank"} para somente listar os buckets s3 de uma conta na AWS.

Depois vamos para step 3 onde vamos apenas colocar o nome da Role e
clicar na opção **Create role**.

Depois da criação da Role vamos editar a relação de confiança dessa role
para era ser menos permissiva. Abaixo segue a trust policy dessa role:

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::aws_account_id:oidc-provider/gitlab.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "gitlab.com:sub": "project_path:user_gitlab/name_repository:ref_type:branch:ref:main"
                }
            }
        }
    ]
}
```

Onde:

-   **aws_account_id** é o id da sua conta da AWS;
-   **user_gitlab** é o nome do usuário utilizado no gitlab. Também
    aceita como opção um projeto do GitLab;
-   **name_repository** é o nome do repository criado no gitlab;

**Configuração GitLab**

No terceiro passo vamos criar um repositório no gitlab, criar uma
pipeline simples que faça o assumerole na role que criamos no passo
anterior e que liste os buckets S3 da conta AWS

Criar um repositório no gitlab e adicionar a seguinte variável de
ambiente indo na opção Settings \>\> CI/CD \>\> Variables. Deverá
mostrar uma tela similar a abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=776"
class="wp-image-365" data-attachment-id="365"
data-permalink="https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/image-29/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png"
data-orig-size="776,555" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-29" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=776"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png 776w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-29.png?w=768 768w"
sizes="(max-width: 776px) 100vw, 776px" width="776" height="555" />
</figure>

onde:

-   **aws_account_id** é o id da sua conta da AWS;
-   **name_role** é o nome da role criada na AWS;

Esse valor também pode ser pego diretamente na role criado na AWS no
campo ARN.

Após isso criar o arquivo .gitlab-ci.yml com o conteúdo abaixo:

``` wp-block-code
image:
  name: "amazon/aws-cli:latest"
  entrypoint:
    - ""

stages:
  - "assume-role"

job_assume_role:
    stage: "assume-role"
    script: 
      - >
        STS=($(aws sts assume-role-with-web-identity
        --role-arn ${ROLE_ARN}
        --role-session-name "GitLabRunner-${CI_PROJECT_ID}-${CI_PIPELINE_ID}"
        --web-identity-token ${CI_JOB_JWT_V2}
        --duration-seconds 3600
        --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]'
        --output text))
      - export AWS_ACCESS_KEY_ID="${STS[0]}"
      - export AWS_SECRET_ACCESS_KEY="${STS[1]}"
      - export AWS_SESSION_TOKEN="${STS[2]}"
      - aws sts get-caller-identity
      - aws s3 ls
```

Com isso será executado uma simples pipeline que lista os buckets S3 da
conta AWS.

**Conclusão**

O Objetivo desse post foi demonstrar a configuração do OpenID Connect
entre AWS e GitLab. Esse mesmo procedimento pode ser efetuado usando
github conforme esse
[link](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services){target="_blank"
rel="noreferrer noopener"}.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a7df1a33b2}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-332" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-332" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-332" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-332-679a7df1a3cc6 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=332&origin=jjasonhenrique.wordpress.com&obj_id=205418077-332-679a7df1a3cc6&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-332-679a7df1a3cc6" data-title="Curtir ou reblogar"}
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
[23 de fevereiro de
2023](https://jjasonhenrique.me/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[GitLab](https://jjasonhenrique.me/category/gitlab/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/category/security/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[GitLab](https://jjasonhenrique.me/tag/gitlab/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/tag/security/){rel="tag"}
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
aria-hidden="true"}[Testando o recurso Synthetics Canaries
do Cloudwatch](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Conhecendo um pouco sobre
o kyverno](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/02/23/configurando-openid-connect-entre-aws-e-gitlab/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

::: verbum-form-meta
