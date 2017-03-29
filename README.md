# Generar Llaves (JKS) para el servidor WEB (https):

Para usar certificados (jks) distintos a los puestos en el repositorio (/src/main/resources/jks):

* 1) Crear llaves privadas usando openSSL

```
> openssl genrsa -out server-private.pem 2048

> openssl genrsa -out client-private.pem 2048
```

* 2) Crear self-signed X509 certificados

```
> openssl req -new -x509 -key server-private.pem -out server-certificate.pem -days 365 -subj "/C=Bo/ST=Santa Cruz/L=Bolivia/O=Red Hat/OU=Consultoria/CN=localhost"

> openssl req -new -x509 -key client-private.pem -out client-certificate.pem -days 365 -subj "/C=Bo/ST=Santa Cruz/L=Bolivia/O=Red Hat/OU=Consultoria/CN=localhost"
```

* 3) Crear JKS truststores

```
> keytool -importcert -trustcacerts -keystore  server_truststore.jks -storetype jks -storepass 123456 -file client-certificate.pem

> keytool -importcert -trustcacerts -keystore  client_truststore.jks -storetype jks -storepass 123456 -file server-certificate.pem
```

* 4) Crear PKCS12 keystores y importar los certificados

```
> openssl pkcs12 -export -inkey server-private.pem -in server-certificate.pem -out server.p12

> openssl pkcs12 -export -inkey client-private.pem -in client-certificate.pem -out client.p12
```

* 5) Convertir el PKCS12 keystores a Java keystores usando Java keytool

```
> keytool -importkeystore -srckeystore server.p12 -srcstoretype pkcs12 -destkeystore server.jks -deststoretype jks

> keytool -importkeystore -srckeystore client.p12 -srcstoretype pkcs12 -destkeystore client.jks -deststoretype jks
```

```java
/**
 * Clase que representa un servidor REST embebido localmente. (servicios REST
 * api)
 *
 * @author: Angel Céspedes Quiroz
 * @Skype: acq1305
 * @Linkedin: https://bo.linkedin.com/in/acq1305
 *
 */
 ```
