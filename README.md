## Dante SOCKS Proxy
This role provides a means to configure a Dante SOCKS proxy on your system.

When using this role on RHEL(-like), ensure you have EPEL configured.

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

## Usage
* Install the role from Galaxy
* Fill in the variables (see defaults)
* Run Ansible
* ???
* Profit!
