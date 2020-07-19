[< Início](README.md) | [Instalando Docker >](2-InstalandoDocker.md)


# Definições Iniciais

## Imagem
Uma Imagem é a entidade que construímos quando utilizamos Docker. Ela é a definição de um container.

## Container
Quando rodamos uma Imagem Docker, temos um Container. Ou seja, um Container é a instância de uma Imagem. Podemos ter diversas instâncias de uma Imagem criadas ou rodando ao mesmo tempo, ou seja, diversos Containers.

## Docker
O Docker permite algo semelhante a uma virtualização mas de uma forma mais leve. Um Container em Docker é um processo rodando sobre o Kernel Linux mas, diferente do que muitos pensam, ele não possui seu próprio Kernel ou Sistema Operacional. 

Quando rodamos um Container com Ubuntu ou CentOS, por exemplo, o que estamos fazendo é rodar um processo, sobre o Kernel Linux da máquina hospedeira contendo as ferramentas e programas do sistema operacinal citado. O Kernel Linux é compartilhado entre o host e todos os Containers que estão sendo rodados.

Docker é uma solução elegante para gerenciamento de containers baseado em uma conceito de 2008 chamado [LXC](https://en.wikipedia.org/wiki/LXC) (Linux Containers).

[< Início](README.md) | [Instalando Docker >](2-InstalandoDocker.md)
