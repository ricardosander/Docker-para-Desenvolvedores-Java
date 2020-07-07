# Comandos Docker : Clássico vs Modernos

Durante a evolução do Docker, decidiu-se migrar dos chamados "comandos clássicos" para os "comandos modernos". Isso foi uma reistruturação pois o Docker estava crescendo tanto que a quantidade de comandos estava começando a gerar confusão e desorganização.

O grande diferencial é que os comandos modernos seguem o padrão "docker recurso comando -parametros", ou seja, o comando será sempre precedido do recurso para o qual estamos rodando o comando. Exemplos de recursos são container e imagem.

Embora os comandos clássicos ainda funcionem normalmente, pode ser que eles venham a ser removidos com o tempo então recomenda-se a adotação dos comandos modernos.

Abaixo, seguem alguns exemplos de comandos clássicos (esquerda) versus comandos modernos (direita):

```
docker pull owner-name/imagem-name | docker image pull owner-name/image-name - baixando imagem
docker ps | docker container ls - listando containers 
docker images | docker image ls - listando imagens baixadas
```
