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
svg
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