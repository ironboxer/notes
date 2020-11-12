
```shell
127.0.0.1:6379> xadd myqueue * key value1
"1583336718220-0"
127.0.0.1:6379> xadd myqueue * key value1
"1583336725558-0"
127.0.0.1:6379> del myqueue
(integer) 1
127.0.0.1:6379> xadd myqueue * key value1
"1583336733070-0"
127.0.0.1:6379> xadd myqueue * key value2
"1583336736278-0"
127.0.0.1:6379> xadd myqueue * key value3
"1583336740230-0"
127.0.0.1:6379> xadd myqueue * key value4
"1583336746332-0"
127.0.0.1:6379> xadd myqueue * key value5
"1583336749703-0"
127.0.0.1:6379> xlen myqueue
(integer) 5
127.0.0.1:6379> xrange myqueue - +
1) 1) "1583336733070-0"
   2) 1) "key"
      2) "value1"
2) 1) "1583336736278-0"
   2) 1) "key"
      2) "value2"
3) 1) "1583336740230-0"
   2) 1) "key"
      2) "value3"
4) 1) "1583336746332-0"
   2) 1) "key"
      2) "value4"
5) 1) "1583336749703-0"
   2) 1) "key"
      2) "value5"
127.0.0.1:6379> xgroup create myqueue group1 0-0
OK
127.0.0.1:6379> xgroup create myqueue group2 0-0
OK
127.0.0.1:6379> xinfo stream myqueue
 1) "length"
 2) (integer) 5
 3) "radix-tree-keys"
 4) (integer) 1
 5) "radix-tree-nodes"
 6) (integer) 2
 7) "groups"
 8) (integer) 2
 9) "last-generated-id"
10) "1583336749703-0"
11) "first-entry"
12) 1) "1583336733070-0"
    2) 1) "key"
       2) "value1"
13) "last-entry"
14) 1) "1583336749703-0"
    2) 1) "key"
       2) "value5"
127.0.0.1:6379> xreadgroup group  group1 consumer1 count 4 streams myqueue 0-0
1) 1) "myqueue"
   2) (empty list or set)
127.0.0.1:6379> xinfo groups myqueue
1) 1) "name"
   2) "group1"
   3) "consumers"
   4) (integer) 1
   5) "pending"
   6) (integer) 0
   7) "last-delivered-id"
   8) "0-0"
2) 1) "name"
   2) "group2"
   3) "consumers"
   4) (integer) 0
   5) "pending"
   6) (integer) 0
   7) "last-delivered-id"
   8) "0-0"
127.0.0.1:6379> xreadgroup group group1 consumer1 count 4 streams myqueue >
1) 1) "myqueue"
   2) 1) 1) "1583336733070-0"
         2) 1) "key"
            2) "value1"
      2) 1) "1583336736278-0"
         2) 1) "key"
            2) "value2"
      3) 1) "1583336740230-0"
         2) 1) "key"
            2) "value3"
      4) 1) "1583336746332-0"
         2) 1) "key"
            2) "value4"
127.0.0.1:6379> xreadgroup group group2 consumer3 count 4 streams myqueue >
1) 1) "myqueue"
   2) 1) 1) "1583336733070-0"
         2) 1) "key"
            2) "value1"
      2) 1) "1583336736278-0"
         2) 1) "key"
            2) "value2"
      3) 1) "1583336740230-0"
         2) 1) "key"
            2) "value3"
      4) 1) "1583336746332-0"
         2) 1) "key"
            2) "value4"
127.0.0.1:6379> xreadgroup group group1 consumer2 count 4 streams myqueue >
1) 1) "myqueue"
   2) 1) 1) "1583336749703-0"
         2) 1) "key"
            2) "value5"
127.0.0.1:6379> xreadgroup group group2 consumer4 count 4 streams myqueue >
1) 1) "myqueue"
   2) 1) 1) "1583336749703-0"
         2) 1) "key"
            2) "value5"
127.0.0.1:6379>

127.0.0.1:6379> xreadgroup group group1 consumer1 count 4 streams myqueue 0-0
1) 1) "myqueue"
   2) 1) 1) "1583336733070-0"
         2) 1) "key"
            2) "value1"
      2) 1) "1583336736278-0"
         2) 1) "key"
            2) "value2"
      3) 1) "1583336740230-0"
         2) 1) "key"
            2) "value3"
      4) 1) "1583336746332-0"
         2) 1) "key"
            2) "value4"
127.0.0.1:6379> xack myqueue group1 1583336733070-0  1583336736278-0 1583336740230-0 1583336746332-0
(integer) 4

127.0.0.1:6379> xinfo groups myqueue
1) 1) "name"
   2) "group1"
   3) "consumers"
   4) (integer) 2
   5) "pending"
   6) (integer) 1
   7) "last-delivered-id"
   8) "1583336749703-0"
2) 1) "name"
   2) "group2"
   3) "consumers"
   4) (integer) 3
   5) "pending"
   6) (integer) 5
   7) "last-delivered-id"
   8) "1583336749703-0"

127.0.0.1:6379> xinfo consumers myqueue group2
1) 1) "name"
   2) "consumer1"
   3) "pending"
   4) (integer) 0
   5) "idle"
   6) (integer) 433238
2) 1) "name"
   2) "consumer3"
   3) "pending"
   4) (integer) 4
   5) "idle"
   6) (integer) 421684
3) 1) "name"
   2) "consumer4"
   3) "pending"
   4) (integer) 1
   5) "idle"
   6) (integer) 442236


127.0.0.1:6379> XREADGROUP group group2 consumer3 count 4 streams myqueue 0-0
1) 1) "myqueue"
   2) 1) 1) "1583336733070-0"
         2) 1) "key"
            2) "value1"
      2) 1) "1583336736278-0"
         2) 1) "key"
            2) "value2"
      3) 1) "1583336740230-0"
         2) 1) "key"
            2) "value3"
      4) 1) "1583336746332-0"
         2) 1) "key"
            2) "value4"
127.0.0.1:6379> xack myqueue group2 1583336733070-0 1583336736278-0 1583336740230-0 1583336746332-0
(integer) 4

127.0.0.1:6379> xinfo consumers myqueue group2
1) 1) "name"
   2) "consumer1"
   3) "pending"
   4) (integer) 0
   5) "idle"
   6) (integer) 552122
2) 1) "name"
   2) "consumer3"
   3) "pending"
   4) (integer) 0
   5) "idle"
   6) (integer) 48202
3) 1) "name"
   2) "consumer4"
   3) "pending"
   4) (integer) 1
   5) "idle"
   6) (integer) 561120

```
