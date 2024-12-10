<hr/>
### Creating a Project with `Cargo`

1.  To create a new project:
```bash
    $ cargo new <projname>
```

2.  It will create a new dir based on the __`<projname>`__,\
    and inside it you will have a __`Cargo.toml`__ file and a __`src/`__ directory with __`main.rs`__ file inside.

3.  It also initialized a new Git repo along with a __`.gitignore`__ file.\
    Git files won't be generated if you run __`cargo new`__ within an exisiting Git repo.\
    You can override this behavior by using __`cargo new --vcs={ none | git | hg | pijul | fossil }`__.

3.  Cargo expects your source files to live inside the __`src/`__ directory.\
    The top-level project directory is just for README files, license information, configuration files,
    and anything else not related to your code.


<hr/>
### Building and Running a Cargo Project

1.  To build your project inside the directory:
```zsh
    $ cargo build
```
- This wil create an executable in __`./target/debug/`__ dir rather than your curr dir,\
and why it's inside a debug dir it's because the default build is a debug build.

\

2.  Running __`cargo build`__ for the first time also causes __`Cargo`__ to create a new file at the top level, which is __`Cargo.lock`__.\
    This file keeps track of the exact versions of dependencies in your project.\
    You will NOT ever need to change this file manually, __`Cargo`__ manages it for you.

\

3.  We can also ___compile + run___ the code in one command:
```zsh
    $ cargo run
```
\

4.  Cargo also provides a command called __`cargo check`__.\
    This command quickly checks your code to make sure it compiles but does `NOT` produce an executable.\
    And this command is much faster than __`cargo build`__.
```zsh
    $ cargo check
```



<hr/>
### Building for Release

1.  When your project is finally ready for release, you can use __`cargo build --release`__ to compile it with optimizations.

2.  This command will create an executable in __`target/release/`__.



<hr/>
### Updating a Crate to Get a New Version

1.  When you do want to update a __`crate`__, Cargo provides the command __`cargo update`__,\
    which will ignore the __`Cargo.lock`__ file and figure out all the latest versions that fit your configurations in __`Cargo.toml`__.\
    And Cargo will then write those versions to the __`Cargo.lock`__ file.

2.  Like if you wrote __`rand="0.8.5"`__ even when updating cargo will only look for versions of `rand` crate
    greater than or equal to `0.8.5` and lower than `0.9.0`.\
    If you wrote __`rand="0.9.0"`__ then it would also notice the change in `0.9.x` series.



<hr/>
### Cargo Documentations

1.  __`cargo doc --open`__ will build documentation provided by all your dependencies locally and open it in your browser.


<hr/>
### let

1.  __`let`__ defines a constant local variable.


<hr/>
### let mut

1.  __`let mut`__ defines a mutable local variable.


<hr/>
### const

1.  __`const`__ `MUST` be used with type annotation, and defines a global-scoped const variable.


<hr/>
### Shadowing

1.  Example `1`:
```rust
    fn main() {
        let x = 5;
        let x = x + 1;

        {
            let x = x * 2;
            println!("The value of x in the inner scope is: {x}");
        }

        println!("The value of x is: {0}", x);
    }

    /*
        The value of x in the inner scope is: 12
        The value of x is: 6
    */
```
\

2.  Example `2`:
```rust
    let spaces = "   ";
    let spaces = spaces.len();


    /* This is not possible, but above possible.
     * An advantage of shadowing.

        let mut spaces = "   ";
        spaces = spaces.len();

    */
```
- The only difference between shadowing and `mut` is that it allows you to change variable types.



<hr/>
### Data Types

1.  Keep in mind that Rust is a __`statically typed`__ language,\
    which means that it must know the types of all variables at compile time.

2.  The compiler can usually infer what type we want to use based on the value and how we use it.\
    In cases when many types are possible, such as when we converted a __`String`__ to a numeric type using __`parse()`__ method,\
    we have to add a `type annotation`.
```rust
    // If we did NOT annote `:u32` it will raise a compile error!
    let guess: u32 = "42"
                        .parse()
                        .expect("NaN!");
```


<hr/>
### Scalar Types

1.  A scalar type represents a single value.\
    Rust has four primary scalar types:
    a) `integer`
    b) `floating point`
    c) `boolean`
    d) `character`


<hr/>
### Integer Types

1. 8-bit __`i8`__, __`u8`__

2. 16-bit __`i16`__, __`u16`__

3. 32-bit __`i32`__, __`u32`__

4. 64-bit __`i64`__, __`u64`__

5. 128-bit __`i128`__, __`u128`__

6. arch, __`size`__, __`usize`__, depends on the architecture of the machine you're running on,\
    e.g. it would be `64-bit` if you're running on a `64-bit architecture`.


<hr/>
### Integer literals

1.  Decimal\
    e.g. `1_000`

2.  Hex\
    e.g. `0xff`, `0xFF`, `0Xff`, `0XFF`

3.  Octal\
    e.g. `0o77`, `0O77`

4.  Binary\
    e.g. `0b1111_0000`, `0B1111_0000`

5.  Byte (`u8` only)\
    e.g. `b'A'`

6.  You can also use a suffix to designate the type,\
    e.g. `57u8`, `57U8`

7.  Integer types are default to __`i32`__.


<hr/>
### Inetger Overflow

1.  Let's say you have a variable of type __`u8`__ that can hold values between __`0`__ and __`255`__.\
    If you try to change the variable to a value outside that range, such as __`256`__,\
    integer overflow will occur, which can result in one of two behaviors.

    When you're compiling in __`debug`__ mode,\
    Rust includes checks for integer overflow that cause your program to panic at runtime if this behavior occurs.\
    Rust uses the term panicking when a program exits with an error.

\

2.  When you're compiling in __`release`__ mode with the __`--release`__ flag,\
    Rust does `NOT` include checks for integer overflow that cause panics.

    Instead, if overflow occurs, Rust performs `two's complement wrapping`.\
    In short, values greater than the maximum value the type can hold "wrap around"
    to the minimum of the values the type can hold.

    In the case of a __`u8`__, the value __`256`__ becomes __`0`__, the value __`257`__ becomes __`1`__, and so on.


<hr/>
### Floating-Point Types

1.  Rust also has two primitive types for floating-point numbers __`f32`__ and __`f64`__.\
    Which are `32`-bit and `64`-bit in size, respectively.

\

2.  The default type is __`f64`__ because on modern CPUs,\
    it's roughly the same speed as __`f32`__ but is capable of more precision.
```rust
    fn main() {
        let x = 2.0;        // :f64

        let y: f32 = 3.0;   // :f32
    }
```
\

3.  All floating-point types are `signed`.

\

4.  Rust also supports the basic mathematical operations you'd expect for all the number types.
```rust
    {
        let sum = 5 + 10;
    }

    {
        let diff = 95.3 - 4;
    }

    {
        let production = 4 * 30;
    }

    {
        let quotient  = 55.7 / 11.2;
        let truncated = -5   / 3;        // -> -1
    }

    {
        let remainder = 43 % 5;
    }
```


<hr/>
### The Boolean Type

1.  A __`bool`__ type has only two possible values: __`true`__ and __`false`__.
```rust
    fn main() {
        let b1 = true;
        let b2: bool = false;
    }
```

<hr/>
### The Character Type

1.  The __`char`__ type is the language's most primitive alphabetic type.
```rust
    fn main() {
        let c1 = 'z';
        let c2: char = 'Z';
        let heart_eyed_cat = '😻';
    }
```
2.  However, a __`char`__ is still yet far from an Unicode type.\
    Tho like ASCII, accented letters, Chinese, Japanese, Korean characters and some emojis are valid.


<hr/>
### Compound Types

1.  Compound types can group multiple values into one type.\
    Rust has two primitive compound types: __`tuple`__ and __`array`__.


<hr/>
### The Tuple Type

1.  A __`tuple`__ is a general way of grouping together a number of values,\
    with a variety of types into one compound type.\
    Tuples have a `fixed length`, once declared, they can `NOT` grow or shrink in size.

\

2.  We create a tuple by writing a `comma-separated` list of values inside `parentheses`.   
    Each position in the tuple has a type,\
    and the types of the different values in the tuple don’t have to be the same.
```rust
    fn main() {
        let tup1: (i32, f64, u8) = (1989, 6.4, 1);  // :(i32, f64, u8)
        let tup2 = (1989, 6.4, 1);                  // :(i32, f64, i32)

        let (x, y, z) = tup1;
        println!("The value of y is {y}");          // -> 6.4

        let ninteen_eighty_four = tup2.0;
        let june_fourth = tup2.1;
        let one = tup2.2;
    }
```
\

3.  You can use pattern matching to de-construct a tuple value,\
    also you can use `0`-indexed index followed by a period (__`.`__) to access the element.

\

4.  The tuple without any values has a special name, __`unit`__.    
    This value and its corresponding type are both written __`()`__,\
    and represent an empty value or an empty return type.

\

5.  Expressions implicitly return the __`unit`__ value if they don’t return any other value.



<hr/>
### The Array Type

1.  Unlike tuple, every element of an array must have the ___same type___.    
    And unlike arrays in some other languages, arrays in Rust have a `fixed length`.

\

2.  We write the values in an array as a comma-separated list inside square brackets:
```rust
    fn main() {
        let arr = [1, 2, 3, 4, 5];
    }
```
\

3.  An array is NOT as flexible as the vector type, though.    
    A vector is a similar collection type provided by the standard library that is allowed to grow or shrink in size.   
    Arrays are useful when you want your data allocated on the `stack` rather than `heap`.

\

4.  You write an array’s type using square brackets with the type of each element, a semicolon,     
    and then the number of elements in the array, like so:
```rust
    let arr: [i32; 5] = [1, 2, 3, 4, 5];
```
\

5.  You can also initialize an array to contain the same value for each element by specifying the initial value,    
    followed by a semicolon, and then the length of the array in square brackets,   
    as shown here:
```rust
    // [value; count]
    let arr = [3; 5];   // [3, 3, 3, 3, 3]
```
\

6.  Arrays are `0`-indexed. Out-of-index checks are performs in run-time.
```rust
    fn main() {
        let arr = [1, 2, 3, 4, 5];


        let first  = arr[0];
        let second = arr[1];


        let mut index = String::new();
        std::io::stdin()
                .read_line(&mut index)
                .expect("Failed to read a line!");


        let index: usize = index
                            .trim()
                            .parse()
                            .expect("Index entered was not a number!");


        if index < 5 {
            let elem = arr[index];
            println!("The value at index {index} is {elem}");
        }
    }
```

<hr/>
### Functions

1.  Rust code uses `snake_case` as the conventional style for _functions_ and _variables_,      
    in which all letters are lowercase and underscores separate words.

\

2.  We define a function in Rust by entering __`fn`__ followed by a function name and a set of parentheses.     
    The curly brackets tell the compiler where the function body begins and ends.
```rust
    fn main() {
        println!("main function.");
        anoth_fn();
    }

    fn anoth_fn() {
        println!("Another function.");
    }
```
\

3.  In function signatures, you must declare the type of each parameter.    
    When defining multiple parameters, separate the parameter declarations with commas.
```rust
    fn main() {
        display_items('A', 100);
    }

    fn display_items(name: char, count: u32) {
        println!("There are {count} {name}(s)");
    }
```
\

4.  Functions can return values to the code that calls them.\
    We don’t name return values, but we must declare their type after an arrow (__`->`__).\
    We can return early from a function by using the __`return`__ keyword and specifying a value,\
    but most functions return the last expression implicitly.
```rust
    fn five() -> i32 {
        5
    }

    fn plus_one(x: i32) -> i32 {
        x + 1
    }

    fn main() {
        let x = five();
        println!("{x}");    // -> 5

        let x = plus_one(10);
        println("{x}");     // -> 11
    }
```

<hr/>
### Statements and Expressions

1.  Function bodies are made up of a series of `statements` optionally ending in an `expression`.

\

2.  Because Rust is an expression-based language, this is an important distinction to understand.\
    `Statements` are instructions that perform some action and do not return a value.\
    `Expressions` evaluate to a resultant value.

\

3.  For example, creating a variable and assigning a value to it with the `let` keyword is a statement.\
    Statements do not return values.

    Therefore, you can’t assign a `let` statement to another variable,\
    as the following code you’ll get a compiler error:
```rust
    // WRONG!!
    fn main() {

        let x = (let y = 6);    // ERR

        let x = let y = 6;      // OK
    }
```
\

4.  Calling a function is an expression.\
    Calling a macro is an expression.\
    A new scope block created with curly brackets is an expression.
```rust
    fn main() {
        let y = {
            let x = 10;
            x + 1
        };
        println("{y}");         // -> 11
    }
```
-   NOTE here that you can NOT write `x + 1;` at the end of the block,\
    because this way the value returned beomces __`()`__ which is an __`unit`__.
    It's like Pascal.


<hr/>
### Control Flow

1.  An __`if`__ expression allows you to branch your code depending on conditions.
```rust
    fn main() {
        let number = 3;


        if number < 5 {
            println!("Too small!");
        } else if number > 5 {
            println!("Too big!");
        } else {
            println!("Perfect!");
        }
        // The semicolon is not required,
        // after an if statement that executes as a standalone control structure (which is this case),
        // but it does no harm if you include it.
    }
```
\

2.  It’s also worth noting that the condition in the control flow must be a __`bool`__.\
    If the condition is `NOT` a __`bool`__, we’ll get an error.\
    For example, try running the following code:
```rust
    fn main() {
        let number = 3;

        let x = if number {
                // ^^^^^^
                // expected `bool`, found `integer`
            number * 2
        } else {
            0
        };      // This semicolon is for terminating the whole assignment statement.
    }
```
-   And also `NOTE` that if you use __`if`__ as an expression `MUST` provide an __`else`__ block!

\

3.  When the __`if`__ expression is the last thing in a block or a function, it returns a value,\
    and you should `NOT` use a semicolon, because that would prevent it from returning the desired value.
```rust
    fn get_value(number: i32) -> i32 {
        if number == 3 {
            1
        } else {
            0
        }       // No semicolon!!
    }

    fn main() {
        let result = get_value(3);
        println!("{result}");
    }
```
\

4.  And here explaining why assignment needs an semicolon (__`;`__) to end but as return expression it does not.\
    Because the assignment is essentially an `(let x = ...) (;)` which needs a semicolon to end.

\

5.  Also notice, __`if`__ expression must return the same type when used in a `let` statement.\
    The following code will cause a compiler error:
```rust
    fn main() {
        let cond = true;

        let number = if cond { 10 } else { "zero" };
                            // _            ^^^^ expected integer, found `&str`
                            // |
                            // expected beacuse of this
    }
```
\

6.  The __`loop`__ keyword tells Rust to execute a block of code over and over again forever,\
    until you explicitly tell it to stop.
```rust
    fn main() {
        loop {
            println!("Again!");
        }
    }
```
\

7.  You can add the value you want returned after the __`break`__ expression you use to stop the __`loop`__.
```rust
    fn main() {
        let mut counter = 0;


        let result = loop {
            counter += 1;

            if counter == 10 {
                // break <label> <expression>
                break counter + 1;
            }
        };


        println!("{result}");   // -> 11
    }
```
\

8.  If you have loops within loops,\
    __`break`__ and __`continue`__ only apply to the innermost loop at that point.

    You can optionally specify a `loop label` on a loop\
    that you can then use with __`break`__ or __`continue`__ to specify that
    those keywords apply to the labeled loop instead of the innermost loop.

    Loop labels must begin with a single quote (`'`).
```rust
    fn main() {
        let mut count = 0;


        'outer: loop {
            println!("{count}");

            let mut remaining = 10;

            'inner: loop {
                println!("{remaining}");

                if remaining == 9 {
                    break;
                }
                if count == 2 {
                    break 'outer;
                }

                remaining -= 1;
            }

            count += 1;
        }


        println!("End.");
    }
```
\

9.  Rust has a built-in language construct for __`while`__ loop.
```rust
    fn main() {
        let mut count = 3;

        while count != 0 {
            println!("{count}");
            count -= 1;
        }
    }
```
\

10.  You can use a __`for`__ loop and execute some code for each item in a collection.
```rust
    fn main() {
        let arr = [10, 20, 30, 40, 50];

        for elem in arr {
            println!("{elem}");
        }

        // (1..4)  is 1,2,3
        // (1..=4) is 1,2,3,4
        // parentheses are optional, added it for better calling the .rev() method.
        // .rev() means to reverse the sequence.
        for number in (1..4).rev() {
            println!("{number}");
        }
    }
```

<hr/>
### What is Ownership?

1.  Ownership is a set of rules that govern how a Rust program manages memory.

2.  All programs have to manage the way they use a computer’s memory while running.\
    Some languages have garbage collection (GC) that regularly looks for no-longer-used memory as the program runs,\
    in other languages, the programmer must explicitly allocate and free the memory.

3.  Rust uses a third approach:\
    memory is managed through a system of ownership with a set of rules that the compiler checks.\
    If any of the rules are violated, the program won’t compile.\
    None of the features of ownership will slow down your program while it’s running.


<hr/>
### The Stack and the Heap

1.  Both the __`stack`__ and the __`heap`__ are parts of memory available to your code to use at runtime,\
    but they are structured in different ways.

\

2.  The __`stack`__ stores values in the order it gets them and removes the values in the opposite order.\
    This is referred to as last in, first out (FILO).

    Adding data is called pushing onto the stack, and removing data is called popping off the stack.\
    All data stored on the stack must have a `known, fixed size`.\
    Data with an `unknown size` at compile time or a size that might `change` MUST be stored on the `heap` instead.

\

3.  The __`heap`__ is less organized, when you put data on the heap, you request a certain amount of space.

    The memory allocator finds an empty spot in the `heap` that is big enough,\
    marks it as being in use, and returns a `pointer`, which is the address of that location.\
    This process is called allocating on the heap and is sometimes abbreviated as just allocating\
    (pushing values onto the stack is not considered allocating).

    Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack,\
    but when you want the actual data, you must follow the pointer.

\

4.  Pushing to the __`stack`__ is `faster` than allocating on the heap\
    because the allocator never has to search for a place to store new data,
    that location is always at the top of the stack.

    Comparatively, allocating space on the heap requires more work\
    because the allocator must first find a big enough space to hold the data
    and then perform bookkeeping to prepare for the next allocation.

\

5.  Accessing data in the __`heap`__ is `slower` than accessing data on the stack\
    because you have to follow a pointer to get there.\
    Contemporary processors are faster if they jump around less in memory.

\

6.  When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap)\
    and the function’s local variables get pushed onto the stack.\
    When the function is over, those values get popped off the stack.

\

7.  Keeping track of what parts of code are using what data on the heap,\
    minimizing the amount of duplicate data on the heap,\
    and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses.


<hr/>
### Ownership Rules

1.  Every value in Rust has an __`owner`__.

2.  There can only be `ONE` __`owner`__ at a time.

3.  When the __`owner`__ goes out of scope, the value will be dropped.



<hr/>
### Varable Scope

1.  A scope is the range within a program for which an item is valid.\
    e.g. a function, a code block, etc.
```rust
    {                       // `s` is not valid here, it's not yet declared.

        let s = "hello";    // `s` is valid from this point.

        // ...
    }                       // This scope is now over, and `s` is no longer valid.
```
\

2.  To illustrate the rules of ownership,\
    we need a data type that is more complex than those we covered in the previous sections.\
    The __`String`__ type is a great example.

    We’ve already seen string literals, where a string value is hardcoded into our program.\
    String literals are convenient, but they aren’t suitable for every situation in which we may want to use text.

    One reason is that the string literal isn't immutable.\
    Another is that not every string value can be known when we write our code,\
    for example, what if we want to take user input and store it?

    For these situations, Rust has a second string type, __`String`__.\
    This type manages data allocated on the `heap`,\
    and as such is able to store an amount of text that is unknown to us at compile time.\
    You can create a __`String`__ from a string literal using the __`from( )`__ function, like so:
```rust
    let hello = "hello";                // :&str

    let mut s = String::from("Hello");  // :String
    s.push_str(", world!");
    println!("{s}");                    // -> Hello, world!
```
-   So, what’s the difference here?\
    Why can __`String`__ be mutated but literals cannot?\
    The difference is in how these two types deal with memory.

\

3.  In the case of a string literal,\
    we know the contents at compile time, so the text is hardcoded directly into the final executable.\
    This is why string literals are fast and efficient.\
    But these properties only come from the string literal’s immutability.

    With the __`String`__ type,\
    in order to support a mutable, growable piece of text,
    we need to allocate an amount of memory on the heap,\
    unknown at compile time, to hold the contents.

    This means:
    - The memory must be requested from the allocator at runtime.
    - We need a way of returning this memory to the allocator when we're done with it.

    That first part is done by us:
    - When we call __`String::from( )`__, its implementation requests the memory it needs.
    - This is pretty much universal in programming languages.

    However, the second part is different:
    - In languages with a garbage collector (GC),\
      the GC keeps track of and cleans up memory that isn’t being used anymore, and we don’t need to think about it.
    - In most languages without a GC,
      it’s our responsibility to identify when memory is no longer being used and to call code to explicitly free it, just as we did to request it.
    - Doing this correctly has historically been a difficult programming problem.
    - If we forget, we’ll waste memory. If we do it too early, we’ll have an invalid variable.
    - If we do it twice, that’s a bug too. We need to pair exactly one allocate with exactly one free.

    Rust thus takes a different path:
    - Memory is automatically returned once the variable that owns it goes out of scope.
    - When a variable goes out of scope, Rust calls a special function for us.
    - This function is called __`drop()`__, and it’s where the author of __`String`__ can put the code to return the memory.
    - Rust calls __`drop()`__ automatically at the closing curly bracket.

\

4.  NOTE in C++,
    this pattern of deallocating resources at the end of an item’s lifetime is sometimes called Resource Acquisition Is Initialization (`RAII`).\
    The __`drop()`__ function in Rust will be familiar to you if you’ve used `RAII` patterns.

\

5.  To ensure memory safety, for types allocated on the heap, after the line `let s2 = s1;`, Rust considers ___`s1`___ as no longer valid.\
    Therefore, Rust doesn’t need to free anything when ___`s1`___ goes out of scope.
```rust
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{s1}");
            //^^^^ value borrowed here after move
```
-   If you wanna use both ___`s1`___ and ___`s2`___, do this:
```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("{s1}");
    println!("{s2}");
```
\

6.  Stack-Only data's copy is much simpler.
```rust
    let x = 10;
    let y = x;

    println!("{x}, {y}");
```
-   The reason is that types such as integers that have a `known fixed size` at compile time are stored entirely on the stack,\
    so copies of the actual values are quick to make.
-   That means there’s no reason we would want to prevent ___`x`___ from being valid after we create the variable ___`y`___.
-   In other words, there’s no difference between deep and shallow copying here,\
    so calling __`clone()`__ would `NOT` do anything different from the usual shallow copying, and we can leave it out.

\

7.  Rust has a special annotation called the __`Copy`__ trait,\
    we can place on types that are stored on the stack, as integers are.\
    If a type implements the __`Copy`__ trait, variables that use it do not move, but rather are trivially copied,\
    making them still valid after assignment to another variable.\
    And they do NOT call __`drop()`__ when come out of scope.

\

8.  Rust will NOT let us annotate a type with __`Copy`__ if the type, or any of its parts, has implemented the __`Drop`__ trait.\
    If we add the __`Copy`__ annotation to that type, we’ll get a compile-time error.

\

9.  Here are some of the types that implicitly implement __`Copy`__:

    a) All integer types, such as `u32`.
    b) The Boolean type `bool`.
    c) All floating types, sucha s `f32`.
    d) The Character type `char`.
    e) Tuples if they only contain types also implement `Copy` trait.\
        e.g. `(i32, i32)` YES, but `(i32, String)` NO.


<hr/>
### Ownership and Functions

1.  The mechanics of passing a value to a function are similar to those when assigning a value to a variable.\
    Passing a variable to a function will move or copy, just as assignment does.
```rust
    fn main() {
        let s = String::from("hello");  // `s` comes into scope.

        takes_ownership(s);             // `s` value moves into the function,
                                        // ... and so is no longer valid here.

        let x = 5;                      // `x` comes into scope.

        makes_copy(x);                  // `x` would move into the function, but  i32  is a Copy.
                                        // ... so it's still valid to use it after here.

    }                                   // Here, `x` goes out of scope, then `s`.
                                        // But because the value of `s` was moved, nothing special happens.


    fn takes_ownership(some_string: String) {   // `some_string` comes into scope.
        println!("{some_string}");
    }                                           // `some_string` comes out of scope and  drop()  is called,
                                                // ... the backing memory is freed.


    fn makes_copy(some_integer: i32) {          // `some_integer` comes into scope.
        println!("{some_integer}");
    }                                           // `some_integer` goes out of scope, nothing special happens.
```
\

2.  Returning values can also transfer ownership.
```rust
    fn main() {
        let s1 = gives_ownership();                     // `gives_ownership()` moves its return value into `s1`.

        let s2 = String::from("hello");                 // `s2` comes into scope.

        let s3 = takes_and_gives_ownership_back(s2);    // `s2` is moved into `takes_and_gives_ownership_back()`,
                                                        // which later moves its return value into `s3`.

    }                                                   // Here, `s3` goes out of scope and is dropped.
                                                        // `s2` was moved, so nothing happens.
                                                        // `s1 goes out of scope and is dropped.`

    fn gives_ownership() -> String {
        let some_string = String::from("yours");        // `some_string` comes into scope.

        some_string                                     // `some_string` is returned and moves out to the calling function.
    }

    fn takes_and_gives_ownership_back(a_string: String) -> String { // `a_string` comes into scope.
        a_string                                                    // `a_string` is returned and moves out to the calling function.
    }
```
\

3.  When a variable that includes data on the heap goes out of scope,\
    the value will be cleaned up by __`drop()`__ unless ownership of the data has been moved to another variable.

\

4.  Inside a __`tuple`__ there could be both move and copy:
```rust
    fn main() {
        let s1 = String::from("hello");

        let (s2, len) = calc_len(s1);
    }

    fn calc_len(s: String) -> (String, usize) {
        let len = s.len();

        (s, len) // Here, `s` will be moved, but `len` will be copied.
    }
```

<hr/>
### References and Borrowing

1.  What if we want to let a function use a value but `NOT` take ownership?\
    Luckily, Rust has a feature for using a value without transferring ownership, called __`reference`__.

\

2.  A __`reference`__ is like a pointer in that it’s an address we can follow to access the data stored at that address,\
    that data is owned by some other variable.

    However, unlike a pointer,\
    a __`reference`__ is guaranteed to point to a valid value of a particular type for the life of that reference.
```rust
    fn main() {
        let s1 = String::from("hello");

        let len = calc_len(&s1);

        println!("The length of {s1} is {len}.");
    }

    fn calc_len(s: &String) -> usize {
        s.len()
    }
```
-   Those ampersands represent references, and they allow you to refer to some value without taking ownership of it.

\

3.  The opposite of referencing by using __`&`__ is __`de-referencing`__,\
    which is accomplished with the dereference operator (__`*`__).\
    We'll see some uses of the dereference operator in later chapters!

\

4.  When functions have references as parameters instead of the actual values,\
    we do `NOT` need to return the values in order to give back ownership, because we never had ownership.

    We call the action of creating a reference __`borrowing`__.\
    As in real life, if a person owns something, you can borrow it from them.\
    When you’re done, you have to give it back. You don’t own it.

\

5.  So, what happens if we try to modify something we’re borrowing?\
    Spoiler alert: it does NOT work!
```rust
    fn main() {
        let mut s = String::from("Hello");

        change(&s);
    }

    fn change(some_string: &String) {
        some_string.push_str(", world!");
    //  ^^^^^^^^^^^ `some_string` is a `&` reference,
    //              so the data it refers to cannot be borrowed as mutable.
    }
```
-   Just as variables are __`immutable`__ by default, so are references.\
    We are NOT allowed to modify something we have only reference to.

\

6.  We can fix the code allowing us to modify a borrowed value with just a __`mutable reference`__:
```rust
    fn main() {
        let mut s = String::from("Hello");

        change(&mut s);
    }

    fn change(something: &mut String) {
        something.push_str(", world");
    }
```
\

7.  Mutable references have one big restriction,\
    if you have a mutable reference to a value, you must have NO other references to that value at a time.\
    This code that attempts to create two mutable references to ___`s`___ will fail:
```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    //      -------- first mutable borrow occurs here.
    let r2 = &mut s;
    //      ^^^^^^^^ second mutable borrow occurs here.

    println!("{r2}");
    // This is ok.
    // If you borrowed it multiple times, you can always ONLY use the LAST BORROW.

    println!("{r1}");
    //         -- first borrow later used here.
```
\

8.  As always, we can use curly brackets to create a new scope, allowing for multiple mutable references,\
    just not simultaneous:
```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    }

    let r2 = &mut s;
```
\

9.  We also can NOT have a mutable reference while we have an immutable one to the same value.
```rust
    let mut s = String::from("hello");

    let r1 = &s;        // OK
    //       -- immutable borrow occurs here.
    let r2 = &s;        // OK
    let r3 = &mut s;    // ERR!
    //      ^^^^^^^ mutable borrow occurs here.

    println!("{} {} {}", r1, r2, r3);
    //                   -- immutable borrow later used here.
```
\

10. Note that a reference's scope starts from where it is introduced, 
    and continues through the last time that reference is `used`.\
    For instance, this code will compile because the last usage of the immutable references, the __`println!`__,\
    occurs before the mutable reference is introduced:
```rust
    let mut s = String::from("hello");

    let r1 = &s;
    let r2 = &s;
    println!("{r1} {r2}");

    let r3 = &mut s;
    println!("{r3}");
```
\

11. Example:
```rust
    fn main() {
        let mut my_string = String::from("a");
        first_fn(&mut my_string);
        println!("{my_string}");
    }


    fn fist_fn(s: &mut String) {
        s.push_str("a");
        println!("{s}");

        second_fn(s);

        println!("{s}");
    }


    fn second_fn(s: &mut String) {
        s.push_str("b");
        println!("{s}");
    }

    /* Output
        -> aa
        -> aab
        -> aab
        -> aab
    */
```


<hr/>
### Dangling References

1.  In languages with pointers, it's easy to erroneously create a `dangling pointer`,\
    a pointer that references a location in memory that may have been given to someone else
    by freeing some memory while preserving a pointer to that memory.

\

2.  In Rust, by contrast, the compiler guarantees that references will NEVER be dangling references,\
    if you have a reference to some data,
    the compiler will ensure that the data will not go out of scope before the reference to the data does.
```rust
    fn main() {
        let ref_to_nothing = dangling();
    }

    fn dangling() -> &String {
    //               ^ expected named lifetime parameter.
        let s = String::from("hello");

        &s
    //  ^^ returns a reference to data owned by the current function.
    }
```
-   This error message refers to a feature we haven’t covered yet: `lifetimes`.
    We’ll discuss `lifetimes` in later chapters!



<hr/>
### The Rules of References Summary

1.  At any given time,\
    you can have either ONE mutable reference or any number of immutabe references.

\

