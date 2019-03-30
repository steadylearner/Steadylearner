[<!-- Link here -->

[Rust Sitemap Crate]: https://github.com/svmk/rust-sitemap
[Steadylearner]: https://www.steadylearner.com
[Steadylearner Github Repository]: https://github.com/steadylearner/Steadylearner
[How to deploy Rust Web App]: https://medium.com/@steadylearner/how-to-deploy-rust-web-application-8c0e81394bd5?source=---------9------------------
[Rust Diesel]: http://diesel.rs/

<!-- Post for this series -->

[How to build a static sitemap with Rust]: https://www.steadylearner.com/blog/read/How-to-build-a-static-sitemap-with-Rust

<!-- Title - How to build a dynamic sitemap with Rust Diesel -->
<!-- Subtitle - Learn how to build sitemap.xml with database.
<!-- Image - https://www.steadylearner.com/static/images/code/Rust-Diesel.png -->
<!-- Description - Rust Diesel Image from the official site -->

Before I dive deep to write a sitemap for [Steadylearner][Steadylearner], Which is built with React Frontend and Rust Backend, Building a sitemap for dynamic pages seemed to be solved problem for there were many automated way to do that. 

The site uses React for Frontend and there were already some sitemap builders in React development environment.

I tried to use them. But it wans't that helpful for the website with dynamic page using databases, because they just extract path parts from react-router.

So I thought that it would be better to find the solution at serverside using Rust because I already use Rust Diesel to handle database.

In this post, I will show you what I learnt from the proces. I hope you already followed the previous post [How to build a static sitemap with Rust][How to build a static sitemap with Rust] for in this post,  I will assume that you already read it.

<br />

<h2 class="red-white">[Prerequisite]</h2>

1. [Rust Sitemap Crate][Rust Sitemap Crate]
2. [Rust Diesel][Rust Diesel]
3. [What is sitemap](https://support.google.com/webmasters/answer/156184?hl=en) 
4. [How to build a sitemap](https://www.google.com/search?client=firefox-b-d&q=how+to+build+sitemap)

---

You don't have to visit all links above if you already knew them, I want you to follow Rust Diesel and Sitemap Crate examples and could make your own CLI to use CRUD operation for blog posts.

If you just want to see the final project, you may refer to [Steadylearner Github Repository][Steadylearner Github Repository].

<br />

<h2 class="blue">Table of Contents</h2>

1. Setup Diesel and Rust files
2. How to write sitemap with database with **Rust Diesel**
3. **Conclusion**

---

<br />

## 1. Setup Diesel and Rust files

We will first setup some Diesel and Rust codes before we write code for dynamic sitemap.xml. You may skip or use your own project instead if you are not interesed in this part. 


Let's start from the your_lib.rs you built before.

```rust
// In your_lib.rs you built before
// You should have code snippet similar to this
// If you followed the official documentation from Rust Disel
extern crate dotenv;
use dotenv::dotenv;
use std::env;

use diesel::pg::PgConnection;
use diesel::r2d2::{ConnectionManager, Pool, PooledConnection};

// use code beow when there is no rocket path relevant things?

use chrono::prelude::Utc;

type PgPool = Pool<ConnectionManager<PgConnection>>;
pub fn init_pool() -> PgPool {
    dotenv().ok();

    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    let manager = ConnectionManager::<PgConnection>::new(database_url);
    Pool::new(manager).expect("db pool")
}
```

It maybe different from your project depending on the database you use. You should have used this function inside your CLI for blog posts already. It will be used to call datas from the database such as blog posts etc.

```rust
// models.rs
use crate::schema::{posts};

#[derive(Debug, Queryable, Identifiable, Associations, AsChangeset, Serialize, Deserialize)]
pub struct Post {
    pub id: i32,
    pub title: String,
}

```
Inside the Post struct, You should have defined at least title parts for we will use that to write sitemap.xml dynamcially. It may be differ from your project and you may modify it. I hope you already have working codes for writing a sitemap should be the last process of your site.

<br />

## 2. How to write sitemap with database with **Rust Diesel**

I hope you followed well the first part of this post. In this part, we will write some codes to read data from the database and write sitemap.xml from it. 

If you followed the previous post well, You will find that the major difference from the previous post is that we just set paths with datas from the database.

```rust
// main.rs or whatever file to build sitemap.xml

// You should know how to use models for your project. It might be diffrent for each project
// or import models you want from models.rs
use your_lib::models::*; 
// Import everything and we will use init_pool function we defined above to use database
use your_lib::*;

fn main() -> std::io::Result<()> {
    // 1.
    use crate::schema::posts::dsl::*; // It will difer from your project, You should find what is corresponding code for this
    let connection = init_pool().get().unwrap();

    let post_results = posts
        .filter(published.eq(true))
        .order(created_at.desc())
        .load::<Post>(&*connection)
        .expect("Error loading posts");

    println!(
        "\nWrite sitemap for {} posts",
        bold.apply_to(post_results.len())
    );
    //

    let mut output = Vec::<u8>::new();

    {
      <Code Snippet from the previous post>
      
       // 2.
       for post in post_results {
          let post_url = format!(
            "https://www.steadylearner.com/blog/read/{}",
              post.title.replace(" ", "-")
          );
            // Use Monthly or Yeary
          let url_entry = UrlEntry::builder()
            .loc(post_url)
            .changefreq(ChangeFreq::Yearly)
            .lastmod(date)
            .build()
            .expect("valid");

           urlwriter.url(url_entry).expect("Unable to write url");
        }

        //
 
      <Code Snippet from the previous post>
    }

```
If you read the code snippet above, we included two Rust codes that work together.

1. We connected main function to database we defined above and write some codes to make our development process easier. You should understand that the data loaded from the process is saved at `post_results` variable and we can use it later.

2. We use `for in` loops in Rust again here. The major difference from the previous post is that we are using datas to iterate from the dabase instead of hard coded static paths we defined inside vector.

What is importnat here is that we define customized url(post_url here) to pass for the loc API from [Rust Sitemap Crate][Rust Sitemap Crate]. Other codes or parameters will be less important in the code snippet.

(**post.title.replace(" ", "-")** is used here to make url for readable in React Frontend and Rust Backend also. You should find what is right for your project.)

When you make this project compile, you will find that the 

```xml
<loc>https://www.steadylearner.com/blog/read/How-to-build-a-dynamic-sitemap-with-Rust-Diesel</loc>
```
and others with various blog titles is written for your sitemap.xml file. 

<br />

## 3. Conclusion

I hope you could find hints to write your dynamic sitemap.xml with it. For you already had the boiler plate to build sitemaps with Rust, It would be easier for you to follow this post than before.

What you really needed was just used `for in` loop again with different data source and preparation for that. The code snippet wouldn't work exactly the same for your project but I want you to understand what you need to make the process work.

In the next post, I will help you to write **sitemap.txt** file from the **sitemap.xml**. You should have read that some search engine uses it.

Thanks and please share this post with others.
