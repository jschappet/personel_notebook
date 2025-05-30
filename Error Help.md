Posted to:  
https://www.reddit.com/r/learnrust/comments/1kzcaln/rust_and_diesel_error_saving_to_db_wont_compile/


I've been having lots of trouble with diesel and having to get the exact match between the schema and the structs for my tables. Can someone help me troubleshoot this setup.

## SQL Schema

```sql
    CREATE TABLE mailing_list_subscribers (
id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
name TEXT NOT NULL,
email TEXT NOT NULL UNIQUE,
confirmed BOOLEAN DEFAULT FALSE not null,
confirmation_token TEXT,
unsubscribed BOOLEAN DEFAULT FALSE not null,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_mailing_list_subscribers_email ON mailing_list_subscribers(email);

CREATE INDEX idx_mailing_list_subscribers_confirmed ON mailing_list_subscribers(confirmed);

```
    
## Diesel Schema Definition


```rust

diesel::table! {
mailing_list_subscribers (id) {
		id -> Integer,
		name -> Text,
		email -> Text,
		confirmed -> Bool,
		confirmation_token -> Nullable<Text>,
		unsubscribed -> Bool,
		created_at -> Nullable<Timestamp>,
	}
}
```
## Rust Struct

```rust 
#[derive(Debug, Queryable, Selectable, Insertable, Identifiable, Serialize, Deserialize)]
#[diesel(table_name = crate::schema::mailing_list_subscribers)]
pub struct Subscriber {
	pub id: i32,
	pub name: String,
	pub email: String,
	pub confirmed: bool,
	pub confirmation_token: Option<String>,
	pub unsubscribed: bool,
	pub created_at: NaiveDateTime,

}
```

## Diesel Call 

```rust 
let mut conn = data.db_pool.get().expect("DB connection failed");
let existing = mailing_list_subscribers
	.filter(email.eq(&form.email))
	.select(Subscriber::as_select())
	.first::<Subscriber>(&mut conn)
	.optional()
	.expect("DB error");
```
### Compile Error

```shell 

error[E0277]: the trait bound `SelectBy<Subscriber, Sqlite>: CompatibleType<Subscriber, Sqlite>` is not satisfied
    --> src/mailing_list.rs:95:30
     |
95   |         .first::<Subscriber>(&mut conn)
     |          -----               ^^^^^^^^^ the trait `load_dsl::private::CompatibleType<Subscriber, Sqlite>` is not implemented for `expression::select_by::SelectBy<Subscriber, Sqlite>`
     |          |
     |          required by a bound introduced by this call
     |
     = note: this is a mismatch between what your query returns and what your type expects the query to return
     = note: the fields in your struct need to match the fields returned by your query in count, order and type
     = note: consider using `#[diesel(check_for_backend(Sqlite))]` on either `#[derive(Selectable)]` or `#[derive(QueryableByName)]` 
             on your struct `Subscriber` and in your query `.select(Subscriber::as_select())` to get a better error message
     = help: the trait `load_dsl::private::CompatibleType<U, DB>` is implemented for `expression::select_by::SelectBy<U, DB>`
     = note: required for `SelectStatement<FromClause<table>, ..., ..., ..., ..., ...>` to implement `LoadQuery<'_, _, Subscriber>`
note: required by a bound in `first`
    --> /Users/schappet/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/diesel-2.2.10/src/query_dsl/mod.rs:1779:22
     |
1776 |     fn first<'query, U>(self, conn: &mut Conn) -> QueryResult<U>
     |        ----- required by a bound in this associated function
...
1779 |         Limit<Self>: LoadQuery<'query, Conn, U>,
     |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `RunQueryDsl::first`
```