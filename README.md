# ansible-lab

Repository created to be used in lab during an Ansible's Basic Trainning.

Repositório criado para ser utilizado durante o laboratório de um treinamento
básico de Ansible.
________________________________________________________________________________

## Sobre o Repositório

Este repositório tem como objetivo auxiliar no entendimento básico sobre a
ferramenta Ansible.

### Estrutura de arquivos e diretórios

Este repositório é formado pela seguinte estrutura:

Item                | Tipo      | Descrição
------------------- | --------- | ---------
group_vars/         | Diretório | Variáveis aplicadas a grupos do inventário (não utilizado atualmente)
host_vars/          | Diretório | Variávies aplicadas a hosts do inventário (não utilizado atualmente)
roles/              | Diretório | Roles reutilizáveis do Ansible
git-ansible-lab.yml | Arquivo   | Playbook básico: Faz o download deste repositório
lab                 | Arquivo   | Arquivo de inventário
lab-configure.yml   | Arquivo   | Playbook intermediário: Utiliza roles e variáveis
README.md           | Arquivo   | Documentação do repositório (também conhecido como **este arquivo**)

### Roles

As roles disponíveis atualmente são:

Role            | Descrição
--------------- | ---------
common          | *Template* de role - Utilize esta role como exemplo para criar a sua própria role
git-pull        | Faz o download (pull) de um repositório git
system-user-add | Adiciona um usuário ao Sistema Operacional
yum-httpd       | Instala e configura o daemon do Apache Web
yum-mysqld      | Instala e configura o daemon do MySQl/MariaDB
________________________________________________________________________________

## Pré-Requisitos

Para utilizar este repositório, você precisa de um laboratório formado por
três servidores.

Caso tenha recebido o *appliance de VirtualBox* **vm-ansible-lab.ova**, você
já tem os arquivos necessários para criar o seu laboratório e começar a usar
o Ansible.

Siga o roteiro abaixo para configurar seu laboratório:

1. Instale o *Oracle VirtualBox* em seu equipamento. A instalação depende
do seu Sistema Operacional, portanto não será abordada nesta documentação;
1. Abra o *Oracle VirtualBox* e navegue até *"File / Import Appliance..."*;
1. Na janela que abrir, localize e selecione o arquivo **vm-ansible-lab.ova**.
Este arquivo contém as três máquinas virtuais utilizadas no laboratório. Clique
em *"Next"*;
1. Confira as informações exibidas na tela. Verifique se você precisa alterar
alguma configuração (como CPU ou memória). Na dúvida, não altere nada e clique
em *"Import"*;
1. Verifique que três máquinas virtuais foram criadas: *vm-ansible-01*,
*vm-ansible-02* e *vm-ansible-03*;
1. Inicie as três máquinas virtuais. A senha do usuário root é *zxczxc*;
1. Verifique o IP de cada uma das máquinas virtuais digitando `ifconfig` ou
`hostname -I`. Anote o IP de cada uma das máquinas virtuais;
1. No *Oracle VirtualBox*, navegue até *"File / Preferences..."*;
1. Na janela que abrir, selecione *"Network"*. Em *"NAT Networks"*, dê um duplo
clique em *"NatNetwork"*;
1. Na janela que abrir, clique em *"Port Forwarding"*;
1. Na janela *Port Forwarding Rules*, em *IPv4*, adicione três regras clicando
três vezes no ícone *"+"*;
1. Preencha cada uma das três regras da seguinte forma:
* **Name**: Nome da máquina virtual
* **Protocol**: TCP
* **Host IP**: 127.0.0.1
* **Host Port**: 7122 para a primeira VM; 7222 para a segunda VM; 7322 para
a terceira VM;
* **Guest IP**: O IP da máquina virtual;
* **Guest Port**: 22;
1. Pronto. O laboratório já está criado, e você pode prosseguir para o próximo
tópico da documentação.
________________________________________________________________________________

## Utilizando o Repositório
________________________________________________________________________________



























