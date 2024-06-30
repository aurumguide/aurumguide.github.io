---
title: "SQL server 2022 install guide"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql/Mssql_Install/

toc: true
toc_sticky: true
 
date: 2024-06-23
last_modified_at: 2024-06-23
---

Today, I will explain how to install 2022, the latest version of MSSQL.  
We have captured the screen in detail so that anyone can do it for beginners and non-majors, so please explain it and install it while looking at the screen overlapping.  
There is also a process of installing the SSMS (MSSQL connection tool), so please follow it.  
Please refer to the operating system support table before installing sql server 2022.  

## Sql server 2022 install requirements.

### Hardware Requirements  

- `Hard disk capacity`: MS says that you need at least 6GB of available hard drive space to install MSSQL, but I think you need 30GB to TEST after installation.  
- `memory`: MSSQL requires at least 1GB of memory. However, you must have 8GB.  
- `CPU Recommendations`: It should be at least 2.0 GHz.  
- `Processor Type`: x64 Processor: AMD Opteron, AMD Athlon 64, IIntel Xeon with Intel EM64T.  
- `Internet connection`: We're going to download it and install it right away, so it should be a computer connected to the Internet.  

### Software Requirements.

- Commonly used computers only support Windows 10 1607 or later If you want to install it on a server, it only supports Windows Server 2016 or later.
- We'll install it in window 11.  

## Sql server 2022 installation order

#### <mark>1. Search for MSSQL 2022 downloads and go to the site.</mark>

![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/1.jpg)  

#### <mark>2. Click Download as a free version of the developer.</mark>

![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/2.jpg)  

#### <mark>3. On your computer, go to the downloaded path and click it.</mark>
 
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/3.jpg)  

#### <mark>4. We install it by default.</mark>
 
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/4.jpg)  

#### <mark>5. Read the Terms of Use and click Accept. </mark>

![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/5.jpg)  

#### <mark>6. Click Install. If you want to reroute, you can do so by browsing.</mark>

![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/6.jpg) 

#### <mark>7. Please install SSMS after installation. This is a tool to connect to MSSQL.</mark>

![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/7.jpg)

#### <mark>8. Click Download SSMS. </mark>
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/8.jpg)

#### <mark> 9. Please click Install.</mark>
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/9.jpg)

#### <mark> 10. If you're done, click Close and you're done.</mark>
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/10.jpg)

#### <mark> 11. Please check if it is installed.</mark>
![MSSQL 2022 downloads](/assets/images/postsImages/MsSql/1001_Eng_Mssql_install/11.jpg)

## If you have the sql server 2022 installation manual, please share it with each other

### Operating system support table.

<mark>Verify support by server.</mark>

| **SQL Server  Datacenter** | **Enterprise** | **Developer** | **Standard** | **web** | **Express** |
| --- | --- | --- | --- | --- | --- |
| Windows server 2022 Datacenter Datacenter | Yes | Yes | Yes | Yes | Yes |
| Windows server 2022 Datacenter | Yes | Yes | Yes | Yes | Yes |
| Windows server 2022 Standard | Yes | Yes | Yes | Yes | Yes |
| Windows server 2022 Essentials | Yes | Yes | Yes | Yes | Yes |
| Windows server 2019 Datacenter | Yes | Yes | Yes | Yes | Yes |
| Windows server 2019 Standard | Yes | Yes | Yes | Yes | Yes |
| Windows server 2019 Essentials | Yes | Yes | Yes | Yes | Yes |
| Windows server 2016 Datacenter | Yes | Yes | Yes | Yes | Yes |
| Windows server 2016 Standard | Yes | Yes | Yes | Yes | Yes |
| Windows server 2016 Essentials | Yes | Yes | Yes | Yes | Yes |
| Windows 11 IoT Enterprise | **No** | Yes | Yes | **No** | Yes |
| Windows 11 Enterprise | **No** | Yes | Yes | **No** | Yes |
| Windows 11 Professional | **No** | Yes | Yes | **No** | Yes |
| Windows 11 Home | **No** | Yes | Yes | **No** | Yes |
| Windows 10 IoT Enterprise | Yes | Yes | Yes | **No** | Yes |
| Windows 10 Enterprise | Yes | Yes | Yes | **No** | Yes |
| Windows 10 Professional | Yes | Yes | Yes | **No** | Yes |
| Windows 10 Home | **No** | Yes | Yes | **No** | Yes |

### Complete the MS SQL Server 2022 installation.

- Please check if there is an error when installing MS SQL Server 2022.  
- Check your current operating system and install MS SQL Server 2022.  
- If installing on ssd, please check the hardware requirements.  
- If you are installing on linux, please check the software requirements.
- If you are running out of capacity, please check the disk space requirements.  
- In practice, please check the storage type of the data file when installing it.  
- Please check the installation of MS SQL Server 2022 on the domain controller.  