2.  References must always be valid.\
    Which means the data will NEVER go out of scope before the reference to the data does.


<hr/>
### The Slice Type

1.  __`Slice`__ lets you reference a contiguous sequence of elements in a collection, rather than the whole collection.\
    A slice is a kind of reference, so it ___does not___ have ownership.


<hr/>
### String Slices

1.  A string slice is a reference to part of a __`String`__, and it looks like this:
```rust
    let s = String::from("Hello, world");

    let hello = &s[0..5];
    let world = &s[7..12];

    let entire_str = &s[..];
    let entire_str2 = &s[0..s.len()];
```
-   With Rust's __`..`__ range syntax, if you want to start at index `0`, you can simply write `let hello = &s[..5];`.\
    By the same token, if your slice incluses the last byte of `String`, you write `let world = &s[7..];`.

\

2.  Example 1 : To get the first word in a string, or the entire string.
```rust
    fn main() {
        let mut s = String::from("hello world");

        let word = first_word(&s);

        println!("{word}");

        s.clear();  // Empties the String, making it equal to "",
                    // thus the slice {word} will also no longer no valid!
                    // But NOT because it clears the String, but because it has used a mutable borrow inside it,
                    // so an immutable borrow like {word} could no longer be used.
                    // Immutable borrows can NOT be used after a mutable borrow!
    }

    fn first_word(s: &String) -> &str {
        let bytes = s.as_bytes();

        for (i, &item) in bytes.iter().enumerate() {
            if item == b' ' {
                return &s[0..i];
            }
        }

        &s[..]
    }
```

<hr/>
### String Literals as Slices

1.  Recall that we talked about string literals being stored inside the binary.\
    Now that we know about slices, we can properly understand string literals:
```rust
    let s = "Hello, world!";
```
-   The type of ___`s`___ here is __`&str`__, it is a slice pointing to that specific point of the binary.\
    This also explains why string literals are immutable. __`&str`__ is an immutable reference.


<hr/>
### String Slices as Parameters

1.  Knowing that you can take slices of literals,\
    and __`String`__ values leads us to one more improvement on __`first_word()`__, and that’s its signature:
```rust
    fn first_word(s: &String) -> &str
```
-   A more experienced Rust user would write the signature shown below instead,\
    because it allows us to use the same function on both __`&String`__ values and __`&str`__ values.
```rust
    fn first_word(s: &str) -> &str
```
-   If we have a string slice, we can pass that directly.\
    If we have a __`String`__, we can pass a slice of the __`String`__ or a reference to the __`String`__.\
    This flexibility takes advantage of ___deref coercions___, a feature we will cover in later chapters!
```rust
    fn main() {
        let my_string = String::from("hello world");

        let word = first_word(&my_string[0..7]);
        let word = first_word(&my_string[..]);
        let word = first_word(&my_string);

        let my_string_literal = "hello world";

        let world = first_word(&my_string_literal[0..7]);
        let world = first_word(&my_string_literal[..]);
        let world = first_word(my_string_literal);
    }
```

<hr/>
### Other Slices

1.  Just as we might want to refer to part of a string, we might also want to refer to part of an array.
```rust
    let arr = [1, 2, 3, 4, 5];

    let slice = &arr[1..3];     // : &[i32]

    assert_eq!(slice, &[2, 3]);
```


<hr/>
### Defining and Instantiating Structs

1.  Like tuples, the pieces of a struct can be different types.\
    Unlike with tuples, in a struct you’ll name each piece of data so it’s clear what the values mean.

\

2.  To define a struct, we enter the keyword __`struct`__ to name the entire structure.\
    Then, inside curly brackets, we define the names and types of the pieces of data, which we call ___fields___.
```rust
    struct User {
        active: bool,
        username: String,
        email: String,
        sign_in_counts: u64,
    }
```
\

3.  To use a __`struct`__ after we’ve defined it,\
    we create an ___instance___ of that it by specifying concrete values for each of the fields.

    We create an instance by stating the name of the struct and then add curly brackets containing _key / value pairs_,\
    where the keys are the names of the fields and the values are the data we want to store in those fields.

    We don’t have to specify the fields in the same order in which we declared them in the __`struct`__.
```rust
    fn main() {
        let mut user1 = User {
            active: true,
            username: String::from("John"),
            email: String::from("someone@random.com"),
            sign_in_counts: 0,
        };

        // To access a specific value from a struct, we use dot notation.
        user1.email = String::from("another@example.com");

        let user2 = build_user(String::from("hello@hi.com"), String::from("Pete"));
    }


    fn build_user(email: String, username: String) -> User {
        User {
            active: true,
            username: username,
            email: email,
            sign_in_counts: 0,
        }
    }
```
\

4.  NOTE that the ___entire___ ___instance___ of __`struct`__ must be __`mut`__ if you want to change any field.\
    Rust does `NOT` allow you to mark only certain fields as mutable.


<hr/>
### Use the Field Init Shorthand

1.  If the property names and the __`struct`__ field names are exactly the same,\
    we can use the ___field init shorthand___ syntax:
```rust
    fn build_user(email: String, username: String) -> User {
        User {
            active: true,
            username,
            email,
            sign_in_counts: 0,
        }
    }
```

<hr/>
### Creating Instances from Other Instances with Struct Update Syntax

1.  It’s often useful to create a new instance that includes most of the values from another, but changes some.\
    You can do this using ___struct update___ syntax.
```rust
    fn main() {
        let user1 = build_user(String::from("hello@hi.com"), String::from("Pete"));

        // Here I initialize {user2.username} and {user2.email}
        // because they are `String`, so they do not implement the `Copy` trait.
        // If not updating this way the ownership will be transferred.
        let user2 = User {
            username: String::from("John"),
            email: String::from("hejo@hi,com"),
            ..user1
        };
    }
```
-   The syntax __`..`__ specifies that the remaining fields not explicitly set should have the same value\
    as the fields in the given instance.

\

2.  However, you can NOT use ___struct update___ syntax to update an instance of a different type.



<hr/>
### Use Tuple Structs without Named Fields to Create Different Types

1.  Rust also supports structs that look similar to tuples, called ___tuple structs___.\
    They just have the types of the fields.
```rust
        struct Color(i32, i32, i32);
        struct Point(i32, i32, i32);

        fn main() {
            let black = Color(0, 0, 0);
            let origin = Point(0, 0, 0);
        }
```
-   Function that takes a parameter of type `Color` cannot take a `Point` as an argument,\
    even though both types are made up of three `i32` values.
-   Otherwise, ___tuple struct___ instances are similar to tuples in that you can destructure them into their individual pieces,\
    and you can use a dot (`.`) followed by the `0`-based index to access an individual value.



<hr/>
### Ownership of Struct Data

1.  In the `User` struct definition below,\
    we used the owned __`String`__ type rather than the __`&str`__ string slice type.\
    This is a deliberate choice because we want each instance of it to OWN all of its data, 
    and for that data to be valid for as long as the entire struct is valid.

\

2.  It’s also possible for structs to store references to data owned by something else,\
    but to do so requires the use of ___lifetimes___, a Rust feature that we’ll discuss in later chapters!\
    Lifetimes ensure that the data referenced by a struct is valid for as long as the struct is valid.

    Let’s say you try to store a reference in a struct without specifying lifetimes,\
    like the following, this won’t work:
```rust
    struct User {
        active: bool,
        username: &str,
    //            ^ expected named lifetime parameter.
        email: &str,
    //         ^ expected named lifetime parameter.
        sign_in_counts: u64,
    }

    fn main() {
        let user1 = User {
            active: true,
            username: "John",
            email: "john@example.com",
            sign_in_counts: 0,
        };
    }
```
\

3.  An example of using struct:
```rust
    struct Rectangle {
        width: u32,
        height: u32,
    }

    fn main() {
        let rect = Rectangle {
            width: 10,
            height: 20,
        };

        println!("The are of the rectangle is {}", area(&rect));
    }

    fn area(rectangle: &Rectangle) -> u32 {
        rectangle.width * rectangle.height
    }
```

<hr/>
### Adding Useful Functionality with Derived Traits

1.  The __`println!`__ macro can do many kinds of formatting, 
    and by default, the curly brackets tell __`println!`__ to use formatting known as __`Display`__,\
    an output intended for direct end user consumption.

\

2.  The primitive types we've seen so far all implement __`Display`__ by default,\
    because there’s only one way you’d want to show an integer or any other primitive type to a user.\
    But with structs, the way __`println!`__ should format the output is less clear.

\

3.  And the __`{:?}`__ or __`{:#?}`__ (for pretty-print) require to implement the __`Debug`__ trait.

\

4.  Rust does include functionality to print out debugging information,\
    but we have to explicitly opt in to make that functionality available for our struct.

    To do that, we add the outer attribute __`#[derive(Debug)]`__ just before the __struct__ definition:
```rust
    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    fn main() {
        let rect = Rectangle {
            width: 10,
            height: 20,
        };

        println!("{rect:?}");
    }
```
\

5.  Another way to print out a value using the __`Debug`__ format is to use the __`dbg!`__ macro,\
    which takes ownership of an expression (as opposed to __`println!`__, which takes a reference),\
    prints the file and line number of where that __`dbg!`__ macro call occurs in your code
    along with the resultant value of that expression, and returns ownership of the value.

    And calling the __`dbg!`__ macro prints to the standard error console stream (__`stderr`__),\
    as opposed to __`println!`__, which prints to the standard output console stream (__`stdout`__).\
```rust
    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    fn main() {
        let scale = 2;

        let rect = Rectangle {
            // Because `dbg!` returns ownership of the expression’s value,
            // the width field will get the same value as if we didn’t have the `dbg!`.
            width: dbg!(10 * scale),
            height: 20,
        };

        // We don’t want `dbg!` to take ownership of {rect},
        // so we use a reference to {rect} in the next call.
        dbg!(&rect);
    }
```

<hr/>
### Method Syntax

1.  Unlike functions, methods are defined within the context of a __`struct`__ (or an __`enum`__ or a __`trait`__ object).
```rust
    #[derive(Debug)]
    struct Rectangle {
        width:  u32,
        height: u32,
    }

    // NOTE: here {self} is of type `Self`.
    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }

        fn set_width(&mut self, width: u32) {
            self.width = width;
        }

        fn get_width(&self) -> u32 {
            self.width
        }

        fn set_height(self: &mut Self, height: u32) {
            self.height = height;
        }

        fn get_height(self: &Self) -> u32 {
            self.height
        }

        fn can_hold(&self, other: &Rectangle) -> bool {
            (self.width > other.width)
            &&
            (self.height > other.height)
        }
    }

    fn main() {
        let rect = Rectangle {
            width:  20,
            height: 30,
        };

        println!("{}", rect.area());
    }
```
\

2.  Rust does NOT have an equivalence to the __`->`__ operator as in C / C++.\
    Instead, Rust has a feature called ___automatic referencing___ and ___automatic dereferencing___.\

    Here's how it works:
    - when you call a method with `object.something(`),\
      Rust automatically adds in __`&`__, __`&mut`__, or __`*`__ so object matches the signature of the method.

    In other words, the following are the same:
```rust
    p1.distance(&p2);
    (&p1).distance(&p2);
```
\

4.  All functions defined within an __`impl`__ block are called ___associated functions___.\
    We can define associated functions that don’t have __`self`__ as their first parameter (and thus are not methods).
```rust
    impl Rectangle {
        fn square(size: u32) -> Self {
            Self {
                width:  size,
                height: size,
            }
        }
    }

    fn main() {
        let sq = Rectangle::square(10);
    }
```
\

5.  Each __`struct`__ is allowed to have multiple __`impl`__ blocks.
```rust
    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }
    }

    impl Rectangle {
        fn square(size: u32) -> Self {
            Self {
                width:  size,
                height: size,
            }
        }
    }
```


<hr/>
### Defining an Enum

1.  __`enum`__ gives you a way of saying a value is one of a possible set of values.
```rust
    enum IpAddrKind {
        v4,
        v6
    }

    struct IpAddr {
        kind: IpAddrKind,
        address: String,
    }

    fn main() {
        let home = IpAddr {
            kind: IpAddrKind::v4,
            address: String::from("127.0.0.1"),
        };

        let loopback = IpAddr {
            kind: IpAddrKind::v6,
            address: String::from("::1");
        };

        let ipv6 = IpAddrKind::v6;
        route(IpAddrKind::v4);
        route(ipv6);
    }

    fn route(ip: IpAddrKind) {
    }
```
\

2.  Rather than an __`enum`__ inside a __`struct`__, we can put data directly into each __`enum`__ variant.
```rust
    enum IpAddr {
        V4(u8, u8, u8, u8),
        V6(String),
    }

    let home = IpAddr::V4(127, 0, 0, 1);
    let loopback = IpAddr::V6(String::from("::1"));
```
\

3.  Let's look at another example of an __`enum`__ has a wide variety of types embedded in its variants.
```rust
    enum Message {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }

    impl Message {
        fn call(&self) {
        }
    }

    fn main() {
        let msg = Message::Write(String::from("hello"));
        msg.call();
    }
```


<hr/>
### The Option Enum and Its Advantages Over Null values

1.  The __`Option`__ enum encodes the very common scenario,\
    in which a value could be something or it could be nothing.

\

2.  Rust does not have nulls,\
    but it does have an enum that can encode the concept of a value being present or absent.\
    This enum is __`Option<T>`__, and it is defined by the standard library as follows:
```rust
        enum Option<T> {
            None,
            Some(T),
        }
```
\

3.  The __`Option<T>`__ enum is so useful that it’s even included in the ___prelude___,\
    you don’t even need to bring it into scope explicitly.

    Its variants are also included in the ___prelude___,\
    you can use __`Some(T)`__ and __`None`__ directly without the __`Option::`__ prefix.

    The __`<T>`__ is a generic type parameter, and we’ll cover generics in more detail in later chapters!
```rust
        let some_number = Some(5);
        let some_char   = Some('e');

        let absent_number: Option<i32> = None;
```
\

4.  Because __`Option<T>`__ and __`T`__ (where `T` can be any type) are different types,\
    the compiler won’t let us use an __`Option<T>`__ value as if it were definitely a valid value.\
    For example, this code won’t compile, because it’s trying to add an __`i8`__ to an __`Option<i8>`__:
```rust
        let x: i8 = 5;
        let y: Option<i8> = Some(5);

        let sum = x + y;
        //          ^ no implementation for `i8 + Option<i8>`
```


<hr/>
### The `match` Control Flow Construct

1.  Rust has an extremely powerful control flow construct called __`match`__,\
    that allows you to compare a value
    against a series of patterns and then execute code based on which pattern matches.
```rust
        enum Coin {
            Penny,
            Nickel,
            Dime,
            Quarter,
        }

        fn value_in_cents(coin: Coin) -> u8 {
            match coin {
                Coin::Penny     => {
                    println!("A lucky penny!");
                    1
                },
                Coin::Nicke     => 5,
                Coin::Dime      => 10,
                Coin::Quarter   => 25,
            }
        }
```
\

2.  Another useful feature of __`match`__ arms is that they can bind to the parts of the values that match the pattern.
```rust
        #[derive(Debug)]
        enum UsState {
            Alabama,
            Alaska,
            // --snip--
        }

        enum Coin {
            Penny,
            Nickel,
            Dime,
            Quarter(UsState),
        }

        fn value_in_cents(coin: Coin) -> u8 {
            match coin {
                Coin::Penny => 1,
                Coin::Nicke => 5,
                Coin::Dime  => 10,
                Coin::Quarter(state)  => {
                    println!("A quarter from {state:?}");
                    25
                },
            }
        }
```
\

3.  We can also handle __`Option<T>`__ using __`match`__, as we did with the `Coin` enum!
```rust
        fn plus_one(x : Option<i32>) -> Option<i32> {
            match x {
                None => None,
                Some(i) => Some(i + 1),
            }
        }

        fn main() {
            let five = Some(5);

            let six = plus_one(five);

            let nothing = plus_one(None);
        }
```
\

4.  There’s one other aspect of we need to discuss - __`match`__ is exhaustive.\
    Which means the arms’ patterns must cover all possibilities.\
    Underscore ( __`_`__ ) is a special pattern that matches any value and does not bind to that value.
```rust
    fn main() {
        let dice_roll = 5;

        match dice_roll {
            1 => add_fancy_hat(),
            6 => remove_fancy_hat(),
            other => move_player(other),
        }

        match dice_roll {
            1 => add_fancy_hat(),
            6 => remove_fancy_hat(),
            _ => reroll(),
        }
    }


    fn reroll() {}
    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn move_player(num_spaces: u8) {}
```


<hr/>
### Concise Control Flow with if let

1.  The __`if let`__ syntax lets you into a less verbose way to handle match while ignoring thr rest.
```rust

        let config_max = Some(3u8);
        match config_max {
            Some(max) => println!("The maximum is configured to be {max}"),
            _ => (),
        }



        let config_max = Some(3u8);
        if let Some(max) = config_max {
            println!("The maximum is configured to be {max}");
        }

```
\

2.  We can include an __`else`__ with an __`if let`__.\
    The block of code that goes with the __`else`__ is the same as the block of code that would go with the `_` case in the __`match`__ expression.
```rust

        let mut count = 0;
        match coin {
            Coin::Quarter(state) => println!("A quarter from {state:?}"),
            _ => count += 1,
        }



        let mut count = 0;
        if let Coin::Quarter(state) = coin {
            println!("A quarter from {state:?}");
        } else {
            count += 1;
        }

```
\

3.  Naturally there is __`else if let`__ also!
```rust

        let mut count = 0;
        match coin {
            Coin::Quarter(state) => println!("A quarter from {state:?}"),
            Coin::Dime => println!("It's a dime!"),
            _ => count += 1,
        }



        let mut count = 0;
        if let Coin::Quarter(state) = &coin {
            println!("A quarter from {state:?}");
        } else if let Coin::Dime = &coin {
            println!("It's a dime!");
        } else {
            count += 1;
        }



```

<hr/>
### Project Management

1.  Rust has a number of features that allow you to manage your code’s organization,\
    including which details are exposed, which details are private, and what names are in each scope in your programs.

\

2.  These features, sometimes collectively referred to as the ___module system___, include:

    - __`package`__
        - A Cargo feature that lets you build, test, and share crates.
    - __`crate`__
        - A tree of modules that produces a library or executable.
    - __`module`__ and __`use`__
        - Let you control the organization, scope, and privacy of paths.
    - __`path`__
        - A way of naming an item, such as a ___structure___, ___function___, or ___module___.


<hr/>
### Packages and Crates

1.  A __`crate`__ is the smallest amount of code that the Rust compiler considers at a time.\
    Even if you run __`rustc`__ rather than __`cargo`__ and pass a single source code file, the compiler considers that file to be a __`crate`__.

    Crates can contain modules, and the modules may be defined in other files that get compiled with the crate,\
    as we’ll see in the coming sections.

\

2.  A `crate` can come in one of two forms: a ___binary crate___ or a ___library crate___.

    - ___Binary crates___ 
        - Programs you can compile to an executable that you can run, such as a command-line program or a server.
        - Each must have a function called __`main()`__ that defines what happens when the executable runs.
        - All the crates we’ve created so far have been ___binary crates___.

    - ___Library crates___ 
        - Do `NOT` have a __`main()`__ function, and they don’t compile to an executable.
        - Instead, they define functionality intended to be shared with multiple projects.
        - For example, the _rand_ crate we used in Chapter 2 provides functionality that generates random numbers.\
        - Most of the time when Rust users say __`crate`__, they mean ___library crate___,\
          and they use __`crate`__ interchangeably with the general programming concept of a __`library`__.

\

3.  The ___crate root___ is a source file that the Rust compiler starts from,\
    and makes up the ___root___ `module` of your crate.

\

4.  A __`package`__ is a bundle of one or more _crates_ that provides a set of functionality.\
    A package contains a `Cargo.toml` file that describes how to build those crates.\
    `Cargo` is actually a `package` that contains the _binary crate_ for the command-line tool you’ve been using to build your code.
    The `Cargo` package also contains a _library crate_ that the _binary crate_ depends on.

    Other projects can depend on the `Cargo` _library crate_ to use the same logic the Cargo command-line tool uses.

\

5.  A `crate` can come in one of two forms: a _binary crate_ or a _library crate_.\
    A `package` can contain as many _binary crates_ as you like, but at most ONLY ONE _library crate_.\
    A `package` must contain at least ONE crate, whether that’s a _library_ or _binary_ crate.


<hr/>
### A Walkthrough of What Happens When We Create a Package

1.  First we enter the command `cargo new my-project`.
```zsh
    $ cargo new my-project
        Created binary (application) `my-project` package

    $ ls my-project
    Cargo.toml
    src

    $ ls my-project/src
    main.rs
```
\

2.  In the project directory, there’s a `Cargo.toml` file, giving us a `package`.\
    There’s also a `src/` directory that contains `main.rs`.\
    Open `Cargo.toml` in your text editor, and note there’s no mention of `src/main.rs`.\
    Cargo follows a convention that `src/main.rs` is the _crate root_ of a _binary crate_ with the same name as the package.\
    Likewise, Cargo knows that if the package directory contains `src/lib.rs`,\
    the package contains a _library crate_ with the same name as the package, and `src/lib.rs` is its _crate root_.\
    Cargo passes the _crate root_ files to `rustc` to build the _library_ or _binary_.

\

3.  Here, we have a package that only contains `src/main.rs`, meaning it only contains a _binary crate_ named `my-project`.\
    If a package contains `src/main.rs` and `src/lib.rs`, it has two crates, a _binary_ and a _library_, both with the same name as the package.\
    A package can have _multiple_ binary crates by placing files in the `src/bin/` directory, each file will be a separate binary crate.\



<hr/>
### Defining Modules to Control Scope and Privacy

1.  When compiling a crate,\
    the compiler first looks in the crate root file (usually __`src/lib.rs`__ for a ___library crate___ or __`src/main.rs`__ for a ___binary crate___).

\

2.  The ___crate root___ in Rust is the source file that serves as the entry point for a Rust __`crate`__,\
    and it forms the root of the module tree for that crate.

\

3.  In the ___crate root___ file, you can declare new ___modules___.\
    Let's say you declare a _`garden`_ module with `mod garden;`.\
    The compiler will look for the module’s code in these places:
    - Inline, within curly brackets that replace the semicolon (`;`) following __`mod garden`__.
    - In the file __`src/garden.rs`__
    - In the file __`src/garden/mod.rs`__

\

4.  In any file other than the ___crate root___, you can declare ___submodules___.\
    For example, you might declare `mod vegetables;` in `src/garden.rs`.\
    The compiler will look for the submodule’s code within the directory named for the parent module in these places:
    - Inline, directly following `mod vegetables`, within curly brackets instead of the semicolon (`;`).
    - In the file __`src/garden/vegetables.rs`__
    - In the file __`src/garden/vegetables/mod.rs`__

\

5.  Once a module is part of your crate, you can refer to code in that module from anywhere else in that same crate,\
    as long as the privacy rules allow, using the path to the code.\
    For example, an _`Asparagus`_ type in the _`garden vegetables`_ module would be found at _`crate::garden::vegetables::Asparagus`_.

\

6.  Code within a module is ___private___ by default,\
    meaning it can NOT be accessed from outside that module, inclduing parent module.\
    But child modules can access its parent's any item even private items freely.\
    But if a parent wants to access child's item, only the item needs to be ___public___, the child module could remain ___private___.\
    Sibling modules could access each other's items as long as the items are __`pub`__, no need for the modules to be ___public___.\
    To make a module ___public___, declare it with __`pub mod`__ instead of __`mod`__.

    Items inside a __`pub mod`__ is still private,\
    to make items within a public module public as well, use __`pub`__ before their declarations.

    To access a module’s elements (functions, structs, constants, etc.) from outside,\
    both the module itself and the items inside it must be made public

    If you have a two-level module hierarchy, and you want to access the child module and its items from outside,\
    both the parent module and the child module must be public.

\

7.  Within a scope, the __`use`__ keyword creates shortcuts to items to reduce repetition of long paths.\
    In any scope that can refer to _`crate::garden::vegetables::Asparagus`_,\
    you can create a shortcut with _`use crate::garden::vegetables::Asparagus;`_\
    and from then on you only need to write _`Asparagus`_ to make use of that type in the scope.

\

8.  Consider following example:

- __File Structure__:
```dir
        backyard/
        |---------- Cargo.lock
        |---------- Cargo.toml
        |---------- src/
                    |---------- garden/
                                    |---------- vegetables.rs
                    |---------- garden.rs
                    |---------- main.rs
```
\

- __`src/main.rs`__
```rust
        use crate::garden::vegetables::Asparagus;

        // To tell the compiler to include the code it finds in the `src/garden.rs`.
        pub mod garden;

        fn main() {
            let plant = Asparagus {};
            println!("{plant:?}");
        }
```
- __`src/garden.rs`__
```rust
        // To tell the compiler to include the code in `src/garden/vegetables.rs` too.
        pub mod vegetables;
```
- __`src/garden/vegetables.rs`__
```rust
        #[derive(Debug)]
        pub struct Asparagus {}
```


<hr/>
### Grouping Related Code in Modules

1.  Create a new library named `restaurant` by running __`cargo new restaurant --lib`__.\
    Then enter the code in __`src/lib.rs`__ to define some modules and function signatures.

- __`src/lib.rs`__
```rust
        mod front_of_house {

            mod hosting {
                fn add_to_waitlist() {}

                fn seat_at_table() {}
            }


            mod serving {
                fn take_order() {}

                fn serve_order() {}

                fn handle_payment() {}
            }

        }
```
\

2.  We define a module with the __`mod`__ keyword followed by the name of the module.\
    The body of the module then goes inside curly brackets.\
    Inside modules, we can place other modules.\
    Modules can also hold definitions for other items, such as structs, enums, constants, traits and function etc.

\

3.  Earlier, we mentioned that __`src/main.rs`__ and __`src/lib.rs`__ are called ___crate roots___.\
    The reason is that the contents of either of these two files form a module named __`crate`__
    at the root of the crate’s module structure, known as the ___module tree___.

\

4.  Module tree for the `restaurant`:
```dir
    crate
    |---------- front_of_house
                        |---------- hosting
                                    |---------- add_to_waitlist
                                    |---------- seat_at_table
                        |---------- serving
                                    |---------- take_order
                                    |---------- serve_order
                                    |---------- handle_payment
```


<hr/>
### Paths for Referring to an Item in the Module Tree

1.  To call a function, we need to know its path.

\

2.  A path can take two forms:

    An __`absolute path`__ is the full path starting from a ___crate root___.\
    For code from an external crate, the absolute path begins with the ___crate name___,\
    and for code from the current crate, it starts with the literal __`crate`__.

    A __`relative path`__ starts from the ___current module___ and uses __`self`__, __`super`__, or an _identifier_ in the current module.\
    Both _absolute_ and _relative_ paths are followed by one or more identifiers separated by double colons (__`::`__).

\

3.  All the source code in your __`src/`__ directory of a project is part of the ___same crate___.

\

4.  Top-level modules inside a crate (those declared in __`main.rs`__ or __`lib.rs`__ via __`mod`__) are ___private___ by default,\
    but they are accessible to other modules ___within the same crate___.\
    This means that sibling modules (like __`main.rs`__ and another top-level module declared via __`mod`__)\
    can access each other without needing to be marked __`pub`__, but their items to be accessed needs to be __`pub`__.\
    This visibility extends throughout the crate internally.

- ___src/lib.rs___
```rust
        mod front_of_house {
            pub mod hosting {
                pub fn add_to_waitlist() {}
            }

            fn start_hosting() {
                self::hosting::add_to_waitlist();
            }
        }

        pub fn eat_at_restaurant() {
            // Absolute path:
            crate::front_of_house::hosting::add_to_waitlist();

            // Relative path:
            front_of_house::hosting::add_to_waitlist();
        }



        mod back_of_house {
            fn fix_incorrect_order() {
                self::prepare_ingradients();
                cook_order();
                super::deliver_order();
            }

            fn prepare_ingradients() {}

            fn cook_order() {}
        }

        fn deliver_order() {}
```
\

5.  You can bring paths into scope with the __`use`__ keyword.

- ___src/lib.rs___
```rust
        mod front_of_house {
            pub mod hosting {
                pub fn add_to_waitlist() {}
            }

            use crate::customer::eat_at_restaurant;
            fn eat() {
                eat_at_fancy_restaurant();
            }
        }


        use crate::front_of_house::hosting;
        pub fn eat_at_restaurant() {
            hosting::add_to_waitlist();
        }


        mod customer {
            use crate::front_of_house::hosting;
            pub fn eat_at_fancy_restaurant() {
                hosting::add_to_waitlist();
            }
        }
```
\

6.  You can also specify __`as`__ with a new local name, or alias, for the type.
```rust
        use std::io::Result;
        fn test() -> Result<()> {
        }



        use std::io::Result as IoResult;
        fn test() -> IoResult<()> {
        }
```
\

7.  When we bring a name into scope with the __`use`__ keyword, the name available in the new scope is ___private___.\
    To enable the code that calls our code to refer to that name as if it had been defined in that code’s scope,
    we can do __`pub use`__.\
    This technique is called ___re-exporting___,\
    because we’re bringing an item into scope but also making that item available for others to bring into their scope.
```rust
    {
        mod front_of_house {
            pub mod hosting {
                pub fn add_to_waitlist() {}
            }
        }

        // Brings `hosting` into the current module scope.
        use crate::front_of_house::hosting;

        pub fn eat_at_restaurant() {
            hosting::add_to_waitlist(); // Works here because of `use`.
        }
    }


    {
        mod front_of_house {
            pub mod hosting {
                pub fn add_to_waitlist() {}
            }

            // Re-exports `hosting` module.
            pub use crate::front_of_house::hosting;
        }

        pub fn eat_at_restaurant() {
            // Accessible because of `pub use`
            hosting::add_to_waitlist();
        }
    }
```
-   NOTE that the __`pub use`__ does not change the privacy of the item it re-exports.
-   If the item being re-exported (like _`mod hosting`_ here) was not public,\
    then even if you use __`pub use`__, it won’t make that item accessible outside the module.

\

8.  __`pub use`__ is used to re-export items from a module.\
    This means that the item being re-exported can be accessed from the outside,\
    as if it were defined in the module where __`pub use`__ is applied, but only if the item itself is already ___public___.
```rust
        mod front_of_house {
            pub mod hosting {
                pub fn add_to_waitlist() {}
            }

            pub use hosting::add_to_waitlist;
        }

        fn main() {
            front_of_house::add_to_waitlist();
        }
```
\

9.  If we’re using multiple items defined in the same crate or same module,\
    we can use nested paths to bring the same items into scope in one line.
```rust
    {
        use std::cmp::Ordering;
        use std::io;
    }
    {
        use std::{cmp::Ordering, io};
    }


    {
        use std::io;
        use std::io::Write;
    }
    {
        use std::io::{self, Write};
    }
```
\

10. If we want to bring `ALL` ___public___ items defined in a path into scope,\
    we can specify that path followed by the `*` glob operator:
```rust
    use std::collections::*;
```


<hr/>
### Storing Lists of Values with Vectors

