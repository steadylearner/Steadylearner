<!-- 
 Post{ 
   title: "How to use Rust to copy files at Github",
   subtitle: "Learn how to use Reqwest to save request.body in your machine.",
   image: "/code/Rust.svg",
   image_decription: "Rust Image from the website",
   tags: "Rust reqwest Github request",
   theme: "rust",
 }
-->

<!-- Steadylearner -->

[Steadylearner]: s-
[Steadylearner Github]: g-/steadylearner
[posts]: g-/steadylearner/Steadylearner
[Blog]: s-/blog
[Markdown]: https://www.steadylearner.com/markdown
[prop-passer]: https://www.npmjs.com/package/prop-passer
[How to write less code for links in markdown with React]:  s-/blog/read/How-to-write-less-code-for-links-in-markdown-with-React

<!-- \Steadylearner -->

<!-- Shortcut -->

[Rust]: https://www.rust-lang.org/
[JSON]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
[What is Binary Data]: https://www.techopedia.com/definition/17929/binary-data
[Reqwest]: https://github.com/seanmonstar/reqwest
[CURL]: https://curl.haxx.se/
[git]: https://git-scm.com/

<!-- \Shortcut -->

If you have used [CURL][CURL] before, you should know that you can print the page with
```curl
$curl https://www.rust-lang.org
```
and save it in your machine with a little POSIX knowledge. 
```
$curl https://www.rust-lang.org > rust-org.html
``` 
What we are going to learn in this post is to find how to use [Rust][Rust] to replace those comamnds. It won't be difficult for we already have [reqwest][reqwest] crate that can be used nstead of **CURL**.

The final goal of this post is to use it to download blog posts saved at [Github][posts]. You may wonder why you should use it instead of [git][git] commands. But, There are some benefits of using programming languages such as Rust and Python for this purpose instead of **git** and **CURL** etc.

1. You can make some predefined **variables** and functions to save you from typing same things repeatedly.  
2.  You can print some helper messages and you don't have to remember all requirements.
3. In case of **git**, you donwload .git files at the same time and its size grows over time and you shouldn't need it just to download a single file to your machine.

<br />

<h2 class="red-white">[Prerequisite]</h2>

1. [How to use Rust][Rust]
2. [Rust Reqwest Crate][Reqwest]
---

You should already know how to write [Rust][Rust] code before you read on. I also want you to to visit the [reqwest][reqwest] Github repository to briefly understand how to use it.

We will start from the [simple.rs](https://github.com/seanmonstar/reqwest/blob/master/examples/simple.rs) file  at the repository. I hope you read the other example files also before read on.
 
<br />

<h2 class="blue">Table of Contents</h2>

1. **How to print response.body** on your console with **Reqwest**
2. **How to save it** in your machine with **Rust**
3. **Conclusion**

---

<br />

## 1. How to print response.body on your console with Reqwest

It was not so difficult to find how to do it with **Rust**. For it has very simple way to do that already.

In the code snippet below
```rust
fn main() {
    let bianry:&amp;[u8] = b"rust";
    println!("{:?}", binary);
    // result in [114, 117, 115, 116]
}
```
what we need to do was just to put [`b`](https://medium.com/r/?url=https%3A%2F%2Fdoc.rust-lang.org%2Freference%2Ftokens.html%23byte-and-byte-string-literals) in front of the `&amp;str` type data(**"rust"**) and **Rust** does the process to convert **chars into binary** instead of you. By far, it was the easiest way comparing to how to do that in other programming languages. 

For example, in **JavaScript**
```js
const rust = "rust";
const binary = rust.split("").map(x => x.charCodeAt());
console.log(binary);
```

You can see that we don't need to use methods such as `split` and `map` for our purpose when we use Rust for the same purpose. 

What we need was just a single character `b` and nothing more.

<br />

## 2. Vice Versa

For we learnt how to encode our data into binary format in the previous part, we will learn how to decode it. Comparing to the previous process, it will be a little bit more complicated.

The entire code example for this part is 
```rust
fn main() {
    let binary: Vec<u8> = vec![114, 117, 115, 116]; // 1.
    let characters: Vec<char> = binary.into_iter().map(|x| x as char).collect(); // 2.
    let result: String = characters.into_iter().collect(); // 3.
    println!("{:?}", &amp;result); // 4
}
```

You should have found that we need much more code to make **chars**(characters) from **binary** datas.

1. In the previous part, we already found that **binary data** type equivalent to **"rust"** is `[114, 117, 115, 116]` so we will use it for the purpose of this process is to **decode** instead of encode.
2. We first define type of variable with `Vec<char>` for **characters** variable for **Rust** is typed language and you should help it to understand what you want to do. We need **char** type instead of **u8** type after passing this process with **binary.into_iter().map(|x| x as char).collect()**. Notice that difference to understand this code.
3. We already made vector of **chars** from the vector of u8(**binary data**) in the previous process. But it will not be easy to understand what data was in this format. So we turn it into **String** type to make it more human readable.
4. We use **println!** macro to show the result to our console and you will see "rust" when you execute this code in your machine.

You may found that it is not so easy. To understand this code completly, you should understand what each parts of it does with details. 

For that, You can use `$rustup doc --std` to find the documentation easily in your local machine.

Because the process used many times, it will be better for you to save the process inside function or macro and use it again whenever you want.

<br />

## 3. Conclusion

The code used here is simple. However, understanding it may not be that easy if you are new to system programming. But it will be useful if you just wanted to know how to **decode** and **encode** binary data in **Rust** and find working codes.

You can contribute to this post and other posts at [Github repository for blog][posts].

**Thanks and please share this post with others.**

