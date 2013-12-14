lxc-ovh
=======

Debian vm configuration
-----------------------

### Template file

Copy the following file in the template folder of lxc:
```
https://raw.github.com/ArnaudValensi/lxc-ovh/master/files/templates/lxc-debian-custom
```

### VM creation
   lxc-create -n name -t debian-custom

### Host configuration

Add this :
```
       post-up echo 1 > /proc/sys/net/ipv4/ip_forward
       post-up iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE

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
       post-up iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE

   auto br0
   iface br0 inet static
       address 192.168.10.1
       netmask 255.255.255.0
       bridge_ports none
       bridge_stp off
       bridge_fd 0
       bridge_maxwait 5
```

### Autostart VMs

Copy the following file in `/etc/init.d/`:
```
https://raw.github.com/ArnaudValensi/lxc-ovh/master/files/init.d/lxc
```

Then:
```
$ chmod +x /etc/init.d/lxc
$ update-rc.d lxc defaults
```

Now to autostart a vm you can add `1` in a file named `autostart` in the vm folder:
```
echo "1" > /path/to/lxc/myvm/autostart
```

### References

I followed this tutorial for the network configuration (2.4.3.1. Mode NATé):
[http://www.delloye.org/linux/lxc.html]
