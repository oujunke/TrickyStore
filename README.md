# To pass Google Play Integrity checks, follow these steps:
## 1. Install [Magisk](https://github.com/topjohnwu/MagiskManager) or [KernelSU](https://github.com/tiann/KernelSU) or [KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next) and enable Zygisk (‌Required‌)
## 2. Download the [‌PlayIntegrityFixFork‌](https://github.com/oujunke/PlayIntegrityFixFork) module (‌Required‌) (Device fingerprint spoofing)
## 3. Download the [‌playcurlNEXT‌](https://github.com/oujunke/playcurlNEXT) module (Optional) (Automated fingerprint updates)
## 4. Download the [‌TrickyStore‌](https://github.com/oujunke/TrickyStore) module (‌Required‌) (Device certificate configuration)
## 5. Prepare a valid ‌KeyBox.xml‌ file (‌Required‌) (Device certificate spoofing)
### (Note: Steps marked as "Required" are essential for bypassing integrity checks.)

## 1. ‌Key Terms Clarification:‌

## 2. ‌Zygisk‌: Magisk's Zygote injection mechanism for system-level modifications.
## 3. ‌Fingerprint Spoofing‌: Mimicking a certified device's hardware/software profile.
## 4. ‌KeyBox.xml‌: Contains cryptographic keys for device attestation.
# Tricky Store (Fork)

A trick of keystore. **Android 10 or above is required**.

## Feature

- FOSS

## Usage

1. Flash this module and reboot.  
2. For more than DEVICE integrity, put an unrevoked hardware keybox.xml at `/data/adb/tricky_store/keybox.xml` (Optional).  
3. Customize target packages at `/data/adb/tricky_store/target.txt` (Optional).  
4. Enjoy!  

**All configuration files will take effect immediately.**

## keybox.xml

format:

```xml
<?xml version="1.0"?>
<AndroidAttestation>
    <NumberOfKeyboxes>1</NumberOfKeyboxes>
    <Keybox DeviceID="...">
        <Key algorithm="ecdsa|rsa">
            <PrivateKey format="pem">
-----BEGIN EC PRIVATE KEY-----
...
-----END EC PRIVATE KEY-----
            </PrivateKey>
            <CertificateChain>
                <NumberOfCertificates>...</NumberOfCertificates>
                    <Certificate format="pem">
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
                    </Certificate>
                ... more certificates
            </CertificateChain>
        </Key>...
    </Keybox>
</AndroidAttestation>
```

## Support TEE broken devices

Tricky Store will hack the leaf certificate by default. On TEE broken devices, this will not work because we can't retrieve the leaf certificate from TEE. You can add a `!` after a package name to enable generate certificate support for this package.

For example:

```
# target.txt
# use leaf certificate hacking mode for KeyAttestation App
io.github.vvb2060.keyattestation
# use certificate generating mode for gms
com.google.android.gms!
```

## Customize security patch level 

Edit the file `/data/adb/tricky_store/devconfig.toml`.

For example:

```
securityPatch = "2024-04-05"
osVersion = 34
```

## TODO

- Support automatic selection mode.

PR is welcomed.

## Acknowledgement

- [PlayIntegrityFix](https://github.com/chiteroman/PlayIntegrityFix)
- [FrameworkPatch](https://github.com/chiteroman/FrameworkPatch)
- [BootloaderSpoofer](https://github.com/chiteroman/BootloaderSpoofer)
- [KeystoreInjection](https://github.com/aviraxp/Zygisk-KeystoreInjection)
- [LSPosed](https://github.com/LSPosed/LSPosed)
