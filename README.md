# March
A Rust library for PostgreSQL In-Database CRUD. This crate assumes the usage of PostgreSQL as an `Object Database`.

## Examples

### Insert some data:

```Rust
let user = User {
              user_id: "38476a73",
              devices: vec![
                  Device {
                      device_id: "a916",
                  },
                  Device {
                      device_id: "445d",
                  },
              ],
           };
March::new()
      .save("insert into user {{user}}")
      .params_obj(user)
      .exec();
```

### Query for a single object:

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

### Query for child objects

```Rust
let devices = March::new()
                .query("select * from device where device_id in (
                            select unnest(devices) from user where user_id = {{user_id}} )")
                .params_obj(User {
                    user_id: "38476a73"
                })
                .as::<Device>()
                .get_vec()
                .expect("Error getting user devices");
```

### Update an object

```Rust
March::new()
      .modify("update user set mobile = {{mobile}}, update_time = LOCALTIMESTAMP where user_id = {{user_id}}")
      .params_val((mobile, user_id))
      .exec();
```

### Delete an object

```Rust
March::new()
      .modify("delete from user where user_id = {{user_id}}")
      .params_val((user_id))
      .exec();
```
