AAA - Radius/Tacacs+ Configuration

**TACACS+**

- In conf t -> username Greg password cisco
- aaa new model
- username Greg privilege 15
- aaa group server tacacs+ *gns3group*
    - server name *container*
    - exit
- tacacs server container
    - address ipv4 *192.168.122.201*
    - key *gns3*
    - *exit*
- aaa authentication login default group gns3group local

* * *

**RADIUS**

- In conf t -> username Greg password cisco
- aaa new model
- radius server radius1
    - address ipv4 *192.168.122.201 * auth-port 1812 acct-port 1813
    - key gns3
    - exit
- aaa group server radius gns3group2
    - server name radius1
    - exit
- aaa authentication login default group gns3group2 local

* * *