
# 开始
让我们来编写第一个`actix-web`应用吧

# Hello,World!
使用`Cargo`创建一个的项目,并且`cd`到指定新目录
```sh
cargo new hello-world --bin
cd hello-world
```
现在往`Cargo.toml`文件中添加`actix-web`的依赖.
```toml
[dependencies]
actix-web = "0.7"
```

为了实现一个web服务,我们首先需要创建一个请求处理器(request handler)

一个请求处理器是一个函数,这个函数只有一个参数,用于接受`HttpRequest`的实例,并且返回类型能够转换为`HttpResponse`
```rs
//FileName : src/main.rs

extern crate actix_web;
use actix_web::{server, App, HttpRequest};

fn index(_req: &HttpRequest) -> &'static str {
    "Hello world!"
}
```

接下来就需要创建一个`Application`的实例,并且注册请求处理器和应用的资源(指定的http方法和路径)绑定起来.`Application`的实例使用`HttpServer`来监听即将到来的链接.这个`Server`接受返回一个`HttpHandler`实例的函数.`server::new`是`HttpServer::new`的缩写形式.这个就能够创建一个服务实例.

```rs
fn main() {
    server::new(|| App::new().resource("/", |r| r.f(index)))
        .bind("127.0.0.1:8088")
        .unwrap()
        .run();
}
```
接下来使用`cargo run`编译并且运行程序,就能够通过`http://localhost:8080/`来访问你编写出来的web服务了.(不出意外的话,浏览器将会显示"Hello wrold!"这一个字符串,取决于你的请求处理器具体返回值)

```rs
//完整代码 filename : src/main.rs
extern crate actix_web;
use actix_web::{server, App, HttpRequest};

fn index(_req: &HttpRequest) -> &'static str {
    "Hello world!"
}


fn main() {
    server::new(|| App::new().resource("/", |r| r.f(index)))
        .bind("127.0.0.1:8088")
        .unwrap()
        .run();
}

```
*如果你需要在开发期间自动的重新编译并重加载服务,请查看**自动重新加载模式(autoreload pattern)***
