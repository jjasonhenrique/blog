---
title: "Guia Prático: Configurando Taint, Toleration e NodeAffinity no Kubernetes"
author: Jason
date: 2024-05-31
tags: [ "containers", "kubernetes" ]
type: post
weight: 8
showTableOfContents: true
---
O objetivo desse post é demonstrar como configurar que os pods da sua
aplicação executem em um determinado nó usando o kind. Essa configuração
também é valida para quando você utiliza um Kubernetes em algum cloud
provider por exemplo o EKS da AWS.

## Pré Requisitos

Para conseguir fazer as demonstrações desse post é necessário os
seguintes pré requisitos abaixo:

-   Ter o kubectl instalado.
-   Ter um cluster local usando o kind para isso pode seguir esse
    [post](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/).

## Conceitos {#conceitos .wp-block-heading .has-x-large-font-size}

Antes de partir para a parte prática é necessário nivelar alguns
conceitos que serão usados para essa configuração. Abaixo seguem esses
conceitos:

## Taint {#taint .wp-block-heading .has-large-font-size}

O conceito de taint dentro do Kubernetes tem como principal função
bloquear ou repelir que determinados pods executem ou sejam schedulados
para determinado nó. Dentro do taint temos dois principais tipos de
efeitos:

-   **NoSchedule** -- apartir do momento da configuração de um taint
    mais nenhum pod será schedulado para esse nó porém se esse nó já
    tiver pods os mesmos continuaram executando nesse nó.

<!-- -->

-   **NoExecute** -- apartir do momento da configuração de um taint mais
    nenhum pod será executado naquele nó inclusive os pods que estiverem
    executando nesse nó serão terminados e vão ser schedulados em outro
    nó sem esse taint.

Além das duas opções acima temos outra opção de effect chamada
**PreferNoSchedule** que tem como principal função ser um efeito mais
soft ou preferencial. Com esse efeito aplicado em um taint o
ControlPlane do kubernetes tentará não schedular um pod no nó com esse
taint porém não é garantido que não será schedulado.

Abaixo segue a sintaxe utilizada para configurar um taint em um nó:

``` wp-block-code
kubectl taint node name-node key=value:effect

kubectl taint node node01 enable=true:NoExecute
```

Para desconfigurar um taint usar o comando abaixo:

``` wp-block-code
kubectl taint node name-node key=value:effect -

kubectl taint node node01 enable=true:NoExecute -
```

Para verificar se um node está com o taint configurado usar o comando
abaixo:

``` wp-block-code
kubectl describe node name-node
```

## Toleration {#toleration .wp-block-heading .has-large-font-size}

O conceito de toleration dentro do Kubernetes tem como principal função
permitir que determinados pods de um deploy sejam schedulados e
executados em nó que tenha um taint configurado. Abaixo segue um exemplo
de configuração que deve ser inserido no arquivo de manifesto do seu
deploy para configurar um toleration:

``` wp-block-code
tolerations:
- key: "enable"
  operator: "Equal"
  value: "true"
  effect: "NoExecute"
```

## NodeAffinity {#nodeaffinity .wp-block-heading .has-large-font-size}

O conceito de nodeAffinity dentro do Kubernetes tem a função de garantir
que um determinado pod vá para um determinado nó. Abaixo segue um
exemplo de configuração que deve ser inserido no arquivo de manifesto do
seu deploy para configurar um nodeAffinity:

``` wp-block-code
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values:
            - antarctica-east1
```

Para fazer a configuração de um nodeAffinity é necessário criar uma
label para o node ou usar uma label que já exista.

## Criação de deploy {#criação-de-deploy .wp-block-heading .has-x-large-font-size}

Agora vamos partir para parte prática para isso é necessário que você
crie um cluster local usando kind. Para verificar se o cluster está
operacional executar o comando abaixo:

``` wp-block-code
kubectl get nodes
```

O primeiro passo é criar um namespace para nossa aplicação usando o
comando abaixo:

``` wp-block-code
kubectl create namespace app
```

Agora vamos criar um deploy do nginx nesse namespace com 15 replicas e
vamos ver como ele se distribui entre os nós dentro do nosso cluster
para isso vamos usar o arquivo de manifesto abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
  namespace: app
spec:
  replicas: 15
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

Para gerar esse arquivo de manifesto usar o comando abaixo:

``` wp-block-code
kubectl create deploy nginx --image=nginx --replicas=15 -n app --dry-run=client -o yaml > nginx.yaml
```

Agora vamos aplicar esse arquivo de manifesto usando o comando abaixo:

``` wp-block-code
kubectl apply -f nginx.yaml
```

Agora vamos verificar onde os pods estão executando usando o comando
abaixo:

``` wp-block-code
kubectl get pods -n app -o wide
```

Deverá apresentar uma saída conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=1024"
class="wp-image-960" data-attachment-id="960"
data-permalink="https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/image-1-8/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png"
data-orig-size="1201,335" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-1" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-1.png 1201w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="285" />
</figure>

Podemos perceber que os pods foram distribuidos entre os nós
**cluster-jason-worker** e **cluster-jason-worker2**.

## Configuração Taint e toleration {#configuração-taint-e-toleration .wp-block-heading .has-x-large-font-size}

Agora vamos partir para o segundo passo que é fazer a confiuração do
taint e após isso fazer a inclusão do toleration em nosso deploy. Abaixo
segue o comando utilizado para configurar o taint no nó
**cluster-jason-worker**:

