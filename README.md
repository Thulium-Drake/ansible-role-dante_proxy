## Dante SOCKS Proxy
This role provides a means to configure a Dante SOCKS proxy on your system.

When using this role on RHEL(-like), ensure you have EPEL configured. This role has been tested with Dante 1.4.x.

## Configuration
The role has been preconfigured to have sane details, which will allow any client to connect to any server that is reachable by your proxy server. Below is a list of all variables that are available for this role. For more information see [the defaults file](defaults/main.yml)

* `dante_listen_uri`: The interface or IP address that Dante listens on (default: `eth0`)
* `dante_listen_port`: The port that Dante listens on (default: `8080`)
* `dante_outgoing_uri`: The interface or IP address that Dante uses for outgoing connections (default: `eth0`)

* `dante_loglevel`: The level of detail in the logging (default: `0`)
* `dante_client_authentication_methods`: Supported authentication methods in the server for connecting clients (default: `none`)
* `dante_socks_authentication_methods`: Supported authentication methods for permitting clients access to external targets (default: `eth0`)
* `dante_client_rules`: ACL rules for controlling whichs clients can connect. The default is to allow any client.
* `dante_socks_rules`: ACL rules for controlling which external targets can be connected to. The default is to allow any target and allow incoming packets for established and related connections.

### ACL rule options
For more information on how Authentication works in Dante, please see: https://www.inet.no/dante/doc/1.4.x/config/auth.html

Client rules control which clients can connect to the proxy server
```
dante_client_rules:
  - comment: 'Allow connections from any client (IPv4 +IPv6)'
    action: 'pass'
    from: '0/0'                    # Can be an IPv4 address (0.0.0.0/0), IPv6 address (::/0) or 0/0 for 'any' (both IPv4 and IPv6)
    to: '0/0'
    log: 'error'                   # Optional, default 'error connect disconnect'
    authmethod: 'your_authmethod'  # Optional
    group: 'allow_proxy'           # Mandatory when authmethod is defined, designates the users that this rule is applied to
```

SOCKS rules control which upstream targets can be reached by clients.
```
dante_socks_rules:
  - comment: 'Allow connections to target (IPv4 +IPv6)'
    action: 'pass'
    from: '0/0'
    to: '0/0'
    command: 'bind'                # Optional, default 'bind connect udpassociate'
    log: 'error'                   # Optional, default 'error connect disconnect'
    authmethod: 'your_authmethod'  # Optional
    group: 'allow_proxy'           # Mandatory when authmethod is defined, designates the users that this rule is applied to
```

Routes determine how traffic is handled, all these rules are optional. If you have multiple rules with the same from/to selectors, Dante will load balance between them. For more information about chaining, please see: https://www.inet.no/dante/doc/1.4.x/config/chaining.html
```
dante_route_rules:
  - comment: 'Send traffic for this network through eth1
    from: '0.0.0.0/0'
    to: '192.168.2.0/24'
    via: 'eth1'
  - comment: 'Chain to upstream proxy'
    from: '0/0'
    to: '0/0'
    proxyprotocol: 'socks_v4 socks_v5'  # Or http_v1.0 or upnp
    via: 'socks5@http-proxy.example.nl'
```

## Usage
* Install the role from Galaxy
* Fill in the variables (see defaults)
* Run Ansible
* ???
* Profit!

## Upstream documentation
Dante home page: https://www.inet.no/dante/index.html
