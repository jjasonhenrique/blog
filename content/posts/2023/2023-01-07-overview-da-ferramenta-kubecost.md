---
title: "Overview da ferramenta kubecost"
author: Jason
date: 2023-01-07
tags: [ "cloud", "AWS" ]
type: post
weight: 26
showTableOfContents: true
---
O objetivo desse post é fazer um overview da console de administração da
ferramenta kubecost que ajuda no gerenciamento e monitoramento do custo
de um ambiente kubernetes e também do custo do cloud provider (AWS,
Azure e GCP) que você utiliza.

# Pré-Requisitos {#c244 .wp-block-heading .has-large-font-size}

Como pré-requisito é necessário já ter feito a instalação do kubecost
para fazer a instalação basta seguir
esse [link](https://medium.com/@jjason.henrique/instalando-a-ferramenta-kubecost-a099b6bec3b7) com
as intruções.

# Overview da console do Kubecost {#9cab .wp-block-heading .has-large-font-size}

Nas seções abaixo irei apresentar um pequeno overview da console de
adminstração do kubecost.

## Seção Overview {#15dd .wp-block-heading .has-medium-font-size}

Abaixo alguns prints da seção overview:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/50e60-1o7awrr9b5jzdkr1hldg1nw.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/d3c71-1bfbvpqvp48x1o8ih5nf9wa.png" />
</figure>

As seguintes informações estão disponíveis:

-   Custo total do Kubernetes nos últimos 7 dias;
-   Custo total da Cloud nos últimos 7 dias (é necessário a integração
    com o cloud provider mais informações
    nesse [link](https://docs.kubecost.com/install-and-configure/install/cloud-integration/multi-cloud){rel="noreferrer noopener"
    target="_blank"});
-   Custo total dos últimos 7 dias;
-   Possible Savings são recomendações que podem ser aplicadas ao
    ambiente;
-   Efficiency é um calculo feito baseado na utilização de CPU e Memória
    durante determinado tempo;
-   Quantidade de nós e pods por cluster;
-   Namespaces com maior gasto;

## Seção Savings {#af3a .wp-block-heading .has-medium-font-size}

Abaixo alguns prints da seção Savings:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/879a6-1sgf5evhme1o7g-xzgm2wyq.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/705a8-1uknuhzigjzcpgkp0j4ubvq.png" />
</figure>

Na seção savings são feitas recomendações para o ambiente que irá gerar
economia para o ambiente kubernetes e para cloud:

-   Recomendação de Request/Limit para CPU e Memória;
-   Mostra os recursos não utilizados na Cloud;
-   Mostra Workloads abandonados;
-   Resize de disco no node ou resize de node;

## Seção Assets e Allocations {#1f0f .wp-block-heading .has-medium-font-size}

Abaixo alguns prints da seção Allocations:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/96f4b-1cmiq2lxr7dlmppcq7vwjig.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/0ddb2-1c1jkyw7uutknisz4yagpcw.png" />
</figure>

Na seção Allocations são mostrados informações de custo separado por
CPU, RAM, Network e LoadBalance separado por namespace.

Prints da seção Assets:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/aa838-1gxqjl_zcxyfrtvv3wcp7wg.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/6503e-1bgkqvmgkg_8uixztz37_za.png" />
</figure>

Na seção Assets mostra as seguintes informações:

-   Mostra os custos baseados nos recursos por exemplo Node, Disk,
    Network;
-   Tem amostragem por data (lembrando que na versão free mostra
    informações de apenas 15 dias);
-   Mostra o quanto cada node ou cada disk no ambiente está gastando;

## Seção Health {#f2ed .wp-block-heading .has-medium-font-size}

Abaixo prints da seção Health:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/ced44-1i27rgipqygpdqnvhanth-w.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/cb824-1ler3b2azmionjqanvb8lpg.png" />
</figure>

Na seção Health é feito um checklist com boas práticas baseado em
confiabilidade e performance. Nesse checklist é feito um score de 0 a
100. Abaixo algumas dessas checagens:

-   Se existem pods com status CrashLoopBackOff;
-   Se existem pods com status pending;
-   Pods com baixo uso de cpu;
-   Memoria e Disco Pressure;
-   Custo do Cluster aumentou mais de 10%;
-   Evento de Out Of Memory;

## Seção Alerts e Reports {#189f .wp-block-heading .has-medium-font-size}

Na seção reports permite você criar relatórios baseado na seção Asset e
Allocation conforme imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/dfb1b-1_pswslrj2zxl19qg_owdqw.png" />
</figure>

Na seção Alerts possui a funcionalidade de enviar alertas para um canal
do slack. Segue abaixo imagem:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/6e28d-11ias2mz6aczflry5yavurw.png" />
</figure>

Abaixo tipos de alertas que podem ser criados:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/5a405-1ufcqxqc-u0jbowdgv4zn9q.png" />
</figure>

# Conclusão {#b15a .wp-block-heading .has-large-font-size}

O intuito desse post foi fazer um overview sobre a console de
administração do kubecost. Para mais informações do kubecost pode
consultar a documentação através
desse [link](https://docs.kubecost.com/){rel="noreferrer noopener"
target="_blank"}.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a8393cf3e7}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-151" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-151" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-151" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-151-679a8393cfc76 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=151&origin=jjasonhenrique.wordpress.com&obj_id=205418077-151-679a8393cfc76&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-151-679a8393cfc76" data-title="Curtir ou reblogar"}
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
2023](https://jjasonhenrique.me/2023/01/28/overview-da-ferramenta-kubecost/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[FinOps](https://jjasonhenrique.me/category/finops/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubecost](https://jjasonhenrique.me/category/kubecost/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::
