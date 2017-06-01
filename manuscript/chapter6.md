Reports:

Install bouncycastle using `apt-get install libbcprov-java`


Update `/etc/java-8-openjdk/security/java.security`


```
security.provider.1=sun.security.provider.Sun
security.provider.2=org.bouncycastle.jce.provider.BouncyCastleProvider
security.provider.3=sun.security.rsa.SunRsaSign
security.provider.4=sun.security.ec.SunEC
security.provider.5=com.sun.net.ssl.internal.ssl.Provider
security.provider.6=com.sun.crypto.provider.SunJCE
security.provider.7=sun.security.jgss.SunProvider
security.provider.8=com.sun.security.sasl.Provider
security.provider.9=org.jcp.xml.dsig.internal.dom.XMLDSigRI
security.provider.10=sun.security.smartcardio.SunPCSC
```

- restart `apache` , `puppet`, `puppetmaster`
- optionally add `SSLCipherSuite RSA:!EXP:!NULL:+HIGH:+MEDIUM:-LOW` to `/etc/apache2/sites-enabled/05-foreman-ssl.conf`
- check log file tailf `/var/log/puppetlabs/puppetserver/puppetserver.log`
