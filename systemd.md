# Creating new systemd services
This example shows how to create a unit file for a custom service. Custom unit files are located in /etc/systemd/system/ and have a .service extension. For example, a custom foo service uses /etc/systemd/system/foo.service unit file.

Prerequisites
You are logged in as a user with administrator-level permissions.

Procedure
This procedure creates a basic configuration file to control the foo service.

Create and edit the new configuration file:
  nano /etc/systemd/system/foo.service

The next few steps describe each section its parameters to add to the file:

The [Unit] section provides basic information about the service. The foo service uses the following parameters:

Description
A string describing the unit. Systemd displays this description next to the unit name in the user interface.

After
Defines a relationship with a second unit. If you activate the unit, systemd activates it only after the second one. For example, the foo service might require network connectivity, which means the foo services specifies network.target as an After= condition.

The resulting [Unit] section looks like this:

[Unit]
Description=My custom service
After=network.target
The [Service] section provides instructions on how to control the service. The foo service uses the following parameters:

Type
Defines the type of systemd service. In this example, the foo service is a simple service, which starts the service without any special consideration.

ExecStart
The command to run to start the service. This includes the full path to the command and arguments to modify the service.

The resulting [Service] section looks like this:

[Service]
Type=simple
ExecStart=/usr/bin/sleep infinity
The [Install] section provides instructions on how systemd installs the service. The foo service uses the following parameters:

WantedBy
Defines which service triggers the custom service if enabled with systemctl enable. This is mostly used for starting the custom service on boot. In this example, foo.service uses multi-user.target, which starts foo.service when systemd loads multi-user.target on boot.

The full foo.service file contains the following contents:

[Unit]
Description=My custom service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/sleep infinity

[Install]
WantedBy=multi-user.target
Save the file.

To make systemd aware of the new service, reload its service files

# systemctl daemon-reload
Start the custom foo service:

# systemctl start foo
Check the status of the service to ensure the service is running:

$ systemctl status foo
● foo.service - My custom service
   Loaded: loaded (/etc/systemd/system/foo.service; static; vendor preset: disabled)
   Active: active (running) since Thu 2017-12-14 14:09:12 AEST; 6s ago
 Main PID: 31837 (sleep)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/foo.service
           └─31837 /usr/bin/sleep infinity

Dec 14 14:09:12 dansmachine systemd[1]: Started My custom service.