1.  The first collection type we’ll look at is __`Vec<T>`__, also known as a ___vector___.\
    Vectors allow you to store one or more value(s) in a single data structure
    that puts all the values next to each other in memory.

\

2.  Vectors can only store values of the ___same type___.

\

3.  To create a new empty vector, we call the __`Vec::new()`__ function.
```rust
        let v: Vec<i32> = Vec::new();
```
-   `NOTE` we added a type annotation here, because we aren’t inserting any values into this vector.

\

4.  More often, you want to create a __`Vec<T>`__ with initial values
    and Rust will infer the type of value you want to store.\
    Rust conveniently provides the __`vec!`__ macro,\
    which will create a new vector that holds the values you give it.
```rust
        // : Vec<i32>
        let v = vec![1, 2, 3]; 
```
\

5.  To add elements to a vector, we can use the __`push()`__ method.\
    As with any variable, if we want to be able to change its value, we need to make it mutable using the __`mut`__ keyword.
```rust
        let mut v = Vec::new();

        v.push(10);
        v.push(20);
        v.push(30);
        v.push(40);
```
-   The numbers we place inside are all of type __`i32`__, and Rust infers this from the data,\
    so we do `NOT`need the __`Vec<i32>`__ annotation.

\

6.  There are __`2`__ ways to reference a value stored in a vector: 
    - via ___indexing___.
        - ___Indexing___ will cause the program to panic if out of range.
    - via the __`get()`__ method go get an __`Option<T>`__.
```rust
        let v = vec![1, 2, 3, 4, 5];


        let third: &i32 = &v[2];
        println!("The third element is {third}");


        let third: Option<&i32> = v.get(2);
        match third {
            Some(third) => println!("The third element is {third}"),
            None => println!("There is no third element!"),
        }
```
\

7.  But be care of the borrowing, the following code result in a ___compile-time error___:
```rust
        let mut v = vec![1, 2, 3, 4, 5];

        let first = &v[0];
        //          - immutable borrow occurs here.

        v.push(10);
        //^^^^^^^^ mutable borrow occurs here.

        println!("The first elem is {first}");
        //                          ------- immutale borrow later used here.
```
\

8.  To access each element in a vector in turn, 
    we would ___iterate___ through all of the elements.
```rust
        let mut v = vec![10, 20, 30];

        // About dereferencing operator we'll talk in later chapters!
        for i in &mut v {
            *i += 100;
        }

        // This creates temporary immutable references for each element of the vector
        for i in &v {
            println!("{i}");
        }
```
\

9.  If you do NOT use reference in iteration, 
    after the iteration, the vector is no longer valid.
```rust
        let v = vec![10, 20, 30];

        for i in v {
            println!("{}", i);
        }
        // After the loop, {v} is moved thus no longer valid,
        // and also all its elements were moved too.
```


<hr/>
### Storing UTF-8 Encoded Text with Strings

1.  The __`String`__ type,\
    which is provided by Rust’s standard library rather than coded into the core language,\
    is a growable, mutable, owned, UTF-8 encoded string type.

\

2.  Many of the same operations available with __`Vec<T>`__ are available with __`String`__ as well,\
    because __`String`__ is actually implemented as a wrapper around a vector of bytes, with some extra guarantees, restrictions, and capabilities.

    An example of a function that works the same way with __`Vec<T>`__ and __`String`__ is the __`new()`__ function to create an instance.
```rust
    let mut s = String::new();
```
-   This line creates a new, empty string called ___s___, into which we can then load data.

\

3.  Often, we’ll have some initial data with which we want to start the string.\
    For that, we use the __`to_string()`__ method,\
    which is available on any type that implements the __`Display`__ trait, such as string literals do.
```rust
    let data = "words";             // : &str

    let s = data.to_string();       // : String

    let s = "words".to_string();    // : String
```
\

4.  A __`String`__ can grow in size and its contents can change, just like the contents of a __`Vec<T>`__.\
    In addition, you can conveniently use the __`+`__ operator or the __`format!`__ macro to concatenate __`String`__ values.
```rust
    {
        let s1 = String::from("Hello");
        let s2 = String::from(", world!");

        // The `+` operator can concatenate `String` and `&str`.
        // fn add(self, &str) -> String { ... }
        // {s1} is moved, but {s2} remains accessible.
        // &String -> &str is a deref coercion, like &s2 == &s2[..]
        let s3 = s1 + &s2;
        println!("{s3}");       // Hello, world!
    }


    {
        let s1 = String::from("tic");
        let s2 = String::from("tac");
        let s3 = String::from("toe");

        let s = s1 + " - " + &s2 + " - " + &s3;
        println!("{s}");        // tic - tac - toe
    }


    {
        let s1 = String::from("Hello");
        let s2 = String::from(", world!");

        // {s1} and {s2} are not moved.
        let s3 = format!("{}{}", s1, s2);

        println!("{}", s1);
        println!("{}", s2);
        println!("{}", s3);
    }
```
\

5.  We also can grow a `String` by using the `push_str()` method to append a string slice.\
    And the `push()` method takes a single `char` as parameter.
```rust
    {
        let mut s = String::from("foo");
        s.push_str("bar");
    }
    {
        let mut s1 = String::from("foo");
        let s2 = "bar";
        s1.push_str(s2);

        // {s2} is still valid, because it is a string slice.
        println!("{s2}");
    }

    {
        let mut s = String::new();
        s.push('l');
        s.push('o');
        s.push('l');
        println!("{s}");    // -> lol
    }
    {
        let mut s = String::new();
        let ch = 'A';
        s.push(ch);         // {ch} is still valid, because it implements `Copy`
        println!("{s}");    // -> A
        println!("{ch}");   // -> A
    }
```
\

6.  Internally, a __`String`__ is a wrapper over a __`Vec<u8>`__.
```rust
        let hello = String::from("Hejo");
        let len = hello.len();
```
-   In this case, ___len___ will be __`4`__, which means the vector storing the string ___`"Hejo"`___ is __`4`__ bytes long.

\

7.  Here we might think the following len is __`12`__:
```rust
        let hello = String::from("Здравствуйте");
        let len = hello.len();
```
-   But the Rust's answer is __`24`__!
-   That's the number of bytes it takes to encode __`"Здравствуйте"`__ in `UTF-8`,\
    because each Unicode scalar value in that string takes __`2`__ bytes of storage.

\

8.  Therefore, an index into the string’s bytes will not always correlate to a valid Unicode scalar value.\
    To demonstrate, consider this invalid Rust code:
```rust
        let hello = "Здравствуйте";
        let answer = &hello[0];
```
-   You already know that answer will not be __`З`__, the first letter.\
-   When encoded in `UTF-8`, the first byte of __`З`__ is __`208`__ and the second is __`151`__,\
    so it would seem that answer should in fact be __`208`__, but __`208`__ is not a valid character on its own.
-   Returning __`208`__ is likely not what a user would want if they asked for the first letter of this string.\
    However, that’s the only data that Rust has at ___byte index 0___.\
    Users generally don’t want the byte value returned, even if the string contains only Latin letters.

\

9.  Rather than indexing using __`[]`__ with a single number,\
    you can use __`[]`__ with a range to create a string slice containing particular bytes:
```rust
        let hello = "Здравствуйте";
        let s = &hello[0..4];
```
-   Here, ___s___ will be a __`&str`__ that contains the first four bytes of the string.\
    Earlier, we mentioned that each of these characters was two bytes, which means s will be __`Зд`__.

\

10. Also, If we were to try to slice only part of a character’s bytes with something like __`&hello[0..1]`__,\
    Rust would ___panic at runtime___ in the same way as if an invalid index were accessed in a vector.

\

11. For individual Unicode scalar values, use the __`chars()`__ method.\
    Calling it on __`"Зд"`__ separates out and returns two values of type __`char`__, 
    and you can iterate over the result to access each element:
```rust
    {
        for ch in "Зд".chars() {
            println!("{ch}");
        }

        // -> З
        // -> д
    }
```
\

12. Alternatively, the __`bytes()`__ method returns each raw byte, 
    which might be appropriate for your domain:
```rust
    {
        for b in "Зд".bytes() {
            println!("{b}");
        }

        // -> 208
        // -> 151
        // -> 208
        // -> 180
    }
```


<hr/>
### Hash Maps

1.  The last of our common collections is the hash map.\
    The type __`HashMap<K, V>`__ stores a mapping of ___keys___ of type __K__ to ___values___ of type __V__ using a hashing function.

\

2.  Hash maps are useful when you want to look up data not by using an index,\
    as you can with vectors, but by using a ___key___ that can be of any type.

\

3.  One way to create an empty hash map is to use __`new()`__ and to add elements with __`insert()`__:
```rust
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Yellow"), 50);
```
-   Like vectors, hash maps are homogeneous:\
    all of the keys must have the ___same type___, and all of the values must have the ___same type___.

\

4.  We can get a value out of the hash map by providing its key to the __`get( )`__ method.
```rust
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Yellow"), 50);

        let team_name = String::from("Blue");
        let score = scores
                        .get(&team_name)    // : Option<&V>
                        .copied()           // : Option<V>
                        .unwrap_or(0);
```
\

5.  We can iterate over each ___key___ / ___value___ pair in a hash map in a similar manner as we do with vectors,\
    using a __`for`__ loop:
```rust
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Yellow"), 50);

        // key: &String
        // value: &i32
        for (key, value) in &scores {
            println!("{key}: {value}");
        }
```

\

6.  For types that implement the __`Copy`__ trait, like __`i32`__, the values are ___copied___ into the hash map.\
    For owned values like __`String`__, the values will be moved and the hash map will be the owner of those values.

\

7.  If we insert a ___key___ and a ___value___ into a hash map and then insert that ___same key___ with a ___different value___,\
    the value associated with that key will be ___replaced___.
```rust
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Blue"), 25);

        println!("{:?}", scores);   // -> {"Blue": 25}
```
\

8.  Hash maps have a special API called __`entry( )`__ that takes the ___key___ you want to check as a parameter.\
    The return value of the __`entry( )`__ method is an __`enum`__ called __`Entry`__ that represents a value that might or might not exist.
```rust
        use std::collections::HashMap;
        fn main() {
            let mut scores = HashMap::new();


            scores.insert(String::from("Blue"), 10);


            scores
                .entry(String::from("Yellow"))
                .or_insert(50);
            scores
                .entry(String::from("Blue"))
                .or_insert(100);


            println!("{:?}", scores);
            // -> {"Blue": 10, "Yellow": 50}
        }
```
\

9.  Another common use case for hash maps is to look up a key’s value and then update it based on the old value.
```rust
        use std::collections::HashMap;

        fn main() {
            let text = "hello world wonderful world";

            let mut map = HashMap::new();

            for word in text.split_whitespace() {
                let count = map
                            .entry(word)
                            .or_insert(0);
                *count += 1;
            }

            println!("{:?}", map);      // -> {"world": 2, "hello": 1, "wonderful": 1}
        }
```
-   The __`or_insert()`__ method returns a mutable reference (__`&mut V`__) to the value for the specified key.

\

10. By default, __`HashMap`__ uses a hashing function called __`SipHash()`__\
    that can provide resistance to denial-of-service (DoS) attacks involving hash tables.

    This is NOT the fastest hashing algorithm available,\
    but the trade-off for better security that comes with the drop in performance is worth it.

    If you profile your code and find that the default hash function is too slow for your purposes,\
    you can switch to another function by specifying a different ___hasher___.\
    A ___hasher___ is a type that implements the __`BuildHasher`__ trait.

    We’ll talk about traits and how to implement them in later chapters!

- ___Cargo.toml___
```toml
        [dependencies]
        fxhash = "0.2"
```
- ___./src/main.rs___
```rust
        use std::collections::HashMap;
        use fxhash::FxBuildHasher;

        fn main() {
            let mut scores
                : HashMap<String, i32, FxBuildHasher>
                = HashMap::with_hasher(FxBuildHasher::default());

            scores.insert(String::from("Blue"), 10);
            scores.insert(String::from("Yellow"), 20);

            for (team, score) in &scores {
                println!("{}: {}", team, score);
            }
        }
```


<hr/>
### Error Handling

1.  Rust groups errors into two major categories: ___recoverable___ and ___unrecoverable___ errors.

\

2.  For a recoverable error, such as a ___file not found___ error,\
    we most likely just want to report the problem to the user and retry the operation.

\

3.  Unrecoverable errors are always symptoms of bugs,\
    such as trying to access a location beyond the end of an array, and so we want to immediately stop the program.

\

4.  Most languages don't distinguish between these two kinds of errors and handle both in the same way,\
    using mechanisms such as exceptions.

    Rust does NOT have exceptions.\
    Instead, it has the type __`Result<T, E>`__ for recoverable errors,\
    and the __`panic!`__ macro that stops execution when the program encounters an unrecoverable error.


<hr/>
### Unrecoverable Errors with `panic!`

1.  There are __`2`__ ways to cause a panic in practice:
    - by taking an action that causes our code to panic (such as accessing an array past the end).
    - by explicitly calling the __`panic!`__ macro directly.

\

2.  By default, these panics will print a failure message, unwind, clean up the stack, and quit.\
    Via an environment variable,
    you can also have Rust display the call stack when a panic occurs
    to make it easier to track down the source of the panic.

\

4.  By default, when a panic occurs the program starts unwinding,\
    which means Rust walks back up the stack and cleans up the data from each function it encounters.

    However, walking back and cleaning up is a lot of work.\
    Rust, therefore, allows you to choose the alternative of immediately aborting, which ends the program without cleaning up.

    Remember that the program was using will then need to be cleaned up by the OS.\
    If in your project you need to make the resultant binary as small as possible,\
    you can switch from unwinding to aborting upon a panic by adding __`panic='abort'`__
    to the appropriate __`[profile]`__ sections in your __`Cargo.toml`__ file.

    For example, if you want to abort on panic in ___release___ mode, add this:
```toml
    [profile.release]
    panic='abort'
```
\

5.  Let's firstly try calling __`panic!`__ in a simple program:
```rust
        fn main() {
            panic!("Crash and burn!");
        }
```
\

6.  We can set the __`RUST_BACKTRACE`__ environment variable to __`1`__ to get a ___backtrace___ of exactly what happened to cause the error.\
    A ___backtrace___ is a list of all the functions that have been called to get to this point.
```zsh
        export RUST_BACKTRACE=1
        cargo run
```

<hr/>
### Recoverable Errors with `Result<T, E>`

1.  The __`Result<T, E>`__ enum is defined as having two variants, __`Ok(T)`__ and __`Err(E)`__, as follows:
```rust
        enum Result<T, E> {
            Ok(T),
            Err(E),
        }
```
-   The __`T`__ and __`E`__ are generic type parameters.

\

2.  Example Usage `1`:
```rust
    use std::fs::File;


    {
        let greeting_file_result = File::open("hello.txt");

        let greeting_file = match greeting_file_result {
            Ok(file)    => file,
            Err(error)  => panic!("Problem while opening the file: {error:?}"),
        }
    }


    {
        let greeting_file_result = File::open("hello.txt");

        let greeting_file = match greeting_file_result {
            Ok(file) => file,
            Err(error) => match error.kind() {
                ErrorKind::NotFound => match File::create("hello.txt") {
                    Ok(fc) => fc,
                    Err(e) => panic!("Problem creating the file: {e:?}"),
                },
                other_error => {
                    panic!("Problem opening the file: {other_error:?}");
                }
            }
        }
    }


    {
        let greeting_file =
            File::open("hello.txt") .unwrap_or_else(|error| {
                if error.kind() == ErrorKind::NotFound {
                    File::create("hello.txt").unwrap_or_else(|error| {
                        panic!("Problem creating the file: {error:?}");
                    })
                } else {
                    panic!("Problem opening the file: {error:?}");
                }
            });
    }
```
\

3.  Using __`match`__ works well enough, but it can be a bit verbose and does NOT always communicate intent well.

    If the __`Result`__ value is the __`Ok`__ variant, __`unwrap()`__ will return the value inside the __`Ok`__.\
    If the Result is the __`Err`__ variant, __`unwrap()`__ will call the __`panic!`__ macro for us:
```rust
        use std::fs::File;
        fn main() {
            let greeting_file = File::open("hello.txt").unwrap();
        }
```
\

4.  Similarly, the __`expect()`__ method lets us also choose the __`panic!`__ error message:
```rust
        use std::fs::File;
        fn main() {
            let greeting_file = File::open("hello.txt").expect("hello.txt NOT found!");
        }
```
\

5.  When a function's implementation calls something that might fail,\
    instead of handling the error within the function itself you can return the error to the calling code
    so that it can decide what to do.

    This is known as ___propagating___ the error and gives more control to the calling code,\
    where there might be more information or logic that dictates how the error should be handled
    than what you have available in the context of your code.
```rust
    use std::fs::File;
    use std::io::{self, Read};

    fn read_user_name_from_file() -> Result<String, io::Error> {
        let mut username_file = match File::open("user.txt") {
            Ok(file) => file,
            Err(e)   => return Err(e),
        }

        let mut username = String::new();
        match username_file.read_to_string(&mut username) {
            Ok(_) => Ok(username),
            Err(e) => Err(e),
        }
    }
```
\

6.  There is a shortcut for propagating errors via the ( __`?`__ ) operator.:
    - If the value is an __`Ok`__, the value inside the __`Ok`__ will get returned.\
    - If the value is an __`Err`__, the __`Err`__ will be returned from the function,\
    as if we had used the __`return`__ keyword so the error value gets propagated to the calling code.
```rust
        use std::fs::File;
        use std::io::{self, Read};

        fn read_user_name_from_file() -> Result<String, io::Error> {
            let mut username_file = File::open("user.txt")?;
            let mut username = String::new();
            username_file.read_to_string(&mut username)?;
            Ok(username)
        }
```
\

7.  There is a difference between what the __`match`__ expression does and what the ( __`?`__ ) operator does.\
    Error values that have the ( __`?`__ ) operator called on them go through the __`from()`__ function,\
    defined in the __`From`__ trait in the standard library, which is used to convert values from one type into another.

    When the ( __`?`__ ) operator calls the __`from()`__ function,\
    the error type received is converted into the error type defined in the return type of the current function.\
    This is useful when a function returns one error type to represent all the ways a function might fail,\
    even if parts might fail for many different reasons.

    For example, we could change the `read_username_from_file()` function
    to return a custom error type named `OurError` that we define.\
    If we also define __`impl From<io::Error> for OurError`__ to construct an instance of `OurError` from an `io::Error`.

\

8.  The ( __`?`__ ) operator eliminates a lot of boilerplate and makes this function’s implementation simpler.\
    We could even shorten this code further by chaining method calls immediately after the ( __`?`__ ).
```rust
    use std::fs::File;
    use std::io::{self, Read};

    fn read_user_name_from_file() -> Result<String, io::Error> {
        let mut username = String::new();

        File::open("user.txt")?.read_to_string(&mut username)?;

        Ok(username)
    }
```
\

9.  The ( __`?`__ ) operator can only be used in functions whose return type is compatible with the value the ( __`?`__ ) is used on.\
    Attempting to use the ( __`?`__ ) in the main function that returns __`()`__ won’t compile.
```rust
    use std::fs::File;

    fn main() {
        let greeting_file = File::open("user.txt")?
    //                                            ^ cannot use the `?` operator in a function that returns `()`
    }
```
-   Luckily, main can also return a __`Result<(), E>`__.
```rust
    use std::error::Error;
    use std::fs::File;

    fn main() -> Result<(), Box<dyn Error>> {
        let greeting_file = File::open("hello.txt")?;

        Ok(())
    }
```
-   The __`Box<dyn Error>`__ type is a ___trait object___, which we’ll talk about in later chapters!
-   For now, you can read it as to mean ___any kind of error___.

\

10. The error message also mentioned that ( __`?`__ ) can be used with __`Option<T>`__ values as well.\
    As with using ( __`?`__ ) on __`Result<T, E>`__,\
    you can only use ( __`?`__ ) on __`Option<T>`__ in a function that returns an __`Option<T>`__.\
    Here either the value inside __`Some(T)`__ or the __`None`__ will be returned.
```rust
        fn last_char_of_first_line(text: &str) -> Option<char> {
            text.lines().next()?.chars().last()
        }
```
\

11. The __`main()`__ function may return any types that implement the __`std::process::Termination`__ trait,\
    which contains a function __`report()`__ that returns an __`ExitCode`__.\
    Consult the standard library documentation for more information on implementing the trait for your own types.



<hr/>
### Generic Types, Traits, and Lifetimes

1.  Starting function:
```rust
        fn largest_i32(list: &[i32]) -> &i32 {
            let mut largest = &list[0];

            for item in list {
                if item > largest {
                    largest = item;
                }
            }

            largest
        }

        fn largest_char(list: &[char]) -> &char {
            let mut largest = &list[0];

            for item in list {
                if item > largest {
                    largest = item;
                }
            }

            largest
        }


        fn main() {
            let number_list = vec![34, 50, 25, 100, 65];

            let result = largest_i32(&number_list);
            println!("{result}");

            // --- --- --- ---

            let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

            let result = largest_i32(&number_list);
            println!("{result}");
        }
```
\

2.  Modify it to generic:
```rust
        fn largest<T>(list: &[T]) -> &T {
            let mut largest = &list[0];

            for item in list {
                if item > largest {
                    largest = item;
                }
            }

            largest
        }


        fn main() {
            let number_list = vec![34, 50, 25, 100, 65];

            let result = largest(&number_list);
            println!("{result}");

            // --- --- --- ---

            let char_list = vec!['a', 'x', 'q', 'p'];

            let result = largest(&char_list);
            println!("{result}");
        }
```
-   But if we compile this code right now, we’ll get this error:
```txt
        error[E0369]: binary operation `>` cannot be applied to type `&T`
        --> src/main.rs:5:17
          |
        5 |         if item > largest {
          |            ---- ^ ------- &T
          |            |
          |            &T
          |
        help: consider restricting type parameter `T`
          |
        1 | fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
          |             ++++++++++++++++++++++
```
-   The help text mentions __`std::cmp::PartialOrd`__, which is a ___trait___, and we’re going to talk about traits in the next section.\

-   For now, know that this error states that the body of __`largest()`__ won’t work for all possible types that __T__ could be.\
    Because we want to compare values of type __T__ in the body, we can only use types whose values can be ordered.

-   To enable comparisons, the standard library has the __`std::cmp::PartialOrd`__ trait that you can implement on types.\
    By following the help text’s suggestion, we restrict the types valid for __T__ to only those that implement __`PartialOrd`__,\
    and this example will compile, because the standard library implements __`PartialOrd`__ on both __`i32`__ and __`char`__.

\

3.  We can also define __`struct`__ to use a generic type parameter in one or more fields using the __`<>`__ syntax.
```rust
    struct Point<T> {
        x: T,
        y: T,
    }

    impl<T> Point<T> {
        fn new(x: T, y: T) -> Point<T> {
            Point { x, y }
        }

        fn test(x: T) {}

        fn do_sth(&self, data: T) {}

        fn do_that<U>(&self, data: U) {}

        fn coordinate(&self) -> (&T, &T) {
            (&self.x, &self.y)
        }

        fn convert_with<U>(self, other: Point<U>) -> Point<U> {
            Point {
                x: other.x,
                y: other.y,
            }
        }
    }

    // implement methods only on Point<f64> instances
    impl Point<f64> {
        fn distance_from_origin(&self) -> f64 {
            (self.x.powi(2) + self.y.powi(2))
            .sqrt()
        }
    }

    fn get_value<T>(value: T) -> T {
        value
    }



    fn main() {
        let p1 = Point { x:5,   y:10  };                // : Point<i32>
        let p2: Point<f64> = Point { x:1.0, y:2.0 };    // : Point<f64>

        let coord1 = p1.coordinate();
        println!("(x:{}, y:{})", coord1.0, coord1.1);

        let coord2 = p2.coordinate();

        // Only methods have its own generic parameter can be called this way:
        //      let p = p1.convert_with::<f64>(p2);
        // So, calling convert_with() this way above is ok,
        // but you cannot call coordinate() this way, because it does not have its own generic param.
        let p = p1.convert_with(p2);

        let x = get_value(10u8);
        let x = get_value::<u8>(10u8);

        let x = get_value(10);
        let x = get_value::<i32>(10);
        let x = get_value::<f64>(3.14);

        // will NOT compile!
        // let p3 = Point { x:1, y:2.0 };
        let p3 = Point::<f64>::new(1f64, 2.0);          // : Point<f64>

        Point::test(1);
        Point::test(1i32);
        Point::<i32>::test(1);
        Point::<i32>::test(1i32);

        // Cannot call with turbofish syntax here.
        p3.do_sth(1f64);

        // This you can.
        p3.do_that(1);
        p3.do_that(1i32);
        p3.do_that::<i32>(1);
        p3.do_that::<i32>(1i32);
    }
```


<hr/>
### Publicity of Structure

1.  When you declare a __`pub struct`__,\
    the structure itself is ___public___ and can be accessed from outside its defining module.\
    However, its fields are still ___private___ by default unless you make them __`pub`__ as well.
```rust
    mod my_module {
        // Private struct, only accessible within this module
        struct PrivateStruct {
            x: i32,
            y: i32,
        }

        // Public struct, but its fields are private
        pub struct PublicStruct {
            x: i32, // private field
            pub y: i32, // public field
        }

        pub fn create_structs() -> PublicStruct {
            // PrivateStruct can be instantiated inside its own module
            let _private = PrivateStruct { x: 5, y: 10 };

            // PublicStruct can also be instantiated inside the module
            PublicStruct { x: 5, y: 10 }
        }
    }

    fn main() {
        // `PrivateStruct` is not accessible outside the module
        // let p = my_module::PrivateStruct { x: 5, y: 10 }; // ERROR: struct is private

        // `PublicStruct` is accessible because it's `pub`
        let p = my_module::PublicStruct { y: 10 }; // Can access `y` (public field)

        // However, `x` is still private and cannot be accessed
        // let p = my_module::PublicStruct { x: 5, y: 10 }; // ERROR: field `x` is private
    }
```


<hr/>
### Traits: Defining Shared Behavior

1.  A ___trait___ defines the functionality a particular type has and can share with other types.

\

2.  We can use ___trait bounds___ to specify that a generic type can be any type that has certain behavior.

\

3.  We want to make a media aggregator library crate named ___aggregator___
    that can display summaries of data that might be stored in a ___NewsArticle___ or ___Tweet___ instance.\
    To do this, we need a summary from each type, and we’ll request that summary by calling a summarize method on an instance.
```rust
        pub trait Summary {
            fn summarize(&self) -> String;
        }
```
-   Here, we declare a ___trait___ using the __`trait`__ keyword and then the trait’s name, which is ___Summary___ in this case.

-   We also declare the trait as __`pub`__ so that crates depending on this crate can make use of this trait too.

-   Inside the curly brackets, we declare the method signatures that describe the behaviors of the types that implement this trait.

-   After the method signature, instead of providing an implementation within curly brackets, we use a semicolon ( __`;`__ ).

-   Each type implementing this trait must provide its own custom behavior for the body of the method.

-   The compiler will enforce that any type that has the ___Summary___ trait will have the method __`summarize()`__ defined with this signature exactly.

-   A trait can have multiple methods in its body: the method signatures are listed one per line, and each line ends in a semicolon.

\

4.  To implement a ___trait___ on a type:
```rust
        pub struct NewsArticle {
            pub author:   String,
            pub content:  String,
            pub headline: String,
            pub location: String,
        }

        impl Summary for NewsArticle {
            fn summarize(&self) -> String {
                format!("{}, by {} ({})\n\t{}",
                    self.headline,
                    self.author,
                    self.location,
                    self.content);
            }
        }

        pub struct Tweet {
            pub username: String,
            pub content:  String,
            pub reply:    bool,
            pub retweet:  bool,
        }

        impl Summary for Tweet {
            fn summarize(&self) -> String {
                format!("{}: {}", self.username, self.content);
            }
        }
```
\

5.  To use them, in Rust, if you want to use the trait's methods (like __`summarize()`__ from the ___Summary___ trait in the example),\
    you MUST also import the trait into the scope.
```rust
        use aggregator::{Summary, Tweet};

        fn main() {
            let tweet = Tweet {
                username: String::from("some_tweet_name"),
                content: String::from("some_content"),
                reply: false,
                retweet: false,
            };

            println!("I tweeted: {}", tweet.summarize());
        }
```
\

6.  Other crates that depend on the ___aggregator___ crate can also bring the ___Summary___ trait into scope
    to implement ___Summary___ on their own types.

    One restriction to note is that we can implement a ___trait___ on a type, only if either the trait or the type, or both, are ___local___ to our crate.

    For example,\
    we can implement standard library traits like __`Display`__ on a custom type like ___Tweet___ as part of our ___aggregator___ crate functionality,\
    because the type ___Tweet___ is local to our ___aggregator___ crate.

    We can also implement ___Summary___ on __`Vec<T>`__ in our ___aggregator___ crate because the trait ___Summary___ is local to our ___aggregator___ crate.

    But we can’t implement external traits on external types.
    - For example, we can’t implement the __`Display`__ trait on __`Vec<T>`__ within our ___aggregator___ crate.

    This restriction is part of a property called ___coherence___, and more specifically the ___orphan rule___.
    - This rule ensures that other people’s code can’t break your code and vice versa.\
    - Without the rule, two crates could implement the same trait for the same type,\
      and Rust wouldn’t know which implementation to use.

\

7.  Trait can have default implementations, and be overriden specifically.
```rust
        pub trait Summary {
            fn summarize(&self) -> String {
                String::from("(Read more...)");
            }
        }


        pub struct NewsArticle {
            pub author:   String,
            pub content:  String,
            pub headline: String,
            pub location: String,
        }
        // To use a default implementation.
        impl Summary for NewsArticle {}


        pub struct Tweet {
            pub username: String,
            pub content:  String,
            pub reply:    bool,
            pub retweet:  bool,
        }
        // To use a custom implementation.
        impl Summary for Tweet {
            fn summarize(&self) -> String {
                format!("{}: {}", self.username, self.content);
            }
        }
```
\

8.  Now that you know how to define and implement traits,\
    we can explore how to use traits to define functions that accept many different types.
```rust
    pub fn notify(item: &impl Summary) {
        println!("{}", item.summarize());
    }
```
-   Instead of a concrete type for the ___item___ parameter, we specify the __`impl`__ keyword and the ___trait name___.
-   This parameter accepts any type that implements the specified trait.

\

9.  The __`impl`__ Trait syntax works for straightforward cases but is actually a ___syntax sugar___
    for a longer form known as a ___trait bound___,\
    it looks like this:
```rust
    pub fn notify<T: Summary>(item: &T) {
        println!("{}", item.summarize());
    }
```
\

10. In Rust, you can NOT have fields inside a ___trait___. ___Traits___ in Rust are used to define shared behavior.

\

11. We can also specify more than one ___trait bound___.\
    Say we wanted notify to use display formatting as well as summarize on item,\
    we specify in the notify definition that item must implement both __`Display`__ and __`Summary`__.\
    We can do so using the __`+`__ syntax:
