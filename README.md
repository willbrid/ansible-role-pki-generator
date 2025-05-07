# Ansible-role-pki-generator

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-pki-generator/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-pki-generator/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-pki-generator/actions/workflows/ci.yml)

Le rôle **ansible-role-pki-generator** permet d’automatiser la génération des composants d'une infrastructure à clé publique (PKI). Il commence par créer une autorité de certification (CA) avec sa clé privée et son certificat. Ensuite, il génère un certificat serveur (ou client) avec sa clé privée, signé par cette autorité de certification. Ce rôle est compatible avec les environnements Linux et s'appuie sur **openssl**.

## Exigences

La collection **community.crypto** doit être préalablement installée via la commande **ansible-galaxy**.

```bash
ansible-galaxy collection install community.crypto
```

## Description des Variables

|Nom|Type|Description|Obligatoire|Valeur par défaut|
|---|----|-----------|-----------|-----------------|
`pkigen_output_path`|string|repertoire de génération des fichiers pki|oui|`""`
`pkigen_ca_common_name`|string|"common name" pour le certificat de l'autorité de certification|oui|`""`
`pkigen_ca_secret_passphrase`|string|Mot de passe de la clé privée de la CA|oui|`""`
`pkigen_cert_file_name`|string|nom sans extension du fichier du certificat signé et de sa clé|oui|`""`
`pkigen_cert_common_name`|string|"common name" du certificat signé|oui|`""`
`pkigen_cert_organization_name`|string|nom de l’organisation détenteur du certificat|oui|`""`
`pkigen_cert_organizational_unit_name`|string|unité organisationnelle du certificat|non|`""`
`pkigen_cert_email_address`|string|adresse email liée au certificat|non|`""`
`pkigen_cert_country_name`|string|code pays (ex: FR, US)|non|`""`
`pkigen_cert_valid_for`|string|durée de validité du certificat|non|`"+365d"`
`pkigen_cert_valid_since`|string|date de début de validité du certificat|non|`"-1d"`
`pkigen_cert_alt_names`|list|liste des noms alternatifs (SAN) pour le certificat (ex: ["willbrid.com", "*.willbrid.com"])|oui|`[]`

## Dépendances

- **community.crypto**

## Exemple Playbook

- Installation du rôle

```bash
mkdir -p $HOME/install-pki-generator/roles
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
cd $HOME/install-pki-generator && ansible-galaxy install -r requirements.yml --roles-path roles
```

- Utilisation du rôle dans un playbook

```bash
vim $HOME/install-pki-generator/playbook.yml
```

```yaml
---
- hosts: localhost

  vars:
    pkigen_output_path: "/tmp"
    pkigen_ca_common_name: "*.willbrid.com"
    pkigen_ca_secret_passphrase: "test@test"
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

## Licence

MIT

## Informations sur l'auteur

William Bridge NGASSAM
