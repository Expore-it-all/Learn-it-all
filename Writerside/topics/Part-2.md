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
```rust
let listener = TcpListener::bind("127.0.0.1:5656").unwrap();
```

Our main file should now look like:

```rust
use std::net::TcpListener;

fn main(){
    let listener = TcpListener::bind("127.0.0.1:5656").unwrap();

}
```

Now We want to get a list of incoming connections. 

We can do that through looping through the incoming connections. 
```rust
for stream in listener.incoming()
```
Now we are getting something. We can technically read out the stream, but we don't have any idea what we are doing with it just yet.
For now we can just print out "Connection established".
```rust
use std::net::TcpListener;

fn main(){
    let listener = TcpListener::bind("127.0.0.1:5656").unwrap();
    for stream in listener.incoming() {
        let stream = stream.unwrap();

        println!("Connection established!");
    }
}
```
> Though that we are doing these one at a time, which is bad for a database, we are going ahead with this for now to better understand the interactions.

Now we are getting a connection! Well sort of. How do we test this? Well, one thing that you can do is attempt a HTTP conection to the server. HTTP uses TCP under the hood. Now we are not going to respond to it, but we can make an initial connection and see if it works or not.

For this I am going to use curl
```Bash
curl http://localhost:5656
```
Now curl isn't happy with the response of the server. We are not implementing the HTTP protocol, so it never got anything that it expected, but we can see that a connection or two happened on our server!

```Text
curl : The underlying connection was closed: An unexpected error occurred on a receive.
At line:1 char:1
+ curl http://localhost:5656
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

```
```Text
cargo run
Connection established!
Connection established!
```

> Why more than one connection? I think it has to do with curl not getting data back and retrying the connection to make sure that it wasn't a network failure.

Now that we can technically get data, we need to be able to read it and process it.



