# Mysql 设计规范

1、**数据库名字小写、单数、下划线，指定使用utf8字符**

```
CREATE DATABASE dbname  DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
```

2、**表名称小写、单数、下划线，指定使用utf8字符**

    CREATE TABLE `post_comment` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `post_id` int(11) NOT NULL COMMENT '文章ID',
      `author` varchar(255) NOT NULL COMMENT '作者',
      `title` varchar(255) NOT NULL COMMENT '标题',
      `content` text NOT NULL COMMENT '评论',
      `status` tinyint(11) NOT NULL DEFAULT '1' COMMENT '1审核、 2审核通过、 3审核失败',
      `create_at` datetime NOT NULL COMMENT '创建时间',
      `update_at` datetime NOT NULL COMMENT '更新时间',
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='文章评论'

* 字段尽量使用 tinyint， 而非 int
* 字段用中文注释、包括表名称注释
* 每个表带一个自增主键ID，其它业务可设置非自增或者bigint等
* 全部NOT NULL ，而非允许NULL
* Engine除非特殊要求，全部为 InnoDB，而非MyISAM
* 时间类尽量使用int类型，业务要求可设置为datetime等



