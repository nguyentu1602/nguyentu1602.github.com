---
layout: post
title: "Concurrency with inter process communication through sockets   Part 1"
description: ""
category:
tags: [concurrency, socket, cpp, tcp]
---
{% include JB/setup %}

Today I have the idea that I will build a concurency system to actually understand the concept. My project aims to build a client-server messaging system in which the server handles data-retrieval requests from multiple clients. The data will be cached in memory for better performance.

I read about conccurency in [A. Williams' book.] [conc_book_link] The book actually goes deep into threading, not inter-process communication, but he explains flavors of conccurency crystally clear, so I just gonna summary it below to remember the whole thing better. My idea of a messaging system emerges from Anthony's comment that such system is more apt for scalability in a multi-server setup. I then look into [the tutorial here][socket_guide_link] as a guide to implement my system using socket. Credits to both authors.

### What is conccurency?

Conccurency is just doing things at the same time. Multiple things. If the CPU physically has multi-cores, then we have **_hardware conccurency_**; if the CPU switches between tasks, then we have **_context switching_**. If the CPU has less physical cores than the number of tasks, then by pigeon hole principle, we encounter context switching.

### How to use conccurency in practice?

There're two main ways to do it. First way, you can use multiple processes and let them talk to each other; such talking is called "inter-process communication" (IPC). There are [several IPC channels: signals, sockets, files, pipe...][ipc_link], and I pick socket because it's convenient to learn how to communicate through network by sockets as a side product. Processes are well-guarded by the OS, so these IPCs in general are hard, slow, complicated compared to threading. Each process has quite a bit of overhead, so it takes time for them to start up and takes system resources to keep them alive. You may ask what is the upside then? Well, because it's hard, it probably weeds out shaky minds; Anthony also says that it allows (and forces) programmers to write safer concurrency code. Besides, because processes have their own internal resources, they can live on different machines and communicate through a messaging system. Quite scalable if you ask me.

The second way to apply conccurency uses multiple threads within a process. It will be summarized in a separate post.

### How to make processes talk to each other through sockets?
First thing to learn is the **_client-server_** model, which is the de-facto IPC model.

#### Client-server model
This model consists of two processes: the **_server_** and the **_client_**. The client connects to the server and requests information. The client must know the address of the server prior to making connection to the server; however, the server doesn't need to know anything about the client prior to the connection.

To connect, both processes need to construct their respective sockets. On the client side, you create socket by the **_socket()_** system call, then you call **_connect()_** with the address of the server. Once the connection is made, you **_read()_** from and **_write()_** to the server. On the server side, you also create a socket with **_socket()_**, then bind the socket to an address using the **_bind()_** system call. For a server socket on the Internet, an address consists of a port number on the host machine. Then you can listen for the connections with the **_listen()_** system call, and also **_accept()_** a connection.

#### What type of socket that we need to create?
This bit of [knowledge borrowed entirely here.] [socket_guide_link]
We must specify the address domain and the socket type when we make a socket. Two processes can communicate with each other only if their sockets are of the same type and on the same domain. The two main domains in practice are the Unix domain and the Internet domain. In the Unix domain, two processes share the same file system, so the address format is a character string representing an entry in the file system. In the Internet domain, two processes running on any two hosts can communicate, so the first part of the address of a socket in the Internet domain is the IP address of its host. The second part of a socket on the Internet is the port number on the host, which is a 16 bit unsigned intergers. Lower port numbers are reserved by Unix, but numbers > 2000 are generally available.

There are 2 commonly used socket types: stream socket and datagram socket. Stream sockets treat communication as a continuous stream of characters, while datagram sockets have to read entire messages at once. Each use its own protocol; stream sockets use TCP (Transmisson Control Protocol) and generally are more reliable, so we will use it.

To be continued...

[conc_book_link]: https://www.manning.com/books/c-plus-plus-concurrency-in-action
[socket_guide_link]: https://www.linuxhowtos.org/C_C++/socket.htm
[ipc_link]: https://en.wikipedia.org/wiki/inter-process_communication