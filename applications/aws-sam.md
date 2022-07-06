# AWS-SAM

Note: From https://www.shellhacks.com/aws-cli-ssl-validation-failed-solved/

## Ubuntu
Example:
```{bash}
$ cat ~/aws/.config
[default]
ca_bundle = /etc/ssl/certs/ca-certificates.crt
```

### Alternatives

#### Environment
- Ubuntu/Linux
```
export AWS_CA_BUNDLE="/etc/ssl/certs/ca-certificates.crt"
```

- Windows
```
setx AWS_CA_BUNDLE C:\data\ca-certs\ca-bundle.pem
```


#### Cli
```
aws s3 ls --ca-bundle "/data/ca-certs/ca-bundle.pem"
```
