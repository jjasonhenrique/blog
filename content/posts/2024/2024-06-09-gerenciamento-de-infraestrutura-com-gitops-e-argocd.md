---
title: "Gerenciamento de Infraestrutura com GitOps e ArgoCD"
author: Jason
date: 2024-06-09
tags: [ "containers", "kubernetes" ]
type: post
weight: 7
showTableOfContents: true
---
O objetivo desse artigo é criar um tutorial de como instalar o ArgoCD no
kubernetes usando kind e também vamos subir uma aplicação (o nosso
famoso e querido nginx) usando o ArgoCD.

## Pré Requisitos 

Para fazer a instalção do ArgoCD e acompanhar esse artigo será
necessário os seguintes pré requisitos abaixo:

-   Ter o helm instalado.
-   ter o kubectl instalado.
-   Ter uma conta no github.
-   Ter um cluster local usando kind. Mais informações nesse
    [link](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/).

## Conceitos {#conceitos .wp-block-heading .has-x-large-font-size}

Antes de partir para parte prática será necessários dar alguns passos
atrás para entendermos sobre GitOps.

GitOps é um forma poderosa de gerenciar e automatizar a sua
infraestrutura de forma declarativa usando um repositório git.

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png?w=277"
class="wp-image-998" data-attachment-id="998"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-2-9/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png"
data-orig-size="277,182" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-2" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png?w=277"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png?w=277"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png 277w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-2.png?w=150 150w"
sizes="(max-width: 277px) 100vw, 277px" width="277" height="182" />
</figure>

Basicamente o desenvolvedor e/ou engenheiro devops vai ter todas as
definições de infraestrutura em um repositório e o ArgoCD irá fazer
verificações em um intervalo de tempo e caso haja alguma modificação no
repositório será iniciado um novo deploy da sua aplicação no seu cluster
kubernetes.

Com gitops existe a possibilidade de fazer o reconcile das suas
definições, ou seja, caso alguém faça uma alteração manual por exemplo
alterando o número de réplicas o argocd irá verificar suas definições e
vai fazer o reverte caso o número de réplicas correto não esteja
declarado no seu repositório git.

## Instalação ArgoCD {#instalação-argocd .wp-block-heading .has-x-large-font-size}

Para seguir com a instalação será necessário a criação de um cluster
local usando kind. Após isso deve ser criado um namespace chamado argocd
conforme comando abaixo:

``` wp-block-code
kubectl create namespace argocd
```

