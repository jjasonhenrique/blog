---
title: "Usando o AWS App Runner para subir uma aplicação"
author: Jason
date: 2023-12-02
tags: [ "cloud", "AWS" ]
type: post
weight: 15
showTableOfContents: true
---
O objetivo desse artigo é demonstrar a utilização do serviço AWS App
Runner. Vamos subir uma aplicação usando uma imagem publica do ECR para
testar o serviço.

## O que é AWS App Runner {#o-que-é-aws-app-runner .wp-block-heading .has-x-large-font-size}

AWS App Runner é um serviço da AWS que permite você implantar gerenciar
aplicações web e API sem a necessidade de gerenciar toda Infraestrutura
como por exemplo LoadBalancer, Autoscaling. Permite você subir
rapidamente uma aplicação.

Para mais informações sobre esse serviço da AWS consultar esse
[link](https://aws.amazon.com/pt/apprunner/).

## Aplicação usando um ECR Público {#aplicação-usando-um-ecr-público .wp-block-heading .has-x-large-font-size}

Vamos subir essa aplicação que está localizada em um ECR público
conforme esse
[link](https://gallery.ecr.aws/aws-containers/hello-app-runner).

Você deve acessar sua conta da AWS procurar pelo serviço **AWS App
Runner** e clicar na opção **Create an App Runner Service** será
apresentado a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=1024"
class="wp-image-682" data-attachment-id="682"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-16-43-20/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png"
data-orig-size="1385,898" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-16-43-20"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-43-20.png 1385w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="663" />
</figure>

Podemos usar o AWS App Runner executando através de um Container
Registry sendo o ECR o único suportado. Você tem a opção de usar um ECR
público ou um ECR privado. Abaixo seguem as configurações utilizadas
para subir uma aplicação usando um ECR público:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=818"
class="wp-image-684" data-attachment-id="684"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-16-48-00/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png"
data-orig-size="818,699" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-16-48-00"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=818"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png 818w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-16-48-00.png?w=768 768w"
sizes="(max-width: 818px) 100vw, 818px" width="818" height="699" />
</figure>

Na próxima tela vamos configurar o nome do serviço, a quantidade de CPU
e memória que será utilizada, a porta e também pode ser configurado
alguma variavel de ambiente.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png?w=730"
class="wp-image-697" data-attachment-id="697"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-29-29/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png"
data-orig-size="730,631" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-29-29"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png?w=730"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png 730w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-29-29.png?w=300 300w"
sizes="(max-width: 730px) 100vw, 730px" width="730" height="631" />
</figure>

Na próxima tela vamos configurar o Auto scaling da nossa aplicação por
ser apenas um teste vamos subir com as definições Default porém você
pode criar as suas próprias definições clicando botão create

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=810"
class="wp-image-688" data-attachment-id="688"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-01-36/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png"
data-orig-size="810,474" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-01-36"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=810"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png 810w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-01-36.png?w=768 768w"
sizes="(max-width: 810px) 100vw, 810px" width="810" height="474" />
</figure>

Na próxima fazer fazer as configurações de Health Check conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=809"
class="wp-image-690" data-attachment-id="690"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-10-09/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png"
data-orig-size="809,691" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-10-09"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=809"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png 809w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-10-09.png?w=768 768w"
sizes="(max-width: 809px) 100vw, 809px" width="809" height="691" />
</figure>

Também podemos fazer a configurações de security, networking,
observability e tags. Na próxima tela será feito um review das
configurações da sua aplicação e depois você deve clicar no botão
**Create & Deploy**

Na próxima tela podemos acompanhar o progresso do Deploy conforme
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=1024"
class="wp-image-695" data-attachment-id="695"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-18-50/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png"
data-orig-size="1770,671" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-18-50"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-18-50.png 1770w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="388" />
</figure>

Você também verificar os logs da aplicação conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=1024"
class="wp-image-696" data-attachment-id="696"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-26-33/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png"
data-orig-size="1855,999" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-26-33"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-26-33.png 1855w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="551" />
</figure>

Para testar a aplicação basta clicar no endereço que será gerado em
Default Domain conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=803"
class="wp-image-699" data-attachment-id="699"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-38-38/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png"
data-orig-size="803,286" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-38-38"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=803"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png 803w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-38-38.png?w=768 768w"
sizes="(max-width: 803px) 100vw, 803px" width="803" height="286" />
</figure>

Deve aparecer uma imagem conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=1024"
class="wp-image-701" data-attachment-id="701"
data-permalink="https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/screenshot-from-2023-12-02-17-40-04/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png"
data-orig-size="1956,1080" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-02-17-40-04"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-02-17-40-04.png 1956w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="565" />
</figure>

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objetivo desse artigo foi demonstrar a utilização do AWS App Runner
usando o metódo de Container Registry.fazendo o deploy de uma aplicação
em um ECR público. Em um próximo artigo pretendo demonstrar a utilização
do AWS App Runner usando uma API localizada no Github.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a774870f1a}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-667" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-667" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-667" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-667-679a77487170c .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=667&origin=jjasonhenrique.wordpress.com&obj_id=205418077-667-679a77487170c&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-667-679a77487170c" data-title="Curtir ou reblogar"}
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
[2 de dezembro de
2023](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[containers](https://jjasonhenrique.me/category/containers/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[containers](https://jjasonhenrique.me/tag/containers/){rel="tag"}
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
aria-hidden="true"}[Testando o falco em uma
máquina virtual](https://jjasonhenrique.me/2023/11/22/testando-o-falco-em-uma-maquina-virtual/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Deployando uma API usando AWS
App Runner](https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}
