# Atividade-PB-Compass
Versionamento das atividades realizadas no Programa de Bolsas DevSecOps - Compass.uol.

## Objetivos

Subir uma segunda VM (seguindo as mesmas regras da atividade anterior);

Criar uma apresentação de slides com 1 slide para cada um dos topicos.

## Instruções
1 - Subir uma segunda VM (seguindo as mesmas regras da atividade anterior);

2 - Mudar a VLAN das 2 para BRIGDE e fazer os ajustes necessarios;

3 - Criar uma apresentação de slides com 1 slide para cada um dos topicos:

3.1 - Qual a diferença entre o comando "touch teste" e "echo > teste"

3.2 - Qual o comando para ver o manual de um comando?

3.3 - Explique o permissionamento: "-rwxr--r-x"?

3.4 - O que faz o comando: systemctl status docker? ;

4- Configurar relação de confiança entre duas VM's;

5 - Fazer o versionamento da atividade;

6 - Fazer a documentação explicando o processo de instalação do Linux


## Instalando o Linux
Baixar Uma imagem do Linux FULL ISO no site da Oracle

Na tela INSTALLATION SUMARY, altere em:

LOCALIZATION

KEYBOARD = Portugues;

LANGUAGE SUPPORT = Inglês;

TIME & DATE = Belém;
SOFTWARE

INSTALLATION SOURCE = Local Media - Não alterar;

SOFTWARE SELECTION = Minimal Install;
SYSTEM

KDUMP = Enable - Não alterar;

NETWORK & HOSTNAME = ON Ethernet(enp0s3);

INSTALLATION DESTINATION = Em "LOCAL STANDARD DISK" selecionar o SDA.

Para particionar o disco, em "STORAGE CONFIGURATION" selecionar a opção "Custom" e clicar em "Done".

Surgirá a tela "MANUAL PARTITIONING", clique em "+" para adicionar as partições do disco uma a uma.

Clique em "Done" e em "Accept Changes".

 USER SETTINGS

ROOTPASSWORD = Crie a senha do Root;

USER CREATION = Crie um usuário;

Clique em "Begging Installation".

## Passo a passo

### Alterando Hostname

Altere o arquivo "hostname":

$ sudo vi /etc/hostname

Substitua "localhost" pelo nome desejado.

### Fixando IP
Verifique o seu número de IP e Broadcast:

$ ifconfig -a

Verifique o seu número de Gateway:

$ route -n

Altere o arquivo /etc/sysconfig/network-scripts/ifcfg-enp0s3:

$ sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
Adicione as seguintes linhas ao final do arquivo:

IPADDR= 192.168.0.54

NETMASK=255.255.255.0

GATEWAY=192.168.0.1 

Altere o BOOTPROTO, substituindo "dhcp" por "static".

### Alterando DNS
Altere o arquivo "hosts":

$ sudo vi /etc/hosts
Inclua uma nova linha com o IP fixo e DNS da VM atual. Inclua outra linha com o IP fixo e o DNS da VM que será acessada.

### Configurando SSH
Configure o SSH para iniciá-lo automaticamente junto ao sistema:

$ sudo systemctl enable sshd

Verifique se o SSH está ativo :

$ sudo systemctl status sshd

Bloqueando acesso SSH ao root

Edite o arquivo sshd_config:

$ sudo vi /etc/ssh/sshd_config

Altere a linha "PermitRootLogin", substituindo o "yes" por "no".

Configurando par de chaves

No terminal do cliente, gere o par de chaves:

$ ssh-keygen

Copie a chave para a máquina que será acessada:

$ ssh-copy-id -i /home/usuário/.ssh/id_rsa.pub usuário@dns_servidor

Acesse a outra maquina:

$ ssh usuário@dns_servidor -i ~/.ssh/id_rsa
