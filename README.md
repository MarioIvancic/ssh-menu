# ssh-cert-menu
## Version 1.1.0


This is a simple Perl script for SSH key and certificate management. 

It is a wrapper around **ssh-keygen** tool.

Features:
*   make simple SSH CA
*   make SSH KEY
*   sign SSH KEY by this CA
*   export SSH KEY from native OpenSSH format to other formats
*	import SSH KEY from various formats to native OpenSSH format.
*   print KEY, CRT or CA fingerprint to console
    
Default encoding is native OpenSSH but keys can be imported from or exported to various other formats: **PEM**, **RFC4716/SSH2** or **PKCS#8**.
All key files will have no extensions but it will end with  **_key** for private key and  **_key.pub** for public key and **_key-cert.pub** for certificate.

CA KEY will be 4096 bit RSA key by default, but any other key type can be used. All other keys will default to 2048 bits RSA keys, but can be any of 1024, 2048, 3072, 4096 bits for RSA, 1024 bits for DSA, or 256, 384 or 521 bits for ECDSA. 
ECDSA-SK, Ed25519 and Ed25519-SK keys have a fixed length.

## OpenSSH commands

print KEY:
```sh
ssh-keygen -l -f $key
```

print CRT
```sh
ssh-keygen -L -f $crt
```

make new KEY (2048 bit RSA)
```sh
ssh-keygen -t rsa -b 2048 -f $key
```

make new CA (type: host)
```sh
ssh-keygen -t rsa -b 4096 -N '' -C host_ca -f $key
```

make new CA (type: user)
```sh
ssh-keygen -t rsa -b 4096 -N '' -C user_ca -f $key
```

make new CRT (valid for 365 days, for host)
```sh
ssh-keygen -s $host_ca_private_key -V +365d -h -z $serial -I $identity -n $principal_list $host_public_key
```

make new CRT (valid for 365 days, for user)
```sh
ssh-keygen -s $user_ca_private_key -V +365d -z $serial -I $identity -n $principal_list $user_public_key
```

export KEY to PEM/RFC4716/PKCS8
```sh
ssh-keygen -e -f $inkey -m $outfmt > $outkey
```

import KEY from PEM/RFC4716/PKCS8
```sh
ssh-keygen -i -f $inkey -m $infmt > $outkey
```

In this version v1.1.0:
* script is renamed from ssh-menu to ssh-cert-menu.
* User CA and host CA now can be separated. 
* Operation mode is selected by command line argument or by script or link name if you make copy of it or link to it.
* There is a install feature where script is copied to /usr/local/bin/ssh-host-cert-menu and link is made to it named /usr/local/bin/ssh-user-cert-menu.

For **user** mode rename it or copy it or link to it by name ssh-user-cert-menu or call it like this:
```sh
ssh-cert-menu user
```

For **host** mode rename it or copy it or link to it by name ssh-host-cert-menu or call it like this:
```sh
ssh-cert-menu user
```

For **installation** to /usr/local/bin:
```sh
ssh-cert-menu install
```
The installed scripts will be /usr/local/bin/ssh-host-cert-menu and /usr/local/bin/ssh-user-cert-menu.

Some useful links: 
* https://goteleport.com/blog/how-to-ssh-properly/
* https://goteleport.com/blog/comparing-ssh-keys/
* https://smallstep.com/blog/use-ssh-certificates/


