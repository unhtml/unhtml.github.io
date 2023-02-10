---
marp: true 
theme: UCASSimple
papaginate: false
---
<style scoped>
  blockquote {
        text-align:right;
    }
</style> 

# Glance of Gorm 

The fantastic ORM library for Golang aims to be developer friendly.
> 黄滚 @ 2022.7.15

---

# 特性 

- 全功能ORM 
- 关联（1对1，1对多，多对多）
- 迁移支持
- 连接池
---
# 特性 
- 事务支持
- 基于会话的设置
- 调试友好
- Logger

---
# 基本用法

```Golang
import (
  "gorm.io/gorm"
  "gorm.io/driver/sqlite"
)
type Product struct {
  gorm.Model
  Code  string
  Price uint
}

```
--- 
# 基本用法
```Golang
func main(){
  db, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }
  // 迁移 schema
  db.AutoMigrate(&Product{})
  
}
```
---
# 连接池
gorm 使用database/sql 来管理连接池
```Golang
    sqlDB, err := sql.Open("mysql", "username:password@tcp(127.0.0.1:3306)/smeDB?charset=utf8mb4&parseTime=True&loc=Local")
db, err := gorm.Open(mysql.New(mysql.Config{
  Conn: sqlDB,
}), &gorm.Config{})
	sqlDBConn, err := db.DB()
	sqlDBConn.SetMaxIdleConns(4)
	sqlDBConn.SetMaxOpenConns(200)
```
---
# CRUD 之create

```Golang
user := User{Name: "Jinzhu", Age: 18, Birthday: time.Now()}
result := db.Create(&user) // 通过数据的指针来创建

var users = []User{{Name: "jinzhu1"}, {Name: "jinzhu2"}, {Name: "jinzhu3"}}
db.Create(&users)


```
---
# CRUD之 Query
```Golang
db.Where("name = ?", "jinzhu").First(&user)
db.Where("name <> ?", "jinzhu").Find(&users)
db.Where("name LIKE ?", "%jin%").Find(&users)
db.Where(&User{Name: "jinzhu", Age: 20}).First(&user)
db.Limit(10).Offset(100).Where(&User{Name: "jinzhu", Age: 20}).First(&user)
```
---
# CRUD之 update

```Golang
db.First(&user)

user.Name = "jinzhu 2"
user.Age = 100
db.Save(&user)

//更新单个列
db.Model(&User{}).Where("active = ?", true).Update("name", "hello")
```
---
# CRUD 之delete
```Golang
// Email 的 ID 是 `10`
db.Delete(&email)
// DELETE from emails where id = 10;

// 带额外条件的删除
db.Where("name = ?", "jinzhu").Delete(&email)

db.Delete(&User{}, 10)
// DELETE FROM users WHERE id = 10;

db.Delete(&User{}, "10")
// DELETE FROM users WHERE id = 10;

db.Delete(&users, []int{1,2,3})
// DELETE FROM users WHERE id IN (1,2,3);

```

---
# 关联:Belongs to
```Golang
// `User` 属于 `Company`，`CompanyID` 是外键
type User struct {
  gorm.Model
  Name      string
  CompanyID int
  Company   Company
}

type Company struct {
  ID   int
  Name string
}


```

---
# 关联:Has One (Declare)

```Golang
// User 有一张 CreditCard，UserID 是外键
type User struct {
  gorm.Model
  CreditCard CreditCard
}

type CreditCard struct {
  gorm.Model
  Number string
  UserID uint
}
```
---
# 关联:Has one (Retrieve)

```Golang
// Retrieve user list with eager loading credit card
func GetAll(db *gorm.DB) ([]User, error) {
    var users []User
    err := db.Model(&User{}).Preload("CreditCard").Find(&users).Error
    return users, err
}

```
---
# 关联: has many (Declare)

```Golang
// User 有多张 CreditCard，UserID 是外键
type User struct {
  gorm.Model
  CreditCards []CreditCard
}

type CreditCard struct {
  gorm.Model
  Number string
  UserID uint
}


```

---
# 关联:hash many (Retrieve)
```Golang
// Retrieve user list with edger loading credit cards
func GetAll(db *gorm.DB) ([]User, error) {
    var users []User
    err := db.Model(&User{}).Preload("CreditCards").Find(&users).Error
    return users, err
}

```

---
# Context 
```Golang
	ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
	defer cancel()
    //单一操作
	db.WithContext(ctx).Find(&users)
    //持续操作
    tx := db.WithContext(ctx)
    tx.First(&user, 1)
    tx.Model(&user).Update("role", "admin")


```

---
# 事务支持

> Gorm默认是开启事务的
```Golang
// 全局禁用
db, err := gorm.Open(sqlite.Open("gorm.db"), &gorm.Config{
  SkipDefaultTransaction: true,
})

// 持续会话模式
tx := db.Session(&Session{SkipDefaultTransaction: true})
tx.Find(&users)
tx.Model(&user).Update("Age", 18)
```

---
# 手动事务
```Golang
// 开始事务
tx := db.Begin()

// 在事务中执行一些 db 操作（从这里开始，您应该使用 'tx' 而不是 'db'）
tx.Create(...)

// ...

// 遇到错误时回滚事务
tx.Rollback()

// 否则，提交事务
tx.Commit()
```

---

<style scoped>
   
    h1,h2 {
        text-align: center;
        font-size:3em;
    }
    h2 {
        color:orange;
    }
    blockquote {
        text-align:right;
    }

</style>
# 谢谢
## ありがとうございました。