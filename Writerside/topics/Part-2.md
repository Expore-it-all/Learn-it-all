# Part 2 
_- Look at me Ma! No Hands!_
<warning>
Unfinished!
</warning>
Now that the easy things are done, we can start to look at what we need to do for this project.

First off, we are doing a database. We need to understand that we have two major entities. We have the `Client` and the `Server`.

The `Client` is any software that want to communicate with the database. This could be your new shiny, unicorn-isk, going to make 1 Million dollars application that you coded in a weekend.
Or the other program that you said the same thing but gave up on it after 3 days cause of another shinny thing caught your eyes.

We don't care who is connecting right now, the only thing we do care about is how they will communicate with us.
In this case we are going to use TCP to communicate.

I'm not going to go into depth on what TCP means or the low level work of it. _If you don't understand the basics of TCP then building a database might be a little bit too much for you to bite off._
 I am going to reference the [Rust Book](https://doc.rust-lang.org/stable/book/ch20-01-single-threaded.html) for the TPC here. This is for a single threaded webserver, but we can modify it for our own needs!
We are doing this quick and dirty right now, so no multi threading and KISS all the way!

In Rust when we want to listen to a socket for incoming connections we can use the TcpListener.
> We are going to use port 5656 for this project.
```Rust
let listener = TcpListener::bind("127.0.0.1:5656").unwrap();
```

Our main file should now look like:

```Rust
use std::net::TcpListener;

fn main(){
    let listener = TcpListener::bind("127.0.0.1:8080").unwrap();

}
```

Now We want to get a list of incoming connections. 

We can do that through
```Rust
for stream in listener.incoming()
```
> Though that we are doing these one at a time, which is bad for a database, we are going ahead with this for now to better understand the interactions.

