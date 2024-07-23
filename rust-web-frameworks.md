# Comparing Rust Web Frameworks

- [Actix](https://actix.rs/)
- [Axum](https://docs.rs/axum/latest/axum/)
  - [GitHub](https://github.com/tokio-rs/axum)

---

## Rust HTTP Ecosystem in 2024

- <https://kerkour.com/rust-http-ecosystem-2024>
- [www.techempower.com/benchmarks](https://www.techempower.com/benchmarks/#section=data-r22&test=plaintext&l=yyku7z-cn3&hw=ph)

---

## OpenAPI Crates

### There are a couple of options for a code-first approach to OpenAPI in Rust

- Actix uses [apistos](https://github.com/netwo-io/apistos) for OpenAPI support
  - Also requires `schemars` for Data Schema support
  - Larger learning curve
- Axum uses [utoipa](https://github.com/juhaku/utoipa) for OpenAPI support
  - Does not require additional dependencies for Data Schema support
  - Framework agnostic

---

## Actix Web

```rust [ |3|8|15]
use actix_web::{get, web, App, HttpServer, Responder};

#[get("/")]
async fn index() -> impl Responder {
    "Hello, World!"
}

#[get("/{name}")]
async fn hello(
    name: web::Path<String>
) -> impl Responder {
    format!("Hello {}!", &name)
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(||
         App::new()
            .service(index)
            .service(hello)
    )
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

Note: Actix uses macros for route definitions

---

## Axum

```rust
use axum::{ routing::get, Router };
use tokio::net::TcpListener;

#[tokio::main]
async fn main() {
  // build our application with a single route
  let app = Router::new()
    .route("/", get(|| async { "Hello, World!" }));

  // run our app with hyper, listening globally on port 3000
  let listener = TcpListener::bind("0.0.0.0:3000")
    .await.unwrap();
  axum::serve(listener, app).await.unwrap();
}
```

Note: Axum does not require macros for route definitions

---

