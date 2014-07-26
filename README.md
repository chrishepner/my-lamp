#my-lamp

*my-lamp* makes it easy to for teams of one or more to develop simple LAMP (Linux/Apache/MySQL/PHP) applications without having to spend unnecessary time on configuration.

*my-lamp* provisions the base server configuration and LAMP stack, and also looks for a developer-specific local.yml file for specifying what folders should be forwarded to the VM and specifying additional MySQL databases and users. This allows your development team to use a common virtual machine, but allows developers to work with different projects and different paths on the host machine without hard-coding these in version control.

## Requirements

*my-lamp* is based on Red Hat Enterprise Linux (RHEL) 6 and uses [Vagrant](http://www.vagrantup.com) to manage the VM and [Ansible](http://www.ansible.com.home) to provision the LAMP stack.

You will need to have the following installed before using *my-lamp*:

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://www.vagrantup.com/downloads.html)
* [Ansible](http://www.ansible.com) ([instructions for Mac OS X](https://devopsu.com/guides/ansible-mac-osx.html))



## Usage

To download *my-lamp*, first clone the repository, either using your favorite Git client or via the terminal:
```
git clone git@github.com/chrishepner/my-lamp.git
```

This will create a "my-lamp" folder in the current directory. This contains the configuration for building and provisioning your VM.

To initialize and provision the development virtual machine, run `vagrant up` in the terminal from this directory:
```
cd my-lamp
vagrant up
```
By default, this configuration forwards the web server to listen on port 8080. Content in the DocumentRoot (/var/www/html/) on the VM will be viewable at http://localhost:8080 in your local browser.

To specify which folders  on the local machine should be forwarded to the web server on the virtual machine, copy local.yml.example to local.yml and edit as necessary.

If you make any changes to local.yml while the box is up, you will need to run `vagrant reload --provision`. If you only change the forwarded folders, you can save some time by running `vagrant reload` only. 

If you need to make any changes or additions to the base configuration (e.g., if you need to install additional PHP extensions) you'll want to fork this repository and make your own changes. 

Enjoy!

## License

*my-lamp* is licensed under the [MIT License](LICENSE). If you use this I'd love to hear from you!
