# docker-hands-on-for-java-developers-course
Repositório utilizado para aprendizado do curso Docker - Hands On for Java Developers

## Definições Iniciais

### Imagem
Uma Imagem é a entidade que construímos quando utilizamos Docker. Ela é a definição de um container.

### Container
Quando rodamos uma imagem Docker, temos um container. Ou seja, um container é a instância de uma imagem. Podemos ter diversas instâncias de uma imagem criadas ou rodando ao mesmo tempo, ou seja, diversos containers.

### Docker
Docker permite algo semelhante a uma virtualização mas de uma forma mais leve. Um container de Docker é um processo rodando sobre o Kernel Linux mas, diferente do que muitos pensam, ele não possui seu próprio Kernel ou Sistema Operacional. 

Quando rodamos um container com Ubuntu ou CentOS, por exemplo, o que estamos fazendo é rodar um processo, sobre o Kernel Linux da máquina hospedeira contendo as ferramentas e programas do sistema operacinal citado. O Kernel Linux é compartilhado entre o host e todos os containers que estão sendo rodados.

Docker é uma solução elegante para gerenciamento de containers baseado em uma conceito de 2008 chamado [LXC](https://en.wikipedia.org/wiki/LXC) (Linux Containers).

## Instalando Docker

### Habilitando Virtualização na BIOS
Caso você esteja usando sistemas operacionais Windows ou Mac e queira utilizar Docker nesses sistemas operacinais, o primeiro passo é verificar e habilitar o suporte a virtualização. Isso é realizado na BIOS do seu sistema e o processo depende de cada computador.

### Instação
Para instalar Docker basta seguir os passos descritos [aqui](https://www.docker.com/get-started)

## Rodando Docker
No Docker também temos nossa versão do "Hello World!", como nas linguagens de programação. Basta rodar o comando 
```
docker run hello-world
```
Caso seja a primeira vez que esteja rodando este comando, ele avisará que não existe uma imagem localmente e fará o download da imagem "hello-world". Após isso, um container será criado - a partir da imagem baixada - e executado, exibindo uma mensagem "Hello Docker!" com diversas informaçes introdutórios do Docker. Após isso, o container é finalizado.

## Baixando Imagens
Como dito anteriormente, uma imagem é a definição de um container. Para criar um container e pode executá-lo, precisamos de uma imagem para isso.

Para baixar imagens basta rodar o seguinte comando:
```
docker image pull owner-username/image-name
```
