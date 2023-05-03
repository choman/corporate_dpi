# Abstract

This is an attempt to document how to get things in Linux (specifically Ubuntu)
to work behind a corporate DPI/proxy where MITM may be required.

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


## Runtimes

### Ubuntu
Add to the top-level direnvrc file: ~/.config/direnv/direnvrc
```
# SSL CERT FILE Variables
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
export NODE_EXTRA_CA_CERTS="${SSL_CERT_FILE}"
```


## Applications

- [aws-sam](applications/aws-sam.md)
- [chrome/electron](applications/chrome-electron.md)
- [electron/chrome](applications/chrome-electron.md)


### Chrome and Electron

```
certutil -d sql:$HOME/.pki/nssdb -A -t "C,," -n <certificate nickname> \
-i <certificate filename>
```

### Firefox

### Local Store

### Misc

#### Ubuntu ppas
- [Ubuntu ppas]

The ppa process on Ubuntu uses the httplib2 library. This module appears to use
the cacert.pen bundled under/usr/lib/python3/dist-packages/certifi/cacert.pem

copy /etc/ssl/certs/ca-certificates.crt to this file
```bash
sudo cp /etc/ssl/certs/ca-certificates.crt /usr/lib/python3/dist-packages/certifi/cacert.pem
```
#### poetry

Append certs to the cacert.pem, example locations are:
```
find $HOME/.poetry/lib/poetry/_vendor -name cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py3.6/certifi/cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py3.9/certifi/cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py2.7/certifi/cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py3.7/certifi/cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py3.5/certifi/cacert.pem
${HOME}/.poetry/lib/poetry/_vendor/py3.8/certifi/cacert.pem


```

#### pip/pip3

One needs to modify the pip.conf to contain the following

```
[global]
cert = <path_to_cert_bundle_file>
```
##### RHEL/CentOS

- certfile: cert = /etc/ssl/certs/ca-bundle.crt
- pip.conf
  - /etc/pip.conf
  - ${HOME}/.config/pip/pip.conf

#### vagrant

##### Ubuntu 

For this to work use the deb file from Linux. After install, cp the 
cacert.pem to a backup file and replace it with the system store version

```
cd /opt/vagrant/embedded
sudo cp -p cacert.pem cacert.pem.BAK
sudo cp /etc/ssl/certs/ca-certificates.crt cacert.pem
```
