---
title: "Criando e Configurando uma role IAM"
author: Jason
date: 2024-03-30
tags: [ "cloud", "AWS" ]
type: post
weight: 10
showTableOfContents: true
---
O objetivo desse artigo é criar um tutorial demonstrando como criar e
configurar uma role IAM na AWS para que você possa interagir de forma
programatica com a console de administração da AWS. Usando uma role IAM
é uma forma mais segura do que usando um usuário IAM conforme demonstrei
nesse
[post](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/).

## Pré Requisitos {#pré-requisitos .wp-block-heading .has-x-large-font-size}

Abaixo os pré requisitos para seguir esse tutorial:

-   Conta AWS
-   Uma máquina Linux
-   Um usuário IAM sem nenhuma permissão
-   AWS CLI instalado

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

Na próxima tela não deve ser inserido nenhuma permissão a esse usuário
IAM a permissão será concedida através da Role IAM usando uma política
IAM.

Depois disso clicar na opção **Create User** e seu usuário será criado.

Para como gerar o **Access Key** e o **Secret Access Key** e como
configurar você pode consultar esse outro
[post](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/)
que escrevi.

## Criando uma Role IAM {#criando-uma-role-iam .wp-block-heading .has-x-large-font-size}

Para demonstrar a utilização de uma Role iremos criar duas Roles IAM
conforme abaixo:

-   Uma Role IAM com permissão de listar buckets no S3
-   Uma Role IAM com permissão para parar, inicializar e reiniciar uma
    instancia EC2

Antes de criar as roles vamos criar duas politicas no IAM para as roles
IAM acima.

Para efetuar a criação você deve acessar a console de administração da
AWS ir no serviço **IAM** e depois em **Políticas** e então clicar no
botão **Criar política**. Apos clicar no botão **Criar política** será
apresentado a tela abaixo:

<figure class="wp-block-image size-large is-resized">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=1024"
class="wp-image-900" style="width:664px;height:auto"
data-attachment-id="900"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/image-41/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png"
data-orig-size="1846,558" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image.png 1846w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="309" />
</figure>

Do lado direito da tela acima clicar na opção JSON e substituir o
conteúdo conforme abaixo:

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        }
    ]
}
```

Clicar no botão Próximo e criar um nome para a política no meu caso irei
colocar o nome de **s3-list** e depois clicar no botão **Criar
política**.

Deverá ser criado outra política com o conteúdo abaixo com o nome de
**ec2-reboot**.

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2:RebootInstances",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "*"
        }
    ]
}
```

Agora vamos criar duas roles uma para cada política criada. Para isso
você deve ir no serviço **IAM** e depois na opção **Funções** e então
clicar no botão **Criar perfil**. Deverá aparecer a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=1024"
class="wp-image-904" data-attachment-id="904"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/screenshot-from-2024-03-30-16-08-48/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png"
data-orig-size="1844,713" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-03-30-16-08-48"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-08-48.png 1844w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="395" />
</figure>

Você deve selecionar a opção **Conta da AWS** e clicar em Próximo. Após
isso selecionar a policy **s3-list** criada anteriormente e clicar em
Próximo. Será apresentado a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=1024"
class="wp-image-906" data-attachment-id="906"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/screenshot-from-2024-03-30-16-20-44/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png"
data-orig-size="1800,909" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-03-30-16-20-44"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-20-44.png 1800w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="517" />
</figure>

Deve ser inserido o nome do perfil como **s3** e clicar no botão **Criar
perfil**. Agora vamos precisar editar o conteúdo da política de
confiança do perfil criado na tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=1024"
class="wp-image-908" data-attachment-id="908"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/screenshot-from-2024-03-30-16-28-49/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png"
data-orig-size="1639,894" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-03-30-16-28-49"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-16-28-49.png 1639w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="558" />
</figure>

Agora vamos substituir o conteúdo da política de confiança conforme
abaixo:

``` wp-block-code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::id_conta:user/blog"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}
```

Onde id_conta é o número da sua conta AWS. Com isso finalizamos a
configuração da role **s3**. Você deve fazer o mesmo procedimento para
role **ec2** e inserir a política **ec2-reboot** nessa role.

## Configurando as roles na CLI {#configurando-as-roles-na-cli .wp-block-heading .has-x-large-font-size}

