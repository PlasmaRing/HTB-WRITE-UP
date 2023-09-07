![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/80df468b-3f9e-4cc2-8fa9-b93fb689d627)# Keeper 
**SOLVED : SEPT 07 | 2023**


## nmap
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/04372d38-b9ed-4d6c-921b-625f3dd43b35)  
```
nmap 10.10.11.227 -p- -sVC  --min-rate 1000
```
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/32830cfb-6e00-4403-b98f-594a8d2948ee)  

// SS SEBELUM TIKETS KEEPER


```
sudo nano etc/hosts
```
tambahkan `10.10.11.227    tickets.keeper.htb`

## exploit
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/791cc01b-a53c-4a47-9e43-e4eaa8849ef8)  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4406f6c6-502f-4467-be94-5a7cfb29a146)




