---
title: "Wireguard"
date: 2019-12-11T15:09:12+08:00
draft: false
---

## FAST, MODERN, SECURE VPN TUNNEL

WireGuard is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

### Install WireGuard

[Here](https://www.wireguard.com/install/) is the official guides. follow it carefully.

### Configure WireGuard Server

1. Generate a private and public key pair for the WireGuard server:

```shell
umask 077
wg genkey | tee server-privatekey | wg pubkey > server-publickey
```

2. Create file `/etc/wireguard/wg0.conf`:

```shell
[Interface]
PrivateKey = <server-privatekey>
Address = 192.168.199.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
SaveConfig = true
```

- **Address** defines the private IPv4 and IPv6 addresses for the WireGuard server. Each peer in the VPN network should have a unique value for this field.
- **ListenPort** specifies which port WireGuard will use for incoming connections.
- **PostUp** and **PostDown** defines steps to be run after the interface is turned on or off, respectively. In this case, iptables is used to set Linux IP masquerade rules to allow all the clients to share the server’s IPv4 and IPv6 address. The rules will then be cleared once the tunnel is down.
- **SaveConfig** tells the configuration file to automatically update whenever a new peer is added while the service is running.

### Set up firewall rules

1. Allow SSH connections and WireGuard’s VPN port:

```shell
sudo ufw allow 22/tcp
sudo ufw allow 51820/udp
sudo ufw enable
```

2. Verify the settings:

```shell
sudo ufw status verbose
```

### Start the Wireguard Service

1. Start Wireguard:

```shell
wg-quick up wg0
```

2. Enable the Wireguard service to automatically restart on boot:

```shell
sudo systemctl enable wg-quick@wg0
```

3. Check if the VPN tunnel is running with the following two commands:

```shell
sudo wg show;
ifconfig wg0
```

### Setup WireGuard Client

The process for setting up a client is similar to setting up the server. When using Ubuntu as your client’s operating system, the only difference between the client and the server is the contents of the configuration file. If your client uses Ubuntu, follow the steps provided in the above sections and in this section.

1. Generate a key pair for the client if you have not already:

```shell
umask 077
wg genkey | tee client1-privatekey | wg pubkey > client1-publickey
```

2. The main difference between the client and the server’s configuration file, `wg0.conf`, is it must contain its own IP addresses and does not contain the `ListenPort`, `PostUP`, `PostDown`, and `SaveConfig` values.

```shell
[Interface]
PrivateKey = <Output of privatekey file that contains your private key>
Address = 192.168.199.2/32
DNS = 192.168.199.1

[Peer]
PublicKey = <Server Public key>
Endpoint = <Server Public IP>:51820
AllowedIPs = 0.0.0.0

```

Client config done.

3. Lets get it transferred to our device. We will do this using a QR code, which we can create on our server using qrencode. You can install this via `apt install qrencode`. To generate the QR code and display it on the screen:  
   `qrencode -t ansiutf8 < /etc/wireguard/client.conf`

### Last but not least

You have seen `DNS`,`PostUP` and `PostDown` above but did you think what matters do them do ? If your WireGuard server doesn't provider dns service but some machines does within the LAN and you want dns records resolved in the private network. Then you should use `iptables` to do some forward.

For example: `192.168.0.6/22` is the dns server in the LAN and `192.168.3.111/22` is the WireGuard server with `wg0` (192.168.199.1/24). In this scenario, you shoud do this:

- add this append to `PostUp`

```shell
iptables -tnat -A PREROUTING -i wg0 -p udp -m udp --dport 53 -j DNAT --to-destination 192.168.0.6:53; iptables -A FORWARD -i ens3 -o wg0 -j ACCEPT
```

- add this append to `PostDown`

```shell
iptables -tnat -A PREROUTING -i wg0 -p udp -m udp --dport 53 -j DNAT --to-destination 192.168.0.6:53; iptables -D FORWARD -i ens3 -o wg0 -j ACCEPT
```

So the finally `/etc/wireguard/wg0.conf` should be this:

```shell
[Interface]
Address = 192.168.199.1/24
SaveConfig = true
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE; iptables -tnat -A PREROUTING -i wg0 -p udp -m udp --dport 53 -j DNAT --to-destination 192.168.0.6:53; iptables -A FORWARD -i ens3 -o wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens3 -j MASQUERADE; iptables -tnat -A PREROUTING -i wg0 -p udp -m udp --dport 53 -j DNAT --to-destination 192.168.0.6:53; iptables -D FORWARD -i ens3 -o wg0 -j ACCEPT
ListenPort = 51820
PrivateKey = <server private key>

[Peer]
PublicKey = HLVZ3c7CMHPRzlgPZBxYI7lYJpjdNnRIGhlHPeB9zSI=
AllowedIPs = 192.168.199.2/32
```
