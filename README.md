# rorm
go 语言编写 的数据库映射框架  支持各种数据库 例如mysql,oracle,sqlserver,达梦数据库等
```go
package main

import (
	"https://github.com/rabbitcch/rorm"
	_ "github.com/rabbitcch/rorm/dialects/mssql"
	_ "github.com/rabbitcch/rorm/dialects/mysql"
	_ "github.com/rabbitcch/rorm/dialects/postgres"
	_ "github.com/rabbitcch/rorm/dialects/sqlite"
)

var db *rorm.DB

func init() {
	var err error
	db, err = rorm.Open("sqlite3", "test.db")
	// Please use below username, password as your database's account for the script.
	// db, err = rorm.Open("postgres", "user=gorm dbname=gorm sslmode=disable")
	// db, err = rorm.Open("mysql", "gorm:gorm@/dbname?charset=utf8&parseTime=True")
	// db, err = rorm.Open("mssql", "sqlserver://gorm:LoremIpsum86@localhost:1433?database=gorm")
	if err != nil {
		panic(err)
	}
	db.LogMode(true)
}

func main() {
	// your code here

	if /* failure condition */ {
		fmt.Println("failed")
	} else {
		fmt.Println("success")
	}
}
```
