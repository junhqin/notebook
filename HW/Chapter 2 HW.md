## Chapter 2 HW

> 秦君豪 10204804421

#### **P2. Consider an HTTP client that wants to retrieve a Web document at a given URL. The IP address of the HTTP server is initially unknown. What transport and application-layer protocols besides HTTP are needed in this scenario?**

1、域名系统协议：DNS用于将URL解析为IP地址

2、传输控制协议：HTTP是建立在TCP之上的，于客户端及服务端建立一个可靠的链接通道。



#### P8. Suppose within your Web browser you click on a link to obtain a Web page. The IP address for the associated URL is not cached in your local host, so a DNS lookup is necessary to obtain the IP address. Suppose that n DNS servers are visited before your host receives the IP address from DNS; the successive visits incur an RTT of RTT1, . . ., RTTn. Further suppose that the Web page associated with the link contains exactly one object, consisting of a small amount of HTML text. Let RTT0 denote the RTT between the local host and the server containing the object. Assuming zero transmission time of the object, how much time elapses from when the client clicks on the link until the client receives the object?**

在DNS查询IP地址时，花费时间T1= RTT1+RTT2+.....+RTTn.在获得IP地址后，建立HTTP连接以
及发送物品，花费的时间为T2=RTT0*2.最终花费时间为：T= RTT1+RTT2+.....+RTTn+2 * RTT0.



#### **P9. Referring to Problem P8, suppose the HTML file references three very small objects on the same server. Neglecting transmission times, how much time elapses with a. Non-persistent HTTP with parallel connections? b. Non-persistent HTTP with no parallel TCP connections? c. Persistent HTTP?**

a. Non-persistent http+parallell connection,包含了并行的三个TCP连接和请求传输文件 Ta=
RTT1+RTT2+.....+RTTn+2 * RTT0.
b. Non-persistent http+no parallell connection,包含了三次的TCP连接和请求传输文件 Ta=
RTT1+RTT2+.....+RTTn+6 * RTT0.
a. persistent http,包含了一个TCP连接和三次请求传输文件 Ta= RTT1+RTT2+.....+RTTn +4 * RTT0.



#### **P11. What is the difference between MAIL FROM: in SMTP and From: in the mail message itself?**

`MAIL FROM:` 命令是SMTP对话的一部分，用于在发送邮件时告知邮件服务器发件人的地址。这是SMTP协议中定义的一个命令，用于在邮件传输开始时指定发信人。

` FROM`:RFC 5332中定义的邮件首部行内容信息，向收件人告知了寄信人是谁。





#### **P15. Consider accessing your e-mail with POP3.**

- **a. Suppose you have configured your POP mail client to operate in the download-and-keep mode. Complete the following transaction:**

以下是完整的transaction：

```
C: list
S: 1 498 
S: 2 912 
S: . 
C: retr 1 
S: blah blah ... 
S: ..........blah 
S: . 
C： retr 2
S: blah blah ... 
S: ..........blah 
S: . 
C：quit
S:+OK pop3 server signing off
```



- **b. Suppose you have configured your POP mail client to operate in the download-and-delete mode. Complete the following transaction:**

  

  ```
  S: ..........blah 
  S: .
  C: dele 1
  C： retr 2
  S: blah blah ... 
  S: ..........blah 
  S: . 
  C：dele 2
  C： quit
  S：S:+OK pop3 server signing off
  
  ```

  

- **c.  Suppose you have configured your POP mail client to operate in the download-and-keep mode. Using your transcript in part (b), suppose you retrieve messages 1 and 2, exit POP, and then five minutes later you again access POP to retrieve new e-mail. Suppose that in the five-minute interval no new messages have been sent to you. Provide a transcript of this second POP session.**

```
C: list
S: +OK 0 messages
S: .
C: quit
S: +OK POP3 server signing off
```



#### **P23. Consider query flooding, as discussed in Section 2.6. Suppose that each peer is connected to at most N neighbors in the overlay network. Also suppose that the node-count field is initially set to K. Suppose Alice makes a query. Find an upper bound on the number of query messages that are sent into the overlay network**.



对于每一层的查询，第一层Alice发给了她的n个邻居，然后第二层n个邻居又发给了他们的邻居，假设这n个邻居互不为邻居，则他们各发送了（n-1）个人，共n*(n-1)个消息，按无重复邻居的假设类推，第k层有 n * (n-1)k-1 个消息。
所以最大消息总数为 $n+n*\sum^{k-1}_{i=1}{(n-1)^i}$