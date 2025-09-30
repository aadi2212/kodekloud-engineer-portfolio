Create a Docker Network



1]

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:



a. Create a docker network named as blog on App Server 3 in Stratos DC.

b. Configure it to use macvlan drivers.

c. Set it to use subnet 192.168.0.0/24 and iprange 192.168.0.0/24.



->



📘 Task: Create Docker Network with macvlan Driver



Problem Statement



The Nautilus DevOps team needs to set up docker environments for different applications.

As part of this, a docker network must be created on App Server 3 with the following requirements:



Network Name: blog

1]Driver: macvlan

2]Subnet: 192.168.0.0/24

3]IP Range: 192.168.0.0/24

4]Gateway: 192.168.0.1



Steps that I performed -

1]Login to app server 3

&nbsp;ssh@172.16.238.12



2]Created Docker Network with macvlan driver

&nbsp; sudo docker network create -d macvlan  --subnet=192.168.0.0/24  --ip-range=192.168.0.0/24  --gateway=192.168.0.1  blog



-d macvlan → specifies the network driver

--subnet → assigns network subnet

--ip-range → restricts IP allocation range

--gateway → sets gateway inside the subnet

blog → network name





3]Verified the created network

sudo docker network ls

sudo docker network inspect blog





Verification:

docker network ls showed blog network with macvlan driver.

docker network inspect blog output contained:



"Name": "blog",

"Driver": "macvlan",

"IPAM": {

&nbsp;   "Config": \[

&nbsp;       {

&nbsp;           "Subnet": "192.168.0.0/24",

&nbsp;           "IPRange": "192.168.0.0/24",

&nbsp;           "Gateway": "192.168.0.1"

&nbsp;       }

&nbsp;   ]

}





Notes / Learnings



macvlan driver is used when containers need to appear as separate physical devices on the local network.



Gateway is mandatory for subnet-based macvlan networks, else Docker may throw errors.



Always verify using docker network inspect <network\_name> after creation.



