# dnsmasq

dnsmasq provides a DNS server, a DHCP server with support for DHCPv6 and PXE,
and a TFTP server. It is designed to be lightweight and have a small footprint,
suitable for resource constrained routers and firewalls. dnsmasq can also be
configured to cache DNS queries for improved DNS lookup speeds to previously
visited sites.

We will be only using the DNS server, to resolve `*.qlico` to your docker bridge
network.

## Linux

At the moment the Linux installation has been tested with >= Ubuntu 18.04.

First open the `/etc/NetworkManager/NetworkManager.conf` file with your
favourite text editor, make sure you'll have `sudo` rights.

Under `[main]` add: `dns=dnsmasq`.

Now we need to tell our Operating System to use the `resolv.conf` created by
NetworkManager, we'll move the old `resolv.conf` to `resolv.conf.old` and create
a symlink to the one created by NetworkManager.

```bash
sudo mv /etc/resolv.conf /etc/resolv.conf.old
sudo ln -s /var/run/NetworkManager/resolv.conf /etc/resolv.conf
```

Now we'll need the IP address of your Docker bridge network, we will use this IP
address in the next step.

```bash
ip addr show docker0 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1
```

Now it's time to create a new dnsmasq configuration:

Use the IP address, you've received from the previous step (for
example: `172.17.0.1`), and place it in this line: `address=/qlico/172.17.0.1`

Open the file: `/etc/NetworkManager/dnsmasq.d/qlico` with your favourite text
editor, make sure you'll have `sudo` rights. And add the following to the file,
make sure you'll replace the IP address with the IP address from the previous
step (if it's different).

```plain
domain-needed
bogus-priv
strict-order
 
address=/qlico/172.17.0.1
 
listen-address=::1,127.0.0.1
```

Finally, reload NetworkManager

```bash
sudo systemctl reload NetworkManager.service
```

## macOS

To use dnsmasq on macOS we need to install it first, make sure you
have [homebrew](https://brew.sh/) installed.

After that open your favourite Terminal, and run the following commands.

```bash
brew install dnsmasq
```

Get the IP address of Docker for Mac.

```bash
docker run --rm -it alpine:latest nslookup host.docker.internal
```

Use the IP adress from:

```plain
Non-authoritative answer:
Name:	host.docker.internal
Address: 192.168.64.1
```

```bash
# Create a configuration folder
mkdir -pv $(brew --prefix)/etc/

# Create a dnsmasq configuration file
vim $(brew --prefix)/etc/dnsmasq.conf
```

Paste the following inside the file (remember to change the IP address when it's
different):

```plain
address=/qlico/192.168.64.1
listen-address=127.0.0.1
port=35353
```

Finish with by running the following commands

```bash
# Always start dnsmasq when starting macOS.
sudo brew services start dnsmasq

# Create a dns resolver directory
sudo mkdir -v /etc/resolver

# Tell the resolver to use the local installed dnsmasq for the qlico tld. 
sudo bash -c 'echo "nameserver 127.0.0.1
port 35353" > /etc/resolver/qlico'
```

## Windows

Unfortunately dnsmasq is not available for Windows, if you have alternatives
please contribute to the documentation.
