# Documentação InteliNetwork

> O documento foi dividido por integrante do grupo, pois a etapa inicial do desenvolvimento foi a divisão de tarefas. Cada integrante ficou responsável por uma ou mais tarefas e documentou ela em paralelo com a resolução da mesma.

## Diário de Bordo

> Abaixo, está uma tabela que lista as atividades realizadas pelo grupo. A tabela está em ordem cronológica e cada linha explica qual era o objetivo da atividade. O diário de bordo foi colocado na documentação para facilitar uma suposta réplica, ou seja, olhando pela sequência de atividades dos membros pode-se ter uma ídeia de como começar e terminar a implementação completa.

| Data     | Título | Responsável     | Descrição |
|    :----:   |    :----:   |    :----:   |    :----:   |
| 17/03/2019  | Software para montar a rede em um ambiente virtual | Grupo Todo | O software definido para ser utilizado foi o _Cisco Packet Tracer_ |
| 20/03/2019  | Remontando a rede | Grupo Todo | O ambiente apresentado na descrição do trabalho foi remontado no software escolhido pelos integrantes |
| 02/04/2019  | Definição da solução | Grupo Todo | Concordou-se que utilizaríamos o _pfSense_ como a principal ferramenta para atender os objetivos do trabalho |
| 09/04/2019  | Primeiros Testes | João Vitor | Duas máquinas virtuais instaladas e a base que se necessitava para saber utilizar foi aprendida |
| 17/04/2019  | Modelo de Proposta Comercial | Vinicius | O modelo de proposta comercial foi apresentado aos demais integrantes do grupo |
| 22/04/2019  | Sugestões de logo e nome da empresa | Vinicius | Foram criados alguns nomes e logotipos para a empresa fictífica. Posteriormente, eles foram postos para votação |
| 23/04/2019  | Divisão de Tarefas | Grupo Todo | Todos os problemas apresentados na rede foram divididos entre os integrantes do grupo. Os integrantes tiveram a possibilidade de argumentar sobre os problemas que queriam resolver |
| 27/04/2019  | Conferência | João Vitor | Instalação das máquinas virtuais refeitas com nova versão do VirtualBox (software de virtualização escolhido) e constatado que um erro do dia anterior ocorreu em virtude da versão do mesmo |
| 01/05/2019  | Criando e testando o servidor de arquivos | Vinicius | Criação de um servidor de arquivos com o UbuntuServer e o Samba4 em uma máquina virtual, para testá-lo foram utilizadas outras máquinas virtuais com Windows7 | 
| 04/05/2019  | Configuração DNS | Kelwin | Configuração do servidor de DNS pelo PFSense. Realização de testes para resolução de endereços de domínio |
| 10/05/2019  | Testando Novamente | João Vitor | Todos os testes foram refeitos para ajustar pontos pendentes e preparar a apresentação |
| 11/05/2019  | Configuração das VLANs | Cristian | Criação das interfaces das VLANs e configurações do DHCP |
| 15/05/2019  | Definição de nome e logo da empresa | Grupo Todo | A votação havia sido feita através da ferramente Trello, porém em aula que ocorreu a definição final do nome e do logotipo |
| 15/05/2019  | Instalação no Laboratório | João Vitor | Criando o ambiente local nas máquinas do laboratório e demonstrando ao professor o que vamos fazer (a configuração está incompleta) |
| 15/05/2019 | Testes em Laboratório | Vinicius | O servidor de arquivos foi testado na rede do laboratório com múltiplas máquinas físicas acessando ele |
| 18/05/2019  | Reconfiguração DNS | Kelwin | Após sugestão do professor, foi feita uma nova configuração de DNS para fazer buscas no servidor local |
| 22/05/2019  | Validando a Solução | João Vitor | Finalizando a configuração do ambiente no laboratório e juntando a solução dos problemas **1** e **3** com os demais problemas solucionados pelos outros integrantes do grupo |
| 22/05/2019  | Testes das VLANs | Cristian | Realização de testes das VLANs no laboratório |
| 22/05/2019  | Validando a Solução | Vinicius | Últimos testes realizados com o servidor de arquivos. O servidor estava com sua configuração completa, ou seja, grupos definidos conforme departamento da empresa |

