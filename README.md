# Port Scanner #

When we talk about ports what are we really talking about? A port at the O.S. (Operating System) level is just a “file descriptor” associated with a process. A file descriptor is just a number which is used to reference an open I/O channel, like stdout (standard output), a network socket or a file.

When the OS receives a TCP/IP packet it looks at the destination port & tries to find a process which is listening on this port. Then if there is a listening process the packet is delivered to it.

There are 65.535 ports available in total, but in practice not all of them are used regularly. 

If you don't use **nmap** or other software and need to verify if some port in your server is listening, you can use Ruby for that job! 

Feel free to fork, change, modify and re-use this files.

### About Port Scanner in Ruby ###

This repository has two files: 

* tcpsocket_scanner.rb 
* socket_scanner.rb

You can use either file depending on your need.

### TCPSocket Scanner ###

The *tcpsocket_scanner.rb* open a new TCP connection using **TCPSocket** & then rely on the fact that a rejected connection will raise the **Errno::ECONNREFUSED** exception. One limitation with this code is that a non-responding port will make you wait for about 20 seconds until the connection times out.You can only scan one port at a time.

You can use *tcpsocket_scanner.rb* file passing port and server address as command line arguments: 
````
$ ruby tcpsocket_scanner.rb 22 example.com
````
This command line above will check if server under example.com is listening port 22. 

### Socket Scanner ###

The *socket_scanner.rb* is more robust. It use a combination of **connect_nonblock**, **IO.select & Socket** (instead of **TCPSocket**, which initiates a connection as soon as you create the object). The **IO.select** call will wait until the socket is ready to receive data (which means it’s open) or until **TIMEOUT** time has passed, after which we can assume that the port is either closed or ignoring connection requests.

You need to set the server address (localhost is default) and the ports you can check (21, 22, 23, 25, 53, 80, 443, 3306, 5432, 8080 are set) in the file before you use it. Here how you can use:
````
$ ruby socket_scanner.rb
````
The return will look similar to the output below:
````
Port 21 is open
Port 23 is open
Port 53 is open
...
````

## Pull-Requests! ##

This is an example how to use TCPSocket and Socket in Ruby and definitely can be improved. Pull requests are very much welcomed and desired. Don't be afraid. Just do it! :)


## Copyright & License ##

Copyright (C) 2017 Kirk Patrick - Released under the MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
