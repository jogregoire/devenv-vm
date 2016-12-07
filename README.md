# Set up a virtual machine as a development environment

Create a reproducible, automated, isolated development environment for Python and Node.js. Redis is also there. The OS is Ubuntu 16.04.

## Vagrant, VirtualBox and Ansible
 
Vagrant, VirtualBox and Ansible can be used to automate the creation and provisioning of a virtual machine (VM).

Vagrant lets you script the creation of the VM. The machine type is defined in the Vagrantfile. In the background, Vagrant relies on VirtualBox as the default provider. The Vagrantfile also sets a provisioner (Ansible here) that configures the VM.

The Ansible playbook in this repo installs Node.js, Redis. You can easily remove what you don't want by removing roles in the `provisioning/playbook.yml` file. Python 2.7 and Python 3.5 are already provided by Ubuntu.

The provided Vagrantfile works on all OS, including Windows; Usually, Ansible runs from the host. However, Ansible from a Windows control machine is not supported. Instead, using the `ansible_local` provisioner, Vagrant install Ansible and execute the playbook directly _on_ the VM. 

## Requirements

You need the following:

- Download and Install VirtualBox
- Download and Install Vagrant
- Enable VT-x in your computer's BIOS.
- If you are behind a corporate firewall, you also need to set the VM proxy:
    - install the vagrant-proxyconf plugin: `vagrant plugin install vagrant-proxyconf`
    - set the http_proxy, https_proxy, and no_proxy environment variable on your host.

## How to use

Open a shell prompt, cd into the folder containing the Vagrantfile, and run `vagrant up`. After a few minutes, your VM is ready. `vagrant ssh` to connect to the machine via SSH. 

If you make changes:

- `vagrant reload` restarts the VM and reload the Vagrantfile.
- `vagrant provision` runs the Ansible playbook again
- `vagrant reload --provision` if you make changes to the Vagrantfile and Ansible playbook.

Finally, `vagrant destroy` stops and deletes the VM.

If you already have Ubuntu 16.04 running and don't need to create the VM, you can still use the Ansible playbook to configure it:

```
# Install Ansible
pip install ansible

# Run Ansible playbook
ansible-galaxy install -r provisioning/requirements.yml
ansible-playbook -c local provisioning/playbook.yml
```

As a bonus, you can install docker. Go to /vagrant/provisioning in the VM and run the following command:

``ansible-playbook -c local provisioning/playbook.yml``

Then log-out, log-in.

## Troubleshooting

### ansible local provisioner bug

```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
There are errors in the configuration of this machine. Please fix
the following errors and try again:

ansible local provisioner:
* `playbook` does not exist on the guest: C:/vagrant/provisioning/playbook.yml
* `inventory_path` does not exist on the guest: C:/vagrant/vagrant/provisioning/inventory
* `galaxy_role_file` does not exist on the guest: C:/vagrant/provisioning/requirements.yml
```

Please upgrade Vagrant to >= 1.8.5. [](https://github.com/mitchellh/vagrant/issues/6740)

## Author

Joel Gregoire
