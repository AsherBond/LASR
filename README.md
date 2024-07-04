# LASR
The LASR-stack is a 95% Rust-native web-framework inspired by the LAMP stack. LASR stands for:

- **Linux**: Same as in LAMP but you can use Vanilla OS to add a little more Rust integration
- **Actix**: A powerful Rust-native web framework (alternatively, Rocket).
- **SurrealDB**: A scalable, multi-model database supporting relational semantics, written in Rust.
- **Rust**: The programming language used throughout the stack.

This stack aims to provide a high-performance, memory-safe alternative to traditional stacks like LAMP.

## Getting Started

Follow these steps to set up and run your LASR-Stack application.

### Prerequisites

Ensure you have the following installed on your system:

1. **Rust**: Install Rust from [rust-lang.org](https://www.rust-lang.org/).
2. **Linux OS**: LINUX of any type. (Alternatively) If you prefer to use Redox OS, follow the [Redox installation guide](https://doc.redox-os.org/book/ch01-01-installation.html).
3. **Docker**: For containerizing the database (optional).

### Step 1: Setting Up Your Rust Project

1. Create a new Rust project:
    ```bash
    cargo new my_lasr_project
    cd my_lasr_project
    ```

2. Add the necessary dependencies to your `Cargo.toml`:
    ```toml
    [dependencies]
    actix-web = "4"
    surrealdb = "0.1"
    ```
   If you prefer Rocket, replace `actix-web` with `rocket`:
    ```toml
    [dependencies]
    rocket = "0.5.0-rc.2"
    surrealdb = "0.1"
    ```

### Step 2: Setting Up SurrealDB

You can run SurrealDB using Docker:

```bash
docker run --rm -p 8000:8000 surrealdb/surrealdb:latest start --log debug
```

Alternatively, download SurrealDB from the [official website](https://surrealdb.com/) and run it directly.

### Step 3: Writing Your Application

Create a simple Actix web server that connects to SurrealDB.

#### `src/main.rs`

```rust
use actix_web::{web, App, HttpServer, Responder};
use surrealdb::Surreal;
use surrealdb::sql::Value;

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let db = Surreal::new("http://localhost:8000").await.unwrap();

    HttpServer::new(move || {
        App::new()
            .data(db.clone())
            .route("/", web::get().to(index))
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}

async fn index(db: web::Data<Surreal<surrealdb::http::Client>>) -> impl Responder {
    let response: Value = db.query("SELECT * FROM my_table").await.unwrap();
    format!("Hello from LASR-Stack! Data: {:?}", response)
}
```

### Step 4: Running Your Application

Run your application with:

```bash
cargo run
```

Visit `http://127.0.0.1:8080` in your web browser to see your running application.

## Additional Resources

- [100% Pure Rust LAMP-inspired RASR Stack](https://github.com/ElasticProvisioner/RASR)
- [Redox OS Documentation](https://doc.redox-os.org/)
- [Actix Web Documentation](https://actix.rs/docs/)
- [Rocket Documentation](https://rocket.rs/v0.5-rc/guide/)
- [SurrealDB Documentation](https://surrealdb.com/docs)

## Contributing

Feel free to open issues or submit pull requests if you have suggestions for improvements or find any bugs.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Happy coding with the LASR-Stack! 🚀
