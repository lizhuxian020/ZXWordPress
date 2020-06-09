# DES 数据加密标准
1. Data Encryption Standard
2. 64位(8字节)为一组, 进行分组加密. 秘钥也是64位的, 但实际有效只有56位, 其他事校验位

## MODE(分组处理模式)
1. 常用的mode有ECB（Electronic Codebook, 电码本）, CBC(Cipher-block chaining, 密码分组链接). 还有其他模式, 不记了.
2. ECB是把所有分组都用秘钥直接进行DES加密, 得到一段段密文后拼接起来.
3. CBC是把第一组数据与初始化向量进行DES加密后, 得到密文再与第二组数据进行DES加密, 得到密文再与第三组数据进行DES加密

### 两种模式的优缺点
1. ECB比较简单, 可以并行计算. 容易被攻击
2. CBC安全性胜于ECB, 不能并行计算, 需要初始化向量IV

## Padding(填充)
1. 当明文最后一组不足8字节时, 根据padding模式进行填充: Pkcs5 | Pkcs7