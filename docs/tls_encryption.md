# Encrypted connections

The application supports encrypted connections to brokers which support TLS (SSL or WSS).  
The aim of this page is to clarify what you need to provide to the app, the type of expected input files and the available options. The discussion below further expands what stated in the [brokers](brokers.md) section.

:   !!! tip "In short"
        The app requires a CA certificate following the X509 standard, and a Client certificate in PKCS12. If you instead have a Client certificate and a Client private key  in PEM format, you can build your PKCS12 from them.

## Encrypted, unauthenticated connections

To establish a secure SSL connection not requiring authentication, follow these steps:

- Set `ssl://` or `wss://` as your address' protocol (depending on whether you need secure TCP or websocket).
- Enable *Use SSL connection* in the dedicated dialog.
- Depending on the CA certificate source, you may need to perform one or more of the following steps:

    - Add your CA cert file as part of the Android Trust chain, via the dedicated menu in Settings > Security > Certificates.
    - Provide your CA cert to the app for it to directly verify the server; this step is alternative to the previous. The 
    - Select *Accept self-signed certificates*. This option instructs the connection manager of the app to **not** verify the server. This may be dangerous in case of a man-in-the-middle attack. Use it at your own risk.

That's it. Now try to connect.

### Example: Flespi
[Flespi](flespi.io) provides an online MQTT broker free of charges with TLS encryption.

To test it out, register and create a new account. Once you have your broker address, create a new broker instance in the app and paste it there.

- Be sure to set `ssl://` as the protocol (or `wss://` if you want a websocket).
- Use the correct port, i.e. `8883`, or `443` for the websocket.
- Check *Use SSL connection* as enable in the related dialog.

That's it. You **do not** need to upload Flespi's CA certificate since it is already recognized by the Android built-in trust chain.

## Encrypted, authenticated connections

To establish a secure SSL connection not requiring authentication, follow these steps:

- Follow the steps outlined by the *Encrypted, unauthenticated connections* section.
- If authentication takes place via a username/password combination, provide them as part of the broker setup.
- If instead authentication occurs via Client certificate, proceed as follows depending on your certificate type:

    - PKCS12 Client certificate: select your file from the dedicated dialog; if protected by a password, provide the password as well in *Certificate password*.
    - PEM Client certificate: PEM client certificates are **not** directly supported. You will need to convert them to the PKCS12 format. But worry not, this is not complicated. Read the section below for a step-by-step guide.

### Generating a PKCS12 certificate

#### Using OpenSSL locally

To convert your PEM Client certificate/key pair to PKCS12 use `openssl` from command line; chances are you already have it installed in your system. To perform the conversion, type
``` bash
openssl pkcs12 -export -in cert.pem -inkey key.pem -certfile cacert.pem -out cert.p12 
```
and, when prompted to, provide a password, or just press enter to skip.

The input-output files are:

- `cert.pem`: your PEM-format Client certificate
- `key.pem`: your PEM-format Client private key.
- `cacert.pem`: the Certification Authority PEM certificate, the same you provide to the app under the name of *CA cert*.
- `cert.p12`: the newly generated PKCS12 certificate to be copied to the smartphone and used in the app.

#### Using an Online service

There may be online services offering certificate type conversions. If you choose to go this way, be sure to trust the website, as you would be uploading sensitive information.

## Example: Amazon AWS

The app does support connecting to AWS. Below we will give a brief outline of the steps you need to perform to get MqttDashboard to successfully connect to AWS.

- In the AWS IoT Core console, look for your endpoint name, in the form of `<identifier>.iot.<region>.amazonaws.com` and note it somewhere.
- Now, under the *Security* menu, generate a new certificate with *one click* option. Download the following files:

    - `xxx.cert.pem`
    - `xxx.private.key`
    - `AmazonCA.pem`

- Convert the `xxx.cert.pem` and `xxx.private.key` into a `client.p12` following the steps described above.

- Upload the two files (`AmazonCA.pem` and `client.p12`) to your smartphone.
- In the same Security screen, select your certificate and activate it. Then, link it to a valid *policy*; if you do not have any, create one. A good starting point is:  
``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Publish",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": "*"
    }
  ]
}
```
- Open MqttDashboard on your phone. Create a new broker with settings:
    
    - *Address*: `ssl://<identifier>.iot.<region>.amazonaws.com`.
    - *Port*: 8883.
    - *Use SSL connection*: true.

:   !!! Attention
        Only use `8883` as port. `443` requires additional configuration that this app does not perform.

- Under the SSL settings:
    
    - *CA cert*: pick `AmazonCA.pem`.
    - *Client cert*: pick `client.p12`.
    - *Client password*: type in the password if you set one.

Done. Enjoy the app!