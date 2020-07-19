[< Hello Docker](3-HelloDocker.md) | [Início](README.md) | [Containers >](5-Containers.md)

# Imagens

## Baixando Imagens
Como dito anteriormente, uma Imagem é a definição de um Container. Para criar um Container e pode executá-lo, precisamos de uma Imagem para isso.

Para baixar Imagens basta rodar o seguinte comando:
```bash
docker image pull owner-username/image-name
```

Para listar todas as Imagens que temos baixadas localmente podemos usar o seguinte comando:
```bash
docker image ls
```

### Imagens e Versões

Na verdade, a referência completa para uma Imagem é feita por três partes, não apenas duas. Uma referência completa seria:

```
owner-name/image-name:version
```

Ou seja, ```nome-proprietario/nome-imagem:versão```. Quando não especificamos a versão, o Docker sempre irá considerar que estamos nos referindo a última versão (latest). Embora tenhamos usado esse recurso nos exemplos desse texto, não recomenda-se que isso seja feito pois pode levar a problemas de compatibilidade e instabilidade sem ao menos estarmos ciente das atualizações.

Por isso, recomendo que em uso real, ou quando encontrarmos problemas em uso de teste, sempre seja especificado a versão da Imagem. 

[< Hello Docker](3-HelloDocker.md) | [Início](README.md) | [Containers >](5-Containers.md)
