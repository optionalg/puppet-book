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
4. Additional Setup tasks:
    - Confirm that Puppet is present and installed.
    - Execute ```puppet -version```
    - Check the puppet directories by doing a  ```ls -ltr /etc/puppet```
    - Test whether the puppet installation is fine. Open the file ```/etc/puppet/manifests/site.pp``` and write to it.
    ```
    $ sudo vim /etc/puppet/manifests/site.pp
    
    node default{
        file { '/tmp/file1':
          ensure => file,
        }
       notify { "We are creating a file using puppet ;) ": }
    }
    ```
5. Execute ```puppet apply /etc/puppet/manifests/site.pp``` . Check if the file /tmp/file1 has be created successfully! If yes, Congratulations! Your Puppet installation has been tested :)
6. Update the code to add another file, but this time we won’t apply the changes. We’ll test it first.
    ```
    node default{

        file { '/tmp/file1':

          ensure => file,

        }

        notify { "System is up": }

        file { '/tmp/file2':

          ensure => file,

        }

 }
 ```
7. Execute ```puppet apply –noop /etc/puppet/manifests/site.pp```.
    Even though the code mandates creation of a second file, it doesn't get created because of the `-noop` flag. Instead the output prints to console what would happen if the manifest was **actually** executed.
8. Now, execute ```puppet apply /etc/puppet/manifests/site.pp``` to see the file `/tmp/file1` actually getting created.
