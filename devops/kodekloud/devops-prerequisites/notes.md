# DevOps Pre-Requisite Course

* * * 

## Section 1: Linux Basics

**Services:** 
*Services* in the linux context are applications that run continuously in the background and help to provide
with number of background functionality, such as process management, login, DNS, remote login (SSH), etc.

Tools such as Docker or web servers are also automatically configured as a *service* on the system during
installation. 

**Important commands:** 
- Start service: 
  `service <service> start` OR `systemctl start <service>`
  Difference between `service` and `systemctl`: Both commands serve the same purpose, but `service` actually
  uses `systemctl` underneath

- Stop service: 
  `systemctl stop <service>`

- Get status: 
  `systemctl status <service>`

- Configure startup: 
  `systemctl enable <service>` to start at startup
  `systemlctl disable <service>` to disable at startup

**Configuring a program/application as a service:**
If we want to run one of our programs/application continuously on a server, we can configure and manage it,
using `systemctl`. 

1. _Configuring our program as a `system dservice`:_
  A `sytem dservice` is configured using a systemd unit file. These files may be located at `/etc/systemd/system`.

  *Important:* The file must be named after how you want your service
  to be named eventually. 
  Create a .service file (e.g. `my_app.service`): 
  ```
  [Service]
  ExecStart=/usr/bin/python3 /opt/code/my_app.py
  ```
2. _Let systemd know that there is a new service configured:_
  `systemctl deamon-reload`

3. _Start the new service:_
  `systemctl start my_app`

4. _Configuring it to run automatically:_ 
  Add the following directive to your .service file: 
  ```
  [Install]
  WantedBy=multi-user.target
  ```
5. _Enable the new service:_
  Then you can configure it to run at startup using `systemctl enable my_app`

6. _Further possible steps:_
  Provide a description/metadata: 
  ```
  [Unit]
  Description="My test application"
  ```

  You can also add/automatically run other dependencies in the [Service] section or add a directive to 
  automatically restart it if it crashes:

  ```
  [Service]
  ExecStartPre=/opt/code/configure_db.sh
  ExecStartPost=/opt/code/email_status.sh
  Restart=always
  ```

* * * 

## Section 2: Virtualization

**What is Vagrant?** 
Vagrant is a tool that lets you automate the building, configuring and maintaining of virtualization
software (VMs). 

**Important commands:** 

- _Initalize a box:_
`vagrant init centos/7` lets you deploy a specific box (e.g. "centos/7"). A *vagrant box* is a specific
OS image with scripts that help configuring it properly. The init command creates a *Vagrantfile*. 


- _Start a box:_
`vagrant up` then starts the box. It will download the image and configure the necessary settings (networking, etc.)

- _SSH into the VM:_
`vagrant ssh` vagrant will set up and use the correct port to ssh into the VM

- _Stop VM:_
`vagrant halt`

**Vagrant File layout example:**
```
Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"
  
  # port forwarding
  config.vm.network "forwarded_port", guest: 80, host: 8080
  # sync folder
  config.vm.synced_folder "../data", "/vagrant_data"
  
  # automatically assign RAM
  config.vm.provider "virtualbox" do |vb|
    
    vb.memory = "1024"

  end
  
  # execute specific shell commands at startup
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  SHELL

end
```

* * * 

## Section 3: Networking

**Important commands:** 
- `ip link` list and modify interfaces on the host
- `ip addr` see the ips assigned to these interfaces
- `ip addr add <ip> dev eth0` temporarily add a; for persistance longer than the current system runtime
  you can set them in the /etc/network/interfaces file 
- `ip route`/`route` views the routing table
- `ip route add <network> via <gateway ip>` adds a route over a specific gateway
- `cat /proc/sys/net/ipv4/ip_forward` checks whether ip forwarding is enabled on the system
