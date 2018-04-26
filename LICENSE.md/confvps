#!/bin/bash
clear
#by crazy

x="ok"
while true $x != "ok"
do

function  add_user() {
echo ""
echo -e "\033[1;32mNome do usuario:\033[1;37m"
read -p "" nome
echo -e "\033[1;32mSenha do usuario:\033[1;37m"
read -p "" pass
echo ""
useradd $nome -M -s /bin/false && echo $nome:$pass | chpasswd && sed -ri 's/^# (%sudo.+ALL=(ALL).+NOPASSWD: ALL)/\1/' /etc/sudoers
clear
echo ""
echo -e "\033[1;32mCriado com sucesso!"
echo -e ""
echo -e "\033[1;32mUSUARIO\033[1;37m $nome"
echo -e "\033[1;32mSENHA\033[1;37m $pass"
sleep 2
}

function del_user() {
echo -e "\033[1;36mLista de Usuarios \033[1;37m"
echo ""
awk -F : '$3 >= 500 { print $1 }' /etc/passwd | grep -v '^nobody' ; tput sgr0
echo ""
echo -ne "\033[1;32mNome do usuario a ser removido:\033[1;37m "; read user
if [[ -z $user ]]
then
	echo -e "\033[1;31m Vc digitou um nome invalido\033[0m"
	exit 1
else
	if [[ `grep -c /$user: /etc/passwd` -ne 0 ]]
	then
		ps x | grep $user | grep -v grep | grep -v pt > /tmp/rem
		if [[ `grep -c $user /tmp/rem` -eq 0 ]]
		then
			userdel --force $user 1>/dev/null 2>/dev/null
		  echo ""
         echo -e "\033[1;31mO Usuario \033[1;32m$user \033[1;31mfoi removido com sucesso\033[0m"
          sleep 1
			exit 1
		else
			echo -e "\033[1;31mUsuario \033[1;31mconectado \033[1;32mdesconectando...\033[0m"
			pkill -f "$user"
			userdel --force $user 1>/dev/null 2>/dev/null
			echo ""
          echo -e "\033[1;31mO Usuario \033[1;32m$user \033[1;31mfoi removido com sucesso\033[0m"
          sleep 1
			exit 1
		fi
	else
		echo -e "\033[1;31m O Usuario \033[1;32m$user \033[1;31mnão existe\033[0m"
	fi
fi
}

function monitor() {
users=$(awk -F : '$3 > 900 { print $1 }' /etc/passwd |grep -v "nobody" |grep -vi polkitd |grep -vi systemd-[a-z] |grep -vi systemd-[0-9] |sort)

echo -e "\E[44;1;37mUsuario        Status         Conexão    \E[0m"
echo ""
echo ""
for user in $users; do
	if [[ $(ps -u $user |grep sshd |wc -l) -eq 0 ]]; then
		status=$(echo -e "\033[1;31mOffline \033[1;36m       ")
		echo -ne "\033[1;36m"
		printf '%-14s%-18s%s\n' "$user" " $status" "   $(ps -u $user |grep sshd |wc -l)" 
	else
		status=$(echo -e "\033[1;32mOnline\033[1;36m         ")
		echo -ne "\033[1;36m"
		printf '%-14s%-18s%s\n' "$user" " $status" "   $(ps -u $user |grep sshd |wc -l)"
	fi
echo -e "\033[0;34m═════════════════════════════════════════\033[0m"
done
}

function conf_squid () {
IP=$(wget -qO- ipv4.icanhazip.com)
tput setaf 1 ; tput bold ; read -p "Para continuar confirme seu IP :" -e -i $IP ipdovps ; tput sgr0
sleep 2s
apt-get install squid3 -y > /dev/null
echo ".claro.com.br
.claro.com.sv
.facebook.net
.netclaro.com.br
.speedtest.net
.tim.com.br
.vivo.com.br
.oi.com.br"> /etc/squid3/payload.txt

mv /etc/squid3/squid.conf /etc/squid3/squid.conf.old
echo "acl url1 dstdomain -i 127.0.0.1
acl url2 dstdomain -i localhost
acl url3 dstdomain -i $ipdovps

acl payload dstdomain -i "/etc/squid3/payload.txt"

http_access allow url1
http_access allow url2
http_access allow url3
http_access allow payload

http_access deny all
 
http_port 80
http_port 3128
http_port 8080
http_port 8799
visible_hostname SSHPLUS
 
via off
forwarded_for off
pipeline_prefetch off" > /etc/squid3/squid.conf
service squid3 restart > /dev/null
echo ""
clear
echo -e "Squid configurado com sucesso!"
sleep 3
    menu
}

