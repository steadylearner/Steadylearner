<!-- Type this first -->

<!-- Title - How to build sitemap for images with Rust -->
<!-- Subtitle - Learn how to build image sitemap.xml with Rust Sitemap Crate -->
<!-- Image - https://www.steadylearner.com/static/images/code/image-sitemap.png-->
<!-- Description - Rust image-sitemap.xml example from the Steadylearner-->

<!-- Link here -->

[Rust Sitemap Crate]: https://github.com/svmk/rust-sitemap
[Steadylearner]: https://www.steadylearner.com
[Steadylearner Github Repository]: https://github.com/steadylearner/Steadylearner
[How to deploy Rust Web App]: https://medium.com/@steadylearner/how-to-deploy-rust-web-application-8c0e81394bd5?source=---------9------------------
[Rust Diesel]: http://diesel.rs/
[What is image sitemap]: https://support.google.com/webmasters/answer/178636

<!-- Post for this series -->

[How to build a static sitemap with Rust]: https://www.steadylearner.com/blog/read/How-to-build-a-static-sitemap-with-Rust
[How to build a dynamic sitemap with Rust Diesel]: https://www.steadylearner.com/blog/read/How-to-build-a-dynamic-sitemap-with-Rust-Diesel
[How to build a sitemap.txt from sitemap.xml with Rust]: https://www.steadylearner.com/blog/read/How-to-build-a-sitemap.txt-from-sitemap.xml-with-Rust
[How to build a sitemap for React App]: https://medium.com/@steadylearner/how-to-build-a-sitemap-for-react-app-7bbc3040dc1f


<!-- -->

This is the final post for the sitemaps. I hope you already read the previous posts [How to build a static sitemap with Rust][How to build a static sitemap with Rust], [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel] and [How to build a sitemap.txt from sitemap.xml with Rust][How to build a sitemap.txt from sitemap.xml with Rust].

The source code used for image sitemap.xml here should be separated from  the previous examples with the name like **image_sitemap.rs**.

<br />

<h2 class="red-white">[Prerequisite]</h2>

