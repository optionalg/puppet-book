### Puppet Master and Agent with facter and hiera

- Setup puppet master and puppet agent.
- Test out basic consumption of catalogue in site.pp
- Show off how to use specific domain names.
- Show off that facter is a agent-side thingy.
- Show off that heira is a  master-side thingy.


#### Setting up puppet master and puppet agent

Setup as per steps:
https://s3-ap-southeast-1.amazonaws.com/puppet.training.sbose.in/10-master-agent-puppet.htm
 
#### Show off specific domain names.

The command `hostname -f` gives the full hostname.

```
node /^(foo|bar)\.example\.com$/ {
  include common
}

# The above example would match foo.example.com and bar.example.com, but no other nodes.
```


Run the below `site.pp` and show how the content in the agent changes.

```
# node 'www1.example.com', 'www2.example.com', 'www3.example.com' {

node "ip-172-31-0-114.ap-south-1.compute.internal.yaml"{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey I'm from the host specific block inside master",  # note the ipaddress_eth0 fact
     }
}

node default{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey I'm from the default :(  ",  # note the ipaddress_eth0 fact
     }
}
```


#### Show off that facter is a `agent`-side thingy :

```
node "ip-172-31-0-114.ap-south-1.compute.internal.yaml"{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey I'm from the host specific block inside master.Public IP Address: ${ipaddress_eth0} ",  # note the ipaddress_eth0 fact
     }
}

node default{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey I'm from the default :( My IP is Public IP Address: ${ipaddress_eth0}   ",  # note the ipaddress_eth0 fact
     }
}
```


#### Show off that heira is a `master`-side thingy :


1. Configure data dir and hiera conf : https://s3-ap-southeast-1.amazonaws.com/puppet.training.sbose.in/12-hierra.htm

2. Update `site.pp`
```
node "ip-172-31-0-114.ap-south-1.compute.internal.yaml"{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey ${magic_word} I'm from the host specific block inside master.Public IP Address: ${ipaddress_eth0} ",  # note the ipaddress_eth0 fact
     }
}

node default{
     file {'/tmp/mastersetup-success':                                   # resource type file and filename
       ensure  => present,                                               # make sure it exists
       mode    => 0644,                                                  # file permissions
       content => "Hey ${magic_word} I'm from the default :(  My IP is Public IP Address: ${ipaddress_eth0}  ",  # note the ipaddress_eth0 fact
     }
}
```


