### Facter



```
#!/bin/sh

echo users=`who`
```

Use the following codebase:
https://github.com/sbose78/nginx-puppet/tree/server-info



### Heira 

```
# common.yaml

domain_usage  : "localhost"
```

```
# /etc/puppet/data/vagrant-ubuntu-trusty-32.yaml

domain_usage : 'sites.puppet.sbose.in'
```

https://github.com/sbose78/nginx-puppet/tree/hiera
