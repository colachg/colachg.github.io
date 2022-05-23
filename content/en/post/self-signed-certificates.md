---
title: "Self-signed Certificates"
date: 2021-11-27T19:20:30+08:00
draft: false
---
# Getting started with easy-rsa
1. init pki  
    ```shell
    ❯ easyrsa init-pki;
    init-pki complete; you may now create a CA or requests.
    Your newly created PKI dir is: /usr/local/etc/pki
    
    ❯ tree /usr/local/etc/pki;                      
    /usr/local/etc/pki
    ├── openssl-easyrsa.cnf
    ├── private
    ├── reqs
    └── safessl-easyrsa.cnf
    
    2 directories, 2 files
    ```

2. build ca
    ```shell
    ❯ easyrsa build-ca nopass # just for test purpose without pass, this will generate ca.crt and other files inside /usr/local/etc/pki.
    
    Common Name (eg: your user, host, or server name) [Easy-RSA CA]:colachg.com
    CA creation complete and you may now import and sign cert requests.
    Your new CA certificate file for publishing is at:
    /usr/local/etc/pki/ca.crt
    ```

3. generate certificate request
    ```shell
    ❯ easyrsa gen-req COLACHG nopass
    
    Common Name (eg: your user, host, or server name) [COLACHG]:
    
    Keypair and certificate request completed. Your files are:
    req: /usr/local/etc/pki/reqs/COLACHG.req
    key: /usr/local/etc/pki/private/COLACHG.key
    ```

4. import cert request
    ```shell
    ❯ easyrsa import-req /usr/local/etc/pki/reqs/COLACHG.req COLACHG.com
    Using SSL: /usr/local/opt/openssl@1.1/bin/openssl OpenSSL 1.1.1l  24 Aug 2021
    
    The request has been successfully imported with a short name of: COLACHG.com
    You may now use this name to perform signing operations on this request.
    ```
5. sign the request  
    ```shell
    ❯ easyrsa sign-req server COLACHG.com
    Using SSL: /usr/local/opt/openssl@1.1/bin/openssl OpenSSL 1.1.1l  24 Aug 2021
    
    
    You are about to sign the following certificate.
    Please check over the details shown below for accuracy. Note that this request
    has not been cryptographically verified. Please be sure it came from a trusted
    source or that you have verified the request checksum with the sender.
    
    Request subject, to be signed as a server certificate for 825 days:
    
    subject=
        commonName                = COLACHG
    
    
    Type the word 'yes' to continue, or any other input to abort.
      Confirm request details: yes
    Using configuration from /usr/local/etc/pki/easy-rsa-70819.Rt7VjG/tmp.uEA9j7
    Check that the request matches the signature
    Signature ok
    The Subject's Distinguished Name is as follows
    commonName            :ASN.1 12:'COLACHG'
    Certificate is to be certified until Mar  1 13:09:30 2024 GMT (825 days)
    
    Write out database with 1 new entries
    Data Base Updated
    
    Certificate created at: /usr/local/etc/pki/issued/COLACHG.com.crt
    ```

# Getting started  with openssl
1. Creating a Private Key
    ```shell
    openssl genrsa -des3 -out domain.key 2048 # If we want our private key unencrypted, we can simply remove the -des3 option from the command.
    ```
2. Creating a Certificate Signing Request(CSR)  
   ```shell
    openssl req -key domain.key -new -out domain.csr
    openssl req -newkey rsa:2048 -keyout domain.key -out domain.csr # create both the private key and CSR with a single command.
    openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr # add -nodes to make private key unencrypted.
    ```
3. Creating a Self-Signed Certificate  
    ```shell
    openssl req -newkey rsa:2048 -keyout domain.key -x509 -days 365 -out domain.crt
    ```
4. Creating a CA-Signed Certificate With Our Own CA
    ```shell
    openssl req -x509 -sha256 -days 1825 -newkey rsa:2048 -keyout rootCA.key -out rootCA.crt # generate root CA
    ```
    we'll create a configuration text-file (`domain.ext`) with the following content:  
   > authorityKeyIdentifier=keyid,issuer  
   basicConstraints=CA:FALSE  
   subjectAltName = @alt_names  
   [alt_names]  
   DNS.1 = domain  
   
    The “DNS.1” field should be the domain of our website.  
    Then, we can sign our CSR (domain.csr) with the root CA certificate and its private key:   
    ```shell
     openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in domain.csr -out domain.crt -days 365 -CAcreateserial -extfile domain.ext
    ```  
    As a result, the CA-signed certificate will be in the domain.crt file.

5. View Certificates  
    ```shell
    openssl x509 -text -noout -in domain.crt
    ```
   
# Reference:  
- https://easy-rsa.readthedocs.io/en/latest/
- https://www.baeldung.com/openssl-self-signed-cert