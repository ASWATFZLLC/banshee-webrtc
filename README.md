# Installation

## Development
```sh
git clone https://github.com/ASWATFZLLC/banshee-webrtc
cd banshee-webrtc
npm install -g grunt grunt-cli bower
npm install
bower install
grunt serve
```

## Create distribution
```
cd banshee-webrtc
grunt build
cp -r ./dist/* /var/www/
```

# Setup banshee (mod_verto)

## Create certificates:
```sh 
wget http://files.freeswitch.org/downloads/ssl.ca-0.1.tar.gz
tar zxfv ssl.ca-0.1.tar.gz
cd ssl.ca-0.1/
perl -i -pe 's/md5/sha256/g' *.sh
perl -i -pe 's/1024/4096/g' *.sh
./new-root-ca.sh
./new-server-cert.sh freeswitch.local
./sign-server-cert.sh freeswitch.local
cat freeswitch.local.crt freeswitch.local.key > /usr/local/freeswitch/certs/wss.pem
```

## Setup vars.xml:
 ```xml
<X-PRE-PROCESS cmd="set" data="internal_ssl_enable=true"/>
<X-PRE-PROCESS cmd="set" data="external_ssl_enable=true"/>
```

## Setup Apache:
```
SSLCertificateFile    /usr/local/freeswitch/certs/wss.pem
SSLCertificateKeyFile /usr/local/freeswitch/certs/wss.pem
SSLCertificateChainFile /usr/local/freeswitch/certs/wss.pem
```

