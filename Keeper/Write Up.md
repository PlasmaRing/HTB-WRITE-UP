# Keeper  
**SOLVED : SEPT 07 | 2023**  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/54087457-a1ad-4ed3-beb3-cb3ae9b29ccc)

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
1. Pertama-tama dicoba untuk mengakses `tickets.keeper.htb` yang ternyata diarahkan kedalam login page "request tracker"  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/e72718d0-1395-4c66-86a8-fdcbf0ec2ac2)  
2. Dikarenakan login page memerlukan kredensial, maka dicarilah kredensial umum untuk website RT  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/713ebe29-9be0-4083-b47c-705670c9fb5c)  
Default Creds : `root:password`  

3. Setelah berhasil masuk dengan _default creds_, didapati tampilan web berupa admin panel. Dan setelah melakukan analisa singkat, ditemukan adanya panel user yang memuat adanya 2 registered user termasuk root.  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/791cc01b-a53c-4a47-9e43-e4eaa8849ef8)
4. Analisa pada user **lnorgaard**, dan disini ditemukan adanya password yang sengaja disimpan pada _comments_
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4406f6c6-502f-4467-be94-5a7cfb29a146)  
User Creds : `lnorgaard:Welcome2023!`

5. Setelah itu bisa dicoba untuk **ssh login** dengan menggunakan kredensial user  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4e66758c-7015-42a8-8025-70f54fc79365)
Flag user disimpan dalam `user.txt`  
**USER FLAG : 4568e6ca6ced018b446f6ae5688ff786**

6. Selanjutnya dikarenakan dalam directory `/home/lnorgaard/` terdapat zip file `RT30000.zip`, maka akan dicoba untuk melakukan analisa isi file tersebut  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/4ab0c0a4-0a6d-426b-b414-15c8b7b25286)  
Untuk mendownload zip file tersebut, dilakukan command seperti dibawah
   ```
   scp lnorgaard@10.10.11.227:/home/lnorgaard/RT30000.zip .
   ```
   Hasilnya didapati 2 file dengan ekstensi **.dmp** dan **.kdbx**   
   ![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/c17a80b9-6b02-405b-b26e-fa9a1f6a3ec4)

7. Karena file merupakan **.kdbx**, maka dilakukan _research_, dan didapati bahwa ekstensi tersebut dapat dibuka dengan tools bernama **KeePass**, disini setelah dibuka dengan **KeePass**, ternyata dibutuhkan **Master Key** untuk mengaksesnya.  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/434bbd45-eea2-4f9a-8336-eb2691c3470f)  
Dari sini sudah ditarik kesimpulan bahwa file **.dmp**, merupakan file yang bisa saja menyimpan **Master Key**.

8. Setelah itu dilakukan sedikit _research_ lagi dan ditemukan ternyata adanya kerentanan pada _dump file_ yang dihasilkan oleh **KeePass**, yaitu pada **(CVE-2023-32784)**, disini kerentanannya adalah possible password dapat direstore dari file **.dmp**

   Tools: 
   ```
   KeePass 2.X Master Password Dumper   
   https://github.com/vdohney/keepass-password-dumper  
   https://github.com/CMEPW/keepass-dump-masterkey
   ```
9. Disini saya menggunakan **KeePass 2.X Master Password Dumper Python Version** dan didapati beberapa **possible passwords**  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/57e4a5f7-30d6-4003-9779-b72f5c973130)
10. Dikarenakan terdapat beberapa Simbol yang diluar ASCII, maka saya mencoba mencarinya menggunakan **Google Search Engine**, dan ditemukan adanya nama makanan yang sangat mirip dengan **possible password** yakni **Rodgrod Med Flode**  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/b2e06a31-133a-4e5e-a131-59e05b1eb159)  
   Source : https://www.196flavors.com/rodgrod-med-flode/  
   Password : `rødgrød med fløde`  

11. Buka **.kdbx** file dengan Master Key `rødgrød med fløde`, disini ditemukan password untuk root dan lnorgaard, namun bila memasukannya langsung untuk **root creds** pada ssh tidak diketahui kenapa tidak bisa masuk  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/8de7076b-a290-44c1-b23c-49dac95dbc43)
12. Karena disini terdapat key dari **puttygen** yang disematkan dalam notes, maka dapat dicoba untuk **generate RSA KEY**  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/2d05fd30-c7d4-4582-99fe-9b3541a86e39)  
13. Generate RSA KEY dengan **puttygen** dengan command `puttygen RSAKEY.ppk -O private-openssh -o rsa_key`  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/8e7dab46-03ed-4c24-89c0-44e9c0e26574)  

14. Setelah itu bisa dicoba untuk **root ssh login** dengan menggunakan rsa key yang sudah di generate  
![image](https://github.com/PlasmaRing/HTB-WRITE-UP/assets/92077284/7968b1a4-89b3-40ad-b992-b0f9e844f949)  
Flag user disimpan dalam `root.txt`  
**ROOT FLAG : 8828d4244059a3ef5077e9bae5700c89**




