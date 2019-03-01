# 记录一下有用代码片段

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