---
title: Golang-Gin
date: 2022-07-21 22:08:11
categories: [Golang]
tags: Golang, Gin
---
# 快速开始

导入Gin
``` golang
import "github.com/gin-gonic/gin"
```

``` golang
r := gin.Default()
r.POST("/", func(c *gin.Context) {
	c.String(200, "OK")
})
r.Run(":5701")
```

## 中间件

### 全局中间件

```go
// 定义中间
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        t := time.Now()
        fmt.Println("中间件开始执行了")
        // 设置变量到Context的key中，可以通过Get()取
        c.Set("request", "中间件")
        status := c.Writer.Status()
        fmt.Println("中间件执行完毕", status)
        t2 := time.Since(t)
        fmt.Println("time:", t2)
    }
}

func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    // 注册中间件
    r.Use(MiddleWare())
    // {}为了代码规范
    {
        r.GET("/ce", func(c *gin.Context) {
            // 取值
            req, _ := c.Get("request")
            fmt.Println("request:", req)
            // 页面接收
            c.JSON(200, gin.H{"request": req})
        })

    }
    r.Run()
}
```