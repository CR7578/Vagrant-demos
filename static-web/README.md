## Static Web (Vagrant)

This directory contains a Vagrant configuration that provisions an Ubuntu Jammy VM (`ubuntu/jammy64`) and installs an Apache2 web server to serve a static website from the guest web root at `/var/www/html`.

Key details from the `Vagrantfile`:
- Box: `ubuntu/jammy64`
- Network: private network with IP `192.168.33.10`
- Provider: `virtualbox` with `vb.memory = "1024"`
- Provisioning: installs `apache2`, downloads a static template and copies it into `/var/www/html`

**Purpose:** Provide a reproducible VM to host and test static web content locally.

**Prerequisites**
- Install Vagrant (recommended >= 2.2.x): https://www.vagrantup.com/
- Install a provider such as VirtualBox (recommended) or another supported hypervisor.
- A supported shell/terminal (Windows: Git Bash, WSL, or PowerShell). The commands below assume a Bash-like shell.

**Quick Start — Run the site**

1. Clone the Vagrant-demos Git repo

	```bash 
    git clone https://github.com/CR7578/Vagrant-demos.git
    ```

2. Change directory to vagrant-demos

    ```bash
    cd Vagrant-demos/static-web
    ```

3. Change Vagrantfile config if required ( like changing private ip address, changing static template website URL )

    At line 35  ==>   `config.vm.network "private_network", ip: "192.168.33.10"`

    At line 82  ==>   `wget https://www.tooplate.com/zip-templates/2144_parallax_depth.zip`

4. Start the VM and provision it:

	```bash
    vagrant up
    ```

    After configuration, Open browser and enter your private ip address `192.168.33.10` in URL tab. 

    Your static website is live on local machine.

5. To connect to the VM for debugging or inspection:

	`vagrant ssh`

5. When finished, stop the VM:

	`vagrant halt`

6. To remove the VM and all state (destructive):

	`vagrant destroy`

**Common Commands**
- `vagrant status` — show VM status
- `vagrant reload --provision` — restart and re-run provisioning
- `vagrant provision` — re-run provisioning scripts without rebooting
- `vagrant ssh -c "command"` — run a single command on the guest and exit

**Where to edit site files**
By default, the project folder is synced into the guest VM (usually at `/vagrant`). Edit files on your host inside this `static-web` folder; changes should be reflected immediately (or after the webserver reloads) inside the VM.


**Troubleshooting**
- If `vagrant up` fails: ensure your provider (VirtualBox) is installed and that virtualization is enabled in BIOS/UEFI.

---
Last updated: 2025-11-18