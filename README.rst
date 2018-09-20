Ansible Exercices Over Vagrant
===============================

This is a sample project that shows how to create tree VMs with Vagrant and configurate with Ansible.

Requeriments:
-------------------

- Vagrant https://www.vagrantup.com/downloads.html
- Ansible https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

Project tree:
-------------

::

    $ tree
    .
    ├── README.rst
    ├── Vagrantfile
    └── playbooks
        ├── ansible.cfg
        ├── demo.out
        └── hosts

    1 directory, 5 files

Create VMs with Vagrant based on VagrantFile
--------------------------------------------
::

    $ vagrant up
    
    #Enter to VM
    $ vagrant ssh vagrant1


Configure Ansible with Vagrant:
-------------------------------

::

    # Export ANSIBLE_CONFIG 
    $ export ANSIBLE_CONFIG=./playbooks/ansible.cfg

    #List all hosts
    $ ansible --list-hosts all
    hosts (3):
      vagrant2
      vagrant3
      vagrant1

    #List hosts by role.
    $ ansible --list-hosts webservers
    hosts (1):
      vagrant1

    $ ansible --list-hosts appservers
    hosts (1):
      vagrant2

    $ ansible --list-hosts dbservers
    hosts (1):
      vagrant3


Excersice 1 - Copy file in all VMs:
-----------------------------------

::

    $ ansible all -m copy -a "src=./playbooks/demo.out dest=/tmp/demo.out"
    
    #View file on VM.
    $ vagrant ssh vagrant1
    vagrant@vagrant-ubuntu-trusty-64:~$ cat /tmp/demo.out
    DOU Ansible Demo
    $ exit
    $ vagrant ssh vagrant2
    vagrant@vagrant-ubuntu-trusty-64:~$ cat /tmp/demo.out
    DOU Ansible Demo
    $ exit
    $ vagrant ssh vagrant3
    vagrant@vagrant-ubuntu-trusty-64:~$ cat /tmp/demo.out
    DOU Ansible Demo
    $ exit


Excersice 2 - Provision Nginx on webservers
-------------------------------------------

