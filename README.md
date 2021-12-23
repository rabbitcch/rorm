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
                        # 汇联RORM介绍
An RPC framework based on Netty, ZooKeeper and Spring  
中文详情：[Chinese Details](http://www.cnblogs.com/luxiaoxun/p/5272384.html)
### Features:
* Simple code and framework
* Service registry/discovery support by ZooKeeper
* High availability, load balance and failover
* Support different load balance strategy
* Support asynchronous/synchronous call
* Support different versions of service
* Support different serializer/deserializer
* Dead TCP connection detecting with heartbeat
### Design:
![design](https://github.com/luxiaoxun/NettyRpc/blob/master/picture/NettyRpc-design.png)
### How to use (netty-rpc-test)
1. Define an interface:
    ```  
    public interface HelloService { 
        String hello(String name); 
        String hello(Person person);
    }
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
