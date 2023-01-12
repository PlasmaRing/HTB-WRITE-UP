# Precious 
**SOLVED : JAN 11 | 2023**
![Precious](https://user-images.githubusercontent.com/92077284/211991670-4b07aee9-b558-4b25-9b42-47e17659e981.png)

## Information Gathering
![image](https://user-images.githubusercontent.com/92077284/211992854-535f66c3-a172-4997-9d7d-82a58bb42449.png)  
Didapati ada 2 port, yaitu port **22 dan 80**, serta alamat web yaitu precious.htb  
Untuk mengakses precious.htb diperlukan command `sudo vim /etc/hosts` untuk menambahkan `10.10.11.189 precious.htb`  
![image](https://user-images.githubusercontent.com/92077284/211993812-b3068588-4e9d-408d-af3d-45356d7fba78.png)  
Masuk ke **10.10.11.189**  
![image](https://user-images.githubusercontent.com/92077284/211994354-6a6245cb-f01b-4bde-8634-d19b03e707eb.png)  
Apabila dicoba isi 'http://google.com' maka akan menunjukan tampilan seperti ini  
![image](https://user-images.githubusercontent.com/92077284/211995365-debd1424-dd52-41d1-a55e-4d5855cc7e37.png)
Maka dari itu akan dicoba untuk hosting pada port 80 dengan ip yang sesuai dengan ip di **tun0**  
![image](https://user-images.githubusercontent.com/92077284/211996025-da71de3e-fdf1-44f5-9b52-be06a30c0176.png)  
![image](https://user-images.githubusercontent.com/92077284/211996193-9f0b90e1-ca91-4d1f-94df-656e60a70f0c.png)  
Lalu coba masukan 'http://10.10.14.30:80' ke dalam website  
![image](https://user-images.githubusercontent.com/92077284/211996453-a4bd650a-8551-4b1d-bd86-c485a273c7d7.png)  
Didapati file pdf sebagai berikut  
![image](https://user-images.githubusercontent.com/92077284/211996521-9d32d634-253e-4ae5-b2f6-144b1ce999dc.png)  
Setelah itu analisa file pdf menggunakan **exiftool**  
![image](https://user-images.githubusercontent.com/92077284/211996948-fb392098-7706-4054-8e85-0d6e1068d64f.png)  
Disini saya memanfaatkan **pdfkit v0.8.6** sebagai celah untuk masuk kedalam sistem, pdfkit adalah _PDF document generation library_ untuk browser, yang fungsinya memudahkan browser membuat file.  
**CVE-2022–25765** Source: https://security.snyk.io/vuln/SNYK-RUBY-PDFKIT-2869795  
Dari situ dapat dibuat _reverse shell_ 

## a
## a
## a
## a