## Controle de Banda e Bloqueio de Sites

> Este documento contém o passo a passo realizado para possibilitar a criação de duas máquinas virtuais (utilizando o programa **VirtualBox**), uma com o software _firewall pfSense_ instalada e a outra com o _Debian 9_ para servir como um cliente e demonstrar as regras e restrições em funcionamento.

### Configurações Iniciais

> Configurações feitas antes de configurar propriamente o teste

- Baixar o virtualbox (.deb) do site oficial (https://www.virtualbox.org/)
- Rodar o `dpkg -i install` (no linux)
- Alguns erros de biblioteca ocorreram e eu executei:
	- `sudo apt-get install libqt5opengl5`
	- `sudo apt --fix-broken install`
	- Rodar o `dpkg -i` de novo
- Depois um erro ocorreu ao tentar iniciar uma nova máquina, aí executei:
	- `sudo apt install gcc`
	- `sudo /sbin/vboxconfig`
	
### Criando a máquina Debian (cliente)

##### Configurações Externas

- 3 GB de RAM
- Criar um disco rígido virtual agora > VDI > Dinamicamente Alocado > 50 GB
- Configurações > Armazenamento > Controladora: IDE > Escolher a .iso do Debian
- Configurações > Rede > Adaptador 1 > Rede Interna
- Iniciar > Install > 'Next next' em quase tudo, exceto o item abaixo:
	* Usuário: {root} / Senha: {123456}
	* Usuário: {suporte} / Senha: {123456}
- [EXCEÇÃO] "Escolha o software a ser instalado":
	* Desmarca servidor SSH
	* Desmarca utilitários do sistema padrão
	* Marca o MATE
- Na hora de escolher o grub (tem que ter espaço de armazenamento suficiente):
	* grub (sim)
	* escolhe o /dev/sda que vem default
	
##### Configurações Internas (utilizando a interface gráfica)

- Quando acabar a instalação e o firewall (pfSense) estiver configurado, fazer:
	* Trocar a placa de rede: Configurações > Rede > Adaptador 1 > Rede Interna 
	* Desabilitar a rede e habilitar de novo (pelo sistema operacional)
	* O ping para 192.168.200.251 (ou o ip da LAN do pfSense) deve funcionar
		+ Consequentemente, o acesso ao pfSense (web) funciona também
- Instalar o pacote net-tools (ifconfig)
	* su -
	* apt-get install net-tools	
	* Posso mostrar que ele já pegou o ip 192.168.200.2, o primeiro do range que eu configurei no meu DHCP Server
	
### Criando a máquina pfSense (firewall)

##### Configurações Externas

- Máquina > Novo
- Digitar "freeb" > FreeBSD (64-bit)
- 512 MB de RAM
- Criar um disco rígido virtual agora > VDI > Dinamicamente Alocado > 5 GB
- Configurações > Armazenamento > Controladora: IDE > Escolher a .iso do pfSense
- Configurações > Rede:
	- Adaptador 1: Placa em modo Bridge
		* Aqui escolhi "wlp2s0b1" porque estou no notebook
		* Se fizer cabeado, escolhe a "eno1"
	- Ativar adaptador 2
		* Rede interna

==_Colocar as duas placas em modo promíscuo => **Permitir Tudo**_==

- 4 *next's* (opções default)
- *No* (não desejamos fazer nenhuma mudança manual)
- Instalar e depois *rebootar*
- *Accept* > Desligar a máquina e remover o disco
- Reiniciar a máquina

##### Configurações Internas (terminal)

1. Entra na opção 1 (assign interfaces | define quem é a WAN e quem é a LAN)
	- n (não)
	- minha WAN = em0 (por causa do VB, poderia ser outra coisa)
	- em1
		* y (proceder)
2. Entra na opção 2 (Set interfaces IP address | definiremos um endereço IP para cada uma das interfaces de rede)
	- WAN = respeita o que o provedor entregar (para poder navegar)
	- 2 = IP único (que não seja usado no local de teste - URI)
		* 192.168.200.251 (será o ip de acesso pela web => admin / pfsense)
		* Máscara: 24
		* Enter > Enter
	- Habilita DHCP Server na LAN (é quem vai entregar o IP para o cliente - debian)
		* 192.168.200.2 (início) e 192.168.200.200 (final)
		* y (se eu desejo habilitar o portal web na LAN)

##### Configurações Internas (utilizando o portal web)

- 1ª coisa depois de aceitar os termos para usar o pfsense: atualizar a versão
	* Fiz da v2.4.2 para a v2.4.3_1
- Sistema > Preferências > Hardware > Visualização (para redimensionar a janela)

##### Controle de Banda (problema 1)

1. DHCP Server
	- DNS = 8.8.8.8 (parte do Kelwin)
2. LAN (existe um gráfico no topo da página para listar todas as máquians)
	- IP pode ser colocado estático (final 201, por ex, pois é o 1º do range "fixo", que deixei após o 200)
	- Para fazer isso, basta clicar no '+' à esquerda e definir um IP
	- Será necessário desabilitar e habilitar de novo a rede do cliente debian
	- ==AQUI A MÁQUINA JÁ DEVE NAVEGAR==
3. Deletar regra para IPv6
4. Firewall > Traffic Shapper
	- No pfSense, eu poderia contrlar esta demanda por VLANs, por grupo de IPs, por IP específico ou por placa de rede
	- Porém, como queremos simular no VirtualBox e não temos um switch físico, farei por IP (que é o mais simples)
		* Fazendo assim, no entanto, existe a possibilidade de controlar a banda por porta (isso é um extra)
4.1. Limiters > New
	- Para criar uma nova regra de limite, preencher os campos como segue abaixo:
		* enable (habilitar)
		* "up_financeiro" (nome)
		* 1024 Kb (10% do total de banda que eu tiver)
			+ Para download, usar 20%
		* Descrição (algo que facilite o reconhecimento do funcionamento da regra)
		* Save
		* Appy
5. Firewal > Rules > LAN
	- Visualizar que existe uma regra 'liberando tudo'
	- Firewall > Alias (Criando um grupo)
		* Add 
		* Nome = "pcs_financeiro"
		* IP = "192.168.200.201"
		* Save and Apply
	- Firewall > Rules > WAN
		* Desabilitar 'bogon network' para que esse tipo de acesso (no caso do meu teste, o ip do pfSense ficou com 192.168.140.36 - classe C) possa acessar
		* A segunda regra vai ter como descrição algo do tipo 'Block bogon networks'
			+ Clicar na engrenagem
			+ Rolar até o fim da página
			+ As duas últimas opções serão um checkbox (devem ser desmarcados: Block private networks and loopback addresses e Block bogon networks)
	- Firewall > Rules (essa regra permitirá que acessemos o pfSense do browser do hospedeiro)
		* Add
		* Action: Pass
		* Interface: WAN
		* Address Family: IPV4
		* Protocol: TCP
		* Source: any
		* Destination: This firewall (self)
		* Destination Port: other	404		other 	404
		* Description: acesso_externo
			+ Em resumo: "tudo que vem em direção ao firewall na porta customizada (WAN), autoriza para liberar a interface para fora (isso possibilitaria que o admin de redes acesse o firewall de casa, por exemplo)"
		
6. System > Advanced
	- TCP Port = "404"
		* O firewall deve ser acessado nessa porta, a partir de agora
	- Na mesma página, habilitar senha no console (é segurança a mais, pois só quem souber a senha poderá utilizar o console do pfSense) = Console menu (CHECK)
7. Firewall > Rules > LAN > Add
	- Configuração da Regra
		- Action: Pass
		- Interface: LAN
		- Address Family: IPV4
		- Protocol: Any
		- Source: Single host or alias		'pcs_financeiro'
		- Destination: any
		- Description: liberando_acesso_pcs_financeiro_com_controle_de_banda
		- Save and Apply
	- Desabilitar a regra default (e jogar para baixo, última)
7.1. TESTANDO
	- Tentar fazer download de algo
		* (133 Kb/s, nesse caso) 
	- Cancelar!
7.2.1. Habilitar a regra default novamente (e comparar com a velocidade de download agora)
	- Como a regra default permite mais banda, a velocidade vai aumentar
7.2.2. Começar o download, trocar o ip do 'ALIAS' e notar que o download continua
	- Isso ocorre porque o websocket já foi aberto
		* Caso necessário, enquanto eu testava, encontrei uma opção que lista todas as conxões abertas > permite reiniciar todas > seria o ideal para matar todas conexões, mas com o contra de que todas as conexões legítimas deveriam se conectar de novo)
	- Cancelar o download e tentar de novo
		* NÃO vai funcionar!



