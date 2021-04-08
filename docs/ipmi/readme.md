### Run:
```
ansible-playbook -i inventory.ini playbook.yaml
```

### Inventory
contains two groups of servers
- running: these servers will be powered on
- stopped: these servers will be shutdown

make sure to change `ipmi_user` and `ipmi_password` for each server accordingly
