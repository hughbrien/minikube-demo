# Install AppDynamics COP Host and Database Collectors 

Installing Hosts and DBs
Installation Page https://<HOST>.observe.appdynamics.com/ui/configure/databasesAndHosts


**Need super user  / root permission.**

The installer will create a User called "appdynamics"

sudo apt install appdinfracol
sudo apt install appdotelcol

sudo apt show  appdotelcol
sudo apt show appdinfracol


Reading package lists... Done
Building dependency tree... Done
Unpacking appdinfracol (24.2.0~5093) over (24.2.0~5093) ...
Setting up appdinfracol (24.2.0~5093) ...

Created symlink /etc/systemd/system/multi-user.target.wants/appdnodeexporter.service → /lib/systemd/system/appdnodeexporter.service.
Created symlink /etc/systemd/system/multi-user.target.wants/appdinfracol.service → /lib/systemd/system/appdinfracol.service.

Then run the following:

systemctl  | grep appd

appdinfracol.service                                                                                     loaded active     running   Appdynamics Infra Collector

appdnodeexporter.service                                                                            loaded active     running   Prometheus Node Exporter

● appdotelcol.service                                                                                   loaded failed     failed    AppDynamics Distribution of OpenTelemetry Collector


Apt show 


Configuration Files Exist : 

/etc/systemd/system/multi-user.target.wants

There are three parts that 
appdinfracol.service
appdnodeexporter.service
appdotelcol.service


Made up of three components:

root@lucy:~# ps -ef | grep appdynamics 
appdyna+  325777       1  0 20:20 ?        00:00:00 /opt/appdynamics/appdinfracol/third_party/node_exporter/node_exporter --collector.netclass.ignore-invalid-speed
appdyna+  333354       1  0 20:27 ?        00:00:00 /opt/appdynamics/appdinfracol/bin/appdynamics-infra-manager
appdyna+  333367  333354  0 20:27 ?        00:00:00 /opt/appdynamics/appdinfracol/collectors/appdynamics-server-collector




root@lucy:~# systemctl status appdinfracol > data.txt



Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [privileged] = [false]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [type] = [Collector]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [default-collector] = [true]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [dependency] = [/third_party/node_exporter/node_exporter]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [exporter-address] = [127.0.0.1]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [exporter-port] = [9100]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [name] = [Server Monitor]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.204Z        INFO        Servermon_collector        config/config.go:129        lucy: Setting Key: [path] = [./collectors/appdynamics-server-collector]
Mar 06 20:32:53 lucy appdynamics-infra-manager[885]: 2024-03-06T20:32:53.205Z        INFO        Servermon_collector        clientlib/clientLib.go:565        lucy: Event : Message received, by clientlib from Infra Manager and sent to collector [{config map[default-collector:true dependency:/third_party/node_exporter/node_exporter enabled:true expor>
Mar 06 20:33:10 lucy appdynamics-infra-manager[885]: 2024-03-06T20:33:10.204Z        WARN        Servermon_collector        cloud/provider.go:547        lucy: Error obtaining Amazon meta token, err: error executing request for Amazon metadata token: Put "http://169.254.169.254/latest/api/token": context deadline exceeded


root@lucy:~# systemctl status appdinfracol > data.txt
root@lucy:~# more data.txt 
● appdinfracol.service - Appdynamics Infra Collector
     Loaded: loaded (/lib/systemd/system/appdinfracol.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2024-03-06 20:32:52 UTC; 3min 19s ago
   Main PID: 694 (appdynamics-inf)
      Tasks: 20 (limit: 9390)
     Memory: 34.6M
        CPU: 99ms
     CGroup: /system.slice/appdinfracol.service
             ├─694 /opt/appdynamics/appdinfracol/bin/appdynamics-infra-manager
             └─885 /opt/appdynamics/appdinfracol/collectors/appdynamics-server-collector ""

Mar 06 20:33:10 lucy appdynamics-infra-manager[885]: 2024-03-06T20:33:10.204Z        WARN        Servermon_collector 
       cloud/provider.go:547        lucy: Error obtaining Amazon meta token, err: error executing request for Amazon 
metadata token: Put "http://169.254.169.254/latest/api/token": context deadline exceeded
Mar 06 20:35:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:35:00.016Z        INFO        Servermon_collector 
       client/grpcclient.go:66        lucy: Custom SSL Certificate Authority not found at 'certs/ca/ca.pem', using sy
stem default
Mar 06 20:35:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:35:00.017Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:35:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:35:00.017Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:35:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:35:00.018Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:35:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:35:00.018Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:36:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:36:00.017Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:36:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:36:00.017Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:36:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:36:00.017Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"
Mar 06 20:36:00 lucy appdynamics-infra-manager[885]: 2024-03-06T20:36:00.018Z        ERROR        Servermon_collector
        transformer/sender.go:51        lucy: Metrics Error: Send Failed, for: failed to send message to gRPC receive
r: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial tcp 127.0.0.1:
4317: connect: connection refused"





I am running an installation Installation of Otel Infrastructure Collector on Lab1 : 
https://lab1.observe.appdynamics.com/ui/configure/databasesAndHosts

We  should add some additional instructions

You MUST create the /opt/appdynamics/appdynamics.conf file 
Installation needs to be done with super user privileges (root access)
After installation run the following : 


# ps -ef | grep appdynamics
 
appdyna+ 325777 1 0 20:20 ? 00:00:00 /opt/appdynamics/appdinfracol/third_party/node_exporter/node_exporter --collector.netclass.ignore-invalid-speed

appdyna+ 333354 1 0 20:27 ? 00:00:00 /opt/appdynamics/appdinfracol/bin/appdynamics-infra-manager

appdyna+ 333367 333354 0 20:27 ? 00:00:00 /opt/appdynamics/appdinfracol/collectors/appdynamics-server-collector
,

Add the AppDynamics repository:
sh -c "echo 'deb [trusted=yes] https://appdynamics.jfrog.io/artifactory/deb-hosted/ appdynamics main' >> /etc/apt/sources.list"

Refresh the repository:
apt-get update

Install the AppDynamics Infrastructure Collector:
apt-get install appdinfracol