##### Bloqueio de Sites (problema 5)

> [ https://www.youtube.com/watch?v=3CRaYShxG7g ]

1. System > Package Manager > Available Pkg
	- "squid"
		* "pfsense-pkg-squid"
		- Install
	- "squidGuard"
		- Install
2. Services > Squid Proxy Server > General
	- Enable Squid Proxy: check
	- Resolve DNS IPv4 First: check
	- Transparent HTTP Proxy: check   [ !! IMPORTANTE !! ]
	- SSL Proxy Port: apagar (era 3129)
	- SSL Proxy Compatibility Mode: Intermediate (era Modern)
	- Remote Cert Checks: "Accept..." (1ª opção)
	- Enable Access Logging: check
	- Visible Hostname: 'localhost; proxy; tanto faz...'
	- Administrator's Email: 'ti@intellinetwork.com.br'
3. Services > SquidGuard Proxy Filter > General Settings
	- Enable: check  &&  Apply
	- Logging options: habilita os 3 ( Enable GUI log; Enable log; Enable log rotation )
	- Blacklist: check
	- Blacklist URL: http://www.shallalist.de/Downloads/shallalist.tar.gz (blacklist pronta disponibilizada pela comunidade)
	- Save
	- Apply
4. Services > SquidGuard Proxy Filter > Blacklist
	- Download
5. Services > SquidGuard Proxy Filter > Common ACL
	- Target Rules List (expandir)
	- Proxy Denied Error: 'Acesso Negado! - IntelliNetwork' (Mensagem de Erro customizada para provar que nossa regra funcionou hehe)
	- Use SafeSearch engine: check
	- Log: check
	- Save  &&  Apply (General)
6. Tentar acessar 'globo.com' [ !! NÃO ACESSA !! ]
7. Services > SquidGuard Proxy Filter > Target categories
	- Add
	- Name: 'Sites_Liberados'
	- Domain List: 'globo.com'
	- Log: check
	- Save  &&  Apply (General)
8. Services > SquidGuard Proxy Filter > Common ACL
	- Target Rules List (expandir)
	- Encontrar a linha com 'Sites_Liberados'
	- Colocar como 'access': 'whitelist' (passa sempre)
	- Save  &&  Apply (General)
9. Tentar acessar 'globo.com' [ !! AGORA ACESSA !! ]
10. Se quiser inverter e liberar todos os sites por categoria por padrão, podemos bloquear apenas algumas domínios:
	- Altera na Target Rules List para liberar todas por padrão (última linha)
	- Criar uma Target Categories 'Sites_Bloqueados'
	- Adiciona o domínio no 'Domain List'
	- Na Target Rules List, marca essa categoria como 'deny'
	- Apply ( feito =] )
