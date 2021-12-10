# Bash Shell

* The main non-graphical tool to interact with the operating system is Shell.
* In FreeBSD there is no GUI at all.
* Other notable shells: `ksh`, `zsh`, `dash`.

### Bash Environment

* Upon the start of the shell, the OS checks for the existence of several files as: `~/.bashrc`, `~/.bash_login`, `~/.bash_profile`, `~/.bash_logout`.
* Environment Variables can be viewed by typing `env`.
* `PATH` is a relevant environment variable, which has a format of `[location]:[location]:...:[location]`.
* The `PATH` variable is one of the execution helpers.

### Bash Commands and Programs

* Bash has some built-in commands that provide basic functionality.
* Examples are: `fg`, `echo`, `set`, `while`.
* Most commands that are used in everyday tasks are external mini-programs kept in `PATH` locations (use `which` to find the real location).
* `man` displays help about commands.

### Bash Output Redirectors and Special Characters

* `~`: current user's home directory.
* `*`: wildcard that can be used for choosing only certain types of files.
* `$()`: will be evaluated before the whole statement and will become part of this statement.
* Use `command > file.txt` format to create a file containing command's output.
* Use `command >> file.txt` format to append containing command's output to an existing file.
* `|`: pipe.
* chaining commands is a quite powerful Bash feature, one-liners.

```bash
file `ls /etc/*.conf | sort` > test.txt && cat test.txt | wc -l
```

### Bash Conditional Statements and Loops

* `chmod +x scriptname` so you can execute this script with `./scriptname`

```bash
echo '#!/bin/bash' > script.sh
echo 'ls /tmp | wc -l' >> script.sh
chmod +x script.sh
./script.sh
```

```bash
if <conditions>; then
<commands>
fi
```

```bash
if [ x ]; then
  docommand
elif [ y ]; then
  doothercommand
else
  dosomethingelse
fi
```

Conditional statements:

* `-eq`: equal
* `-ne`: not equal
* `-lt`: less than
* `-le`: less than or equal
* `-gt`: greater than
* `-ge`: greater than or equal

Loops:

```bash
#!/bin/bash

for i in $(ls); do
  echo item: $i
done
```

or:

```bash
#!/bin/bash
for i in `seq 1 10`;
do
  echo $i
done
```

While loop:

```bash
while [condition]; do command1; command2; done
```

for example:

```bash
while read line; do echo $line; done < file.txt
```

Filters open ports from nmap output files

```bash
cat *.nmap | grep "open" | grep -v "filtered" | cut -d '/' -f 1 | sort -u | xargs | tr ' ' ',' > ports.txt
```

Fingerprint potential applications:

```bash
cat domains.txt
domain1.com
domain2.com
```

alivecheck.sh:

```bash
#!/bin/bash

for protocol in 'http://' 'https://';do
  while read line;
  do
    code=$(curl -L --write-out "%{http_code}\n" --output /dev/null --silent --insecure $protocol$line)
    if [ $code = "000" ]; then
      echo "$protocol$line: not responding."
    else
      echo "$protocol$line: HTTP $code"
      echo "$protocol$line: $code" >> alive.txt
    fi
  done < domains.txt
done
```
