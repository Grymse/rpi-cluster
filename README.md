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