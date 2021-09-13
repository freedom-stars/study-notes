
# 作业

## 1.安装Go1.17

[安装Go1.17](https://golang.org/doc/install)

## 2.刷一遍go-tour

[go-tour](https://tour.go-zh.org/welcome/1)

## 3.刷一遍gobyexample

[gobyexample](https://gobyexample-cn.github.io/)

## 4.解析JSON文件到struct并输出

把下面json保存为“users.json”，然后写一个main.go 从文件读取并解析成user。
```
{
  "users": [
    {
      "name": "Elliot",
      "type": "Reader",
      "age": 23,
      "social": {
        "facebook": "https://facebook.com",
        "twitter": "https://twitter.com"
      }
    },
    {
      "name": "Fraser",
      "type": "Author",
      "age": 17,
      "social": {
        "facebook": "https://facebook.com",
        "twitter": "https://twitter.com"
      }
    }
  ]
}
```

执行结果如下

```
$ go run main.go
Successfully opened users.json
User Type: Reader
User Age: 23
User Name: Elliot
Facebook Url: https://facebook.com
User Type: Author
User Age: 17
User Name: Fraser
Facebook Url: https://facebook.com
{[{Elliot Reader 23 {https://facebook.com https://twitter.com}} {Fraser Author 17 {https://facebook.com https://twitter.com}}]}
```

## 5.Go 反射实战作业

反射是Go里面很重要的一个特性，反射是程序在运行时检查其变量和值并找到它们类型的能力。你可能不明白这意味着什么，但这没关系。通过这个作业你将对反射有一个清晰的了解。

通过这个作业我希望大家了解这些概念
- reflect.Type和 reflect.Value
- reflect.Kind
- NumField() 和 Field() 方法
- Int() 和 String() 

通过下面的例子来动手实战

提供下面两个struct结构体
```
type order struct {  
    ordId      int
    customerId int
}

type employee struct {  
    name    string
    id      int
    address string
    salary  int
    country string
}
```

下面是主函数的实现，就是通过传入的struct来生成SQL语句，加入我们要根据不同的结构体插入到不同的数据库表里面去。

```
func main() {  
    o := order{
        ordId:      456,
        customerId: 56,
    }
    createQuery(o)

    e := employee{
        name:    "Naveen",
        id:      565,
        address: "Coimbatore",
        salary:  90000,
        country: "India",
    }
    createQuery(e)
    i := 90
    createQuery(i)
}

# 实现createQuery

func createQuery(q interface{}) { 

}
```

最终的输出是这三个
insert into order values(456, 56)  
insert into employee values("Naveen", 565, "Coimbatore", 90000, "India")  
unsupported type

## 6.Go 实现一个命令行解析工具

这一次我们引入第三方依赖包，https://github.com/urfave/cli

这个项目取名netcmd，推荐用下面的目录结构，这种目录结构是目前推荐的一种做命令行工具的结构：

netcli/
- pkg/
- cmd/cli/
- vendor/
- README.md
- ...

首先下载第三方包：go get github.com/urfave/cli

然后先通过一个列子一个小例子理解一下。
```
// cmd/cli/cli.go
import (
  "log"
  "os"

  "github.com/urfave/cli"
)

func main() {
  err := cli.NewApp().Run(os.Args)
  if err != nil {
    log.Fatal(err)
  }
}
```

在自己的终端运行一下命令行：

```
➜  go run cmd/my-cli/cli.go
NAME:
   cli - A new cli application

USAGE:
   cli [global options] command [command options] [arguments...]

VERSION:
   0.0.0

COMMANDS:
     help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```

是不是很帅，实现了自己第一个终端命令行工具。

接下来我们要去实现我们更多的命令参数，这个作业我们的目标是实现三个命令行工具

- ns - 根据host获取 name servers
- cname - 根据host获取 CNAME
- ip - 根据host获取IP地址

那么三个都怎么去获取的呢？给大家一些提示，这些实际上go内置的net库都已经实现了。

- ns 使用函数 ns, err := net.LookupNS(host)
- cname 使用函数 cname, err := net.LookupCNAME(host)
- ip 使用函数         ip, err := net.LookupIP(host)

所以首先这个命令行要接收两个参数，cmd和host


最终的效果如下：

```
./cli help
NAME:
   Website Lookup CLI - Let's you query IPs, CNAMEs and Name Servers!

USAGE:
   cli [global options] command [command options] [arguments...]

VERSION:
   0.0.0

COMMANDS:
     ns       Looks Up the NameServers for a Particular Host
     cname    Looks up the CNAME for a particular host
     ip       Looks up the IP addresses for a particular host
     help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```