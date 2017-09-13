---
title: Setup SSL Certificate for Tomcat
date: 2017-02-19 09:52:00 Z
categories:
- server
layout: post
meta_keywords: ssl certificate, ssl tomcat
meta_description: Setup SSL to run HTTPS on Apache Tomcat.
comments: true
---

Today, I and my friend (mczal) try to setup SSL certificate for Apache Tomcat. We use a Comodo PositiveSSL as Certificate Authority (CA). To setup SSL in Tomcat we need to generate PKCS12 file.

These are the requirements to generate PKCS12 file.

- OpenSSL
- Private key - you generated from your server (*.key)
- SSL certificate file from CA (*.crt)
- Certificate bundle from CA (*.ca-bundle) (Might be different format for each CA)

Then open terminal and run openssl command.

**Generate p12 file**
```
openssl pkcs12 -export -chain \
    -inkey wilianto.com.key \
    -in wilianto.com.crt \
    -name "wilianto.com" \
    -CAfile wilianto.com.ca-bundle \
    -out wilianto.com.p12
```

Then you will need to enter your export password twice. You will need this password to use this in the future, so make sure you remember it.

If the process run success you will get **_wilianto.com.p12_** on your directory.

Unfortunatelly, when we try it, we got an error **_unable to write 'random state'_**. It is caused by **.rnd** file in our home directory is owned by root rather than my account. The quick fix we do is remove that folder by running this command.

**Remove .rnd file**

```
sudo rm ~/.rnd
```

Next, you need to add it to your **_application.properties_** file on Spring Boot application resources.

**application.properties**

```
server.port: 8443
server.ssl.key-store: wilianto.com.p12
server.ssl.key-store-password: exportpassword
server.ssl.keyStoreType: PKCS12
server.ssl.keyAlias: wilianto.com
```
**_server.ssl.keyAlias_** should be the same with -name param you give when generating PKCS12 file.

Now let's try to access your application https://yourapp.com:8443.

Reference:
- [https://www.jamf.com/jamf-nation/articles/138/using-openssl-to-create-a-certificate-keystore-for-tomcat](https://www.jamf.com/jamf-nation/articles/138/using-openssl-to-create-a-certificate-keystore-for-tomcat)
- [http://stackoverflow.com/a/94458/1285923](http://stackoverflow.com/a/94458/1285923)
- [https://drissamri.be/blog/java/enable-https-in-spring-boot/](https://drissamri.be/blog/java/enable-https-in-spring-boot/)