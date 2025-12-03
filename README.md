# sscgen

Minimal GUI tool to generate self-signed certificates and keys for Trojan / 3x-ui / XRay / etc...

**Dependencies:** gtk2 (libgtk2.0-0), openssl

## Directory Structure

All files are created **next to the executable** inside the `certs` folder:
```
certs/
├─ private.key # private RSA 2048 key
├─ certificate.crt # self-signed certificate
├─ pem/
│ ├─ private.pem # copy of private key in PEM format
│ ├─ certificate.pem # copy of certificate in PEM format
│ └─ combined.pem # private key + certificate combined (PEM)
└─ p12/
└─ certificate.p12 # PKCS#12 container (.p12), empty password
```

---

## Certificate Generation

All actions are done automatically via the **Create** button in the GUI.  
OpenSSL must be installed on your system.

Commands executed internally by the program:

```
cd ./certs
# Generate key and certificate
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -keyout private.key -out certificate.crt -sha256 -subj "/CN=localhost"

# Copy to pem folder
cp -fv private.key pem/private.pem
cp -fv certificate.crt pem/certificate.pem

# Create combined PEM
cat private.key certificate.crt > pem/combined.pem

# Export PKCS#12 (.p12) with empty password
openssl pkcs12 -export -out p12/certificate.p12 \
  -inkey private.key -in certificate.crt -passout pass:
```
## Usage

+ **private.key / certificate.crt** — for 3x-ui, Trojan, XRay, and similar services.
+ **pem/combined.pem** — for OpenVPN or servers requiring a combined PEM.
+ **p12/certificate.p12** — for Windows, mobile clients, containers, CI/CD.
+ Copy keys and certificates to clipboard via GUI buttons.

## Notes
+ Keys are RSA 2048, self-signed.
+ No password is set on private key or .p12 to allow fully automated connections.
+ pem/ and p12/ directories are created automatically on first run.
