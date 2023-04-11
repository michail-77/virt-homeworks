
# Домашнее задание к занятию 2. «Применение принципов IaaC в работе с виртуальными машинами»

## Как сдавать задания

Обязательны к выполнению задачи без звёздочки. Их нужно выполнить, чтобы получить зачёт и диплом о профессиональной переподготовке.

Задачи со звёздочкой (*) — дополнительные задачи и/или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---


## Важно

Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.

Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

---

## Задача 1

- Опишите основные преимущества применения на практике IaaC-паттернов.

  Непрерывная интеграция (CI) — практика разработки ПО, которая заключается в постоянном слиянии рабочих веток в общую основную ветку разработки и выполнении частых автоматизированных сборок проекта.  
  Непрерывная доставка (CD) — CI + CD Позволяет выпускать изменения небольшими партиями, которые легко изменить или устранить путём отката на предыдущую версию и последующего перезапуска процесса сборки с учётом исправления выявленных дефектов.  
  Непрерывное развёртывание (CD) — CI + CD + СD Практика непрерывного развёртывания упраздняет ручные действия, не требуя непосредственного утверждения со стороны разработчика или любого другого ответственного лица.  
  Весь процесс развертывания может  быть автоматизирован и быть запущен любым пользователем в команде DevOps. А поскольку процесс развертывания автоматизирован, то сокращается время и усилия, необходимые на создания инфраструктуры  для разработки и тестирования.  


- Какой из принципов IaaC является основополагающим?

  Основополагающим принципом IaaC является идемпотентность - операция, которая при многократном вызове возвращает один и тот же результат и что все взаимодействия с инфраструктурой осуществляются через конфигурационные файлы, что в свою очередь несет минимизацию ошибок со стороны настройки системы пользователем.  

## Задача 2

- Чем Ansible выгодно отличается от других систем управление конфигурациями?

  Ansible - это гибкий инструмент с открытым исходным кодом, который может значительно упростить методы управления конфигурацией и управления процессами при эксплуатации и обслуживании, а также метод push, который не требует установки агентов для настройки клиентских систем, так что вся работа может выполняться на стороне главного сервера.  

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный — push или pull?

  Думаю, что  push, т.к. администратор  вручную или скриптом запускает процесс применения изменений на сервере (локально или удаленно).  


## Задача 3

Установите на личный компьютер:

