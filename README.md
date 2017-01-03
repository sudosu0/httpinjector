# [ Comandos da VPS ]
/etc/init.d/ssh restart 
/etc/init.d/squid3 restart 
/etc/init.d/squid3 reload

[Atualização da VPS]

dpkg-reconfigure tzdata

apt-get update && apt-get upgrade && apt-get install squid3 && apt-get install wget && apt-get install nano && apt-get install screen && apt-get install dos2unix

[ COM VPS MANAGER ]

wget http://phreaker56.site88.net/VPSMANAGER/vpsmanagersetup.sh
 
chmod +x vpsmanagersetup.sh
 
./vpsmanagersetup.sh

Após a instalação do vpsmanager ir para [ Configurações ]

[ SEM VPS MANAGER ]

cd /etc/squid3/

wget http://pastebin.com/raw/RpHmzqUk -O squid.conf

wget http://pastebin.com/raw/2Gccd2Zz -O payload.txt

screen
wget http://pastebin.com/raw/yuDq8x0s -O sshlimiter.sh 
dos2unix sshlimiter.sh
./sshlimiter.sh
screen -r

wget http://pastebin.com/raw/ks4t2VaS -O CriarUsuario.sh
dos2unix CriarUsuario.sh
chmod +x CriarUsuario.sh
./CriarUsuario.sh

apt-get install bc
wget http://pastebin.com/raw/4iTN0YtY -O ExpCleaner.sh
dos2unix ExpCleaner.sh
chmod +x ExpCleaner.sh
./ExpCleaner.sh

[ Configurações ]

nano /etc/squid3/squid.conf

nano /etc/squid3/payload.txt

/etc/init.d/squid3 restart

nano /etc/ssh/sshd_config

/etc/init.d/ssh restart

[ FIREWALL ]
nano /etc/rc.local

echo "1" > /proc/sys/net/ipv4/ip_forward 
Isso aqui abaixo libera todas as portas do VPS
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

[ SSHD ]

nano /etc/ssh/sshd_config

port 443 3456
GatewayPorts yes
AllowTcpForwarding yes
#UseDNS yes sem o # usa o DNS do VPS
PermitTunnel yes

/etc/init.d/ssh restart 

---> isso aqui abaixo permite o roteamento atraves da rede tirar soh #
< nano /etc/sysctl.conf

net.ipv4.ip_forward=1 >

[ Adição de Usuários ]

useradd brasil123
passwd brazil123

[ Adição de Usuários com Expiração ]
useradd -e 2016-12-25 -M -s /bin/false nomedousuario

[ Listar Usuários ou Conectados ]
awk -F : '$3 >= 500 {print $1}' /etc/passwd | grep -v '^nobody'

ps aux | grep ssh

O sshlimiter fica sempre logado para derrubar quem tem mas conecções abertas no servidor

[ Instalar OPENVPN Server 2 Usuarios ]

wget http://swupdate.openvpn.org/as/openvpn-as-2.0.10-Debian7.amd_64.deb
dpkg -i openvpn-as-2.0.10-Debian7.amd_64.deb

passwd openvpn

[ Instalar OPENVPN No-GUI + WEBMIN ]

wget http://prdownloads.sourceforge.net/webadmin/webmin_1.820_all.deb

apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python

apt-get -f install

dpkg --install webmin_1.820_all.deb

https://IPSERVER:943/admin

[ OPEN VPN Que Funcionou ]

https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-
debian-8
http://www.pontikis.net/blog/openvpn_on_debian_server
https://en.opensuse.org/SDB:OpenVPN_Installation_and_Setup

Instalar OPENVPN

Open the terminal and install OpenVPN using these commands:

wget git.io/vpn --no-check-certificate -O openvpn-install.sh; bash openvpn-install.sh

/etc/openvpn/server.conf
/etc/init.d/openvpn restart

[ SSLH MULTIPLEX SSH E OPENVPN ]

apt-get install sslh
apt-get install build-essential
apt-get install libwrap0-dev libconfig8-dev

nano /etc/default/sslh

RUN=yes

DAEMON_OPTS="--user sslh --listen 0.0.0.0:443 --ssh 127.0.0.1:22 --ssl 127.0.0.1:443 --pidfile /var/run/sslh/sslh.pid"

/etc/init.d/sslh start
/etc/init.d/sslh restart

nano /etc/init.d/sslh

[Usuários Conectados]
ps -ef | grep sslh

[ COMANDOS DO VPS MANAGER 2.0 ]

[ ajuda ]
Mostra a lista de comandos disponíveis. 
[ addhost ] Comando para adicionar um domínio a lista de domínios permitidos no Proxy Squid. 
[ alterarlimite ] Comando para alterar o número máximo de conexões simultâneas permitidas para um usuário. Usado para definir o funcionamento do SSH Limiter.
[ alterarsenha ] Comando que simplifica a troca de senha de usuário mesmo que ele esteja conectado. 
[ criarusuario ] Comando para criar um usuário com acesso apenas ao túnel SSH (sem acesso ao terminal) com data de expiração e um número máximo de conexões simultâneas permitidas. 
[ delhost ] Comando para remover um domínio da lista de domínios permitidos no Proxy Squid. 
[ expcleaner ] Comando que remove automaticamente todas os usuários que passaram da data de expiração. Use com cuidado. 
[ mudardata ] Comando que altera a data de expiração de um usuário ou reativa um usuário expirado. 
[ remover ] Comando para remover um usuário SSH, mesmo que ele esteja conectado.
[ sshlimiter ] Comando que deve continuar funcionando em segundo plano, desconecta os usuários que ultrapassarem o limite de conexões simultâneas definidos na criação do usuário. 
[ sshmonitor ] Comando que lista os usuários SSH e mostra o número de conexões de cada um. Útil para ver quem está online sem precisar retornar a sessão screen do sshlimiter.