function otimiz_server () {
clear 
echo -e "\033[1;37mLIMPANDO MEMORIA \033[1;32mRAM\033[0m"
usoram2=$(free -m | awk 'NR==2{printf "%.2f%%\t\t", $3*100/$2 }')
sleep 3
sync  
echo 3 > /proc/sys/vm/drop_caches 
sync && sysctl -w vm.drop_caches=3 1>/dev/null 2>/dev/null 
sysctl -w vm.drop_caches=0 1>/dev/null 2>/dev/null 
sleep 2 
clear 
echo -e "\033[0;34m═══════════════════════════════════════════════\033[0m" 
echo "" 
usoram=$(free -m | awk 'NR==2{printf "%.2f%%\t\t", $3*100/$2 }') 
ram1=$(free -h | grep -i mem | awk {'print $2'}) 
ram2=$(free -h | grep -i mem | awk {'print $4'}) 
ram3=$(free -h | grep -i mem | awk {'print $3'}) 
echo -e "\033[1;31m•\033[1;32mMemoria RAM\033[1;31m•\033[0m" 
echo -e " \033[1;33mTotal: \033[1;37m$ram1"
echo -e " \033[1;33mEm Uso: \033[1;37m$ram3" 
echo -e " \033[1;33mLivre: \033[1;37m$ram2\033[0m" 
echo "" 
echo -e "\033[1;37mMemória \033[1;32mRAM \033[1;37mapós a Otimizacao:\033[1;36m" $usoram  
echo ""
echo -e "\033[0;34m═══════════════════════════════════════════════\033[0m"
}
echo -e "\E[44;1;37m                  EdgeOS                  \E[0m"
echo ""
usoram=$(free -m | awk 'NR==2{printf "%.2f%%\t\t", $3*100/$2 }')
ram1=$(free -h | grep -i mem | awk {'print $2'}) 
cpucores=$(grep -c cpu[0-9] /proc/stat)
uso=$(top -bn1 | awk '/Cpu/ { cpu = "" 100 - $8 "%" }; END { print cpu }')
echo -e "\033[1;32m Memória Ram                Processador    "
echo -e " \033[1;31mTotal: \033[1;37m$ram1                \033[1;31mNucleos: \033[1;37m$cpucores"
echo -e " \033[1;31mEm Uso: \033[1;37m$usoram    \033[1;31mEm Uso: \033[1;37m$uso"
echo -e "\033[0;34m═════════════════════════════════════════\033[0m"
echo ""
echo -e "\033[1;36m1\033[1;31m • \033[1;36mCRIAR USUARIO"
echo""
echo -e "\033[1;36m2\033[1;31m • \033[1;36mREMOVER USUARIO"
echo ""
echo -e "\033[1;36m3\033[1;31m • \033[1;36mMONITOR SSH"
echo ""
echo -e "\033[1;36m4\033[1;31m • \033[1;36mCONFIG SQUID"
echo ""
echo -e "\033[1;36m5\033[1;31m • \033[1;36mOTIMIZAR"
echo ""
echo -e "\033[1;36m0\033[1;31m • \033[1;36mSAIR"
echo ""
echo -e "\033[0;34m═════════════════════════════════════════\033[0m"
echo ""
echo -e "\033[1;32mOque deseja fazer ?\033[0m"
read x
clear
case $x in

1)
add_user
clear
;;
2)
del_user
clear
;;
3)
monitor
echo -e "\033[1;32mPressione ENTER para retornar\033[0m"
read
clear
;;
4)
conf_squid
clear
;;
5)
otimiz_server
sleep 2
echo -e "\033[1;32mPressione ENTER para retornar\033[0m"
read
clear
;;
0)
echo -e "\033[1;31msaindo\033[0m"
sleep 2
exit
;;
*)
echo -e "\033[1;31mopcao invalida...\033[0m"
sleep 2
menu
;;
esac
done
