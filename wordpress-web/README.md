# WordPress Vagrant Box

This Vagrant project boots a single Ubuntu VM and provisions a fully-working
WordPress site using Apache, PHP, and MySQL. The provisioning script (in the
`Vagrantfile`) installs required packages, downloads WordPress into
`/srv/www/wordpress`, configures Apache, and creates a MySQL database and user.

**Quick summary:** boot the VM with `vagrant up` and open the site at
`http://192.168.33.15` on the host machine.

![Wordpress-dashboard](../assets/wordpress-install.png)
![Wordpress-dashboard](../assets/wordpress-dashboard.png)

**Prerequisites**
- **Vagrant** (tested with Vagrant 2.x)
- **VirtualBox** (or another provider supported by Vagrant)
- Internet connection to download packages and WordPress
- On Windows: run commands from Git Bash / WSL or an elevated terminal where
	VirtualBox and Vagrant are available

**Provisioning Details**
- Base box: `ubuntu/jammy64`
- Installs: `apache2`, `mysql-server`, `php` and several PHP extensions
- Downloads WordPress to: `/srv/www/wordpress`
- Apache site config: `/etc/apache2/sites-available/wordpress.conf`
- MySQL: creates a database `wordpress` and a dedicated user `wordpress@localhost`
- The `wp-config.php` file is updated automatically during provisioning

Relevant provisioning excerpts (see `Vagrantfile`):
- Private network IP configured to: `192.168.33.15`
- VM memory: set in the provider block via `vb.memory` (default `1024`)

**Usage**
1. Clone or open this project and change to the `wordpress-web` directory:

	```bash 
    git clone https://github.com/CR7578/Vagrant-demos.git
    cd Vagrant-demos/wordpress-web
    ```

2. Boot the VM (this runs the provisioning script on first `up`):

	 `vagrant up`

3. Open your browser and visit:

	 `http://192.168.33.15`

4. Complete the WordPress web installer (create admin account, site title, etc.).

5. To SSH into the VM:

	 `vagrant ssh`

6. To re-run provisioning after edits to `Vagrantfile` or provisioning script:

	 `vagrant provision`

7. Halt or destroy the VM when finished:

	 `vagrant halt`

	 `vagrant destroy -f`

**Database & Credentials**
- Database name: `wordpress`
- DB user: `wordpress@localhost`
- DB password: `Word@123`
- WordPress files: `/srv/www/wordpress`

Notes:
- The provisioning script uses `mysql -u root` (on Ubuntu this typically uses
	socket authentication). If you encounter authentication issues, SSH into the
	VM and run `sudo mysql` to access the MySQL shell for debugging.

**Customizations**
- Increase VM RAM: edit the `vb.memory` value in `Vagrantfile` (provider block)
- Change the VM IP: edit `config.vm.network "private_network", ip: "..."`
- Add synced folders: uncomment or add `config.vm.synced_folder` lines to share
	host directories with the VM (see commented example in `Vagrantfile`)
- Change DB credentials: update the MySQL commands and the `sed` lines that
	modify `wp-config.php` in the provisioning script

**Troubleshooting**
- If provisioning fails partway through, try `vagrant provision` or
	`vagrant up --provision` to retry provisioning.
- If Apache won't start, SSH into the box and check logs:

	`sudo systemctl status apache2`
	`sudo journalctl -u apache2 --no-pager`

- If MySQL commands fail, check MySQL status and logs:

	`sudo systemctl status mysql`
	`sudo journalctl -u mysql --no-pager`

- If the site does not load from the host, verify network settings and that
	the host can reach `192.168.33.15` (ensure VirtualBox host-only network is
	configured correctly on Windows).

**Security & Cleanup**
- The VM provisions a MySQL user with a hard-coded password for convenience in
	development. Do not use these credentials in production. If you change the
	password, update the `wp-config.php` replacement lines in the provisioning
	script accordingly.
- To fully remove the VM and associated storage:

	`vagrant destroy -f`

**License**
This repository contains example provisioning code for development/demo
purposes. Use and adapt as you like.

If you'd like, I can also:
- Add an example `Vagrantfile` variable block for customizing DB credentials
- Add a synced-folder example that maps the project into `/srv/www`

---
Updated to include full usage, provisioning, and troubleshooting instructions.