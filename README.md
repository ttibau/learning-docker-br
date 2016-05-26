# learn-docker-br
Repositório para compartilhamento de aprendizado de docker

## Capítulos  

###0. Home  

###1. Introdução  
1.1 [Bem vindo](chapters/01-introducao/01a-welcome.md)  
1.2 [O que é Docker?](chapters/01-introducao/01b-oque-e.md)  
1.3 [Qual a diferença?](chapters/01-introducao/01c-qual-a-diferenca.md)  
1.4 [Instalando o Docker](chapters/01-introducao/01d-instalacao.md)  


####1. Introdução  
##### Bem vindo  
Seja bem vindo ao repositório, a intenção é o compartilhamento de conteúdo do básico ao avançado de Docker, fique a vontade para contribuir para o repositório na seção de [Contribuição](link_de_contribuinting.md).  

##### O que é Docker?  
*"O Docker possibilita o empacotamento de uma aplicação ou ambiente inteiro dentro de um container, e a partir desse momento o ambiente inteiro se torna portável para qualquer outro Host que contenha o Docker instalado. Outra facilidade do Docker é poder criar suas imagens (containers prontos para deploy) a partir de arquivos de definição chamados Dockerfiles (veremos isso melhor em posts futuros)."*
  
Docker é uma plataforma aberta para desenvolvedores e administradores de sistemas, usada para construir, executar e distribuir "máquinas". 

##### Qual a diferença?  
![alt text](http://www.rightscale.com/blog/sites/default/files/docker-containers-vms.png "Diferença entre uma máquina virtual e um container")

**Máquina física**: Você instala o seu sistema operacional e utiliza normalmente;  
**Máquina virtual**: Você roda um novo sistema operacional dentro do sistema operacional que já está sendo executado na máquina física;  
**Container**: Não é preciso rodar um sistema operacional dentro do sistema operacional da máquina física. Quando utilizado o container, somente o processo que for pedido estará em execução.  

##### Instalando o Docker:  
Para instalar, rode o comando no terminal:  
```{r, engine='bash', count_lines}
curl -sSL https://get.docker.com | sh
```

##### Iniciando o Docker:  
```{r, engine='bash', count_lines}
/etc/init.d/docker start
```
ou  

```{r, engine='bash', count_lines}
sudo service docker start
```

##### Verificando se o Docker foi iniciado:  
Para verificar use o comando:  
```{r, engine='bash', count_lines}
ps -ef | grep docker
```

##### Informações sobre o Docker ps:
**CONTAINER ID**: Identificação única do container;  
**IMAGE**: A imagem que o container está utilizando. Exemplo: Ubuntu, Debian etc;  
**COMMAND**: Qual o comando o container está utilizando;  
**CREATED**: Há quanto tempo o container foi criado;  
**STATUS**: Informa se o container está "em pé" ou desligado;  
**PORTS**: As portas utilizadas pelo container;  
**NAMES**: Nome do container.

##### Criando container:

Para criarmos um novo container usamos o comando:  
(•) ANTES DE DAR O COMANDO PARA CRIAÇÃO, FICA AO SEU CRITÉRIO UTILIZAR O COMANDO DOCKER IMAGES, O COMANDO
DOCKER IMAGES VAI VERIFICAR SE EXISTE ALGUMA IMAGEM JÁ BAIXADA LOCALMENTE.

```{r, engine='bash', count_lines}
docker run -i -t ubuntu:14.10 /bin/bash/
# docker run -i -t IMAGE COMMAND
```
*Se a imagem não existir localmente*, o Docker irá baixar do hub. Ao terminar o download (se necessário) o container já vai estar criado e já vai estar logado nele, use o comando *cat /etc/issue* para verificar se realmente está no container.