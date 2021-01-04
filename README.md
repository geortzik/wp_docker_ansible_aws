## Provision a Wordpress Docker container with Ansible on an AWS EC2 VM instance

This repository is part of a bigger project. All necessary resources needed for proper WordPress installation with this playbook are deployed from HCL code in the repository [wp-terraform-aws](https://github.com/geortzik/wp-terraform-aws).

This playbook is written for an EC2 VM instance running Ubuntu 20.04 LTS (Focal) and an RDS DB instance running a mySQL server.

### Configuration


The following table lists the configurable parameters of the playbook and their default values. These can be found in `./group_vars/all.yml`.

Parameter | Description | Default
--------- | ----------- | -------
`docker_repo_gpg_key_url` | gpg key url | `"https://download.docker.com/linux/ubuntu/gpg"`
`docker_repo_deb_repo_url` | Docker repository. Currently adjusted for Ubuntu 20.04 LTS (Focal). Keep in mind that you may need to change that to fit your host's OS. e.g. if it is Ubuntu 18.04 LTS you should change `focal` with `bionic` | `"deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"`
`wordpress_docker_image` | Docker image from docker hub | `"wordpress:5.6.0-apache"`
`wordpress_db_name` | MySQL db name for the application | `"wordpress"`
`wordpress_db_user` | MySQL wordpress db user name | `"wordpress"`
`db_admin_user` | Username for logging in RDS db instance | `"rds_admin"`
`db_host` | RDS db endpoint | `"CHANGEME"`
`db_admin_password` | Password for logging in RDS db instance. Provide this parameter to Ansible using command line arguments. | `" "`
`wordpress_db_password` | MySQL wordpress db password. Provide this parameter to Ansible using command line arguments. | `" "`


If you want more info on how to use docker image and its environment variables, you should check docker hub:

[https://hub.docker.com/_/wordpress](https://hub.docker.com/_/wordpress)


### How to run the playbook

You have to pass the path to your ssh private key for your hosts through the command line and define the user:

```

ansible-playbook -i hosts -u ubuntu --private-key=/your/location/foo.pem provision_wp_container_host.yml

```
> Note: You have to change your target host(s) in `hosts` inventory file.

### Secrets

From a security perspective you should find a way to hide your secrets (e.g. passwords) from plain sight. Ansible Vault is a good choice for this task. Although, in order to keep things as simple, we provide secrets to Ansible using command line arguments:


```

ansible-playbook -i hosts -u ubuntu --private-key=/your/location/foo.pem --extra-vars "db_admin_password=your_rds_pass_here wordpress_db_password=your_wp_new_user_pass_here" provision_wp_container_host.yml

```

### License

All code is licensed under the [GNU General Public License version 3](https://www.gnu.org/licenses/gpl-3.0.html).
