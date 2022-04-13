# Linux learnings
## `ufw` - `iptables` frontend for managing Linux firewalls
1. To solve the complexities managing firewall rules with `iptables` - `ufw` is used as a front-end for `iptables`.
2. firewall consists from predetermined security rules and policies used to control outgoing and incoming traffic.
3. therefore it controls all the packets sent over `socket` - which is the combination of protocol, ip address and port - that applications/services use to exchange data between them.
4. By default `ufw` is disabled (at least on Ubuntu systems). To enable it:
    1. if you are using ssh, then allow ssh connection first: `sudo ufw allow 22`. Because by default `ufw` blocks all incoming connections.
    2. `sudo ufw enable`
5. Most used commands used by `ufw`:
    1. `status verbose` - show verbose status of the rules
    2. `status numbered` - show status with assigned numbers
    3. `reload` - the same as `stop then start`
    4. `reset` - reset to factory defaults by removing all custom rules by backing up all existing rules.
6. Syntax of adding a rule: `ufw allow|deny in|out <port>[/<protocol>]`
7. Some examples of `allow/deny` usages:
    1. `sudo ufw allow in 53/udp` - allow UDP connections on port 53
    2. `sudo ufw allow 53/tcp from <IP address>` - allow TCP connections only specified ip address
    3. `sudo ufw deny from <IP_address> to any` - deny any connection from IP address only
    4. `sudo ufw deny out from any to <IP_address>` - deny all outgoing connections to specified IP address
    5. `sudo ufw allow from <IP_address> to any port 22` - allow SSH connections to any iface from specified IP address
