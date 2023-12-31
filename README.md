# server-discovery

Python library for automatically finding your server(s) by port on the local network


## Install

Just do `pip install server-discovery`


## Usage

On the server side start the response mainloop with your chosen ports:
```python
import server_discovery
server_discovery.server_responseloop(7491, 7492)
```

The clients can then search for servers using those same ports, and devices will be returned in a generator as they are found:

```python
import server_discovery
for server in server_discovery.search_for_servers(7491, 7492):
    print(server)
```

The server calls out on all available interfaces and the client gets the relevant address, so this still works when using multiple (virtual) networks (like when in a Docker container).


## What's returned?

Servers are returned as ServerInfo objects (uses [Pydantic](https://docs.pydantic.dev) models) that include the ip address and persistent device id of the server, so the same server can be found even if the device's ip address changes.

You can also pass a device name or the service version as optional info to be sent from the server.

```python
class ServerInfo:
    ip_address: str
    device_id: int
    device_name: Optional[str]
    service_version: Optional[str]
```