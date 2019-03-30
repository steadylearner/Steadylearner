<!-- Type this first -->

<!-- Title - How to build a sitemap.txt from sitemap.xml with Rust -->
<!-- Subtitle - Learn how to build sitemap.txt easily with Rust -->
<!-- Image - https://www.steadylearner.com/static/images/code/txt-sitemap.png-->
<!-- Description - Rust Sitemap.txt example from the Steadylearner-->

<!-- Link here -->

[Rust Sitemap Crate]: https://github.com/svmk/rust-sitemap
[Steadylearner]: https://www.steadylearner.com
[Steadylearner Github Repository]: https://github.com/steadylearner/Steadylearner
[How to deploy Rust Web App]: https://medium.com/@steadylearner/how-to-deploy-rust-web-application-8c0e81394bd5?source=---------9------------------
[Rust Diesel]: http://diesel.rs/

<!-- Post for this series -->

[How to build a static sitemap with Rust]: https://www.steadylearner.com/blog/read/How-to-build-a-static-sitemap-with-Rust
[How to build a dynamic sitemap with Rust Diesel]: https://www.steadylearner.com/blog/read/How-to-build-a-dynamic-sitemap-with-Rust-Diesel
[How to build a sitemap for React App]: https://medium.com/@steadylearner/how-to-build-a-sitemap-for-react-app-7bbc3040dc1f

<!-- -->

This is a bonus post from the previous ones to build sitemap.xml files. I hope you already read [How to build a static sitemap with Rust][How to build a static sitemap with Rust] and [How to build a dynamic sitemap with Rust Diesel][How to build a dynamic sitemap with Rust Diesel]. 

 The source code used here can be separtaed or included in your previous Rust(.rs) file to build sitemap.xml.

<br />

<h2 class="red-white">[Prerequisite]</h2>

