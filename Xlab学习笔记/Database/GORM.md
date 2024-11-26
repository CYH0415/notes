在 go 中操作数据库
# 使用 GORM
```go
import (
    "fmt"
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
)
```
貌似还可以用其他包，先不管了
## 连接数据库
有模板：`username:password@protocol(address)/dbname?param=value`
连接本地的数据库：
```go
dsn := `root:[password]@/xlab_intro?charset=utf8mb4`
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
```
-  `db` 是 `*gorm.DB` 类型
# CURD
## 插入
注意传递指针
```
newComment := Comment{ ... }
db.Create(&newComment)
```
- 如果设置了 UNIQUE 字段，重复插入会报错：
![[Pasted image 20240605185814.png]]
- 可以传多个，指针数组直接传数组名，结构数组传数组指针
- 可以让数据库创建事务，分批次插入
```go
db.CreateInBatches(comments, 100)
```
## 检索
十分的奇异，需要传结构体（数组）指针给函数 `db.Find(&comments)`，搜索结果呈现在原数组：
```go
comments := []Comment{}
db.Find(&comments)
fmt.Println(comments)
```
相当于 `Select * from comments`
另有检索函数：`First()`，`Take()`，`Last()`
更奇异的是，带条件的检索，是通过一些函数实现的：
```go
db.Offset(3).Limit(10).Where("name = ?", "WYT").Find(&comments)
```
Where 也可以放结构体指针，或者 `map[string]interface{}`，以下是奇异的 where 条件：
```go
// Get first matched record  
db.Where("name = ?", "jinzhu").First(&user)  
// SELECT * FROM users WHERE name = 'jinzhu' ORDER BY id LIMIT 1;  
  
// Get all matched records  
db.Where("name <> ?", "jinzhu").Find(&users)  
// SELECT * FROM users WHERE name <> 'jinzhu';  
  
// IN  
db.Where("name IN ?", []string{"jinzhu", "jinzhu 2"}).Find(&users)  
// SELECT * FROM users WHERE name IN ('jinzhu','jinzhu 2');  
  
// LIKE  
db.Where("name LIKE ?", "%jin%").Find(&users)  
// SELECT * FROM users WHERE name LIKE '%jin%';  
  
// AND  
db.Where("name = ? AND age >= ?", "jinzhu", "22").Find(&users)  
// SELECT * FROM users WHERE name = 'jinzhu' AND age >= 22;  
  
// Time  
db.Where("updated_at > ?", lastWeek).Find(&users)  
// SELECT * FROM users WHERE updated_at > '2000-01-01 00:00:00';  
  
// BETWEEN  
db.Where("created_at BETWEEN ? AND ?", lastWeek, today).Find(&users)  
// SELECT * FROM users WHERE created_at BETWEEN '2000-01-01 00:00:00' AND '2000-01-08 00:00:00';
```
## 删除
同上，将指针传给 `db.Delete()`，会删除有相同字段的行，如：
```go
toDelete := Comment{}
toDelete.ID = 114
db.Delete(&toDelete)
```