# Abstract

This is an attempt to document how to get things in Linux (specifically Ubuntu) to work behind a corporate proxy where MITM may be required.

## View certs

`
test
`
## Operating Systems

### Ubuntu
1. Place the pem in /usr/local/share/ca-certificates/
   1. NOTE: can be placed in a sub-directory for cleanliness 
2. run update-ca-certificates
### Redhat
1. Place the pem in /etc/pki/ca-trust/source/anchors
2. run update-ca-trust
## Chrome and Electron

``

## Firefox

## Local Store

## Other Things

### pip/pip3

pip/pip3 may require the cert chain.