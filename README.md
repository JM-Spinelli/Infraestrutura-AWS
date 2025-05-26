<h1 align="center">💻API rodando na Infraestrutura AWS🚀</h1>
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

Objetivo: Criar uma infraestrutura altamentente resiliente e atuomatizada Utilizando o Provider Cloud AWS e nela instalar a aplicação BIA (baseada em Node.js e React), disponibilizando-a para o cliente final. </a>

<div>
  <h2> Tecnologias </h2>
  - Amazon EC2 <br>
  - Amazon ECS <br>
  - Amazon ECR <br>
  - Amazon RDS <br>
  - Amazon VPC <br>
  - Docker <br>
  - Github <br>

  <h1 align="center">Infraestrutura Sem Resiliência Aplicada</h1>
<!--<div align="center">--!>

A infraestrutura disposta de tal forma que a aplicação está rodando com docker dentro de uma instância EC2, no entanto, apesar de ter duas subnets (uma em cada Zone Avaiability disntinta), em caso de qualquer falha que gere indisponibilidade para a aplicação, a mesma não terá redundância, pois ainda não está associada a um cluster com load balancer para direcionar o tráfego. <br><br>
<h2>1º Passo - Construção da rede (VPC)</h2>
<b> VPC-MAP </b> <br><br>

Antes de provisionarmos as Instâncias, associamo-las a um cluster e demais componentes da infra, primeiro criamos a rede que irá conectar todos os elementos e permitir que a aplicação esteja disponível para internet. Na imagem, é possível ver o Resource Map da rede. Eu criei uma VPC com duas Subnets, uma em cada zona (Us-east-1a e Us-east-1b) e para acesso a internet, criei um Internet Gateway.
<br>
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/VPC.png)

<h2>2º Passo - Criação Role e User</h2>

<b> Role SSM e User jm </b> <br><br>
Optei pelo acesso a EC2 via SSM, então é necessário criar uma role e nela atribuir a Policys permitindo a que EC2 consiga se comunicar com o recurso SSM da AWS. Como estou realizando o acesso direto a minha máquina linux local, precisei tembám cirar um usuário (User jm) e gerar um par de chaves (Acess Key e Secret Key). 
<br>
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Role-ssm.png)

![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/User-And-AcessKey.png)

<h2>3º Passo - Criação Security Group</h2>
<b>Security Group - EC2</b> <br><br>

Para que o acesso SSM funcione não é necessária a existencia de um security group, no entanto, para que seja possível acessar a api Bia hospedada da minha EC2, é preciso que seja configurada uma regra de entrada para o tráfego vindo de fora e é ai que o security group entra. Criada a regra de entrada por meio a porta 3001 na seção Inbound Rule. 

![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Security-group-Inbound.png)

<h2>4º Passo - EC2 </h2>

<b>Criação EC2</b> <br>
Com a Rede, IAM e Securiy Group criado, temos todas as dependências necessárias para subir a EC2 e poder acessá-la remotamente. <br>

EC2 lançada e configurada na AWS
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/blob/main/EC2.png)

Conectando remotamente à EC2
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Acessando-ec2-diretamente.png)

<h2>5º Passo - Instalando e Disponibilizando aplicação</h2>

A aplicação que utilizarei é baseada em Node.js e React. Essa aplicação é a mesma utilizada para laboratórios no curso prático que estou realizando de AWS. 

1 - Clonando api do repositório Git direto na minha EC2
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Clone-Projeto-Bia.png)

2 - Parando Api e gerando nova build após alteração feita no Dockerfile
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Stop-Projeto-v.png)

3 - Rodando a Api novamente
![Meu Print](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/Api-up-novamente.png)

4 - Api armazenando dados e disponível para acesso através do Ip da EC2 na porta 3001
![Meu Porjeto](https://github.com/JM-Spinelli/Minhas-Imagens/raw/main/BIA.png)

<h1 align="center">Infraestrutura Com Resiliência Aplicada</h1>
<!--<div align="center">--!>
Agora, por meio de serviço como ECS (Cluster), ECR (Repositorio de imagens), RDS (Serviço de Bancos na AWS), ALB e Target Group (Aplication Load Balancer) e EC2 (Maquinas da AWS), teremos uma infraestrutura resiliente para sustentae nossa API BIA.<br><br>


