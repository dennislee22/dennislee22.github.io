---
layout: default
title: Vault Deployment
parent: "Openshift Deployment"
grand_parent: CDP Private Cloud
nav_order: 2
---

# Vault Deployment
{: .no_toc }

This article describes the steps to deploy the external Hashicorp Vault in the designated Vault server based on the guidelines published in this [link](https://learn.hashicorp.com/tutorials/vault/raft-deployment-guide?in=vault/raft).

---


1. Install the following software package.
    ```bash
    # yum install -y yum-utils
    ```

2. Add the repo.

    ```bash
    # yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
    ```

3. Install the Vault.

    ```bash
    # yum -y install vault
    ```

4. Create SSL certificate using the authorized (or self-signed) CA certificate. Example is shown below.

    ```bash
    # export DOMAIN=vaultnode.cdpkvm.cldr
    
    # openssl req -x509 \
            -sha256 -days 356 \
            -nodes \
            -newkey rsa:2048 \
            -subj "/CN=${DOMAIN}/C=SG/L=Spore" \
            -keyout hashirootCA.key -out hashirootCA.crt 

    # openssl genrsa -out ${DOMAIN}.key 2048
    # cat > csr.conf <<EOF
    [ req ]
    default_bits = 2048
    prompt = no
    default_md = sha256
    req_extensions = req_ext
    distinguished_name = dn

    [ dn ]
    C = SG
    ST = Spore
    L = City
    CN = ${DOMAIN}

    [ req_ext ]
    subjectAltName = @alt_names

    [ alt_names ]
    DNS.1 = ${DOMAIN}
    IP.1 = 127.0.0.1
    IP.2 = 10.15.4.168

    EOF 
    
    # openssl req -new -key ${DOMAIN}.key -out ${DOMAIN}.csr -config csr.conf

    ```
      

5. You should now have the following SSL certificates.

    ```bash
    # ls -l | grep crt
    -rw-r--r--  1 root root 1245 Jun 26 19:58 vaultnode.cdpkvm.cldr.crt
    -rw-r--r--  1 root root 1204 Jun 26 19:58 hashirootCA.crt
    ```

6. Copy the certificates to `/opt/vault/tls` directory and change the permission.

    ```bash
    # cp vaultnode.cdpkvm.cldr.crt /opt/vault/tls/
    # cp hashirootCA.crt /opt/vault/tls/
    # chown root:root /opt/vault/tls/vaultnode.cdpkvm.cldr.crt /opt/vault/tls/hashirootCA.crt
    # chmod 0644 /opt/vault/tls/vaultnode.cdpkvm.cldr.crt /opt/vault/tls/hashirootCA.crt
    # cp vaultnode.cdpkvm.cldr.key /opt/vault/tls/
    # chown root:vault /opt/vault/tls/vaultnode.cdpkvm.cldr.key
    # chmod 0640 /opt/vault/tls/vaultnode.cdpkvm.cldr.key
    ```
    
7. Backup the `/etc/vault.d/vault.hcl` file.

    ```bash
    # cp /etc/vault.d/vault.hcl /etc/vault.d/vault.hcl-orig
    ```

8. Edit the `/etc/vault.d/vault.hcl` file. In this demo, `10.15.4.188` is the IP address of the Vault node.

    ```bash
    ui = true
    cluster_addr  = "https://10.15.4.188:8201"
    api_addr      = "https://10.15.4.188:8200"
    #mlock = true
    disable_mlock = true

    storage "file" {
    path = "/opt/vault/data"
    }

    # HTTPS listener
    listener "tcp" {
    address       = "0.0.0.0:8200"
    tls_cert_file = "/opt/vault/tls/vaultnode.cdpkvm.cldr.crt"
    tls_key_file  = "/opt/vault/tls/vaultnode.cdpkvm.cldr.key"
    tls_client_ca_file = "/opt/vault/tls/hashirootCA.crt"
    }
    ```

9. Enable and start the Vault service.

    ```bash
    # systemctl enable vault.service
    # systemctl start vault.service
    ```
10. Check that port 8200 is up and running.

    ```bash
    # netstat -an | grep 8200
    tcp        0      0 0.0.0.0:8200            0.0.0.0:*               LISTEN   
    ```
11. Check the status of the Vault. You might encounter the following errors and if yes, proceed with the following procedures to fix the errors.

    ```bash
    # vault status
    Error checking seal status: Get "https://127.0.0.1:8200/v1/sys/seal-status": x509: cannot validate certificate for 127.0.0.1 because it doesn't contain any IP SANs
    
    # export VAULT_ADDR=https://vaultnode.cdpkvm.cldr:8200
    
    # vault status
    Error checking seal status: Get "https://vaultnode.cdpkvm.cldr:8200/v1/sys/seal-status": x509: certificate signed by unknown authority
    
    # cp hashirootCA.crt /etc/pki/ca-trust/source/anchors/
    # update-ca-trust extract
    
    # vault status
    Key                Value
    ---                -----
    Seal Type          shamir
    Initialized        false
    Sealed             true
    Total Shares       0
    Threshold          0
    Unseal Progress    0/0
    Unseal Nonce       n/a
    Version            1.11.0
    Build Date         2022-06-17T15:48:44Z
    Storage Type       file
    HA Enabled         false
    
    ```

12. Initialize the Vault operator. Jot down the root token and its associated `Unseal key` to unseal the Vault when needed.

    ```bash
    # vault operator init
    ```

13. You may now log in to the SSL-enabled Vault portal `https://vaultnode.cdpkvm.cldr:8200`. 

    ![](../../assets/images/ocp4/vault1.png)
    
    ![](../../assets/images/ocp4/vault2.png)  

    