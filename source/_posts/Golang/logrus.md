---
title: logrus使用
date: 2022-09-24 14:02:35
categories:
    - Golang
tags:
    - Golang
    - logrus
---

# logrus使用：

logrus是一个流行的Golang日志库。

参考：

> https://pkg.go.dev/github.com/sirupsen/logrus#pkg-functions 官方

## 安装

```shell
go get github.com/sirupsen/logrus
```

## 快速使用

- Panic：记录日志，然后panic。
- Fatal：致命错误，出现错误时程序无法正常运转。输出日志后，程序退出；
- Error：错误日志，需要查看原因；
- Warn：警告信息，提醒程序员注意；
- Info：关键操作，核心流程的日志；
- Debug：一般程序中输出的调试信息；
- Trace：很细粒度的信息，一般用不到；

> 日志级别从上向下依次增加，Trace最大，Panic最小。logrus有一个日志级别，高于这个级别的日志不会输出。 默认的级别为InfoLevel。

## 函数

### SetLevel/设置日志等级

- SetLevel(level Level)

### SetFormatter/设置格式化

内置JSONFormater和TextFormatter。

- SetFormatter(formatter Formatter)

### SetReportCaller/日志加入文件信息

- SetReportCaller(include bool)

### WithField/添加自定义字段

有时候需要在输出中添加一些字段，可以通过调用logrus.WithField和logrus.WithFields实现。
logrus.WithFields接受一个logrus.Fields类型的参数，其底层实际上为map[string]interface{}：

```go
Fields map[string]interface{}

// 示例
func main() {
  logrus.WithFields(logrus.Fields{
    "name": "dj",
    "age": 18,
  }).Info("info msg")
}
```

### SetOutput/设置输出日志位置

接收一个标准io流，默认输出到``io.Stderr``：

- func SetOutput(out io.Writer)

```go
// 输出到文件
func main() {
  writer1 := &bytes.Buffer{}
  writer2 := os.Stdout
  writer3, err := os.OpenFile("log.txt", os.O_WRONLY|os.O_CREATE, 0755)
  if err != nil {
    log.Fatalf("create file log.txt failed: %v", err)
  }

  logrus.SetOutput(io.MultiWriter(writer1, writer2, writer3))
  logrus.Info("info msg")
}
```

## HOOK

接口：

```go
type Hook interface {
	Levels() []Level
	Fire(*Entry) error
}
```

简单示例：

```go
type DefaultFieldHook struct {
}

func (hook *DefaultFieldHook) Fire(entry *log.Entry) error {
    entry.Data["appName"] = "MyAppName"
    return nil
}

func (hook *DefaultFieldHook) Levels() []log.Level {
    return log.AllLevels
}

```

使用示例：

```go
package main
import (
	log "github.com/sirupsen/logrus"
	"gopkg.in/gemnasium/logrus-airbrake-hook.v2" // the package is named "airbrake"
	logrus_syslog "github.com/sirupsen/logrus/hooks/syslog"
	"log/syslog"
)

func init() {

	// Use the Airbrake hook to report errors that have Error severity or above to
	// an exception tracker. You can create custom hooks, see the Hooks section.
	log.AddHook(airbrake.NewHook(123, "xyz", "production"))

	hook, err := logrus_syslog.NewSyslogHook("udp", "localhost:514", syslog.LOG_INFO, "")
	if err != nil {
		log.Error("Unable to connect to local syslog daemon")
	} else {
		log.AddHook(hook)
	}
}

```

- AddHook(hook Hook) 添加一个hook