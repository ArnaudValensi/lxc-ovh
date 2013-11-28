lxc-ovh
=======

VM creation
--------------
   lxc-create -n name -t debian-custom

Host configuration
--------------

Add this :
```
       post-up echo 1 > /proc/sys/net/ipv4/ip_forward
       iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE

   auto br0
   iface br0 inet static
       address 192.168.10.1
       netmask 255.255.255.0
       bridge_ports none
       bridge_stp off
       bridge_fd 0
       bridge_maxwait 5
```

The file should look like this :
```
   auto eth0
   iface eth0 inet static
       address 91.123.123.123
       netmask 255.255.255.0
       network 91.123.123.0
       broadcast 91.123.123.255
       gateway 91.123.123.254
       post-up echo 1 > /proc/sys/net/ipv4/ip_forward
       iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE

   auto br0
   iface br0 inet static
       address 192.168.10.1
       netmask 255.255.255.0
       bridge_ports none
       bridge_stp off
       bridge_fd 0
       bridge_maxwait 5
```
