# gorequests
**像python一样优雅的Go爬虫库**

通过对net/http的再一次封装，使得gorequests的语法更接近python requests的写法

一、安装

> go get github.com/gamegrd/gorequests

二、Demo

1.get请求

```go
package main

import (
	"fmt"

	"github.com/gamegrd/gorequests"
)

func main() {
	resp, err := gorequests.Get("https://www.httpbin.org/get?a=1&b=2")
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(resp.Text())

	// Params
	params := gorequests.Params{"a": "1", "b": "2"}
	res, err := gorequests.Get("https://www.httpbin.org/get", params)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(res.Text())
}
```

2.post请求

```go
package main

import (
	"fmt"

	"github.com/gamegrd/gorequests"
)

func main() {
	// form post
	data := gorequests.Data{
		"name": "xiaobulv",
	}
	resp, _ := gorequests.Post("https://www.httpbin.org/post", data)
	fmt.Println(resp.Text())

	// json post
	jdata := gorequests.Json{"AGE": "131"}
	respb, _ := gorequests.Post("https://www.httpbin.org/post", jdata)
	fmt.Println(respb.Text())

}
```

3.put delete patch等

```go
package main

import (
	"fmt"

	"github.com/gamegrd/gorequests"
)

func main() {
	// put
	putdata := gorequests.Json{"AGE": "131"}
	respp, _ := gorequests.Put("https://www.httpbin.org/put", putdata)
	fmt.Println(respp.Text())
}
```

4.auth

```go
package main

import (
	"github.com/gamegrd/gorequests"
)

func main() {
	respa, _ := gorequests.Get("https://www.httpbin.org/basic-auth/admin/123456", gorequests.Auth{"admin", "123456"})
	println(respa.Text())
}
```

5.header, cookie, timeout, proxy, json等设置

```go
package main

import (
	"fmt"

	"github.com/gamegrd/gorequests"
)

func main() {
	// 设置header
	header := gorequests.Header{"user-agent": "hello"}

	// post 请求参数
	data := gorequests.Data{
		"name": "xiaobulv",
	}

	// 设置cookie
	// 当Header里有Cookie时, 此设置无效
	ck := gorequests.Cookie{"BIDUPSID": "C855441CA6145FBB2741293580"}

	// timeout
	timeout := gorequests.SetTimeout(10)

	// 代理
	// proxy := gorequests.Proxy("http://xxx.ip.com")

	resp, _ := gorequests.Post("https://www.httpbin.org/post", data, header, ck, timeout)

	// content
	fmt.Println(resp.Content())

	// text
	fmt.Println(resp.Text())

	// response.Json
	m := make(map[string]interface{})
	resp.Json(&m)
	fmt.Println(m["headers"])
	c := m["headers"]
	fmt.Println(c.(map[string]interface{})["Host"])

	// 状态码
	fmt.Println(resp.StatusCode)

	// 响应cookie
	fmt.Println(resp.Cookies())
}
```

