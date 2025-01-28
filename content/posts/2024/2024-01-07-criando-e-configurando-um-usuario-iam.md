---
title: "Criando e Configurando um usuário IAM"
author: Jason
date: 2024-01-07
tags: [ "cloud", "AWS" ]
type: post
weight: 12
showTableOfContents: true
---
O objetivo desse artigo é criar um tutorial demonstrando como criar e
configurar um usuário IAM na AWS para que você possa interagir de forma
programatica usando um **Access Key** e um **Secret Access Key**.

Também será demosntrado a instalação do awscli para o gereciamento dos
seus serviços na AWS.

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Abaixo os pré requisitos para seguir esse tutorial:

-   Conta AWS
-   Uma máquina Linux

## Criação usuário IAM {#criação-usuário-iam .wp-block-heading .has-x-large-font-size}

Como primeiro passo você deve acessar sua conta AWS e acessar o serviço
**IAM** e ir na opção **Users** e depois clicar na opção **Create User**
será apresentado a seguinte tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=1024"
class="wp-image-787" data-attachment-id="787"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-1-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png"
data-orig-size="1834,473" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-1" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-1.png 1834w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="264" />
</figure>

Não deve ser habiltado a opção **Provide user access to the AWS
Management Console** pois será habilitado acesso a console de
administração da AWS.

Deve ser inserido um nome para nosso usuário IAM e clicar em **Next**.

Na próxima tela vamos inserir as permissões para esse usuário IAM. No
meu caso vou inserir uma policy com permissão total no S3 conforme tela
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=1024"
class="wp-image-788" data-attachment-id="788"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-2-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png"
data-orig-size="1801,887" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-2" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-2.png 1801w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="504" />
</figure>

Lembrando que você pode customizar essa policy.

Depois disso vamos clicar em Next e será apresentado um Review da
criação do seu usuário conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=1024"
class="wp-image-790" data-attachment-id="790"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-3-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png"
data-orig-size="1815,685" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-3" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-3.png 1815w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="386" />
</figure>

Depois disso clicar na opção **Create User** e seu usuário será
criado.Seu usuário deve ser apresentado conforme tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=1024"
class="wp-image-792" data-attachment-id="792"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-4-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png"
data-orig-size="1635,354" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-4" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-4.png 1635w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="221" />
</figure>

## Criando a Acess Key do seu usuário {#criando-a-acess-key-do-seu-usuário .wp-block-heading .has-x-large-font-size}

Agora vamos criar clicando sobre seu usuário IAM indo na opção Security
credentials e depois clicando em Create access key conforme imagem
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=1024"
class="wp-image-795" data-attachment-id="795"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-5-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png"
data-orig-size="1635,410" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-5" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-5.png 1635w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="256" />
</figure>

Depois da tela você deve escolher a primeira opção **Command Line
Interface (CLI)** e habilitar a opção **I understand the above
recommendation and want to proceed to create an access key** e clicar em
Next conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=1024"
class="wp-image-797" data-attachment-id="797"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-6-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png"
data-orig-size="1521,824" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-6" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-6.png 1521w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="554" />
</figure>

Na próxima tela você pode configurar uma descrição para sua access key e
depois clicar em **Create access key** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=1024"
class="wp-image-799" data-attachment-id="799"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-7-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png"
data-orig-size="1519,315" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-7" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-7.png 1519w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="212" />
</figure>

Apos clicar no botão **Create access key** será gerado o **Acces key** e
o **Secret access key** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=1024"
class="wp-image-801" data-attachment-id="801"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-8-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png"
data-orig-size="1521,601" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-8" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-8.png 1521w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="404" />
</figure>

Lembrando que o **Secret access key** só estará disponivel nesse
momento. É recomendavel fazer o download do arquivo cvs com os valores
do **Access key** e do **Secret access key** e depois clicar em **Done**
e retornará para a tela inicial conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=1024"
class="wp-image-803" data-attachment-id="803"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-9-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png"
data-orig-size="1559,358" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-9" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-9.png 1559w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="235" />
</figure>

