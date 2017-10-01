# learn-docker-br
Repositório para compartilhamento de aprendizado de docker, a idéia é ser um repositório mais de consulta e pouco conteúdo didático, passando apenas o necessário para um aprendizado rápido de docker.

## Capítulos  

### Bem vindo  
Seja bem vindo ao repositório, a intenção é o compartilhamento de conteúdo do básico ao avançado de Docker, Ocasionalmente surgirão alguns comandos de console para Linux Ubuntu 14.

### Básico

1. [Qual a diferença?]()
1. [O que é Docker?]()
1. [Instalando o Docker:]()
1. [Iniciando o Docker:]()
1. [Verificando se o Docker foi iniciado:]()
  1. [Informações do docker ps:]()
1. [Criando container:]()
1. [Saindo/Encerrando o container]()
1. [Voltando para o container:]()
1. [Verificando diferenças]()
1. [Criando container com portas]()
1. [Salvando imagem customizada]()
1. [Sobre o Docker exec, executando comandos no container sem estar no container]()
1. [Docker inspec, todas as informações de um container]()
1. [Docker stats, consumo de cpu, mem e rede]()
1. [Docker rm, removendo de vez o container de sua máquina:]()
1. [Apagando imagens salvas na máquina:]()
1. [Comunicando 2 containers pela rede]()
1. [Criando uma imagem personalizada com Dockerfile]()
  1. [Criando a imagem:]()
  1. [Buildando o arquivo criado:]()
  1. [Criando container com a imagem criada:]()
1. [Limitando a quantidade de memória]()
1. [Limitando a CPU do container]()
1. [Docker update, gerenciando container sem precisar derrubá-lo]()
1. [Adicionando hosts]()

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
sudo service docker status
```

O comando **docker ps** é também utilizado pois traz informações sobre os containers que estão rodando no momento.  

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
docker exec ID COMMAND
```