```rust
    {
        pub fn notify(item: &(impl Summary + Display)) {}
    }

    {
        pub fn notify<T: Summary + Display>(item: &T) {}
    }
```
\

12. Rust has also an alternate syntax
    for specifying ___trait bounds___ inside a __`where`__ clause after the function signature.
```rust
    {
        fn some_func<T: Display + Clone, U: Debug + Clone>(t: &T, u: &U) -> i32 {
        }
    }

    {
        fn some_func<T, U>(t: &T, u: &U) -> i32
        where
            T: Display + Clone,
            U: Debug + Clone,
        {
        }
    }
```
\

13. We can also use the __`impl`__ trait syntax in the return position
    to return a value of some type that implements a trait.
```rust
    fn return_summarizable() -> impl Summary {
        Tweet {
            username: String::from("some_tweet_name"),
            content: String::from("some_content"),
            reply: false,
            retweet: false,
        }
    }
```
\

14. You can also use ___trait bounds___ to conditionally implement methods.
```rust
    use std::fmt::Display;

    struct Pair<T> {
        x: T,
        y: T,
    }

    impl<T> Pair<T> {
        fn new(x: T, y: T) -> Self {
            Self { x, y }
        }
    }

    impl<T: Display + PartialOrd> Pair<T> {
        fn cmp_display(&self) {
            if self.x < self.y {
                println!("{}", self.y);
            } else {
                println!("{}", self.x);
            }
        }
    }

    impl Pair<char> {
        fn display(&self) {
            println!("{}{}", self.x, self.y);
        }
    }
```
\

15. We can also conditionally implement a trait for any type that implements another trait.\
    Implementations of a trait on any type that satisfies the trait bounds are called __blanket implementations__
    and are used extensively in the Rust standard library.

    For example, the standard library implements the `ToString` trait on any type that implements the `Display` trait.\
    The `impl` block in the standard library looks similar to this code:
```rust
    impl<T: Display> ToString for T {
        // --snip--
    }
```
-   Because the standard library has this blanket implementation,\
    we can call the `to_string()` method defined by the `ToString` trait on any type that implements the `Display` trait.
```rust
    let s = 3.to_string();
```


<hr/>
### Validating References with Lifetimes

1.  `Lifetimes` are another kind of generic that we’ve already been using.\
    Rather than ensuring that a type has the behavior we want,
    lifetimes ensure that references are _valid_ as long as we need them to be.

\

2.  _Every reference_ in Rust has a lifetime, which is the scope for which that reference is valid.\
    Most of the time, lifetimes are implicit and inferred, just like most of the time, types are inferred.

    We __MUST__ annotate types only when multiple types are possible.\
    In a similar way,\
    we __MUST__ annotate lifetimes when the lifetimes of references could be related in a few different ways.

\

3.  The main aim of lifetimes is to prevent dangling references,\
    which cause a program to reference data other than the data it's intended to reference.\
    Consider the following example. (Which will cause a compile-time error).
```rust
    fn main() {
        let r;

        {
            let x = 10;
            r = &x;
        }

        println!("{r}");
    }
```
4.  The Rust compiler has a `borrow checker` that compares scopes to determine whether all borrows are.
```rust
    fn main() {
        let r;              // ----------+-- 'a
                            //           |
        {                   //           |
            let x = 10;     // -+-- 'b   |
            r = &x;         //  |        |
        }                   // -+        |
                            //           |
        println!("{r}");    // ----------+
    }
```
-   Here, we’ve annotated the lifetime of ___r___ with `'a` and the lifetime of ___x___ with `'b`.\
    As you can see, the inner `'b` block is much smaller than the outer `'a` lifetime block.\
    At compile time,\
    Rust compares the size of the two lifetimes and sees that ___r___ has a lifetime of `'a` but that it refers to memory with a lifetime of `'b`.
    The program is rejected because `'b` is _shorter_ than `'a`: the subject of the reference doesn’t live as long as the reference.

-   The following code will work.
```rust
    fn main() {
        let x = 10;         // ----------+-- 'b
                            //           |
        let r = &x;         // -+-- 'a   |
                            //  |        |
        println!("{r}");    //  |        |
                            // -+        |
    }                       // ----------+
```
-   Here, ___x___ has the lifetime `'b`, which in this case is larger than `'a`.\
    This means ___r___ can reference ___x___ because Rust knows that the reference in ___r___ will always be valid while ___x___ is valid.

\

5.  Consider the following example now.\
    We’ll write a function that returns the longer of two string slices.\
    But it will cause compiler error.
```rust
    fn main() {
        let string1 = String::from("abcd");
        let string2 = "xyz";

        let result = longest(string1.as_str(), string2);
        println!("{}", result);
    }

    fn longest(x: &str, y: &str) -> &str {a
    //            ----     ----     ^ expected named lifetime parameter
        if x.len() > y.len() {
            s
        } else {
            y
        }
    }
```
-   We'll be working on to fix this problem from now on.

\

6.  `Lifetime annotations` do NOT change how long any of the references live.\
    Rather, they describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes.

    Lifetime annotations have a slightly unusual syntax,\
    the names of lifetime parameters must start with an apostrophe (`'`) and are usually all lowercases and very short.\
    Most people use the name `'a` for the first lifetime annotation.\
    We place lifetime parameter annotations after the `&` of a reference,\
    using a space to separate the annotation from the reference’s type.
```rust
    &i32            // a reference
    &'a i32         // a reference with an explicit lifetime
    &'a mut i32     // a mutable reference with an explicit lifetime
```
\

7.  To use lifetime annotations in function signatures,
    we need to declare the generic lifetime parameters inside angle brackets between the function name and the parameter list,
    just as we did with generic type parameters.
```rust
    fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
        if x.len() > y.len() {
            x
        } else {
            y
        }
    }
```
-   The function signature now tells Rust that for some lifetime `'a`, the function takes two parameters,\
    both of which are string slices that live at least as long as lifetime `'a`.
    The function signature also tells Rust that the string slice returned from the function will live at least as long as lifetime `'a`.
    The returned value must adhere _exactly_ to the lifetime specified in the function signature.
>
    In practice, it means that the lifetime of the reference returned by the `longest()` function\
    is the same as the _smaller_ of the lifetimes of the values referred to by the function arguments.

\

8.  Consider following example, it will raise a compiler error.
```rust
    fn main() {
        let string1 = String::from("abcd");
        let result;
        {
            let string2 = String::from("xyz");
            //  ------- binding `string2` declared here
            result = longest(string1.as_str(), string2.as_str());
            //                                  ^^^^^^^ borrowed value does not live enough
        }
        println!("{}", result);
        //             ------ borrow later used here
    }
```
    But this will compile:
```rust
    fn main() {
        let string1 = String::from("abcd");
        let result;
        {
            let string2 = String::from("xyz");
            result = longest(string1.as_str(), string2.as_str());
            println!("{}", result);
        }
    }
```
\

9.  So far, the `struct` we’ve defined all hold owned types.\
    We can define `struct` to hold _references_,
    but in that case we would need to add a lifetime annotation on every reference in the struct’s definition.
```rust
    struct ImportantExcerpt<'a> {
        part: &'a str,
    }

    fn main() {
        let novel = String::from(
            "Here goes the story. Once upon a time..."
        );

        let first_sentence = novel.split('.').next().unwrap();
        let excerpt = ImportantExcerpt {
            part: first_sentence,
        }
    }
```
\

10. Lifetime Elision.
```rust
    {
        fn test<'a>(s: &'a str) -> &'a str {}
        fn test(s: &str) -> &str {}
    }
```
\

11. Lifetime names for `struct` fields always need to be declared after the `impl` keyword\
    and then used after the struct’s name because those lifetimes are part of the struct's type.
```rust
    impl<'a> ImportantExcerpt<'a> {
        fn part(&self) -> &str {
            self.part
        }

        fn part_again(&self) -> &'a str {
            self.part
        }

        fn part_again_again(&'a self) -> &'a str {
            self.part
        }
    }
```
\

12. One special lifetime we need to discuss is `'static`,\
    which denotes that the affected reference can live for the _entire duration of the program_.\
    All string literals have the `'static` lifetime, which we can annotate as follows:
```rust
    let s: &'static str = "i have a static lifetime.";
```
-   You might see suggestions to use the `'static` lifetime in error messages.
-   But before specifying `'static` for a reference,\
    think about whether the reference you have actually lives the entire lifetime of your program or not,
    and whether you want it to.

\

13. Let’s briefly look at the syntax of specifying generic type parameters, trait bounds,\
    and lifetimes all in one function!
```rust
    use std::fmt::Display;

    fn longest_with_an_anouncement<'a, T> (
        x: &'a str,
        y: &'a str,
        ann: T,
    ) -> &'a str
    where
        T: Display
    {
        println!("Announcement: {}!", ann);

        if x.len() < y.len() {
            x
        } else {
            y
        }
    }
```
\

14. Rust does support __lifetime bounds__.\
    Lifetime bounds are used to specify how long a reference should live relative to another reference or type.\
    `'a: 'b` means that `'a` must live AT LEAST as long as `'b`, which means `'a >= 'b` in lifetime. (`'a` outlives `'b`).
```rust
    fn test<'a, 'b: 'a>(
        x: &'a str,
        y: &'b str,
    ) -> &'a str
    {
        x
    }

    fn test2<'a, 'b>(
        x: &'a str,
        y: &'b str,
    ) -> &'a str
    where
        'b : 'a
    {
        x
    }
```
-   We'll have more detailed explanations in the later chapters!



<hr/>
### Automated Tests

1.  Tests are Rust functions that verify that the non-test code is functioning in the expected manner.\
    The bodies of test functions typically perform these three actions:

    a) Set up any needed data or state.
    b) Run the code you want to test.
    c) Assert that the results are what you expect.

\

2.  At its simplest, a test in Rust is a function that’s annotated with the `test` attribute.\
    Attributes are metadata about pieces of Rust code.\
    One example is the `derive` attribute we used with structs.\
    To change a function into a test function, add `#[test]` on the line before `fn`.\
    When you run your tests with the `cargo test` command,\
    Rust builds a _test runner binary_ that runs the annotated functions
    and reports on whether each test function passes or fails.

> - Setup project.
```zsh
    $ cargo new adder --lib
        Created library `adder` project
    $ cd adder/
```
> - src/lib.rs
```rust
    pub fn add(lhs: usize, rhs: usize) -> usize {
        lhs + rhs
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn it_works() {
            let four = add(2, 2);
            assert_eq!(four, 4);
        }
    }
```
> - Run test.
```zsh
    $ cargo test

    Compiling adder v0.1.0 (file:///projects/adder)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.57s
    Running unittests src/lib.rs (target/debug/deps/adder-92948b65e88960b4)

    running 1 test
    test tests::it_works ... ok
    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

        Doc-tests adder

    running 0 tests
    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```
> - Adding a must fail test.
```rust
    pub struct Guess {
        value: i32,
    }

    impl Guess {
        pub fn new(value: i32) -> Guess {
            if value < 1 {
                panic!(
                    "Guess value too small! Should be between 1 and 100! Got {value}"
                );
            } else if value > 100 {
                panic!(
                    "Guess value too big! Should be between 1 and 100! Got {value}"
                );
            }

            Guess { value }
        }
    }

    pub fn add(lhs: usize, rhs: usize) -> usize {
        lhs + rhs
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn it_works() {
            let four = add(2, 2);
            assert_eq!(four, 4);
            assert!(four == 4);
            assert!(four == 4, "Error message here!");
        }

        #[test]
        fn it_also_works() -> Result<(), String> {
            let four = add(2, 2);

            if result == 4 {
                Ok(())
            } else {
                Err(String::from("two plus two does not equal four!"))
            }
        }

        #[test]
        fn must_fail() {
            panic!("Anyway this fails...");
        }

        #[test]
        #[should_panic]
        fn too_large_guess() {
            Guess::new(10_000);
        }

        // `expected` parameter makes sure the panic message contains certain text.
        #[test]
        #[should_panic(expected="too small!")]
        fn too_small_guess() {
            Guess::new(-10);
        }
    }
```
\

3.  When you run multiple tests, by default they run in parallel using threads,\
    to finish running faster and you get feedback quicker.
>
    However, because the tests are running at the same time,\
    you must make sure your tests don’t depend on each other or on any shared state,\
    including a shared environment, such as the current working directory or environment variables.
>
    For example, say each of your tests runs some code that creates a file on disk named `test-output.txt`
    and writes some data to that file.\
    Then each test reads the data in that file and asserts that the file contains a particular value,
    which is different in each test.\
    Because the tests run at the same time, one test might overwrite the file in the time between another.\
    The second test will then fail, not because the code is incorrect
    but because the tests have interfered with each other while running in parallel.
>
    One solution is to make sure each test writes to a different file,\
    another solution is to run the tests one at a time.
>
    If you don’t want to run the tests in parallel
    or if you want more fine-grained control over the number of threads used,\
    you can send the `-- --test-threads` flag and the number of threads you want to use to the test binary.\
    Take a look at the following example:
```zsh
    $ cargo test -- --test-threads=1
```
    We set the number of test threads to `1`, telling the program not to use any parallelism.\
    Running the tests this way will take longer than running them in parallel,\
    but the tests won’t interfere with each other if they share state.

\

4.  By default, if a test passes, Rust’s test library captures anything printed to standard output.\
    For example, if we call `println!` in a the function to be tested or in the test and the test passes,\
    we won’t see the `println!` output in the terminal, we’ll see only the line that indicates the test passed.\
    If a test fails, we’ll see whatever was printed to standard output with the rest of the failure message.
>
    If we want to see printed values for passing tests as well,
    we can tell Rust to also show the output of successful tests with `-- --show-output`:
```zsh
    $ cargo test -- --show-output
```

\

5.  Sometimes, running a full test suite can take a long time.\
    If you’re working on code in a particular area, you might want to run only the tests pertaining to that code.\
    You can choose which tests to run by passing cargo test the name or names of the test(s)
    you want to run as an argument.

> - src/lib.rs
```rust
    pub fn add_two(a: usize) -> usize {
        a + 2
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn add_two_and_two() {
            let result = add_two(2);
            assert_eq!(result, 4);
        }

        #[test]
        fn add_three_and_two() {
            let result = add_two(3);
            assert_eq!(result, 5);
        }

        #[test]
        fn one_hundred() {
            let result = add_two(100);
            assert_eq!(result, 102);
        }
    }
```
-   To run one specific test only.
```zsh
    $ cargo test one_hundred
```
-   And since `add_two_and_two()` and `add_three_and_two()` both have "add" in their names,\
-   if you only pass `add` as test name, it will run all tests contains `add` as part of their names. Thus perform multiple tests.
```zsh
    $ cargo test add
```
\

6.  We can also ignore some tests unless specifically requested.\
    After `#[test]`, we add the `#[ignore]` line to the test we want to exclude.
```rust
    pub fn add(left: usize, right: usize) -> usize {
        left + right
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn it_works() {
            let result = add(2, 2);
            assert_eq!(result, 4);
        }

        #[test]
        #[ignore]
        fn expensive_test() {
            // code that takes an hour to run
        }
    }
```
> - Run without ignored test(s).
```zsh
    $ cargo test
```
> - Run only ignored test(s).
```zsh
    $ cargo test -- --ignored
```
> - Run all test(s).
```zsh
    $ cargo test -- --include-ignored
```
\

7.  The purpose of __unit tests__ is to test each unit of code in isolation from the rest of the code
    to quickly pinpoint where code is and isn’t working as expected.
>
    You’ll put unit tests in the `src/` directory in each file with the code that they’re testing.\
    The convention is to create a module named `mod tests` in each file to contain the test functions
    and to annotate the module with `#[cfg(test)]`.
>
    The `#[cfg(test)]` annotation on the `mod tests` module tells Rust to compile and run the test code only when you run `cargo test`,
    not when you run `cargo build`.
>
    You’ll see that because integration tests go in a different directory, they don’t need the `#[cfg(test)]` annotation.
    However, because unit tests go in the same files as the code,\
    you’ll use `#[cfg(test)]` to specify that they shouldn’t be included in the compiled result.


<hr/>
### Integration Test

1.  In Rust, integration tests are entirely external to your library.\
    They use your library in the same way any other code would,\
    which means they can only call functions that are part of your library’s public API.

\

2.  Their purpose is to test whether many parts of your library work together correctly.\
    Units of code that work correctly on their own could have problems when integrated,\
    so test coverage of the integrated code is important as well.

\

3.  To create integration tests, you first need a `tests/` directory.
    Usually here:
```console
    adder/
    |---------- Cargo.lock
    |---------- Cargo.toml
    |---------- src/
                |---------- lib.rs
    |---------- tests/
                |---------- integration_test.rs
```
-   We create a `tests/` directory at the top level of our project directory, next to `src/`.
-   Cargo knows to look for integration test files in this directory.\
    We can then make as many test files as we want, and Cargo will compile each of the files as an individual crate.

> - tests/integration_test.rs
```rust
    use adder::add_two;

    #[test]
    fn it_adds_two() {
        let result = add_two(2);
        assert_eq!(result, 4);
    }
```
-   We don’t need to annotate any code in `tests/integration_test.rs` with `#[cfg(test)]`.
    Cargo treats the tests directory specially and compiles files in this directory only when we run `cargo test`.

\

4.  To run all the tests in a particular integration test file,\
    use the `--test` argument of `cargo test` followed by the name of the file
```zsh
    $ cargo test --test integration_test
```
    Or more general:
```zsh
    $ cargo test --test <test-file-name> <test-func-name>
```
\

5.  Files in subdirectories of the `tests/` directory don’t get compiled as separate crates
    nor have sections in the test output.


<hr/>
### Closures: Anonymous Functions that Capture Their Environment.

1.  Rust’s `closures` are __anonymous functions__ you can save in a variable or pass as arguments to other functions.\
    You can create the `closure` in one place and then call the `closure` elsewhere to evaluate it in a different context.\
    Unlike functions, `closures` can capture values from the scope in which they’re defined.

\

2.  We’ll first examine how we can use closures
    to capture values from the environment they’re defined in for later use.
```rust
    #[derive(Debug, PartialEq, Copy, Clone)]
    enum ShirtColor {
        Red,
        Blue,
    }

    struct Inventory {
        shirts: Vec<ShirtColor>,
    }

    impl Inventory {
        fn give_away(&self, preference: Option<ShirtColor>) -> ShirtColor {
            preference.unwrap_or_else(|| self.most_stocked())
        }

        fn most_stocked(&self) -> ShirtColor {
            let mut num_red = 0;
            let mut num_blu = 0;

            for color in &self.shirts {
                match color {
                    ShirtColor::Red  => num_red += 1,
                    ShirtColor::Blue => num_blu += 1,
                }
            }

            if num_red > num_blu {
                ShirtColor::Red
            } else {
                ShirtColor::Blue
            }
        }
    }

    fn main() {
        let inventory = Inventory {
            shirts: vec![
                ShirtColor::Blue,
                ShirtColor::Blue,
                ShirtColor::Red,
                ShirtColor::Blue,
            ],
        };

        let preference = Some(ShirtColor::Blue);
        let shirt = inventory.give_away(preference);
        println!(
            "The user who prefers {:?} gets {:?}.",
            preference,
            shirt,
        );

        let preference = None;
        let shirt = inventory.give_away(preference);
        println!(
            "The user who prefers {:?} gets {:?}.",
            preference,
            shirt,
        );
    }
```
-   You may wonder why here no need to `borrow` the value of `Option<ShirtColor>` in `give_away()`,\
    since `Option<ShirtColor>` holds a value of `ShirtColor`, which implements `Copy`,
    the `Option` also benefits from this behavior.

\

3.  Closures don’t usually require you to annotate the types of the parameters or the return value like `fn` functions do.\
    But as with variables, we can add type annotations if we want to increase explicitness
    and clarity at the cost of being more verbose than is strictly necessary.
```rust
    {
        fn add_one_v1    (x: u32) -> u32 { x + 1 }
        let add_one_v2 = |x: u32| -> u32 { x + 1 };
        let add_one_v3 = |x|             { x + 1 };
        let add_one_v4 = |x|               x + 1  ;
    }
```
\

4.  For closures, if no explicit notation,\
    the compiler will infer one concrete type for each parameters and for return value when first time using them.
```rust
    fn main() {
        let simple_closure = |x| x;

        let s = simple_closure(String::from("hello"));
        let n = simple_closure(10);
    }
```
-   The code above will cause a compiler ERROR!
-   Because the first time we call `example_closure()` with the `String` value,\
    the compiler infers the type of ___x___ and the return type of the closure to be `String`.
-   Those types are then __locked into the closure__ in `example_closure()`,\
    and we get a type error when we next try to use a different type with the same closure.

\

5.  Closures can capture values from their environment in three ways,\
    which directly map to the three ways a function can take a parameter.\
    `Borrowing immutably`, `borrowing mutably`, and `taking ownership`.

    The closure will decide which of these to use
    based on what the body of the function does with the captured values.
```rust
    fn main() {
        let v = vec![1, 2, 3];
        println!("Before defining closure {:?}", v);

        let borrowed_immutably = || println!("From closure {:?}", v);

        println!("Before calling  closure {:?}", v);
        borrowed_immutably();
        println!("After  calling  closure {:?}", v);

        println!("\n---- ---- ---- ----\n");

        let mut v = vec![1, 2, 3];
        println!("Before defining closure {:?}", v);

        // A closure that mutates the environment (in this case, it modifies v)
        // needs to be declared as `mut` when the closure itself is called.
        let mut borrowed_mutably = || v.push(10);

        println!("Before calling  closure {:?}", v);
        borrowed_mutably();
        println!("After  calling  closure {:?}", v);
    }

    // Before defining closure [1, 2, 3]
    // Before calling  closure [1, 2, 3]
    // From closure [1, 2, 3]
    // After  calling  closure [1, 2, 3]
    //
    // ---- ---- ---- ----
    //
    // Before defining closure [1, 2, 3]
    // Before calling  closure [1, 2, 3]
    // From closure [1, 2, 3, 10]
    // After  calling  closure [1, 2, 3, 10]
```
\

6.  If you want to force the closure to take ownership of the values it uses in the environment
    even though the body of the closure does NOT strictly need ownership,\
    you can use the `move` keyword before the parameter list.
```rust
    use std::thread;

    fn main() {
        let v = vec![1, 2, 3];

        thread::spawn(
            move || println!("{:?}", v)
        ).join().unwrap();
    }
```
\

7.  The way a closure captures and handles values from the environment
    affects which traits the closure implements,\
    and traits are how `fn` and `struct` can specify what kinds of closures they can use.

    Closures will automatically implement one, two, or all three of these `Fn` traits,\
    in an additive fashion, depending on how the closure’s body handles the values:
>
    `FnOnce` applies to closures that can be called once.\
    All closures implement at least this trait, because all closures can be called.\
    A closure that moves captured values out of its body will only implement `FnOnce` and none of the other `Fn` traits,
    because it can only be called once.\
    (ownership taken)
>
    `FnMut` applies to closures that don’t move captured values out of their body,
    but that might mutate the captured values.\
    These closures can be called more than once.\
    (mutable borrow)
>
    `Fn` applies to closures that don’t move captured values out of their body and also mutate captured values,
    as well as closures that capture nothing from their environment.\
    These closures can be called more than once without mutating their environment,
    which is important in cases such as calling a closure multiple times concurrently.\
    (immutably borrow)

\

8.  Let’s look at the definition of the `unwrap_or_else()` method on `Option<T>` that we used
```rust
    impl<T> Option<T> {
        pub fn unwrap_or_else<F>(self, f: F) -> T
        where
            F: FnOnce() -> T
        {
            match self {
                Some(x) => x,
                None => f(),
            }
        }
    }
```
-   The trait bound specified on the generic type `F` is `FnOnce() -> T`,
    which means `F` must be able to be called once, take no arguments, and return a `T`.
    Using `FnOnce` in the trait bound expresses the constraint that `unwrap_or_else()` is only going to call ___f___ at most one time.
-   In the body of `unwrap_or_else()`, we can see that if the `Option` is `Some`, ___f___ won’t be called.
-   If the `Option` is `None`, ___f___ will be called once.
-   Because all closures implement `FnOnce`, `unwrap_or_else()` accepts all three kinds of closures and is as flexible as it can be.

\

9.  Note:\
    Functions can implement all three of the `Fn` traits too.\
    If what we want to do doesn’t require capturing a value from the environment,\
    we can use the name of a function rather than a closure where we need something that implements one of the `Fn` traits.
    For example, on an `Option<Vec<T>>` value, we could call `unwrap_or_else(Vec::new)` to get a new, empty vector if the value is `None`.
```rust
    fn add_one(x: i32) -> i32 { x + 1 }

    fn do_twice<F>(f: F, arg: i32) -> i32
    where
        F: Fn(i32) -> i32,
    {
        f(arg) + f(arg)
    }

    fn main() {
        let result = do_twice(|x| x * 2, 5);
        println!("{}", result);                 // -> 20

        let result = do_twice(add_one, 5);
        println!("{}", result);                 // -> 12
    }
```


<hr/>
### Iterators

1.  An iterator is responsible for the logic of iterating over each item and determining when the sequence has finished.\
    When you use iterators, you don’t have to reimplement that logic yourself.

\

2.  In Rust, iterators are __lazy__,\
    meaning they have no effect until you call methods that consume the iterator to use it up.

\

3.  To create an iterator:
```rust
    let v = vec![1, 2, 3];
    let v_iter = v.iter();

    for i in v_iter {
        println("{i}");
    }
```
-   In languages that don’t have iterators provided by their standard libraries,
    you would likely write this same functionality by starting a variable at index `0`,\
    using that variable to index into the vector to get a value,
    and incrementing the variable value in a loop until it reached the total number of items.

\

4.  All iterators implement a trait named `Iterator` that is defined in the standard library.
```rust
    pub trait Iterator {
        type Item;

        fn next(&mut self) -> Option<Self::Item>;

        // --snipet--
        // Some methods already with default implementation...
    }
```
-   NOTE this definition uses some new syntax, `type Item` and `Self::Item`,
    which are defining an __associated type__ with this trait.
    We’ll talk about __associated types__ in depth in later chapters!

-   For now, all you need to know is that implementing the `Iterator` trait
    requires that you also define an `Item` type, and this `Item` type is used in the return type of the `next()` method.
    In other words, the `Item` type will be the type returned from the iterator.

-   The `Iterator` trait only requires to implement the `next()` method,
    which returns one item of the iterator at a time wrapped in `Some` and, when iteration is over, returns `None`.

\

5.  We can also call the `next()` method on iterators directly.
```rust
    #[test]
    fn iterator_test() {
        let v = vec![1, 2, 3];

        // You only need to make the iterator mutable if you are manually advancing the iterator.
        // The `for in` loop automatically handles advancing the iterator.
        let mut v_iter = v.iter();

        assert_eq!(v_iter.next(), Some(&1));
        assert_eq!(v_iter.next(), Some(&2));
        assert_eq!(v_iter.next(), Some(&3));
        assert_eq!(v_iter.next(), None);
    }
```
\

6.  The `Iterator` trait has a number of different methods with default implementations provided by the standard library,
    you can find out about these methods by looking in the standard library API documentation for the `Iterator` trait.\
    Some of these methods call the `next()` method inside their default implementation,
    which is why you’re required to implement the `next()` method when implementing the `Iterator` trait.
```rust
    #[test]
    fn iter_test() {
        let v = vec![1, 2, 3];

        let v_iter = v.iter();

        let summation: i32 = v_iter.sum();
        assert_eq!(summation, 6);
    }
```
\

7.  `Iterator adaptors` are methods defined on the `Iterator` trait that do NOT consume the iterator.\
    Instead, they produce different iterators by changing some aspect of the original iterator.
```rust
    #[test]
    fn iter_adaptor_test() {
        let v: Vec<i32> = vec![1, 2, 3];
        let v: Vec<_> = v
                        .iter()
                        .map(|x| x + 1)
                        .collect();
        assert_eq!(v, vec![2, 3, 4]);
    }
```
\

8.  Many iterator adapters take closures as arguments,\
    and commonly the closures we’ll specify as arguments to iterator adapters
    will be closures that capture their environment.
```rust
    #[derive(PartialEq, Debug)]
    struct Shoe {
        size: u32,
        style: String,
    }

    fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
        shoes.into_iter().filter(|shoe| shoe.size == shoe_size).collect()
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn filter_by_size_test() {
            let shoes = vec![
                Shoe {
                    size: 41,
                    style: String::from("Sneaker"),
                },
                Shoe {
                    size: 45,
                    style: String::from("Nike"),
                },
                Shoe {
                    size: 43,
                    style: String::from("Adidas"),
                },
                Shoe {
                    size: 41,
                    style: String::from("Fila"),
                },
                Shoe {
                    size: 41,
                    style: String::from("Fila"),
                },
            ];

            let shoes_in_my_size = shoes_in_size(shoes, 45);
            assert_eq!(
                shoes_in_my_size,
                vec![
                    Shoe {
                        size: 45,
                        style: String::from("Nike"),
                    },
                ]
            );
        }
    }
```


<hr/>
### More about Cargo and Crates.io

1.  In Rust, __`release`__ profiles are predefined and customizable profiles with different configurations
    that allow a programmer to have more control over various options for compiling code.\
    Each profile is configured independently of the others.

\

2.  `Cargo` has two main profiles,
    the `dev` profile is when you run `cargo build` and the `release` profile is when you run `cargo build --release`.\
    The `dev` profile is defined with good defaults for development,
    and the `release` profile has good defaults for release builds.

\

3.  Cargo has default settings for each of the profiles that apply
    when you haven't explicitly added any `[profile.*]` sections in the project's `Cargo.toml` file.\
    By adding `[profile.*]` sections for any profile you want to customize, you override any subset of the default settings.

- __`Cargo.toml`__
```toml
        # The `opt-level` setting controls the number of optimizations
        # Rust will apply to your code, with a range of 0 to 3.
        # 3 is the best optimization and extends compiling time.
        [profile.dev]
        opt-level = 0

        [profile.release]
        opt-level = 3

        [profile.custom]
        opt-level = 1
```

<hr/>
### Publishing a Crate to Crates.io

1.  We`'`ve used packages from `crates.io` as dependencies of our project,\
    but you can also share your code with other people by publishing your own packages.

\

2.  The __crate registry__ at `crates.io` distributes the source code of your packages,\
    so it primarily hosts code that is open source.

\

3.  Accurately documenting your packages will help other users know how and when to use them,\
    Rust has a particular kind of comment for documentation `///`, known conveniently as a documentation comment,\
    that will generate HTML documentation.
```rust
        /// Adds one to the number given.
        ///
        /// # Examples
        // This is simply three backquotes `,
        // I wrote it this way beacuse markdown does not display it.
        /// \`\`\`
        /// let arg = 5;
        /// let answer = my_crate::add_one(arg);
        ///
        /// assert_eq!(6, answer);
        /// \`\`\`
        pub fn add_one(x: i32) -> i32 {
            x + 1
        }
