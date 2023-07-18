## Setup jails

###### Setup directories

```
mkdir -p /jails
chown root:wheel /jails
chmod 750 /jails
```

###### Ifaces

```
ifconfig bridge create

ifconfig epair create
ifconfig bridge0 addm epair0b up
ifconfig epair0b up

ifconfig bridge0 inet 192.168.20.1/24

sysrc net.inet.ip.forwarding=1
```

###### PF Firewall

```
ext_if="vtnet0"       # physical interface
int_if="bridge0" # bridge interface

nat pass on $ext_if from $int_if:network to any -> ($ext_if) # nat
```

```
service pf enable
service pf start && pfctl -sn
sysrc jail_enable="YES"
```

###### Jails host conf

Example wireguard configuration:
```
exec.start = "/bin/sh /etc/rc";
exec.stop = "/bin/sh /etc/rc.shutdown";
exec.clean;
mount.devfs;

host.hostname = $name;
path = "/jails/$name/fs";
exec.consolelog = "/var/log/jail_${name}_console.log";

wg {
        $vif = "epair0a";

        vnet;
        vnet.interface = $vif;

        # workaround
        # https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=238326
        exec.prestop  += "ifconfig $vif -vnet $name";

        allow.chflags;
        devfs_ruleset = 10;
}
```

```
service jail start wg
jls
jexec wg /bin/sh
```

on jail:
```
jail# sysrc defaultrouter="192.168.20.1"
jail# sysrc ifconfig_epair0a="192.168.20.2"
```

## Setup a jail

```
mkdir /jails/${name}/fs
bsdinstall jail /jails/${name}/fs

ifconfig epair create
ifconfig bridge0 addm epair${n}b up
ifconfig epair${n}b up
```

