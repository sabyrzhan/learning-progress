# Linux learnings
## `ufw` - `iptables` frontend for managing Linux firewalls
1. To solve the complexities managing firewall rules with `iptables` - `ufw` is used as a front-end for `iptables`.
2. firewall consists from predetermined security rules and policies used to control outgoing and incoming traffic.
3. therefore it controls all the packets sent over `socket` - which is the combination of protocol, ip address and port - that applications/services use to exchange data between them.
4. By default `ufw` is disabled (at least on Ubuntu systems). To enable it:
  a. if you are using ssh, then allow ssh connection first: `sudo ufw allow 22`
  b. `sudo ufw enable`
