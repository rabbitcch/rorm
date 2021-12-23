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
#                                 汇联RORM介绍
## 1.方案概述:
	     中科汇联凭借多年软件产品开发经验和政企大型信息化项目实施经验，结合雄厚的技术底蕴积累，现推出了汇联RORM开发框架。
	     汇联RORM是一款支持自定义SQL、存储过程和高级映射的持久化框架。
	     汇联RORM几乎消除了所有的JDBC代码，也基本不需要手工去设置参数和获取检索结果，能够让开发人员不再关注数据库细节以及不同数据库之间的语法差异，
	     不需要自己编写复杂的SQL语句，也不需要封装复杂的数据底层，能够赋能开发人员将宝贵的时间与精力放在业务逻辑构建上。
## 2.总体架构:
![design](https://raw.githubusercontent.com/rabbitcch/rorm/main/123.png)

	汇联RORM的功能架构分为三层：
	API接口层：提供给外部使用的接口API，开发人员通过这些本地API来操纵数据库。
	接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。
	
	数据处理层：负责具体的SQL查找、SQL解析、SQL执行和执行结果映射处理等，它主要的目的是根据调用的请求完成一次数据库操作。
	
	基础支撑层：负责最基础的功能支撑，包括连接管理、事务管理、配置加载和缓存处理，这些都是共用的东西，
	将他们抽取出来作为最基础的组件。为上层的数据处理层提供最基础的支撑。
##3.工作流程与功能介绍
### 3.1初始化
    ```  
	连接比较简单，直接调用 rorm.Open 传入数据库地址即可
	db,err:=gorm.Open("mysql", "root:root@(127.0.0.1:3306)/db1")
	实际上支持基本上所有主流的关系数据库，连接方式上略有不
    ```  
2. Implement the interface with annotation @NettyRpcService:
    ```  
    @NettyRpcService(HelloService.class, version = "1.0")
    public class HelloServiceImpl implements HelloService {
        public HelloServiceImpl(){}
	
        @Override
        public String hello(String name) {
            return "Hello " + name;
        }
        @Override
        public String hello(Person person) {
            return "Hello " + person.getFirstName() + " " + person.getLastName();
        }
    }
    ```  
3. Run zookeeper

   For example: zookeeper is running on 127.0.0.1:2181

4. Start server:
   1. Start server with spring config: RpcServerBootstrap
   2. Start server without spring config: RpcServerBootstrap2

5. Call the service:
    1. Use the client:
    ```  
   	final RpcClient rpcClient = new RpcClient("127.0.0.1:2181");
   		
   	// Sync call
   	HelloService helloService = rpcClient.createService(HelloService.class, "1.0");
   	String result = helloService.hello("World");
   		
   	// Async call
   	RpcService client = rpcClient.createAsyncService(HelloService.class, "2.0");
   	RPCFuture helloFuture = client.call("hello", "World");
   	String result = (String) helloFuture.get(3000, TimeUnit.MILLISECONDS);
	``` 
    2. Use annotation @RpcAutowired:
    ``` 
    public class Baz implements Foo {
        @RpcAutowired(version = "1.0")
        private HelloService helloService1;
           
        @RpcAutowired(version = "2.0")
        private HelloService helloService2;
           
        @Override
        public String say(String s) {
            return helloService1.hello(s);
        }
    }
    ``` 
