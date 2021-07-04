
## 🧷 Reverse & Build APK

![JRE](https://img.shields.io/badge/JRE-java%20runtime%201.8-blue.svg)
![apktool.jar](https://img.shields.io/badge/apktool.jar-2.4.1-green.svg)
![signapk.jar](https://img.shields.io/badge/signapk.jar-anrdroid%20toolkit-brightgreen.svg)
![openssl](https://img.shields.io/badge/openssl-optional-yellow.svg)

Reverse an APK file to modify some built-in configs and functionalities, and repack the contents back to the managed format. In order to make the repacked APK installable, you'll also need to attach a signiture on the file.


### 📝 Step-By-Step Commends

Download the files provided in this repository. Make sure you have your own JRE8 installed if the OS is NOT windows x64. 

Use your own key files, or some usable samples can be found in the [release page](https://github.com/der3318/repackapk-readme/releases/tag/2021.07.04).

Open the cmd prompt and navigate to the folder where the files locates:

```shell
# decode the original APK file
> Runtime\bin\java.exe -Duser.language=en -Dfile.encoding=UTF8 -jar apktool.jar d [APK-FILE](.apk) -o DecodedFiles

# modify the source files in DecodedFiles/ to customize your need

# rebuild the APK after things are done
> Runtime\bin\java.exe -Duser.language=en -Dfile.encoding=UTF8 -jar apktool.jar b DecodedFiles

# sign the rebuilt APK with key, and output as "RepackedAndSigned.apk"
> Runtime\bin\java.exe -Duser.language=en -Dfile.encoding=UTF8 -jar signapk.jar [PUBLIC-KEY](.x509.pem) [PRIVATE-KEY](.pk8) DecodedFiles/dist/[APK-FILE](.apk) RebuiltAndSigned.apk
```


### 💡 How to Get .x509.pem & .pk8 From .pfx

PFX file is a single, password protected certificate archive that contains the entire certificate chain plus the matching private key.

Install [openssl](https://github.com/openssl/openssl#build-and-install) and run the following steps to convert:

```shell
# generate "DerChien.pem" from "DerChien.pfx"
> openssl pkcs12 -in DerChien.pfx -out DerChien.pem

# retrieve required info from .pem file and save as "DerChien.x509.pem"
copy "CERIFICATE" section (included) into DerChien.x509.pem

# retrieve required info from .pem file and save as "DerChien.rsa.pem"
copy "PRIVATE KEY" section (included) into DerChien.rsa.pem

# convert "DerChien.rsa.pem" to "DerChien.pk8"
> openssl pkcs8 -topk8 -outform DER -in DerChien.rsa.pem -inform PEM -out DerChien.pk8 -nocrypt
```

![pem.png](/pem.png)


### References
- https://ibotpeaches.github.io/Apktool/install/
- https://stackoverflow.com/questions/4217107/how-to-convert-pfx-file-to-keystore-with-private-key
- https://www.javatt.com/p/79533
- https://www.twblogs.net/a/5c7c0f49bd9eee31cea5f78a


