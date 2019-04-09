# March
A Rust library for In-Database CRUD.

## Examples

```Rust
let user = March::new()
                .query("select * from user where id = {{user_id}}")
                .params_map(json!({
                    "user_id": "38476a73"
                }))
                .as::<User>()
                .get();
```
