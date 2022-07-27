# RDMA exmaple

A simple RDMA server client example. The code contains a lot of comments. Here is the workflow that happens in the example: 

Client: 
  1. setup RDMA resources   
  2. connect to the server 
  3. receive server side buffer information via send/recv exchange 
  4. do an RDMA write to the server buffer from a (first) local buffer. The content of the buffer is the string passed with the `-s` argument. 
  5. do an RDMA read to read the content of the server buffer into a second local buffer. 
  6. compare the content of the first and second buffers, and match them. 
  7. disconnect 

Server: 
  1. setup RDMA resources 
  2. wait for a client to connect 
  3. allocate and pin a server buffer
  4. accept the incoming client connection 
  5. send information about the local server buffer to the client 
  6. wait for disconnect

###### How to run      
```text
git clone https://github.com/animeshtrivedi/rdma-example.git
cd ./rdma-example
cmake .
make
``` 
 
###### server
```text
./bin/rdma_server -a 10.1.0.6
```
###### client
```text
inho@nsl-node3:~/ndsmr/rdma-example$ ./bin/rdma_client -a 10.1.0.6 -s textstring
Passed string is : textstring , with count 10
Trying to connect to server at : 10.1.0.6 port: 20886
The client is connected successfully
---------------------------------------------------------
buffer attr, addr: 0x557bfccd0c90 , len: 10 , stag : 0x4bcdb
---------------------------------------------------------
...
SUCCESS, source and destination buffers match
Client resource clean up is complete
inho@nsl-node3:~/ndsmr/rdma-example$

```
###### server
```text
inho@nsl-node4:~/ndsmr/rdma-example$ ./bin/rdma_server -a 10.1.0.6
Server is listening successfully at: 10.1.0.6 , port: 20886
A new connection is accepted from 10.1.0.5
Client side buffer information is received...
---------------------------------------------------------
buffer attr, addr: 0x563c89388330 , len: 10 , stag : 0x54af6
---------------------------------------------------------
The client has requested buffer length of : 10 bytes
A disconnect event is received from the client...
Server shut-down is complete
inho@nsl-node4:~/ndsmr/rdma-example$
```


## Does not have an RDMA device?
In case you do not have an RDMA device to test the code, you can setup SofitWARP software RDMA device on your Linux machine. Follow instructions here: [https://github.com/animeshtrivedi/blog/blob/master/post/2019-06-26-siw.md](https://github.com/animeshtrivedi/blog/blob/master/post/2019-06-26-siw.md).