::

    # Install nginx with apt module (Bad state value)
    $ ansible webservers -m apt -a "name=nginx state=presetn"
    vagrant1 | FAILED! => {
        "changed": false,
        "msg": "value of state must be one of: absent, build-dep, installed, latest, present, removed, present, got: presetn"
    }

    # Instal nginx with api module
    $ ansible webservers -m apt -b -a "name=nginx state=present"
    
    vagrant1 | SUCCESS => {
    "cache_update_time": 1537220692,
    "cache_updated": false,
    "changed": true,
    "stderr": "",
    "stderr_lines": [],
    "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...\nThe following extra packages will be installed:\n  libxslt1.1 nginx-common nginx-core\nSuggested packages:\n  fcgiwrap nginx-doc\nThe following NEW packages will be installed:\n  libxslt1.1 nginx nginx-common nginx-core\n0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.\nNeed to get 495 kB of archives.\nAfterthis operation, 1802 kB of additional disk space will be used.\nGet:1 http://archive.ubuntu.com/ubuntu/ trusty-updates/main libxslt1.1 amd64 1.1.28-2ubuntu0.1 [145 kB]\nGet:2 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx-common all 1.4.6-1ubuntu3.8 [19.1 kB]\nGet:3 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx-core amd64 1.4.6-1ubuntu3.8 [325 kB]\nGet:4 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx all 1.4.6-1ubuntu3.8 [5394 B]\nPreconfiguring packages ...\nFetched 495 kB in 4s (110kB/s)\nSelecting previously unselected package libxslt1.1:amd64.\n(Reading database ... 63126 files and directories currently installed.)\nPreparing to unpack .../libxslt1.1_1.1.28-2ubuntu0.1_amd64.deb ...\nUnpacking libxslt1.1:amd64 (1.1.28-2ubuntu0.1) ...\nSelecting previously unselected package nginx-common.\nPreparing to unpack .../nginx-common_1.4.6-1ubuntu3.8_all.deb ...\nUnpacking nginx-common (1.4.6-1ubuntu3.8) ...\nSelecting previously unselected package nginx-core.\nPreparing to unpack .../nginx-core_1.4.6-1ubuntu3.8_amd64.deb ...\nUnpacking nginx-core (1.4.6-1ubuntu3.8) ...\nSelecting previously unselected package nginx.\nPreparing to unpack .../nginx_1.4.6-1ubuntu3.8_all.deb ...\nUnpacking nginx (1.4.6-1ubuntu3.8) ...\nProcessing triggers for ufw (0.34~rc-0ubuntu2) ...\nProcessing triggers for ureadahead (0.100.0-16) ...\nProcessing triggers for man-db (2.6.7.1-1ubuntu1) ...\nSetting up libxslt1.1:amd64 (1.1.28-2ubuntu0.1) ...\nSetting up nginx-common (1.4.6-1ubuntu3.8) ...\nProcessing triggers for ufw (0.34~rc-0ubuntu2) ...\nProcessing triggers for ureadahead (0.100.0-16) ...\nSetting up nginx-core (1.4.6-1ubuntu3.8) ...\nSetting up nginx (1.4.6-1ubuntu3.8) ...\nProcessing triggers for libc-bin (2.19-0ubuntu6.14) ...\n",
    "stdout_lines": [
        "Reading package lists...",
        "Building dependency tree...",
        "Reading state information...",
        "The following extra packages will be installed:",
        "  libxslt1.1 nginx-common nginx-core",
        "Suggested packages:",
        "  fcgiwrap nginx-doc",
        "The following NEW packages will be installed:",
        "  libxslt1.1 nginx nginx-common nginx-core",
        "0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.",
        "Need to get 495 kB of archives.",
        "After this operation, 1802 kB of additional disk space will be used.",
        "Get:1 http://archive.ubuntu.com/ubuntu/ trusty-updates/main libxslt1.1 amd64 1.1.28-2ubuntu0.1 [145 kB]",
        "Get:2 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx-common all 1.4.6-1ubuntu3.8 [19.1 kB]",
        "Get:3 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx-core amd64 1.4.6-1ubuntu3.8 [325 kB]",
        "Get:4 http://archive.ubuntu.com/ubuntu/ trusty-updates/main nginx all 1.4.6-1ubuntu3.8 [5394 B]",
        "Preconfiguring packages ...",
        "Fetched 495 kB in 4s (110 kB/s)",
        "Selecting previously unselected package libxslt1.1:amd64.",
        "(Reading database ... 63126 files and directories currently installed.)",
        "Preparing to unpack .../libxslt1.1_1.1.28-2ubuntu0.1_amd64.deb ...",
        "Unpacking libxslt1.1:amd64 (1.1.28-2ubuntu0.1) ...",
        "Selecting previously unselected package nginx-common.",
        "Preparing to unpack .../nginx-common_1.4.6-1ubuntu3.8_all.deb ...",
        "Unpacking nginx-common (1.4.6-1ubuntu3.8) ...",
        "Selecting previously unselected package nginx-core.",
        "Preparing to unpack .../nginx-core_1.4.6-1ubuntu3.8_amd64.deb ...",
        "Unpacking nginx-core (1.4.6-1ubuntu3.8) ...",
        "Selecting previously unselected package nginx.",
        "Preparing to unpack .../nginx_1.4.6-1ubuntu3.8_all.deb ...",
        "Unpacking nginx (1.4.6-1ubuntu3.8) ...",
        "Processing triggers for ufw (0.34~rc-0ubuntu2) ...",
        "Processing triggers for ureadahead (0.100.0-16) ...",
        "Processing triggers for man-db (2.6.7.1-1ubuntu1) ...",
        "Setting up libxslt1.1:amd64 (1.1.28-2ubuntu0.1) ...",
        "Setting up nginx-common (1.4.6-1ubuntu3.8) ...",
        "Processing triggers for ufw (0.34~rc-0ubuntu2) ...",
        "Processing triggers for ureadahead (0.100.0-16) ...",
        "Setting up nginx-core (1.4.6-1ubuntu3.8) ...",
        "Setting up nginx (1.4.6-1ubuntu3.8) ...",
        "Processing triggers for libc-bin (2.19-0ubuntu6.14) ..."
        ]
    }

    #Check if nginx is statarted
    $ ansible webservers -m service -b -a "name=nginx state=started"
    vagrant1 | SUCCESS => {
        "changed": false,
        "name": "nginx",
        "state": "started"
    }

Excersice 3 - Check if Nginx is running on webservers hosts
-----------------------------------------------------------

::

    $ curl http://localhost:8080
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>


Excersice 4 - Stop Nginx on webservers
--------------------------------------
::

    $ ansible webservers -m service -b -a "name=nginx state=stopped"
    vagrant1 | SUCCESS => {
        "changed": true,
        "name": "nginx",
        "state": "stopped"
    }

Excersice 5 - Check if Nignx is stopped
---------------------------------------

::

    $ curl http://localhost:8080
    curl: (56) Recv failure: Connection reset by peer

Excersice 6 - Destroy all
-------------------------

::

    $ vagrant destroy -f
    ==> vagrant3: Forcing shutdown of VM...
    ==> vagrant3: Destroying VM and associated drives...
    ==> vagrant2: Forcing shutdown of VM...
    ==> vagrant2: Destroying VM and associated drives...
    ==> vagrant1: Forcing shutdown of VM...
    ==> vagrant1: Destroying VM and associated drives...
