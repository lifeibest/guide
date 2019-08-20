# Mysql 设计规范

1、**数据库名字小写、单数、下划线，指定使用utf8mb4字符，名称和业务线或者产品线关联**

```
CREATE DATABASE dbname  DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_unicode_ci;
```

* 存储emoj表情，使用UTF8mb4

2、**表名称小写、单数、下划线，指定使用utf8mb4字符**

CREATE TABLE `post_comment` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `post_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '文章ID',
  `author` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '作者',
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '标题',
  `content` text COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '评论',
  `status` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '1审核中、 2审核通过、 3审核失败',
  `is_del` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否删除 1、是  0、否',
  `price` bigint(20) NOT NULL DEFAULT '0' COMMENT '阅读价格（精确到分）',
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  KEY `post_id` (`post_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='文章评论'

* Engine除非特殊要求，全部为 InnoDB，而非MyISAM
* 字段段名称小写、单数、下划线；禁止出现数字开头，禁止两个下划线中间出现数字
* 禁用保留字，如 order、desc、range、match、delayed 等
* 字段尽量使用 tinyint， 而非 int，并且要有默认值,如果没有负数，必须为unsigned
* 字段用中文注释、包括表名称注释
* 每个表带一个自增主键ID，其它业务可设置非自增或者bigint，尽量不使用int等
* 全部字段NOT NULL ，而非允许NULL,索引字段字段如果NULL,影响优化器对索引的选择,不能保证有值
* is_ 是否类字段，1代表真，0代表假，增加默认值
* 时间类为datetime
* 资金和钱相关的为bigint(20)，精确到分
* 使用tinyint来代替ENUM类型，将字符转化为数字
* 枚举使用 tinyint或者int 代替，从数字1开始
* 每个表自带3个字段，id、created_at、updated_at
* 说明:其中 id 必为主键，类型为 unsigned bigint、单表时自增、步长为 1;如果ID和业务相关，考虑生成不同格式，比如订单id，业务id等
* created_at、updated_at为时间类datetime
* 图片、文件等内容不允许直接存储在数据库，只存文件地址
* 用户密码等需加密后存储
* 同一字段在不同表设计的字段类型要一样

**3、其它说明**

* 存储过程尽量不用
* 触发器尽量不用
* sql语句避免使用临时表
* 禁止使用外键约束，在程序上面控制约束
* 临时表名必须以tmp为前缀，并以日期为后缀

**4、表数据一致性说明**

* 为了性能考虑，表中设计多余字段，同一个字段值在多张表存储，以空间换时间，这点和mysql本身的设计初衷有出入

**5、索引**

* 索引是双刃剑，会增加维护负担，增大IO压力，索引占用空间是成倍增加的,单张表的索引数量控制在5个以内，或不超过表字段个数的20%
* varchar类字段禁止创建索引（阿里云Mysql5.7不支持），可考虑用char(N)代替，N控制大小



