# 网络进程之间通信的方式
在网络中标识一个进程：网络层ip确定主机位置，传输层协议+端口确定唯一的进程    

使用TCP/IP协议的应用程序编程接口通常采用socket

# 什么是socket？
socket是一种特殊的文件，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作。

# socket基本操作
socket是在网络通信中实现"open--read/write--close"过程的文件，因此需要一些函数来帮助实现这一功能
## socket()函数
`int socket(int domain, int type, int protocol); `   
分别确定协议族、socket类型，协议

## bind()函数
` int bind(int sockfd, const struct sockaddr \*addr, socklen_t addrlen); `    

将ip和端口分配给socket   

**sockfd**：即socket描述字，它是通过socket()函数创建了，唯一标识一个socket。bind()函数就是将给这个描述字绑定一个名字。  
**addr**：一个const struct sockaddr* 指针，指向要绑定给sockfd的协议地址。这个地址结构根据地址创建socket时的地址协议族的不同而不同，
如ipv4对应的是：
```
struct sockaddr_in {
    sa_family_t sin_family; /* address family: AF_INET */ 
    in_port_t sin_port; /* port in network byte order */ 
    struct in_addr sin_addr; /* internet address */
}; /* Internet address. */ 
struct in_addr { uint32_t s_addr; /* address in network byte order */ };
```
ipv6对应的是：
```
struct sockaddr_in6 {
    sa_family_t sin6_family; /* AF_INET6 */
    in_port_t sin6_port; /* port number */
    uint32_t sin6_flowinfo; /* IPv6 flow information */
    struct in6_addr sin6_addr; /* IPv6 address */
    uint32_t sin6_scope_id; /* Scope ID (new in 2.4) */
};
struct in6_addr { unsigned char s6_addr[16]; /* IPv6 address */ };
```
Unix域对应的是：
```
#define UNIX_PATH_MAX 108 struct sockaddr_un {
sa_family_t sun_family; /* AF_UNIX */
char sun_path[UNIX_PATH_MAX]; /* pathname */ };
```
**addrlen**:地址长度
## listen(),connect()函数
完成socket()，bind()后，服务器会监听这个socket，客户端调用connect会发送连接请求，服务端会受到这个请求。
```
int listen(int sockfd, int backlog);
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```
listen函数的第一个参数即为要监听的socket描述字，第二个参数为相应socket可以排队的最大连接个数。socket()函数创建的socket默认是一个主动类型的，listen函数将socket变为被动类型的，等待客户的连接请求。    

connect函数的第一个参数即为客户端的socket描述字，第二参数为服务器的socket地址，第三个参数为socket地址的长度。客户端通过调用connect函数来建立与TCP服务器的连接。

## accept()函数

 `int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);`
 accept函数的第一个参数为服务器的socket描述字，第二个参数为指向struct sockaddr *的指针，用于返回客户端的协议地址，第三个参数为协议地址的长度。如果accpet成功，那么其返回值是由内核自动生成的一个全新的描述字，代表与返回客户的TCP连接。
## read(),write()函数

## close()函数
