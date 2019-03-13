# 记录一下有用代码片段

##　禁止单元测试缓存
```bash
go test -count=1 . 
```

## 调试golang程序

### 1.附加调试代码

```go
import _ "net/http/pprof"

go func() {
    log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

### 2. pprof查看

```bash
go tool pprof http://localhost:6060/debug/pprof/heap
```

### 3. 图形化

```bash
svg
```

## 防止多进程实例

```go
// 防止多进程
const (
    PID_FILE = "one_instance.pid"
)

type CmdHandler func()

func OneInstance(handle CmdHandler) CmdHandler {
    return func() {
        pidFile, err := os.OpenFile(PID_FILE, os.O_RDWR|os.O_CREATE, 0644)
        if err != nil {
            fmt.Println("OpenFile:%v", err.Error())
            return
        }
        defer pidFile.Close()

        pid := os.Getpid()

        err = syscall.Flock(int(pidFile.Fd()), syscall.LOCK_EX|syscall.LOCK_NB)
        if err != nil {
            fmt.Println(fmt.Sprintf("Flock:%v, %v", pid, err.Error()))
            return
        }

        _, err = pidFile.Write([]byte(strconv.Itoa(pid)))
        if err != nil {
            fmt.Println(("handle:%v, %v", pid, err.Error())
            return
        }

        handle()

        syscall.Flock(int(pidFile.Fd()), syscall.LOCK_UN)
    }
}
```

## 重定向严重错误输出

```golang
func InitFatalLog(fileName string)  {
	logFile, err := os.OpenFile(fileName, os.O_CREATE|os.O_APPEND|os.O_RDWR, 0660)
	if err != nil {
		Log.Printf("服务启动出错, 打开异常日志文件失败, err:%+v", err)
		return
	}
	//将进程标准出错重定向至文件，进程崩溃时运行时将向该文件记录协程调用栈信息
	syscall.Dup2(int(logFile.Fd()), int(os.Stderr.Fd()))
}
```

## 获取本机ip
```golang
    addrs, err := net.InterfaceAddrs()
	if err != nil {
		panic(err.Error())
	}
	// handle err
	for _, addr := range addrs {
		ipNet, ok := addr.(*net.IPNet)
		if !ok {
			continue
		}
		ip := ipNet.IP
		if ip.IsUnspecified() || ip.IsLoopback() || ip.IsMulticast() || ip.IsLinkLocalUnicast() || ip.IsLinkLocalMulticast() {
			continue
		}

		selfIp = ip.String()
		if IsPublicIP(ip) {
			break
		}
	}

	if selfIp == "" {
		selfIp, _ = os.Hostname()
	}

	func IsPublicIP(IP net.IP) bool {
    if ip4 := IP.To4(); ip4 != nil {
        switch true {
        case ip4[0] == 10:
            return false
        case ip4[0] == 172 && ip4[1] >= 16 && ip4[1] <= 31:
            return false
        case ip4[0] == 192 && ip4[1] == 168:
            return false
        }
    }
    return false
}
```

## 优秀的配置库viper+监控文件fsnotify,动态修改参数
```golang
import(
	"github.com/spf13/viper"
	"github.com/fsnotify/fsnotify"
)

viper.SetConfigName("config") // name of config file (without extension)
viper.AddConfigPath("/etc/appname/")   // path to look for the config file in
viper.AddConfigPath("$HOME/.appname")  // call multiple times to add many search paths
viper.AddConfigPath(".")               // optionally look for config in the working directory
err := viper.ReadInConfig() // Find and read the config file
if err != nil { // Handle errors reading the config file
	panic(fmt.Errorf("Fatal error config file: %s \n", err))
}

viper.WatchConfig()
viper.OnConfigChange(func(e fsnotify.Event) {
	fmt.Println("Config file changed:", e.Name)
})
```

## 通用的sql库
```golang
package main

import (
    "database/sql"
    "fmt"
    "log"
    
    _ "github.com/lib/pq"
    "github.com/jmoiron/sqlx"
)

var schema = `
CREATE TABLE person (
    first_name text,
    last_name text,
    email text
);
`

type Person struct {
    FirstName string `db:"first_name"`
    LastName  string `db:"last_name"`
    Email     string
}

func main() {
    // this Pings the database trying to connect, panics on error
    // use sqlx.Open() for sql.Open() semantics
    db, err := sqlx.Connect("postgres", "user=foo dbname=bar sslmode=disable")
    if err != nil {
        log.Fatalln(err)
    }

    // exec the schema or fail; multi-statement Exec behavior varies between
    // database drivers;  pq will exec them all, sqlite3 won't, ymmv
    db.MustExec(schema)
    
    tx := db.MustBegin()
    tx.MustExec("INSERT INTO place (country, telcode) VALUES ($1, $2)", "Singapore", "65")
    // Named queries can use structs, so if you have an existing struct (i.e. person := &Person{}) that you have populated, you can pass it in as &person
    tx.NamedExec("INSERT INTO person (first_name, last_name, email) VALUES (:first_name, :last_name, :email)", &Person{"Jane", "Citizen", "jane.citzen@example.com"})
    tx.Commit()

    // Query the database, storing results in a []Person (wrapped in []interface{})
    people := []Person{}
    db.Select(&people, "SELECT * FROM person ORDER BY first_name ASC")
    jason, john := people[0], people[1]

    fmt.Printf("%#v\n%#v", jason, john)
    // Person{FirstName:"Jason", LastName:"Moiron", Email:"jmoiron@jmoiron.net"}
    // Person{FirstName:"John", LastName:"Doe", Email:"johndoeDNE@gmail.net"}

    // You can also get a single result, a la QueryRow
    jason = Person{}
    err = db.Get(&jason, "SELECT * FROM person WHERE first_name=$1", "Jason")
    fmt.Printf("%#v\n", jason)
    // Person{FirstName:"Jason", LastName:"Moiron", Email:"jmoiron@jmoiron.net"}

    // if you have null fields and use SELECT *, you must use sql.Null* in your struct
    places := []Place{}
    err = db.Select(&places, "SELECT * FROM place ORDER BY telcode ASC")
    if err != nil {
        fmt.Println(err)
        return
    }
    usa, singsing, honkers := places[0], places[1], places[2]
    
    fmt.Printf("%#v\n%#v\n%#v\n", usa, singsing, honkers)
    // Place{Country:"United States", City:sql.NullString{String:"New York", Valid:true}, TelCode:1}
    // Place{Country:"Singapore", City:sql.NullString{String:"", Valid:false}, TelCode:65}
    // Place{Country:"Hong Kong", City:sql.NullString{String:"", Valid:false}, TelCode:852}

    // Loop through rows using only one struct
    place := Place{}
    rows, err := db.Queryx("SELECT * FROM place")
    for rows.Next() {
        err := rows.StructScan(&place)
        if err != nil {
            log.Fatalln(err)
        } 
        fmt.Printf("%#v\n", place)
    }
    // Place{Country:"United States", City:sql.NullString{String:"New York", Valid:true}, TelCode:1}
    // Place{Country:"Hong Kong", City:sql.NullString{String:"", Valid:false}, TelCode:852}
    // Place{Country:"Singapore", City:sql.NullString{String:"", Valid:false}, TelCode:65}

    // Named queries, using `:name` as the bindvar.  Automatic bindvar support
    // which takes into account the dbtype based on the driverName on sqlx.Open/Connect
    _, err = db.NamedExec(`INSERT INTO person (first_name,last_name,email) VALUES (:first,:last,:email)`, 
        map[string]interface{}{
            "first": "Bin",
            "last": "Smuth",
            "email": "bensmith@allblacks.nz",
    })

    // Selects Mr. Smith from the database
    rows, err = db.NamedQuery(`SELECT * FROM person WHERE first_name=:fn`, map[string]interface{}{"fn": "Bin"})

    // Named queries can also use structs.  Their bind names follow the same rules
    // as the name -> db mapping, so struct fields are lowercased and the `db` tag
    // is taken into consideration.
    rows, err = db.NamedQuery(`SELECT * FROM person WHERE first_name=:first_name`, jason)
}
```

##　map+mutex和sync.map比较
> map+sync.mutex在小数据范围内效率高,大并发及cpu效率下降明显
> sync.map性能比较平稳,主要采用读写分开存储读取,建议采用.

## proto文件enum定义,可以自转产生名称字符串

```pb
enum Platfrom 
{
    IOS = 1;			//ios//
    Android = 2;		//安卓//
};
```

```golang
type Platfrom int32

const (
	Platfrom_IOS     Platfrom = 1
	Platfrom_Android Platfrom = 2
)

var Platfrom_name = map[int32]string{
	1: "IOS",
	2: "Android",
}

var Platfrom_value = map[string]int32{
	"IOS":     1,
	"Android": 2,
}

func (x Platfrom) Enum() *Platfrom {
	p := new(Platfrom)
	*p = x
	return p
}

func (x Platfrom) String() string {
	return proto.EnumName(Platfrom_name, int32(x))
}

func (x *Platfrom) UnmarshalJSON(data []byte) error {
	value, err := proto.UnmarshalJSONEnum(Platfrom_value, data, "Platfrom")
	if err != nil {
		return err
	}
	*x = Platfrom(value)
	return nil
}

func (Platfrom) EnumDescriptor() ([]byte, []int) {
	return fileDescriptor_08cf373aec2ae6fc, []int{0}
}
```

## 发邮件
```golang
import (
	"github.com/spf13/viper"
	"gopkg.in/gomail.v2"
)
/*
 发送邮件
*/
func SendMail(subject, body string) {
	m := gomail.NewMessage()
	m.SetHeader("From", mailConfigViper_.GetString("from"))
	m.SetHeader("To", mailConfigViper_.GetString("to"))

	m.SetHeader("Subject", subject)
	m.SetBody("text/plain", body)

	d := gomail.NewDialer(
		mailConfigViper_.GetString("host"),
		mailConfigViper_.GetInt("port"),
		mailConfigViper_.GetString("user"),
		mailConfigViper_.GetString("pwd"),
	)

	// Send the email to Bob, Cora and Dan.
	if err := d.DialAndSend(m); err != nil {
		// handle errr
	}
}
```