```
-   We can generate the HTML documentation from this documentation comment by running `cargo doc`.
-   This command runs the `rustdoc` tool distributed with Rust
    and puts the generated HTML documentation in the `target/doc/` directory.

\

2.  For convenience, running `cargo doc --open` will build the HTML for your current crate’s documentation
    (as well as the documentation for all of your crate’s dependencies) and open the result in a web browser.

\

3.  We used the `# Examples` Markdown heading to create a section in the HTML with the title “Examples”
    Here are some other sections that crate authors commonly use in their documentation:

    - `Panics`
        - The scenarios in which the function being documented could panic.
        - Callers of the function who don’t want their programs to panic should make sure they don’t call the function in these situations.
    - `Errors`
        - If the function returns a `Result`, describing the kinds of errors that might occur
        and what conditions might cause those errors to be returned can be helpful.
    - `Safety`
        - If the function is `unsafe` to call (talk about it later),\
        there should be a section explaining why the function is `unsafe`
        and covering the invariants that the function expects callers to uphold.

\

4.  Adding example code blocks in your documentation comments can help demonstrate how to use your library,\
    and doing so has an additional bonus: running `cargo test` will run the code examples in your documentation as tests!

\

5.  The style of doc comment `//!` adds documentation to the item that contains the comments
    rather than to the items following the comments.\
    We typically use these doc comments inside the crate root file (`src/lib.rs` by convention)
    or inside a module to document the crate or the module as a whole.

    For example, to add documentation that describes the purpose of the __my_crate__ crate that contains the `add_one()` function,
    we add documentation comments that start with `//!` to the beginning of the `src/lib.rs` file,
```rust
    //! # My Crate
    //!
    //! `my_crate` is a collection of utilities to make performing certain
    //! calculations more convenient.

    /// Adds one to the number given.
    // --snipet--
```
\

6.  Before you can publish any crates, you need to create an account on `crates.io` and get an API token.\
    To do so, visit the home page at `crates.io` and log in via a GitHub account.\
    Once you’re logged in, visit your account settings at https://crates.io/me/ and retrieve your API key.\
    Then run the cargo login command and paste your API key when prompted, like this:
```zsh
    $ cargo login
```
-   This command will inform `Cargo` of your API token and store it locally in `~/.cargo/credentials`.
-   Note that this token is a secret, do not share it with anyone else!
-   If you do share it with anyone for any reason, you should revoke it and generate a new token on `crates.io`.

\

7.  Let’s say you have a crate you want to publish.\
    Before publishing, you’ll need to add some metadata in the `[package]` section of the crate’s `Cargo.toml` file.
>
    Your crate will need a unique name.\
    While you’re working on a crate locally, you can name a crate whatever you’d like.\
    However, crate names on `crates.io` are allocated on a first-come, first-served basis.\
    Once a crate name is taken, no one else can publish a crate with that name.
>
    Before attempting to publish a crate, search for the name you want to use.\
    If the name has been used, you will need to find another name
    and edit the name field in the `Cargo.toml` file under the `[package]` section to use the new name for publishing,
    like so:

- __`Cargo.toml`__
```toml
    [package]
    name = "my_strange_game"
    version = "0.1.0"
    edition = "2024"
    description = "A strange game."
    license = "MIT OR Apache-2.0"
```
-   ONLY with a `name`, `version`, `description` and `license` added, the `cargo publish` will work!
```zsh
    $ cargo publish
```
-   It uploads or updates your crate on `crate.io`.

\

8.  Although you can’t remove previous versions of a crate,\
    you can prevent any future projects from adding them as a new dependency.\
    This is useful when a crate version is broken for one reason or another.\
    In such situations, Cargo supports _yanking_ a crate version.

\

9.  To yank a version of a crate, in the directory of the crate that you’ve previously published,
    run `cargo yank` and specify which __version__ you want to yank.\
    For example, if we’ve published a crate named `guessing_game version 1.0.1` and we want to yank it.\
    Any future `Cargo.lock` files generated will not use the yanked version.
```zsh
    $ cargo yank --vers 1.0.1

        Updating crates.io index
            Yank guessing_game@1.0.1
```
-   By adding `--undo` to the command, you can also _undo_ a yank and allow projects to start depending on a version again:
```zsh
    $ cargo yank --vers 1.0.1 --undo

        Updating crates.io index
            Unyank guessing_game@1.0.1
```
\

10. A yank does NOT delete any code.\
    So do NOT upload your secrets there!


<hr/>
### Cargo Workspace

1.  As your project develops, you might find that the library crate continues to get bigger
    and you want to split your package further into multiple library crates.

    Cargo offers a feature called `workspaces` that can help manage multiple related packages that are developed in tandem.

\

2.  A `workspace` is a set of packages that share the same `Cargo.lock` and __output directory__.
    Let’s make a project using a workspace!

\

3.  There are multiple ways to structure a workspace, so we’ll just show one common way.\
    We’ll have a workspace containing a binary and two libraries.\
    The binary, which will provide the main functionality, will depend on the two libraries.\
    One library will provide an `add_one()` function, and a second library an `add_two()` function.\
    These three crates will be part of the same workspace.

    We’ll start by creating a new directory for the workspace:
```zsh
        $ mkdir add
        $ cd add/
```
-   Next, in the `add/` directory, we create the `Cargo.toml` file that will configure the entire workspace.
-   This file won’t have a `[package]` section.
-   Instead, it will start with a `[workspace]` section that will allow us to add members to the workspace
    by specifying the path to the package with our binary crate.

    In this case, that path is _adder_:

- __`Cargo.toml`__
```toml
        [workspace]
        members = [
            "adder",
        ]
```
-   Next, we’ll create the _adder binary crate_ by running `cargo new` within the `add/` directory:
```zsh
        $ cargo new adder
            Created binary (application) `adder` package
```
-   At this point, we can build the workspace by running `cargo build`.
-   The files in your `add/` directory should look like this:
```console
        |---------- add/
                    |---------- Cargo.lock
                    |---------- Cargo.toml
                    |---------- adder/
                                |---------- Cargo.toml
                                |---------- src/
                                            |---------- main.rs
                    |---------- target/
```
-   The workspace has one `target/` directory at the top level that the compiled artifacts will be placed into,
    the __adder__ package does NOT have its own `target/` directory.

-   Even if we were to run `cargo build` from inside the `adder/` directory,
    the compiled artifacts would still end up in `add/target/` rather than `add/adder/target/`.\
    Cargo structures the `target/` directory in a workspace like this because the crates in a workspace are meant to depend on each other.

-   Next, let’s create another member package in the workspace and call it `add_one()`.\
    Change the top-level `Cargo.toml` to specify the `add_one()` path in the __members__ list:

- __`Cargo.toml`__
```toml
        [workspace]
        members = [
            "adder",
            "add_one",
        ]
```
-   Then generate a new library crate named `add_one`:
```zsh
        $ cargo new add_one --lib
```
- __`add_one/src/lib.rs`__
```rust
        pub fn add_one(x: i32) -> i32 {
            x + 1
        }
```
-   Now we can have the __adder__ package with our binary depend on the __add_one__ package that has our library.
-   First, we’ll need to add a _path dependency_ on __add_one__ to `adder/Cargo.toml`.

- __`adder/Cargo.toml`__
```toml
        [dependencies]
        add_one = { path="../add_one" }
```
-   Cargo doesn’t assume that crates in a workspace will depend on each other,
    so we need to be explicit about the dependency relationships.

-   Next, let’s use the `add_one()` function (from the __add_one__ crate) in the __adder__ crate.
-   Open the `adder/src/main.rs` file and add a use line at the top to bring the new __add_one__ library crate into scope.
-   Then change the `main()` function to call the `add_one()` function.

- __`adder/src/main.rs`__
```rust
        use add_one;

        fn main() {
            let n = 10;
            println!("{}", add_one::add_one(n));    // -> 11
        }
```
-   Then let's build the workspace by running `cargo build` in the top-level `add/` directory!

-   To run the binary crate from the `add/` directory,
    we can specify which package in the workspace we want to run by using the `-p` argument
    and the package name with `cargo run`:
```zsh
        $ cargo run -p adder
```
-   If we add the external `rand` package to the `adder/Cargo.toml` and `add_one/Cargo.toml` files,
    and if the specified versions are compatible.
-   Cargo will resolve both of those to one version of `rand` and record that in the one `Cargo.lock`.
    Otherwise Cargo will resolve each of them, but will still try to resolve as few versions as possible.

\

4.  For another enhancement,\
    let’s add a test of the `add_one::add_one()` function within the `add_one()` crate:

- __`add_one/src/lib.rs`__
```rust
        pub fn add_one(x: i32) -> i32 {
            x + 1
        }

        #[cfg(test)]
        mod tests {
            use super::*;

            #[test]
            fn it_works() {
                assert_eq!(3, add_one(2));
            }
        }
```
-   Running `cargo test` in a workspace will run the tests for all the crates in the workspace.

-   We can also run tests for one particular crate in a workspace from the top-level directory
    by using the `-p` flag and specifying the name of the crate we want to test:
```zsh
        $ cargo test -p add_one
```
-   For additional practice, add an `add_two` crate to this workspace in a similar way as the `add_one` crate!
    Not implementing it here!


<hr/>
### Install Binaries with `cargo install`

1.  The `cargo install` command allows you to install and use binary crates locally.\
    This isn’t intended to replace system packages,
    it’s meant to be a convenient way for Rust developers to install tools that others have shared on `crates.io`.

\

2.  Note that you can only install packages that have binary targets.\
    A binary target is the runnable program that is created if the crate has a `src/main.rs` file
    or another file specified as a binary,\
    as opposed to a library target that isn’t runnable on its own but is suitable for including within other programs.\
    Usually, crates have information in the `README` file about whether a crate is a library, has a binary target, or both.

\

3.  All binaries installed with `cargo install` are stored in the installation root’s `bin/` folder.\
    If you installed Rust using `rustup.rs` and don’t have any custom configurations,
    this directory will be `$HOME/.cargo/bin`.\
    Ensure that directory is in your `$PATH` to be able to run programs you’ve installed with cargo install.

\

4.  For example, we know that there’s a Rust implementation of the `grep` tool called `ripgrep` for searching files.\
    To install `ripgrep`, we can run the following:
```zsh
        $ cargo install ripgrep

        Updating crates.io index
        Downloaded ripgrep v13.0.0
        Downloaded 1 crate (243.3 KB) in 0.88s
        Installing ripgrep v13.0.0
        --snip--
        Compiling ripgrep v13.0.0
        Finished release [optimized + debuginfo] target(s) in 3m 10s
        Installing ~/.cargo/bin/rg
        Installed package `ripgrep v13.0.0` (executable `rg`)
```
-   The second-to-last line of the output shows the location and the name of the installed binary, which in this case is `rg`.
-   As long as the installation directory is in your `$PATH`, as mentioned previously, you can then run `rg --help` and start using a faster, rustier tool for searching files!



<hr/>
### Smart Pointers

1.  We’ve already encountered a few smart pointers in this book, including `String` and `Vec<T>`.\
    Both these types count as smart pointers because they own some memory and allow you to manipulate it.\
    They also have metadata and extra capabilities or guarantees.

    `String`, for example, stores its capacity as metadata and has the extra ability to ensure its data will always be valid UTF-8.

\

2.  Smart pointers are usually implemented using structs.\
    Unlike an ordinary struct, smart pointers implement the `Deref` and `Drop` traits.\
    The `Deref` trait allows an instance of the smart pointer struct to behave like a reference
    so you can write your code to work with either references or smart pointers.\
    The `Drop` trait allows you to customize the code that’s run when an instance of the smart pointer goes out of scope.

\

3.  This chapter won’t cover every existing smart pointer.\
    Many libraries have their own smart pointers, and you can even write your own.\
    We’ll cover the most common smart pointers in the standard library:

    `Box<T>`, for allocating values on the heap.

    `Rc<T>`, a reference counting type that enables multiple ownership.

    `Ref<T>` and `RefMut<T>`, accessed thru `RefCell<T>`,\
    a type that enforces the borrowing rules at runtime instead of compile time.



## Box<T>

1.  The most straightforward smart pointer is a box, whose type is written `Box<T>`.\
    Which allows you to store data on the heap rather than the stack.\
    What remains on the stack is the pointer to the heap data.

\

2.  Boxes don’t have performance overhead, other than storing their data on the heap instead of on the stack.\
    But they don’t have many extra capabilities either.

\

3.  You’ll use them most often in these situations:

    a)  When you have a type whose size can’t be known at compile time,\
        and you want to use a value of that type in a context that requires an exact size.
    b)  When you have a large amount of data and you want to transfer ownership\
        but ensure the data won’t be copied when you do so.
    c)  When you want to own a value and you care only that it’s a type that implements a particular trait\
        rather than being of a specific type.

\

4.  Storing an __`i32`__ value on the heap using a box.
```rust
        fn main() {
            let b = Box::new(5);
            // Auto-dereferencing behavior, wouldn't be wrong if you wrote `*b` here.
            println!("{}", b);      // -> 5
        }
```
-   We define the variable ___b___ to have the value of a `Box` that points to the value `5`, which is allocated on the heap.
-   In this case, we can access the data in the box similar to how we would if this data were on the stack.
-   Just like any owned value, when a box goes out of scope, as ___b___ does at the end of `main()`, it will be deallocated.
-   The deallocation happens both for the box (stored on the stack) and the data it points to (stored on the heap).

\

5.  Enabling Recursive Types with Boxes.\
    Consider the following code which causes compiler ERROR:
```rust
        enum LinkedList {
            Data(i32, LinkedList),
        //            ---------- recursive without indirection.
            Nil,
        }

        fn main() {
            let my_linkedlist = Data(1, Data(2, Data(3, Nil)));
        }
```
-   To eliminate the compiler ERROR:
```rust
        enum LinkedList {
            Data(i32, Box<LinkedList>),
            Nil,
        }

        use crate::LinkedList::{Data, Nil};

        fn main() {
            let my_linkedlist = Data(1, Box::new(Data(2, Box::new(Data(3, Box::new(Nil))))));
        }
```

<hr/>
### The `Deref` Trait

1.  Implementing the __`Deref`__ trait allows you to customize the behavior of the dereference operator ( `*` ).\
    By implementing __`Deref`__ in such a way that a smart pointer can be treated like a regular reference,\
    you can write code that operates on references and use that code with smart pointers too.

\

2.  A regular reference is a type of pointer,\
    and one way to think of a pointer is as an arrow to a value stored somewhere else.
```rust
        fn main() {
            let x = 5;
            let y = &x;

            assert_eq!(5, x);
            assert_eq!(5, *y);
        }
```
-   The variable ___x___ holds an __`i32`__ value `5`. We set ___y___ equal to a reference to ___x___.\
-   We can assert that ___x___ is equal to `5`. However, if we want to make an assertion about the value in ___y___,\
    we have to use `*y` to follow the reference to the value it’s pointing to (hence dereference),\
    so the compiler can compare the actual value.
-   Once we dereference ___y___, we have access to the integer value ___y___ is pointing to that we can compare with `5`.

\

3.  We can rewrite the code to use a __`Box<T>`__ instead of a reference,\
    the dereference operator used on the __`Box<T>`__ is the same way as the dereference operator used on the reference.
```rust
        fn main() {
            let x = 5;
            let y = Box::new(x);

            assert_eq!(5, x);
            assert_eq!(5, *y);
        }
```
\

4.  Defining Our Own Smart Pointer.
```rust
        use std::ops::Deref;

        struct CustomBox<T>(T);

        impl<T> CustomBox<T> {
            fn new(x: T) -> CustomBox<T> {
                CustomBox(x)
            }
        }

        impl<T> Deref for CustomBox<T> {
            type Target = T;

            fn deref(&self) -> &Self::Target {
                &self.0
            }

            /* Could also be

            fn deref(&self) -> &CustomBox<T>::Target {
                &self.0
            }

            */
        }

        impl<T> DerefMut for CustomBox<T> {
            type Target = T;

            fn deref_mut(&mut self) -> &mut Self::Target {
                &mut self.0
            }
        }

        fn main() {
            let x = 5;
            let y = CustomBox::new(x);

            assert_eq!(5, x);
            assert_eq!(5, *y);

            assert_eq!(&5, y.deref());
            assert_eq!(5, *(y.deref()));

            // Actually `*y` is shorthand for `*(y.deref())`
        }
```
\

5.  __Deref coercion__ converts a reference to a type that implements the `Deref` trait into a reference to another type.\
    For example, deref coercion can convert `&String` to `&str` because `String` implements the `Deref` trait
    such that it returns `&str`.

\

6.  __Deref coercion__ is a convenience Rust performs on arguments to functions and methods,
    and works on types that implement the `Deref` trait.\
    And also works ONLY on references, not owned values!\
    It happens automatically when we pass a reference to a particular type’s value as an argument to a function or method
    that doesn’t match the parameter type in the function or method definition.
>
    A sequence of calls to the `deref()` method converts the type we provided into the type the parameter needs.
>
    __Deref coercion__ was added to Rust so that programmers writing function and method calls
    don’t need to add as many explicit references and dereferences with `&` and `*`.
```rust
        fn main() {
            let name = CustomBox::new(String::from("Rust"));
            hello(&name);
            // OR
            // hello(name.deref());
            // OR
            // hello(&(*name)[..]);
        }

        fn hello(name: &str) {
            println!("Hello, {name}!");
        }
```
-   Here we’re calling the `hello()` function with the argument `&name`, which is a reference to a `CustomBox<String>` value.
-   Because we implemented the `Deref` trait on `CustomBox<T>`, Rust can turn `&CustomBox<String>` into `&String` by calling `deref()`.
-   The standard library provides an implementation of `Deref` on `String` that returns a string slice,
    and this is in the API documentation for `Deref`.
-   Rust calls `deref()` again to turn the `&String` into `&str`, which matches the `hello()` function’s definition.

\

7.  Similar to how you use the `Deref` trait to override the `*` operator on __immutable references__,\
    you can use the `DerefMut` trait to override the `*` operator on __mutable references__.

    Rust does __deref coercion__ when it finds types and trait implementations in three cases:

    From __`&T`__     to __`&U`__     when __`T: Deref<Target=U>`__

    From __`&mut T`__ to __`&mut U`__ when __`T: DerefMut<Target=U>`__

    From __`&mut T`__ to __`&U`__     when __`T: Deref<Target=U>`__

\

8.  In Rust, when you have a borrowed __`struct`__ (either immutable or mutable),\
    you can typically access its fields directly without needing to dereference it with ( `*` ).\
    Rust automatically dereferences references when accessing fields, thanks to a feature called __auto-dereferencing__.
```rust
    struct A {
        value: String,
    }

    fn print_A(a: &A) {
        println!("{}", a.value);
        println!("{}", (*a).value);
    }

    fn modify_A(a: &mut A) {
        a.value = String::from("aaaA");
        // (*a).value = String::from("aaaA");
    }

    fn main() {
        let a = A {
            value: String::from("Aaaa"),
        };
        print_A(&a);
        modify_A(&a);
    }
```



<hr/>
### The `Drop` Trait

1.  The second trait important to the smart pointer pattern is `Drop`,\
    which lets you customize what happens when a value is about to go out of scope.\
    You can provide an implementation for the `Drop` trait on any type.
```rust
        struct CustomPointer {
            data: String,
        }

        impl Drop for CustomPointer {
            fn drop(&mut self) {
                println!("Dropping custom pointer with data: {}!", self.data);
            }
        }

        fn main() {
            let p1 = CustomPointer {
                data: String::from("A"),
            };
            let p2 = CustomPointer {
                data: String::from("B"),
            };
        }

        // -> B
        // -> A
```
\

2.  Explicitly calling `.drop()` is not possible.\
    So try dropping a value early with `std::mem::drop()`.
```rust
        use std::mem::drop;

        fn main() {
            let p1 = CustomPointer {
                data: String::from("A"),
            };
            let p2 = CustomPointer {
                data: String::from("B"),
            };

            drop(p1);
            drop(p2);
        }

        // -> A
        // -> B
```
\

3.  You cannot call `std::mem::drop()` on types that implement the `Copy` trait
    because types that implement `Copy` are implicitly copied rather than moved.


<hr/>
### `Rc<T>`, the Reference Counted Smart Pointer.

1.  You could enable multiple ownership explicitly by using the Rust type `Rc<T>`, __reference counting__.\
    The `Rc<T>` type keeps track of the number of references to a value to determine whether or not the value is still in use.
    If there are zero references to a value, the value can be cleaned up without any references becoming invalid.

\

2.  Note that `Rc<T>` is only for use in __single-threaded__ scenarios.\
    When we discuss concurrency in later chapters, we’ll cover how to do reference counting in multithreaded programs.
```rust
        enum LinkedList {
            Data(i32, Rc<LinkedList>),
            Nil,
        }

        use crate::LinkedList::{Data, Nil};
        use std::rc::Rc;

        fn main() {
            let a = Rc::new(Data(1, Rc::new(Data(2, Rc::new(Nil)))));
            let b = Data(3, Rc::clone(&a));
            let c = Data(4, Rc::clone(&a));

            {
                let d = Data(5, Rc::clone(&a));
                println!("{}", Rc::strong_count(&a));   // -> 4
            }

            println!("{}", Rc::strong_count(&a));       // -> 3
        }
```
-   We could have called `a.clone()` rather than `Rc::clone(&a)`, but Rust’s convention is to use `Rc::clone()` in this case.
-   The implementation of `Rc::clone()` doesn’t make a deep copy of all the data like most types’ implementations of `clone()` do.
-   The call to `Rc::clone()` only increments the reference count, which doesn’t take much time.
-   Deep copies of data can take a lot of time. By using `Rc::clone()` for reference counting,
    we can visually distinguish between the deep-copy kinds of clones and the kinds of clones that increase the reference count.
-   When looking for performance problems in the code, we only need to consider the deep-copy clones and can disregard calls to `Rc::clone()`.


<hr/>
### `RefCell<T>` and the Interior Mutability Pattern

1.  __Interior mutability__ is a design pattern in Rust that allows you to mutate data
    even when there are immutable references to that data.

    Normally, this action is disallowed by the borrowing rules.\
    To mutate data, the pattern uses `unsafe` code inside a data structure to bend Rust’s usual rules that govern mutation and borrowing.
    Unsafe code indicates to the compiler that we’re checking the rules manually instead of relying on the compiler to check them for us.

\

2.  We can use types that use the __interior mutability__ pattern
    only when we can ensure that the borrowing rules will be followed at runtime, even though the compiler can’t guarantee that.
    The `unsafe` code involved is then wrapped in a safe API, and the outer type is still immutable.

\

3.  Unlike `Rc<T>`, the `RefCell<T>` type represents single ownership over the data it holds.\
    Remember that at any given time, you can have either (but not both) one mutable reference or any number of immutable references.
    References must always be valid.\
    With references and `Box<T>`, the borrowing rules’ invariants are enforced at __compile time__.\
    With `RefCell<T>`, these invariants are enforced at __runtime__.\
    With references, if you break these rules, you’ll get a __compiler error__.\
    With `RefCell<T>`, if you break these rules, your program will __panic and exit__.

\

4.  Because some analysis is impossible, if the Rust compiler can’t be sure the code complies with the ownership rules,
    it might reject a correct program, in this way, it’s conservative.\
    If Rust accepted an incorrect program, users wouldn’t be able to trust in the guarantees Rust makes.\
    However, if Rust rejects a correct program, the programmer will be inconvenienced, but nothing catastrophic can occur.
    The `RefCell<T>` type is useful when you’re sure your code follows the borrowing rules
    but the compiler is unable to understand and guarantee that.

\

5.  Similar to `Rc<T>`, `RefCell<T>` is only for use in __single-threaded__ scenarios
    and will give you a compile-time error if you try using it in a multithreaded context.

\

6.  Here is a recap of the reasons to choose `Box<T>`, `Rc<T>`, or `RefCell<T>`:

    `Rc<T>` enables _multiple owners_ of the same data.
    `Box<T>` and `RefCell<T>` have _single owners_.

    `Box<T>` allows _immutable_ or _mutable borrows_ checked at __compile time__.
    `Rc<T>` allows only _immutable borrows_ checked at __compile time__.
    `RefCell<T>` allows _immutable_ or _mutable borrows_ checked at __runtime__.

    Because `RefCell<T>` allows _mutable borrows_ checked at __runtime__,
    you can mutate the value inside the `RefCell<T>` even when the `RefCell<T>` is _immutable_.

\

7.  A Use Case for Interior Mutability: __Mock Objects__.\
    Sometimes during testing a programmer will use a type in place of another type,\
    in order to observe particular behavior and assert it’s implemented correctly. This placeholder type is called a __test double__.
    __Mock objects__ are specific types of test doubles that record what happens during a test
    so you can assert that the correct actions took place.

    Example for this test doubles mock object:
```rust
    // Define a trait for the database connection behavior
    trait Database {
        fn query(&self, query: &str) -> Result<String, &str>;
    }

    // Real implementation of DatabaseConnection
    // In a real-world scenario, this would actually connect to a database.
    struct DatabaseConnection;

    impl Database for DatabaseConnection {
        fn query(&self, query: &str) -> Result<String, &str> {
            // Here you'd normally connect to the actual database
            // For the sake of the example, this is just a placeholder
            Ok("Real data from database".to_string())
        }
    }

    // (Test double) Mock implementation of DatabaseConnection
    // This is used for testing, returning mock data instead of real database interaction.
    struct MockDatabaseConnection;

    impl Database for MockDatabaseConnection {
        fn query(&self, query: &str) -> Result<String, &str> {
            if query == "SELECT * FROM users;" {
                Ok("Mock data".to_string()) // Return mock data
            } else {
                Err("Query failed") // Simulate a failure for other queries
            }
        }
    }

    fn main() {
        // Using the real database connection
        let real_db = DatabaseConnection;
        let result = real_db.query("SELECT * FROM users;");
        println!("Real DB result: {:?}", result);

        // Using the mock database connection for testing
        let mock_db = MockDatabaseConnection;
        let mock_result = mock_db.query("SELECT * FROM users;");
        println!("Mock DB result: {:?}", mock_result);
    }
```
\

8.  Now consider the following scenario.

> - src/lib.rs
```rust
    pub trait Messenger {
        fn send(&self, msg: &str);
    }

    pub struct LimitTracker<'a, T: Messenger> {
        messenger: &'a T,
        value: usize,
        max: usize,
    }

    impl<'a, T> LimitTracker<'a, T>
    where
        T: Messenger,
    {
        pub fn new(messenger: &'a T, max: usize) -> LimitTracker<'a, T> {
            LimitTracker {
                messenger,
                value: 0,
                max,
            }
        }

        pub fn set_value(&mut self, value: usize) {
            self.value = value;

            let percentage_of_max = self.value as f64 / self.max as f64;

            if percentage_of_max >= 1.0 {
                self
                    .messenger
                    .send("Error: You are over your quota!");
            } else if percentage_of_max >= 0.9 {
                self
                    .messenger
                    .send("Urgent warning: You've used up over 90% of your quota!");
            } else if percentage_of_max >= 0.75 {
                self
                    .messenger
                    .send("Warning: You've used up over 75% of your quota!");
            } else {
                self
                    .messenger
                    .send("Info: You've used less than 75% of your quota!");
            }
        }
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        struct MockMessenger {
            messages: Vec<String>,
        }

        impl MockMessenger {
            fn new() -> MockMessenger {
                Self {
                    messages: vec![],
                }
            }
        }

        impl Messenger for MockMessenger {
            fn send(&self, message: &str) {
                self.messages.push(String::from(message));
            }
        }

        #[test]
        fn over_75_percent_warning_test() {
            let mock_messenger = MockMessenger::new();

            let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);

            limit_tracker.set_value(80);

            assert_eq!(mock_messenger.messages.len(), 1);
        }
    }
```
-   However, there’s one problem with this test, as shown here:
```rust
    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
            self.messages.push(String::from(message));
    //      ^^^^^^^^^^^^^^^^^^ `self` is a `&` reference, so the data it refers to cannot be borrowed as mutable.
        }
    }
```
-   Clearly we can solve the problem via `fn send(&mut self, msg: &str);` changing its signature, but here it won't work.
-   Because then the signature of `send()` wouldn’t match the signature in the `Messenger` trait definition.
>
-   This is a situation in which interior mutability can help!
```rust
    #[cfg(test)]
    mod tests {
        use super::*;
        use std::cell::RefCell;

        struct MockMessenger {
            messages: RefCell<Vec<String>>,
        }

        impl MockMessenger {
            fn new() -> MockMessenger {
                Self {
                    messages: RefCell::new(vec![]),
                }
            }
        }

        impl Messenger for MockMessenger {
            fn send(&self, message: &str) {
                self.messages.borrow_mut().push(String::from(message));
                // EXPLICITY DEREF
                // *(self.messages.borrow_mut()).push(String::from(message));
            }
        }

        #[test]
        fn over_75_percent_warning_test() {
            let mock_messenger = MockMessenger::new();

            let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);

            limit_tracker.set_value(80);

            assert_eq!(mock_messenger.messages.borrow().len(), 1);
        }
    }
```
\

9.  When creating immutable and mutable references, we use the `&` and `&mut` syntax, respectively.\
    With `RefCell<T>`, we use the `borrow()` and `borrow_mut()` methods, which are part of the safe API that belongs to it.\
    The `borrow()` method returns the smart pointer type `Ref<T>`, and `borrow_mut()` returns the smart pointer type `RefMut<T>`.\
    Both types implement `Deref`, so we can treat them like regular references.

    The `RefCell<T>` keeps track of how many `Ref<T>` and `RefMut<T>` smart pointers are currently active.\
    Every time we call `borrow()`, the `RefCell<T>` increases its count of how many immutable borrows are active.\
    When a `Ref<T>` value goes out of scope, the count of immutable borrows goes down by one.\
    Just like the compile-time borrowing rules, `RefCell<T>` lets us have __many__ immutable borrows or __one__ mutable borrow at any point in time.

    Like this program will panic at runtime:
```rust
    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
            let mut one_borrow = self.sent_messages.borrow_mut();
            let mut two_borrow = self.sent_messages.borrow_mut();

            one_borrow.push(String::from(message));
            two_borrow.push(String::from(message));
        }
    }
```
-   Because it has two mutable borrows at the same time, namely `one_borrow` and `two_borrow`!

\

10. Having Multiple Owners of Mutable Data by Combining `Rc<T>` and `RefCell<T>`.
```rust
    #[derive(Debug)]
    enum LinkedList {
        Data(Rc<RefCell<i32>>, Rc<LinkedList>),
        Nil,
    }

    use crate::LinkedList::{Data, Nil};
    use std::cell::RefCell;
    use std::rc::Rc;

    fn main() {
        let value = Rc::new(RefCell::new(5));

        let a = Rc::new( Data(Rc::clone(&value), Rc::new(Nil)) );

        let b = Data(Rc::new(RefCell::new(1)), Rc::clone(&a));

        let c = Data(Rc::new(RefCell::new(2)), Rc::clone(&a));

        // DO NOT USE value.borrow_mut() += 10;
        // here value.borrow_mut() returns a RefMut<i32>,
        // RefMut is a smart pointer,
        // Rust’s auto-dereferencing behavior kicks in when you’re calling ``methods`` on types wrapped in smart pointers,
        // but operations like `+=` has to be derefed to call.
        *value.borrow_mut() += 10;

        println!("{a:?}");  // -> Data(RefCell { value: 15 }, Nil)
        println!("{b:?}");  // -> Data(RefCell { value: 1 }, Data(RefCell { value: 15 }, Nil))
        println!("{c:?}");  // -> Data(RefCell { value: 2 }, Data(RefCell { value: 15 }, Nil))
    }
```

