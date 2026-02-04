# General Information
The Java Card IsoApplet (e.g. for use with OpenSC).
The Applet is capable of saving a PKCS#15 file structure and performing PKI related operations using the private key, such as signing or decrypting.
Private keys can be generated directly on the smart card or imported from the host computer.
The import of private keys is disabled in the default security configuration.
The applet targets modern smartcards with Java Card 3.0.4 or above.

# IsoApplet Version and Smartcard requirements
IsoApplet is maintained in two different versions: one for newer smartcards and a legacy version for older smartcards.
If your smartcard supports the newer version of IsoApplet, you should prefer it.
For both versions, the support of the "requestObjectDeletion()"-mechanism of the Java Card API is recommended to be able to properly delete files.
Also, the javacardx.crypto.Cipher-package needs to be supported by your smart card.
This is very common among Java Card smartcards.

## New version of IsoApplet (v1)
This version is found on the [main branch](https://github.com/philipWendland/IsoApplet/tree/main).
It targets smartcards with Java Card version >= 3.0.4.
This version requires extended APDUs the be used and supported by your reader and smartcard (javacardx.apdu.ExtendedLength).
If supported by your smart card, the newer version of IsoApplet supports the following additional features:
* RSA keys of 4096 bit length
* RSA PSS signatures
* ECDSA with off-card hashing, which makes ECC actually usable in practice

## Legacy Version of IsoApplet (v0)
The legacy version is found on the [main-javacard-v2.2.2 branch](https://github.com/philipWendland/IsoApplet/tree/main-javacard-v2.2.2) branch.
It targets smartcards with Java Card version >= 2.2.2.
The ECDSA implementation with Java Card version 2.2.2 is hardly usable in practice because it requires on-card hash generation.
If your smartcard implements javacardx.apdu.ExtendedLength and IsoApplet is configured with `DEF_EXT_APDU` in `IsoApplet.java`, you can use extended APDUs.

## Version for NXP JCOP 2.4.1 R3 cards (v1.01 in this fork)

Can be found in the [jcop_241_card](https://github.com/czietz/IsoApplet/tree/jcop_241_card) branch.

NXP JCOP 2.4.1 R3 Java Cards – while based on Java Card 2.2.2 – offer a proprietary API that allows _off-card_  hash generation for ECDSA, enabling algorithms such as `ecdsa-sha2-nistp256`, used by SSH. This has been tested on a NXP J3A081 card, using OpenSC: `pkcs11-tool.exe --login --keypairgen --key-type ec:secp256r1 --usage-sign`. Note that (at least) the NXP J3A081 card supports a maximum EC key bit length of 320 bits, so the `nistp384` and `nistp521` curves cannot be used.

Sadly, the required NXP library _jcopx-2.4.1.R3.jar_ cannot be distributed freely. But maybe you will manage to find it. 😉

# Build process
This project uses [ant-javacard](https://github.com/martinpaljak/ant-javacard) to build cap-files.
After cloning the IsoApplet repository, all you have to do is:
* Perform `git submodule init && git submodule update` to retrieve the Java Card SDKs, in case you did not `git clone --recursive` to clone this repository.
* Install Apache `ant`, `openjdk-17-jdk-headless`
* If needed, configure the correct Java SDK version or set the JAVA_HOME environment variable.
* Invoke `ant` to produce the cap file.
If you have build errors on an existing repository, try deleting the ant-javacard.jar file so that the newest version is downloaded.

# Installation
Install the CAP-file (IsoApplet.cap) to your Java Card smart card (e.g. with [GlobalPlatformPro](https://github.com/martinpaljak/GlobalPlatformPro)).

**Have a look at the wiki for more information:** https://github.com/philipWendland/IsoApplet/wiki

