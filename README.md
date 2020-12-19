# pyns2
pyns2(renamed from netns-siml) is network simulator with linux network namespace create virtual network by yaml format config file.
## support
### interface type
Now `veth` and `bridge` type interface is supported.

### environment
This tool works on Linux and need root access.
To quick use, please you can use `Vagrant`.
This also works on privileged docker container. But under docker for mac environment, external access(NAT) don't work.
## example
This is the example of network definition. This file is in `examples/example-container.yml`

```yaml
example-network-container:
    host:
        ifaces:
            br0:
                type: "bridge"
                address: "192.168.50.2/24"
                ifaces:
                    - "host1-veth1-br"
                    - "host2-veth1-br"
                    - "host-veth1-br"
            host-veth1:
                type: "veth"
                address: "192.168.50.1/24"
                peer: "host-veth1-br"
        nat:
          src: "192.168.50.0/24"
          out_iface: "eth0"

    netns:
        host1:
            ifaces:
                host1-veth1:
                    type: "veth"
                    address: "192.168.50.100/24"
                    peer: "host1-veth1-br"
            routes:
              - route:
                    gateway: "192.168.50.1"
                    dest: "default"

        host2:
            ifaces:
                host2-veth1:
                    type: "veth"
                    address: "192.168.50.101/24"
                    peer: "host2-veth1-br"
            routes:
                - route:
                    gateway: "192.168.50.1"
                    dest: "default"
```
Other exampele is located in `examples/`

## format
  
## command
- `create <file>`
  
  This command create netns and interface.

- `set <file>`

  This command set created interfaces in netns

- `run <file>`
  
  This command run virtual network from config file. This command is equal to `create` and `set`
- `delete <file>`
  
  This command deletes virtual network created by run command
- `exec <netns name> <command>`
  This command executes the command(default is `bash`) in the target network namespace.
  
  *Note: if you want to execute the command contains space(exp: ping 8.8.8.8) <command>, you have to write as list object like ['ping', '8.8.8.8']*

- `up <file>`

  This command up created interfaces

- `down <file>`

  This command down creted interfaces

- `list <file>`

  This command shows created netns

- `validate <file>`

  This command checks whether config file valid

- `check_netns`

  This command output current netns name 

## Future work
- implement monitoring modules
- add supports of other interface types