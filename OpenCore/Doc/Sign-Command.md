Method: Cloud Shell
///From https://github.com/dongcodebmt/VX5-591G-OpenCore

```sh
sudo apt-get install -y gnu-efi help2man sbsigntool uuid-runtime libfile-slurp-unicode-perl
```

```sh
mkdir sign && cd sign
```

```sh
git clone https://github.com/vathpela/efitools.git && cd efitools && make
```

```sh
cp cert-to-efi-sig-list sign-efi-sig-list KeyTool.efi .. && cd ..
```

```sh
openssl req -new -x509 -newkey rsa:4096 -sha256 -days 365 -subj "/CN=ASUSFX504 Platform Key" -keyout PK.key -out PK.pem
```

```sh
openssl req -new -x509 -newkey rsa:4096 -sha256 -days 365 -subj "/CN=ASUSFX504 Exchange Key" -keyout KEK.key -out KEK.pem
```

```sh
openssl req -new -x509 -newkey rsa:4096 -sha256 -days 365 -subj "/CN=ASUSFX504 Image Signing Key" -keyout ISK.key -out ISK.pem
```

```sh
./cert-to-efi-sig-list -g "$(uuidgen)" PK.pem PK.esl
./cert-to-efi-sig-list -g "$(uuidgen)" KEK.pem KEK.esl
./cert-to-efi-sig-list -g "$(uuidgen)" ISK.pem ISK.esl
```

```sh
wget https://www.dropbox.com/s/un9q4ryu9il0ynd/MicWinProPCA2011_2011-10-19.crt?dl=1 -O MicWinProPCA2011_2011-10-19.crt
wget https://www.dropbox.com/s/qwbi3gk7h9qc716/MicCorUEFCA2011_2011-06-27.crt?dl=1 -O MicCorUEFCA2011_2011-06-27.crt
```

```sh
openssl x509 -in MicWinProPCA2011_2011-10-19.crt -inform DER -out MsWin.pem -outform PEM
openssl x509 -in MicCorUEFCA2011_2011-06-27.crt -inform DER -out UEFI.pem -outform PEM
```

```sh
./cert-to-efi-sig-list -g "$(uuidgen)" MsWin.pem MsWin.esl
./cert-to-efi-sig-list -g "$(uuidgen)" UEFI.pem UEFI.esl
cat ISK.esl MsWin.esl UEFI.esl > db.esl
```

```sh
./sign-efi-sig-list -k PK.key -c PK.pem PK PK.esl PK.auth
./sign-efi-sig-list -k PK.key -c PK.pem KEK KEK.esl KEK.auth
./sign-efi-sig-list -k KEK.key -c KEK.pem db db.esl db.auth
```

```sh
mkdir OC
git clone https://github.com/pnapt/ASUS-FX504GD-Opencore.git && cd ASUS-FX504GD-Opencore
cp Drivers/* OpenCore.efi ../OC && cd ..
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem BOOTx64
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem AudioDxe.efi
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem HfsPlus.efi
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem OpenCanopy.efi
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem OpenCore.efi
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem OpenRuntime.efi
sbsign --key ~/sign/ISK.key --cert ~/sign/ISK.pem ResetNvramEntry.efi
rm -rf *.efi && cd .. && mv OC .. && cd .. &&  zip -r OC.zip OC
cloudshell download OC.zip


cd ~/sign/ASUS-FX504GD-Opencore/OpenCore/OC
ls
cd ..
cd ~
