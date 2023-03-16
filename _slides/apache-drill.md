---
marp: true
class:
  - lead
  - invert
---

# Apache Drill: A Powerful Data Analysis Tool

## Querying Multiple Data Sources
## Schema-Free Data Analysis
## High Performance Querying



---
# Launch drill-embedded

![-](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/5e/5e61edcac5d588cca0c68165e04435ac.png)

type `!quit` to exit

---
# Web console

[http://localhost:8047/query](http://localhost:8047/query)


---
### Various storage 
![w:900 h:580](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/e9/e9f0b887a25d5ee8b4701873003d051b.png)

---
# Load MySQL Plugin

- download mysql connector,put it in 
```
libexec/jars/3rdparty/
```
- restart the drill

---

# Configurate MySQL storage

![w:900 h:540](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/72/729aa01691da711cabb6ae2ed48ae7bf.png)

---


# Query MySQL database

![w:800 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/ea/eac700404e145e7a85c256c98b1f9555.png)

---

# Query MySQL database

![w:1000 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/c1/c12886e4c2234d36f065cf3cb3872210.png)


---

# Configure Elastic Plugin

![w:1000 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/cb/cb0a3d998dbcc6f93c6e740e6cb0aac1.png)

---

# ElasticSearch query execution

![w:1000 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/74/74eed521c4fe3467634ef0e4d2964a19.png)

---

# Configurate filesystem storage

![w:800 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/9b/9b27c50afeec107227e33b270537466a.png)

---
# Query local file system

```SQL
select * from mydata.data.`index.json`
```


---

# Query data from ES and dump to local storage

```SQL
create table from mydata.data.`cached.json`
as select * from elastic.`mysql_binlog_wxwork`
where
(tolist= 'user0001' and `from`='wmxCvzEQAAFxxR4kwmxCvzEQAAFxxR4k')
or (`from`= 'user0001' and `tolist`='wmxCvzEQAAFxxR4kwmxCvzEQAAFxxR4k')
and msg_time > 1677746678530 and msg_time < 1677775478530
order by msg_time asc 
```

---

# Parse json field

```SQL
select t.content from (
select convert_fromJSON(`cached.json`.content) as content  from mydata.data.`cached.json`
) t
```
---

# Parse json field

![w:800 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/f4/f4da1e2f82d7345f9ad33bd3e53adff8.png)


---
# Parse json field 

```SQL
select t.content.content from (
select convert_fromJSON(`cached.json`.content) as content  from mydata.data.`cached.json`
) t
```

---
# Parse json field
![w:800 h:600](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/37/37b595f4ef1e1d6d362c3c761fa63a9f.png)


---
# Quering data with drill-jdbc-driver

## 1. add jdbc driver to pom.xml
```xml
        <dependency>
            <groupId>org.apache.drill.exec</groupId>
            <artifactId>drill-jdbc-all</artifactId>
            <version>1.20.2</version>
        </dependency>
```

---
# Quering data with drill-jdbc-driver

## 2. connect to jdbc-drill data source

```java
    static DataSource getSource() throws ClassNotFoundException {
        String driver = "org.apache.drill.jdbc.Driver";
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        return dataSource;
    }

    static JdbcTemplate getTemplate() throws ClassNotFoundException {
        return new JdbcTemplate(getSource());
    }
```

---

## 2. connect to jdbc-drill data source

```java
  String q = "select *  from mydata.data.`cached.json`";
  JdbcTemplate jdbcTemplate = getTemplate();
  List<MessageItem> items =  jdbcTemplate.query(q,new BeanPropertyRowMapper<>(MessageItem.class));
```
