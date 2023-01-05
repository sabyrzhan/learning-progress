- ## Working with [[ufw]]
	- Resource: [https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/](https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/)
	- `sudo ufw allow ssh`
	- `sudo ufw enable`
	- `sudo ufw disable`
	- `sudo ufw allow http`
	- `sudo ufw allow 80/tcp`
	- `sudo ufw allow https`
	- `sudo ufw allow 443/tcp`
	- Allow application profile
	  ```
	  sudo ufw allow 'Nginx HTTPS'
	  ```
- ## [[How-To]]
	- Install specific version with `apt`:
	  ```
	  sudo apt-get install apache2=2.2.20-1ubuntu1
	  ```
	- Execute [[CRON job]] on startup:
	  ```
	  @reboot /path/to/script
	  ```