1. [Rust Sitemap Crate][Rust Sitemap Crate]
2. [What is image sitemap][What is image sitemap]
3. [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel] 
4. [What is sitemap](https://support.google.com/webmasters/answer/156184?hl=en) 
5. [How to build a sitemap](https://www.google.com/search?client=firefox-b-d&q=how+to+build+sitemap)

---

For this process is similar to [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel] and major difference is the data used is images instead of blog posts, I hope you already followed the post.

You should also read [What is image sitemap][What is image sitemap] because we will replicate the given example with Rust in this post.

I also hope that you already have database setup inside your project for images such as [Rust Diesel][Rust Diesel]. I will give you an example and you will need to modify it for your project.

If you prefer to see the final project first, you can refer to [Steadylearner Github Repository][Steadylearner Github Repository].

<br />

<h2 class="blue">Table of Contents</h2>

1. **Image_sitemap.rs** to write sitemap for images
2. **How to include it** inside your main sitemap.xml
3. **Conclusion**

---

<br />

## 1. Image_sitemap.rs to write sitemap for images

For we will separate the process to write sitemap for images, we will write **image_sitemap.rs **first. You can refer to **models.rs** and **cargo.toml** below if you want. 

(Your project will not be the xactly the same for these examples and you should find corresponding codes inside your project on your own)

```rust
// models.rs to use Rust Diesel, it will difer from your project.
#[derive(Debug, Queryable, Identifiable, Serialize, Deserialize)]
pub struct Image {
    pub id: i32,
    pub title: String,
    pub media_url: String,
    pub content: String,
}
```

<br />

```toml
# Inside cargo.toml, write the code like below
# You may modify it for your project

# $cargo run --bin <name> will point to the path we define here
# (Replace <name> to image-sitemap for this post)
[[bin]]
name = "main"
path = "src/bin/main.rs"
[[bin]]
name = "sitemap"
path = "src/bin/sitemap.rs"
[[bin]]
name = "image-sitemap" 
path = "src/bin/image_sitemap.rs"

# lib is used like crate and can import and export publicly 
# other files in the same directory level.
[lib]
name = "your_lib"
path = "src/lib.rs"
```

<br />

```rust
// image_sitemap.rs to write image_sitemap.xml
extern crate your_lib;
extern crate console;
extern crate diesel;

use std::fs;
use std::fs::File;
use std::fs::OpenOptions;
use std::io::prelude::*;
use std::str;

use std::io::BufReader;

use your_lib::models::*;
use your_lib::*;

use console::Style; // use just to decorate the output

use diesel::prelude::*;

fn main() -> std::io::Result<()> {
    // 1. 
    use crate::schema::images::dsl::*;
    let connection = init_pool().get().unwrap();

    let bold = Style::new().bold();

    let image_results = images
        .load::<Image>(&*connection)
        .expect("Error loading images");

    //

    println!(
        "\nIt starts to write image_sitemap.xml for {} images",
        bold.apply_to(image_results.len())
    );

    // Read https://support.google.com/webmasters/answer/178636?hl=en before
    // to understand what is necessary parts for image_sitemap.xml

    // 2.

    let start_xml = r#"<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
  <url>
    <loc>https://www.steadylearner.com</loc>
"#;

    fs::write("image_sitemap.xml", &start_xml)?;
    let mut result = OpenOptions::new().append(true).open("image_sitemap.xml").unwrap();

    let mut image_xml = String::new();
    // main part for the entire code to write image_sitemap.xml 
    for image in image_results {
        let image_url = format!(
            "    <image:image>
      <image:title>{}</image:title>
      <image:caption>{}</image:caption>
      <image:loc>https://www.steadylearner.com{}</image:loc>
      <image:license>https://www.steadylearner.com</image:license>
    </image:image>
",
            image.title,
            image.content,
            image.media_url,
            // image.title.replace(" ", "-")
        );
        image_xml.push_str(&image_url);
    }

    // 4. 
    if let Err(e) = writeln!(result, "{}{}", image_xml , r#"  </url>
</urlset> "#) {
        eprintln!("Couldn't write to file: {}", e);
    }

    println!("image_sitemap.xml was built. Include it to main sitemap.xml.");

    Ok(())
}
```

The examples are not so different from the ones from [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel]. I hope you followedthe post already.

We just needed to tweak them to be used for image sitemap. What we should do were

1. **Data setup for images** instead of posts 
2. **Manually start to write repeated parts** for image_sitemap.xml
3. Define mutable variable **image_xml** and later **pass the data** processed inside `for in` loop for  the paylod of image sitemap.
4. Manually end writing wrapper for image_sitemap.xml

and that was all the important process to write image sitemap. You can test it with `cargo c` or `cargo run --bin <name>` inside your console. 

It will create image_sitemap.xml similar to the **main image** of this post.

I hope it worked for your project also. Your project would be different from the example here. I want you to invest your time to find what is right for it.

For we could build sitemap for images using datas from the database, what we need to do next is to include it  inside your main sitemap.xml file. You will find that we end up using the almost all API that [Rust Sitemap Crate][Rust Sitemap Crate] offers to write sitemap.xml in the next part.

<br />

## 2. How to include it inside your main sitemap.xml

In the previous part, we built sitemap for images. For it is separtated from your main **sitemap.xml** we will use API from [Rust Sitemap Crate][Rust Sitemap Crate]. If you visited its examples page already, you should have noticed that the API to chain other sitemap was not used until this moment.

We will use it from now on and you can verfiy it inside the code snippet below. It is almost same for the sitemap.rs used for the [Steadylearner][Steadylearner].

```rust
// sitemap.rs
extern crate chrono;
extern crate console;
extern crate diesel;
extern crate sitemap;
extern crate your_lib;

use std::fs::{write, File};

use console::Style;
use diesel::prelude::*;

use chrono::{DateTime, FixedOffset, NaiveDate};
use sitemap::reader::{SiteMapEntity, SiteMapReader};
use sitemap::structs::{ChangeFreq, SiteMapEntry, UrlEntry};
use sitemap::writer::SiteMapWriter;

use your_lib::models::*;
use your_lib::*;

use your_lib::custom::str_from_stdin;

// inside custom.rs
// pub fn str_from_stdin() -> String {
//    let mut var = String::new();
//    stdin().read_line(&mut var).unwrap();
//    let var = var[..(var.len() - 1)].to_string(); // remove \n from the input
//
//    var
// }

fn main() -> std::io::Result<()> {
    // To decorate console output
    let bold = Style::new().bold();

    // Use database with Rust diesel to write sitemap.xml first
    use crate::schema::posts::dsl::*;
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

    let mut output = Vec::<u8>::new();
    {
        let sitemap_writer = SiteMapWriter::new(&mut output);

        let mut urlwriter = sitemap_writer
            .start_urlset()
            .expect("Unable to write urlset");

        let today = what_is_date_today();

        let date = DateTime::from_utc(
            NaiveDate::from_ymd(today.year, today.month, today.day).and_hms(0, 0, 0),
            FixedOffset::east(0),
        );

        let home_entry = UrlEntry::builder()
            .loc("http://www.steadylearner.com")
            .changefreq(ChangeFreq::Monthly)
            .lastmod(date) // priority is removed for some search engines ignore it and personal choice.
            .build()
            .expect("valid");
        urlwriter.url(home_entry).expect("Unable to write url");

        let static_routes: Vec<&str> = vec!["about", "video", "blog", "code", "image", "slideshow"];

        for route in static_routes.iter() {
            let static_url = format!("http://www.steadylearner.com/{}", route);
            let url_entry = UrlEntry::builder()
                .loc(static_url)
                .changefreq(ChangeFreq::Monthly)
                .lastmod(date)
                .build()
                .expect("valid");

            urlwriter.url(url_entry).expect("Unable to write url");
        }

        for post in post_results {
            let post_url = format!(
                "http://www.steadylearner.com/blog/read/{}",
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

        // 1.
        let sitemap_writer = urlwriter.end().expect("close the urlset block");

        // To link other sitemap to sitemap.xml(works as a index for other .xml type sitemap)
        println!("You wanna chain other .xml type sitemap here?");
        println!("Type yes for that or no to proceed to the nexts process");

        // 2.
        let choice = str_from_stdin()
            .chars()
            .next() // equals to .nth(0)
            .expect("string is empty");

        match choice {
            'y' => {
                // 3.
                let mut sitemap_index_writer = sitemap_writer
                    .start_sitemapindex()
                    .expect("start sitemap index tag");
                println!("Type path for the other sitemap");

                // 4.
                // Type react_sitemap.xml image_sitemap.xml(with empty spaces)
                let paths_for_other_sitemaps = str_from_stdin();

                let sitemap_paths_to_string = String::from(paths_for_other_sitemaps);
                let split_paths_for_other_sitemaps: Vec<&str> = sitemap_paths_to_string.split(" ").collect();
                 
                // 5.
                for path_for_other_sitemap in split_paths_for_other_sitemaps {
                    let entire_path_for_other_sitemap =
                        format!("https://www.steadylearner.com/{}", path_for_other_sitemap);

                    let sitemap_entry = SiteMapEntry::builder()
                        .loc(entire_path_for_other_sitemap)
                        .lastmod(date)
                        .build()
                        .expect("valid");

                    sitemap_index_writer
                        .sitemap(sitemap_entry)
                        .expect("Can't write the file");
                }

                // 6.
                sitemap_index_writer.end().expect("close sitemap block");
            }
            _ => println!("You may not need it. Let's move on to the next phase"),
        }
    }

    println!("{:#?}", std::str::from_utf8(&output));

    write("sitemap.xml", &output)?;

    // sitemap.txt is based on the sitemap.xml and it is already built at the point
    println!("You wanna write sitemap.txt file also?");
    println!("Type yes to write sitemap.txt or no to end the process");

    let choice = str_from_stdin().chars().next().expect("string is empty");

    match choice {
        'y' => {
            let mut urls = Vec::new();
            let mut sitemaps = Vec::new();
            let mut errors = Vec::new();

            let file = File::open("sitemap.xml").expect("Unable to open file.");
            let parser = SiteMapReader::new(file);
            for entity in parser {
                match entity {
                    SiteMapEntity::Url(url_entry) => {
                        urls.push(url_entry);
                    }
                    SiteMapEntity::SiteMap(sitemap_entry) => {
                        sitemaps.push(sitemap_entry);
                    }
                    SiteMapEntity::Err(error) => {
                        errors.push(error);
                    }
                }
            }

            // get_url from the source code, unwrap to get the payload value inside Some
            println!("payload = {:?}", urls[0].loc.get_url().unwrap());

            let mut output = String::new();

            for url in urls {
                let payload = url.loc.get_url().unwrap();
                println!("{}", &payload);
                let payload_with_new_line = format!("{}\n", &payload);
                output.push_str(&payload_with_new_line);
            }

            println!("{:#?}", &output);
            write("sitemap.txt", &output)?;

            println!("errors = {:?}", errors);
        }
        _ => {
            println!("The process ends here");
        }
    }

    Ok(())
}
```
The code snippet is long. But  I thought that it is the time to inlcude the enitre code snippet for it is the last post for sitemap with Rust. You can refer to the entire code snippet or take just necessary for your project. What we should include for the sitemap.rs or main.rs were

1. **Giving ownership of the process** to write sitemap to variable **sitemap_writer**
(It is important to include this code to the rest of the code to work)
2. **Consider only first letter** of user input
3. Note that **sitemap_write** variable is used again for this process. For normal process to write sitemap from **static path** and **the datas from the data** were ended before. We start to write **sitemap index** to chain other sitemaps before the end of writing entire sitemap.xml file.
4. The code here is to easily chain other .xml sitemap format
5. Use `for in` loop again to **use .xml files typed by user** at the process 4.
6. **End the entire process to write sitemap.xml** and pass the process to question users to whether they want to write sitemap.txt  file also or not

and that was all to explain the process to include image_sitemap.xml built before. You can use it to chain other .xml format sitemap also. It would be easy following the code snippet above.
 
<br />

## 3. Conclusion

That was a long code snippet and post. But I hope it worth reading and including it inside your project. With the help from the [Rust Sitemap Crate][Rust Sitemap Crate], we could save our time to write our **sitemap.xml** easily. 

If you read the other posts also, the main part of API of  the crate is 

```rust
let url_entry = UrlEntry::builder()
                .loc(post_url)
                .changefreq(ChangeFreq::Yearly)
                .lastmod(date)
                .build()
                .expect("valid");
``` 
and what we should code were just to modify and organize the projects for our purpose.

I hope you could have the entire code snippet work. If not, please refer to [Steadylearner Github Repository][Steadylearner Github Repository] and modify the examples for your project and read the documentations inside each post.

If you haven't read the other posts for sitemap with Rust before, please read them also.

1. [How to build a static sitemap with Rust][How to build a static sitemap with Rust]
2. [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel]
3. [How to build a sitemap.txt from sitemap.xml with Rust][How to build a sitemap.txt from sitemap.xml with Rust]

They will help you to understand the entire code snippet inside **sitemap.rs** here.

**Thanks and please share this post with others.**
