[< Comandos Docker](6-ComandosDocker.md) | [Início](README.md) | [Criando Imagens >](8-CriandoImagens.md)

# Repositório Docker e Docker Hub

O [Docker Hub](https://hub.docker.com/) é um respositório público de imagens para Docker. Ele é o repositório padrão mas pode-se criar repositórios privados e usá-los também.

Quando baixamos uma Imagem ou rodamos um Container (o que irá baixar a imagem caso ela não exista localmente ainda) é do repositório Docker que o Docker irá buscar a Imagem.

Uma Imagem Docker pode ser usada diretamente ou pode ser extendida e utilizada para criar outras Imagens por terceiros. Uma Imagem não pode ser alterada por terceiros.

## Imagens Alpine e Slim

Quando procuramos Imagens no Docker Hub, veremos que diversas Imagens possuem várias tags com versões e nomes diferentes. Entre estas diversas opções, existem a denominação ```alpine``` e ```slim```. Imagens que tem essa denominação são Imagens feitas para serem muito mais leves que as Imagens padrões pois os recursos não essenciais da Imagem foram removidas. Nesse tipo de Imagem, comandos como bash, por exemplom, não irão existir.

As Imagens Alpine eram bem comuns mas devido algum problema ao qual não me aprofundei, mas acredito que eram relativos a segurança, acabaram sendo descontinuídas. No lugar delas, hoje em dia, é mais comum ver as slim.

Imagens que não contém ```alpine``` ou ```slim``` no nome são as imagens comuns, ou seja, com todas as ferramentas.

[< Comandos Docker](6-ComandosDocker.md) | [Início](README.md) | [Criando Imagens >](8-CriandoImagens.md)
