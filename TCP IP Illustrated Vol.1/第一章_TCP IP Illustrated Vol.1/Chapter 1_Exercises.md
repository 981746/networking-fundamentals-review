Chapter 1
 1.1 Calculate the maximum number of class A, B, and C network IDs.
 A:

*A類網路地址

0 [ 7bits netid ][ 24bits hostid ]
2^7 = 128


128 - 2 = 126（去掉開頭0和127這兩個保留地址）。



*B類網路地址

Class B
開頭填10
10 [ 14bits netid ][ 16bits ]

2^14 = 16,384
16,384 -2 = 16,382（去掉開頭0和127這兩個保留地址）。


*C類網路地址
110 [ 21bits netid ][ 8bits hostid ]

2^21 = 2,097,152 -2 = 2,097,150（去掉開頭0和127這兩個保留地址）。


所以A + B + C 網路地址的數量
126 + 16,382 + 2,097,150 = 2,113,658

