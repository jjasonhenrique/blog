---
title: "Instalando e Testando a ferramenta Falco"
author: Jason
date: 2023-11-16
tags: [ "cloud", "AWS" ]
type: post
weight: 17
showTableOfContents: true
---
O falco é uma ferramenta desenvolvida pela empresa sysdig e tem a função
de detectar ameaças em near real time na cloud, em workloads executando
no kubernetes e containers.

O falco pode monitorar eventos de várias origens como por exemplo o
kernel do Linux, Kubernetes API, containers e outros. O falco permite
você fazer intregação com ferrramentas de mensagens como slack e teams
como também permite você integrar com o prometheus para isso é
necessário a utilização de um daemon do falco que se chama
falcosidekick.

Para mais informações do falco consultar os links abaixo:

-   <https://github.com/falcosecurity/falcosidekick>
-   <https://falco.org/docs/>

O falco também é um dos tópicos abordados no exame de certificação CKS
(Certified Kubernetes Security Specialist) para quem pretende tirar.

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Nessa demostração do funcionamento do falco vamos necessitar os
seguintes pré requitios:

-   Cluster kubernetes local pode ser criado através do kind. Mais
    informações nesse
    [link](https://kind.sigs.k8s.io/docs/user/quick-start/){target="_blank"
    rel="noreferrer noopener"}. Também criei esse
    [artigo](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/)
    para ajudar no processo de criação de um cluster local.
-   Helm instalado. Mais informações nesse
    [link](https://helm.sh/docs/intro/install/){target="_blank"
    rel="noreferrer noopener"}.
-   kubectl instalado. Mais informações nesse
    [link](https://kubernetes.io/docs/tasks/tools/).

## Instalação {#instalação .wp-block-heading .has-x-large-font-size}

Antes de fazer a instalação do falco você deve adicionar o repositório
do falco no helm executando os comandos abaixo:

``` wp-block-code
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
```

Também deve ser criado um namespace separado para a instalação do falco
usando o comando abaixo:

``` wp-block-code
kubectl create namespace falco
```

Agora vamos efetuar a instalação do falco executando o comando abaixo:

``` wp-block-code
helm install falco -n falco --set driver.kind=ebpf --set tty=true falcosecurity/falco
```

Para verificar se o falco subiu corretamente executar o comando abaixo:

``` wp-block-code
kubectl get pods -n falco
```

Deverá apresentar uma saída conforme a imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png?w=426"
class="wp-image-595" data-attachment-id="595"
data-permalink="https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/screenshot-from-2023-11-15-12-23-42/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png"
data-orig-size="426,88" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-11-15-12-23-42"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png?w=426"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png 426w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-23-42.png?w=300 300w"
sizes="(max-width: 426px) 100vw, 426px" width="426" height="88" />
</figure>

Será criado um daemonset do falco com isso será executado um pod em cada
node do seu cluster no meu caso há 3 pods do falco. Para verificar
executar o comando abaixo:

``` wp-block-code
kubectl get daemonset -n falco
```

Agora vamos verificar os logs do pods do falco para isso vamos executar
o comando abaixo:

``` wp-block-code
kubectl logs -f daemonset/falco -n falco
```

Deverá aparecer uma saída similar a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=1024"
class="wp-image-597" data-attachment-id="597"
data-permalink="https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/screenshot-from-2023-11-15-12-26-09/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png"
data-orig-size="1226,215" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-11-15-12-26-09"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-15-12-26-09.png 1226w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="179" />
</figure>

## Demonstração {#demonstração .wp-block-heading}

Agora vamos simular o funcionamento do falco criando um pod, conectando
nesse pod usando o kubectl e verificar os logs do falco.

Abaixo comando para criar um pod para esse teste:

``` wp-block-code
kubectl run alpine --image alpine -- sh -c "sleep infinity"
```

Agora vamos conectar no pod criado usando o comando abaixo:

``` wp-block-code
kubectl exec -it alpine -- sh
```

Verificando os logs do falco terá uma saída simlar com a abaixo:

``` wp-block-code
15:27:42.692441989: Notice A shell was spawned in a container with an attached terminal (evt_type=execve user=root user_uid=0 user_loginuid=-1 process=sh proc_exepath=/bin/busybox parent=runc command=sh terminal=34816 exe_flags=EXE_WRITABLE container_id=cf009dcc3814 container_image=<NA> container_image_tag=<NA> container_name=<NA> k8s_ns=<NA> k8s_pod_name=<NA>)
```

Através do evento acima podemos constatar que alguém conectou no
terminal do container com o id cf009dcc3814.

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objectivo desse artigo foi fazer a instalação e a demonstração da
ferramenta falco usando um cluster local do kubernetes.

O falco também pode ser instalado em uma máquina virtual executando
Linux pretendo fazer essa demonstração em um próximo artigo.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a7929cceb2}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-514" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-514" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-514" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-514-679a7929cd617 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=514&origin=jjasonhenrique.wordpress.com&obj_id=205418077-514-679a7929cd617&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-514-679a7929cd617" data-title="Curtir ou reblogar"}
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
[16 de novembro de
2023](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/category/security/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[falco](https://jjasonhenrique.me/tag/falco/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}[,
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
aria-hidden="true"}[Criando um cluster local de kubernetes com
o kind](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Testando o falco em uma
máquina virtual](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/11/16/instalando-e-testando-a-ferramenta-falco/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

