# Upgrading Debian 8 -> 9

## Steps

1. Create backup (`/etc`, `/home`, `/root`, `/var/lib/mysql`, mysqldump, ...)
2. Update sources in `/etc/apt/sources.list` and `/etc/apt/sources.list.d/` to stretch
3. `apt update`
4. `apt upgrade`
5. `apt dist-upgrade`
6. Fix errors and warnings and _only then_ `reboot`
7. `dpkg -l | grep -v ^ii` should be empty or packages should be removed
8. `reboot`
9. test

## Known problems

### fail2ban

Some configuration changes, e.g. multiple `ignore` directives are no longer allowed, so

```
ignore = 127.0.0.1/8
ignore = 10.0.0.1/8
```

becomes

```
ignore = 127.0.0.1/8 10.0.0.1/8
```

### IPv6

`/etc/rc.local`

```
/sbin/ip -6 addr add <ipv6-address>/64 dev eth0
/sbin/ip -f inet6 route add <ipv6-gateway> dev eth0		# this one is new
/sbin/route -A inet6 add default gw <ipv6-gateway> dev eth0
```

### dnscrypt

Switch from init.d script to systemd. Systemd script can be obtained from the [Arch
Wiki](https://wiki.archlinux.org/index.php/DNSCrypt)
