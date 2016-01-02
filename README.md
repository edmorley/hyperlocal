# hyperlocal

> [hyper](https://github.com/hyperium/hyper) bindings for [unix domain sockets](https://github.com/rust-lang-nursery/unix-socket)

## usage

### servers

```rust
extern crate hyper;
extern crate hyperlocal;

use hyper::server::{Request, Response};
use hyperlocal::UnixSocketServer;

fn main() {
    let path = "test.sock";
    let server = UnixSocketServer::new(path).unwrap();
    server.handle(|_: Request, res: Response| {
        let _ = res.send(b"It's a Unix system. I know this.");
    }).unwrap();
    println!("listening @ {}", path);
}
```

### client

```rust
extern crate hyper;
extern crate hyperlocal;

use hyper::Client;
use hyperlocal::{DomainUrl, UnixConnector};

fn main() {
    let path = "test.sock";
    let client = Client::with_connector(UnixConnector);
    let mut res = client.get(DomainUrl::new(path, "/")).send().unwrap();
    std::io::copy(&mut res, &mut std::io::stdout()).unwrap();
}
```

Doug Tangren (softprops) 2015
