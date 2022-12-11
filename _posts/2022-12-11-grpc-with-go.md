---
title: GRPC with Go
date: 2022-12-11 18:00:00
categories: [architecture]
tags: [go, golang, grpc, microsservice]
---
# GRPC with Golang
One of the subjects that I like to study the most is the #microservices environment.

Recently at WE ARE META, I'm having the opportunity to migrate a project to this architecture and face all the challenges in this process.

One of these challenges is to understand the options of communication between services and choose the best one for the situation.

So, I took some time to study grpc, a framework created by Google that aims to turn communication easier independently of the programming language chosen.

But what is gRPC (Remote Procedure Call)? Basically, is when you have a client and a server and want the client to execute one function on the server. I created an example of communication between client-server using gRPC with Protocol Buffers.

Protocol Buffer is the format of the file that is sent between client-server. Protocol Buffers are binary, which makes them lighter and faster than JSON.

The project was made in golang and you can see it on my GitHub: https://github.com/eduardodecarvalho/grpc-project/tree/main