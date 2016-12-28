# Mysql 设计规范

1、**数据库名字小写、单数、下划线，指定使用utf8字符**

```
CREATE DATABASE dbname  DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
```

* 存储emoj表情，使用UTF8mb4

2、**表名称小写、单数、下划线，指定使用utf8字符**

    CREATE TABLE `post_comment` (
      `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
      `post_id` int(11) unsigned NOT NULL COMMENT '文章ID',
      `author` varchar(255) NOT NULL COMMENT '作者',
      `title` varchar(255) NOT NULL COMMENT '标题',
      `content` text NOT NULL COMMENT '评论',
      `status` tinyint(11) NOT NULL DEFAULT '1' COMMENT '1审核、 2审核通过、 3审核失败',
      `create_at` datetime NOT NULL COMMENT '创建时间',
      `update_at` datetime NOT NULL COMMENT '更新时间',
      PRIMARY KEY (`id`),
      KEY `post_id` (`post_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='文章评论'

* 字段段名称小写、单数、下划线
* 字段尽量使用 tinyint， 而非 int，并且要有默认值
* 字段用中文注释、包括表名称注释
* 每个表带一个自增主键ID，其它业务可设置非自增或者bigint等
* 全部字段NOT NULL ，而非允许NULL,索引字段字段如果NULL,影响优化器对索引的选择,不能保证有值
* Engine除非特殊要求，全部为 InnoDB，而非MyISAM
* 时间类尽量使用int类型，业务要求可设置为datetime或者TIMESTAMP等
* 使用TINYINT来代替ENUM类型，将字符转化为数字
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



