these playbooks were used to configure the tor switch + servers used to boot a farm


### Environment Descriptions
1- ToR switch connected to the router on a trunk link with management ip configured
2- ToR switch accessible over its management ip address
3- MAC addresses of the servers
4- OpenWRT node used as dhcp/ipxe server
5- (optional) if you have an old ipxe version on the servers, create your own ipxe script on OpenWRT node


### Playbooks:
1- `01_network_setup.yaml`: used to configure vlans, ports and upstream trunk on the ToR switch. It is used to add static DHCP entries for the servers' BMC on OpenWRT node.


2- `02_zos_boot.yaml`: after the BMC is assigned an IP address, it is used to fetch each server's zos interface MAC address and use it for static DHCP entry. Also, to configure ipxe script for each server and boot the server.


### Inventory:
1- `fs3900`: the ToR switches running `FSOS`

variables to configure:
- `ansible_ssh_common_args`: defines ssh options if the switches use different ssh versions
- `ansible_ssh_user`: ssh username for the switch
- `ansible_ssh_pass`: ssh password for the switch
- `private_vlan`: vlan id for the BMC and ipxe traffic
- `public_vlan`: vlan for public zos traffic
- `private_interface_range`: port range on the switch that is connected to the servers' BMC and ipxe boot interfaces
- `public_interface_range`: port range on the switch that is connected to the public zos interface for the servers
- `trunk_port`: the switch port that is connected upstream to the router


2- `openwrt`: the node that runs the DHCP and ipxe servers

variables to configure:
- `ansible_ssh_user`: ssh username for the node
- `ansible_ssh_pass`: ssh password for the node
- `ipmi_username`: IPMI username for the servers
- `ipmi_password`: IPMI password for the servers
- `farm_id`: farm id that the servers will be part of (used to download ipxe script from https://bootstrap.grid.tf)
- `farm_name`: the farm name (used as name for the ipxe script)
- `env`: the explorer network that the farm is part of. can be `test` for testnet, `dev` for devnet or `prod` for mainnet
- `ipxe_file` (optional): in case you need to tweak the ipxe file and not download it, this variable contains the path of the modified ipxe filed on the node.
- `servers`: servers info that is used to add static DHCP entries and ipxe config.

    * `hostname`: used as hostname for BMC DHCP entry and `$hostname_zos` for zos
    * `mac_address`: mac address of the BMC interface on the server
    * `ip_address`: ip address that will be assigned to the BMC interface of that server
    * `zos_address`: ip address that will be assigned to the server to boot from ipxe server


3- `ipmi`: contains the ip addresses that were assigned via static DHCP entries to the servers

variables to configure:
- `ipmi_username`: IPMI username for the servers
- `ipmi_password`: IPMI password for the servers


### Run:
The playbooks should run in specific order as numbered in their names
1- configure the ToR switch and static DHCP for servers' BMC.
```
ansible-playbbok -i inventory.yaml 01_network_setup.yaml
```

then wait until the dhcp server leases the bmc ip addresses

2- after servers' BMC is reachable, they are now ready to boot zos
```
ansible-playbbok -i inventory.yaml 02_zos_boot.yaml
```
