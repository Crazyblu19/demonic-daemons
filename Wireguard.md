### PROD

#### Prod Server

```
[Interface]
Address = 10.100.100.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = {{ private_key }}

[Peer]
AllowedIPs = 10.0.0.0/8
PreSharedKey = {{ preshared_key }}
PublicKey = {{ public_key }}
```


#### Prod Client

```
[Interface]
PrivateKey = {{ private_server_key }}
Address = 10.100.100.2/32

[Peer]
PublicKey = {{ public_client_key }}
PreSharedKey = {{ preshared_key }}
AllowedIPs = 10.100.100.1/32
Endpoint = {{ prod_ip }}:51820
PersistentKeepalive = 21
```

### DEV

#### Dev Server

```
[Interface]
Address = 10.200.200.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = {{ private_key }}

[Peer]
PublicKey = {{ public_key }}
PresharedKey = {{ preshared_key }}
AllowedIPs = 10.200.200.2/32
```

#### Dev Client (User)

```
[Interface]
Address = 10.200.200.2/32
PrivateKey= {{ private_client_key }}

[Peer]
PublicKey= {{ public_server_key }}
PreSharedKey= {{ preshared_key }}
Endpoint={{ dev_ip }}:51820
AllowedIPs = 10.0.0.0/8
PersistentKeepalive = 21
```
