# March
A Rust library for In-Database CRUD.

## Examples

### Query for a single row:

```Rust
let user = March::new()
                .query("select * from user where user_id = {{user_id}}")
                .params_map(json!({
                    "user_id": "38476a73"
                }))
                .as::<User>()
                .get()
                .expect("Error getting user");
```

### Query for rows

```Rust
let devices = March::new()
                .query("select * from device where user_id = {{user_id}}")
                .params_obj(User {
                    "user_id": "38476a73"
                })
                .as::<Device>()
                .get_arr()
                .expect("Error getting user devices");
```

### Update a row

```Rust
March::new()
      .update("update user set mobile = {{mobile}}, update_time = LOCALTIMESTAMP where user_id = {{user_id}}")
      .params_val((mobile, user_id))
      .exec();
```

### Delete a row

```Rust
March::new()
      .update("delete from user where user_id = {{user_id}}")
      .params_val((user_id))
      .exec();
```
