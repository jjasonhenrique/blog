---
title: "Conhecendo a ferramenta Octant"
author: Jason
date: 2023-01-09
tags: [ "cloud", "AWS" ]
type: post
weight: 25
showTableOfContents: true
---
A ferramenta Octant é um projeto Open Source que permite a visulização
via interface gráfica do seu cluster Kubernetes podendo assim fazer todo
o gerenciamento do seu cluster kurbenetes e aplicações através dessa
ferramenta.

A ferramenta Octant é mantida pela VMWare abaixo segue links com mais
informações:

-   [https://octant.dev/](https://octant.dev/){rel="noreferrer noopener"
    target="_blank"}
-   [https://github.com/vmware-tanzu/octant](https://github.com/vmware-tanzu/octant){rel="noreferrer noopener"
    target="_blank"}

# Pré-Requisitos {#c7e7 .wp-block-heading .has-large-font-size}

Para testar essa ferramenta é necessário ter um cluster kubernetes
local. Pode ser usado o kind ou o rancher desktop para teste. Abaixo
segue links com mais informações:

-   [https://kind.sigs.k8s.io/](https://kind.sigs.k8s.io/){rel="noreferrer noopener"
    target="_blank"}
-   [https://rancherdesktop.io/](https://rancherdesktop.io/){rel="noreferrer noopener"
    target="_blank"}

Também é necessário que o kubeconfig esteja configurado pois a
ferramenta coleta informações desse arquivo conforme imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/ca490-1jo73cmlijkfl4jropj8vua.png" />
</figure>

Para mais informações sobre a configuração do arquivo kubeconfig
consultar
esse [link](https://kubernetes.io/pt-br/docs/concepts/configuration/organize-cluster-access-kubeconfig/){rel="noreferrer noopener"
target="_blank"}.

# Instalação {#63ed .wp-block-heading .has-large-font-size}

Para utilização é necessário a instalação de um cliente na sua máquina
local. O mesmo suporta Windows, Linux e Mac.

Para fazer o download basta acessar o link abaixo:

-   [https://github.com/vmware-tanzu/octant/releases/tag/v0.25.1](https://github.com/vmware-tanzu/octant/releases/tag/v0.25.1){rel="noreferrer noopener"
    target="_blank"}

No momento que escrevo esse post essa é a ultima versão da ferramenta.

# Overview {#451f .wp-block-heading .has-large-font-size}

Nessa seção será apresentado um overview da ferramenta.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/71bfd-1i18gvdjjfsnvcsqxmcqkgq.png" />
</figure>

Na imagem acima conseguimos ter a visualização de todos os workloads que
estão rodando em determinado namespace. Assim como o uso de CPU e
Memória para cada workload.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/f7a16-1kytm_1ilmvlyhbgucuvcjg.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/53ad9-1ybzbwffzqldmipbzdtbb0w.png" />
</figure>

Acima outros exemplos de visualizações.

Permite a visualização dos objetos do kubernetes que determinado
workload está utilizando. Segue abaixo alguns exemplos:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/27390-1zjdz4ckxk88z4p_u0yrwmg.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/f9cc0-1tglqkrowyrh7skneuti84w.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/26abd-1jqpl2hd9fr3phlxtpmyh6q.png" />
</figure>

É possível fazer um port-forward com apenas um clique

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/94603-1yzi5jxbqlyhheepy00l9uq.png" />
</figure>

Permite visualizar as variáveis de ambiente

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/838dd-1ynzx329vw2ygqlyxhgwa1a.png" />
</figure>

Visualização do yaml utilizado pelo Deploy

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/112d3-1cnohjgsrkxpxknmjj-miha.png" />
</figure>

Visualização de logs e conexão ao pod através da ferramenta

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/012d1-1405j-2a12nsvsbo1_38tqq.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/120d6-1zvj-mozcyzx8wmp98eszdw.png" />
</figure>

É possível visualização de eventos conforme imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/29a1e-1t6pbot3gc36zf59ithdw-a.png" />
</figure>

Permite aplicar um yaml através da ferramenta

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/4da71-1kddk-xiacveck_eii6uvgq.png" />
</figure>

Também é possível integrar a ferramenta com outros plugins conforme link
abaixo:

-   [https://github.com/topics/octant-plugin](https://github.com/topics/octant-plugin){rel="noreferrer noopener"
    target="_blank"}

# Conclusão {#8535 .wp-block-heading .has-large-font-size}

O objetivo desse post foi demostrar a utilização da ferramenta Octant e
como pode ajudar no gerenciamento de um cluster kubernetes via interface
gráfica ajudando assim na sua produtividade.

Existem ferramentas similares como o Kubernetes-Dashboard e o Lens.
Abaixo links com mais informações:

-   [https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/){rel="noreferrer noopener"
    target="_blank"}
-   [https://k8slens.dev/](https://k8slens.dev/){rel="noreferrer noopener"
    target="_blank"}

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a82d021684}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-109" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-109" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-109" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-109-679a82d021e94 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=109&origin=jjasonhenrique.wordpress.com&obj_id=205418077-109-679a82d021e94&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-109-679a82d021e94" data-title="Curtir ou reblogar"}
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
2023](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-octant/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[EKS](https://jjasonhenrique.me/category/eks/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[produtividade](https://jjasonhenrique.me/tag/produtividade/){rel="tag"}
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
aria-hidden="true"}[Testando a
ferramenta HardenEKS](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-hardeneks/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Instalando a
ferramenta kubecost](https://jjasonhenrique.me/2023/01/28/instalando-a-ferramenta-kubecost/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/01/28/conhecendo-a-ferramenta-octant/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
