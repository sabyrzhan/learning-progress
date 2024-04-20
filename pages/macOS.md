## [[Creating launchd service]]
	- `launchd` is analogue of `systemd` in Linux. You can register and control your services in bash using `launchctl`
	- Small not regarding [[Creatiing bash service in Automator]]: Before you could create service (bash for example) in Automator by selecting `Service` workflow type. However, what I noticed in macOS Sonoma you can do so by selecting `Quick Action` type.
	- More about it
		- Small tutorial: https://somesh-rokz.medium.com/how-to-create-services-in-macos-using-bash-launchctl-and-plutil-commands-step-by-step-guide-d736d25cdeeb
		- Launchd services in macOS: https://support.apple.com/guide/terminal/script-management-with-launchd-apdc6c1077b-5d5d-4d35-9c19-60f2397b2369/mac
		- About `launchd` and its configurations: https://www.launchd.info/
		-
	- ```
	  # Create plist file with following content. For example: my-service.plist
	  # Label is required and must be unique.
	  # The property list file should be saved in the ~/Library/LaunchAgents directory 
	  # and should have a filename in the following format: <label>.plist, 
	  # where <label> is a unique identifier for your service.
	  <?xml version="1.0" encoding="UTF-8"?>
	  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	  <plist version="1.0">
	  <dict>
	  	<key>Label</key>
	  	<string>kz.sabyrzhan.dashy</string>
	  	<key>ProgramArguments</key>
	  	<array>
	  		<string>/bin/bash</string>
	  		<string>-c</string>
	  		<string>
	          source /Users/sabyrzhan/.zshrc
	          cd /Users/sabyrzhan/projects/dashy
	          yarn start
	        </string>
	  	</array>
	      <key>StandardOutPath</key>
	      <string>/Users/sabyrzhan/projects/my-launchd-services/dashy-stdout.log</string>
	      <key>StandardErrorPath</key>
	      <string>/Users/sabyrzhan/projects/my-launchd-services/dashy-stderr.log</string>
	      <key>RunAtLoad</key>
	      <true/>
	  </dict>
	  </plist>
	  
	  # Load it with launchctl
	  launchctl load my-service.plist
	  
	  # Control with launchctl: stop, start, list (specify Label as a parameter)
	  launchctl start kz.sabyrzhan.dashy
	  ```
- ## [[Logging to syslog to view in Console]]
	- SO: https://superuser.com/questions/246045/on-mac-os-x-how-can-i-log-to-console-app-from-the-terminal
	- ```
	  # Following will not show in Console because of low than default priory of the log
	  syslog -s Your message goes here. \(quote special chars for the shell'!)'
	  
	  # To show it, specify level with -l <level> (notice, error etc)
	  syslog -s -l notice "This message should show up in \"All Messages\" \
	    with a Facility of syslog."
	  ```