<hr/>
### Reference Cycles Can Leak Memory

1.  Rust’s memory safety guarantees make it difficult, but not impossible,
    to accidentally create memory that is never cleaned up (known as a memory leak).\
    Preventing memory leaks entirely is not one of Rust’s guarantees, meaning memory leaks are memory safe in Rust.

\

2.  We can see that Rust allows memory leaks by using `Rc<T>` and `RefCell<T>`,\
    it’s possible to create references where items refer to each other in a cycle.\
    This creates memory leaks because the reference count of each item in the cycle will never reach `0`, and the values will never be dropped.
```rust
    use crate::LinkedList::{Data, Nil};
    use std::cell::RefCell;
    use std::rc::Rc;

    #[derive(Debug)]
    enum LinkedList {
        Data(i32, RefCell<Rc<LinkedList>>),
        Nil,
    }

    impl LinkedList {
        fn tail(&self) -> Option<&RefCell<Rc<List>>> {
            match self {
                Data(_, item) => Some(item),
                Nil => None,
            }
        }
    }

    // In case if you wonder why Option above has to be a reference inside type.
    // This struct could explain a lot!
    struct A {
        value: String,
    }

    impl A {
        fn tail(&self) -> &String {
            &self.value
        }

        fn tail2(&self) -> &String {
            match self {
                // ALSO WORKS
                // A { value } => value,
                A { value, .. } => value,
            }
        }
    }

    fn main() {
        let a = Rc::new(Data(10, RefCell::new(Rc::new(Nil))));
        println!("{}", Rc::strong_count(&a));   // -> 1
        println!("{:?}", a.tail());;            // -> Some(RefCell { value: Nil })

        let b = Rc::new(Data(20, RefCell::new(Rc::clone(&a))));
        println!("{}", Rc::strong_count(&a));   // -> 2
        println!("{}", Rc::strong_count(&b));   // -> 1
        println!("{:?}", b.tail());;            // -> Some(RefCell { value: Data(10, RefCell: { value: Nil }) })

        if let Some(item) = a.tail() {
            *item.borrow_mut() = Rc::clone(&b);
        }

        println!("{}", Rc::strong_count(&a));   // -> 2
        println!("{}", Rc::strong_count(&b));   // -> 2

        // Uncomment the next line to see that we have a cycle, it will overflow the stack.
        // println!("{:?}", a.tail());
    }
```


<hr/>
### Preventing Reference Cycles: Turning an `Rc<T>` into a `Weak<T>`.

1.  So far, we’ve demonstrated that calling `Rc::clone()` increases the `strong_count()` of an `Rc<T>` instance,\
    and an `Rc<T>` instance is only cleaned up if its `strong_count()` is `0`.

\

2.  However, You can also create a __weak reference__ to the value within an `Rc<T>` instance by calling `Rc::downgrade()`
    and passing a reference to the `Rc<T>`.\
    __Strong references__ are how you can share ownership of an `Rc<T>` instance.\
    __Weak references__ don’t express an ownership relationship, and their count doesn’t affect when an `Rc<T>` instance is cleaned up.
    They won’t cause a reference cycle because any cycle involving some weak references will be broken
    once the strong reference count of values involved is `0`.

\

3.  When you call `Rc::downgrade()`, you get a smart pointer of type `Weak<T>`.\
    Instead of increasing the `strong_count()` in the `Rc<T>` instance by `1`, calling `Rc::downgrade()` increases the `weak_count()` by `1`.
    The `Rc<T>` type uses `weak_count()` to keep track of how many `Weak<T>` references exist.\
    The difference is the `weak_count()` doesn’t need to be `0` for the `Rc<T>` instance to be cleaned up.

\

4.  Because the value that `Weak<T>` references might have been dropped,
    so before doing anything with the value that a `Weak<T>` is pointing to, you must make sure the value still exists.\
    Do this by calling the `upgrade()` method on a `Weak<T>()` instance, which will return an `Option<Rc<T>>`.\
    You’ll get a result of `Some` if the `Rc<T>` value has not been dropped yet and a result of `None` if it has been dropped.\
    Because `upgrade()` returns an `Option<Rc<T>>`,
    Rust will ensure that the `Some` case and the `None` case are handled, and there won’t be an invalid pointer.
```rust
    use std::cell::RefCell;
    use std::rc::{Rc, Weak};

    #[derive(Debug)]
    struct Node {
        value: i32,
        parent: RefCell<Weak<Node>>,
        children: RefCell<Vec<Rc<Node>>>,
    }

    fn main() {
        let leaf = Rc::new(Node {
            value: 10,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![]),
        });

        println!("{:?}", leaf.parent.borrow().upgrade());   // -> None
        println!("{:?}", leaf);                             // ->
        // Node {
        //      value: 10,
        //      parent: RefCell {
        //          value: (Weak),
        //      },
        //      children: RefCell {
        //          value: [],
        //      },
        //  }

        let branch = Rc::new(Node {
            value: 5,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![Rc::clone(&leaf)]),
        });

        *leaf.parent.borrow_mut() = Rc::downgrade(&branch);
        println!("{:?}", leaf.parent.borrow().upgrade());   // ->
        // Some(
        //  Node {
        //      value: 5,
        //      parent: RefCell { value: (Weak) },
        //      children: RefCell {
        //          value: [
        //              Node {
        //                  value: 10,
        //                  parent: RefCell { value: (Weak) },
        //                  children: RefCell { value: [] } }
        //          ]                                               -- value
        //      }                                                   -- children: RefCell
        //  }                                                       -- Node
        // )                                                        -- Some

        println!("{:?}", leaf);                             // ->
        // Node {
        //  value: 10,
        //  parent: RefCell {
        //      value: (Weak),
        //  },
        //  children: RefCell {
        //      value: [],
        //  },
    }
```
\

5.  Visualizing Changes to `strong_count()` and `weak_count()`.
```rust
    fn main() {
        let leaf = Rc::new(Node {
            value: 3,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![]),
        });

        println!(
            "leaf strong = {}, weak = {}",          // -> leaf strong = 1, weak = 0
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf),
        );

        {
            let branch = Rc::new(Node {
                value: 5,
                parent: RefCell::new(Weak::new()),
                children: RefCell::new(vec![Rc::clone(&leaf)]),
            });

            *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

            println!(
                "branch strong = {}, weak = {}",    // -> branch strong = 1, weak = 1
                Rc::strong_count(&branch),
                Rc::weak_count(&branch),
            );

            println!(
                "leaf strong = {}, weak = {}",      // -> leaf strong = 2, weak = 0
                Rc::strong_count(&leaf),
                Rc::weak_count(&leaf),
            );
        }

        println!("leaf parent = {:?}", leaf.parent.borrow().upgrade()); // -> leaf parent = None
        println!(
            "leaf strong = {}, weak = {}",          // -> leaf strong = 1, weak = 0
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf),
        );
    }
```

<hr/>
### Fearless Concurrency

1.  Programming languages implement threads in a few different ways,\
    and many operating systems provide an API the language can call for creating new threads.

\

2.  The Rust standard library uses a 1:1 model of thread implementation,\
    whereby a program uses one operating system thread per one language thread.\
    There are crates that implement other models of threading that make different tradeoffs to the 1:1 model.

\

3.  To create a new thread, we call the `thread::spawn()` function\
    and pass it a closure containing the code we want to run in the new thread.
```rust
    use std::thread;
    use std::time::Duration;

    fn main() {
        thread::spawn(|| {
            for i in 1..=10 {
                println!("From thread: {}", i);
                thread::sleep(Duration::from_millis(1000));
            }
        });

        for i in 1..=5 {
            println!("From main: {}", i);
            thread::sleep(Duration::from_millis(1000));
        }
    }

    /* Possible output:
        From main: 1
        From thread: 1
        From thread: 2
        From thread: 3
        From thread: 4
        From main: 2
        From thread: 5
        From main: 3
        From thread: 6
        From thread: 7
        From main: 4
        From thread: 8
        From main: 5
    */
    //
    // From thread: 9
    // From thread: 10
    //
    // are not printed because the main thread has been shutdown.
```
\

4.  The return type of `thread::spawn()` is `JoinHandle`.\
    A `JoinHandle` is an owned value that, when we call the `join()` method on it, will wait for its thread to finish.
```rust
    std::thread;
    use std::time::Duration;

    fn main() {
        let handle = thread::spawn(|| {
            for i in 1..=10 {
                println!("From thread: {}", i);
                thread::sleep(Duration::from_millis(1000));
            }
        });

        for i in 1..=5 {
            println!("From main: {}", i);
            thread::sleep(Duration::from_millis(1000));
        }

        // .unwrap() is used with Result<T, E> or Option<T>,
        // if the result is valid, it unwraps it, otherwise it directly panics the program.
        // So only use it when you are completely sure the program returns a valid result.
        handle.join().unwrap();
    }
```
\

5.  We’ll often use `move` with closures passed to `thread::spawn()`
    because the closure will then take ownership of the values it uses from the environment,\
    thus transferring ownership of those values from one thread to another.
>
    Consider the following example which will not compile:
```rust
    use std::thread;

    fn main() {
        let v = vec![1, 2, 3];

        let handle = thread::spawn(|| {
            println!("Here's the vector: {:?}", v);
        });

        drop(v); // Oh NO!

        handle.join().unwrap();
    }
```
    Thus try to use `move` to transfer the ownership:
```rust
    use std::thread;

    fn main() {
        let v = vec![1, 2, 3];

        let handle = thread::spawn(move || {
            println!("Here's the vector: {:?}", v);
        });

        handle.join.unwrap();
    }
```
-   The `move` keyword overrides Rust’s conservative default of borrowing,\
    it doesn’t let us violate the ownership rules!

\

6.  One increasingly popular approach to ensuring safe concurrency is __message passing__,\
    where threads or actors communicate by sending each other messages containing data.

\

7.  To accomplish message-sending concurrency, Rust’s standard library provides an implementation of __channels__.\
    A __channel__ is a general programming concept by which data is sent from one thread to another.

    You can imagine a __channel__ in programming as being like a directional channel of water, such as a stream or a river.\
    If you put something like a rubber duck into a river, it will travel downstream to the end of the waterway.

\

8.  A channel has two halves: a __transmitter__ and a __receiver__.\
    The __transmitter__ half is the upstream location where you put rubber ducks into the river,
    and the __receiver__ half is where the rubber duck ends up downstream.\
    One part of your code calls methods on the __transmitter__ with the _data you want to send_,
    and another part checks the __receiver__ end for _arriving messages_.\
    A channel is said to be __closed__ if _either_ the transmitter or receiver half is dropped.

\

9.  Here, we’ll work up to a program that has one thread to generate values and send them down a channel,\
    and another thread that will receive the values and print them out.
```rust
    // multiple producer, single consumer (mpsc)
    use std::sync::mpsc;
    use std::thread;

    fn main() {
        let (tx, rx) = mpsc::channel();

        thread::spawn(move || {
            let hi = String::from("Hi");
            tx
                .send(hi)           // :Result<T, E>
                .unwrap();
        });

        let received = rx
                        .recv()     // :Result<T, E>
                        .unwrap();
        println!("Got: {}", received);
    }
```
-   Rust’s standard library channel means a channel can have `multiple sending` ends that produce values
    but only `one receiving` end that consumes those values.
>
-   The `mpsc::channel()` function returns a tuple,
    the first element of which is the sending end -- the transmitter,
    and the second element is the receiving end -- the receiver.
>
-   The transmitter has a `send()` method that takes the value we want to send.
>
-   The receiver has two useful methods: `recv()` and `try_recv()`.
    - `recv()`
        - Will block the calling thread’s execution and wait until a value is sent down the channel.
        - When the transmitter closes, `recv()` will return an `Err(E)` to signal that no more values will be coming.
    - `try_recv()`
        - Does NOT block, but will instead return a `Result<T, E>` immediately.
        - An `Ok(T)` holding a message if one is available or an `Err(E)` if there aren’t any messages this time.

\

10. Sending Multiple Values and Seeing the Receiver Waiting.
```rust
    use std::sync::mpsc;
    use std::thread;
    use std::time::Duration;

    fn main() {
        let (tx, rx) = mpsc::channel();

        thread::spawn(move || {
            let v = vec![
                String::from("A"),
                String::from("B"),
                String::from("C"),
                String::from("D"),
            ];

            for s in v {
                tx.send(s).unwrap();
                thread::sleep(Duration::from_secs(1));
            }
        });

        for received in tx {
            println!("Got: {received}");
        }
    }
```
\

11. Creating Multiple Producers by Cloning the Transmitter.
```rust
    use std::sync::mpsc;
    use std::thread;
    use std::time::Duration;

    fn main() {
        let (tx, rx) = mpsc::channel();

        let tx1 = tx.clone();
        thread::spawn(move || {
            let v = vec![
                String::from("A"),
                String::from("B"),
                String::from("C"),
                String::from("D"),
            ];

            for s in v {
                tx1.send(s).unwrap();
                thread::sleep(Duration::from_secs(1));
            }
        });

        let tx2 = tx.clone();
        thread::spawn(move || {
            let v = vec![
                String::from("X"),
                String::from("Y"),
                String::from("Z"),
            ];

            for s in v {
                tx2.send(s).unwrap();
                thread::sleep(Duration::from_secs(1));
            }
        });

        for received in tx {
            println!("Got: {received}");
        }
    }
```


<hr/>
### Shared-State Concurrency

1.  __Message passing__ is a fine way of handling concurrency, but it’s not the only one.\
    Consider this part of the slogan from the ___Go___ language documentation:
    "Do not communicate by sharing memory; instead, share memory by communicating"

    What would communicating by sharing memory look like?\
    In addition, why would message-passing enthusiasts caution not to use memory sharing?

\

2.  In a way, channels in any programming language are similar to single ownership,\
    because once you transfer a value down a channel, you should no longer use that value.\
    Shared memory concurrency is like multiple ownership,\
    multiple threads can access the same memory location at the same time.\
    As you saw in before, where smart pointers made multiple ownership possible,\
    multiple ownership can add complexity because these different owners need managing.\
    Rust’s type system and ownership rules greatly assist in getting this management correct.

\

3.  For an example, let’s look at mutexes, one of the more common concurrency primitives for shared memory.\
    `Mutex` is an abbreviation for __mutual exclusion__,\
    as in, a `mutex` allows only ONE thread to access some data at any given time.\
    To access the data in a `mutex`, a thread must first signal that it wants access by asking to acquire the mutex’s `lock`.
    Therefore, the `mutex` is described as `guarding` the data it holds via the locking system.
```rust
    use std::sync::Mutex;

    fn main() {
        let m = Mutex::new(10);

        {
            // This call will block the calling thread until it’s our turn to have the lock.
            //
            // The call to lock would fail if another thread holding the lock panicked.
            // In that case, no one would ever be able to get the lock,
            // so we’ve chosen to unwrap and have this thread panic if we’re in that situation.
            let mut n = m.lock().unwrap();
            *n = 20;
        }

        println!(":?", m);  // ->
                            // Mutex {
                            //  data: 20,
                            //  poisoned: false,
                            //  ..
                            // }
    }
```
-   The type of ___m___ is `Mutex<i32>`, and the call to `lock()` returns a smart pointer called `MutexGuard<T>`,\
    wrapped in a `LockResult<T, E>` that we handled with the call to `unwrap()`.\
    The `MutexGuard` is a smart pointer implements `Deref` and `Drop`.

\

4.  Sharing a `Mutex<T>` Between Multiple Threads.\
    Consider the following example, which has a compiler ERROR:
```rust
    use std::sync::Mutex;
    use std::thread;

    fn main() {
        let counter = Mutex::new(0);
        let mut handles = vec![];

        for _ in 0..10 {
            let counter = Rc::clone(&counter);
            let handle = threda::spawn(move || {
    //                                 ^^^^ required by a bound in `spawn`.
                let mut n = counter.lock().unwrap();
                *n += 1;
            });
    //      ^ `Rc<Mutex<i32>>` cannot be sent between threads safely.
    //        The trait `Send` is not implemented for `Rc<Mutex<i32>>`.
            handles.push(hanle);
        }

        for handle in handles {
            handle.join().unwrap();
        }

        println!("The result: {}", *counter.lock().unwrap());
    }
```
-   Wow, that error message is very wordy!
-   Here’s the important part to focus on: `Rc<Mutex<i32>>` cannot be sent between threads safely.
-   The compiler is also telling us the reason why: the trait `Send` is not implemented for `Rc<Mutex<i32>>` .
-   We’ll talk about `Send` in the next section, it’s one of the traits that ensures the types
    we use with threads are meant for use in concurrent situations.
>
-   Unfortunately, `Rc<T>` is not safe to share across threads.
-   When `Rc<T>` manages the reference count, it adds to the count for each call to `clone()`
    and subtracts from the count when each clone is dropped.
-   But it doesn’t use any concurrency primitives to make sure that changes to the count can’t be interrupted by other thread.
-   What we need is a type exactly like `Rc<T>` but one that makes changes to the reference count in a __thread-safe__ way.

\

5.  Fortunately, Arc<T> is a type like Rc<T> that is safe to use in concurrent situations. The 'A' stands for Atomic.\
    Atomics are an additional kind of concurrency primitive that we won’t cover in detail here,
    see the standard library documentation `std::sync::atomic` for more details.

\

6.  You might then wonder why all primitive types aren’t atomic
    and why standard library types aren’t implemented to use `Arc<T>` by default.\
    The reason is that thread safety comes with a performance penalty that you only want to pay when you really need to.\
    If you’re just performing operations on values within a single thread,
    your code can run faster if it doesn’t have to enforce the guarantees atomics provide.

\

7.  Let’s return to our example: `Arc<T>` and `Rc<T>` have the same API.
```rust
    use std::sync::{Arc, Mutex};
    use std::thread;

    fn main() {
        let counter = Arc::new(Mutex::new(0));
        let mut handles = vec![];

        for _ in 0..10 {
            let counter = Arc::clone(&counter);
            let handle = thread::spawn(move || {
                let mut n = counter.lock().unwrap();
                *n += 1;
            });
            handles.push(handle);
        }

        for handle in handles {
            handle.join().unwrap();
        }

        println!("Result: {}", *counter.lock().unwrap()); // -> Result: 10
        println!("Result: {}", counter.lock().unwrap());
        /* ->
         * Result: MutexGuard { lock: 1 }
         * */
    }
```

\

8.  When spawning a new thread,\
    Rust enforces that the closure captures the values it uses by moving them into the thread’s scope.\
    So even a reference usage would be an ownership capture in concurrency context.


<hr/>
### Extensible Concurrency with the Sync and Send Traits

1.  Interestingly, the Rust language has very few concurrency features.\
    Almost every concurrency feature we’ve talked about so far in this chapter has been part of the standard library, not the language.
    Your options for handling concurrency are not limited to the language or the standard library,\
    you can write your own concurrency features or use those written by others.

\

2.  Also, two concurrency concepts are embedded in the language: the `std::marker` traits `Sync` and `Send`.

\

3.  The `std::marker::Send` trait indicates that ownership of values of the type implementing `Send` can be transferred between threads.
    Almost every Rust type is `Send`, but there are some exceptions, including `Rc<T>`,\
    it cannot be `Send` because if you cloned an `Rc<T>` value and tried to transfer ownership of the clone to another thread,
    both threads might update the reference count at the same time.\
    Therefore, Rust’s type system and trait bounds ensure that you can never accidentally send an `Rc<T>` value across threads unsafely.

    Any type composed entirely of `Send` types is automatically marked as `Send` as well.\
    Almost all primitive types are `Send`, aside from raw pointers, which we’ll discuss in later chapters.
```rust
    struct MyStruct {
        x: i32,  // `i32` is `Send`
        y: f64,  // `f64` is `Send`
    }
    // `MyStruct` is automatically `Send` because both `i32` and `f64` are `Send`.
```
\

4.  The `std::marker::Sync` trait indicates that it is safe for the type implementing `Sync` to be referenced from multiple threads.
    In other words, any type `T` which is `Sync` if `&T` (an immutable reference to `T`) is `Send`,\
    meaning the reference can be sent safely to another thread.\
    Similar to `Send`, primitive types are `Sync`, and types composed entirely of types that are `Sync` are also `Sync`.

    If a type `T` is `Sync`, then `&T` (an immutable reference) can be safely shared between threads.\
    Mot directly applicable to mutable references `&mut T`. You'll use `Arc<Mutex<T>>` for mutating then.\
    But the `T` inside `Arc<Mutex<T>>` does not need to implement `Sync`
    because the `Mutex` itself ensures that only one thread can access the data at a time.\
    But the the `T` inside `Arc<T>` must implement `Sync` in order to be shared across threads.

    The smart pointer `Rc<T>` is also not `Sync` for the same reasons that it’s not `Send`.\
    The `RefCell<T>` type and the family of related `Cell<T>` types are not `Sync`.\
    The implementation of borrow checking that `RefCell<T>` does at runtime is not __thread-safe__.\
    The smart pointer `Mutex<T>` is `Sync` and can be used to share access with multiple threads.

\

5.  Because types that are made up of `Send` and `Sync` traits are automatically also `Send` and `Sync`,
    we don’t have to implement those traits manually.\
    As marker traits, they don’t even have any methods to implement.\
    They’re just useful for enforcing invariants related to concurrency.

    Manually implementing these traits involves implementing `unsafe` Rust code.\
    We’ll talk about using unsafe Rust code in later chapters!\
    For now, the important information is that building new concurrent types not made up of `Send` and `Sync` parts
    requires careful thought to uphold the safety guarantees.

\

6.  Example of `Send` trait.
```rust
    fn send_example() {
        // `i32` is `Send`
        let x = 42;
        std::thread::spawn(move || {
            println!("x in another thread: {}", x);
        }).join().unwrap();
    }
```
-   But here actually ___x___ will be copied rather than moved, because it implements the `Copy` trait also.

\

7.  Example of `Sync` trait.
```rust
    fn main() {
        // `i32` is both `Send` and `Sync`
        let num: i32 = 42;
        // Create an immutable reference to `num`
        let num_ref = &num;

        // Create a vector to hold thread handles
        let mut handles = vec![];

        for i in 0..3 {
            // Each thread gets an immutable reference to `num_ref`
            let handle = thread::spawn(move || {
                println!("Thread {} sees num: {}", i, num_ref);
            });

            handles.push(handle);
        }

        // Wait for all threads to finish
        for handle in handles {
            handle.join().unwrap();
        }

        println!("Main thread sees num: {}", num);
    }
```
\

8.  Modifying data across threads:\
    If you want to read/modify the data across threads, `T` does not need to implement `Sync` directly.\
    Instead, you can wrap `T` in a `Mutex<T>`, which provides thread-safe mutable access.\
    In this case, it’s `Mutex<T>` that guarantees the thread safety.



<hr/>
### Object-Oriented Programming Features of Rust

1.  One aspect commonly associated with OOP is the idea of __encapsulation__,\
    which means that the implementation details of an object aren’t accessible to code using that object.
```rust
    pub struct AveragedCollection {
        list: Vec<i32>,
        average: f64,
    }
```
\

2.  Inheritance is a mechanism whereby an object can inherit elements from another object’s definition,\
    thus gaining the parent object’s data and behavior without you having to define them again.

\

3.  To many people, polymorphism is synonymous with inheritance.\
    But it’s actually a more general concept that refers to code that can work with data of multiple types.\
    For inheritance, those types are generally subclasses.


<hr/>
### Patterns and Matching

1.  Patterns are a special syntax in Rust for matching against the structure of types.\
    A pattern consists of some combination of the following:
    - Literals
    - Destructured arrays, enums, structs, or tuples
    - Variables
    - Wildcards
    - Placeholders

\

2.  Formally, `match` expressions are defined as the keyword `match`, a value to match on,\
    and one or more match arms that consist of a pattern and an expression to run if the value matches that arm’s pattern,
    like this:
```rust
    let x = Some(10);
    match x {
        Some(i) => Some(i + 1),
        None => None,
    }
```
-   The particular pattern `_` will match anything, but it never binds to a variable, so it’s often used in the last match arm.

\

3.  `if let` expressions mainly as a shorter way
    to write the equivalent of a `match` that only matches one case.\
    No need for examples, just look previous chapters!

\

4.  Similar in construction to `if let`,\
    the `while let` conditional loop allows a `while` loop to run for as long as a pattern continues to match.
```rust
    let mut stack = Vec::new();

    stack.push(1);
    stack.push(2);
    stack.push(3);

    while let Some(top) = stack.pop() {
        println!("{}", top);
    }
```
\

5.  In a `for` loop, the value that directly follows the keyword `for` is a pattern.\
    For example, in `for x in y` the `x` is the pattern.
```rust
    let v = vec!['a', 'b', 'c'];

    for (index, value) in v.iter().enumerate() {
        println!("{index}: {value}");
    }
```
\

6.  Function parameters can also be patterns.
```rust
    struct Point {
        x: i32,
        y: i32,
    }

    fn foo(x: i32) {
    }

    fn print_coordinates(&(x, y): &(i32, i32)) {
        println!("Coordinate: ({x}, {y})");
    }

    fn print_point(Point {x, y}: Point) {
        println!("Point: ({x}, {y})");
    }
    fn print_point2(Point { x: a, y: b }: Point) {
        println!("Point coordinates are: ({a}, {b})");
    }
    fn print_point3(Point { x: a, y: b }: &Point) {
        println!("Point coordinates are: ({a}, {b})");
    }

    fn main() {
        let point = (10, 20);
        print_coordinates(&point);
    }
```
\

7.  We can also use patterns in closure parameter lists in the same way as in function parameter lists,\
    because closures are similar to functions.

\

8.  More advanced usage of `let`.
```rust
    let x = Some(10);
    // If `x` failed to de-compose in the `if` branch,
    // its ownership will not be transfered, and goes to the next conditional branch for judge.
    let Some(i) = x else {
        println!("{x:?} was gone!");
        return;
    }
    println!("{i}");
```
\

9.  In match expressions, you can `match` multiple patterns using the `|` syntax, which is the pattern `or` operator.
```rust
    let x = 1;

    match x {
        1 | 2 => println!("ONE or TWO"),
        3     => println!("THREE"),
        _     => println!("Anything Else"),
    }
```
\

10. Matching Ranges of Values with `..` or `..=`
```rust
    let x = 10;
    match x {
        0..10 => println!("ONE digit"),
        10..100 => println!("TWO digits"),
        _ => println!("Too large!"),
    }

    let ch = 'c';
    match ch {
        'a'..='z' => println!("Upper"),
        'A'..='Z' => println!("Lower"),
        _ => println!("Unknown"),
    }
```
\

11. Destructuring Structs.
```rust
    struct Point {
        x: i32,
        y: i32,
    }

    struct Messenger {
        message: String,
    }

    fn main() {
        /// This does not transfer ownership, only copies.
        let p = Point { x:10, y:20 };
        let Point { x, y } = p;
        assert_eq!(x, 10);
        assert_eq!(y, 20);
        let Point { x: a, y: b } = p;
        assert_eq!(a, 10);
        assert_eq!(b, 20);

        /// This transfers ownership.
        let messenger = Messenger {
                            message: String::from("MSG")
                        };
        let Messenger { message: msg } = messenger;
        assert_eq!(msg, "MSG");
        // Uncommenting the following line would cause a compiler ERROR,
        // beacuse the ownership was already moved.
        // let Messenger { message: msg } = messenger;

        /// This borrows the field(s).
        let messenger = Messenger {
                            message: String::from("MSG")
                        };
        let Messenger { ref message: msg } = messenger;
        assert_eq!(msg, "MSG");
        // Could do it again!
        let Messenger { ref message: msg } = messenger;
        assert_eq!(msg, "MSG");
        // This also does the borrowing.
        let Messenger { message: msg } = &messenger;
        assert_eq!(msg, "MSG");
    }
```
\

12. Destructuring Enums.
```rust
    enum Message {
        Quit,
        Move {},
        Write(String),
        SetColor(i32, i32, i32),
    }

    fn main() {
        let msg = Message::SetColor(0, 177, 255);

        match msg {
            Message::Quit => {
            },

            Message::Move {x, y} => {
            },

            Message::Write(text) => {
            },

            Message::SetColor(r, g, b) => {
            },
        }
        // Now `msg` is no longer usable, its ownership taken.
    }
```
\

13. Ignoring an Entire Value with `_`
```rust
    fn foo(_: i32, y: i32) {
        println!("This code only uses the y parameter: {y}");
    }

    fn main() {
        foo(100, 200);
    }
```
\

14. Ignoring Remaining Parts of a Value with `..`
```rust
    struct Point {
        x: i32,
        y: i32,
        z: i32,
    }

    fn main() {
        let origin = Point {
            x: 0,
            y: 0,
            z: 0,
        };
        match origin {
            Point { x: 0, y: 0, z: 0 } => println!("ORIGIN!"),
            Point { x, .. } => println!("x is {x}!"),
        }

        let numbers = (1, 2, 3, 4, 5);
        match numbers {
            (0, 0, 0, 0, 0) => println!("ZERO"),
            (first, .., last) => println!("First and Last: {}, {}", first, last),
        }
    }
```
\

15. Extra Conditionals with Match Guards.
```rust
    fn main() {
        let x = Some(100);
        match x {
            Some(0) => println!("Zero"),
            Some(i) if i > 0 => println!(">0"),
            Some(i) if i < 0 => println!("<0"),
            None => println!("None"),
        }

        let x = 1;
        let cond = false;
        match x {
            1 | 2 | 3 if cond => println!("YES"),
            _ => println!("NO"),
        }
        // -> NO
    }
```
\

16. The `at` operator `@` lets us create a variable that holds a value at the same time
    as we’re testing that value for a pattern match.
```rust
    enum Message {
        Hello { id: i32 },
    }

    struct Point {
        x: i32,
        y: i32,
        z: i32,
    }

    fn main() {
        let msg = Message::Hello { id: 5 };
        match msg {
            Message::Hello {
                // Bind the entire `id` to `special_id` while also checking that it's within the range 1..10
                id: special_id @ 0..10,
            }
            => println!("Found an id in range: {special_id}"),

            Message::Hello {
                id: 10..=100
            }
            => println!("Found an id {id} in another range!"),

            Message::Hello { id } => println!("{id}")
        }
        // -> Found an id in range: 5


        let p = Point {
            x: 0,
            y: 0,
            z: 0,
        };
        match p {
            origin @ Point { x:0, y:0, z:0 } => println!("({}, {}, {})", origin.x, origin.y, origin.z),
            Point { z, .. } if z < 0 => println!("{z}"),
            _ => println!("No match!"),
        }
    }
```


<hr/>
### Unsafe Rust

