##Log Establish VPN: 
Matched Jan 16 10:53:33 KAFKA10 ovpn-server[23219]:

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 TLS: Initial packet from [AF_INET]172.31.81.66:53183, sid=eea2a213 f936b44a

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 VERIFY OK: depth=1, C=VN, ST=HN, L=CauGiay, O=VED, OU=UET, CN=VED CA, name=server, emailAddress=locvx1234@gmail.com

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 VERIFY OK: depth=0, C=VN, ST=HN, L=CauGiay, O=VED, OU=UET, CN=client1, name=server, emailAddress=locvx1234@gmail.com

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 Data Channel Encrypt: Cipher 'BF-CBC' initialized with 128 bit key

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 Data Channel Encrypt: Using 160 bit message hash 'SHA1' for HMAC authentication

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 Data Channel Decrypt: Cipher 'BF-CBC' initialized with 128 bit key

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 Data Channel Decrypt: Using 160 bit message hash 'SHA1' for HMAC authentication

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 Control Channel: TLSv1, cipher TLSv1/SSLv3 DHE-RSA-AES256-SHA, 2048 bit RSA

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: 172.31.81.66:53183 [client1] Peer Connection Initiated with [AF_INET]172.31.81.66:53183

`%{HOSTPORT:ip-client} %{GREEDYDATA:message}`

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 MULTI_sva: pool returned IPv4=10.8.0.6, IPv6=(Not enabled)

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 MULTI: Learn: 10.8.0.6 -> client1/172.31.81.66:53183

Jan 16 10:53:33 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 MULTI: primary virtual IP for client1/172.31.81.66:53183: 10.8.0.6

Jan 16 10:53:34 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 PUSH: Received control message: 'PUSH_REQUEST'

Jan 16 10:53:34 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 send_push_reply(): safe_cap=940

Jan 16 10:53:34 KAFKA10 ovpn-server[23219]: client1/172.31.81.66:53183 SENT CONTROL [client1]: 'PUSH_REPLY,redirect-gateway def1 bypass-dhcp,dhcp-option DNS 208.67.222.222,dhcp-option DNS 208.67.220.220,route 10.8.0.1,topology net30,ping 10,ping-restart 120,ifconfig 10.8.0.6 10.8.0.5' (status=1)

```
LEARN %{WORD:action}\: %{IP:ip_clientnew} \-\> %{GREEDYDATA:message}
%{WORD:client-name}\/%{HOSTPORT:ip-client} (?:%{WORD:status}|%{GREEDYDATA})\: (?:%{LEARN}|%{GREEDYDATA:message})
```