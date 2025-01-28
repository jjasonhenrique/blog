---
title: "Configuração de Remote Terraform State com Bucket S3 na AWS"
author: Jason
date: 2024-08-11
tags: [ "iac", "cloud", "AWS" ]
type: post
weight: 6
showTableOfContents: true
---
O objetivo desse post é fazer a configuração de um remote terraform
state usando um bucket s3 em vez de deixar seu terraform state
localmente. E também criar algum recurso na aws para testar o
funcionamento do terraform.

## Pré-Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Para continuar a configuração do seu remote terraform state serão
necessários os seguintes pré-requitios abaixo:

-   Uma conta na AWS
-   Um Usuário IAM
-   AWS CLI instalado.
-   Uma conta no github
-   Um bucket S3
-   CLI do terraform instalado

## Criação Usuário IAM {#criação-usuário-iam .wp-block-heading .has-x-large-font-size}

Nesse primeiro passo vamos fazer a criação de um usuário IAM para usar
nos arquivos de configurações do terraform.

Para fazer a criação do usuário IAM seguir esse outro
[artigo](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/)
que escrevi há um tempo atrás.

Para a nossa configuração vamos anexar a policy **AdmistratorAccess** no
nosso usuário **blog** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=1024"
class="wp-image-1099" data-attachment-id="1099"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-45/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png"
data-orig-size="1571,302" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=1020 1020w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-1.png 1571w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="196" />
</figure>

Foi anexada essa policy pois é apenas um lab para ambientes produtivos
devem ser colocadas as devidas permissões.

## Criação bucket S3 {#criação-bucket-s3 .wp-block-heading .has-x-large-font-size}

Nesse próximo passo vamos criar o nosso bucket S3 que terá a função de
armazenar o nosso terraform state. Para isso devemos ir na console de
administração da AWS procurar pelo serviço S3 e clicar em **Create
Bucket** deve ser apresentado a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png?w=746"
class="wp-image-1107" data-attachment-id="1107"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-49/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png"
data-orig-size="746,854" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png?w=262"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png?w=746"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png 746w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png?w=131 131w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-5.png?w=262 262w"
sizes="(max-width: 746px) 100vw, 746px" width="746" height="854" />
</figure>

No meu caso vou colocar o nome do bucket de terraform-blog-jason e outro
detalhe é que esse bucket será criado na região us-east-1.

Devemos deixar habilitado a opção **Block all public access** conforme
imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png?w=762"
class="wp-image-1105" data-attachment-id="1105"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-48/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png"
data-orig-size="762,474" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png?w=762"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png 762w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-4.png?w=300 300w"
sizes="(max-width: 762px) 100vw, 762px" width="762" height="474" />
</figure>

Depois disso vamos clicar no botão **Create bucket** no final da tela.
Com isso o nosso bucket será criado conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=1024"
class="wp-image-1108" data-attachment-id="1108"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-50/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png"
data-orig-size="1619,516" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-6.png 1619w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="326" />
</figure>

## Criação repositório Github {#criação-repositório-github .wp-block-heading .has-x-large-font-size}

Agora vamos criar um repositório no Github para armazenar os nossos
arquivos de configurações do terraform conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=1024"
class="wp-image-1110" data-attachment-id="1110"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-51/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png"
data-orig-size="1645,760" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-7.png 1645w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="473" />
</figure>

Na opção Add .gitignore adicionar o template do Terraform. Segue o link
do [repositório](https://github.com/jjasonhenrique/terraform-blog-jason)
criado.

Agora vamos fazer um clone desse repositório usando o comando abaixo:

``` wp-block-code
git clone https://github.com/jjasonhenrique/terraform-blog-jason.git
```

## Criando os arquivos de configurações {#criando-os-arquivos-de-configurações .wp-block-heading .has-x-large-font-size}

Agora vamos fazer a criação dos arquivos de configurações do terraform.
Como pré-requisito o usuário IAM já deve estar configurado via AWS CLI
conforme o artigo que escrevi anteriormente e que deixei o link no
começo desse artigo.

O primeiro arquivo que vamos criar é o versions.tf conforme abaixo:

``` wp-block-code
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

Em seguida vamos fazer a criação do arquivo provider.tf conforme abaixo:

``` wp-block-code
provider "aws" {
  shared_config_files      = ["/home/jason/.aws/config"]
  shared_credentials_files = ["/home/jason/.aws/credentials"]
  profile                  = "blog"
}
```

Nesse arquivo passamos o usuário que será utilizado para a conexão com a
AWS.

Agora vamos fazer a criação do arquivo main.tf conforme abaixo:

``` wp-block-code
terraform {
  backend "s3" {
    bucket  = "terraform-blog-jason"
    key     = "terraform.tfstate"
    region  = "us-east-1"
    profile = "blog"
  }
}
```

No arquivo acima estamos fazendo a configuração de um bucket S3 na AWS
para que seja usado como remote terraform state.

Agora vamos criar um recurso na AWS criando o arquivo ecs.tf conforme
abaixo:

``` wp-block-code
resource "aws_ecs_cluster" "teste-blog" {
  name = "teste-blog"

  setting {
    name  = "containerInsights"
    value = "disabled"
  }
}
```

Lembrando que para a criação de um cluster ECS na AWS existem mais
configurações a serem feitas. É apenas um teste para validar o
funcionamento do remote terraform state.

Após a criação dos arquivos você deve commitar eles excutando os
comandos abaixo:

``` wp-block-code
git add .
git commit -m "Criação Cluster ECS"
git push
```

## Validação {#validação .wp-block-heading .has-x-large-font-size}

Agora vamos validar o remote terraform state e a criação de um recurso
na AWS. Primeiramente vamos inicializar o nosso terraform state
executando o comando abaixo:

``` wp-block-code
terraform init
```

A saída do comando deve ser similar a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=830"
class="wp-image-1117" data-attachment-id="1117"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-52/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png"
data-orig-size="830,510" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=830"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png 830w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-8.png?w=768 768w"
sizes="(max-width: 830px) 100vw, 830px" width="830" height="510" />
</figure>

Agora vamos executar o comando terraform plan para validar o que será
criado na AWS. A saída deve ser similar a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=1024"
class="wp-image-1119" data-attachment-id="1119"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-53/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png"
data-orig-size="1185,390" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-9.png 1185w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="337" />
</figure>

Agora vamos executar o comando terraform apply -auto-approve para a
criação do recurso na AWS.

Para validar vamos no serviço ECS e verificar se o cluster ECS foi
criado. Deve ter criado conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=1024"
class="wp-image-1121" data-attachment-id="1121"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-54/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png"
data-orig-size="1623,288" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=1020 1020w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-10.png 1623w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="181" />
</figure>

Também vamos validar se foi criado um arquivo terraform.tfstate no
bucket conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=1024"
class="wp-image-1123" data-attachment-id="1123"
data-permalink="https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/image-55/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png"
data-orig-size="1623,424" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-11.png 1623w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="267" />
</figure>

Para destruir o recurso criado na AWS para esse teste executar o comando
abaixo:

``` wp-block-code
terraform destroy -auto-approve
```

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O Objetivo desse artigo foi demonstar como deve ser feito a configuração
de um remote terraform state usando um bucket S3 na AWS.

