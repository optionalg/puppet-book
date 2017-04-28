

In this section, we will create our own learner's NGINX module which can be used to server a couple of websites:

`/etc/hosts/nginx/manifests/init.pp`

```
class nginx {

  package { 'nginx': ensure => installed }
 
  service { 'nginx':
    ensure => running,
    enable => true, # Start the service on system reboot.
    require => Package['nginx'] # Forcing the correct order.
  }
 
  exec { 'reload nginx':
    command     => '/usr/sbin/service nginx reload',
    require     => Package['nginx'],
    refreshonly => true,  # Execute only if some resource has changed.
  }

}
```


`manifests/vhost.pp`

```
class  nginx::vhost {

 $default_parent_root = "/home/ubuntu/nginxsites-puppet"
 $dir_tree = [ "$default_parent_root"]
 file { $dir_tree :
          owner   => 'ubuntu',
          group   => 'ubuntu',
          ensure  => 'directory',
         mode    => '777',
  }


 define apply( $domain="UNSET",$root="UNSET"){  

    include nginx  # Class was declared inside init.pp

    # Default value overrides

    if $domain == 'UNSET' {
      $vhost_domain = $name
    } else {
      $vhost_domain = $domain
    }

    if $root == 'UNSET' {
      $vhost_root = "$default_parent_root/${name}"
    } else {
      $vhost_root = $root
    }


    # Creating the virtual host conf file out of the template in nginx/templates directory

    file { "/etc/nginx/sites-available/${vhost_domain}.conf":
      content => template('nginx/vhost.erb'), # vhost.erb is present in nginx/templates/
      require => Package['nginx'],
      notify  => Exec['reload nginx'], # Resource was declared in init.pp
    }

    # Enabling the site by creating a sym link from sites-available to sites-enabled

    file { "/etc/nginx/sites-enabled/${vhost_domain}.conf":
      ensure  => link,
      target  => "/etc/nginx/sites-available/${vhost_domain}.conf",
      require => File["/etc/nginx/sites-available/${vhost_domain}.conf"],
      notify  => Exec['reload nginx'],
    }

    addStaticFiles{ "staticfiles-${vhost_root}":
    default_parent_root =>  $default_parent_root, 
    vhost_root => $vhost_root,
    vhost_domain => $vhost_domain 
    }
 }
 
 define addStaticFiles( $default_parent_root, $vhost_root , $vhost_domain ){

 	 $dir_tree = [ "$vhost_root" ]
	 file { $dir_tree :
        	  owner   => 'ubuntu',
	          group   => 'ubuntu',
	          ensure  => 'directory',
	          mode    => '777',
 	  }
	  ->   # This arrow ensures that the dir structure is created first.
	  file {  ["$vhost_root/index.html"]:
       	     owner   => 'ubuntu',
	            group   => 'ubuntu',
	            source => "puppet:///modules/nginx/${vhost_domain}/index-html", # index.html was dropped under nginx/files/
        	    mode    => '755',
	  }
   }
}
```

`/etc/puppet/manifests/site.pp`


```
node vagrant-ubuntu-trusty-32 {
        nginx::vhost::apply{ "web1":
                domain => "site1.puppet.sbose.in",
                root => "/home/ubuntu/site1"
        }
        nginx::vhost::apply{"web2":
                domain => "site2.puppet.sbose.in",
                root => "/home/ubuntu/site2"

        }
}
```

Inside `/etc/puppet/modules/nginx/files` create a `site1` and `site2` directory which would have respective `index-html` files.


Now - the icing on the cake.. manually add the `/etc/hosts` entries.
