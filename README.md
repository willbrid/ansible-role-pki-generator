# Ansible-role-pki-generator

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-pki-generator/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-pki-generator/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-pki-generator/actions/workflows/ci.yml)

The **ansible-role-pki-generator** role automates the generation of public key infrastructure (PKI) components. It begins by creating a certificate authority (CA) with its private key and certificate. Then, it generates a server (or client) certificate with its private key, signed by that certificate authority. This role is compatible with Linux environments and relies on **openssl**.

## Requirements

The **community.crypto** collection must be installed beforehand using the **ansible-galaxy** command.

```bash
ansible-galaxy collection install community.crypto
```

## Description of Variables

|Name|Type|Description|Mandatory|Default value|
|----|----|-----------|---------|-------------|
`pkigen_output_path`|str|pki file generation directory|yes|`""`
`pkigen_ca_common_name`|str|"common name" for the certificate of the certification authority|yes|`""`
`pkigen_ca_secret_passphrase`|str|CA private key password|yes|`""`
`pkigen_cert_file_name`|str|filename (without extension) of the signed certificate and its key|`""`
`pkigen_cert_common_name`|str|"common name" of the signed certificate|yes|`""`
`pkigen_cert_organization_name`|str|name of the organization holding the certificate|yes|`""`
`pkigen_cert_organizational_unit_name`|str|organizational unit of the certificate|yes|`""`
`pkigen_cert_email_address`|str|email address linked to the certificate|no|`""`
`pkigen_cert_country_name`|str|country code (e.g., FR, US)|non|`""`
`pkigen_cert_valid_for`|str|certificate validity period|no|`"+365d"`
`pkigen_cert_valid_since`|str|certificate validity start date|no|`"-1d"`
`pkigen_cert_alt_names`|list|list of alternative names (SANs) for the certificate (e.g., ["willbrid.com", "*.willbrid.com"])|yes|`[]`

## Dependencies

- **community.crypto**

## Example Playbook

- Role installation

```bash
mkdir -p $HOME/install-pki-generator
```

```bash
vim $HOME/install-pki-generator/requirements.yml
```

```yaml
- name: ansible-role-pki-generator
  src: git+https://github.com/willbrid/ansible-role-pki-generator.git
  version: v0.0.1
```

```bash
cd $HOME/install-pki-generator && ansible-galaxy install --force -r requirements.yml
```

- Using the role in a playbook

```bash
vim $HOME/install-pki-generator/playbook.yml
```

```yaml
---
- hosts: localhost

  vars:
    pkigen_output_path: "/tmp"
    pkigen_ca_common_name: "*.willbrid.com"
    pkigen_ca_secret_passphrase: "test@test" # Change this password: do not use the one provided as an example.
    pkigen_cert_file_name: "willbrid.com"
    pkigen_cert_common_name: "*.willbrid.com"
    pkigen_cert_organization_name: "willbrid"
    pkigen_cert_organizational_unit_name: "willbrid"
    pkigen_cert_email_address: "admin@willbrid.com"
    pkigen_cert_country_name: "FR"
    pkigen_cert_valid_for: "+365d"
    pkigen_cert_valid_since: "-1d"
    pkigen_cert_alt_names:
      - "willbrid.com"
      - "*.willbrid.com"

  roles:
    - ansible-role-pki-generator
```

```bash
cd $HOME/install-pki-generator && ansible-playbook playbook.yml
```

## License

MIT

## Author Information

William Bridge NGASSAM
