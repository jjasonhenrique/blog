---
title: "Criando uma policy do tipo Mutation com o kyverno"
author: Jason
date: 2023-11-02
tags: [ "cloud", "AWS" ]
type: post
weight: 19
showTableOfContents: true
---
O objectivo dese artigo e a criação de um policy do tipo Mutation no
kyverno. Esse artigo é uma continuação do primeiro artigo onde falei
como instalar o kyverno e criei uma policy do tipo validation no
kyverno. Para ver o primeiro artigo basta clicar nesse
[link](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/).

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Para conseguir criar uma policy do tipo Mutation no kyverno você
necessitará dos seguintes pré requisitos:

-   Ter um cluster kubernetes
-   Ter o kubectl instalado
-   Ter o kyverno ja instalado no seu cluster

Para a instalação do kyverno você também pode consultar o primeiro
artigo que falei sobre o kyverno.

## Testando uma policy do tipo Mutation {#testando-uma-policy-do-tipo-mutation .wp-block-heading .has-x-large-font-size}

Para testar uma policy do tipo mutation no kyverno vamos criar uma
clusterpolicy que força a aplicação da label **environment: develop**
conforme abaixo:

``` wp-block-code
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-labels
spec:
  rules:
  - name: add-labels
    match:
      any:
      - resources:
          kinds:
          - Deployment
    mutate:
      patchStrategicMerge:
        metadata:
          labels:
            environment: develop
        spec:
          template:
            metadata:
              labels:
                environment: develop
  validationFailureAction: Enforce
```

Para aplicar a clusterpolicy criar um arquivo chamado add-labels.yaml
com o conteúdo acima e executar o comando abaixo:

``` wp-block-code
kubectl apply -f add-labels.yaml
```

Para validar a criação da cluster policy executar o comando abaixo:

``` wp-block-code
kubectl get clusterpolicy
```

Agora vamos criar um deployment sem a label **environment: develop**
conforme abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
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
      labels:
        app: app
        owner: jason
    spec:
      containers:
      - image: nginx
        name: nginx
```

Para aplicar o deployment criar um arquivo chamado nginx.yaml e executar
o comando abaixo:

``` wp-block-code
kubectl apply -f nginx.yaml
```

Para visualizar se a label foi aplicada você pode executar os comando
abaixos:

``` wp-block-code
kubectl get deploy -o jsonpath='{.items[0].metadata.labels}' -n app

kubectl get deploy -o jsonpath='{.items[0].spec.template.metadata.labels}' -n app
```

Deve retornar as labels conforme abaixo:

``` wp-block-code
{"app":"app","environment":"develop"}

{"app":"app","environment":"develop","owner":"jason"}
```

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objectivo desse artigo foi demonstrar a utilizacao de uma policy
mutation no kyverno. Existem outras policies que podem ser criadas
consultando a documentação do kyverno através desse
[link](https://kyverno.io/policies/).

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a7bf3877b4}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-406" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-406" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-406" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-406-679a7bf387e56 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=406&origin=jjasonhenrique.wordpress.com&obj_id=205418077-406-679a7bf387e56&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-406-679a7bf387e56" data-title="Curtir ou reblogar"}
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
[2 de novembro de
2023](https://jjasonhenrique.me/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[kyverno](https://jjasonhenrique.me/category/kyverno/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[kyverno](https://jjasonhenrique.me/tag/kyverno/){rel="tag"}[,
]{.wp-block-post-terms__separator}[policy](https://jjasonhenrique.me/tag/policy/){rel="tag"}
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
aria-hidden="true"}[Conhecendo um pouco sobre
o kyverno](https://jjasonhenrique.me/2023/10/29/conhecendo-um-pouco-sobre-o-kyverno/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Conhecendo a
ferramenta Trivy](https://jjasonhenrique.me/2023/11/05/conhecendo-a-ferramenta-trivy/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/11/02/criando-uma-policy-do-tipo-mutation-com-o-kyverno/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::
