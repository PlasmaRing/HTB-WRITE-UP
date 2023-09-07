# Keeper  
**SOLVED : SEPT 07 | 2023**



## nmap
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/04372d38-b9ed-4d6c-921b-625f3dd43b35)  
```
nmap 10.10.11.227 -p- -sVC  --min-rate 1000
```
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/32830cfb-6e00-4403-b98f-594a8d2948ee)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/593eeae6-4fd7-42f5-8be7-63aa57d7c19a)  



```
sudo nano etc/hosts
```
tambahkan `10.10.11.227    tickets.keeper.htb`

## exploit
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/e72718d0-1395-4c66-86a8-fdcbf0ec2ac2)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/713ebe29-9be0-4083-b47c-705670c9fb5c)  

![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/791cc01b-a53c-4a47-9e43-e4eaa8849ef8)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4406f6c6-502f-4467-be94-5a7cfb29a146)  
User Creds : `lnorgaard:Welcome2023!`  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4e66758c-7015-42a8-8025-70f54fc79365)  
**USER FLAG : 4568e6ca6ced018b446f6ae5688ff786**  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4ab0c0a4-0a6d-426b-b414-15c8b7b25286)  
```
scp lnorgaard@10.10.11.227:/home/lnorgaard/RT30000.zip .
```
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/c17a80b9-6b02-405b-b26e-fa9a1f6a3ec4)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/434bbd45-eea2-4f9a-8336-eb2691c3470f)  

KeePass 2.X Master Password Dumper (CVE-2023-32784)  
https://github.com/vdohney/keepass-password-dumper  
https://github.com/CMEPW/keepass-dump-masterkey  

![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/57e4a5f7-30d6-4003-9779-b72f5c973130)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/b2e06a31-133a-4e5e-a131-59e05b1eb159)  

https://www.196flavors.com/rodgrod-med-flode/  
`rødgrød med fløde`  

![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/8de7076b-a290-44c1-b23c-49dac95dbc43)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/2d05fd30-c7d4-4582-99fe-9b3541a86e39)  

![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/8e7dab46-03ed-4c24-89c0-44e9c0e26574)  
`puttygen RSAKEY.ppk -O private-openssh -o rsa_key`  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/7968b1a4-89b3-40ad-b992-b0f9e844f949)  
**ROOT FLAG : 8828d4244059a3ef5077e9bae5700c89**