Vamos fazer a instalação do ArgoCD utilizando o helm. Nesse
[link](https://artifacthub.io/packages/helm/argo-cd-oci/argo-cd) você
terá mais informações sobre esse helm chart.

Abaixo o comando utilizado para a instalação do ArgoCD:

``` wp-block-code
helm install argocd oci://ghcr.io/argoproj/argo-helm/argo-cd --version 7.1.3 -n argocd
```

Será iniciado a instalação e caso tenha sucesso será apresentado uma
tela conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=1024"
class="wp-image-1001" data-attachment-id="1001"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-3-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png"
data-orig-size="1903,506" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-3" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-3.png 1903w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="272" />
</figure>

Para verificar se os pods subiram corretamente podemos executar o
seguinte comando:

``` wp-block-code
kubectl get pods -n argocd
```

## Acessando o ArgoCD {#acessando-o-argocd .wp-block-heading .has-x-large-font-size}

Nesse passo vamos recuperar a senha de admin do ArgoCD que está em um
secret executando o comando abaixo:

``` wp-block-code
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Você deve salvar o valor apresentado no seu terminal.

Agora vamos acessar a console do ArgoCD fazendo um port-forward
apontando para o service do ArgoCD usando o comando abaixo:

``` wp-block-code
kubectl port-forward service/argocd-server -n argocd 8080:443 -n argocd
```

Através de qualquer navegador de Internet acessar o endereço
[http://127.0.0.1:8080](http://127.0.0.1:8080){rel="nofollow"} deverá
apresentar um erro de certificado mas pode ignorar esse erro. Deverá ser
apresentada a tela abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=1024"
class="wp-image-1005" data-attachment-id="1005"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-4-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png"
data-orig-size="1925,1048" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-4" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-4.png 1925w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="557" />
</figure>

Agora você deve usar o usuário admin e a senha recuperada através do
secret feito no passo anterior para acessar a console do ArgoCD. Deverá
aparecer uma tela igual a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=1024"
class="wp-image-1007" data-attachment-id="1007"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-5-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png"
data-orig-size="1925,1048" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-5" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-5.png 1925w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="557" />
</figure>

## Criando um repositório git {#criando-um-repositório-git .wp-block-heading .has-x-large-font-size}

Agora vamos criar um repósitorio git com as definições de um nginx
conforme abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
  namespace: argocd
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
```

Caso queira pode usar o repositório que criei para esse artigo segue o
[link](https://github.com/jjasonhenrique/nginx-example).

## Criando a conexão entre um repositório git e o ArgoCD {#criando-a-conexão-entre-um-repositório-git-e-o-argocd .wp-block-heading .has-x-large-font-size}

Agora vamos fazer a conexão entre o servidor do ArgoCD e o seu
repositório criado no passo anterior. Para isso vamos na opção
**Settings** do lado esquerdo e depois em **Repositories** e clicar em
**Connect Repo** e depois será apresentado a tela abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=1024"
class="wp-image-1010" data-attachment-id="1010"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-6-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png"
data-orig-size="1925,1048" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-6" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-6.png 1925w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="557" />
</figure>

Nessa opção você consegue adicionar tanto um repositório do git como um
repositório de helm chart.

Para o nosso caso deve ser configurado conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=1024"
class="wp-image-1012" data-attachment-id="1012"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-7-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png"
data-orig-size="1917,662" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-7" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-7.png 1917w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="353" />
</figure>

Depois você deve clicar no botão **Connect** e será apresentado a
conexão com o repositório do github conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=1024"
class="wp-image-1014" data-attachment-id="1014"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-8-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png"
data-orig-size="1917,384" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-8" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-8.png 1917w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="205" />
</figure>

## Criação da App usando a console do ArgoCD {#criação-da-app-usando-a-console-do-argocd .wp-block-heading .has-x-large-font-size}

Para fazer a criação da nossa aplicação usando o argoCD será necssário
ir no menu **Applications** e depois clicar na opção **New App**
conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=1024"
class="wp-image-1020" data-attachment-id="1020"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-9-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png"
data-orig-size="1916,1044" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-9" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-9.png 1916w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="557" />
</figure>

Agora vamos fazer as configurações da nossa aplicação. Abaixo segue
imagem das primeiras configurações:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=1024"
class="wp-image-1023" data-attachment-id="1023"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-10-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png"
data-orig-size="1916,1044" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-10" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-10.png 1916w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="557" />
</figure>

Em **General** temos as seguintes configurações abaixo:

-   **Application Name** vai ser o nome da nossa aplicação.
-   **Project** podemos selecionar em qual projeto dentro do ArgoCD a
    nossa aplicação será criada. Como não criamos nenhum projeto vamos
    colocar no projeto **Default** que já vem criado.
-   **Sync Policy** vamos alterar para **Automatic** e habilitar as duas
    opções abaixo:
    -   **Prune Resources** essa opção quando habilitada faz com que o
        ArgoCD delete qualquer recurso que não esteja declarado no git
        por exemplo se alterar a quantidade de replicas de um deploy
        usando o comando kubectl scale.
    -   **Self Heal** essa opção quando habilitada força que o estado do
        seu deploy seja igual ao que está definido no seu repositório
        git.
-   **Auto-Create Namespace** caso o namespace não exista o mesmo será
    criado no seu cluster kubernetes.

Agora vamos partir pra segunda parte das configurações conforme imagem
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=1024"
class="wp-image-1025" data-attachment-id="1025"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-11-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png"
data-orig-size="1919,922" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-11" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-11.png 1919w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="491" />
</figure>

Em **Source** temos as seguintes configurações abaixo:

-   **Repository URL** será a Url do nosso repositório git que está a
    definição do nossa app.
-   **Revision** vamos deixar o valor default que é HEAD mas podemos
    configurar outras branches ou tags do github.
-   **Path** é o diretório do github onde está a definição do nosso
    arquivo yaml.

Em **Destination** temos as seguintes configurações abaixo:

-   **Cluster URL** vamos passar a URL do nosso cluster kubernetes como
    só temos um cluster só aparecerá apenas uma opção mas existe a
    possibilidade de configurar mais de um cluster.
-   **Namespace** é o nome do namespace que será criado no nosso cluster
    caso não exista e onde será feito a criação da nossa aplicação.

Agora basta clicar no botão **Create** e será apresentado a nossa
aplicação conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png?w=670"
class="wp-image-1027" data-attachment-id="1027"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-12-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png"
data-orig-size="670,608" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-12" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png?w=670"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png 670w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-12.png?w=300 300w"
sizes="(max-width: 670px) 100vw, 670px" width="670" height="608" />
</figure>

Clicando sobre o quadro da nossa aplicação você terá uma visão de todos
os objectos criados no kubernetes para esse deploy conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=1024"
class="wp-image-1030" data-attachment-id="1030"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-13-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png"
data-orig-size="1924,571" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-13" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-13.png 1924w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="303" />
</figure>

## Criação da aplicação via linha de comando {#criação-da-aplicação-via-linha-de-comando .wp-block-heading .has-x-large-font-size}

No passo anterior criamos a nossa aplicação usando a console do ArgoCD
mas podemos também criar via linha de comando usando o kubectl. Para
isso criar um arquivo chamado app.yaml com conteúdo abaixo:

``` wp-block-code
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/jjasonhenrique/nginx-example.git'
    path: deploy
    targetRevision: HEAD
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: nginx
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

Depois disso executar o comando abaixo:

``` wp-block-code
kubectl apply -f app.yaml
```

Para verificar o estado na sua aplicação no ArgoCD você pode usar o
comando abaixo:

``` wp-block-code
kubectl get apps -n argocd 
```

Deve ser apresentado uma tela similar a abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png?w=446"
class="wp-image-1033" data-attachment-id="1033"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-14-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png"
data-orig-size="446,42" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-14" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png?w=446"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png 446w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-14.png?w=300 300w"
sizes="(max-width: 446px) 100vw, 446px" width="446" height="42" />
</figure>

## Validação {#validação .wp-block-heading .has-x-large-font-size}

Nesse passo vamos fazer uma alteração manual na nossa aplicação
diretamente no cluster alterando o número de replicas para 10 usando o
comando abaixo:

``` wp-block-code
kubectl scale deploy/nginx --replicas 10 -n nginx
```

Você irá perceber que instantaneamente serão terminados vários pods e
ficará apenas 2 pods como Running conforme está configurado na definição
que está no git. Isso é o conceito de reconcile do ArgoCD. Abaixo você
pode ver uma imagem com os pods sendo terminados:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png?w=632"
class="wp-image-1037" data-attachment-id="1037"
data-permalink="https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/image-15-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png"
data-orig-size="632,232" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-15" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png?w=632"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png 632w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/06/image-15.png?w=300 300w"
sizes="(max-width: 632px) 100vw, 632px" width="632" height="232" />
</figure>

Você também pode verificar os eventos do kubernetes usando o comando
abaixo:

``` wp-block-code
kubectl get events -n nginx
```

## Deletar cluster local {#deletar-cluster-local .wp-block-heading .has-x-large-font-size}

Para deletar seu cluster criado usando o kind seguir os comandos abaixo:

``` wp-block-code
kind get clusters
```

Para pegar o nome do cluster.

``` wp-block-code
kind delete clusters name-cluster
```

Comando acima usado para deletar o cluster .

## Conclusão {#conclusão .wp-block-heading .has-x-large-font-size}

O objetivo desse artigo foi explicar um pouco sobre GitOps, demonstrar a
instalação do ArgoCD e a criação de uma aplicação usando um repositório
git.

Lembrando que a forma como foi instalado o ArgoCD é apenas pra testes.
Para ambiente de produção é recomendável a criação do ArgoCD no modo HA.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-67995f20168f0}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-991" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-991" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-991" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-991-67995f20173b1 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250128#blog_id=205418077&post_id=991&origin=jjasonhenrique.wordpress.com&obj_id=205418077-991-67995f20173b1&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-991-67995f20173b1" data-title="Curtir ou reblogar"}
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
[9 de junho de
2024](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[ArgoCD](https://jjasonhenrique.me/category/argocd/){rel="tag"}[,
]{.wp-block-post-terms__separator}[containers](https://jjasonhenrique.me/category/containers/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[ArgoCD](https://jjasonhenrique.me/tag/argocd/){rel="tag"}[,
]{.wp-block-post-terms__separator}[gitops](https://jjasonhenrique.me/tag/gitops/){rel="tag"}[,
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
aria-hidden="true"}[Guia Prático: Configurando Taint, Toleration e
NodeAffinity
no Kubernetes](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Configuração de Remote Terraform State com Bucket S3
na AWS](https://jjasonhenrique.me/2024/08/11/configuracao-de-remote-terraform-state-com-bucket-s3-na-aws/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

::: verbum-form-meta
