1、**减少与数据库交互的次数**

比如多次插入、更新的sql语句，不允许在程序里循环和数据库查询，而是先用程序拼接成一条sql，然后再数据库交互。

    错误：
    for () {
        query("insert into  `post_comment` ( `post_id`) values ( '1');");
        query("insert into  `post_comment` ( `post_id`) values ( '2');");
        query("insert into  `post_comment` ( `post_id`) values ( '3');");
    }

    正确：
    for(){
        //拼接sql
        sql = "insert into  `post_comment` ( `post_id`) values ( '1'),( '2'),( '3');" 
    }
    query(sql);

**2、减少查询结果集**

举个例子，查询用户手机号15012345678是否已经注册

    错误：
    SELECT * FROM `user` WHERE phone=15012345678;
    以上有三处错误，1、* 查询结果太广 2、phone 查询 没有加引号 3、没有limit 限制

    正确：
    SELECT id FROM `user` WHERE phone=`15012345678` LIMIT 1;

**3、如何使用子查询**

一般是避免子查询，但一定要使用子查询，需要分开查询

```
错误：
SELECT * from post where id IN (
SELECT post_id from  post_comment
)

正确：分两步查询
1、SELECT post_id from  post_comment，程序查询结果后，将post_id存一个变量中。
2、SELECT * from post where id IN(post_ids)

```

4、拒绝join联表

如何联表查询?

```
错误：
SELECT * FROM post left JOIN post_comment on post.id=post_comment.post_id

正确：
1、SELECT  * FROM post  结果集，取取对应关联ids
2、SELECT * FROM post_comment WHERE id IN (ids)，如果ids结果集过大，可分段再查询
```



