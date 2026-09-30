# Raspberry PI kubernetes home-setup

For no reasons but learning about new technologies, have I decided to build a home-cluster. I currently deploy 8 raspberry PI's (5x rpi4 + 3rpi5). I intend to expand to 13 pieces down the line. They're connected through a UniFi 16-port switch, and access internet through a 5G router.

The 5G router is clearly a bottleneck in the setup, but found a spot where the router has line-of-sight and spectacular reception, comparable to what you'd expect from applying an external antenna.

## Cluster Design
The system is split into three sections:
- 3 dev nodes (rpi4)
- 2 compute nodes (rpi4)
- 3 data nodes (rpi5)

The compute and data nodes resemble a production environment and I intend to build a smaller but similar environment with future upgrades. They all share the same cluster, and each group of nodes have a single node that is a part of the kubernetes control plane. I cannot dedicate nodes to only handling control plane work, so to avoid work on certain areas of the cluster to overwhelm the control plane tasks, I've decided to distribute it to each type of load.

All 8 nodes have SSD's attached. RPI5 use top-hats and RPI4's use USB-to-NVME adapters. The dev nodes have a 2tb SSD each, the compute nodes 256gb each and the data nodes 512gb each. I decided to dedicate the largest SSD's to the dev nodes, as they will store code, artifacts, logging, telemetry and fileshares. If I ever run out of space on the rpi5's, then I'll upgrade.

## Commands

Here are relevant commands:

```sh
# Check status
ansible all -m ping

# Run playbook
ansible-playbook -i inventory/hosts.ini playbooks/01-bootstrap.yml

# Setup full system
ansible-playbook -i inventory/hosts.ini playbooks/site.yml

# Shutdown
ansible-playbook -i inventory/hosts.ini playbooks/shutdown.yml
```


## Installing a new node

Adding a new node requires:
- Burning it with Raspberry PI Imager
  - Usr: admin
  - Pwd: same as others
  - Add public key of Macbook
  - Disable Wifi, and pwd login
  - Change hostname to rpi4-X or rpi5-X (e.g. rpi4-2) depending on version and number
- Assign static IP on home router
- Add node to ansible inventory and run playbook