1.  All the code we’ve discussed so far has had Rust’s memory safety guarantees enforced at compile time.
    However, Rust has a second language hidden inside it that doesn’t enforce these memory safety guarantees,
    it’s called `unsafe` Rust and works just like regular Rust, but gives us extra superpowers.

\

2.  Unsafe Rust exists because, by nature, static analysis is conservative.
    When the compiler tries to determine whether or not code upholds the guarantees,
    it’s better for it to reject some valid programs than to accept some invalid programs.
    Although the code might be okay, if the Rust compiler doesn’t have enough information to be confident, it will reject the code.
    In these cases, you can use unsafe code to tell the compiler, “Trust me, I know what I’m doing.”

\

3.  Another reason Rust has an `unsafe` alter ego is that the underlying computer hardware is inherently `unsafe`.
    If Rust didn’t let you do `unsafe` operations, you couldn’t do certain tasks.
    Rust needs to allow you to do low-level systems programming, such as directly interacting with the operating system
    or even writing your own operating system.
    Working with low-level systems programming is one of the goals of the language.

\

4.  To switch to `unsafe` Rust, use the `unsafe` keyword and then start a new block that holds the `unsafe` code.
    You can take five actions in `unsafe` Rust that you can’t in safe Rust, which we call __unsafe superpowers__.
    Those superpowers include the ability to:
    - Dereference a raw pointer
    - Call an unsafe function or method
    - Access or modify a mutable static variable
    - Implement an unsafe trait
    - Access fields of a union

\

5.  It’s important to note that `unsafe` does NOT turn off the borrow checker or disable any other of Rust’s safety checks,
    if you use a reference in `unsafe` code, it will still be checked.
    The `unsafe` keyword only gives you access to these five features that are then not checked by the compiler for memory safety.


<hr/>
### Dereferencing a Raw Pointer

1.  A raw pointer is a special kind of pointer that can point to ANY memory address.\
    Unsafe Rust has two new types called __raw pointers__ that are similar to references.\
    As with references, raw pointers can be immutable or mutable and are written as `*const T` and `*mut T`, respectively.\
    The asterisk isn’t the dereference operator, it’s part of the type name.\
    When we say a pointer is immutable, it means that after you dereference the pointer
    (that is, when you access the value the pointer is pointing to), you can’t directly modify that value.

\

2.  Different from references and smart pointers, raw pointers:

    - Are allowed to ignore the borrowing rules by having both immutable and mutable pointers
      or multiple mutable pointers to the same location.

    - Aren’t guaranteed to point to valid memory.

    - Are allowed to be `std::ptr::null()`.

    - Don’t implement any automatic cleanup.
```rust
    use std::slice;

    static mut COUNTER: u32 = 0;

    fn incre_count(inc: u32) {
        unsafe {
            COUNTER += inc;
        }
    }

    // A trait is unsafe when at least one of its methods has some invariant that the compiler can’t verify.
    // 全凭自觉！
    unsafe trait Foo {
        fn divide(&self, divisor: i32) -> i32;
    }

    unsafe impl Foo for i32 {
        fn divide(&self, divisor: i32) -> i32 {
            if divisor == 0 {
                panic!("Division by zero!");
            }
            self / divisor
        }
    }

    fn main() {
        ///
        incre_count(10);
        unsafe {
            println!("{}", COUNTER);
        }
        ///


        ///
        let x: i32 = 10;
        unsafe {
            println!("10 / 2 = {}", x.divide(2))
        }
        ///


        ///
        let x: i32 = 100;
        let valid_ptr: *const i32 = &x;
        let null_ptr: *const i32 = std::ptr::null();

        unsafe {
            if valid_ptr.is_null() {
                println!("valid_ptr is null.");
            } else {
                println!("valid_ptr points to {}", *valid_ptr);
            }

            if null_ptr.is_null() {
                println!("null_ptr is null.");
            } else {
                println!("null_ptr points to {}", *null_ptr);
            }
        }
        ///


        ///
        let address = 0x012345usize;
        let address = address as *const i32;
        unsafe {
            println!("The address points to {}", *address);
        }
        ///


        ///
        let mut n = 10;
        let r1 = &n as *const i32;
        let r2 = &mut n as *mut i32;
        unsafe {
            println!("{}, {}", *r1, *r2);
            *r2 = 20;
            println!("{}, {}", *r1, *r2);
        }
        println!("{}", n);
        ///


        ///
        unsafe {
            dangerous();
        }
        ///


        ///
        let mut v = vec![1, 2, 3, 4, 5, 6];
        let r = &mut v[..];
        let (a, b) = split_at_mut(r, 3);

        assert_eq!(a, &mut [1, 2, 3]);
        assert_eq!(b, &mut [4, 5, 6]);
        ///


        ///
        unsafe {
            println!("{}", abs(-3));
        }
        ///
    }

    unsafe fn dangerous() {}

    fn split_at_mut(
        values: &mut [i32],
        mid: usize
    )  -> (&mut [i32], &mut [i32])
    {
        let len = values.len();
        let ptr = values.as_mut_ptr();

        assert!(mid <= len);

        unsafe {
            (
                slice::from_raw_parts_mut(ptr, mid),
                slice::from_raw_parts_mut(ptr.add(mid), len - mid)
            )
        }
    }

    extern "C" {
        fn abs(input: i32) -> i32;
    }
```
\

3.  We can also use `extern` keyword to create an interface that allows other languages to call Rust functions.\
    Instead of creating a whole `extern` block, we add the `extern` keyword and specify the ABI to use just before the `fn` keyword for the relevant function.
    We also need to add a `#[no_mangle]` annotation to tell the Rust compiler not to mangle the name of this function.
    __Mangling__ is when a compiler changes the name we’ve given a function to a different name
    that contains more information for other parts of the compilation process to consume but is less human readable.
    Every programming language compiler mangles names slightly differently,
    so for a Rust function to be nameable by other languages, we must disable the Rust compiler’s name mangling.
```rust
    #[no_mangle]
    pub extern "C" fn call_from_c() {
        println!("Just called a Rust function from C!");
    }

    fn main() {
        // safely callable.
        call_from_c();
    }
```
    This usage of `extern` does not require `unsafe`.


<hr/>
### Advanced Traits

1.  __Associated types__ connect a `type placeholder` with a `trait` such that the trait method definitions can use these placeholder types in their signatures.
    The implementor of a trait will specify the concrete type to be used for the particular implementation.\
    One example of a trait with an __associated type__ is the `Iterator` trait that the standard library provides.
```rust
    pub trait Iterator {
        type Item;

        fn next(&mut self) -> Option<Self::Item>;

        // --snipet--
    }

    impl Iterator for Counter {
        type Item = u32;

        fn next(&mut self) -> Option<Self::Item> {
            // --snipet--
        }
    }
```
\

2.  This syntax seems comparable to that of generics.\
    So the question is: why not just define the Iterator trait with generics? Like follow:
```rust
    pub trait Iterator<T> {
        fn next(&mut self) -> Option<T>;
    }
```
-   The difference is that when using generics, we must annotate the types in each implementation.
-   In other words, when a trait has a generic parameter, it can be implemented for a type multiple times,
    changing the concrete types of the generic type parameters each time.
-   For example `impl Iterator<String> for Counter` and also `impl Iterator<i32> for Counter` again.
    With associated types, we don’t need to annotate types because we can’t implement a trait on a type multiple times.
    there can only be one `impl Iterator for Counter`.

\

3.  Rust can also use `type` keyword for type aliases.
```rust
    type Kilometers = u32;
    type ResultAlias<T> = Result<T, std::io::Error>;

    struct Meters(i32);
    type Meter = Meters;

    fn read_file_content() -> ResultAlias<String> {
        use std::fs::File;
        use std::io::{self, Read};

        let mut file = File::open("file.txt")?;
        let mut contents = String::new();
        file.read_to_string(&mut contents)?;
        Ok(contents)
    }

    fn main() {
        let distance: Kilometers = 100;
        println!("{}", distance);

        match read_file_content() {
            Ok(contents) => println!("File content: {}", contents),
            Err(e) => println!("Error reading file: {}", e),
        }

        let m1: Meters = Meters(100);
        let m2: Meter = Meters(200);
        let m3 = Meter(300);
    }
```

<hr/>
### Default Generic Type Parameters and Operator Overloading

1.  When we use generic type parameters, we can specify a default concrete type for the generic type.\
    This eliminates the need for implementors of the trait to specify a concrete type if the default type works.\
    You specify a default type when declaring a generic type with the `<PlaceholderType=ConcreteType>` syntax.
```rust
    trait Container<T = String> {
        fn get_value(&self) -> T;
    }

    struct StringContainer;

    impl Container for StringContainer {
        fn get_value(&self) -> String {
            String::from("STR");
        }
    }

    struct I32Container;

    impl Container<i32> for I32Container {
        fn get_value(&self) -> i32 {
            100
        }
    }
```
\

2.  Rust doesn’t allow you to create your own operators or overload arbitrary operators.\
    But you can overload the operations and corresponding traits listed in `std::ops`
    by implementing the traits associated with the operator.

    The following example implements `Add` trait on a Point struct for `+` operator.
```rust
    use std::ops::Add;

    #[derive(Debug, Copy, Clone, PartialEq)]
    struct Point {
        x: i32,
        y: i32,
    }

    impl Add for Point {
        type Output = Point;

        fn add(self, other: Point) -> Point {
            Point {
                x: self.x + other.x,
                y: self.y + other.y,
            }
        }
    }
```
-   The default generic type of `Add` trait is here:
```rust
    trait Add<Rhs=Self> {
        type Output;

        fn add(self, rhs: Rhs) -> Self::Output;
    }
```
\

3.  When calling methods with the same name, you’ll need to tell Rust which one you want to use.
```rust
    trait Pilot {
        fn fly(&self);
    }

    trait Wizard {
        fn fly(&self);
    }

    struct Human;

    impl Pilot for Human {
        fn fly(&self) {
            println!("Pilot fly");
        }
    }

    impl Wizard for Human {
        fn fly(&self) {
            println!("Wizard fly");
        }
    }

    impl Human {
        fn fly(&self) {
            println!("Human fly");
        }
    }

    fn main() {
        let human = Human;

        Pilot::fly(&human);
        Wizard::fly(&human);
        human.fly();
    }
```
-   Another Example:
```rust
    trait Animal {
        fn name() -> String;
    }

    struct Dog;

    impl Animal for Dog {
        fn name() -> String {
            String::from("Puppy")
        }
    }

    impl Dog {
        fn name() -> &'static str {
            "Dogg"
        }
    }

    fn main() {
        println!("{}", <Dog as Animal>::name());
        println!("{}", Dog::name());
    }
```
\

4.  Sometimes, you might write a trait definition that depends on another trait:\
    for a type to implement the first trait, you want to require that type to also implement the second trait.\
    You would do this so that your trait definition can make use of the associated items of the second trait.\
    The trait your trait definition is relying on is called a __supertrait__ of your trait.\
    In Rust, a trait can also have _multiple_ supertraits.

    In this example, we’ll create two traits: `Displayable` and `Printable`.\
    The `Printable` trait will depend on the `Displayable` trait,\
    meaning any type that implements `Printable` must also implement `Displayable`.
```rust
    use std::fmt::Display;

    trait Displayable : Display {
        fn display_value(&self) -> String;
    }

    trait Printable : Displayable {
        fn print(&self) {
            println!("{}", self.display_value());
        }
    }

    trait FancyPrintable : Displayable + Clone {
        fn fancy_print(&self) {
            let cloned_self = self.clone();
            println!("{}", cloned_self.display_value());
        }
    }

    impl Displayable for i32 {
        fn display_value(&self) -> String {
            format!("i32 {}", self)
        }
    }

    impl Printable for i32 {
    }

    struct Point(i32, i32);
    impl Display for Point {
        fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
            write!(f, "({}, {})", self.0, self.1)
        }
    }

    fn main() {
        let n = 42;
        n.print();
    }
```

<hr/>
### Advanced Types

1.  The Newtype Pattern in Rust involves creating a wrapper type (a "new type")\
    around an existing type to provide additional type safety, abstraction, or to implement traits differently.
```rust
    struct Meters(i32);     // Newtype around i32
    struct Kilometers(i32); // Also newtype around i32

    impl Meters {
        fn to_kilometers(self) -> Kilometers {
            Kilometers(self.0 / 1000)
        }
    }

    impl Kilometers {
        fn to_meters(self) -> Meters {
            Meters(self.o * 1000)
        }
    }
```
\

2.  Rust has a special type named `!` that’s known in type theory lingo as the __empty type__, because it has no values.\
    We prefer to call it the __never__ type because it stands in the place of the return type when a function will never return.
```rust
    fn never_returns() -> ! {
        panic!("This function never returns!");
    }

    fn infinite_loop() -> ! {
        loop {
            println!("LOOP");
        }
    }
```
\

3.  Rust needs to know certain details about its types, such as how much space to allocate for a value of a particular type.
    This leaves one corner of its type system a little confusing at first: the concept of __dynamically sized types__.\
    Sometimes referred to as __DST__s or __unsized__ types,
    these types let us write code using values whose size we can know only at runtime.

\

4.  Let’s dig into the details of a dynamically sized type called `str`, which we’ve been using throughout the book.\
    That’s right, not `&str`, but `str` on its own, is a __DST__.\
    We can’t know how long the string is until runtime, meaning we can’t create a variable of type `str`,
    nor can we take an argument of type `str`.\
    Consider the following code, which does not work:
```rust
    let s1: str = "Hello there!";
    let s2: str = "How's it going?";
```
-   Rust needs to know how much memory to allocate for any value of a particular type,
    and all values of a type must use the same amount of memory.
-   If Rust allowed us to write this code, these two `str` values would need to take up the same amount of space.
-   But they have different lengths: ___s1___ needs `12` bytes of storage and ___s2___ needs `15`.
-   This is why it’s not possible to create a variable holding a dynamically sized type.

\

5.  So what do we do? In this case, you already know the answer: we make the types of ___s1___ and ___s2___ a `&str` rather than a `str`.
    As such, we can know the size of a `&str` value at compile time: it’s twice the length of a `usize`.
    That is, we always know the size of a `&str`, no matter how long the string it refers to is.

\

6.  We can also combine `str` with all kinds of pointers: for example, `Box<str>` or `Rc<str>`.\
    In fact, you’ve seen this before but with a different dynamically sized type: `traits`.\
    Every `trait` is a dynamically sized type we can refer to by using the name of the trait.
>
    To use traits as trait objects, we must put them behind a pointer,
    such as `&dyn Trait` or `Box<dyn Trait>` (`Rc<dyn Trait>` would work too).
>
    To work with DSTs, Rust provides the `Sized` trait to determine whether or not a type’s size is known at compile time.
    This trait is automatically implemented for everything whose size is known at compile time.
    In addition, Rust implicitly adds a bound on `Sized` to every generic function.
    For example, if we wrote:
```rust
    fn generic_fn<T>(t: T) {
        // --snipet--
    }
```
    It will be automatically considered as:
```rust
    fn generic_fn<T: Sized>(t: T) {
        // --snipet--
    }
```
\

7.  By default, generic functions will work only on types that have a known size at compile time.\
    However, you can use the following special syntax to relax this restriction:
```rust
    fn generic_fn<T: ?Sized>(t: &T) {
    }
```
-   A trait bound on `?Sized` means "`T` may or may not be `Sized`"
    and this notation overrides the default that generic types must have a known size at compile time.
-   The `?Trait` syntax with this meaning is only available for `Sized`, not any other traits.
-   Also note that we switched the type of the ___t___ parameter from `T` to `&T`.
-   Because the type might not be `Sized`, we need to use it behind some kind of pointer.
-   In this case, we’ve chosen a reference.

\

8.  Example Usage with `?Size`.\
    You need to do so when working with trait objects or slices etc.
```rust
    fn print_debug<T: ?Sized + std::fmt::Debug>(t: &T) {
        println!("{:?}", t);
    }

    fn main() {
        let x = 10;
        let slice_integers: &[i32] = &[1, 2, 3];

        print_debug(&x);                // To work with a sized type.
        print_debug(slice_integers);    // To work with an unsied type. (slice)
    }
```


<hr/>
### Advanced Functions and Closures

1.  We’ve talked about how to pass closures to functions, you can also pass regular functions to functions!
    This technique is useful when you want to pass a function you’ve already defined rather than defining a new closure.
    Functions coerce to the type `fn`. The `fn` type is called a function pointer.
    Passing functions with function pointers will allow you to use functions as arguments to other functions.
```rust
    enum Status {
        Value(u32),
        Stop,
    }

    fn add_one(x: i32) -> i32 {
        x + 1
    }

    fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
        f(arg) + f(arg)
    }

    fn main() {
        let result = do_twice(add_one, 5);
        println!("{}", result);                 // -> 12
        let result = do_twice(|x| {x + 1}, 5);
        println!("{}", result);                 // -> 12

        let list_of_numbers = vec![1, 2, 3];
        let list_of_strings: Vec<String> =
            list_of_numbers
                .iter()
                .map(|i| i.to_string())
                .collect();

        let list_of_numbers = vec![1, 2, 3];
        let list_of_strings: Vec<String> =
            list_of_numbers
                .iter()
                .map(ToString::to_string)
                .collect();


        let list_of_statuses: Vec<Status> =
            (0u32..20)
                .map(Status::Value)
                .collect();

    }
```
\

2.  Closures are represented by `traits`, which means you can’t return closures directly.
    In most cases where you might want to return a `trait`,
    you can instead use the concrete type that implements the `trait` as the return value of the function.
    However, you can’t do that with closures because they don’t have a concrete type that is returnable,
    you’re also not allowed to use the function pointer `fn` as a return type.
    The solution is:
```rust
    fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
        Box::new(|x| x + 1)
    }
```


<hr/>
### Macros

1.  The term `macro` refers to a family of features in Rust:
    __declarative macros__ with `macro_rules!` and three kinds of __procedural macros__:

    - Custom `#[derive]` macros that specify code added with the derive attribute used on structs and enums.
    - Attribute-like macros that define custom attributes usable on any item.
    - Function-like macros that look like function calls but operate on the tokens specified as their argument.

\

2.  The most widely used form of macros in Rust is the __declarative macro__.
    These are also sometimes referred to as "macros by exampl,", "`macro_rules!` macros", or just plain "macros".
    At their core, declarative macros allow you to write something similar to a Rust `match` expression.

\

3.  To define a macro, you use the `macro_rules!` construct.
    Let’s explore how to use `macro_rules!` by looking at how the `vec!` macro is defined.
```rust
    #[macro_export]
    macro_rules! vec {
        ( $( $x:expr ),* ) => {
            {
                let mut temp_vec = Vec::new();
                $(
                    temp_vec.push($x);
                )*
                temp_vec
            }
        };
    }
```
-   This shows a slightly simplified definition of the `vec!` macro.

\

4.  The `#[macro_export]` annotation indicates that this macro should be made available
    whenever the crate in which the macro is defined is brought into scope.
    Without this annotation, the macro can’t be brought into scope.
>
    We then start the macro definition with `macro_rules!` and the name of the macro we’re defining without the exclamation mark.
    The name, in this case ___vec___, is followed by curly brackets denoting the body of the macro definition.
>
    The structure in the `vec!` body is similar to the structure of a `match` expression.
    Here we have one arm with the pattern `( $( $x:expr ),* )`, followed by `=>` and the associated code block with this pattern.
    If the pattern matches, the associated block of code will be emitted.
    Given that this is the only pattern in this macro, there is only one valid way to match, any other pattern will result in an error.
    More complex macros will have more than one arm.
>
    First, we use a set of parentheses to encompass the whole pattern.
    We use a dollar sign (`$`) to declare a variable in the macro system that will contain the Rust code matching the pattern.
    The `$` makes it clear this is a macro variable as opposed to a regular Rust variable.
    Next comes a set of parentheses that captures values that match the pattern within the parentheses for use in the replacement code.
    Within `$()` is `$x:expr`, which matches any Rust expression and gives the expression the name `$x`.
>
    The comma following `$()` indicates that a literal comma separator character could optionally appear
    after the code that matches the code in `$()`.
    The `*` specifies that the pattern matches `zero or more` of whatever precedes the `*`.
>
    When we call this macro with `vec![1, 2, 3];`, the `$x` pattern matches three times with the three expressions `1`, `2`, and `3`.

\

5.  The second form of macros is the procedural macro, which acts more like a function (and is a type of procedure).
    Procedural macros accept some code as an input, operate on that code,
    and produce some code as an output rather than matching against patterns and replacing the code with other code as declarative macros.
    The three kinds of procedural macros are custom derive, attribute-like, and function-like, and all work in a similar fashion.

\

6.  When creating procedural macros, the definitions must reside in their own crate with a special crate type.
    This is for complex technical reasons that we hope to eliminate in the future.
    We'll show now how to define a procedural macro, where `some_attribute` is a placeholder for using a specific macro variety.
```rust
    use proc_macro;
    use proc_macro::TokenStream;

    #[some_attribute]
    pub fn some_name(input: TokenStream) -> TokenStream {
    }
```
-   The function that defines a procedural macro takes a `TokenStream` as an input and produces a `TokenStream` as an output.
    The `TokenStream` type is defined by the `proc_macro` crate that is included with Rust and represents a sequence of tokens.
    This is the core of the macro:
    the source code that the macro is operating on makes up the input `TokenStream`, and the code the macro produces is the output `TokenStream`.
    The function also has an attribute attached to it that specifies which kind of procedural macro we’re creating.
    We can have multiple kinds of procedural macros in the same crate.


<hr/>
### Example of a Procedural Macro Using `proc_macro`

1.  This example demonstrates a procedural macro that repeats any given input code twice,\
    essentially "duplicating" it.

\

2.  To create a new procedural macro crate.
```zsh
    $ cargo new my_macro --lib
    $ cd my_macro/
```
\

3.  Inside the generated `Cargo.toml`, you must specify that this crate is for procedural macros
    by adding the `proc-macro = true` attribute.

> - Cargo.toml
```toml
    [lib]
    proc-macro = true
```
\

4.  Now let's implement it.

> - src/lib.rs
```rust
    use proc_macro::TokenStream;

    // This marks the `duplicate()` function as procedural macro.
    #[proc_macro]
    pub fn duplicate(input: TokenStream) -> TokenStream {
        // Convert the input TokenStream into a string of code.
        let input_code = input.to_string();

        // Duplicate the input code.
        let output_code = format!("{}{}", input_code, input_code);

        // Convert the string back to TokenStream and return it.
        output_code.parse().unwrap()
    }
```
\

5.  To create an executable crate.
```zsh
    $ cargo new macro_client
    $ cd macro_client/
```
\

6.  Inside the `Cargo.toml`, add the procedural macro crate (my_macro) as dependency.
```toml
    [dependencies]
    my_macro = { path = "../my_macro" }
```
\

7.  Inside `src/main.rs`, use the procedural macro.
```rust
    use my_macro::duplicate;

    fn main() {
        duplicate! {
            println!("A");
        }
    }
```
\

8.  Run the `macro_client`.
```zsh
    $ cargo run

    A
    A
```
\

9.  Procedural macros cannot be defined in the same crate as the executable,
    because they must be compiled before they are used,
    having them in the same crate might break the compilation order.

\

10. More Complex Example (Function Generator)
```rust
    use proc_macro::TokenStream;
    use quote::quote;

    #[proc_macro]
    pub fn generate_fn(_input: TokenStream) -> TokenStream {
        let expanded = quote! {
            fn hello_world() {
                println!("Hello, world!");
            }
        };

        expanded.into()
    }
```
    To use it in another project be like:
```rust
    use my_macro::generate_fn;

    // This generates the `hello_world` function
    generate_fn!();

    fn main() {
        hello_world();
    }
```

<hr/>
### How to Write a Custom `derive` Macro

1.  Writing a custom `derive` macro in Rust allows you to automatically generate code for implementing a trait for a given type
    (usually structs or enums).

\

2.  To create a procedural macro crate.
```zsh
    $ cargo new my_derive_macro --lib
    $ cd my_derive_macro/
```
\

3.  In `my_derive_macro/Cargo.toml`,\
    mark this crate as a procedural macro crate by adding `proc-macro = true`.
```toml
    [package]
    name = "my_derive_macro"
    version = "0.1.0"
    edition = "2021"

    [lib]
    proc-macro = true
```
\

4.  To create custom derive macros,
    we often use the `syn` and `quote` crates to help parse Rust code and generate code respectively.\
    Add them into the `Cargo.toml`.
```toml
    [dependencies]
    syn = "2.0"
    quote = "1.0"
```
\

5.  In `src/lib.rs`, define your custom derive macro.
```rust
    use proc_macro::TokenStream;
    use quote::quote;
    use syn;

    #[proc_macro_derive(MyDebug)]
    pub fn my_debug_derive(input: TokenStream) -> TokenStream {
        // Parse the input tokens into a syntax tree.
        let ast = syn::parse(input).unwrap();

        // Build the implementation of the Debug trait.
        impl_my_debug(&ast)
    }

    fn impl_my_debug(ast: &syn::DeriveInput) -> TokenStream {
        // Get the name of the struct or enum.
        let name = &ast.ident;

        // Generate code using the quote! macro.
        let gen = quote! {
            impl std::fmt::Debug for #name {
                fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
                    write!(f, "{}", stringify!(#name))
                }
            }
        };

        gen.into()
    }
```
\

6.  To create a new binary project.
```zsh
    $ cargo new my_macro_client
    $ cd my_macro_client/
```
> Cargo.toml
```toml
    [dependencies]
    my_derive_macro = {
        path = "../my_derive_macro"
    }
```
> src/main.rs
```rust
    use my_derive_macro::MyDebug;

    #[derive(MyDebug)]
    struct TestStruct {
        value: i32
    }

    fn main() {
        let test = TestStruct { value: 10 };
        println!("{:?}", test); // -> TestStruct
    }
```


<hr/>
### Attribute-like Macros

1.  Attribute-like macros in Rust allow you to define custom attributes for items like functions, structs, and enums.
    These macros can modify the code at compile time, similar to `#[derive]` macros,
    but they give you more flexibility since they can work on any item (not just enums or structures).

\

2.  To create new procedural macro crate.
```zsh
    $ cargo new my_attr_macro --lib
    $ cd my_attr_macro/
```
> - Cargo.toml
```toml
    [lib]
    proc-macro = true

    [dependencies]
    syn = "2.0"
    quote = "1.0"
```
> - src/lib.rs
```rust
    use proc_macro::TokenStream;
    use quote::quote;
    use syn::{parse_macro_input, ItemFn};

    #[proc_macro_attribute]
    pub fn my_custom_attrbute(_attr: TokenStream, item: TokenStream) -> TokenStream {
        // Parse the input token stream as a function.
        let input = parse_macro_input!(item as ItemFn);
        let fn_name = &input.sig.ident;
        let code_block = &input.block;

        // Generate the modified function body.
        let output = quote! {
            fn #fn_name() {
                // This will insert the original function body
                #block
                // Add some custom behavior: Print after calling the function.
                println!("Function '{}' is called", stringify!(#fn_name));
            }

        };

        // Convert the output back into a token stream
        output.into()
    }
```
\

3.  To create a binary execuable.
```zsh
    $ cargo new macro_client
    $ cd macro_client/
```
> - Cargo.toml
```toml
    [dependencies]
    my_attr_macro = { path = "../my_attr_macro" }
```
> - src/main.rs
```rust
    use my_attr_macro::my_custom_attrbute;

    #[my_custom_attrbute]
    fn test_function() {
        println!("Inside the function!");
    }

    fn main() {
        test_function();
    }
```
```zsh
    $ cargo run

    Inside the function!
    Function 'test_function' is called.
```


<hr/>
### `dyn` Keyword

1.  The `dyn` keyword in Rust is short for "dynamic dispatch" and is used when working with `trait` objects.
    Trait objects allow for dynamic polymorphism, meaning that at runtime,
    Rust can dynamically choose which implementation of a trait to call based on the concrete type of the object,
    rather than at compile time (which is known as "static dispatch").

\

2.  When you use `dyn` in Rust,\
    you are telling the compiler that you want to use "dynamic dispatch" to handle trait method calls.

\

3.  Syntax:
```rust
    // `x` is a trait object, pointing to `some_type` that implements `Trait`.
    let x: &dyn Trait = &some_type;
```
\

4.  Example in action:
```rust
    trait Draw {
        fn draw(&self);
    }

    struct Circle {
        radius: f64,
    }

    struct Squre {
        side: f64,
    }

    impl Draw for Circle {
        fn draw(&self) {
            println!("Drawing circle with radius {}", self.radius);
        }
    }

    impl Draw for Squre {
        fn draw(&self) {
            println!("Drawing square with side length {}", self.side);
        }
    }

    fn main() {
        let circle = Circle {
            radius: 10.0,
        };
        let square = Squre {
            size: 5.0,
        };

        // let shapes: Vec<Box<dyn Draw>> = vec![Box::new(circle), Box::new(square)];
        let shapes: Vec<&dyn Draw> = vec![&circle, &square];

        for shape in shapes {
            shape.draw();
        }

        static_dispatch(&circle);
        static_dispatch(&square);

        dynamic_dispatch(&circle);
        dynamic_dispatch(&square);
    }

    fn static_dispatch<T: Draw>(shape: &T) {
        shape.draw();
    }

    fn dynamic_dispatch(shape: &dyn Draw) {
        shape.draw();
    }
```



<hr/>
### Raw Identifiers

1.  Raw identifiers are the syntax that lets you use keywords where they wouldn’t normally be allowed.
    You use a raw identifier by prefixing a keyword with `r#`.

\

2.  For example, `match` is a keyword, but what if you wanna use it as a fn name?
```rust
    fn r#match(needle: &str, haystack: &str) -> bool {
        haystack.contains(needle);
    }

    fn main() {
        assert!(r#match("foo", "foobar"));
    }
```


<hr/>
# Operators

1. Macro expansion `!`
    - `ident!(...)`
    - `ident![...]`
    - `ident!{...}`
2.  Bitwise or logical complement `!`
    - `!expr`
    - [Trait] __NOT__
3.  Nonequality comparsion `!=`
    - `expr != expr`
    - [Trait] __PartialEq__
4.  Arithmetic remainder `%`
    - `expr % expr`
    - [Trait] __REM__
5.  Arithmetic remainder and assignment `%=`
    - `var %= expr`
    - [Trait] __RemAssign__
6.  Borrow `&`
    - `&expr`
    - `&mut expr`
7.  Borrowed pointer type `&`
    - `&type`
    - `&mut type`
    - `&'a type`
    - `&'a mut type`
8.  Bitwise AND `&`
    - `expr & expr`
    - [Trait] __BitAnd__
9.  Bitwise AND and assignment `&=`
    - `var &= expr`
    - [Trait] __BitAndAssign__
10. Short-circuiting logical AND `&&`
    - `expr && expr`
11. Dereference `*`
    - `expr * expr`
    - [Trait] __Deref__
12. Row pointer `*`
    - `*const type`
    - `*mut type`
13. Compound type constraint `+`
    - `trait + trait`
    - `'a + trait`
14. Arithmetic addition `+`
    - `expr + expr`
    - [Trait] __Add__
