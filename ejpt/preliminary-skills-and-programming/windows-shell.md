# Windows Shell

* `cmd.exe` or Windows Command Line is the Microsoft equivalent of the Linux Bash Shell.
* Its usual location is `C:\Windows\system32\cmd.exe`.
* CMD relies mainly on built-in commands.
* Some of the are: `dir`, `cls`, `move` or `del`.

### Windows Environment

In Windows 10: `Control Panel` > `System and Security` > `System` > `Advanced System Settings`.

* There are System variables (for all users) and for current user.
* Windows does have a `PATH` variable too, where the executable directories are separated through the `;` symbol.

### Windows Commands and Programs

* Windows CMD supports more built-in commands than the Linux one.
* If you would like to make your newly installed software executable from the command line, syou should place it within any of your `PATH` locations or change the `PATH` variable to contain its location.

### Windows Output Redirectors and Special Characters

* Windows CMD is a less flexible scripting env than Bash.
* PowerShell is better to create advanced scripts in Windows.
* In order to access Windows' env variables: `%varname%`.
* You can print env variables using `echo`, as in: `echo %PATH%`.
* `set` allows you to view variables.
* You can also create your own variables or temporarily modify existing ones.
* Any modifications will not be permanent and will only exist in the current `cmd.exe` window.
* Two different `cmd.exe` windows will not affect each other.
* Output redirection also works in Windows, as in `echo aaa > file.txt` or the append version: `echo bbb >> file.txt`.
* View files with `type` command: `type file.txt`.
* Some ways of command chaining:
  * `command1 & command2`: execute both regardless of the result.
  * `command1 && command2`: execute the 2nd one if the 1st one's execution succeeded.
  * `command1 | command2`: send output from the first command to the second one.
  * `command1 || command2`: execute the 1st command, and if ti fails, execute the second one.

### Windows Conditional Statements and Loops

* `.bat` files allow you to save larger command line scripts

```
SET x=123
if %x%==123 (echo true) # true
if %x%==xyz (echo true) # nothing is output
if %x%==xyz (echo true) else (echo "does not contain xyz")
```

For loop:

```
for %i in (*.*) do @echo FILE: %i
```

> `@`: hides the command prompt and just display the output