- [VirtualBox](https://www.virtualbox.org/),
- [Vagrant](https://github.com/netology-code/devops-materials),
- [Terraform](https://github.com/netology-code/devops-materials/blob/master/README.md),
- Ansible.

*Приложите вывод команд установленных версий каждой из программ, оформленный в Markdown.*

[root@localhost src]# vboxmanage --version  
7.0.6r155176  
[root@localhost src]# vagrant --version  
Vagrant 2.3.4  
[root@localhost src]# ansible --version  
ansible 2.9.27  
config file = /etc/ansible/ansible.cfg  
configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']  
ansible python module location = /usr/lib/python2.7/site-packages/ansible  
executable location = /bin/ansible  
python version = 2.7.5 (default, Jun 28 2022, 15:30:04) [GCC 4.8.5 20150623 (Red Hat IIA4.8.5-44)]  
[root@localhost src]# terraform -v  
Terraform v0.12.26  

Your version of Terraform is out of date! The latest version
is 1.4.4. You can update by downloading from https://www.terraform.io/downloads.html  
[root@localhost src]#  


## Задача 4 

Воспроизведите практическую часть лекции самостоятельно.

- Создайте виртуальную машину.

[root@localhost vagrant]# vagrant up  
Bringing machine 'server1.netology' up with 'virtualbox' provider...  
==> server1.netology: Importing base box 'bento/ubuntu-20.04'...  
==> server1.netology: Matching MAC address for NAT networking...  
==> server1.netology: Setting the name of the VM: server1.netology  
==> server1.netology: Clearing any previously set network interfaces...  
==> server1.netology: Preparing network interfaces based on configuration...  
    server1.netology: Adapter 1: nat  
    server1.netology: Adapter 2: hostonly  
==> server1.netology: Forwarding ports...  
    server1.netology: 22 (guest) => 20011 (host) (adapter 1)  
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)  
==> server1.netology: Running 'pre-boot' VM customizations...  
==> server1.netology: Booting VM...  
==> server1.netology: Waiting for machine to boot. This may take a few minutes...  
    server1.netology: SSH address: 127.0.0.1:2222  
    server1.netology: SSH username: vagrant  
    server1.netology: SSH auth method: private key  
    server1.netology:   
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace  
    server1.netology: this with a newly generated keypair for better security.  
    server1.netology:   
    server1.netology: Inserting generated public key within guest...  
    server1.netology: Removing insecure key from the guest if it's present...  
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...  
==> server1.netology: Machine booted and ready!  
==> server1.netology: Checking for guest additions in VM...  
==> server1.netology: Setting hostname...  
==> server1.netology: Configuring and enabling network interfaces...  
==> server1.netology: Mounting shared folders...  
    server1.netology: /vagrant => /root/iaac/vagrant  
==> server1.netology: Running provisioner: ansible...  
    server1.netology: Running ansible-playbook...  
  
PLAY [nodes] *******************************************************************  
  
TASK [Gathering Facts] *********************************************************  
ok: [server1.netology]  
  
TASK [Create directory for ssh-keys] *******************************************  
ok: [server1.netology]  
  
TASK [Adding rsa-key in /root/.ssh/authorized_keys] ****************************  
An exception occurred during task execution. To see the full traceback, use -vvv. The error was: If you are using a module and expect the file to exist on the remote, see the remote_src option                                                                                         
fatal: [server1.netology]: FAILED! => {"changed": false, "msg": "Could not find or access '~/.ssh/id_rsa.pub' on the Ansible Controller.\nIf you are using a module and expect the file to exist on the remote, see the remote_src option"}                                             
...ignoring  
  
TASK [Checking DNS] ************************************************************  
changed: [server1.netology]  
  
TASK [Installing tools] ********************************************************  
ok: [server1.netology] => (item=[u'git', u'curl'])  
  
TASK [Installing docker] *******************************************************  
changed: [server1.netology]  
  
TASK [Add the current user to docker group] ************************************  
changed: [server1.netology]  
  
PLAY RECAP *********************************************************************  
server1.netology           : ok=7    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1     
  

- Зайдите внутрь ВМ, убедитесь, что Docker установлен с помощью команды
```
docker ps,
```

[root@localhost vagrant]# vagrant ssh  
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-144-generic x86_64)  
  
 * Documentation:  https://help.ubuntu.com  
 * Management:     https://landscape.canonical.com  
 * Support:        https://ubuntu.com/advantage  
  
  System information as of Tue 11 Apr 2023 02:43:48 PM UTC  
  
  System load:  0.15               Users logged in:          0  
  Usage of /:   13.4% of 30.34GB   IPv4 address for docker0: 172.17.0.1  
  Memory usage: 25%                IPv4 address for eth0:    10.0.2.15  
  Swap usage:   0%                 IPv4 address for eth1:    192.168.56.11  
  Processes:    152  
  
  
This system is built by the Bento project by Chef Software  
More information can be found at https://github.com/chef/bento  
Last login: Tue Apr 11 14:41:35 2023 from 10.0.2.2  

vagrant@server1:\~$ docker ps  
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  
vagrant@server1:~$  
  
  
Vagrantfile из лекции и код ansible находятся в [папке](https://github.com/netology-code/virt-homeworks/tree/virt-11/05-virt-02-iaac/src).

Примечание. Если Vagrant выдаёт ошибку:
```
URL: ["https://vagrantcloud.com/bento/ubuntu-20.04"]     
Error: The requested URL returned error: 404:
```

выполните следующие действия:

1. Скачайте с [сайта](https://app.vagrantup.com/bento/boxes/ubuntu-20.04) файл-образ "bento/ubuntu-20.04".
2. Добавьте его в список образов Vagrant: "vagrant box add bento/ubuntu-20.04 <путь к файлу>".

