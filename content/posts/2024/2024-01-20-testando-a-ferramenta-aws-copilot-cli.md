---
title: "Testando a ferramenta AWS Copilot CLI"
author: Jason
date: 2024-01-20
tags: [ "cloud", "AWS" ]
type: post
weight: 11
showTableOfContents: true
---
Nesse artigo vou demonstar a utilização da ferramenta AWS Copilot CLI
para a criação de uma forma simplificada uma aplicação conteinerizada
executando no ECS. Com o AWS Copilot CLI também é possível gerenciar
toda a infraestrutura da aplicação como por exemplo a Infraestrutura
como Código, Armazenamento e os segredos armazenados além da pipeline da
aplicação tudo via linha de comando.

Para mais informações sobre o AWS Copilot CLI acessar esse
[link](https://aws.github.io/copilot-cli/docs/overview/).

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Abaixo os pré requisitos para seguir com esse tutorial:

-   Ter uma conta AWS
-   AWS Copilot CLI instalado no seu computador
-   Ter um usuário IAM configurado na sua máquina

## Instalação {#instalação .wp-block-heading .has-x-large-font-size}

Para efetuar a instalação do AWS Copilot deve ser executado os seguintes
comandos abaixo:

``` wp-block-code
sudo curl -Lo /usr/local/bin/copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux \
   && sudo chmod +x /usr/local/bin/copilot \
   && copilot --help
```

Deve ser apresentado uma tela similar a abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=774"
class="wp-image-773" data-attachment-id="773"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-16-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png"
data-orig-size="774,900" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-16" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=258"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=774"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png 774w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=129 129w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=258 258w, https://jjasonhenrique.me/wp-content/uploads/2023/12/image-16.png?w=768 768w"
sizes="(max-width: 774px) 100vw, 774px" width="774" height="900" />
</figure>

No meu caso eu utilizei um computador executando linux. Para outros
sistemas operacionais consultar esse
[link](https://aws.github.io/copilot-cli/docs/getting-started/install/).

## Configuração usuário AWS IAM {#configuração-usuário-aws-iam .wp-block-heading .has-x-large-font-size}

Como pré requisito para utilização do AWS Copilot CLI é necessário a
configuração de usuário IAM no seu computador. Para fazer essa
configuração pode seguir esse
[artigo](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/).

\
Para esse teste vou usar a policy **AdministratorAccess**. Caso utilize
um profile diferente do default no arquivo .aws/credentials será
necessário exportar a variável de ambiente **AWS_PROFILE** ou inserir
essa variável no arquivo .bashrc ou .zshrc. No meu caso vou exportar a
variável executando o comando abaixo:

``` wp-block-code
export AWS_PROFILE="blog"
```

Para verificar o conteúdo da variável executar o comando abaixo:

``` wp-block-code
echo $AWS_PROFILE
```

## Deploy da Aplicação {#deploy-da-aplicação .wp-block-heading .has-x-large-font-size}

Para testar o AWS copilot CLI vamos fazer o deploy da aplicação que está
nesse
[repositório](https://github.com/aws-samples/amazon-ecs-cli-sample-app.git).
Primeiro passo que necessitamos fazer é o git clone desse repositório
usando o comando abaixo:

``` wp-block-code
git clone https://github.com/aws-samples/amazon-ecs-cli-sample-app.git example 
```

Depois vamos acessar a pasta example e vamos executar o comando
**copilot init** será apresentado a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=891"
class="wp-image-830" data-attachment-id="830"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-18-3/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png"
data-orig-size="891,159" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-18" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=891"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png 891w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-18.png?w=768 768w"
sizes="(max-width: 891px) 100vw, 891px" width="891" height="159" />
</figure>

Você deve inserir o nome da aplicação. No meu caso será teste-app-blog.

Na próxima tela será solicitado a escolha do tipo de workload da sua
aplicação. Para essa demonstração iremos utilizar a opção **Load
Balanced Web Service** mas existem outras opções conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=1024"
class="wp-image-831" data-attachment-id="831"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-19-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png"
data-orig-size="1046,157" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-19" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=1019 1019w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-19.png 1046w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="153" />
</figure>

Agora você deve escolher um nome para seu serviço. No meu caso será
teste-app-blog-svc conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png?w=750"
class="wp-image-833" data-attachment-id="833"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-20-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png"
data-orig-size="750,123" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-20" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png?w=750"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png 750w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-20.png?w=300 300w"
sizes="(max-width: 750px) 100vw, 750px" width="750" height="123" />
</figure>

Agora será solicitado a localização do arquivo Dockerfile ou a
localização de uma imagem da aplicação. Vamos utilizar o arquivo
Dockerfile que está na raiz do repositório.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=1024"
class="wp-image-835" data-attachment-id="835"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-21-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png"
data-orig-size="1073,186" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-21" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-21.png 1073w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="177" />
</figure>

Nesse próximo passo será criado todas as roles de infraestrutura
necessárias para o funcionamento da nossa aplicação. Após essa criação
será perguntado se você gostaria de fazer deploy da sua aplicação
conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=1024"
class="wp-image-837" data-attachment-id="837"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-22-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png"
data-orig-size="1073,399" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-22" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-22.png 1073w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="380" />
</figure>

Na próxima tela será solicitado a criação de um environment. Esse
environment terá o nome de dev conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=1024"
class="wp-image-839" data-attachment-id="839"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-23-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png"
data-orig-size="1073,139" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-23" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=1019 1019w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-23.png 1073w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="132" />
</figure>

Na próxima tela será criado toda a parte de infraestrutura como por
exemplo repositório do ECR, um bucket S3, toda parte de network e o seu
cluster ECS.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=1024"
class="wp-image-840" data-attachment-id="840"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-24-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png"
data-orig-size="1073,567" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-24" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-24.png 1073w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="541" />
</figure>

Na próxima tela o AWS copilot CLI irá fazer o build da imagem da nossa
aplicação e fazer o push para o repositório ECR criado anteriormente
conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=1024"
class="wp-image-846" data-attachment-id="846"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/screenshot-from-2024-01-20-10-49-28-1/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png"
data-orig-size="1073,512" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-01-20-10-49-28-1"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-10-49-28-1.png 1073w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="488" />
</figure>

Na próxima tela a aplicação será configurada dentro do cluster ECS, será
feito o deploy e será disponibilizado a URL da nossa aplicação conforme
image abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=1024"
class="wp-image-848" data-attachment-id="848"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-25-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png"
data-orig-size="1120,546" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-25" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-25.png 1120w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="499" />
</figure>

Acessando a URL da aplicação deve retornar uma página similar a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=1024"
class="wp-image-849" data-attachment-id="849"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-26-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png"
data-orig-size="1915,1080" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-26" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-26.png 1915w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="577" />
</figure>

## Interagindo com AWS Copilot CLI {#interagindo-com-aws-copilot-cli .wp-block-heading .has-x-large-font-size}

Com a aplicação já deployada no seu cluster ECS podemos usar o AWS
Copilot CLI para consultar informações da nossa aplicação. Abaixo segue
alguns exemplos:

``` wp-block-code
copilot svc status
```

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=827"
class="wp-image-852" data-attachment-id="852"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-27-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png"
data-orig-size="827,251" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-27" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=827"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png 827w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-27.png?w=768 768w"
sizes="(max-width: 827px) 100vw, 827px" width="827" height="251" />
</figure>

``` wp-block-code
copilot svc ls
```

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=827"
class="wp-image-854" data-attachment-id="854"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-28-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png"
data-orig-size="827,65" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-28" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=827"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png 827w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-28.png?w=768 768w"
sizes="(max-width: 827px) 100vw, 827px" width="827" height="65" />
</figure>

``` wp-block-code
copilot app show
```

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png?w=576"
class="wp-image-855" data-attachment-id="855"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/screenshot-from-2024-01-20-11-17-49/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png"
data-orig-size="576,479" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-01-20-11-17-49"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png?w=576"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png 576w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/screenshot-from-2024-01-20-11-17-49.png?w=300 300w"
sizes="(max-width: 576px) 100vw, 576px" width="576" height="479" />
</figure>

Existem muitas outras opções que você pode utilizar com o AWS copilot
CLI.

## Deletando sua aplicação {#deletando-sua-aplicação .wp-block-heading .has-x-large-font-size}

Agora vamos deletar a aplicação criada usando o comando abaixo:

``` wp-block-code
copilot app delete
```

Será solicitado uma confirmação e depois será feito processo de exclusão
de todos os recursos criados pelo AWS copilot CLI conforme imagem
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=971"
class="wp-image-857" data-attachment-id="857"
data-permalink="https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/image-29-2/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png"
data-orig-size="971,341" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-29" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=971"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png 971w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-29.png?w=768 768w"
sizes="(max-width: 971px) 100vw, 971px" width="971" height="341" />
</figure>

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O Objetivo desse artigo foi demonstrar a utilização da ferramenta AWS
Copilot CLI implementando toda a infraestrutura e uma aplicação de
teste.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679964f6c4801}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-767" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-767" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-767" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-767-679964f6c5653 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250128#blog_id=205418077&post_id=767&origin=jjasonhenrique.wordpress.com&obj_id=205418077-767-679964f6c5653&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-767-679964f6c5653" data-title="Curtir ou reblogar"}
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
[20 de janeiro de
2024](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[containers](https://jjasonhenrique.me/category/containers/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[ecs](https://jjasonhenrique.me/tag/ecs/){rel="tag"}[,
]{.wp-block-post-terms__separator}[infra](https://jjasonhenrique.me/tag/infra/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/tag/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/tag/kubernetes/){rel="tag"}
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
aria-hidden="true"}[Criando e Configurando um
usuário IAM](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Criando e Configurando uma
role IAM](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}