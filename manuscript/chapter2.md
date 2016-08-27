# The Learning Environment

### Puppet setup options

The learning environment can be setup in 2 ways

- Standalone mode : No Master or Slave specification, configuration is consumed on the same server, where it is specified.
- Master-Agent mode : The Master defines the configuration, and the agent polls the master at regular intervals to consume it.


### Vagrant Setup

The Puppet development environment can be setup inside a Vagrant VM for easier testing. There are 3 ways to do the same :

1. Define Puppet as a provisioning method, and changes get applied at the VM boot time
2. Keep the Puppet configuration application provisioning agnostic. Boot up VM and explicitly apply changes.
3. Run Puppet as a service which automatically applies changes every 30min.

For simplicity during learning, we will use Type 2. Puppet Setup Type 1 and Vagrant Setup Type 3 will be explored in the later chapters.

1. Installing VirtualBox: 

    ```
    sudo apt-get install dpkg-dev virtualbox-dkms
    ```

2. Installing Vagrant ( https://www.vagrantup.com/downloads.html  )  : 
    
    ```
    $ wget  https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb
    $ sudo dpkg –i <downloaded_file>
    ```

3. Importing the  Base Box: 

    - Create the directory under which you want your VM information to be present.
    - vagrant init
    - Edit the Vagrant file to update as follows: config.vm.box = "ubuntu/trusty32"
    - vagrant up
    - vagrant ssh
    
      ```
      $ mkdir my-vagrant-home ; cd my-vagrant-home
      $ vagrant init
      $ vim VAGRANT
      $ vagrant up
      $ vagrant ssh
      ```
