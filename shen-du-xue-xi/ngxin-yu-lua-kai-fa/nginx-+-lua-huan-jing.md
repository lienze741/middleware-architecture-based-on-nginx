# nginx + lua环境

* LuaJIT
* ngx\_devel\_kit和lua-nginx-module
* 重新编译nginx

[如何编译 ](http://www.imooc.com/article/19597)

## nginx调用lua模块指令

nginx的可插拔模块化加载执行,共11个处理阶段

|   |  |
| :--- | --- |
| set\_by\_lua   set\_by\_lua\_file | 设置nginx变量,可以实现复杂的复制逻辑 |
| access\_by\_lua   access\_by\_lua\_file | 请求访问阶段处理,用于访问控制 |
| content\_by\_lua   content\_by\_lua\_file | 内容处理器,接受请求处理并输出相应 |

## nginx lua api

| nginx lua api |  |
| :--- | :--- |
| ngx.var | nginx变量 |
| ngx.req.get\_headers | 获取请求头 |
| ngx.req.get\_url\_args | 获取url请求参数 |
| ngx.redirect | 重定向 |
| ngx.print | 输出相应内容体 |
| ngx.say | 通过ngx.print,但是会最后输出一个换行符 |
| ngx.header | 输出响应头 |
| ... |  |

### 例子:

```text
    ...

    location /hello {
        default_type 'test/plain';
        content_by_lua 'ngx.say("hello,lua")';
    }

    location /myip {
        default_type 'text/plain';
        content_by_lua '
            clientIP = ngx.req.get_headers()["x_forwarded_for"]
            ngx.say("IP:",clientIP)
            ';
    }

    ...
```