11. Bloqueando sites HTTPS
	- System > Cert. Manager > Add
	- Descriptive name: 'Proxy'
	- Method: 'Create an internal...'
	- Country Code: BR
	- State or Province: RS
	- City: Erechim
	- Organization: IntelliNetwork
	- E-mail Address: ti@intellinetwork.com
	- Common Name: proxy-ca
	- **Services > Squid Proxy Server > General**
		- HTTPS/SSL Interception: check
		- CA: Proxy (selecionar)

## Servidor DNS

> Este documento contém o passo a passo realizado para habilitar o uso do serviço de DNS e resolvedor de DNS.

##### Resolução de DNS (problema 2)

1. DNS Resolver
	- Configuração pode ser encontrada em: Services > DNS Resolver
	- Na ultima versão disponível (2.4.4-p1) vem habilitado por padrão.
	- Apenas alguns campos deverão ser alterados:
		* Outgoing Network Interfaces: selecionar a interface WAN (Por padrão envia para todas as interfaces).
		* DNSSEC: utiliza autenticação na resolução, vem habilitado por padrão e, deve-se deixar habilitado caso o servidor de DNS externo possua suporte.
	- Salvar e aplicar alterações.
	- Após estes passos, a resolução de nomes de domínio já deve estar funcionando.
	
