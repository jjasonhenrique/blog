---
title: "Usando eksctl para criação e gerenciamento de um cluster EKS"
author: Jason
date: 2022-09-07
tags: [ "cloud", "AWS", "EKS", "Kubernetes" ]
type: post
weight: 2
showTableOfContents: true
---
![eksctl](https://jjasonhenrique.github.io/blog/images/esctl.jpg)

Nesse artigo irei demostrar a utilização da ferramenta de linha de
comando chamada eksctl feita em Golang e criada pela empresa Weaveworks
para criação e gerenciamento de um cluster EKS.

Com essa ferramenta de linha de comando é possível criar um cluster EKS
e também gerenciar seus node groups que não foram criados com o eksctl
por exemplo um cluster que foi provisionado através do terraform.

Lembrando que o EKS não está na oferta de free tier da AWS então caso
prossiga com esse lab você será cobrado pelo tempo de utilização do
cluster EKS.

## Instalação 

O eksctl pode ser instalado em máquinas que utilizam Windows, Linux e
MAC ou também ser executado através do Docker.

### Para Linux ou Mac:

``` bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmpsudo mv /tmp/eksctl /usr/local/bin
```

Também pode ser utilizado a seguinte linha de comando para usuários Mac:

``` bash
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

### Para Windows

``` bash
chocolatey install eksctl
```

No windows também pode ser utilizado o PowerShell.

No meu caso eu utilizei uma máquina Linux. Para verificar se a
instalação foi bem sucedida basta executar o comando abaixo:

``` bash
eksctl version
```

Ou digitando apenas eksctl deve retornar a saída abaixo:

![saida-eksctl](https://jjasonhenrique.github.io/blog/images/saida-esctl.jpg)

Para mais detalhes sobre a instalação pode consultar
esse [Instalacao esctl](https://eksctl.io/introduction/#installation)

## Pré-Requisitos 

-   Usuário AWS com as permissões abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/c95a6-1x7u19bk-khprmoaswbipva.png" />
</figure>

Para mais informações sobre as permissões necessárias para o eksctl
funcionar acesse
esse [link](https://eksctl.io/usage/minimum-iam-policies/){rel="noreferrer noopener"
target="_blank"}. Abaixo segue como ficou as minhas políticas no IAM:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/5eba3-11gedtbu8jq8nmaoqyuudxa.png" />
</figure>

Para instruções de como criar um usuário na AWS consulte
este [link](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html){rel="noreferrer noopener"
target="_blank"}.

-   AWS CLI instalado. Segue abaixo procedimento para a instalação:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Dependendo da distribuição Linux será necessário instalar o unzip.

Para mais informações sobre a instalação do aws cli consulte
esse [link](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html){rel="noreferrer noopener"
target="_blank"}.

-   Kubectl instalado. Segue abaixo procedimento para a instalação:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"chmod +x ./kubectlmv ./kubectl /usr/local/bin/ 
```

Para mais informações sobre a instalação do kubectl consulte
esse [link](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

# Criando seu primeiro cluster EKS com eksctl 

Após a instalação dos pré-requisitos da seção anterior você está pronto
para a criação do seu primeiro cluster. O eksctl é uma forma mais
simples de subir um cluster EKS em background o eksctl provisiona todos
os recursos de infraestrutura utilizando cloudformation.

Por padrão o eksctl sobe instâncias do tipo m5.large para economizar
dinheiro optei pela criação do cluster utilizando o arquivo abaixo
setando o tipo de instância para utilizar t3.medium

``` bash
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: cluster-demoeksctl              
  region: us-east-1                       

nodeGroups:
  - name: ng-1                
    instanceType: t3.medium   
    desiredCapacity: 1        
  - name: ng-2
    instanceType: t3.medium 
    desiredCapacity: 1
```

Criar um arquivo com o nome eksctl.yml com conteúdo acima e executar o
comando abaixo:

``` bash
eksctl create cluster -f eksctl.yml
```

Abaixo a execução do comando e a saída para criação do cluster.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/5cbf8-1ppqyvwzveiuxdmd348vbza.png" />
</figure>

Na saída abaixo é mostrado o provisionamento dos dois Nodegroups

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/2f2b1-1pep_g_-vxnun5vb1w8ygxq.png" />
</figure>

Criado os dois Nodegroups com um node por nodegroup conforme imagens
abaixo.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/d4f42-1qp8jvut7c1h8m3f6vel-xw.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/c3efa-1otqgdehpbzgiux7hkuf_ya.png" />
</figure>

## Criando um novo Nodegroup com eksctl 

Com o eksctl você consegue também gerenciar seu cluster criado ou não
pelo eksctl. Algumas tarefas que conseguimos executar com o eksctl:

-   Criação de um novo Nodegroup;
-   Criação de um Fargate Profile;
-   Update de versão do cluster;

Para mais informações podem consultar a documentação oficial do eksctl
que vou deixar na seção de referências.

No meu caso irei demonstar a criação de um novo nodegroup no cluster EKS
criado anteriormente. Novamente vou fazer a criação de forma declarativa
usando o arquivo abaixo:

``` bash
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: cluster-demoeksctl            
  region: us-east-1 

nodeGroups:
  - name: ng-3               
    instanceType: t3.medium     
    desiredCapacity: 1  
```

Criar um arquivo com o nome nodegroup.yml com conteúdo acima e executar
o comando abaixo:

``` bash
eksctl create nodegroup -f nodegroup.yml
```

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/1c512-1gvbxeuna0l4xdbovrmqigq.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/b0984-1py94ttd00vk8cjx9kjoq6w.png" />
</figure>

## Deletando os Nodegroups e o cluster EKS

Para deletar apenas o nodegroup basta executar o comando abaixo:

``` bash
eksctl delete nodegroup -f nodegroup.yml --approve
```

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/8a3a7-11bamuyl9jjdmgxgbktdyba.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/5d545-1yd12p9-gr1_btubjihpdrq.png" />
</figure>

Agora para deletar o cluster EKS criado basta executar o comando abaixo:

``` bash
eksctl delete cluster -f eksctl.yml
```

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/b9673-1_kdg6sa4rxygqdvawxiwiq.png" />
</figure>

## Referências 

-   **Github
    eksctl:** [https://github.com/weaveworks/eksctl](https://github.com/weaveworks/eksctl){rel="noreferrer noopener"
    target="_blank"}
-   **Documentação
    Oficial**: [https://eksctl.io/](https://eksctl.io/){rel="noreferrer noopener"
    target="_blank"}
-   **Documentação Oficial
    EKS**: [https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html){rel="noreferrer noopener"
    target="_blank"}
-   **Exemplos de utilização do
    eksctl:** [https://github.com/weaveworks/eksctl/tree/main/examples](https://github.com/weaveworks/eksctl/tree/main/examples){rel="noreferrer noopener"
    target="_blank"}