1. [Rust Sitemap Crate][Rust Sitemap Crate]
2. [What is sitemap](https://support.google.com/webmasters/answer/156184?hl=en) 
3. [How to build a sitemap](https://www.google.com/search?client=firefox-b-d&q=how+to+build+sitemap)
4. Your sitemap.xml

---

For this process is simple, you don't have to visit all links above if you already knew them. I just want you to read and test the code snippet from **Reading sitemap documents** part at [Rust Sitemap Crate][Rust Sitemap Crate] before move on. 

If you are a React developer and you didn't have any sitemap.xml or sitemap.txt file yet, you may read [How to build a sitemap for React App][How to build a sitemap for React App] also to build them. It will be also helpful for you to learn different way to do the same thing with other programming lagunage.

If you prefer to see the final project first, you can refer to [Steadylearner Github Repository][Steadylearner Github Repository].



<br />

<h2 class="blue">Table of Contents</h2>

1. Test official example to read sitemap.xml
2. How to modify it to write **sitemap.txt**
3. **Conclusion**

---

<br />

## 1. Test official example to read sitemap.xml

We will first take a look at the official example to read sitemap.xml and find the exact part(payload) we want to use from it. I hope you already tested it in your local machine. 

The code snippet below is exactly same from the [Rust Sitemap Crate][Rust Sitemap Crate].

```rust
extern crate sitemap;
use sitemap::reader::{SiteMapReader,SiteMapEntity};
use std::fs::File;
fn main() {
    let mut urls = Vec::new();
    let mut sitemaps = Vec::new();
    let mut errors = Vec::new();
    let file = File::open("sitemap.xml").expect("Unable to open file.");
    let parser = SiteMapReader::new(file);
    for entity in parser {
        match entity {
            SiteMapEntity::Url(url_entry) => {
                urls.push(url_entry);
            },
            SiteMapEntity::SiteMap(sitemap_entry) => {
                sitemaps.push(sitemap_entry);
            },
            SiteMapEntity::Err(error) => {
                errors.push(error);
            },
        }
    }
    println!("urls = {:?}",urls);
    println!("sitemaps = {:?}",sitemaps);
    println!("errors = {:?}",errors);
}
```

After having tested this code snippet, you should have seen that some urls and maybe some other sitemaps you included and hopefully no errors.

For sitemap.txt just need **url parts separated by new lines(\n)**, we can think that what we need is just `urls` variable and its relevant codes to build your **sitemap.txt**. 

(**sitemap in .txt format allows you to use all selector * to define url also**. If you followed the previous posts given above, you should already have all dynamic pages you want to include in sitemap.xml and you wouldn't need to use it. But you can modify the sitemap for your preference manually if you want.)

<br />

## 2. How to modify it to write sitemap.txt

In the previous part, What the code snippet do is to **read the sitemap.xml** and show important parts from such as urls, other sitemaps and errors if they exist. So we should modify it to **write sitemap.txt**.

We will find how to do it by reading the code snippet below. 

```rust
// If you want to separate the process to write 
// Save the code snippet below to txt_sitemap.rs or your preference
extern crate sitemap;
use sitemap::reader::{SiteMapReader,SiteMapEntity};
use std::fs::{write, File};
fn main() {
    let mut urls = Vec::new();
    let mut sitemaps = Vec::new();
    let mut errors = Vec::new();
    let file = File::open("sitemap.xml").expect("Unable to open file.");
    let parser = SiteMapReader::new(file);
    for entity in parser {
        match entity {
            SiteMapEntity::Url(url_entry) => {
                urls.push(url_entry);
            },
            SiteMapEntity::SiteMap(sitemap_entry) => {
                sitemaps.push(sitemap_entry);
            },
            SiteMapEntity::Err(error) => {
                errors.push(error);
            },
        }
    }
    
    // You should read the source code to find what get_url does exactly
    // It is a method that return url inside Some()
    // You should unwrap() to get the payload value inside it
    println!("payload = {:?}", urls[0].loc.get_url().unwrap()); // 1.

    let mut output = String::new(); // 2.
    
    for url in urls { 
        let payload = url.loc.get_url().unwrap();
        println!("{}", &payload);
        let payload_with_new_line = format!("{}\n", &payload);
        output.push_str(&payload_with_new_line);
    }
    
    // 3.
    println!("{:#?}", &output);
    write("sitemap.txt", &output)?;

    println!("errors = {:?}", errors);
}
```

The entire code isn't that different from the official example. What we should do were

1. To find the way to get url from the API from [Rust Sitemap Crate][Rust Sitemap Crate]. For it is the only part that we are interested. We could verfiy it with a code below.
```rust
println!("payload = {:?}", urls[0].loc.get_url().unwrap());
// It prints payload = https://www.steadylearner.com alike.  
```

2. Then we create mutable variable **output** with **let mut output = String::new()**; to save data and we use `for in` loop again to use data inside **urls** variable we created and processed with API from [Rust Sitemap Crate][Rust Sitemap Crate].

3. Finally, we verfiy everything worked well with **println!("{:#?}", &output);** first and then write sitemap.txt with **write("sitemap.txt", &output)?;**

In the process, we didn't remove **println!("errors = {:?}", errors);** to verify erros if there are any.

(You may wonder why `for in` loop is repeatedly used in all processes for sitemaps. You can use FP way of programming if you want. It was used because it is in the official examples of the crate.)

If you want to include it inside your former project, You may refer to the code snippet below for that.

```rust
// main.rs, sitemap.rs or whatever you want 
fn main() -> std::io::Result<()> {
    <Code snippet>

    // sitemap.txt is based on the sitemap.xml and it is already built at the point 
    // So the code snippet below to build sitemap.txt should be at the end
    println!("You wanna write sitemap.txt file also?");
    println!("Type yes to write sitemap.txt or no to end the process");

    // use this syntax to care for only the first word user type 
    // and save time when there were error in typo but user intetion is the same
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
            // main point of this code snippet
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

and that is all. You can test it with `Cargo c or cargo run` and verify the result. I hope it worked well in your project.

By following this post, you should have your **sitemap.txt** built from **sitemap.xml**. If there was any problem, please refer to [Steadylearner Github Repository][Steadylearner Github Repository] and the documenation at [Rust Sitemap Crate][Rust Sitemap Crate].

<br />

## 3. Conclusion

I hope you could build sitemap.xml from static routes and the dynamic datas from the database with Rust in the previous posts. After following this post, you should be able to build sitemap.txt from the ones you built before also.

It will be sufficient if you wanted to built porfolio porjects such as blog etc. In the next post, we will learn how to build image sitemap.xml file. The entire process will be similar to previous ones and that will be the end of series to build sitemap with Rust.

(I woudln't handle how to build video sitemap or news sitemap with this for there is YouTube that already handles many video relevant things and you shouldn't need to build new sitemap normally.)

Thanks and please share this post with others.