Nesse
[link](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#securing_access-keys)
você tem boas práticas para o gerenciamento de AWS access keys na AWS.

## Instalação AWS CLI {#instalação-aws-cli .wp-block-heading .has-x-large-font-size}

Agora vamos fazer a instalação do aws cli para podemos configurar o
**Access key** e o **Secret access key** gerados no passo anterior.
Abaixo os comandos para instalação. Para mais informações consultar esse
[link](https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html).

``` wp-block-code
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Para verificar se a instalação ocorreu com sucesso excutar o comando
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=819"
class="wp-image-806" data-attachment-id="806"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-10-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png"
data-orig-size="819,65" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-10" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=819"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png 819w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-10.png?w=768 768w"
sizes="(max-width: 819px) 100vw, 819px" width="819" height="65" />
</figure>

## Configurando o seu Access Key {#configurando-o-seu-access-key .wp-block-heading .has-x-large-font-size}

Agora vamos fazer a configuração do seu Access key e do seu Secret
access key no seu computador. Você deve executar o comando aws
configure conforme abaixo. No meu caso vou inserir a flag --profile para
gerar um novo profile dentro do arquivo .aws/credentials

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png?w=642"
class="wp-image-808" data-attachment-id="808"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-11-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png"
data-orig-size="642,117" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-11" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png?w=642"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png 642w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-11.png?w=300 300w"
sizes="(max-width: 642px) 100vw, 642px" width="642" height="117" />
</figure>

## Testando o seu access Key {#testando-o-seu-access-key .wp-block-heading .has-x-large-font-size}

Agora vamos testar o acesso via CLI tentando listar os buckets S3 na sua
conta executando o comando abaixo:

``` wp-block-code
aws s3 ls --profile blog
```

Deverá retornar o seguinte resultado abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png?w=358"
class="wp-image-810" data-attachment-id="810"
data-permalink="https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/image-12-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png"
data-orig-size="358,59" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-12" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png?w=358"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png 358w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/01/image-12.png?w=300 300w"
sizes="(max-width: 358px) 100vw, 358px" width="358" height="59" />
</figure>

Lembrando que esse Access key e Secret access key tem acesso somente ao
S3. Caso tente executar o comando **aws ec2 describe-availability-zones
--profile blog** deverá retornar o seguinte erro abaixo por falta de
permissão:

``` wp-block-code
An error occurred (UnauthorizedOperation) when calling the DescribeAvailabilityZones operation: You are not authorized to perform this operation. User: arn:aws:iam::id_conta:user/blog is not authorized to perform: ec2:DescribeAvailabilityZones because no identity-based policy allows the ec2:DescribeAvailabilityZones action
```

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objetivo desse artigo foi a criação de um tutorial para a criação e
configuração de um usuário IAM usando o Access key e o Secret access key
para interagir via CLI com a sua conta AWS.

Lembrando que o Access key e o Secret access key não estará disponivel
para uso. Você deve gerar os seus próprios valores na sua conta caso
queira seguir com esse artigo.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-6799661dd7298}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-775" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-775" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-775" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-775-6799661dd7bdf .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250128#blog_id=205418077&post_id=775&origin=jjasonhenrique.wordpress.com&obj_id=205418077-775-6799661dd7bdf&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-775-6799661dd7bdf" data-title="Curtir ou reblogar"}
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
[7 de janeiro de
2024](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/category/security/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[iam](https://jjasonhenrique.me/tag/iam/){rel="tag"}[,
]{.wp-block-post-terms__separator}[security](https://jjasonhenrique.me/tag/security/){rel="tag"}
:::
::::::::
:::::::::
::::::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

::::::::::::: {.wp-block-group .is-layout-constrained .wp-container-core-group-is-layout-12 .wp-block-group-is-layout-constrained style="padding-top:30px;padding-right:20px;padding-bottom:0px;padding-left:20px"}
::::::: {.wp-block-columns .alignwide .next-prev-links .is-layout-flex .wp-container-core-columns-is-layout-1 .wp-block-columns-is-layout-flex}
:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-previous .wp-block-post-navigation-link}
[←]{.wp-block-post-navigation-link__arrow-previous .is-arrow-arrow
aria-hidden="true"}[Minha Jornada para certificação AWS
Solutions Architect](https://jjasonhenrique.me/2023/12/25/minha-jornada-para-certificacao-aws-solutions-architect/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Testando a ferramenta AWS
Copilot CLI](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
### 2 respostas para "Criando e Configurando um usuário IAM". {#comments .wp-block-comments-title}

1.  :::::::::::::: {#comment-6}
    ::::::::::::: {.wp-block-columns .is-layout-flex .wp-container-core-columns-is-layout-2 .wp-block-columns-is-layout-flex}
    ::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow style="flex-basis:40px"}
    :::

    ::::::::::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
    :::::::: {.wp-block-group .is-nowrap .is-layout-flex .wp-container-core-group-is-layout-8 .wp-block-group-is-layout-flex}
    ::: {.wp-block-avatar style="margin-top:10px;"}
    ![Avatar de
    atilarampazo](https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=40&d=identicon&r=G){.avatar
    .avatar-40 .wp-block-avatar__image
    srcset="https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=40&d=identicon&r=G 1x, https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=60&d=identicon&r=G 1.5x, https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=80&d=identicon&r=G 2x, https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=120&d=identicon&r=G 3x, https://1.gravatar.com/avatar/19d9f04140ef521065d060865a055f4c646c3dece69fcb0176a0680b9de42a71?s=160&d=identicon&r=G 4x"
    height="40" width="40" loading="lazy" decoding="async"}
    :::

    :::::: {.wp-block-group .is-layout-flow .wp-block-group-is-layout-flow}
    ::: wp-block-comment-author-name
    atilarampazo
    :::

    :::: {.wp-block-group .is-layout-flex .wp-container-core-group-is-layout-6 .wp-block-group-is-layout-flex style="margin-top:0px;margin-bottom:0px"}
    ::: wp-block-comment-date
    [janeiro 8, 2024 at 11:30
    am](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/comment-page-1/#comment-6)
    :::
    ::::
    ::::::
    ::::::::

    ::: wp-block-comment-content
    Muito bom, como sempre!

    [Curtir](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?like_comment=6&_wpnonce=810756dce3){.comment-like-link
    .needs-login rel="nofollow" blog="205418077"}[Curtido por [1
    pessoa](#){.view-likers like-count="1"}]{#comment-like-count-6
    .comment-like-feedback}
    :::

    ::: wp-block-comment-reply-link
    [Responder](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/comment-page-1/?replytocom=6#respond){.comment-reply-link
    rel="nofollow" commentid="6" postid="775" belowelement="comment-6"
    respondelement="respond" replyto="Responder para atilarampazo"
    aria-label="Responder para atilarampazo"}
    :::
    :::::::::::
    :::::::::::::
    ::::::::::::::

2.  :::::::::::::: {#comment-11}
    ::::::::::::: {.wp-block-columns .is-layout-flex .wp-container-core-columns-is-layout-3 .wp-block-columns-is-layout-flex}
    ::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow style="flex-basis:40px"}
    :::

    ::::::::::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
    :::::::: {.wp-block-group .is-nowrap .is-layout-flex .wp-container-core-group-is-layout-11 .wp-block-group-is-layout-flex}
    ::: {.wp-block-avatar style="margin-top:10px;"}
    ![Avatar de Configuração de Remote Terraform State com Bucket S3 na
    AWS --
    jjasonhenrique](https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=40){.avatar
    .avatar-40 .wp-block-avatar__image
    srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=40 1x, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=60 1.5x, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=80 2x, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=120 3x, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-19-17-39-25.png?w=160 4x"
    height="40" width="40" loading="lazy" decoding="async"}
    :::

    :::::: {.wp-block-group .is-layout-flow .wp-block-group-is-layout-flow}
    ::: wp-block-comment-author-name
    [Configuração de Remote Terraform State com Bucket S3 na AWS --
    jjasonhenrique](https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/){rel="external nofollow ugc"
    target="_self"}
    :::

    :::: {.wp-block-group .is-layout-flex .wp-container-core-group-is-layout-9 .wp-block-group-is-layout-flex style="margin-top:0px;margin-bottom:0px"}
    ::: wp-block-comment-date
    [agosto 11, 2024 at 7:16
    pm](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/comment-page-1/#comment-11)
    :::
    ::::
    ::::::
    ::::::::

    ::: wp-block-comment-content
    \[...\] fazer a criação do usuário IAM seguir esse outro artigo que
    escrevi há um tempo \[...\]

    [Curtir](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/?like_comment=11&_wpnonce=e09a80073d){.comment-like-link
    .needs-login rel="nofollow"
    blog="205418077"}[Curtir]{#comment-like-count-11
    .comment-like-feedback}
    :::

    ::: wp-block-comment-reply-link
    [Responder](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/comment-page-1/?replytocom=11#respond){.comment-reply-link
    rel="nofollow" commentid="11" postid="775" belowelement="comment-11"
    respondelement="respond"
    replyto="Responder para Configuração de Remote Terraform State com Bucket S3 na AWS – jjasonhenrique"
    aria-label="Responder para Configuração de Remote Terraform State com Bucket S3 na AWS – jjasonhenrique"}
