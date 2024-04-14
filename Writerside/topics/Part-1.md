# Part 1
_Init project, the Sisyphus of development_

Starting out we want to generate our project. Assuming that you actually read the text above, you should have cargo accessible to you in your `PATH`

1. Execute the following command in the terminal:

   ```bash
    cargo init
   ```

2. From there we want to do an initial build, this helps at telling us that at least the basic hello world will work.

   ```Bash
   cargo run
   ```
3. You should have a dir that has `main.rs` in it. It should looks something simular to:
   ```rust
   fn main() {
   // Statements here are executed when the compiled binary is called.

   // Print text to the console.
   println!("Hello World!");
   }
   ```
Simple Right? Well lets get to programing!