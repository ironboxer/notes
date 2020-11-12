### TCP_NOPUSH



Enables or disables the TCP_NOPUSH (FreeBSD) or TCP_CORK (Linux) socket option. Note that this option only applies if the sendfile directive is enabled. If tcp_nopush is set to on, Nginx will attempt to transmit the entire HTTP response headers in a single TCP packet.

Syntax: on or off

Default value: off

https://www.oreilly.com/library/view/nginx-http-server/9781788623551/abac29fd-7d75-4aed-a970-58af05dd8fe8.xhtml



---



一篇更加详细的讨论

https://thoughts.t37.net/nginx-optimization-understanding-sendfile-tcp-nodelay-and-tcp-nopush-c55cdd276765





###  send_file + tcp_nopush + tcp_nodelay

传输文件的时候, 设置tcp_nopush，每次发送的数据都是尽可能大的，以减少传输的次数。在传最后一块数据的时候, 设置为tcp_nodelay, 禁用nagle算法, 尽快将剩余的数据发送过去, 节省掉200ms的超时后必须发送的时间。

