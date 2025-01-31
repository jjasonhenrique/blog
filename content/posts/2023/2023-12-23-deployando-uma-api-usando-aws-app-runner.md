---
title: "Deployando uma API usando AWS App Runner"
author: Jason
date: 2023-12-23
tags: [ "Cloud", "AWS", "Containers" ]
type: post
weight: 19
showTableOfContents: true
---
Nesse segundo artigo falando sobre o App Runner vou demonstrar a criação
de uma API feita com FastAPI que retorna um Hello World. O código fonte
desse API está localizado no Github no seguinte
[repositório](https://github.com/jjasonhenrique/apprunner-example.git).

Para conferir meu primeiro artigo falando sobre o AWS App Runner basta
acessar o seguinte
[link](https://jjasonhenrique.github.io/blog/posts/2023/2023-12-02-usando-o-aws-app-runner-para-subir-uma-aplicacao/).

## Pré Requisitos 

Abaixo os pré requisitos necessários para seguir esse artigo:

-   Conta AWS
-   Um repositório no Github com o código fonte informado acima.

## Configurando sua conta do Github 

Como primeiro passo devemos conectar a sua conta do Github com a sua
conta da AWS. Para isso devemos ir em **App Runner** \>\> **Connected
Accounts** e deve aparecer a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=1024"
class="wp-image-721" data-attachment-id="721"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/screenshot-from-2023-12-23-16-01-33/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png"
data-orig-size="1821,388" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-12-23-16-01-33"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/screenshot-from-2023-12-23-16-01-33.png 1821w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="218" />
</figure>

Agora devemos clicar na opção **Add New** selecionar o provider no meu
caso será Github mas existe a possibilidade de usar o Bitbucket também.
Você será redirecionado para uma tela para fazer login na sua conta do
Github para a configuração do **AWS Connector for Gihub** conforme
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png?w=666"
class="wp-image-728" data-attachment-id="728"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-2-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png"
data-orig-size="666,460" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-2" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png?w=666"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png 666w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-2.png?w=300 300w"
sizes="(max-width: 666px) 100vw, 666px" width="666" height="460" />
</figure>

Você deve inserir o nome da sua conexão no meu caso será **GitHub** e
clicar no botão **Install another** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=807"
class="wp-image-727" data-attachment-id="727"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-1-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png"
data-orig-size="807,399" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-1" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=807"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png 807w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-1.png?w=768 768w"
sizes="(max-width: 807px) 100vw, 807px" width="807" height="399" />
</figure>

Na próxima tela você deve configurar apenas o repositório que o código
fonte da nossa API está localizado conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png?w=639"
class="wp-image-730" data-attachment-id="730"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-3-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png"
data-orig-size="639,888" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-3" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png?w=216"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png?w=639"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png 639w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png?w=108 108w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-3.png?w=216 216w"
sizes="(max-width: 639px) 100vw, 639px" width="639" height="888" />
</figure>

Você deve clicar no botão **Install** e com isso a sua conexão com o
github será efetuada com sucesso e retornará para a página abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=1024"
class="wp-image-731" data-attachment-id="731"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-4-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png"
data-orig-size="1908,400" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-4" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-4.png 1908w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="214" />
</figure>

Agora você deve clicar no botão **Next** para finalizar a configuração
na AWS e deve aparecer uma tela similar abaixo informando sobre o
sucesso da conexão entre a conta AWS com a sua conta do Github.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=1024"
class="wp-image-733" data-attachment-id="733"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-5-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png"
data-orig-size="1452,259" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-5" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=1020 1020w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-5.png 1452w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="182" />
</figure>

## Fazendo o deploy da API 

Agora vamos fazer o deploy da nossa API usando o AWS App Runner. Você
deve procurar o serviço AWS App Runner depois ir na opção **Create an
App Runner Service** e depois selecionar o metódo **Source Code
Repository** deve ser carregado uma tela conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=1024"
class="wp-image-736" data-attachment-id="736"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-6-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png"
data-orig-size="1526,865" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-6" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-6.png 1526w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="580" />
</figure>

Foi carregado automaticamente as informações do repositório onde está o
código fonte da nossa API. Agora basta clicar em Next e vamos configurar
as opções de build conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=998"
class="wp-image-745" data-attachment-id="745"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-10-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png"
data-orig-size="998,803" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-10" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=998"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png 998w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-10.png?w=768 768w"
sizes="(max-width: 998px) 100vw, 998px" width="998" height="803" />
</figure>

Na próxima tela vamos configurar o nome do serviço, utilização de CPU e
memória, configuração de Autoscaling, configuração de Health Check e
outros. Para mais informações sobre essas configurações consultar o
primeiro artigo sobre o AWS App Runner nesse
[link](https://jjasonhenrique.me/2023/12/02/usando-o-aws-app-runner-para-subir-uma-aplicacao/).

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=1024"
class="wp-image-740" data-attachment-id="740"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-8-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png"
data-orig-size="1047,926" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-8" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-8.png 1047w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="905" />
</figure>

Na opção de Health Check alterar o protocolo para HTTP conforme abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png?w=734"
class="wp-image-747" data-attachment-id="747"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-12-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png"
data-orig-size="734,620" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-12" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png?w=734"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png 734w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-12.png?w=300 300w"
sizes="(max-width: 734px) 100vw, 734px" width="734" height="620" />
</figure>

Depois vamos avançar para a próxima tela e clicar na opção **Create &
Deploy.** Na tela abaixo podemos verificar que a API foi implementada
com sucesso.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=1024"
class="wp-image-749" data-attachment-id="749"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-13-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png"
data-orig-size="1837,510" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-13" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-13.png 1837w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="284" />
</figure>

Para acessar a API basta acessar o link informado no campo **Default
Domain** deve ser apresentado uma tela conforme a imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=1024"
class="wp-image-751" data-attachment-id="751"
data-permalink="https://jjasonhenrique.me/2023/12/23/deployando-uma-api-usando-aws-app-runner/image-14-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png"
data-orig-size="1268,536" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-14" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-14.png 1268w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="432" />
</figure>

## Conclusão 

O intuito desse artigo vou demonstrar o deploy de uma simples API
localizada no Github executando no serviço AWS App Runner.

