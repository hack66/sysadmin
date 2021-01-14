# hack66 sysadmin repository

## Deploy ansible roles

### Mumble

`cd ansible && ansible-playbook deploy-mumble.yml`

### DokuWiki

`cd ansible && ansible-playbook deploy-dokuwiki.yml`

#### Dokuwiki restore

- https://www.dokuwiki.org/faq:servermove
- https://www.dokuwiki.org/install:upgrade

### Etherpad

`cd ansible && ansible-playbook deploy-etherpad.yml`

#### Etherpad restore DB

For restoring Etherpad data from a SQL DB dump, you can use docker exec command
with -i flag:

```
docker exec -i Container_Name \
sh -c 'exec mysql -uetherpad_user -D etherpad -p"Password"' < db.sql
```

### Discourse

`cd ansible && ansible-playbook deploy-discourse.yml`

## Adding SSH fingerprints to known hosts

1. Get an SSH fingerprint from a local `known_hosts` file for a given hostname
   and IP:

`ssh-keygen -q -f ~/.ssh/known_hosts -F hostname/IP -F $(dig +short A hostname)`

2. Upon verifying add the SSH fingeprints one per line (or seraparated by comma
   if is same host, see `ansible/ssh/known_hosts`).

## Ansible vault

We are using a multi-key encryption via GPG. The ansible-vault decryption is
handled automatically in Ansible with the use of `open_vault.sh` script which
decrypts the vault password and feeds it to the Ansible role.

In order to add/remove the recipients of the GPG encrypted vault file
`vault_pass.gpg` add/remove the `--recipient email` parameter with the
appropriate GPG emails.

You may use the following commands to re-encrypt the encrypted vault password
with the desired recipients GPG emails:

```
mv vault_pass.gpg vault_pass_old.gpg && gpg --batch --decrypt \
vault_pass_old.gpg | gpg --batch --encrypt --recipient \
prometheas@autistici.org --recipient andz@torproject.org \
--always-trust --verbose --output vault_pass.gpg && rm vault_pass_old.gpg
```
