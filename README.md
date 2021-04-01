# tf_farmer_management_tools

This repo is the main place for ThreeFold farmers to educate themselves on how to setup professional datacenter based ThreeFold farms. It also contains tools which can be used by farmers to setup and maintain their farms.

### Installation Prerequisites:
- `pyghmi` used in ansible `ipmi_power` module

```bash
export CRYPTOGRAPHY_DONT_BUILD_RUST=1
pip3 install --upgrade pip
pip3 install --user setuptools_rust pyghmi
```

### Install:
```
git clone https://github.com/threefoldfoundation/tf_farmer_management_tools
cd tf_farmer_management_tools
ansible-galaxy collection build
ansible-galaxy collection install threefold-farm_mgmt-0.1.0.tar.gz
```
