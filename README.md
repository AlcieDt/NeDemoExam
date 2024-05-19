# NeDemoExam
![Loh](https://github.com)/AlcieDt/NeDemoExam/blob/main/reks.png
![reks](https://github.com/AlcieDt/NeDemoExam/assets/151376255/49f4b6f8-4d38-4173-8d15-dcbe40ff331a)

ISP

systemctl restart network NetworkManager frr.service 
vtysh 
conf t
do sho run
ip forwarding
ipv6 forwarding
do wri
ex
ex
sysctl -a|grep forward
nano /etc/net/sysctl.conf
systemctl restart NetworkManager
systemctl restart frr.service
systemctl enable --now iperf3

HQ-R

systemctl restart network NetworkManager frr.service
apt-get update
apt-get install frr
nano /etc/frr/daemons
systemctl start frr
vtysh
conf t
interface eth0
ip address 10.10.11.2/30
ipv6 address 2000::b:2/126
interface eth1
ip address 192.168.1.1/26
ipv6 address 2000::d:1/126
router ospf
ospf router-id 1.1.1.1
network 10.10.11.0/30 area 0
network 192.168.1.0/28 area 0    
do sho run
ip forwarding
ipv6 forwarding
do wri
ex
ex
sysctl -a|grep forward
nano /etc/net/sysctl.conf
systemctl restart NetworkManager
systemctl restart frr.service
apt-get install dhcp-server
nano /etc/dhcp/dhcpd.conf

#конфиг
option domain-name "HQ-R";
option domain-name-servers 192.168.1.1;                                             
iptables -A FORWARD -i eth1 -s 192.168.0.0/24 -p tcp --dport [порт, указанный в задании] -j DROP
iptables-save > /etc/sysconfig/iptables    
                                                                                          
default-lease-time 6000;
max-lease-time 72000;
authoritative;
subnet 192.168.1.0 netmask 255.255.255.192 {
option routers 192.168.1.1;
} 
#конфиг

nano /etc/sysconfig/dhcpd
chkconfig dhcpd on
service dhcpd start
systemctl status dhcpd
useradd branch-admin -m -c "Branch admin" -U
passwd branch-admin 
useradd network-admin -m -U
useradd network-admin -m -c "Network admin" -U
passwd network-admin
systemctl enable --now iperf3
iperf3 -c 192.168.0.1 -f m --get-server-output

HQ-SRV

dnf install openssh-server
nano /etc/openssh/sshd_config
# Раскомментируйте и измените следующую строку
PermitRootLogin yes
порт ssh прописать тот, который указан в задании
# 
systemctl status ssh
systemctl restart sshd

BR-R

systemctl restart network NetworkManager frr.service
apt-get update
apt-get install frr
nano /etc/frr/daemons
systemctl start frr
vtysh
conf t
interface eth0
ip address 10.10.11.6/30
ipv6 address 2000::c:2/126
interface eth1
ip address 192.168.2.1/28
ipv6 address 2000::e:1/124
router ospf
ospf router-id 1.1.1.2
network 10.10.11.4/30 area 0
network 192.168.2.0/28 area 0    
do sho run
ip forwarding
ipv6 forwarding
do wri
ex
ex
sysctl -a|grep forward
nano /etc/net/sysctl.conf
systemctl restart NetworkManager
systemctl restart frr.service

#бэкапы
nano /var/backup-script/backup.sh
#конфиг для файла с бэкапом

echo "Start backup"

data=$(date +%d.%m.%Y-%H:%M:S)
mkdir /var/backup/$data

cp -r /etc/frr /var/backup/$data
cd -r /etc/nftables /var/backup/$data
cp -r /etc/NetworkManager/system-connections  /var/backup/$data
cp -r /etc/dhcp /var/backup/$data
cd /var/backup
tar czfv "/.data/tar/gz" ./$data
rm -r /var/backup/$data

echo "Done"
#

#то, что нужно сделать с файлом бэкапа
chmod +x /var/backup-script/backup.sh
/var/backup-script/backup.sh
