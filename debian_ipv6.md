# IPv6 on Debian hosts

## `/etc/network/interfaces`

```
auto lo
iface lo inet loopback
iface lo inet6 loopback		# IPv6 on lookback device

[...]

iface eth0 inet6 static
address <ipv6-address>
netmask 64
gateway <ipv6-gateway>

# these should work but somehow don't. Therefore the workaround using /etc/rc.local
#post-up /sbin/ip -f inet6 route add <ipv6-gateway> dev eth0
#post-up /sbin/ip -f inet6 route add default via <ipv6-gateway>
#pre-down /sbin/ip -f inet6 route del default via <ipv6-gateway>
#pre-down /sbin/ip -f inet6 route del <ipv6-gateway> dev eth0
```

## `/etc/rc.local`

Since the hooks in `/etc/network/interfaces` don't work, the interface is brought up using a boot hook.

```
/sbin/ip -6 addr add <ipv6-address>/64 dev eth0
/sbin/ip -f inet6 route add <ipv6-gateway> dev eth0
/sbin/route -A inet6 add default gw <ipv6-gateway> dev eth0
```
