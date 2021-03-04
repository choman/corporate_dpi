# Abstract

This is an attempt to document how to get things in Linux (specifically Ubuntu) to work behind a corporate proxy where MITM may be required.

## View certs

### Ubuntu
```
awk -v cmd='openssl x509 -noout -subject' '
    /BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-certificates.crt
```
### Redhat
```
awk -v cmd='openssl x509 -noout -subject' '
    /BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-bundle.crt
```
## Operating Systems

### Ubuntu
1. Place the pem in /usr/local/share/ca-certificates/
   1. NOTE: can be placed in a sub-directory for cleanliness 
2. run update-ca-certificates
### Redhat
1. Place the pem in /etc/pki/ca-trust/source/anchors
2. run update-ca-trust

## Applications

### Chrome and Electron

```
certutil -d sql:$HOME/.pki/nssdb -A -t "C,," -n <certificate nickname> \
-i <certificate filename>
```

### Firefox

### Local Store

### Other Things

### Ubuntu ppas

The ppa process on Ubuuntu uses the httplib2 library. This module appears to use
the cacert.pen bundled under/usr/lib/python3/dist-packages/certifi/cacert.pem

copy /etc/ssl/certs/ca-certificates.crt to this file
```bash
sudo cp /etc/ssl/certs/ca-certificates.crt /usr/lib/python3/dist-packages/certifi/cacert.pem
```

### pip/pip3

One needs to modify the pip.conf to contain the following

```
[global]
cert = <path_to_cert_bundle_file>
```
#### RHEL/CentOS

- certfile: cert = /etc/ssl/certs/ca-bundle.crt
- pip.conf
  - /etc/pip.conf
  - ${HOME}/.config/pip/pip.conf

### vagrant

#### Ubuntu 

For this to work use the deb file from linux. After install, cp the 
cacert.pem to a backup file and replace it with the system store version

```
cd /opt/vagrant/embedded
sudo cp -p cacert.pem cacert.pem.BAK
sudo cp /etc/ssl/certs/ca-certificates.crt cacert.pem
```
