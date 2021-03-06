#DNSMasq Configuration Readme

## Installation 

* [Source link](http://passingcuriosity.com/2013/dnsmasq-dev-osx/)

1. Use Homebrew to install `dnsmasq`
```bash
# Update your homebrew installation
brew up
# Install dnsmasq
brew install dnsmasq
```
2. Copy default configs and start service
```bash
# Copy the default configuration file.
cp $(brew list dnsmasq | grep /dnsmasq.conf.example$) /usr/local/etc/dnsmasq.conf
# Copy the daemon configuration file into place.
sudo cp $(brew list dnsmasq | grep /homebrew.mxcl.dnsmasq.plist$) /Library/LaunchDaemons/
# Start Dnsmasq automatically.
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```

## Configuration

### Set up dnsmasq to resolve `lattice.dev`

1. Append line to `/usr/local/etc/dnsmasq.conf` 
```
address=/lattice.dev/<LATTICE_SYSTEM_IP> # i.e., 192.168.11.11
```

2. Restart dnsmasq service
```bash
sudo launchctl stop homebrew.mxcl.dnsmasq
sudo launchctl start homebrew.mxcl.dnsmasq
```

### Configure workstation to use dnsmasq resolver for `lattice.dev`

1. Create `/etc/resolver` folder
```bash
sudo mkdir -p /etc/resolver
```
2. Define resolver for `lattice.dev`
```bash
sudo tee /etc/resolver/lattice.dev >/dev/null <<EOF
nameserver 127.0.0.1
EOF
```

## Starting Lattice cluster with alternate name

1. Set `LATTICE_SYSTEM_DOMAIN` environment variable during `vagrant up`:
```
LATTICE_SYSTEM_DOMAIN=lattice.dev vagrant up --provider=<PROVIDER> 
```