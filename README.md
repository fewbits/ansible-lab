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

### Montando o Laboratório

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

Pronto. O laboratório já está criado.

### Instalando o Ansible

Instale o Ansible em seu equipamento (e não nas máquinas virtuais). A instalação
depende do seu Sistema Operacional, mas quase sempre é tão simples quanto:

- `apt-get install -y ansible`

ou

- `yum install -y ansible`

## Preparando o Ambiente
1. Ligue as máquinas virtuais do laboratório (`vm-ansible-01`, `vm-ansible-02`
e `vm-ansible-03`);
1. Em seu computador (e não nas máquinas virtuais), faça o download deste
repositório: `git clone https://github.com/fewbits/ansible-lab.git`;
1. Via terminal, navegue até o diretório do repositório que você acabou
de baixar (`cd ansible-lab`);
1. Abra o arquivo de inventário em um editor de arquivos (exemplo: `vim lab`
ou `nano lab`);
1. Verifique que existem três entradas de hosts no inventário, um para cada
máquina virtual de seu laboratório. Verifique também que os `hosts` e `portas`
coincidem com o que foi configurado pelo *Oracle VirtualBox*. Por se tratar de
um laboratório *genérico*, estamos utilizando uma estrutura de rede
do VirtualBox que funciona mesmo que você não esteja conectado a uma rede real.
 Note que, em ambientes reais, os parâmetros `ansible_host` e `ansible_port`
 serão diferentes (o `ansible_port` quase sempre será 22, que é a porta padrão
 para conexões SSH).
1. Feche o arquivo de inventário;
1. Troque chave entre o seu computador e as máquinas virtuais executando os
comandos abaixo:
1. `ssh -p 7122 root@127.0.0.1` (quando solicitado senha, digite **zxczxc**);
1. `ssh -p 7222 root@127.0.0.1` (quando solicitado senha, digite **zxczxc**);
1. `ssh -p 7322 root@127.0.0.1` (quando solicitado senha, digite **zxczxc**);
________________________________________________________________________________

## Utilizando o Repositório
________________________________________________________________________________

Estando tudo configurado, basta começarmos a utilizar o *Ansible* no laboratório
criado.

### Executando comandos ad-hoc

Os comandos *ad-hoc* do *Ansible* são executados através do comando `ansible`. 
São comandos simples que podem ser executados de forma pontual, em um, alguns ou
todos os hosts de nosso inventário.

Execute os comandos abaixo para testar o **ad-hoc**:

Pingando todos os servidores:
```
ansible -i lab all -m ping
```

*Hello World* nos servidores *Web*:
```
ansible -i lab web-servers -m shell -a "Hello World!"
```

Instalando um pacote no servidor vm-ansible-02:
```
ansible -i lab vm-ansible-02 -m yum -a "name=git state=present"
```

Desinstalando um pacote do servidor vm-ansible-02:
```
ansible -i lab vm-ansible-02 -m yum -a "name=git state=absent"
```

Visualizando as configurações (*facts*) de todos os servidores:
```
ansible -i lab all -m setup
```

### Executando um Playbook Simples

Agora executaremos um playbook bastante simples. Playbooks são executados
através do comando `ansible-playbook`.

O playbook executado será o **git-ansible-lab.yml**. Este playbook simplesmente
baixa o conteúdo deste repositório para os servidores do inventário.

Executando o playbook em todos os hosts do inventário:
```
ansible-playbook -i lab git-ansible-lab.yml
```

> Dica: Abra o playbook em um editor de texto para entender como ele funciona.

### Executando um Playbook Intermediário

Em seguida, executaremos um playbook um pouco mais inteligente:
`lab-configure.yml`.

Este playbook executa mais de uma tarefa: Adiciona uma conta de usuário
em todos os hosts, baixa este repositório em todos os hosts, instala
o Apache Web nos servidores do grupo `web-servers` e instala o MySQL/MariaDB
nos servidores do grupo `database-servers`. Além disso, ele utiliza conceitos
como variáveis e roles, dois recursos que fazem com que os mesmos scripts
possam ser reutilizado em diversas tarefas. Estes conceitos são importantes
e fazem parte das *"Melhores Práticas"* do *Ansible*.

Executando o playbook em todos os hosts do inventário:
```
ansible-playbook -i lab lab-configure.yml
```

> Dica: Abra o playbook em um editor de texto para entender como ele funciona.


Por enquanto é só. Utilize este repositório para aprender o básico sobre
o *Ansible*. Edite os arquivos deste repositório, crie seus próprios playbooks
e roles, e continue estudando através da documentação oficial do projeto:
[Ansible Documentation](http://docs.ansible.com/). Esta documentação contém
uma explicação minuciosa de todos os recursos da ferramenta, bem como todas as
novidades que vão surgindo de tempos em tempos.

