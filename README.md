# Ricette per OpenSSL
simple tasks using openssl library

* [Verificare la versione di OpenSSL in uso](#verificare-la-versione-di-openssl-in-uso)
* [Creazione di una coppia di chiavi privata/pubblica](#creazione-di-una-coppia-di-chiavi-privatapubblica)

----

## Verificare la versione di OpenSSL in uso

Per ottenere una infomrazione sintetica sulla versione attualmente installata:

`$ openssl version`

con il seguente risultato:

    OpenSSL 1.0.0 29 Mar 2010

Per ottenere informazionei piu' dettagliate:

`$ openssl version -a`

con il seguente risultato:

    OpenSSL 1.0.0 29 Mar 2010
    built on: Wed Apr 14 18:16:40 EDT 2010
    platform: MSys
    options:  bn(64,32) rc4(4x,int) des(ptr,risc1,16,long) blowfish(idx)
    compiler: gcc -D_WINDLL -DOPENSSL_PIC -DOPENSSL_THREADS  -DDSO_DLFCN -DHAVE_DLFCN_H -DTERMIOS -DL_ENDIAN -D__CYGWIN__ -fomit-frame-pointer -O3 -march=i386 -mtune=i686 -Wall -DOPENSSL_BN_ASM_PART_WORDS -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT-DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DRMD160_ASM -DAES_ASM -DWHIRLPOOL_ASM
    OPENSSLDIR: "/var/ssl"
    engines:  dynamic

## Creazione di una coppia di chiavi privata/pubblica

Generare una coppia di chiavi di 2048 bits in formato PEM (non cifrato):

`$ openssl genrsa -out kes.pem 20148`

Generare una coppia di chiavi di 2048 bits in formato PEM (cifrato usando AES-256 CBC):

`$ openssl genrsa -aes256 -out keys.pem 20148`

viene richiesto di inserire una password per la cifratura del file.

Visualizzare opzioni riguardanti la generazione di chiavi private/pubbliche

`$ openssl genrsa -h`


## Esportare una chiave pubblica

`openssl rsa -in keys.pem -outform PEM -pubout -out public.pem`

Se il file in input `keys.pem` e' cifrato verra' richiesta la password di decifratura.

Se non viene specificato il parametro `-pubout` allora verra' esportata **in chiaro** la chiave privata:

`openssl rsa -in keys.pem -out private.pem -outform PEM`



## Generazione/Verifica firme digitali con algoritmi EC (Elliptic Curves)

Questa sezione descrive come generare e verificare una firma digitale utilizzando algoritmi EC.


### Generazione di una chiave privata

Generazione di una chiave privata **in chiaro** utilizzando una delle curve predefinite `prime256v1`:

`$ openssl ecparam -out key.pem -name prime256v1 -genkey`

E' possibile avere la lista delle curve predefinite attraverso questo comando:

`$ openssl ecparam -list_curves`


### Esportazione della chiave pubblica

Da un certifcato contentene una chiave privata e' possibile estrarre solo la chiave pubblica utilizzando il seguente comando:

`$ openssl ec -in key.pem -pubout -out pubkey.pem`


### Firma di un file tramite chiava privata

E' possibile generare la firma di un file di dati partendo da una chiave privata usando il seguente comando:

`$ openssl dgst -sha256 -sign key.pem -out tobesigned.sign tobesigned.txt`


### Verifica di una firma tramite chiave pubblica

E' possibile verificare la firma associata ad un file di dati utilizzando una chiave pubblica usando il seguente comando:

`$ openssl dgst -sha256 -verify pubkey.pem -signature tobesigned.sign tobesigned.txt`


## Generazione di numeri casuali

### Generare numeri casuali

E' possibile generare un numero casuale di un numero di cifre a scelta:

`$ openssl rand -hex 20`

    3c45bd4c3046df2f3bd1d143fae3b04006d31e90

### Risolvere l'errore "unable to write 'random state'"

Fare riferimento a:
* https://www.openssl.org/docs/faq.html#USER1
