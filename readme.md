# Ansible playbook to setup a localhost in the 'cloud'

This playbook will setup a linode image to have everything it needs to run vs code server and lando in order to replicate a local Drupal developer environment.
All ports except 22 are closed and you need to use port forwarding to connect to the lando site or if you need to run vs code in the browser.
First you need to install the requirements using ansible galaxy:

```
ansible-galaxy install -r requirements.yml
```

Next, update the inventory file to use the ip of the server you are building:

```
vi inventory/hosts.ini
```

Next, you need to setup your secrets.yml by running:

```
ansible-vault create secrets.yml
```

You can look at the example-secrets.yml file in order to see the variables that go into this file:

```
---
new_user: username
upassword: password
terminus_email: me@email.com
terminus_api: 0000000x0d0sjek
github_token: ghp_lfekjflej
uid: 1
```

The variables above are as follows

- new_user: this is the username to be created in linux
- upassword: the new user's password
- terminus_email: email used with pantheon terminus
- terminus_api: [personal user token](https://dashboard.pantheon.io/personal-settings/machine-tokens)
- uid: your Drupal user id


Then you can run the playbook with the following command.

```
ansible-playbook --ask-vault-pass -i inventory/hosts.ini playbook.yml
```

Running a 'lando start' right after running this may run into issues, so a restart of docker seems to clear it up:

```
systemctl restart docker
```

Also, if you can't connect to the server after running this it seems easiest to just trash the image and start over again. Sometimes the firewall rules don't seem to apply correctly and everything gets locked out. All ports are closed except for 22 for ssh.
