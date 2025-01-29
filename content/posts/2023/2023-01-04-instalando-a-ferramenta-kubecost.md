---
title: "Instalando a ferramenta kubecost"
author: Jason
date: 2023-01-04
tags: [ "cloud", "AWS" ]
type: post
weight: 27
showTableOfContents: true
---
O intuito deste post é demostrar a instalação do kubecost uma ferramenta
de monitoramento e gerenciamento de custos para um ambiente kubernetes e
também para cloud suportando os principais cloud providers (AWS, Azure e
GCP).

O kubecost possui dois tipos de licença (free e paga). Na versão free as
informações coletadas ficam disponíveis apenas 15 dias.

Para mais informações sobre kubecost acessar os links abaixo:

-   [https://docs.kubecost.com/](https://docs.kubecost.com/){rel="noreferrer noopener"
    target="_blank"}
-   [https://github.com/kubecost/cost-analyzer-helm-chart](https://github.com/kubecost/cost-analyzer-helm-chart){rel="noreferrer noopener"
    target="_blank"}

## Pré-Requisitos {#5f66 .wp-block-heading .has-large-font-size}

Como pré-requisito é necessário ter um cluster de kubernetes para a
instalação do kubecost. Para essa demonstração pode ser utilizado o kind
ou o rancher desktop.

Para mais informações sobre a instalação e criação de um cluster usando
kind utilizar
esse [link](https://kind.sigs.k8s.io/){rel="noreferrer noopener"
target="_blank"}.

Para informações sobre o rancher desktop acesse
esse [link](https://rancherdesktop.io/){rel="noreferrer noopener"
target="_blank"}.

## Instalação {#e17b .wp-block-heading .has-large-font-size}

No meu cenário como é um ambiente de teste vou fazer a instalação
utilizando o kubectl mas a instalação também pode ser feita via helm.

Criar um namespace chamado kubecost onde será deployado a aplicação.
Segue abaixo comando:

``` wp-block-code
kucebctl create ns kubecost
```

Executar o comando abaixo para instalação:

``` wp-block-code
kubectl apply -f https://raw.githubusercontent.com/kubecost/cost-analyzer-helm-chart/master/kubecost.yaml -n kubecost
```

Será deployado os seguintes objetos conforme imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/e72cb-1tjn4mebjd2zgo-ifwwghqq.png" />
</figure>

Abaixo overview dos objetos deployados pelo kubecost

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/109e0-1tmllj-lwa6uev-p46spieg.png" />
</figure>

Na instalação default é efetuado a instalação de um deploy do
prometheus-server que será responsável por coletar as informações do
cluster. É possivel utilizar seu próprio servidor do prometheus porém é
recomendavel a utilização desse prometheus apartado.

## Acessando a console do kubecost {#1a98 .wp-block-heading .has-large-font-size}

Para acessar a console do kubecost será utilizado o comando kubectl
port-forward para testar localmente. Segue abaixo comando:

``` wp-block-code
 kubectl port-forward service/kubecost-cost-analyzer 9090:9090 -n kubecost
```

Abaixo alguns prints da console do kubecost

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/e7c42-1qhz4rrf2aohn1w6snbqbgq.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/df5a0-1fw0oyzabzusm799au_so9a.png" />
</figure>

## Conclusão {#5b88 .wp-block-heading .has-large-font-size}

O intuito desse post foi demonstrar a instalação do kubecost. No próximo
post estarei mostrando as principais funcionalidades da console do
kubecost.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a84805924b}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-138" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-138" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-138" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-138-679a8480598a9 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=138&origin=jjasonhenrique.wordpress.com&obj_id=205418077-138-679a8480598a9&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-138-679a8480598a9" data-title="Curtir ou reblogar"}
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
[28 de janeiro de
2023](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[FinOps](https://jjasonhenrique.me/category/finops/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubecost](https://jjasonhenrique.me/category/kubecost/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[FinOps](https://jjasonhenrique.me/tag/finops/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubecost](https://jjasonhenrique.me/tag/kubecost/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}
:::
::::::::
:::::::::
