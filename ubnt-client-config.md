```
config to /config/vpn-client1.ovpn

set interfaces openvpn vtun0 config-file /config/vpn-client1.ovpn
set interfaces openvpn vtun0 description OVPN-CLIENT
set interfaces openvpn vtun0 firewall in name WAN_IN
set interfaces openvpn vtun0 firewall local name WAN_LOCAL

set service nat rule 5003 description 'masquerade for OVPN-CLIENT'
set service nat rule 5003 outbound-interface vtun0
set service nat rule 5003 type masquerade

set firewall group address-group VPN_ADDRS address 1.1.1.1
set firewall group address-group VPN_ADDRS address 2.2.2.2

set firewall modify balance rule 40 action modify
set firewall modify balance rule 40 description 'do NOT load balance destination VPN_ADDRS address'
set firewall modify balance rule 40 destination group address-group VPN_ADDRS
set firewall modify balance rule 40 modify table main




```
# sample config for ar300m
# no default route push, manual route config
verb 3
mute 3
client
resolv-retry infinite
dev tun
dev-type tun
proto tcp
remote 10.10.10.10
port 443
comp-lzo
reneg-sec 604800
sndbuf 100000
rcvbuf 100000
nice -13
remote-cert-tls server
reneg-sec 604800
#
route 1.1.1.1 255.255.255.255
route 2.2.2.2 255.255.255.255
#
<ca>
-----BEGIN CERTIFICATE-----
ca cert
-----END CERTIFICATE-----
</ca>

<cert>
-----BEGIN CERTIFICATE-----
client crt
-----END CERTIFICATE-----
</cert>

<key>
-----BEGIN PRIVATE KEY-----
client priv key
-----END PRIVATE KEY-----
</key>

key-direction 1
<tls-auth>
#
# 2048 bit OpenVPN static key (Server Agent)
#
-----BEGIN OpenVPN Static key V1-----
ta key
-----END OpenVPN Static key V1-----
</tls-auth>
```