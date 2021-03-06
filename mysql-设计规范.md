# Mysql 设计规范

**1、数据库规范**

```
CREATE DATABASE dbname  DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_unicode_ci;
```

* 统一字符集utf8mb4，因需存储emoj表情，使用utf8mb4
* 名称小写、单数、下划线，名称和业务线或者产品线关联
* 数据库配置sql\_mode默认配置使用NO\_AUTO\_CREATE\_USER,NO\_ENGINE\_SUBSTITUTION：删除~~STRICT\_TRANS\_TABLES~~，去掉严格校验，如果程序中有相关全局配置也删除；删除~~NO\_ZERO\_DATE~~，日期类可以插入0日期；

**2、数据表规范**

    CREATE TABLE `post_comment` (
      `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
      `user_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '用户id',
      `post_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '文章ID',
      `author` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '作者',
      `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '标题',
      `content` text COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '评论',
      `status` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '审核状态：1、审核中， 2、审核通过，3、审核失败',
      `is_del` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '是否删除 1、是  0、否',
      `price` bigint(20) NOT NULL DEFAULT '0' COMMENT '阅读价格（精确到分）',
      `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
      `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
      `push_date` datetime DEFAULT NULL COMMENT '发布日期',
      PRIMARY KEY (`id`),
      KEY `post_id` (`post_id`),
      KEY `user_id` (`user_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='文章评论'

* Engine除非特殊要求，全部为 InnoDB，而非MyISAM
* 字段段名称小写、单数、下划线；禁止出现数字开头，禁止两个下划线中间出现数字

* 字符：CHARSET=utf8mb4 COLLATE=utf8mb4\_unicode\_ci

* 表名称：小写、单数、下划线

* 字段用中文注释、包括表名称注释

* 字段段名称小写、单数、下划线；禁止出现数字开头，禁止两个下划线中间只 出现数字

* 禁用保留字，如 order、desc、range、match、delayed 等

* 字段尽量使用 tinyint， 而非 int，并且要有默认值,如果没有负数，必须为unsigned
* 字段用中文注释、包括表名称注释
* 每个表带一个自增主键ID，其它业务可设置非自增或者bigint，尽量不使用int等

* 每个表带一个自增主键ID，其它业务可设置非自增或者bigint等

* 全部字段NOT NULL ，而非允许NULL,索引字段字段如果NULL,影响优化器对索引的选择,不能保证有值

* is\_ 是否类字段，1代表真，0代表假，增加默认值

* 时间类为datetime，毫秒时间单位6位datetime\(6\)，如果可以为空，默认值NULL

* 资金和钱相关的为bigint\(20\)，精确到分

* 使用tinyint来代替ENUM类型，将字符转化为数字

* 枚举使用 tinyint或者int 代替，从数字1开始
* 每个表自带3个字段，id、created_at、updated_at
* 说明:其中 id 必为主键，类型为 unsigned bigint、单表时自增、步长为 1;如果ID和业务相关，考虑生成不同格式，比如订单id，业务id等
* created_at、updated_at为时间类datetime

* 每个表自带3个字段，id、created\_at、updated\_at

* 说明:其中 id 必为主键，类型为 unsigned bigint、单表时自增、步长为 1;created\_at、updated\_at为时间类datetime

* 类型用户名有唯一索引要求的，必须添加唯一索引

**3、其它规范**

* 图片、文件等内容不允许直接存储在数据库，只存文件地址

* 用户密码等敏感信息需加密后存储

* 同一字段在不同表设计的字段类型要一样

* 禁止在前端业务需求中使用存储过程，临时工具，解决临时需求可使用存储过程

* 触发器尽量不用

* sql语句避免使用临时表，临时表名必须以tmp为前缀，并以日期为后缀

* 禁止使用外键约束，在程序上面控制约束

**4、表数据一致性说明**

* 为了性能考虑，表中设计多余字段，同一个字段值在多张表存储，以空间换时间，这点和mysql本身的设计初衷有出入

**5、索引**

* 索引是双刃剑，会增加维护负担，增大IO压力，索引占用空间是成倍增加的,单张表的索引数量控制在5个以内，或不超过表字段个数的20%
* varchar类字段禁止创建索引（阿里云Mysql5.7不支持），可考虑用char(N)代替，N控制大小



