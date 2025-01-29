---
title: "Testando o falco em uma máquina virtual"
author: Jason
date: 2023-11-22
tags: [ "cloud", "AWS" ]
type: post
weight: 16
showTableOfContents: true
---
O objectivo deste segundo artigo falando sobre a ferramenta falco é
demonstrar a utilização do falco instalando em uma máquina virtual na
AWS e fazer uma alteração no output de uma regra do falco.

Para ler meu primeiro artigo sobre o falco basta acessar esse
[link](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/).
Nesse artigo eu instalei o falco no Kubernetes.

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Abaixo os pré requisitos necessários para seguir esse artigo:

-   Conta AWS.
-   Uma máquina virtual na AWS (EC2).
-   EC2 deve ter acesso a Internet.
-   EC2 deve estar acessível via SSH.

## Instalação {#instalação .wp-block-heading .has-x-large-font-size}

Para a instalação irei executar uma EC2 rodando Ubuntu 22.04. Abaixo
segue os comandos utilizados para a instalação do falco:

Adicionar a chave repositório do Falco

``` wp-block-code
    curl -fsSL https://falco.org/repo/falcosecurity-packages.asc | \
    sudo gpg --dearmor -o /usr/share/keyrings/falco-archive-keyring.gpg
```

Adicionar o repositório do Falco

``` wp-block-code
echo "deb [signed-by=/usr/share/keyrings/falco-archive-keyring.gpg] https://download.falco.org/packages/deb stable main" > /etc/apt/sources.list.d/falcosecurity.list
```

Fazer o update do SO executando o comando abaixo:

``` wp-block-code
apt-get update -y
```

Após finalizar o update instalar alguns pacotes obrigatórios para a
instalação:

``` wp-block-code
apt-get install -y dkms make linux-headers-$(uname -r) dialog
```

Agora sim vamos fazer a instalação do falco usando o comando abaixo:

``` wp-block-code
apt-get install -y falco
```

Deve ser escolhido a opção eBPF conforme abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png?w=588"
class="wp-image-649" data-attachment-id="649"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-10-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png"
data-orig-size="588,293" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-10" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png?w=588"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png 588w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-10.png?w=300 300w"
sizes="(max-width: 588px) 100vw, 588px" width="588" height="293" />
</figure>

Para verificar se o falco foi instalado com sucesso deve ser executado o
comando abaixo:

``` wp-block-code
systemctl status falco-bpf
```

Deve apresentar uma saída conforme tela abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=1024"
class="wp-image-651" data-attachment-id="651"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-11-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png"
data-orig-size="1151,359" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-11" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-11.png 1151w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="319" />
</figure>

## Demonstração {#demonstração .wp-block-heading .has-x-large-font-size}

Agora vamos validar se o falco realmente está funcionando executando o
comando abaixo:

``` wp-block-code
cat /etc/shadow
```

E depois vamos verificar os logs do falco executando o comando abaixo:

``` wp-block-code
grep Sensitive /var/log/syslog
```

Deve apresentar a seguinte saída:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=1024"
class="wp-image-653" data-attachment-id="653"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-12-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png"
data-orig-size="1903,72" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-12" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=1004 1004w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-12.png 1903w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="38" />
</figure>

## Alteração regra do Falco {#alteração-regra-do-falco .wp-block-heading .has-x-large-font-size}

Para verificar todas as regras que existem por default no falco você
deve acessar o seguinte arquivo /etc/falco/falco_rules.yaml. Vamos
alterar o output da regra Read sensitive file untrusted de Sensitive
file opened for reading by non-trusted program para ACESSO INDEVIDO.

Primeiramente vamos copiar o conteudo abaixo da linha 396 ate 431 para o
arquivo /etc/falco_rule.local.yaml. É uma boa prática colocar as regras
customizadas nesse arquivo.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=1024"
class="wp-image-654" data-attachment-id="654"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-13-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png"
data-orig-size="1903,637" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-13" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-13.png 1903w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="342" />
</figure>

A seção output deve ser alterada conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=1024"
class="wp-image-656" data-attachment-id="656"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-14-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png"
data-orig-size="1903,43" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-14" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-14.png 1903w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="23" />
</figure>

Depois devemos salavar o arquivo/etc/falco_rule.local.yaml, parar e
iniciar o serviço do falco usando os comandos abaixo:

``` wp-block-code
systemctl stop falco-bpf
systemctl start falco-bpf
```

Para verificar se o falco está funcionando executar o comando abaixo:

``` wp-block-code
systemctl status falco-bpf
```

Agora vamos executar o comando cat /etc/shadow porem o grep deve ser
modificado:

``` wp-block-code
grep ACESSO /var/log/syslog
```

Por fim deve apresentar uma saída conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=1024"
class="wp-image-659" data-attachment-id="659"
data-permalink="https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/image-15-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png"
data-orig-size="1903,137" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-15" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=1014 1014w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-15.png 1903w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="73" />
</figure>

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objectivo desse artigo era fazer a instalação do falco em uma máquina
virtual e também a alteração de uma regra do falco.

Para saber mais informações sobre as rEgras default do falco acessar
esse [link](https://falco.org/docs/rules/).

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a783aeea32}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-603" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-603" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-603" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-603-679a783aef152 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=603&origin=jjasonhenrique.wordpress.com&obj_id=205418077-603-679a783aef152&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-603-679a783aef152" data-title="Curtir ou reblogar"}
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
[22 de novembro de
2023](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[containers](https://jjasonhenrique.me/category/containers/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/category/security/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Sem
categoria](https://jjasonhenrique.me/category/sem-categoria/){rel="tag"}
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
aria-hidden="true"}[Instalando e Testando a
ferramenta Falco](https://jjasonhenrique.me/2023/11/16/instalando-e-testando-a-ferramenta-falco/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Usando o AWS App Runner para subir
uma aplicação](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/){rel="next"}[→]{.