``` wp-block-code
kubectl taint node cluster-jason-worker enable=true:NoExecute
```

Para verificar se o taint foi aplicado com sucesso executar o comando
abaixo:

``` wp-block-code
kubectl describe node cluster-jason-worker
```

E procurar pelo campo **Taints** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=892"
class="wp-image-962" data-attachment-id="962"
data-permalink="https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/image-2-8/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png"
data-orig-size="892,276" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-2" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=892"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png 892w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-2.png?w=768 768w"
sizes="(max-width: 892px) 100vw, 892px" width="892" height="276" />
</figure>

Agora vamos fazer uma alteração no nosso deploy adicionando o bloco do
tolerations conforme arquivo abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
  namespace: app
spec:
  replicas: 15
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
      tolerations:
      - key: "enable"
        operator: "Equal"
        value: "true"
        effect: "NoExecute"
```

Agora vamos executar o comando abaixo para fazer o replace dos pods com
a nova configuração:

``` wp-block-code
kubectl replace -f nginx.yaml
```

Para validar se a configuração foi mesma aplicada no cluster executar o
comando abaixo para visualizar o arquivo de manifesto:

``` wp-block-code
kubectl get deploy nginx -o yaml -n app
```

Agora vamos visualizar novamente a distribuição dos pods conforme
abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=890"
class="wp-image-965" data-attachment-id="965"
data-permalink="https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/image-3-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png"
data-orig-size="890,356" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-3" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=890"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png 890w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-3.png?w=768 768w"
sizes="(max-width: 890px) 100vw, 890px" width="890" height="356" />
</figure>

Podemos perceber que ainda há pods nos dois nós (cluster-jason-worker e
cluster-jason-worker2). Com isso se faz necessário a utilização do
conceito de nodeAffinity que vamos ver na sessão seguinte.

## Configurando um nodeAffinity {#configurando-um-nodeaffinity .wp-block-heading .has-x-large-font-size}

Como visto na sessão anterior mesmo usando taint e toleration ainda há
pods nos dois nós. Para fazer a configuração no nodeAffinity vamos usar
a label já existente **kubernetes.io/hostname**. Para visualizar as
labels já existentes nos seus nós executar o comando abaixo:

``` wp-block-code
kubectl get nodes --show-labels
```

Agora vamos fazer outra modificação em nosso deploy adicionando o bloco
affinity conforme arquivo abaixo:

``` wp-block-code
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
  namespace: app
spec:
  replicas: 15
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/hostname
                operator: In
                values:
                - cluster-jason-worker
      containers:
      - image: nginx
        name: nginx
      tolerations:
      - key: "enable"
        operator: "Equal"
        value: "true"
        effect: "NoExecute"
```

Vamos fazer o replace dos pods novamente e verificar que os pods estão
apenas no nó **cluster-jason-worker** conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=865"
class="wp-image-969" data-attachment-id="969"
data-permalink="https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/image-4-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png"
data-orig-size="865,357" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-4" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=865"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png 865w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-4.png?w=768 768w"
sizes="(max-width: 865px) 100vw, 865px" width="865" height="357" />
</figure>

Outro teste que podemos fazer para validar se a configuração está
correta e escalar o número de pods para 20 e verificar se os pods se
mantêm no mesmo nó executando o comando abaixo:

``` wp-block-code
kubectl scale deploy/nginx -n app --replicas 20
```

Podemos verificar que foram criados mais 5 pods no mesmo nó conforme
imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=876"
class="wp-image-971" data-attachment-id="971"
data-permalink="https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/image-5-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png"
data-orig-size="876,442" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-5" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=876"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png 876w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/05/image-5.png?w=768 768w"
sizes="(max-width: 876px) 100vw, 876px" width="876" height="442" />
</figure>

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

O objetivo desse post foi demonstrar como configurar que seus pods da
sua aplicação irão para um determinado nó usando o kind. Essa mesma
configuração serve para AWS usando o conceito de NodeGroup

## Referências {#referências .wp-block-heading .has-x-large-font-size}

Abaixo as documentações que usei como base para escrever esse post:

-   [https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/){rel="nofollow"}
-   <https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity>

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679960361d0b8}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-941" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-941" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-941" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-941-679960361e126 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250128#blog_id=205418077&post_id=941&origin=jjasonhenrique.wordpress.com&obj_id=205418077-941-679960361e126&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-941-679960361e126" data-title="Curtir ou reblogar"}
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
[31 de maio de
2024](https://jjasonhenrique.me/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[containers](https://jjasonhenrique.me/category/containers/){rel="tag"}[,
]{.wp-block-post-terms__separator}[k8s](https://jjasonhenrique.me/category/k8s/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[containers](https://jjasonhenrique.me/tag/containers/){rel="tag"}[,
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
aria-hidden="true"}[Minha Jornada para certificação AWS
SysOps Administrator](https://jjasonhenrique.me/2024/05/03/minha-jornada-para-a-certificacao-aws-sysops-administrator/){rel="prev"}
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Gerenciamento de Infraestrutura com GitOps
e ArgoCD](https://jjasonhenrique.me/2024/06/09/gerenciamento-de-infraestrutura-com-gitops-e-argocd/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2024/05/31/guia-pratico-configurando-taint-toleration-e-nodeaffinity-no-kubernetes/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

::: verbum-form-meta
:::
