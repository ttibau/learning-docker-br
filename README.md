# learn-docker-br
Repositório para compartilhamento de aprendizado de docker, a idéia é ser um repositório mais de consulta e pouco conteúdo didático, passando apenas o necessário para um aprendizado rápido de docker.

## Capítulos  

### Bem vindo  
Seja bem vindo ao repositório, a intenção é o compartilhamento de conteúdo do básico ao avançado de Docker, fique a vontade para contribuir para o repositório na seção de [Contribuição](link_de_contribuinting.md).  

### O que é Docker?  
*"O Docker possibilita o empacotamento de uma aplicação ou ambiente inteiro dentro de um container, e a partir desse momento o ambiente inteiro se torna portável para qualquer outro Host que contenha o Docker instalado. Outra facilidade do Docker é poder criar suas imagens (containers prontos para deploy) a partir de arquivos de definição chamados Dockerfiles (veremos isso melhor em posts futuros)."*
  
Docker é uma plataforma aberta para desenvolvedores e administradores de sistemas, usada para construir, executar e distribuir "máquinas". 

### Qual a diferença?  
![alt text](http://www.rightscale.com/blog/sites/default/files/docker-containers-vms.png "Diferença entre uma máquina virtual e um container")

**Máquina física**: Você instala o seu sistema operacional e utiliza normalmente;  
**Máquina virtual**: Você roda um novo sistema operacional dentro do sistema operacional que já está sendo executado na máquina física;  
**Container**: Não é preciso rodar um sistema operacional dentro do sistema operacional da máquina física. Quando utilizado o container, somente o processo que for pedido estará em execução.  

### Instalando o Docker:  
Para instalar, rode o comando no terminal:  
```{r, engine='bash', count_lines}
curl -sSL https://get.docker.com | sh
```

### Iniciando o Docker:  
```{r, engine='bash', count_lines}
/etc/init.d/docker start
```
ou  

```{r, engine='bash', count_lines}
sudo service docker start
```

### Verificando se o Docker foi iniciado:  
Para verificar use o comando:  
```{r, engine='bash', count_lines}
ps -ef | grep docker
```

O comando **docker ps** é mais utilizado pois traz informações sobre os containers que estão rodando no momento.  

![alt text](http://i.imgur.com/8R4xYHi.png)  

### Informações sobre o Docker ps:
* **CONTAINER ID**: Identificação única do container;  
* **IMAGE**: A imagem que o container está utilizando. Exemplo: Ubuntu, Debian etc;  
* **COMMAND**: Qual o comando o container está utilizando;  
* **CREATED**: Há quanto tempo o container foi criado;  
* **STATUS**: Informa se o container está "em pé" ou desligado;  
* **PORTS**: As portas utilizadas pelo container;  
* **NAMES**: Nome do container.

### Criando container  
Para criarmos um novo container usamos o comando:  
Antes de dar o comando para a criação, fica ao seu critério utilizar o comando **docker images**, o qual vai verificar se existe alguma imagem já baixada localmente.

```{r, engine='bash', count_lines}
docker run -i -t ubuntu:14.10 /bin/bash/
# docker run -i -t IMAGE COMMAND
```
**Se a imagem não existir localmente**, o Docker irá baixar do hub. Ao terminar o download (se necessário) o container já vai estar criado e já vai estar logado nele, use o comando **cat /etc/issue** para verificar se realmente está no container.  

![alt text](http://i.imgur.com/WjblCPt.png)  

### Saindo/encerrando o container  
As palavras podem parecer ter o mesmo sentido, porém não pra docker, veremos:  
* Para **encerrar** utilize: **CTRL + D**  
* Para **sair**, utilize o: **CTRL + P + Q**  
* O encerrar vai matar o container e sair dele.  
* O *sair* vai deixá-lo executando e não vai exclui-lo.  
* Após dar o comando de sair, verifique se o container ainda está ativo:

![alt text](http://i.imgur.com/ECE556f.png)  

### Voltando para o container  
Para voltar para o container, usamos o comando:  

```{r, engine='bash', count_lines}
docker attach ID
```
![alt text](http://i.imgur.com/LlgcfX8.png)  

### Verificando diferenças  

Podemos verificar se existem alterações desde o momento de criação do container com o comando:
```{r, engine='bash', count_lines}
docker diff ID
```

O comando exibe todos os arquivos criados, deletados ou modificados.  

### Criando container com portas  
Para criarmos um container com portas, temos que passar um parâmetro a mais quando for criado o container, o **-p**:
```{r, engine='bash', count_lines}
docker run -i -t -p 8080:80 ubuntu:10.14 /bin/bash
```

No caso acima, passando duas portas, a primeira porta **8080** é a porta do host, ou seja: da máquina host, a porta 80 é a porta usada pelo container, logo, a porta do container vai expor na porta 8080 do host o que deve ser feito pelo container.

### Salvando imagem customizada 
Para salvarmos uma imagem customizada para que possamos utilizá-la em outros containers basta usarmos o comando **commit**:
```{r, engine='bash', count_lines}
docker commit ID NOME:VERSÃO
```

É uma boa prática sempre colocar uma versão em seu container.

### Sobre o Docker exec: executando comandos no container sem estar no container
Usamos o **docker exec** quando queremos usar um comandoeo container sem precisar entrar nele:
```{r, engine='bash', count_lines}
doc exec ID COMMAND
```

![alt text](http://i.imgur.com/btsxTY8.png http://i.imgur.com/Wv9b6Xh.png)

### Docker inspec, todas as informações de um container  
O comando docker inspec dá uma lista de todas as informações de um container, incluindo, MAC Address, IP, quando foi criado, todas as informações que possam ser necessárias:
```{r, engine='bash', count_lines}
docker inspect ID
```
![alt text](http://i.imgur.com/Wv9b6Xh.png)

### Docker stats, consumo de cpu, mem e rede 
O docker stats mostra ao usuário todo o tipo de informação de memória, CPU e rede. **Esse é um comando muito importante para análises do container**.
```{r, engine='bash', count_lines}
docker stats ID
```

### Docker rm, removendo de vez o container de sua máquina
O ** CTRL + D ** mata o container, porém ele não apaga o container, sendo assim, você pode subir o container novmente com o comando docker start ID, para apagarmos de vez o container usamos o comando rm:
```{r, engine='bash', count_lines}
docker rm -f ID
# -f = Force para o caso de o container esteja em execução
```

### Apagando imagens salvas na máquina:
Usando o comando **docker images** nós temos a lista de todas as imagens salvas  em nossa máquina, para podermos apagar uma imagem dessa máquina, basta usar o comando docker rmi
```{r, engine='bash', count_lines}
docker rmi -f ID
# -f = Force para o caso tenha algum container em execução com esta imagem
```

