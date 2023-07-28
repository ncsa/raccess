# Configuration of a Remote Access System

## Introduction

As one of the most popular VPN protocols worldwide, OpenVPN is an appropriate choice to provide secure access for remote developers. This respository thus serves as a simple and easy guide outlining the installation and configuration of the OpenVPN service for remote access to the latest testbed. 

## Getting Started

The following has been tested on Ubuntu Desktop 20.04.5 (Focal Fossa), which would serve as the OpenVPN server.

1. Install relevant packages
``` bash
sudo apt update && sudo apt install wget
```

2. Download the OpenVPN script
``` bash
wget https://git.io/vpn -O openvpn-install.sh
```

3. Run the downloaded script. You will receive prompts on your preferred OpenVPN protocol, port, DNS server etc
``` bash
sudo chmod +x openvpn-install.sh
sudo bash openvpn-install.sh
```

4. Enable packet forwarding
``` bash
sysctl -w net.ipv4.ip_forward=1
```

## Further Server and Client Configuration

1. As indicated in the example client.conf script provided below, do modify client.conf in accordance to your preferences. Note that username/password authentication is adopted.

``` bash
cipher BF-CBC                      # change to your preferred cipher option (e.g., AES-256-CBC / AES-256-GCM)
reneg-sec 0			   
keepalive 10 120
dev tun			 	   # must be the same as in server.conf
remote 141.142.130.244 443 tcp	   # replace with ip-address of OpenVPN server, port and protocol (UDP/TCP)
auth-nocache
client
auth-user-pass			   # authenticates with username and password
mute-replay-warnings
persist-tun
persist-key
float

<ca>
-----BEGIN CERTIFICATE-----        # replace with your cert
MIIEyjCCA7KgAwIBAgIJAKIdt6NfeneTMA0GCSqGSIb3DQEBBQUAMIGeMQswCQYD
VQQGEwJTRzELMAkGA1UECBMCU0cxEjAQBgNVBAcTCVNJTkdBUE9SRTEVMBMGA1UE
ChMMU2hhd24gYW5kIENvMR0wGwYDVQQLExRNeU9yZ2FuaXphdGlvbmFsVW5pdDEY
MBYGA1UEAxMPU2hhd24gYW5kIENvIENBMR4wHAYJKoZIhvcNAQkBFg9zaGF3bkBn
bWFpbC5jb20wHhcNMTgwNzA5MDUzOTAyWhcNMjgwNzA2MDUzOTAyWjCBnjELMAkG
A1UEBhMCU0cxCzAJBgNVBAgTAlNHMRIwEAYDVQQHEwlTSU5HQVBPUkUxFTATBgNV
BAoTDFNoYXduIGFuZCBDbzEdMBsGA1UECxMUTXlPcmdhbml6YXRpb25hbFVuaXQx
GDAWBgNVBAMTD1NoYXduIGFuZCBDbyBDQTEeMBwGCSqGSIb3DQEJARYPc2hhd25A
Z21haWwuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAt43/sStU
+y7/ik13ePfskggnjzp1u2AeGeH9L6TuTETfYxR4Rr0lRNgit6QdJLAmMDojKCqh
qKBgEh29rAS7moItuoIw+mvYm1LbicrQVHfbOGFnHHGqZ1BOjYWANowzXZPiYjb0
VNKQIQQMsFVBgcat027BtaccQ5drHs9z2nqCJKe+a+L5guCe+G2NCnijYTQxQIOp
1KNHeMht4Wm2zqScLmcuQ57Yv+BbbOxWLO4G3H8L3ZU0UsSgYGQmTmWQErSWs9ie
VMzBvLTJ0ZnfJNVIuUCrRMtQ93qG6zL8drCUGln3+2t3bQEaAmA6VkPsvm3NEdwA
OSQA3J4cpeUQRwIDAQABo4IBBzCCAQMwHQYDVR0OBBYEFPrLTP1VaQh7rmm8murL
OYvQm+VwMIHTBgNVHSMEgcswgciAFPrLTP1VaQh7rmm8murLOYvQm+VwoYGkpIGh
MIGeMQswCQYDVQQGEwJTRzELMAkGA1UECBMCU0cxEjAQBgNVBAcTCVNJTkdBUE9S
RTEVMBMGA1UEChMMU2hhd24gYW5kIENvMR0wGwYDVQQLExRNeU9yZ2FuaXphdGlv
bmFsVW5pdDEYMBYGA1UEAxMPU2hhd24gYW5kIENvIENBMR4wHAYJKoZIhvcNAQkB
Fg9zaGF3bkBnbWFpbC5jb22CCQCiHbejX3p3kzAMBgNVHRMEBTADAQH/MA0GCSqG
SIb3DQEBBQUAA4IBAQAmI1gLqEJAb8TgoUmoQhF/arMdFBQJhH9iKkrrr4XX7j1w
ElOmJcZ8gdLASzcXirbG5OXpFT+hmxZ8Y8aDjfHHlbCtJn96I/OYEZp5r5D99xeS
bXIE20ORZwftHhdwotxXKselaysiQubp66DXI8w9tdVI5WxSjR3gt/ceA/wybHso
7pvTkzZgxVqzov+70L+Z6arx8Lb9y1Vuqo4iOhCtz/POhiMTQ/Ad8Bqrw3B73pTo
jRZiJJUn/PLJ53HG3HPQqCDo3gaU4KiZx2mdhTSnHXGc0nF5QRW/lUSXQn5gEulC
RFwPRh0VhbncNY/M5TcpqThn5sE3jcIxFaMKbZ6Q
-----END CERTIFICATE-----
</ca>
```


2. As indicated in the example server.conf script provided below, modify server.conf in accordance to your preferences.

```bash
proto tcp                                         # replace with desired OpenVPN protocol (UDP/TCP)
port 443                                          # replace with desired OpenVPN port 
dev tun

#This is required, if not openvpn will
#re-negotiate TLS every 3600s (1hr)
reneg-sec 0

mute-replay-warnings
ca /etc/openvpn/certs/ca.crt
cert /etc/openvpn/certs/server.crt
key /etc/openvpn/certs/server.key
dh /etc/openvpn/certs/dh2048.pem
client-cert-not-required
auth-user-pass-verify /etc/openvpn/user.sh via-env
script-security 3
username-as-common-name
duplicate-cn

server 172.16.21.0 255.255.255.0                  # replace with desired VPN subnet 
ifconfig-pool-persist ipp.txt
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "route 172.16.18.0 255.255.255.0"

client-to-client
keepalive 10 120

max-clients 10

user nobody
group nogroup

persist-key
persist-tun

status /var/log/openvpn-status.log
log /var/log/openvpn.log
log-append /var/log/openvpn.log

verb 5
mute 20
```

## Usage

1. Run the OpenVPN server on your Ubuntu machine.
``` bash
cd /etc/openvpn
sudo openvpn --config server.conf
```

2. Start the OpenVPN client on a remote machine with OpenVPN installed (Linux/Windows/MacOS is fine).
``` bash
sudo openvpn --config client.conf
```

## Teardown

To stop the OpenVPN server/client, `ctrl-c` would suffice.

[![Analytics](https://earnest-crow-394007.as.r.appspot.com/test?pixel)](https://github.com/ncsa/raccess)
