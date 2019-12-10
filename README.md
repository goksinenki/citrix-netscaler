# citrix-netscaler

Citrix Netscaler Automation Scripts (Check my Python repo - paramiko module to automate all)


Example Citrix Netscaler Load Balancer Configuration 

The variables

Vlan ID : 100
Vlan Description : Vlan_test
Virtual Server Self IP : 192.168.1.11
Virtual Server IP : 192.168.1.12
Node (Server IP Adresses) :  192.168.1.13 and 192.168.1.14
LB Service Port : 443
Node (Server) Service Port : 80
Monitor Type : tcp
Persistence : CookieInsert
Virtual Server Name : test_virtual_server_name
Pool (Service Group) Name :  test_pool_name
SSL Certificate Name : wildcard.sslsertificate.com (manually pre-defined)

You need to edit your variables depends on your configuration :)

These are the Citrix CLI commands that you can execute directly via SSH, you can also use my Python scripts to automate all. (Check my repo)

add vlan 100 -aliasName "Vlan_test"

bind vlan 100 -ifnum LA/1 -tagged

add ns trafficdomain 100 -aliasName "Vlan_test"

bind ns trafficdomain 100 -vlan 100

add ns ip 192.168.1.11 255.255.255.0 -type VIP -td 100 -icmp ENABLED -vServer ENABLED -arp ENABLED -state ENABLED

add lb vserver test_virtual_server_name SSL 192.168.1.12 443 -td 100 -lbMethod ROUNDROBIN

bind ssl vserver test_virtual_server_name -certkeyName wildcard.sslsertificate.com

add servicegroup test_pool_name HTTP -td 100

bind servicegroup test_pool_name 192.168.1.13 80

bind servicegroup test_pool_name 192.168.1.14 80

bind lb vserver test_virtual_server_name test_pool_name

bind serviceGroup test_pool_name -monitorName tcp

Set lb vserver test_virtual_server_name -persistenceType COOKIEINSERT

save ns config
