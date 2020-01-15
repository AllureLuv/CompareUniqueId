# 常见唯一id生成比较

# 原生Java
java原生提供uuid方法
Java UUID (Universally Unique Identifier) class is part of java.util package. Java UUID class represents an immutable universally unique identifier and represents 128-bit value. It is also known as GUID (Globally Unique Identifier)
参考rfc规范https://www.ietf.org/rfc/rfc4122.txt
• 时间戳＋UUID版本号： 分三段占16个字符(60bit+4bit)，
• Clock Sequence号与保留字段：占4个字符(13bit＋3bit)，
• 节点标识：占12个字符(48bit)，
例如  855450f9-4230-4254-959a-2b99d1e29da8
通常采用的算法包括
• version1: 基于时间的算法
• version4: 基于随机数的算法
在jdk中使用version4
/**
     * Static factory to retrieve a type 4 (pseudo randomly generated) UUID.
     *
     * The {@code UUID} is generated using a cryptographically strong pseudo
     * random number generator.
     *
     * @return  A randomly generated {@code UUID}
     */
    public static UUID randomUUID() {
        SecureRandom ng = Holder.numberGenerator;
        byte[] randomBytes = new byte[16];
        ng.nextBytes(randomBytes);
        randomBytes[6]  &= 0x0f;  /* clear version        */
        randomBytes[6]  |= 0x40;  /* set to version 4     */
        randomBytes[8]  &= 0x3f;  /* clear variant        */
        randomBytes[8]  |= 0x80;  /* set to IETF variant  */
        return new UUID(randomBytes);
    }
优点
本地生成，生成简单，性能好，没有高可用风险
缺点
长度过长，字母和数字组合，可读性差，查询效率低，没有顺序

# 数据库生成uuid
优点
简单
缺点
长度过长，字母和数字组合，可读性差，查询效率低，没有顺序 依赖于数据库

# 数据库生成uuid_short
优点
简单 有序 全局唯一
缺点
长度过长【可能超过long】， 依赖于数据库 ，依赖于server_id 

# 数据库自增id
优点
简单 有序
缺点
依赖于数据库 

# 雪花算法
优点
有序 性能高
缺点
依赖于时间协调 存在时钟回拨风险
