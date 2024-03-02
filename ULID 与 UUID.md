# ULID：Universally Unique Lexicographically Sortable Identifier（通用唯一词典分类标识符）
# UUID：Universally Unique Identifier（通用唯一标识符）
# 为什么不选择UUID

## UUID 目前有 5 个版本：
* 版本1：在许多环境中是不切实际的，因为它需要访问唯一的，稳定的MAC地址，容易被攻击；
* 版本2：将版本 1 的时间戳前四位换为 POSIX 的 UID 或 GID，问题同上；
* 版本3：基于 MD5 哈希算法生成，生成随机分布的ID需要唯一的种子，这可能导致许多数据结构碎片化；
* 版本4：基于随机数或伪随机数生成，除了随机性外没有提供其他信息；
* 版本5：通过 SHA-1 哈希算法生成，生成随机分布的ID需要唯一的种子，这可能导致许多数据结构碎片化；
 这里面常用的就是 UUID4 了，但是，即使是随机的，但是也是存在冲突的风险。
 和 UUID 要么基于随机数，要么基于时间戳不同，ULID 是既基于时间戳又基于随机数，时间戳精确到毫秒，毫秒内有1.21e + 24个随机数，不存在冲突的风险，而且转换成字符串比 UUID 更加友好
## ULID特性：
* 与UUID的128位兼容性
* 每毫秒1.21e + 24个唯一ULID
* 按字典顺序(也就是字母顺序)排序！
* 规范地编码为26个字符串，而不是UUID的36个字符
* 使用Crockford的base32获得更好的效率和可读性（每个字符5位）
* 不区分大小写
* 没有特殊字符（URL安全）
* 单调排序顺序（正确检测并处理相同的毫秒）

## ULID规范
```
以下是在python(ulid-py)中实现的ULID的当前规范。二进制格式已实现
01AN4Z07BY      79KA1307SR9X4MV3

|----------|    |----------------|
 Timestamp          Randomness
  10chars            16chars
   48bits             80bits
```
### 组成
#### 时间戳
* 48位整数
* UNIX时间（以毫秒为单位）
* 直到公元10889年，空间都不会耗尽。

#### 随机性
* 80位随机数
* 如果可能的话，采用加密技术保证随机性

#### 排序

最左边的字符必须排在最前面，最右边的字符必须排在最后（词汇顺序）。
必须使用默认的ASCII字符集。在同一毫秒内，不能保证排序顺序

#### 编码方式
如图所示，使用了Crockford的Base32。该字母表不包括字母I，L，O和U，以避免混淆和滥用。

#### 二进制布局和字节顺序
组件被编码为16个八位位组。每个组件都以最高有效字节在前（网络字节顺序）进行编码。
```
0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      32_bit_uint_time_high                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     16_bit_uint_time_low      |       16_bit_uint_random      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       32_bit_uint_random                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       32_bit_uint_random                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```
#### 应用场景
* 替换数据库自增id，无需DB参与主键生成
* 分布式环境下，替换UUID，全局唯一且毫秒精度有序
* 比如要按日期对数据库进行分区分表，可以使用ULID中嵌入的时间戳来选择正确的分区分表
* 如果毫秒精度是可以接受的（毫秒内无序），可以按照ULID排序，而不是单独的created_at字段

### 用法

#### python
```
pip install ulid-py #安装

#创建一个全新的ULID。
#时间戳记值（48位）来自 time.time()，精度为毫秒。
#随机值（80位）来自 os.urandom()。
import ulid
ulid.new()

#根据现有的128位值（例如UUID）创建新的ULID 。
#支持ULID值类型有 int，bytes，str，和UUID。

import ulid, uuid
value = uuid.uuid4()
value
ulid.from_uuid(value)


从现有时间戳值（例如datetime对象）创建新的ULID 。
支持时间戳值类型有int，float，str，bytes，bytearray，memoryview，datetime，Timestamp，和ULID

import datetime, ulid
ulid.from_timestamp(datetime.datetime(1999, 1, 1))


根据现有的随机数创建一个新的ULID。
支持随机值类型有int，float，str，bytes，bytearray，memoryview，Randomness，和ULID。
import os, ulid
randomness = os.urandom(10)
ulid.from_randomness(randomness)

timestamp()方法将为您提供ULID的前48位的时间戳快照，而randomness()方法将为您提供后80位的随机数快照。
import ulid
u = ulid.new()
u.timestamp()  # 前48位的时间戳快照
u.randomness() # 后80位的随机数快照。

```

