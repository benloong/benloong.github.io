FlatBuffers Internal Notes
=================

##Flat Data

所有数据都以二进制存储在字节数组中 读取时拷贝

Table创建后不能动态改变

Scalar变量可以改变

默认值不存储

offset 4 bytes `int32_t`

bottom-up方向创建

`[rootOffset|bytes...]`

###Table
`[vtable|voffset|scalar member and reference offset]`

`vtable`

`[len|offset0|offset1|offset2|....]`

offset 2 bytes `int16_t`
vtable存储 Table成员变量相对Table pivot的偏移

###Struct
`[scalar member]`

###List
`[len|data0|data1|data2...]`

###String
allways zero terminate utf-8 encoding

`[len|utf-8 bytes]`

