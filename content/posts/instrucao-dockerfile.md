+++
title = "Instruções Dockerfile!"
date = 2020-07-01T07:16:50Z
author = "Allyson Oliveira"
tags = ["linux", "terraform", "automação", "AWS", "Devops", "SRE", "jenkins", "github", "docker","kubernetes", "golang","go", "python", "devops", "blog devops", "dockerfile"]
description = "Vamos escrever juntos nosso primeiro Dockerfile?"
+++

Nesse texto, vou compartilhar minhas anotações sobre as principais instruções (comandos) de um dockerfile. Eu uso esses comandos no meu dia-a-dia para criar meus containers e rodar uma app!

Vamos lá:

**ADD**: Copia arquivos, diretórios, arquivos TAR ou arquivos remotos (pode ser de uma URL, como se fosse o comando *wget* no Linux) e os adicionam ao filesystem do container. 

**COPY**: Copia novos arquivos e diretórios, adicionando-os ao filesystem do container. Eu uso para adicionar os códigos da app no meu container, quando necessário, dessa maneira:

Eu baixo o repositório através do git pull e copio os arquivos assim:

*COPY* ./path /path-da-pasta-dentro-do-container

O ponto depois do COPY significa diretório local. Nesse caso, ele adiciona todos os arquivos do diretório que você está para dentro do container

Opa, mas pera lá então: Qual a diferença entre COPY e o ADD? 
Basicamente, são duas:

A origem do ADD pode ser uma URL e do copy tem que ser através do filesystem e o ADD pode ser copiar arquivo com uma formato de compressão conhecido que ele será descomprimido, porém, a própria docker recomenda mais o uso do Copy ao invés do ADD. 

Futuramente, terá um texto sobre as melhores práticas do Dockerfile! #Fiqueligado

**RUN**: Executa qualquer comando em uma nova camada no topo da imagem (aquela camada Copy-on-Write que mencionei no ultimo texto) na hora do build da imagem. È interessante usar para instalar todos os pacotes, bibliotecas e programas necessários para que sua aplicação funcione.

**CMD**: Executa um comando após o build da imagem, diferente do RUN que executa o comando no momento em que está "buildando" a imagem, o CMD executa no início da execução do container. 

Exemplo: 
*CMD* ["./script.sh"]

Existem outras formas de escrever o CMD, mas eu sempre sigo essa que fica mais fácil para eu decorar e usar:

*CMD* ["executável","param1","param2"]

Essa forma é conhecida como EXEC form e conforme a documentação da docker, é a mais recomendável!

**ENTRYPOINT**: Permite você configurar um container para rodar um executável, e quando esse executável for finalizado, o container também será. É como se transformassemos um container em um executável.

Eita! Qual a diferença entre CMD e ENTRYPOINT?

Basicamente, pelo que estudei, não existem muitas diferenças. Recomendo você a usar o entrypoint, baseado nesse texto [aqui](http://www.johnzaccone.io/entrypoint-vs-cmd-back-to-basics/) de um Docker Captain informando que o ideal é usar Entrypoint ao invés do CMD.

**LABEL**: Adiciona metadados a imagem, como por exemplo, versão da app, descrição e fabricante. Deve ser utilizado assim:

*LABEL* ImageName=Ubuntu, App=Frontend

**ENV**: Informa variáveis de ambiente ao container;

**EXPOSE**: Informa qual porta o container estará ouvindo;

**FROM**: Indica qual imagem será utilizada como base, ela precisa ser a primeira linha do Dockerfile;

**MAINTAINER**: Geralmente é escrito o nome e o e-mail do autor da imagem. Essa instrução foi descontinuada, mas você ainda pode ver por ai.

**USER**: Determina qual o usuário será utilizado na imagem. Por default é o root;

**VOLUME**: Permite a criação de um ponto de montagem no container; Esse cara, merece um texto especifico para ele, então #Fiqueligado2

**WORKDIR**: Responsável por mudar do diretório / (raiz) para o especificado nessa instrução, por exemplo:

*WORKDIR* /path-da-app

**HEALTHCHECK**: Essa instrução é um jeito de verificar se o container está funcionando. Confesso que atualmente não utilizo muito, monitoro o health de um container de outras maneiras. 
Um exemplo é:
```
*HEALTHCHECK* --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```
Esses são os principais 'comandos' que uso para escrever um dockerfile. Você pode encontrar todos aqui:

https://docs.docker.com/engine/reference/builder/