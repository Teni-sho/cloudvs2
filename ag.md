# Deployment of a PHP Laravel Application on an Ubuntu Server with the LAMP Stack

This documentation provides a straightforward guide on leveraging the repository mentioned above to deploy a Laravel application on an Ubuntu server equipped with a LAMP stack (Linux, Apache, MySQL, and PHP).

In this infrastructure setup, Apache will serve as the web server, MySQL will handle database operations, and PHP will serve as the foundational programming language for the Laravel application.

Additionally, a master-slave architecture will be established through the execution of a Bash script. On the master machine, the process will involve running a script to install the LAMP stack and deploy the Laravel application.

> **Notice**: *It's important to emphasize that the script operates in a **non-interactive** mode, requiring no user input.*

On the slave machine, we will employ an Ansible playbook to execute a script similar to the one used for deploying Laravel and the LAMP Stack, with minor variations.

Additionally, Ansible will be utilized to facilitate the creation of a cron job that monitors server uptime at the stroke of midnight, 12 AM.

Are you prepared? Let's commence!

---

## Repository Breakdown Analysis


Upon reviewing the repository above, it becomes evident that it consists of one primary directories:

- Ansible-playbook


Our next step is to delve into these directory, comprehending it's respective functions and purposes.

---

### Ansible-Playbook

Upon entering the Ansible playbook directory, you will find a total of three files:

- **ansible.cfg**: This file serves as the repository's central configuration point for Ansible.

- **site.yaml**: This file encapsulates all the defined tasks and plays that are intended for execution.

- **inventory**: Within this file, you will find the specifications pertaining to the domain or IP addresses designated for Laravel application deployment.


Additionally, within the repository, the file named **site.yaml** has 5 tasks The primary function of this file is to perform the following tasks:


- Updating and upgrading the slave machine
- Copy the Bash script responsible for deploying Laravel.
- Set the script as executable.
- Execute the script utilizing the Ansible commands module.
- Creating a scheduled cron job that monitors server uptime, running at midnight (12 AM), all orchestrated through Ansible.



Within the same directory, you will find a concise set of two files:

- **master-slave.sh**: This file serves as the container for a script that orchestrates the deployment of both a master and slave Ubuntu Vagrant machine, each configured with a specific **static IP** address assignment. The slave is allocated the IP address **192.168.20.11**, while the master machine is designated with the IP address **192.168.20.10**.

- **laravel.sh**: This file encompasses the script responsible for executing the deployment of the **LAMP Stack** and **Laravel application** on both the master and slave machines.

> **Note:** *the script is non-interactive*

# HOW TO RUN THE SCRIPT

This script is highly legible and adaptable, thus ensuring its flexibility.

When executing this script, it is imperative to address four key considerations:

- **MySQL database** configuration.
- The Laravel application's **.env file**.
- The **ansible.cfg** configuration file.

Appropriately configured inventory to define the target server(s).

## MySql

Given that we employ parameters for dynamic database creation during script execution, you will be required to provide merely two specific arguments.

```bash
./laravel.sh teni teni
```

The first argument is designated for configuring both the **DB_USERNAME** and **DB_DATABASE** parameters.

The second argument is dedicated to specifying the **DB_PASSWORD**.

> **Important Note**: It's imperative to ensure that the chosen database name username and password align precisely with the credentials outlined in the **.env** configuration file.

## .ENV (laravel file)

As the entire process operates automatically, we must also implement automatic adjustments to the .env configuration file.

```bash
sudo sed -i 's/DB_DATABASE=laravel/DB_DATABASE=teni/' /var/www/html/laravel/.env

sudo sed -i 's/DB_DATABASE=laravel/DB_DATABASE=teni/' /var/www/html/laravel/.env

sudo sed -i 's/DB_PASSWORD=/DB_PASSWORD=teni/' /var/www/html/laravel/.env
```

Ensure that these three lines align with the arguments you will be introducing when executing the script. These lines are located within the **laravel.sh** file, and your modifications should be directed solely at the values following the = symbol on the right-hand side.

## Ansible Configuration File

Within the **laravel.sh** file, you will come across a dedicated section for the Ansible configuration file. In this context, your exclusive adjustment pertains to the **ServerName**, which you will configure to reflect the appropriate domain name or IP address corresponding to the server on which you intend to execute the script.

With these considerations in place, you are now prepared to initiate the script as previously demonstrated.