![alt text](http://i.imgur.com/btsxTY8.png )
![alt text](http://i.imgur.com/Wv9b6Xh.png)

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

### Comunicando 2 containers pela rede
Apos ter o primeiro container "em pé", suba mais um seugndo container passando o parâmetro **--link**:
VOCÊ PODE VERIRICAR O NOME DOS CONTAINERS CRIADOS USANDO O DOCKER PS
```
docker run -it --name NAME --link NOME_CONTAINER_ORIGEM:HOST_CONTAINER_ORIGEM IMAGEM COMANDO

# NOME_CONTAINER_ORIGEM = nome do container de origem, com o qual o seu
novo host vai ser comunicar
# HOST_CONTAINER_ORIGEM = nome de hosto do container de origem, que será
usado no novo host para se comunicar para se comunicar com o container
de origem
```
![alt text](https://i.imgur.com/0Ho3LWV.png)

### Criando uma imagem personalizada com Dockerfile
#### Criando a imagem:
O Docker file é um arquivo que você coloca cada passo na criação de um novo container, gerado em uma imagem, é parecido com o make file do linux. Por exemplo, se eu quero baixar uma imagem do dockerhub, porém, ao subir um container com apache, eu quero que já suba com os arquivos de meu projeto, colocar configurações personalizadas, etc.

1. Crie um diretório;
2. Crie um diretório que contenha o nome da sua imagem. Importante: Eu só posso ter um dockerfile por diretório, pois quando eu for criar a imagem, eu passo o diretório do dockerfile e não o caminho do dockerfile em si;
3. Entre na pasta criada e crie o arquivo **Dockerfile**;
4. A primeira linha semore deve ser o **FROM**, que é a imagem que iremos utilizar;
5. A segunda linha deve ser o **MAINTAINER**, que é a pessoa que criou a imagem;
6. Logo, vem o comando **RUN**, que é um dos comandos mais importantes, ele é o comando que vai executar as ações na criação do container.

![alt text](https://i.imgur.com/aBbdZ7H.png)

### Buildando o arquivo criado:
O comando **docker build path**, é o comando que gera a imagem a partir do Dockerfile criado, o **path** é o caminho do Dockerfile.

![alt text](https://i.imgur.com/YNb9b8g.png)

Feito o passo, podemos dar um **docker images** para listarmos as imagens criadas. Pode-se perceber que foi gerada uma imagem sem informações importantes:

![alt text](https://i.imgur.com/ymfBxoV.png)

Para incluirmos as informações, devemos usar o parâmetro -t de **tag**:
```
docker build -t anser/apache:1.0 .
# docker build -t nome_imagem:versão caminho
```

![alt text](https://i.imgur.com/D81xPgB.png)

**Criando container com a imagem criada:**
```
docker run -it anser/apache:1.0 /bin/bash
```

Dê um **ps -ef** para verificar se o apache está rodando, podemos ver que não, já que não haviamos passado no comando **RUN**, use o comando **netstat -atunp** para ver o ip do container e tente acessar o ip no navegador, ou use um curl.

![alt text](https://i.imgur.com/xpH9U2A.png)

### Limitando a quantidade de memória
Vamos criar um container normalmente como visto na sessão de **Como criar um container**, após criá-lo, iremos usar o comando **free -m**, o comando free -m retornará a memória do container, porém o container quando é criado usa o total de memória do host, então devemos criar um container passando a propriedade -m na sua criação.
```
docker run -ti -m 512M debian /bin/bash
```
Agora, se dermos o **free -m**, ele irá exibir novamente a memória do host inteiro, e por isso que usamos o DOCKER INSPECT:
```
docker inspect ID | grep -i mem
```

![alt text](https://i.imgur.com/1VqPaBz.png)

### Limitando a CPU do container
Para limitarmos a CPU do container, precisamos passar na criação o --cpu-shares QUANTIDADE, por exemplo:
```
docker run -ti --cpu-shares 1024 debian /bin/bash
```

Verifique o cpu com o docker inspect.

### Docker update, gerenciando container sem precisar derrubá-lo
Com o docker update temos a possibilidade de gerenciar memória, cpu e outras coisas sem precisar derrubar o container, **dê o comando docker uptade --help para ver mais opções.**

Vamos usar o container criado acima com 512M de memória e alterar para 256M de memória sem precisar derrubá-lo:
```
docker update -m 256M ef34c299f86a
```

### Adicionando hosts
O docker provê um comando para adicionarmos hosts no arquivo /etc/hosts sem que tenhamos que entrar no container para modificar isso:
```
docker run -it --add-host NOME:IP IMAGEM COMANDO
```

![alt text](https://i.imgur.com/8CVusDk.png)

### Volumes
**Criando volumes:**
O volume é um local na máquina host que ficam persistidos os dados do container, mesmo que o container seja apagado, aqueles dados permanecerão com a integridade
```
docker volume create --name NOME
```
**Mapear volume em um container:**
```
docker run -it -v NOME:/tmp/dados IMAGES COMMAND
```
O que acontece é que estou pegando um determinado volume e mapeando dentro da pasta /tmp/dados que poderia ser qualquer outra definida.
Tudo que será colocado dentro da pasta /tmp/dados será armazenado dentro de um volume que fica no host, o volume é persistido mesmo que o container seja destruido. Se verificarmos, no container teremos a pasta /tmp/dados, como teste, crie um arquivo e crie um novo container passando o mesmo volume.

**Passando mais de um volume:**
Para passar mais de um volume na criação de um container, basta usar mais **-v**:
```
docker run -it -v NOME:/tmp/dados -v NOME2:/tmp/dados/teste IMAGEM COMANDO
```

**Listando volumes**
Basta usar o **docker volume ls**

### Avançado
**Link entre containers**
Para trabalhar-se com link entre containers, não é necessário expor portas prar conexão de ambos os lados.
**É sempre muito importante nomear o container, nomeando o container, podemos trabalhar com o nome dado de diversas formas, dentre elas, fazer o link entre eles.**
```
docker run -it --name NOME ubuntu:14.04 /bin/bash
```
o parâmetro --name dará uma identificação ao container, verifique dando o comando **docker ps**

 O Link permite que você trafegue informações entre os containers de forma segura, pois quem conhece um container conhece apenas o seu par definido no link. Quando você configurar um link, você cria um elo de ligação entre um container de origem e um container de destino. Para criar um link, você deve utilizar o parâmetro **--link**. Em primeiro lugar, deve-se criar um container que será origem de dados para outro container, desta vez vamos criar o container de banco de dados:
 ```
 docker run -it --name rb anser/postgres:1.1
 ```

**A parte mais básica de link entre container está no arquivo: Estudo básiso, vamos avançar mais um pouco.**

**Docker logs**

Para termos o log de tudo que ocorre até o momento da execução do comando, usamos o comando **docker logs ID**:
```
docker logs ID
```

**Copiar arquivo do container em execução**
```
docker cp <containerID>:local/aonde/está/o/arquivo.txt
/local/que/quer/jogar
```
