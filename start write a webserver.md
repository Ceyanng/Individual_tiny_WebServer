# what a webserver?
webserver就是在服务器上运行的一个软件，软件的功能是与通过HTTP协议与客户端（通常是浏览器（Browser））进行通信，来接收，存储，处理来自客户端的HTTP请求，并对其请求做出HTTP响应，返回给客户端其请求的内容（文件、网页等）或返回一个Error信息

# webserver如何与客户端通信
客户端通过在浏览器上键入域名或ip：端口的方式，向webserver服务器发送一个http请求，这一过程通过tcp协议的三次握手与webserver建立连接，然后HTTP协议生成针对目标Web服务器的HTTP请求报文，通过TCP、IP等协议发送到目标Web服务器上  

在同一个时刻，同一ip服务器的一个端口只能绑定一个TCP连接的进程，但是可以绑定一个tcp和一个udp连接的两个进程，这是因为TCP，UDP协议通过五元组{传输协议，源ip，目的ip，源端口，目的端口}区分接收者
只有一种情况存在一个端口被多个进程绑定，就是一个进程先绑定一个端口，再fork一个子进程，这样子进程也与这个端口号绑定

# webserver如何接受客户端发送的http请求报文？
webserver通过`socket` 监听来自用户的请求。   
