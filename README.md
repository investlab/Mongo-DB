CNs are important!!! -days 3650

#### Make PEM containig a public key certificate and its associated private key
```bash
openssl req -newkey rsa:2048 -new -x509 -days 3650 -nodes -subj '/C=VN/ST=Hanoi/L=Hanoi/O=Nexttech/OU=IT/emailAddress=example@example.com/CN=ipa.dinhduylong.co' -out mongodb-cert.crt -keyout mongodb-cert.key
```
```bash
cat mongodb-cert.key mongodb-cert.crt > mongodb.pem
```
```bash
cp mongodb-cert.crt mongodb-ca.crt
```

#### Edit _/etc/mongod.conf_, _network interfaces_ section
```apache
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1
  ssl:
    mode: allowSSL
    PEMKeyFile: /etc/ssl/mongodb.pem
    CAFile: /etc/ssl/mongodb-cert.crt
```

#### Check for startup config errors
```bash
sudo mongod --config /etc/mongod.conf
```

#### Restart mongo
```bash
sudo service mongod restart
```

#### Test-connect
```bash
mongo --ssl --sslAllowInvalidHostnames --sslCAFile mongodb-ca.crt --sslPEMKeyFile /etc/ssl/mongodb.pem
```

#### NodeJs, mongo connection options
```js
{ 
	ssl: true,
	sslValidate: true,
	sslKey: fs.readFileSync('/etc/ssl/mongodb.pem'),
	sslCert: fs.readFileSync('/etc/ssl/mongodb-cert.crt'),
	sslCA: fs.readFileSync('/etc/ssl/mongodb-ca.crt')
}
```
