---
layout: post
title: Setting Ansible untuk instance di google cloud engine
---

Menggunakan ansible untuk orchestration pada instance google cloud engine.
Artikel ini akan menggunakan fresh install ansible pada ubuntu.

## Installing ansible

Pakai pip lebih rekomenden gan.

```bash
python -m pip install ansible
```

## Create instance google cloud engine

![Create instance 1]({{ site.url }}/images/create_instance_1.png)
![Create instance 2]({{ site.url }}/images/create_instance_2.png)
![Create instance 3]({{ site.url }}/images/create_instance_3.png)

## Upload ssh-key

Pada mesin yang diinstall ansible kita harus meng-upload ssh-key public `.ssh/id_rsa.pub` ke google compute engine metadata

![Upload id rsa pub 1]({{ site.url }}/images/upload_id_rsa_1.png)
![Upload id rsa pub 2]({{ site.url }}/images/upload_id_rsa_2.png)
![Upload id rsa pub 3]({{ site.url }}/images/upload_id_rsa_3.png)

## Setting host ansible

Edit atau buat file di `/etc/ansible/hosts`, lalu masukan ip external instance.

```bash
faisal@faisal:~$ "146.148.35.125" >> /etc/ansible/hosts 
```

## Check koneksi

test koneksi ke instance dengan module ping di ansible.

```bash
faisal@faisal:~$ ansible all -m ping
The authenticity of host '146.148.35.125 (146.148.35.125)' can't be established.
ECDSA key fingerprint is SHA256:pvBVz0J2e9dpeMnQDMjRQlhpucbuNB1hhoQkMM27bsQ.
Are you sure you want to continue connecting (yes/no)? yes
146.148.35.125 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
faisal@faisal:~$ 
```

Jika response success maka ansible sudah terkoneksi dengan benar di google cloud engine
