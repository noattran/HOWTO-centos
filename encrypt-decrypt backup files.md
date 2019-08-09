# Encrypt:
```
tar cz backup-files | openssl enc -aes-256-cbc -e > backup-files.tar.gz.enc
```
# Decrypt
```
openssl enc -aes-256-cbc -d -in backup-files.tar.gz.enc | tar xz
```
