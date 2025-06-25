<h1 align="center">💻APP rodando na Infraestrutura AWS🚀</h1>
<!--<div align="center">--!>

<p align="center" >
    <img 
        alt="AWS"
        title="AWS" 
        width="40px" 
        style="display: block; margin: auto; padding-right: 20px;" 
        src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/amazonwebservices/amazonwebservices-original-wordmark.svg"/>          
    <img 
        alt="Bash" 
        title="Bash"
        width="40px" 
        style="display: block; margin: auto; padding-right: 20px;" 
        src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/bash/bash-plain.svg"/>
    <img 
        alt="Linux" 
        title="Linux"
        width="40px" 
        style="display: block; margin: auto; padding-right: 20px;" 
        src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/linux/linux-original.svg"/>
    <img 
        alt="Docker"
        title="Docker" 
        width="40px" 
        style="display: block; margin: auto; padding-right: 20px;" 
        src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg"/>
    <img 
        alt="Postgresql" 
        title="Postgresql"
        width="40px" 
        style="display: block; margin: auto; padding-right: 20px;" 
        src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-plain-wordmark.svg"/>
</p><br/>

<h2> Descrição do Projeto </h2>

Objetivo: Clonar o pacote da APP Bia do Github para uma instância EC2, usar docker e configurá-la via rede para estar disponível para acesso via internet. </a><br>


<div>
  <h2> Tecnologias </h2>
  - Amazon EC2 <br>
  - Amazon VPC <br>
  - Docker <br>
  - Github <br>
</div> <a>

<h2>1º Passo - Construção da rede (VPC)</h2>
<b> VPC-MAP </b> <br><br>

Antes de provisionar a Instância, primeiro criamos a rede que irá conectar todos os elementos e permitir que a aplicação esteja disponível para internet. Na imagem, é possível ver o Resource Map da rede. Eu criei uma VPC com duas Subnets, uma em cada zona (Us-east-1a e Us-east-1b) e para acesso a internet, criei um Internet Gateway.
<br>
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/VPC.png)

<h2>2º Passo - Criação Role e User</h2>

<b> Role SSM e User jm </b> <br><br>
Optei pelo acesso a EC2 via SSM, então é necessário criar uma role e nela atribuir a Policys permitindo a que EC2 consiga se comunicar com o recurso SSM da AWS. Como estou realizando o acesso direto a minha máquina linux local, precisei também cirar um usuário (User jm) e gerar um par de chaves (Acess Key e Secret Key). 
<br>
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Role-ssm.png)

![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/User-And-AcessKey.png)

<h2>3º Passo - Criação Security Group</h2>
<b>Security Group - EC2</b> <br><br>

Para que o acesso SSM funcione não é necessária a existencia de um security group, no entanto, para que seja possível acessar a APP Bia hospedada da minha EC2, é preciso que seja configurada uma regra de entrada para o tráfego vindo de fora e é ai que o security group entra. Criada a regra de entrada por meio a porta 3001 na seção Inbound Rule. 
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Security-group-Inbound.png)

<h2>4º Passo - EC2 </h2>

<b>Criação EC2</b> <br>
Com a Rede, IAM e Securiy Group criado, temos todas as dependências necessárias para subir a EC2 e poder acessá-la remotamente. <br>

EC2 lançada e configurada na AWS
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/ec2.png)

Conectando remotamente à EC2 via SSM
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/acesso-ssm.png)

<h2>5º Passo - Instalando e Disponibilizando aplicação</h2>

A aplicação que utilizarei é baseada em Node.js e React. Essa aplicação é a mesma utilizada para laboratórios no curso prático que estou realizando de AWS. No arquivo docker-compose.yml está toda a estrtura de funcionalidade da APP, onde não só está construído a APP em sí, mas também a estrutura relacional local (database local). 

1 - Clonando APP do repositório Git direto na minha EC2
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/clonado-projeto-Api.png)

2 - Antes de subir a APP, estou alterando o arquivo Dockerfile com o ip da minha EC2 somada a porta de entrada (34.201.101.49:3001) e posteriormente gerando uma nova imagem Docker após a alteração. 
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Alterando-Ip.png)

3 - Subindo a APP com a execução do comando docker compose up -d
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Api-no-ar.png)

4 - Criado a estrutura relacional para armazenar dados da APP localmente na EC2
![Meu Porjeto](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Estrutura-relacional-criada.png)

5 - APP disponível e armazenando dados localmente 
![Meu Projeto](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Api-disponivel%20e%20persistindo%20dados.png) 
<br><br>

<h4>Diagrama Infraestrutura AWS</h4>

![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/esquema-infra-aws.PNG)