2. Configurações opcionais
	- Em System > General Setup é possivel adicionar servidores DNS que o resolvedor irá direcionar as buscas de DNS.
	- Caso a opção DNS Server Override seja desabilitada, deve-se informar um servidor de DNS em DNS Server, acima da configuração inicialmente citada.

## Divisão de Redes

> Este documento contém o passo a passo realizado para separar a rede dos visitantes da rede interna da empresa.

##### Divisão de redes (problema 3)
1. Criação das VLANs.
	- Acesse o pfsense.
	- Interfaces > Assignmetns.
	- VLANs > Add.
	- Selecione a "Parent Interface" em1 (LAN), defina uma VLAN Tag, caso queira uma descrição e clique em salvar (repita esse processo para criarmos as duas VLANs).
	- Acesse "Interface Assingnmets" e adicione as VLANs criadas e clique em salvar.
2. Configurando as novas interfaces
    - Acesse Interfaces OPT1 (Nome da primeira VLAN criada - WIFI).
    - Renomear para WIFI.
    - Marque a opção "Enable interface".
    - Selecione a opção "Static IPv4" em "IPv4 Configuration Type".
    - Em IPv4 Address coloque o IP '192.168.10.1/24.
    - Clique em salvar.
	- Repita esse processo para a OPT2(REDE INTERNA) colocando o IP 192.168.20.1/24
3. Configurando DCHP para as interfaces
	- Services > DHCP Server.
	- Selecione WIFI.
	- Habilite o DHCP.
	- Coloque o range "192.168.10.100 e 192.168.10.200".
	- Clique em salvar.
	- Selecione a REDEINTERNA, siga os mesmos passos e coloque o range "192.168.20.100 e 192.168.20.200".

## Servidor de Arquivos

>   Essa parte da documentação contém o passo a passo para criar um servidor **UbuntuServer** com o **Samba4**. Ele será o servidor de arquivos do projeto e poderá ser testado com clientes **Windows**.

### Pré-Requisitos

