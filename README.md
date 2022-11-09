# ssh-menu
## Version 1.0.0


This is a simple Perl script for SSH key and certificate management. 

It is a wrapper around **ssh-keygen** tool.

Features:
*   make simple SSH CA
*   make SSH KEY
*   sign SSH KEY by this CA
*   export SSH KEY from native OpenSSH format to other formats
*	import SSH KEY from various formats to native OpenSSH format.
*   print KEY, CRT or CA fingerprint to console
    
Default encoding is native OpenSSH but keys can be imported from orexported to varios other formats: **PEM**, **RFC4716/SSH2** or **PKCS#8**.
All key files will have extensions **.key** for private key and  **.pub.key** for public key.

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

