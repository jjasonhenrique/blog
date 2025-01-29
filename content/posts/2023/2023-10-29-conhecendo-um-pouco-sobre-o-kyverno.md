---
title: "Conhecendo um pouco sobre o kyverno"
author: Jason
date: 2023-10-29
tags: [ "cloud", "AWS" ]
type: post
weight: 20
showTableOfContents: true
---
O Objectivo desse artigo é demonstrar a utilização da ferramenta kyverno
dentro de um ambiente kubernetes. O kyverno é um dynamic admission
controller dentro do kubernetes que permite voce aplicar políticas
dentro do seu cluster.

Nos links abaixo você pode ter mais informaçõoes dessa ferramenta:

-   <https://kyverno.io/>
-   <https://github.com/kyverno/kyverno/>

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Para testar o kyverno segue abaixo os pré requisitos:

-   Ter um cluster kubernetes para isso você pode usar o
    [kind](https://kind.sigs.k8s.io/) ou [rancher
    desktop](https://rancherdesktop.io/).
-   Ter o kubectl instalado mais informações nesse
    [link](https://kubernetes.io/docs/tasks/tools/).
-   Ter o helm instalado mais informações nesse
    [link](https://helm.sh/docs/intro/install/).

## Instalação {#instalação .wp-block-heading .has-x-large-font-size}

Para a instalação do kyverno você pode fazer de duas maneiras uma usando
o helm e outra aplicando um arquivo de manifesto do kubernetes usando o
comando kubectl.

Em ambas as formas de instalação e recomendado a criação de um namespace
para o kyverno usando o comando abaixo:

``` wp-block-code
kubectl create ns kyverno
```

## Instalação usando Helm {#instalação-usando-helm .wp-block-heading .has-large-font-size}

Para fazer a instalação do kyverno usando o helm devem ser executados os
comandos abaixo:

``` wp-block-code
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
helm install kyverno-policies kyverno/kyverno-policies -n kyverno
```

## Instalação usando kubectl {#instalação-usando-kubectl .wp-block-heading .has-large-font-size}

Para a instalação usando o kubectl deve ser executado o comando abaixo:

``` wp-block-code
kubectl create -f https://github.com/kyverno/kyverno/releases/download/v1.10.0/install.yaml -n kyverno
```

## Conceitos Kyverno {#conceitos-kyverno .wp-block-heading .has-x-large-font-size}

Após a instalação do kyverno você terá disponível dois tipos de kind
para a criação de suas políticas:

-   **ClusterPolicy** -- que pode ser aplicado a todo o cluster
    kubernetes.
-   **Policy** -- que pode ser aplicado a um determinado namespace.

Também haverá dois tipos de políticas que poderao ser criadas:

-   **Validation** -- onde nessa política será validado algo por exemplo
    se há determinada label em um deployment.
-   **Mutation** -- onde sera alterado determinada configuração dentro
    de um deployment

Dentro desse dois tipos de políticas haverá duas ações que poderão ser
feitas:

-   **Audit** -- onde apenas será logado se determinado deployment esta
    seguindo ou nao determinada política do kyverno.
-   **Enforce** -- caso um deployment não atenda determinada política do
    kyverno o mesmo não será criado.

## Testando uma policy do tipo Validation {#testando-uma-policy-do-tipo-validation .wp-block-heading .has-x-large-font-size}

Para testar uma policy do tipo validation vamos criar a ClusterPolcy
abaixo que valida se existe as labels owner e environment. Em um
primeiro momento usando a ação **Audit** e depois a ação **Enforce**.

``` wp-block-code
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-labels
  annotations:
    policies.kyverno.io/title: Require Labels
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/minversion: 1.6.0
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod, Label
    policies.kyverno.io/description: >-
      This policy validates that the label `owner` and `environment` is specified with some value.      
spec:
  validationFailureAction: Audit
  background: true
  rules:
  - name: check-for-labels
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "The label `owner` and `environment` is required."
      pattern:
        metadata:
          labels:
            owner: "?*"
            environment: "?*"
```

Para aplicar a clusterpolicy acima criar um arquivo chamado
**require-labels.yaml** e executar o comando abaixo:

``` wp-block-code
kubectl apply -f require-labels.yaml
```

Para verificar se a política foi criada executar o comando abaixo:

``` wp-block-code
kubectl get clusterpolicy
```

Deve apresentar uma saída similar com a imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png?w=625"
class="wp-image-395" data-attachment-id="395"
data-permalink="https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/screenshot-from-2023-10-29-11-26-25/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png"
data-orig-size="625,44" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-10-29-11-26-25"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png?w=625"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png 625w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-26-25.png?w=300 300w"
sizes="(max-width: 625px) 100vw, 625px" width="625" height="44" />
</figure>

Para testar vamos criar um arquivo de deployment sem as labels owner e
environment conforme o arquivo abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: app
  name: app
  namespace: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: app
    spec:
      containers:
      - image: nginx
        name: nginx
```

Para aplicar o deployment acima basta criar o arquivo nginx.yaml e
executar o comando abaixo:

``` wp-block-code
kubectl apply -f nginx.yaml
```

Para visualizar se o deployment atende ou não a policy criada podemos
executar os seguintes comandos abaixo:

``` wp-block-code
kubectl describe clusterpolicy require-labels
kubectl get events -n app
```

Onde **app** e o namespace onde o deployment foi aplicado. Nos eventos
devera aparecer uma saída similar com a imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=1024"
class="wp-image-396" data-attachment-id="396"
data-permalink="https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/screenshot-from-2023-10-29-11-44-14/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png"
data-orig-size="1908,93" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-10-29-11-44-14"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=1005 1005w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-44-14.png 1908w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="49" />
</figure>

Agora vamos alterar a clusterpolicy para que seja obrigatório a presença
das labels owner e environment. Caso essas labels não estejam presentes
o deployment não sera criado.

Antes de alterar a clusterpolicy excluir o deployment usando o comando
abaixo:

``` wp-block-code
kubectl delete -f nginx.yaml
```

Para alterar a clusterpolicy para **Enforce** voçê deve alterar a
configuração abaixo de Audit para Enforce conforme abaixo:

``` wp-block-code
spec:
  validationFailureAction: Enforce
```

Depois disso salvar o arquivo e aplicar novamente a policy usando o
comando abaixo:

``` wp-block-code
kubectl apply -f require-labels.yaml
```

Após isso tentar aplicar o deployment novamente usando o comando
kubectl. Deverá aparecer a seguinte mensagem de erro conforme imagem
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=1024"
class="wp-image-401" data-attachment-id="401"
data-permalink="https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/screenshot-from-2023-10-29-11-53-48/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png"
data-orig-size="1070,151" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-10-29-11-53-48"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=1020 1020w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/10/screenshot-from-2023-10-29-11-53-48.png 1070w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="144" />
</figure>

Para resolver essa mensagem de erro deve ser inserido as labels owner e
environment conforme abaixo:

``` wp-block-code
  template:
    metadata:
      labels:
        app: app
        owner: jason
        environment: develop
```

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O Objectivo desse artigo foi demonstar a utilização da ferramenta
Kyverno. Num proximo artigo vou demonstar a utilização do kyverno usando
uma policy do tipo Mutation.

Caso queira se aprofundar na ferramenta os links abaixo podem te ajudar:

-   <https://playground.kyverno.io/#/>
-   <https://kyverno.io/resources/>
-   <https://kyverno.io/policies/>

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a7ce09c6bf}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-377" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-377" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-377" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-377-679a7ce09cf1e .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=377&origin=jjasonhenrique.wordpress.com&obj_id=205418077-377-679a7ce09cf1e&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-377-679a7ce09cf1e" data-title="Curtir ou reblogar"}
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