- Para que seja criado o ambiente, é necessário ter instalado o **virtualbox**, ele pode ser encontrado nesse [nesse link](https://www.virtualbox.org/). 

- Para realizar todos os testes será necessário ter na rede o maior número possível de máquinas com o sistema operacional **Windows**, elas podem ser máquinas físicas ou virtuais (pode-se utilizar o **virtualbox**).

### Criando o Servidor de Arquivos com o UbuntuServer (Problema 4)

##### Configurações Externas (VirtualBox)

- Após criar a máquina virtual, ela deve ser selecionada e o botão de configurações acionado. Abaixo, estarão listadas as configurações que devem ser alteradas:

    * Em **Sistema**, defina a Memória Base para 1GB.
    * Em **Rede**, coloque uma placa de rede em Modo Bridge.
    
- Iniciar a máquina virtual.

##### Configurações Internas (Terminal)

- Efetuar login no Ubuntu Server.

- Entrar em modo root: 
    * Execute o comando: `sudo su -`
    * Insira a senha do usuário.

- Como trata-se de um servidor, será necessário deixar o IP da máquina estático:
	* Abrir o arquivo de configurações com o seguinte comando: `nano /etc/network/interfaces`
	* na linha `iface enp0s3 inet dhcp`, substituir o **dhcp** por **static**
	* adicionar as seguintes configurações nas linhas abaixo:
		- address 192.168.0.100
		- netmask 255.255.255.0
		- gateway 192.168.0.1
		- dns-nameservers 8.8.8.8 8.8.4.4

- Testando a LAN:
	* ping 192.168.0.13

- Testando o DNS:
	* ping google.com

- Atualizar os pacotes da máquina
	* apt-get update

- Instalar o samba 4:
	* apt-get install samba

- Adicionando os grupos:
	* groupadd comercial
	* groupadd financeiro
	* groupadd contabilidade
	* groupadd rh

- Adicionando os usuários
	* useradd -m vinicius -G comercial
	* useradd -m kelwin -G financeiro
	* useradd -m joao -G contabilidade
	* useradd -m cristian -G rh

- Dando acesso ao Samba para os usuários:
	* smbpasswd -a vinicius
	* smbpasswd -a kelwin
	* smbpasswd -a joao
	* smbpasswd -a cristian

	**OBS: De preferência utilizar a mesma senha do Linux para o Samba**

- Configurando o Samba:
	* Abrir o arquivo de configuração com o comando `nano /etc/samba/smb.conf`
		- Inserir a seguinte informação: `security = user`
	* Reiniciando o serviço:
		- Execute o comando: `service smbd restart`

- Criando as pastas que serão compartilhadas:
	* Acessando a pasta: `cd /mnt`
	* `mkdir comercial`
	* `mkdir contabilidade`
	* `mkdir financeiro`
	* `mkdir rh`
	* `mkdir corporativo (pasta pública)`

- Alterando as permissões e os grupos das pastas:
	* `chgrp comercial comercial/`
	* `chgrp contabilidade contabilidade/`
	* `chgrp financeiro financeiro/`
	* `chgrp rh rh/`
	* `chmod 777 corporativo`

- Adicionando as pastas que serão compartilhadas:
	* Abrir (novamente) o arquivo /etc/samba/smb.conf
	* Adicionar as seguintes linhas no final do arquivo
		```
        [corporativo]
		comment        = corporativo
		path           = /mnt/corporativo/
		browseable     = yes
		writable       = yes
		create mask    = 0777
		directory mask = 0777
		public         = yes
		
		[comercial]
		comment        = comercial
		path           = /mnt/comercial/
		writable       = yes
		create mask    = 0770
		directory mask = 0770
		valid users    = +comercial

		[financeiro]
		comment        = financeiro
		path           = /mnt/financeiro/
		writable       = yes
		create mask    = 0770
		directory mask = 0770
		valid users    = +financeiro

		[rh]
		comment        = rh
		path           = /mnt/rh/
		writable       = yes
		create mask    = 0770
		directory mask = 0770
		valid users    = +rh
        ```
- Reiniciando o servico do Samba:
	* `service smbd restart`

- Criando a lixeira, ela manterá todos arquivos exclusos dentro dessas pastas:
	* Acessar o arquivo de configuração do samba: `nano /etc/samba/smb.conf`
	* Adicionar as seguintes linhas abaixo da tag **[global]** para configurar a lixeira:
        ```
		vfs objects         = recycle
		recycle:keeptree    = yes
		recycle:versions    = yes
		recycle:repository  = /mnt/lixeira/
		recycle:exclude     = *.~*, ~*.*, *.bak, *.old, *.iso, *.tmp
		recycle:exclude_dir = temp, cache, tmp
        ```
	* Adicionar as seguintes linhas no final do arquivo para criar o diretório da lixeira:
        ```
		[lixeira]
		path       = /mnt/lixeira/
		writeable  = yes
		browseable = no
        ```
	* Dentro do repositório /mnt, criar a lixeira e dar permissão total:
		- `mkdir lixeira && chmod 777 lixeira`