15. Arithmetic addition and assignment `+=`
    - `var += expr`
    - [Trait] __AddAssign__
16. Argument and element separator `,`
    - `expr, expr`
17. Arithmetic negation `-`
    - `- expr`
    - [Trait] __Neg__
18. Arithmetic subtraction `-`
    - `expr - expr`
    - [Trait] __Sub__
19. Arithmetic subtraction and assignment `-=`
    - `var -= expr`
    - [Trait] __SubAssign__
20. Function and closure return type `->`
    - `fn(...) -> type`
    - `|...| -> type`
21. Member access `.`
    - `expr.ident`
22. Right-exclusive range literal `..`
    - `..`
    - `expr..`
    - `..expr`
    - `expr..expr`
    - [Trait] __PartialOrd__
23. Right-inclusive range literal `..=`
    - `..=expr`
    - `expr..=expr`
    - [Trait] __PartialOrd__
24. Structure literal update syntax `..`
    - `..expr`
25. "And the rest" pattern binding `..`
    - `variant(x, ..)`
    - `struct_type { x, .. }`
26. Arithmetic division `/`
    - `expr / expr`
    - [Trait] __Div__
27. Arithmetic division and assignment `/=`
    - `var /= expr`
    - [Trait] __DivAssign__
28. Constraints `:`
    - `pat: type`
    - `ident: type`
29. Struct field initializer `:`
    - `ident: expr`
30. Loop lable `:`
    - `'a: loop {...}`
31. Statement and item terminator `;`
    - `expr;`
32. Part of fixed-size array syntax `;`
    - `[...; len]`
31. Left-shift `<<`
    - `expr << expr`
    - [Trait] __Shl__
32. Left-shift and assignment `<<=`
    - `var <<= expr`
    - [Trait] __ShlAssign__
33. Less than comparison `<`
    - `expr < expr`
    - [Trait] __PartialOrd__
34. Less than or equal comparison `<=`
    - `expr <= expr`
    - [Trait] __PartialOrd__
35. Assignment / const equivalence `=`
    - `var = expr`
    - `ident = type`
36. Equality comparison `==`
    - `expr == expr`
    - [Trait] __PartialOrd__
37. Part of match arm syntax `=>`
    - `pat => expr`
38. Greater than comparison `>`
    - `expr > expr`
    - [Trait] __PartialOrd__
39. Greater than or equal to comparison `>=`
    - `expr >= expr`
    - [Trait] __PartialOrd__
40. Right-shift `>>`
    - `expr >> expr`
    - [Trait] __Shr__
41. Right-shift and assignment `>>=`
    - `var >>= expr`
    - [Trait] __ShrAssign__
42. Pattern binding `@`
    - `ident @ pat`
43. Bitwise exclusive OR `^`
    - `expr ^ expr`
    - [Trait] __BitXor__
44. Bitwise exclusive OR and assignment `^=`
    - `var ^= expr`
    - [Trait] __BitXorAssign__
45. Pattern alternatives `|`
    - `pat | pat`
46. Bitwise OR `|`
    - `expr | expr`
    - [Trait] __BitOr__
47. Bitwise OR and assignment `|=`
    - `var |= expr`
    - [Trait] __BitOrAssign__
48. Short-cuiting logical OR `||`
    - `expr || expr`
49. Error propagating `?`
    - `expr?`



<hr/>
### Non-Operator Symbols

1.  Named life time or loop label.
    - `'ident`
2.  String literal.
    - `"..."`
3.  Raw string literal, escape characters are not processed.
    - `r"..."`
    - `r#"..."#`
    - `r##"..."##`
    - etc.
4.  Byte string literal, consturcts an array of bytes instead of a string.
    - `b"..."`
5.  Raw byte string literal, combination of raw and byte string literal.
    - `br"..."`
    - `br#"..."#`
    - `br##"..."##`
    - etc.
6.  Character literal.
    - `'...'`
7.  ASCII byte literal.
    - `b'...'`
8.  Namespace path.
    - `ident::ident`
9.  Path relative to the crate root.
    - `::path`
10. Path relative to the current module.
    - `self::path`
11. Associated constants, functions and types.
    - `type::ident`
    - `<type as trait>::ident`
12. Associated item for a type that cannot be directly named.
    (e.g. `<&T>::...`, `<[T]>::...`, etc.)
    - `<type>::...`
13. Specifies parameters to generic type in a type.
    (e.g. `Vec<u8>`)
    - `path<...>`
14. Specifies parameters to generic type, function or method in an expression.
    Often refers to as turbofish.
    - `path::<...>`
    - `method::<...>(...)`
15. Higher-ranked lifetime bounds.
    - `for<...> type`
16. Generic lifetime `'b` must outlive lifetime `'a`
    - `'b: 'a`
17. Generic type `T` must outlive lifetime `'a`
    (meaning the type cannot transitively contain any references with lifetimes shorter than `'a`).
    - `T: 'a`
18. Generic type `T` contains no borrowed references other than `'static` ones.
    - `T: 'static`
19. Compound type constraint.
    - `'a + trait + trait + trait`



<hr/>
### Macros and Attributes

1.  Outer attribute.
    - `#[meta]`
2.  Inner attribute.
    - `#![meta]`
3.  Macro substitution.
    - `$ident`
4.  Macro capture.
    - `$ident:expr`
5.  Macro repetition.
    - `$(...)...`


<hr/>
### Comments

1.  Line comment.
    - `//`
2.  Inner line doc comment.
    - `//!`
3.  Outer line doc comment.
    - `///`
4.  Block comment.
    - `/* ... */`
5.  Inner block doc comment.
    - `/*! ... */`
6.  Outer block doc comment.
    - `/** ... */`



<hr/>
# Macros

## Declarative Macros

1.  Basic Declarative Macro Syntax.
```rust
    macro_rules! my_macro {
        () => {
            println!("Pattern for no argument.");
        };

        ($val:expr) => {
            println!("Pattern with one expression.");
        };

        ($($x:expr),*) => {
            println!("Pattern for multiple values.");
            $(
                println!("Value: {}", $x);
            )*
        };
    }

    macro_rules! add {
        ($a:expr, $b:expr) => {
            (($a) + ($b))
        };

        ($a:expr) => {
            (($a) + (1))
        };
    }

    macro_rules! add_as {
        ($a:expr, $b:expr, $c:ty) => {
            ( (($a) as ($c)) + (($b) as ($c)) )
        }
    }

    macro_rules! add_all {
        $($a:expr),* => {
            // to handle the case without any arguments.
            0
            // block to be repeated.
            $(+ ($a))*
        }
    }

    macro_rules! ok_or_return {
        ($a:ident($($b:tt)*)) => {
            match $a($($b)*) {
                Ok(value) => value,
                Err(e) => {
                    return Err(e);
                },
            }
        };
    }

    fn main() -> Result<(), String> {
        let x = 10;
        let x = add!(1, 2);
        let x = add!(100);

        println!("{}", add_as!(10, 20, u8));

        println!("{}", add_all!(1, 2, 3, 4, 5));

        ok_or_return!(some_work(10, 20));
        ok_or_return!(some_work(20, 10));

        Ok(())
    }

    fn some_work(i: i64, j: i64) -> Result<(i64, i64), String> {
        if i > j {
            Ok( (i,j) )
        } else {
            Err("ERROR".to_owned())
        }
    }
```
\

2.  The declarative macros are declared using `macro_rules!`.

\

3.  Each branch can take multiple arguments, starting with the `$` sign and followed by a `token type`:
    - __item__      an item, like a function, struct, module, etc.
    - __block__     a block of statements, expression, surrounded by braces.
    - __stmt__      a statement.
    - __pat__       a pattern.
    - __expr__      an expression.
    - __ty__        a type.
    - __ident__     an identifier.
    - __path__      a path, like `foo`, `::std::mem::replace`, `transmute::<_, int>`, ...
    - __meta__      a meta item, the things that go inside `#[...]` and `#![...]` attributes.
    - __tt__        a single token tree.
    - __vis__       a possibly empty visibility qualifier, like `pub`.

\

4.  The token type that repeats is enclosed in `$()`, followed by a separator and a `*` or a `+`,
    indicating the number of times the token will repeat.
    The separator is used to distinguish the tokens from each other.
    The `$()` block followed by `*` or `+` is used to indicate the repeating block of code.
    `*` means 0 or more, `+` means one or more, `?` for zero or one.




## Procedural Macros

1.  Procedural macros allow you to operate on the abstract syntax tree (AST) of the Rust code it is given.
    A proc macro is a function from a `TokenStream` (or two) to another `TokenStream`,
    where the output replaces the macro invocation.



<hr/>
# Unions

1.  `union` declaration uses the same syntax as a `struct` declaration,\
    except with `union` in place of `struct`.
```rust
    #[repr(C)]
    union MyUnion {
        f1: u32,
        f2: f32,
    }
```
-   The key property of `union` is that all fields of a `union` share common storage.
-   As a result, writes to one field of a union can overwrite its other fields,
    and size of a union is determined by the size of its _largest_ field.

\

2.  Union field types are restricted to the following subset of types:

    - `Copy` types
    - References (`&T` and `&mut T` for arbitrary `T`)
    - `ManuallyDrop<T>` (for arbitrary `T`)
    - `Tuples` and `arrays` containing only allowed union field types

    This restriction ensures, in particular, that `union` fields never need to be dropped.
    Like for structs and enums, it is possible to `impl Drop` for a `union` to manually define what happens when it gets dropped.

\

3.  Unions without any fields are not accepted by the compiler, but can be accepted by macros.

\

4.  To access the `union` field, you have to use `unsafe`.
```rust
    let my_union = MyUnion {
        f1: 10
    };

    let f = unsafe {
        u.f1
    };
```

\

5.  Another way to access `union` fields is to use pattern matching.
```rust
    fn access_union_fields(u: MyUnion) {
        unsafe {
            match u {
                MyUnion { f1:10 } => println!("f1 TEN"),
                MyUnion { f2 } => println!("f2 {f2}"),
            }
        }
    }
```
\

6.  When you use `#[repr(C)]`,
    you are telling the Rust compiler to use the C language’s memory layout for the type,
    which ensures compatibility with C when interfacing with external (foreign) code or libraries written in C.
    (commonly referred to as FFI, Foreign Function Interface)

\

7.  When you apply `#[repr(u32)]` to an `enum`, each variant of the `enum` is represented internally as a `u32`,
    meaning that the compiler will assign specific `u32` values to each variant.
    This is particularly useful when working with FFI (Foreign Function Interface).



<hr/>
# Conditional Compilation

1.  The `cfg` attribute conditionally includes the thing it is attached to based on a configuration predicate.\
    It is written as `cfg` `(` a configuration predicate `)`.
```rust
    // The function is only included in the build when compiling for macOS.
    #[cfg(target_os = "macos")]
    fn macos_only() {
    }

    // The function is only included when either `foo` or `bar` is defined.
    #[cfg(any(foo, bar))]
    fn needs_foo_or_bar() {
    }

    // The function is only included when compiling for a unix-ish OS with a 32-bit architecture.
    #[cfg(all(unix, target_pointer_width = "32"))]
    fn on_32bit_unit() {
    }

    // The function is only included when foo is not defined.
    #[cfg(not(foo))]
    fn needs_not_foo() {
    }
    /*
        In a project using Cargo, you could define `foo` in `Cargo.toml` under [features],
        [features]
        foo = []
    */

    // The function is only included when the panic strategy is set to unwind.
    #[cfg(panic = "unwind")]
    fn when_unwinding() {
    }
```
\

2.  The built-in `cfg!` macro takes in a single configuration predicate and evaluates.
```rust
    let machine = if cfg!(unix) {
        "unix"
    } else if cfg!(windows) {
        "windows"
    } else {
        "other"
    }

    println!("I'm running on a {} machine.", machine);
```



<hr/>
# Crate-Level Attribute

1.  In Rust, `#![]` is a crate-level attribute, meaning it applies to the entire crate or module,
    not just a single item like `#[...]`.
    Crate-level attributes are typically placed at the top of your main source file (`main.rs` or `lib.rs`)
    and are used to configure certain aspects of the entire crate or program.

\

2.  To allow unused variables throughout the entire crate.
```rust
    #![allow(unused_variables)]

    fn main() {
        let x = 42;  // This would normally trigger a warning, but it's allowed
    }
```
\

3.  The following example makes all warnings into errors at the crate level.
```rust
    #![deny(warnings)]

    fn main() {
        let x = 42;  // Any warning (e.g., unused variable) will now trigger a compile-time error
    }
```
    The `#![deny(...)]` is to make specific warnings into errors at crate level.

\

4.  To not link the standard library (`std`).\
    This is commonly used for embedded systems or writing low-level code where the standard library isn’t available.
```rust
    #![no_std]

    fn main() {
        // You cannot use features from `std`, like printing to the console
    }
```
\

5.  These attributes specify the name and type of the crate.\
    This is mainly useful for libraries.
```rust
    #![crate_name = "my_library"]
    #![crate_type = "lib"]

    pub fn my_function() {
        println!("Hello from my_library");
    }
```
-   `crate_name` sets the name of the crate (which would normally be derived from the file name).
-   `crate_type` specifies whether the crate is a library (`lib`), a binary (`bin`), or other types.



<hr/>
# Const generics

1.  Constant can be used with structs.
```rust
    #[Derive(Debug)]
    struct Miles(u32);

    const FIFTY_MILES: Miles = Miles(50);

    fn main() {
        println!("{:?}", FIFTY_MILES);
    }
```
\

2.  Const generics in Rust allow you to define types that are parameterized by constant values,\
    such as integers, booleans, or even other types of constants.
```rust
    fn print_array<T, const N: usize>(arr: [T; N]) {
        println!("Array has {} elements.", N);

        for elem in arr.iter() {
            println!("{:?}", elem);
        }
    }

    struct Matrix<T, const ROWS: usize, const COLS: usize> {
        data: [[T; COLS]; ROWS].
    }

    impl<T, const ROWS: usize, const COLS: usize> Matrix<T, ROWS, COLS> {
        fn new(data: [[T; COLS]; ROWS]) -> Self {
            Matrix { data }
        }

        fn dimensions(&self) -> (usize, usize) {
            (ROWS, COLS)
        }
    }

    fn main() {
        let arr = [1, 2, 3];
        println!("{}", arr);

        let matrix = Matrix::new([[1, 2], [3, 4]]);
        println!("{}", matrix.dimensions());
    }
```


<hr/>
# Associated Constant

1.  In Rust, __associated constants__ are constants that are associated with a specific type,\
    such as a `struct`, `enum`, or `trait`.\
    These constants are defined as part of the type itself and can be accessed without creating an instance of the type
```rust
    enum Color {
        Red,
        Green,
        Blue,
    }

    impl Color {
        const DEFAULT: Color = Color::Red;
    }

    trait Shape {
        const DEFAULT: Self;
    }

    struct Circle {
        radius: f64,
    }

    impl Circle {
        const PI: f64 = 3.1415926;

        const fn new(radius: f64) -> Circle {
            Circle { radius }
        }

        fn area(&self) -> f64 {
            Circle::PI * self.radius * self.radius
        }
    }

    impl Shape for Circle {
        const DEFAULT: Slef = Circle { radius: 0 };
    }

    fn main() {
        let circle = Circle {
            radius: 2.0,
        };

        const CIRCLE: Circle = Circle::new(1.0);
        let another_circle = Circle::new(10.0);
        println!("{}", circle.area());
        println!("{}", Circle::PI);
    }
```
-   `const fn` keyword allows you to define functions that can be evaluated at compile time.
-   This means that the function can be used in contexts where the result of the function
    needs to be known and fixed before the program runs,
    such as in constant expressions, static variables, or array sizes.
-   On the other hand, a regular (non-const) function can only be evaluated at runtime.



<hr/>
# `for..in` Inner

1.  When you use a `for .. in` loop in Rust, the loop automatically invokes the `into_iter()` method on the collection being iterated over.
    Rust’s `for .. in` loop desugars into an iterator-based approach,
    meaning that it transforms the iterable into an iterator behind the scenes,
    which is then used to generate the values for each iteration.

\

2.  Consider you wrote such code:
```rust
    let v = vec![1, 2, 3];

    for elem in v {
        println!("{}", elem);
    }
```
    It's kinda internally transformed into:
```rust
    let mut v_iter = v.into_iter();
    while let Some(elem) = iter.next() {
        println!("{}", elem);
    }
```


<hr/>
# Lifetime Elision

1.  The placeholder lifetime, `'_`, can be used to have a lifetime inferred.
    For lifetimes in paths, using `'_` is preferred.
    `Trait` object lifetimes follow different rules which will be discussed later.

\

2.  Each elided lifetime in the parameters becomes a distinct lifetime parameter.
    If there is exactly one lifetime used in the parameters (elided or not),
    that lifetime is assigned to all elided output lifetimes.

\

3.  In method signatures there is another rule that if the receiver has type `&Self` or `&mut Self`,
    then the lifetime of that reference to `Self` is assigned to all elided output lifetime parameters.

\

4.  Examples:
```rust
    const STRING1: &str = "str";
    const STRING2: &'static str = "str";

    fn print1(s: &str);
    fn print2(s: &'_ str);
    fn print3<'a>(s: &'a str);

    fn debug1(lvl: usize, s: &str);
    fn debug2(lvl: usize, s: &'_ str);
    fn debug3<'a>(lvl: usize, s: &'a str);

    fn substr1(s: &str, util: usize) -> &str;
    fn substr2<'a>(s: &'a str, util: usize) -> &'a str;

    fn get_mut1(&mut self) -> &mut dyn T;
    fn get_mut2<'a>(&'a mut self) -> &'a mut dyn T;

    fn args1<T: ToCStr>(&mut self, args: &[T]) -> &mut Command;
    fn args2<'a, 'b, T: ToCStr>(&'a mut self, args: &'b [T]) -> &'a mut Command;

    fn new1(buf: &mut [u8]) -> Thing;
    fn new2(buf: &mut [u8]) -> Thing<'_>;
    fn new3<'a>(buf: &'a mut [u8]) -> Thing<'a>;

    type FunPtr1 = fn(&str) -> &str;
    type FunPtr2 = for<'a> fn(&'a str) -> &'a str;

    type FunTrait1 = dyn Fn(&str) -> &str;
    type FunTrait2 = dyn for<'a> Fn(&'a str) -> &'a str;

    const RESOLVED_SINGLE1: fn(&str) -> &str = |x| x;
    const RESOLVED_SINGLE2: for<'a> fn(&'a str) -> &'a str = |x| x;

    const RESOLVED_MULTIPLE1: &dyn Fn(&Foo, &Bar, &Baz) -> usize = &some_func;
    const RESOLVED_MULTIPLE2: for<'a, 'b, 'c> &dyn Fn(&'a Foo, &'b Bar, &'c Baz) -> usize = &some_func;
```
\

5.  The following examples are illegal where is not allowed to elide the lifetime parameter:
```rust
    // Cannot infer, because there are no parameters to infer from.
    fn get_str() -> &str;

    // Cannot infer, ambiguous if it is borrowed from the first or second parameter.
    fn frob(s: &str, t: &str) -> &str;
```
\

6.  Default trait object lifetimes.
```rust
    // For the following trait...
    trait Foo {}

    type T1 = Box<dyn Foo>;
    type T2 = Box<dyn Foo + 'static>;

    impl dyn Foo {}
    impl dyn Foo + 'static {}

    type T3<'a> = &'a dyn Foo;
    type T4<'a> = &'a (dyn Foo + 'a);

    type T5<'a> = std::cell::Ref<'a, dyn Foo>;
    type T6<'a> = std::cell::Ref<'a, dyn Foo + 'a>;


    trait Bar<'a>: 'a {}

    type T7<'a> = Box<dyn Bar<'a>>;
    type T8<'a> = Box<dyn Bar<'a> + 'a>;

    impl<'a> dyn Bar<'a> {}
    impl<'a> dyn Bar<'a> + 'a {}
```
\

7.  `Bar<'a>` (No Lifetime Bound):
	-   This means the trait `Bar` is parameterized by the lifetime `'a`,
        but there is no restriction on how the lifetime `'a` relates to the implementing type’s lifetime.
	-   The `'a` lifetime is simply used for any references that might be returned by the methods of the trait.
	`Bar<'a>`: `'a` (Lifetime Bound):
	-   This adds a lifetime bound to the trait,
        meaning that any type implementing `Bar<'a>` must live at least as long as the lifetime `'a`.
	-   This constraint ensures that the implementing type is valid for the entire `'a` lifetime
        and that no part of the implementation can outlive `'a`.
```rust
    trait Bar<'a>: 'a {
        fn get_data(&self) -> &'a str;
    }

    struct MyStruct<'a> {
        data: &'a str;
    }

    // OK, MyStruct<'a> lives at least as long as 'a
    impl<'a> Bar<'a> for MyStruct<'a> {
        fn get_data(&self) -> &'a str {
            self.data
        }
    }

    // ERR, 'b might be shorter than 'a
    impl<'a, 'b> Bar<'a> for MyStruct<'b> {
        fn get_data(&self) -> &'a str {
            self.data
        }
    }

    // OK, 'b is longer than 'a
    impl<'a, 'b> Bar<'a> for MyStruct<'b>
    where
        'b: 'a
    {
        fn get_data(&self) -> &'a str {
            self.data
        }
    }
```


<hr/>
# `for<'a>` Higher-Ranked Trait Bounds (HRTBs)

1.  It allows you to define `trait` implementations that must hold for all lifetimes,
    rather than for a specific lifetime `'a`.

\

2.  In Rust, when you write a generic trait or function, lifetimes are usually specific.
    For example:
```rust
    fn foo<'a>(x: &'a str) {}
```
-   Here, `'a` represents a specific lifetime tied to a concrete scope at runtime,
    which the compiler determines when you use the function.
-   However, sometimes you want to say: “this function or trait should work for any possible lifetime.”
    This is where higher-ranked trait bounds (HRTBs) come in.
    They allow you to say that the trait or function works for all lifetimes, not just one specific lifetime.
    The syntax to express this is `for<'a>`.
```rust
    fn foo<F>(f: F)
    where
        F: for<'a> Fn(&'a str),
    {
    }
```
-   Here, `for<'a> Fn(&'a str)` means the function ___f___ should work for any lifetime `'a`.

\

3.  To explain the need for HRTBs, consider the following case.
    Imagine you want to write a function that takes a closure,
    and this closure can accept references with any lifetime.
```rust
    fn call_with_ref<F>(f: F)
    where
        F: Fn(&'static str),
    {
        f("Hello, world!");
    }
```
-   In this example, the closure f is constrained to work only with `'static` references.
-   But what if you want the closure to work with references that have any lifetime, not just `'static`?
```rust
    fn call_with_ref<F>(f: F)
    where
        F: for<'a> Fn(&'a str),
    {
        f("Hello, World!");
    }
```
-   Now, the closure ___f___ can accept references with any lifetime,\
    and the compiler ensures that ___f___ works for all possible lifetimes `'a`.



<hr/>
# Names

1.  A __prelude__ is a collection of names that are automatically brought into scope of every module in a crate.\
    It's a convention name, you can also name differently.
```rust
    mod prelude {
        pub use crate::types::{TypeA, TypeB}; // Re-export commonly used types
        pub use crate::traits::MyTrait;       // Re-export a commonly used trait
    }

    mod types {
        pub struct TypeA;
        pub struct TypeB;
    }

    mod traits {
        pub trait MyTrait {
            fn do_something(&self);
        }
    }

    // In the main part of your code:
    use crate::prelude::*; // Import all items from the "prelude" module

    fn main() {
        let _a = types::TypeA;
        let _b = types::TypeB;
    }
```

<hr/>
# Macros

## Declarative Macros

1.  Basic Declarative Macro Syntax.
```rust
    macro_rules! my_macro {
        () => {
            println!("Pattern for no argument.");
        };

        ($val:expr) => {
            println!("Pattern with one expression.");
        };

        ($($x:expr),*) => {
            println!("Pattern for multiple values.");
            $(
                println!("Value: {}", $x);
            )*
        };
    }

    macro_rules! add {
        ($a:expr, $b:expr) => {
            (($a) + ($b))
        };

        ($a:expr) => {
            (($a) + (1))
        };
    }

    macro_rules! add_as {
        ($a:expr, $b:expr, $c:ty) => {
            ( (($a) as ($c)) + (($b) as ($c)) )
        }
    }

    macro_rules! add_all {
        $($a:expr),* => {
            // to handle the case without any arguments.
            0
            // block to be repeated.
            $(+ ($a))*
        }
    }

    macro_rules! ok_or_return {
        ($a:ident($($b:tt)*)) => {
            match $a($($b)*) {
                Ok(value) => value,
                Err(e) => {
                    return Err(e);
                },
            }
        };
    }

    fn main() -> Result<(), String> {
        let x = 10;
        let x = add!(1, 2);
        let x = add!(100);

        println!("{}", add_as!(10, 20, u8));

        println!("{}", add_all!(1, 2, 3, 4, 5));

        ok_or_return!(some_work(10, 20));
        ok_or_return!(some_work(20, 10));

        Ok(())
    }

    fn some_work(i: i64, j: i64) -> Result<(i64, i64), String> {
        if i > j {
            Ok( (i,j) )
        } else {
            Err("ERROR".to_owned())
        }
    }
```
\

2.  The declarative macros are declared using `macro_rules!`.

\

3.  Each branch can take multiple arguments, starting with the `$` sign and followed by a `token type`:

    - __item__      an item, like a function, struct, module, etc.
    - __block__     a block of statements, expression, surrounded by braces.
    - __stmt__      a statement.
    - __pat__       a pattern.
    - __expr__      an expression.
    - __ty__        a type.
    - __ident__     an identifier.
    - __path__      a path, like `foo`, `::std::mem::replace`, `transmute::<_, int>`, ...
    - __meta__      a meta item, the things that go inside `#[...]` and `#![...]` attributes.
    - __tt__        a single token tree.
    - __vis__       a possibly empty visibility qualifier, like `pub`.

\

4.  The token type that repeats is enclosed in `$()`, followed by a separator and a `*` or a `+`, 
    indicating the number of times the token will repeat. 
    The separator is used to distinguish the tokens from each other. 
    The `$()` block followed by `*` or `+` is used to indicate the repeating block of code.
    `*` means 0 or more, `+` means one or more, `?` for zero or one.



<hr/>
### Basic `async` and `await`

1.  In Rust, __`async`__ and __`await`__ are used to write asynchronous code in a more readable and efficient way.\
    Asynchronous programming allows your code to perform tasks without blocking the main thread while waiting for operations like I/O (network calls, file reads/writes) to complete.\
    This is especially useful when you have tasks that can run concurrently.

\

2.  When you declare a function as __`async`__ in Rust,\
    it allows that fn to return a __`future`__ instead of blocking while waiting for the function's result.\
    A __`future`__ is a value that represents a value that might be available at some point in the future.

\

3.  A function marked with __`async`__ does NOT run to completion when called,\
    instead, it returns a __`future`__ that needs to be executed by an __`executor`__.
```rust
    async fn fetch_data() -> String {
        "Data fetched!".to_string()
    }
```
\

4.  __`await`__ is used to wait for a __`future`__ to complete and get its result.\
    When you use __`await`__, it pauses the current function,\
    and the fn temporarily yields control (i.e., it does not block the current thread)\
    and waits for the asynchronous operation to complete.\
    Once it's done, the code resumes.
```rust
    async fn process_data() {
        let result = fetch_data().await;

        println!("{}", result);
    }
```
\

5.  Example: Simple Asynchronous Functions.
```rust
    async fn fetch_data() -> String {
        "Data fetched!".to_string()
    }

    async fn process_data() {
        let result = fetch_data().await;
        println!("{}", result);
    }

    // Entry point for asynchronous code (requires an executor)
    // Or #[async_std::main] if using async-std
    #[tokio::main]
    async fn main() {
        process_data().await;
    }
```
\

6.  Example : Simple Executor.
```rust
    use std::future::Future;
    use std::pin::Pin;
    use std::task::{Context, Poll, Waker};
    use std::collections::VecDeque;
    use futures::task::noop_waker;

    struct Task {
        future: Pin<Box<dyn Future<Output=()> + Send>>,
    }

    struct Executor {
        tasks: VecDeque<Task>;
    }

    fn main() {
        let mut executor = Executor::new();

        executor.spawn(Task::new(simple_task(1)));
        executor.spawn(Task::new(simple_task(2)));

        executor.run();
    }

    async fn simple_task(id: u32) {
        println!("Task {} started...", id);
        std::thread::sleep(std::time::Duration::from_secs(1));
        println!("Task {} completed...", id);
    }

    impl Task {
        fn new(future: impl Future<Output=()> + Send + 'static) -> Self {
            Task {
                future: Box::pin(future),
            }
        }

        fn poll(&mut self, cx: &mut Context) -> Poll<()> {
            self.future.as_mut().poll(cx)
        }
    }

    impl Executor {
        fn new() -> Self {
            Executor {
                tasks: VecDeque::new(),
            }
        }

        fn spawn(&mut self, task: Task) {
            self.tasks.push_back(task);
        }

        fn run(&mut self) {
            let waker = noop_waker();
            let mut cx = Context::from_waker(&waker);

            while let Some(mut task) = self.tasks.pop_front() {
                if task.poll(&mut cx).is_pending() {
                    self.tasks.push_back(task);
                }
            }
        }
    }
```


<hr/>
### Pinning

1.  In Rust, __`Box::pin()`__ is used to create a pinned heap-allocated value.\
    The concept of pinning is important when dealing with types that should NOT be moved in memory after they are created,\
    typically when working with asynchronous programming or self-referential types.

\

2.  In Rust, most values are free to move around in memory.\
    For example, if you pass a value to a function or return it from one, Rust might move it to a new memory location.

\

3.  Considering the following example:
```rust
    fn main() {
        let mut tracker = AddressTracker(None);
        tracker.check_address();
        let mut tracker = tracker;
        tracker.check_address();
    }

    struct AddressTracker(Option<usize>);

    impl AddressTracker {
        fn check_address(&mut self) {
            let current_address = self as *mut Self as usize;
            match self.0 {
                Some(previous_address) => {
                    println!("prev: {}", previous_address);
                    println!("curr: {}", current_address);
                },

                None => self.0 = Some(current_address),
            }
        }
    }
```
- Possible output:
    - prev: `6098595616`
    - curr: `6098595648`

\

4.  How to solve it?
```rust
    fn main() {
        let tracker = AddressTracker(None);
        let mut pinned_tracker = pin!(tracker);
        pinned_tracker.as_mut().check_address();
        let mut pinned_tracker = pinned_tracker;
        pinned_tracker.as_mut().check_address();
    }

    struct AddressTracker(Option<usize>);

    impl AddressTracker {
        fn check_address(mut self: Pin<&mut Self>) {
            let current_address = &*self as *const Self as usize;
            match self.0 {
                Some(previous_address) => {
                    println!("prev: {}", previous_address);
                    println!("curr: {}", current_address);
                },

                None => self.0 = Some(current_address),
            }
        }
    }
```
- Possible output:
    - prev: `6098595616`
    - curr: `6098595616`







