# perfectrestore
MikroTik config restore helper script

This script is designed to restore plain-text, export files created using the following command.

```
/export file=myrtrbackup
```

## Features

* **Introduces a delay** – This allows time for the router’s interfaces to initialize before running the import script.
* **Audible prompts** – Before and after beep sequences to tell you if the import was successful.
* **Logging to disk** – you can inspect perfectrestore.log.0.txt to see if the import was successful. This is useful if your router does not have a builtin speaker (some models don’t).

## Try it out

Copy both `perfectrestore.rsc` and `backup.rsc` to your router and run the following command.

```
/import flash/perfectrestore
```
The script has a pre and post delay of 15 seconds. Be close to your router to here the confirmation beeps and/or inspect the log file `perfectrestore.log.0.txt`

## Documentation
[https://jcutrer.com/howto/networking/mikrotik/perfectrestore-script](https://jcutrer.com/howto/networking/mikrotik/perfectrestore-script)


## Additional notes
In case there are certificates present, which are not part of the backup script, to import them modify import script to load certificates before the other commands.

First export existing certificates using commands (use your own passphrase):

```
/certificate export-certificate ca-certificate export-passphrase=password123456
/certificate export-certificate server-certificate export-passphrase=password123456
/certificate export-certificate client1-certificate export-passphrase=password123456
```

Place exported .crt and .key filed in RouterOS root directory and add the following to top of the backup script to be imported before all of the other commands.
\
(also note that we do not wish to trust client certificates so set trusted=no for them)

```
:delay 15s
/certificate import file-name=cert_export_ca-certificate.crt name="ca-certificate" passphrase=""
/certificate import file-name=cert_export_ca-certificate.key name="ca-certificate" passphrase=password123456
/certificate import file-name=cert_export_server-certificate.crt name="server-certificate" passphrase=""
/certificate import file-name=cert_export_server-certificate.key name="server-certificate" passphrase=password123456
/certificate import file-name=cert_export_client1-certificate.crt name="client1-certificate" passphrase=""
/certificate import file-name=cert_export_client1-certificate.key name="client1-certificate" passphrase=password123456
/certificate set [find name=client1-certificate] trusted=no
```
