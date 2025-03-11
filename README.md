# netmiko-automation
Establishes SSH for Router10 and provides SHA-1 RSA negotiation, gives output for show commands:
-IP interface brief 
-IP route
Clone Repository:

git clone https://github.com/greg5768/netmiko-automation.git
cd netmiko-automation


Prequisites:
-Ensure install of netmiko and paramiko libraries for use:

pip install netmiko paramiko

Code:

from netmiko import ConnectHandler ##Imports netmiko library and ConnectHandler function for SSH access
import paramiko ##Imports paramiko

router10 = {      ##Creates dict for Router10 with associated values
    'device_type': 'cisco_ios',
    'host': 'your device IP address',
    'username': 'greg234',
    'password' : 'password',
    'secret': 'password',
    }
    
paramiko.Transport._preferred_kex = ('diffie-hellman-group14-sha1',) ##Used to authenticate SSH session using SHA-1
paramiko.Transport._preferred_host_key_types = ('ssh-rsa',) 

try: 
    print('Attempting SSH Connection') 
    
    connect = ConnectHandler(**router10) ##Passes Router10 to ConnectHandler function to initate SSH session
    
    print('Connection Established')
    
    ipbri = connect.send_command('show ip int bri') ##Executes "show ip interface brief on R10
    
    print(ipbri) ##Outputs show ip int bri contents
   
    route = connect.send_command('show ip route') ##Executes "show ip route" on R10
    
    print(route) ##Outputs show ip route contents

except Exception as e:
    
    print(f' SSH Connection failed: {e}') ##Provides failover debug for SSH connection failure
