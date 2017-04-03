---
category: Database
path: '/Database'
title: 'Mysql分解关联查询'
type: 'Mysql'

layout: nil
---

    很多高性能的应用都会对关联查询进行分解。简单的说，可以对每一个表进行一次单表查询，然后将结果在应用程序中进行关联。
例如下面这个查询

select * from tag   
      join tag_post on tag_post.tag_id=tag.id  
      join post on tag_post.post_id=post.id  
      where tag.tag='mysql'  

可以分解成下面这些查询来代替

select * from tag where tag='mysql';  
  
select * from tag_post where tag_id = 1234;  
  
select * from post where post.id in (123,456,789,369); 

到底为什么要这样做？乍一看，这样做并没有什么好处，原本一条查询，这里变成多条查询，返回的结果又是一模一样的。事实上，用分解关联查询的方式重构查询有如下优势：
（1）让缓存的效率更高。许多应用程序可以方便的缓存单表查询对应的结果对象。例如，上面查询中的tag已经被缓存了，那么应用就可以跳过第一个查询。再例如，应用中已经缓存了ID为123、789的内容，那么第三个查询的in()
中就可以少几个ID。另外，对mysql的缓存来说，如果关联中的摸个表发生了变化，那么久无法使用查询缓存了，而拆分后，如果摸个表很少改变，那么基于该表的查询就可以重复利用查询缓存结果了
（2）将查询分解后，执行单个查询可以减少锁的竞争。
（3）在应用层做关联，可以更容易对数据库进行拆分，更容易做到高性能和可扩展。
（4）查询本身效率也可能会有所提升。这个例子中，使用in()代替关联查询，可以让msyql按照ID顺序进行查询，这可能比随机的关联要更高效。
（5）可以减少冗余记录的查询。在应用层做关联查询，意味着对应某条记录应用只需要查询一次，而在数据库中做关联查询，则可能需要重复的访问一部分数据。从这点看，这样的重构还可能会减少网络和内存的消耗。
（6）这样做相当于在应用中实现了哈希关联，而不是使用MySQL的嵌套循环关联。某些场景哈希关联的效率要高很多。

### Request

* The headers must include a **valid authentication token**.

### Response

Sends back a collection of things.

```Status: 200 OK```
```{
    {
        id: thing_1,
        name: 'My first thing'
    },
    {
        id: thing_2,
        name: 'My second thing'
    }
}```

For errors responses, see the [response status codes documentation](#response-status-codes).