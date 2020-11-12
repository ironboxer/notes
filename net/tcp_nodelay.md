### TCP_NODELAY VS TCP_CORK



Nagle's algorithm is for reducing more number of small network packets in wire. The algorithm is: if data is smaller than a limit (usually MSS), wait until receiving ACK for previously sent packets and in the mean time accumulate data from user. Then send the accumulated data.

```
if [ data > MSS ]
    send(data)
else
    wait until ACK for previously sent data and accumulate data in send buffer (data)
    And after receiving the ACK send(data)
```

This will help in applications like telnet. However, waiting for the ACK may increase latency when sending streaming data. Additionally, if the receiver implements the 'delayed ACK policy', it will cause a temporary deadlock situation. In such cases, disabling Nagle's algorithm is a better option.

So **TCP_NODELAY** is used for disabling Nagle's algorithm.

**TCP_CORK** aggressively accumulates data. If TCP_CORK is enabled in a socket, it will not send data until the buffer fills to a fixed limit. Similar to Nagle's algorithm, it also accumulates data from user but until the buffer fills to a fixed limit not until receiving ACK. This will be useful while sending multiple blocks of data. But you have to be more careful while using TCP_CORK.

Until 2.6 kernel, both of these options are mutually exclusive. But in later kernel, both of them can exist together. In such case, TCP_CORK will be given more preference.

https://stackoverflow.com/questions/3761276/when-should-i-use-tcp-nodelay-and-when-tcp-cork