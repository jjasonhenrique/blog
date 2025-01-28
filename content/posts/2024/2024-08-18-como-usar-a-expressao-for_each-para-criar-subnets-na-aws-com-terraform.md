---
title: Como usar a expressão for_each para criar subnets na aws com terraform
author: Jason
date: 2024-08-18
tags: [ "iac" ]
type: post
weight: 3
showTableOfContents: true
---

O Objetivo desse artigo é demonstrar a utilizacão da expressão for_each
com o intuito de reduzir a quantidade de código para a criacão de sua
infraestrutura no terraform. No nosso exemplo vamos otimizar a criação
de subntes na AWS.

## Pré Requisitos 

Abaixo segue os pré requisitos para acompnhar esse artigo:

-   Ter uma conta na AWS.
-   Ter o cli do terraform instalado.
-   Ter uma conta no github.
-   Já ter um repositório no github configurado para usar o terraform.
    Pode usar como exemplo esse
    [repositório](https://github.com/jjasonhenrique/terraform-blog-jason).

## O que é for_each ? 

O **`for_each`** é uma expressão que permite iterar sobre uma coleção de
itens para criar recursos no Terraform. Ele é útil quando você precisa
criar múltiplos recursos que compartilham uma estrutura comum, mas que
possuem algumas variações por exemplo o endereço cidr de cada subnet e a
zona de disponibilidade (AZ).

## Criação de uma VPC 

Para a criação das subnets é necessário a criaçºao de uma VPC na AWS.
Você deverá criar um arquivo chamado **vpc.tf** com o código abaixo:

``` 
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "main-vpc"
  }
}
```

## Criação das subnets 

Nesse próximo passo vamos imaginar que precisamos criar múltiplas
subnets em diferentes zonas de disponibilidade. Para fazer isso vamos
usar a expressão for_each para que não haja repetição de código.

Agora vamos declarar a variável subnets no arquivo **variables.tf**
conforme abaixo:

``` 
variable "subnets" {
  type         = map(any)
  default      = {}
  description  = "Subnets para criacao na AWS"
}
```

Agora vamos fazer a declaracao da variável no arquivo
**terraform.tfvars** conforme abaixo:

``` 
subnets = {
    subnet1 = {
        az  = "us-east-1a"
        cidr = "10.0.1.0/24"
    },
    subnet2 = {
        az  = "us-east-1b"
        cidr = "10.0.2.0/24"
    },
    subnet3 = {
        az  = "us-east-1c"
        cidr = "10.0.3.0/24"
    }
}
```

Agora vamos criar o recurso aws_subnet usando a expressao for_each para
que possamos iterar no map que foi declarado acima. Deve ser criado um
arquivo chamado **subnets.tf** com o código abaixo:

``` 
resource "aws_subnet" "subnets" {
  for_each = var.subnets

  vpc_id            = aws_vpc.main.id
  cidr_block        = each.value["cidr"]
  availability_zone = each.value["az"]

  tags = {
    Name = each.key
  }
}
```

Usamos o `for_each` para iterar sobre o mapa `subnets`. Para cada item
no mapa uma subnet é criada na VPC. Cada subnet recebe um CIDR e é
atribuída a uma zona de disponibilidade.

Usamos o each.key para fazer a iteração na criação no nome da subnet.

Com isso não precisamos repetir o código para cada subnet que vamos
criar na AWS tornando assim o gerenciamento dos recursos mais eficiente.

## Validação 

Para validar basta executar os comandos abaixo:

``` 
terraform init
terraform plan -out plano
terraform apply plano
```

Deve aparecer uma imagem conforme abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png?w=765"
class="wp-image-1146" data-attachment-id="1146"
data-permalink="https://jjasonhenrique.me/2024/08/18/como-usar-a-expressao-for_each-para-criar-subnets-na-aws-com-terraform/image-56/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png"
data-orig-size="765,210" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png?w=765"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png 765w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-12.png?w=300 300w"
sizes="(max-width: 765px) 100vw, 765px" width="765" height="210" />
</figure>

Você também pode validar na console da AWS conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=1024"
class="wp-image-1148" data-attachment-id="1148"
data-permalink="https://jjasonhenrique.me/2024/08/18/como-usar-a-expressao-for_each-para-criar-subnets-na-aws-com-terraform/image-57/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png"
data-orig-size="1666,196" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/08/image-13.png 1666w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="120" />
</figure>

## Vantagens de usar o for_each 

Abaixo algumas vantagens que na minha opinião fazem sentido o uso do
for_each:

-   **Flexibilidade na Configuração**: Ao definir subnets em um mapa,
    você pode facilmente ajustar o CIDR ou a AZ sem duplicar código.
-   **Escalabilidade**: Adicionar novas subnets é simples e exige apenas
    a adição de um novo item ao mapa `subnets`, sem alterar o código
    principal do recurso.
-   **Manutenção**: Como cada subnet é identificada por uma chave única
    (`subnet1`, `subnet2`, etc.), você pode gerenciar ou atualizar
    subnets específicas sem afetar outras.

## Conclusão 

O objetivo desse artigo foi demonstrar a utilização do for_each na
criação de subnets porém pode ser usado para a criação de outros
recursos na AWS.