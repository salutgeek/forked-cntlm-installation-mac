# forked-cntlm-installation-mac

## Context
Most of the popular distros contain cntlm packages in their repositories. On macOS, cntlm can be installed with homebrew and can be run as a service with homebrew service.

This repo shows how to install cntlm from scratch **on MacOS**, so for those who want an installation without the dependence on package manager.

## Installation process

1. clone the repo: https://github.com/versat/cntlm
```bash
git clone git@github.com:versat/cntlm.git
```

2. build cntlm from scratch
```bash
cd cntlm
make
```

3. Normally if the installation process finished successfully, a cntlm binary will be created in the current directory
```bash
file ./cntlm
# Try to execute the cntlm binary
./cntlm --help
```

4. Copy (or move) cntlm binary into /usr/local/bin/
```bash
mv ./cntlm /usr/local/bin/
# Check path of cntlm binary, should be now /usr/local/bin/cntlm
which cntlm
```

5. Create a custom cntlm service. You can use options `-c` to point to your custom location of the cntlm.conf file (usually the default path of cntlm.conf on macOS is `/usr/local/etc/cntlm.conf`)
```bash
tee -a ~/Library/LaunchAgents/cntlm.service.plist <<'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>cntlm.service</string>
	<key>ProgramArguments</key>
	<array>
		<string>/usr/local/bin/cntlm</string>
		<string>-f</string>
        	<string>-c</string>
        	<string>/usr/local/etc/cntlm.conf</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
EOF
```

6. Run cntlm service on start-up and in background
```bash
launchctl load -w ~/Library/LaunchAgents/cntlm.service.plist
```

7. Check if cntlm service is activated
```console
foo@bar:~$ launchctl list | grep cntlm
xxx  xxx  cntlm.service 
```

8. If we don't want to run cntlm on start-up
```sh
launchctl unload -w ~/Library/LaunchAgents/cntlm.service.plist
```
