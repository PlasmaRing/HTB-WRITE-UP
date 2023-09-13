# Sau  
**SOLVED : ... | 2023**  


## nmap
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/fe6b7754-9647-4603-a391-2710a78c4113)
  
```
nmap 10.10.11.224 -p- -sVC  --min-rate 1000
```
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/76a92f3a-8783-4a05-9465-faa647c0527f)  
Port yang open di **22** dan **55555**, maka dicoba diakses pada `10.10.11.224:55555`  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/f72067ae-be68-42ee-9c60-37a7bbbfb02e)  
  
## exploit
1. Pertama-tama dicoba untuk mengakses `10.10.11.224:55555` yang ternyata diarahkan kedalam sebuah "Request Baskets" dengan versi **1.2.1**  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/c142b35b-30f1-4c2e-b9ad-63e7abc44d70)  

2. Setelah itu dilakukan sedikit research lagi dan ditemukan ternyata adanya kerentanan pada "Request Baskets" dengan versi **1.2.1**, yaitu pada (CVE-2023-27163), disini kerentanannya adalah kemungkinan dilakukannya **Server-Side Request Forgery (SSRF)**  
   ```md
   # (CVE-2023-27163)
   Attackers to access network resources and sensitive information by exploiting the /api/baskets/{name} component through a crafted API request.
   Source : https://github.com/entr0pie/CVE-2023-27163
   ```
   
