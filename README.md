LABORATORIO V

========================
PRACTICA 1
========================

sudo nano /etc/default/grub
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo reboot

mount -o remount,rw /sysroot
chroot /sysroot
passwd root
touch /.autorelabel
exit
exit


========================
PRACTICA 2
========================

sudo dnf install pacemaker corosync pcs -y
sudo systemctl enable pcsd --now
sudo passwd hacluster
sudo systemctl stop firewalld
sudo systemctl disable firewalld

sudo pcs host auth nodo1 nodo2 -u hacluster -p 123456
sudo pcs cluster setup clusterHA nodo1 nodo2
sudo pcs cluster start --all
sudo pcs cluster enable --all
sudo pcs status

sudo pcs resource create IPflotante ocf:heartbeat:IPaddr2 ip=192.168.100.50 cidr_netmask=24 op monitor interval=30s
ip a
sudo pcs status


========================
PRACTICA 3
========================

sudo dnf install httpd -y
sudo systemctl enable httpd --now
sudo dnf install keepalived -y
sudo systemctl stop firewalld
sudo systemctl disable firewalld

sudo nano /var/www/html/index.html
sudo nano /etc/keepalived/keepalived.conf

sudo systemctl enable keepalived --now
sudo systemctl restart keepalived

ip a
curl http://192.168.100.50
sudo poweroff
curl http://192.168.100.50
