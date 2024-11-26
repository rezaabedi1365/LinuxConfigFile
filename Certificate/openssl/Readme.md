
### How to Convert pfx to pem
https://www.xolphin.com/support/Certificate_conversions/Convert_pfx_file_to_pem_file

##### To convert a PFX file to a PEM file that contains both the certificate and private key, the following command needs to be used:
```
openssl pkcs12 -in filename.pfx -out cert.pem -nodes 
```
 
convert to seperate pem
```
# We can extract the private key form a PFX to a PEM file with this command:
openssl pkcs12 -in filename.pfx -nocerts -out key.pem

# Exporting the certificate only:
openssl pkcs12 -in filename.pfx -clcerts -nokeys -out cert.pem
```
Remove persharkey from key
```
openssl rsa -in key.pem -out server.key 
```
### Generate PFX file from private key and CRT files
* Method1
  - https://serverfault.com/questions/1097033/generate-pfx-file-from-private-key-and-crt-files
  - Combine the CRT files (ServerCertificate.crt then Intermediate.crt then root.crt) into a single chain.pem file
   ```
   openssl.exe pkcs12 -in chain.pem -inkey PRIVATEKEY.key -export -out myPrivateCert.pfx
   ```
* Method2
  - https://stackoverflow.com/questions/6307886/how-to-create-pfx-file-from-certificate-and-private-key
   ```
   openssl pkcs12 -export -out domain.name.pfx -inkey domain.name.key -in domain.name.crt -in intermediate.crt -in rootca.crt
   ```

### How to generate a self-signed SSL certificate using OpenSSL

method 1
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout selfsigned.key -out selfsigned.crt
```
method 2
```
# interactive
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365

# non-interactive and 10 years expiration
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes -subj "/C=XX/ST=StateName/L=CityName/O=CompanyName/OU=CompanySectionName/CN=CommonNameOrHostname"
```
method 3

```
openssl req -new > cert.csr
openssl rsa -in privkey.pem -out key.pem
openssl x509 -in cert.csr -out cert.pem -req -signkey key.pem -days 1001
cat key.pem>>cert.pem
```