Como pré-requisitio o usuário criado deve estar configurado na sua
máquina usando o aws cli. Para mais informações como fazer essa
configuração acessar esse
[post](https://jjasonhenrique.me/2024/01/07/criando-e-configurando-um-usuario-iam/).
Lembrando que esse usuário não pode ter nenhum tipo de acesso na AWS.

Agora vamos configurar o acesso desse usuário nas roles **s3** e
**ec2**. Deverá ser adicionado no arquivo .aws/config os seguintes
profiles abaixo:

``` wp-block-code
[profile s3]
role_arn = arn:aws:iam::id_conta:role/s3
source_profile = blog

[profile ec2]
role_arn = arn:aws:iam::id_conta:role/ec2
source_profile = blog
```

Onde id_conta é o número da sua conta da AWS.

## Testando as roles {#testando-as-roles .wp-block-heading .has-x-large-font-size}

Agora vamos testar as roles criadas. Para esses testes criei um bucket
s3 e uma instância ec2. Primeiramente vamos verificar se o usuário
criado tem alguma permissão no meu caso o nome do usuário IAM criado se
chama blog.

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=1024"
class="wp-image-914" data-attachment-id="914"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/screenshot-from-2024-03-30-18-33-51/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png"
data-orig-size="1920,300" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2024-03-30-18-33-51"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/03/screenshot-from-2024-03-30-18-33-51.png 1920w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="160" />
</figure>

Deve ser apresentado os erros acima para o usuário blog. Agora vamos
testar a role s3 usando o comando abaixo:

``` wp-block-code
aws s3 ls --profile s3
```

Deve ser apresentado a tela abaixo listando um bucket s3:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png?w=413"
class="wp-image-916" data-attachment-id="916"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/image-1-7/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png"
data-orig-size="413,72" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-1" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png?w=413"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png 413w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image-1.png?w=300 300w"
sizes="(max-width: 413px) 100vw, 413px" width="413" height="72" />
</figure>

Agora vamos para o segundo teste onde vamos parar uma instância ec2
usando a role ec2. Para isso vamos executar o comando abaixo:

``` wp-block-code
aws ec2 stop-instances --region sa-east-1 --instance-ids  i-0085922c8e050b223 --profile ec2
```

Executando esse comando acima aparecerá uma tela de mudança de estado da
instância ec2 de running para stopping conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png?w=466"
class="wp-image-919" data-attachment-id="919"
data-permalink="https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/image-2-7/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png"
data-orig-size="466,354" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-2" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png?w=466"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png 466w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/03/image-2.png?w=300 300w"
sizes="(max-width: 466px) 100vw, 466px" width="466" height="354" />
</figure>

Assim que a instância parar você pode executar o comando abaixo para
inicializar a mesma instância ec2.

``` wp-block-code
aws ec2 start-instances --region sa-east-1 --instance-ids  i-0085922c8e050b223 --profile ec2
```

Você também pode testar o desligamento da instância ec2 executando o
comando abaixo. Será apresentado uma tela de erro pois essa role não tem
essa permissão.

``` wp-block-code
aws ec2 terminate-instances --region sa-east-1 --instance-ids  i-0085922c8e050b223 --profile ec2
```

Por fim para garantir que a role s3 não consegue iniciar e parar uma
instância ec2 executar um dos comandos abaixo:

``` wp-block-code
aws ec2 stop-instances --region sa-east-1 --instance-ids  i-0085922c8e050b223 --profile s3

aws ec2 start-instances --region sa-east-1 --instance-ids  i-0085922c8e050b223 --profile s3
```

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objetivo desse artigo foi demonstar a criação de uma role e efetuar
testes usando as roles em vez de utilizar o usuário IAM. Nesse artigo
demonstrei a utilização de uma role usando apenas uma conta AWS porém é
possível utilizar multiplas contas conforme essa
[documentação](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)
da AWS.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679962aec9936}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-893" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-893" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-893" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-893-679962aeca312 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250128#blog_id=205418077&post_id=893&origin=jjasonhenrique.wordpress.com&obj_id=205418077-893-679962aeca312&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-893-679962aeca312" data-title="Curtir ou reblogar"}
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
[30 de março de
2024](https://jjasonhenrique.me/2024/03/30/criando-e-configurando-uma-role-iam/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[iam](https://jjasonhenrique.me/category/iam/){rel="tag"}[,
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

::::::::::::: {.wp-block-group .is-layout-constrained .wp-container-core-group-is-layout-6 .wp-block-group-is-layout-constrained style="padding-top:30px;padding-right:20px;padding-bottom:0px;padding-left:20px"}
::::::: {.wp-block-columns .alignwide .next-prev-links .is-layout-flex .wp-container-core-columns-is-layout-1 .wp-block-columns-is-layout-flex}
:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-previous .wp-block-post-navigation-link}
[←]{.wp-block-post-navigation-link__arrow-previous .is-arrow-arrow
aria-hidden="true"}[Testando a ferramenta AWS
Copilot CLI](https://jjasonhenrique.me/2024/01/20/testando-a-ferramenta-aws-copilot-cli/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Minha Jornada para certificação AWS
SysOps Administrator](https://jjasonhenrique.me/2024/05/03/minha-jornada-para-a-certificacao-aws-sysops-administrator/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
