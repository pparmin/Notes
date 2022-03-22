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

