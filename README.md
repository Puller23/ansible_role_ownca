Ansible Role: ownca
=========

[![CI](https://github.com/Puller23/ansible_role_clamav/actions/workflows/ci.yml/badge.svg)](https://github.com/Puller23/ansible_role_clamav/actions/workflows/ci.yml)

This role creates certificate from an existing selfsigned authority and deploy this certificates to the Server.
The certificates will be generated on the destination hosts, signed on the Ansile host, and then uploaded again to the destination hosts.

Requirements
------------

You need to create manually a certificate authority that will be used to sign the certificates of all hosts. 

    $ openssl genrsa -aes256 -out ca-key.pem 2048
    $ openssl req -x509 -new -nodes -extensions v3_ca -key ca-key.pem -days 1095 -out ca-root.pem -sha512

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ownca_root_ca_folder: "/home/myuser/myownca"

The local folder  where the ownca file are located.

    ownca_root_ca_passphrase: ""

The ownca passphrase to create the certificates.

    ownca_dest_ssl_folder: ""

The directory on the remote server where the certificates should be copied to.

    ownca_subject_alt_name:"DNS:my.example.com:DNS:localhost,IP:127.0.0.1"

The subject alternative name can be defined. For

Dependencies
------------

The below requirements are needed on the hosts to use the Ansible modules (openssl_privatekey, openssl_csr, openssl_certificate and openssl_pkcs12)

    Either cryptography >= 1.2.3 (older versions might work as well)
    Or pyOpenSSL

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - name: Deploy my own ca
      hosts: all
      become: true
      vars:
        ownca_root_ca_folder: "/home/myuser/myownca"
        ownca_subject_alt_name: "DNS:MyServer.local,DNS:localhost,IP:172.16.1.41,IP:127.0.0.1"
        ownca_root_ca_passphrase: "mypassphrase"
        ownca_dest_ssl_folder: "/etc/ssl/internal_ca/"
        ownca_root_ca_key: |
          -----BEGIN ENCRYPTED PRIVATE KEY-----
          MIIFHD.....MlBA==
          -----END ENCRYPTED PRIVATE KEY-----

        ownca_root_ca_certificate: |
          -----BEGIN CERTIFICATE-----
          MIIDl.......TTm9nDF
          -----END CERTIFICATE-----

License
-------

MIT

Author Information
------------------

This role was created in 2021 by Gregor Bartels.
