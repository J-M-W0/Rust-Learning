# Basic Data Types

| Memory only stores binary data, anything can be represented in binary.
`   `|| program determines what the binary represents.

| Boolean
`   `|| e.g. true, false
| Integer
`   `|| e.g. 1, 2, 50, 99, -2
| Double / Float
`   `|| e.g. 1.1, 5.0, 7.0001
| Character
`   `|| e.g. 'A', 'B', 'c', '6', '$'
| String
`   `|| e.g. "hello", "it's a string"


# Variable

| Immutable by default, but can be mutable.
> Example:
```rust
    let two = 2;
    let hello = "hello";
    let j = 'j';
    let my_half = 0.5;
    let mut your_name = "unknown";
    let start_program = true;
    let stop_program = false;
    let your_half = my_half;
```


# Functions

| In Rust, if an expression is the last statement in a function, 
| you can **omit the semicolon** to implicitly return the value of that expression.

| A function returns automatically after its flow of execution.
`   `|| you can also use **return** to return earlier.
`   `|| if a function returns nothing, it implicitly returns a unit **()**.
`   `|| the return type in function signature is not ignorable iff it returns an **unit**.


# *println!* macro

| Macros expand into additional code.

| **println!** displays information to the terminal.
`   `|| **{}** is for displaying variables.
`   `|| **{?:}** is useful for debugging.
`   `|| **{varname:?}** is also commonly used.
> Example:
```rs
    let meaning = 42;

    println!("heeeello");
    println!("{:?}", meaning);
    println!("{:?} {:?}", meaning, meaning);
    println!("what do you mean? {:?}", meaning);

    println!("{life:?}");
    println!("{life}");
```

# *if else* control flow

| Code executes **line-by-line**,
| specific conditions can change control flow:
`   `|| **if**
`   `|| **else if**
`   `|| **else**
> Example:
```rs
    let a = 99;
    if a > 100 {
        println!("huge number");
    } else if a > 50 {
        println!("big number");
    } else if a > 0 {
        println!("small number");
    } else {
        println!("not positive number");
    }
```


# Repetition

| Is called "looping" or "iteration",
| multiple choices of loops:
`   `|| **loop** - infinite loop.
`   `|| **while** - conditional loop.
> Loop:
```rs
    let mut a = 0;
    loop {
        if a == 10 { 
            break; 
        } else if a == 5 {
            continue;
        }
        println!("{:?}", a);
        a += 1;
    }
```
> While loop:
```rs
    let mut a = 0;
    while a != 10 {
        if a == 5 { continue; }
        println!("{:?}", a);
        a += 1;
    }
```


# *match* Pattern Matching

| Similar to **if .. else**, adds logic to program,
| but **match** will be checked by the compiler,
| if new possibilities added, you will be notified when this occurs.

| Exhausive.
`   `|| all options must be accounted for.

> Example with boolean:
```rs
    fn main() {
        let some_bool = true;
        match some_bool {
            true => println!("it's true."),
            false => println!("it's false."),
        }
    }
```

> Example with int:
```rs
    fn main() {
        let some_int = 3;
        match some_int {
            1 => println!("it's 1"),
            2 => println!("it's 2"),
            3 => println!("it's 3"),
            _ => println!("it's something else"),
        }
    }
```


# *enum* Enumeration

| Data that can be one of multiple different possibilities.
`   `|| each possibility is called a "variant".
> Example:
```rs
    enum Direction {
        Up,
        Down,
        Left,
        Right,
    }

    fn which_way(d: Direction) {
        match d {
            Direction::Up => {
                println!("up");
            },
            Direction::Down => println!("down"),
            Direction::Left => println!("left"),
            Direction::Right => println!("right"),
        }
    }
```


# *struct* Structure

| A type that contains multiple pieces of data.
`   `|| each piece is called a "field".
`   `|| all fields must be present to create a **struct**.
`   `|| fields can be accessed using a dot **(.)**
> Example:
```rs
    struct ShippingBox {
        depth: i32,
        width: i32,
        height: i32,
    }

    let my_shippingbox = ShippingBox {
        depth: 3,
        width: 2,
        height: 5,
    };

    let tall = my_shippingbox.height;
    println!("the box is {:?} units tall.", tall);
```


# Tuples

| A type of "record" that stores data anonymously (no need to name fields).
`   `|| Useful to return pairs of data from functions.
`   `|| Can be de-structed easily into variables.
> Example:
```rust
    enum Access {
        Full,
    }

    fn one_two_three() -> (i32, i32, i32) {
        (1,2,3)
    }

    let numbers = one_two_three();
    let (x, y, z) = one_two_three();
    println!("{:?}, {:?}", x, numbers.0);
    println!("{:?}, {:?}", y, numbers.1);
    println!("{:?}, {:?}", z, numbers.2);

    let (employee, access) = ("Jake", Access::Full);
```


# Expressions

| Rust is an "expression-based" language.
`   `|| most things are evaluated and return some value.

> Example: 
```rs
    let my_num = 3;
    let is_lt_5 = if my_num < 5 { 
        true 
    } else { 
        false 
    };
```

# Fundamental **Ownership**

| Programs must track memory.
`   `|| if they failed to do so, a "leak" occurs.

| Rust utilizes an "ownership" model to manage memory.
`   `|| the "owner" of memory is responsible for cleaning up the memory.
`   `|| "owner" is like the scope of the memory variable.

| Memory can be either **moved** or **borrowed**.
`   `|| default behavior is to **move** memory to a new "owner".
`   `|| use an ampersand **(&)** to allow code to **borrow** the "ownership" of the memory.

> Example - Move:
```rs
    enum Light {
        Bright,
        Dull,
    }

    fn display_light(light: Light) {
        match light {
            Light::Bright => println!("bright"),
            Light::Dull => println!("dull"),
        }
    }

    fn main() {
        let dull = Light::Dull;
        display_light(dull);
        // * error when second time calling it, 
        // * because the "ownership" transferred to the previous "display_light" function,
        // * now the previous function is responsible for "dropping" it.
        display_light(dull); 
    }
```

> Example - Borrow: 
```rs    
    enum Light {
        Bright,
        Dull,
    }

    fn display_light(light: &Light) {
        match light {
            Light::Bright => println!("bright"),
            Light::Bright => println!("dull"),
        }
    }

    fn main() {
        let dull = Light::Dull;
        display_light(&dull);
        // * now the "ownership" is just borrowed by the "display_light" function,
        // * its "owner" is still the "main" function scope.
        display_light(&dull);
    }
```


# Vector

| Multiple pieces of data
`   `|| must be the same type.
`   `|| zero-indexing.
`   `|| can **add**, **remove**, and **traverse** the entries.

> Example 1:
```rs
    let mut my_numbers = Vec::new();
    my_numbers.push(1);
    my_numbers.push(2);
    my_numbers.push(3);
    my_numbers.pop();
    my_numbers.len(); // this is 2
    let two = my_numbers[1];
```

> Example 2:
```rs
    let my_numbers = vec![1,2,3];
    // notice here the "ownership" will be moved becauseo of the for-iteration.
    // after it you won't be able to use "my_numbers".
    for num in my_numbers {
        println!("{:?}", num);
    }
```


# Strings

| Two commonly usedd types of strings:
`   `|| **String** - owned.
`   `|| **&str** - borrowed **String** slice.

| Must use an owned **String** to store in a **struct**,
| use **&str** when passing to a function if you don't wanna lose "ownership".

> Example 1:
```rs
    fn print_it(data: &str) {
        println!("{:?}", data);
    }

    fn main() {
        print_it("this is a string slice");
        let owned_string1 = "owned string1".to_owned();
        let owned_string2 = String::from("owned string2");
        print_it(&owned_string1);
        print_it(&owned_string2);
    }
```

> Example 2:
```rs
    struct Employee {
        name: String,
    }

    fn main() {
        let employee_name = String::from("e1");
        let employee = Employee {
            name: employee_name
        };
    }

```


# Type Annotations

| Types are usually inferred.
`   `|| must be required for function signatures though.
`   `|| can also be specified in code sometimes.

> Example - Basic:
```rs
    enum Mouse {
        LeftClick,
        RightClick,
        MiddleClick,
    }

    let num: i32 = 15;
    let a: char = 'a';
    let left_click: Mouse = Mouse::LeftClick;
```

> Example - Generics
```rs
    let numbers: Vec<i32> = vec![1,2,3];
    let letters: Vec<char> = vec!['a','b'];
    let clicks: Vec<Mouse> = vec![
        Mouse::LeftClick,
        Mouse::LeftClick,
        Mouse::RightClick,
    ];
```


# More on *enum*

| **enum** is not limited to just plain variants.
`   `|| each variant can optionally contain additional data.
> Example:
```rs
    enum Mouse {
        LeftClick,
        RightClick,
        MiddleClick,
        Scroll(i32),
        Move(i32, i32),
    }

    enum PromoDiscount {
        NewUser,
        Holiday(String),
    }

    enum Discount {
        Percent(f64),
        Flat(i32),
        Promo(PromoDiscount),
        Custom(String),
    }
```


# Working with *Option*

| A type that may be one of two things.
`   `|| some data of a specified type.
`   `|| nothing.

| Definition:
```rs
    enum Option<T> {
        Some(T),
        None
    }
```

> Example:
```rs
    struct GroceryItem {
        name: String,
        qty: i32,
    }

    fn find_quantity(name: &str) -> Option<i32> {
        let groceries = vec![
            GroceryItem { name: "banana".to_owned(), qty: 4  },
            GroceryItem { name: "eggs".to_owned(),   qty: 32 },
            GroceryItem { name: "bread".to_owned(),  qty: 1  },
        ];
        for item in groceries {
            if item.name == name {
                return Some(item.qty);
            }
        }
        None
    }
```


# Working with *Result*

| A data type that contains one of two types of data:
`   `|| "Successful" data
`   `|| "Error" data

| Used in scenarios where an action needs to be taken, but has the possibility of failure.
`   `|| copying a file.
`   `|| connecting to a website.

| Definition:
```rs
    enum Result<T, E> {
        Ok(T),
        Err(E)
    }
```

> Example:
```rs
    fn get_sound(name: &str) -> Result<SoundData, String> {
        if name == "alert" {
            Ok(SoundData::new("alert"))
        } else {
            Err("unable to find sound data".to_owned())
        }
    }

    let sound = get_sound("alert");
    match sound {
        Ok(_) => println!("sound data located");
        Err(e) => println!("error: {:?}", e);
    }
```


# *HashMap*

| Collection that stores data as key-value pair.
`   `|| data is located using the "key".
`   `|| the data is the "value".

| Similar to definitions in a dictionary.

| Very fast to retrieve data using the key.
`   `|| but values are not stored in the order of insertion, rather randomly.

> Example - Insert & Remove:
```rs
    let mut people = HashMap::new();
    people.insert("Susan", 41);
    people.insert("Ed", 11);
    people.insert("Willie", 14);
    people.insert("Cathy", 22);
    people.remove("Susan");

    match people.get("Ed") {
        Some(age) => println!("Ed's age is {:?}", age);
        None => println!("not found");
    }
```

> Example - Iteration:
```rs
    
    // .iter() does not transfer ownership.
    // it yields (&Key, &Value) pairs.
    for (person, age) in people.iter() {
        println!("person = {person:?}, age = {age:?}");
    }

    // .keys() does not transfer ownership.
    // it yields "&Key".
    for person in people.keys() {
        println!("person = {person:?}");
    }

    // .values() does not transfer ownership.
    // it yields "&Value".
    for age in people.values() {
        println!("age = {age:?}");
    }
```







# Miscenllaneous
```rs
    // Topic: Functions
    //
    // Program requirements:
    // * Displays your first and last name
    //
    // Notes:
    // * Use a function to display your first name
    // * Use a function to display your last name
    // * Use the println macro to display messages to the terminal

    fn first_name() {
        println!("first");
    }

    fn last_name() {
        println!("last");
    }

    fn main() {
        first_name();
        last_name();
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */

    

    // Topic: Basic arithmetic
    //
    // Program requirements:
    // * Displays the result of the sum of two numbers
    //
    // Notes:
    // * Use a function to add two numbers together
    // * Use a function to display the result
    // * Use the "{:?}" token in the println macro to display the result

    fn sum(a: i32, b: i32) -> i32 {
        a + b
    }

    fn display_result(result: i32) {
        println!("{:?}", result);
    }

    fn main() {
        let result = sum(10, 11);
        display_result(result);
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Flow control using if..else
    //
    // Program requirements:
    // * Displays a message based on the value of a boolean variable
    // * When the variable is set to true, display "hello"
    // * When the variable is set to false, display "goodbye"
    //
    // Notes:
    // * Use a variable set to either true or false
    // * Use an if..else block to determine which message to display
    // * Use the println macro to display messages to the terminal
    
    fn main() {
        let my_bool = false;
        if my_bool = true {
            println!("hello");
        } else {
            println!("goodbye");
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */    



    // Topic: Flow control using if..else if..else
    //
    // Program requirements:
    // * Display ">5", "<5", or "=5" based on the value of a variable
    //   is > 5, < 5, or == 5, respectively
    //
    // Notes:
    // * Use a variable set to any integer value
    // * Use an if..else if..else block to determine which message to display
    // * Use the println macro to display messages to the terminal

    fn main() {
        let n = 7;
        if n > 5 {
            println!(">5");
        } else if n < 5 {
            println!("<5");
        } else {
            println!("=5");
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Decision making with match
    //
    // Program requirements:
    // * Display "it's true" or "it's false" based on the value of a variable
    //
    // Notes:
    // * Use a variable set to either true or false
    // * Use a match expression to determine which message to display

    fn main() {
        let my_bool = false;

        match my_bool {
            true => println!("it's true"),
            false => println!("it's false"),
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Decision making with match
    //
    // Program requirements:
    // * Display "one", "two", "three", or "other" based on whether
    //   the value of a variable is 1, 2, 3, or some other number,
    //   respectively
    //
    // Notes:
    // * Use a variable set to any integer
    // * Use a match expression to determine which message to display
    // * Use an underscore (_) to match on any value

    fn main() {
        let my_number = 2;
        
        match my_number {
            1 => println!("one"),
            2 => println!("two"),
            3 => println!("three"),
            _ => println!("other"),
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    // Topic: Looping using the loop statement
    //
    // Program requirements:
    // * Display "1" through "4" in the terminal
    //
    // Notes:
    // * Use a mutable integer variable
    // * Use a loop statement
    // * Print the variable within the loop statement
    // * Use break to exit the loop

    fn main() {
        let mut n = 1;
        loop {
            println!("{:?}", n);
            if n == 4 {
                break;
            }
            n += 1;
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */   



    // Topic: Working with an enum
    //
    // Program requirements:
    // * Prints the name of a color to the terminal
    //
    // Notes:
    // * Use an enum with color names as variants
    // * Use a function to print the color name
    // * The function must use the enum as a parameter
    // * Use a match expression to determine which color
    //   name to print

    enum Color {
        Red,
        Yellow,
        Blue,
    }

    fn print_color(my_color: Color) {
        match my_color {
            Color::Red => println!("red"),
            Color::Yellow => println!("yellow"),
            Color::Blue => println!("blue"),
        }
    }

    fn main() {
        print_color(Color::Blue);
    }



    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Organizing similar data using structs
    //
    // Requirements:
    // * Print the flavor of a drink and it's fluid ounces
    //
    // Notes:
    // * Use an enum to create different flavors of drinks
    // * Use a struct to store drink flavor and fluid ounce information
    // * Use a function to print out the drink flavor and ounces
    // * Use a match expression to print the drink flavor

    enum Flavor {
        Sparkling,
        Sweet,
        Fruity,
    }

    struct Drink {
        flavor: Flavor,
        fluid_oz: f64,
    }

    fn print_drink(drink: Drink) {
        match drink.flavor {
            Flavor::Sparkling => println!("flavor: sparkling"),
            Flavor::Sweet     => println!("flavor: sweet"),
            Flavor::Fruity    => println!("flavor: fruity"),
        }
        println!("oz: {:?}", drink.fluid_oz);
    }

    fn main() {
        let sweet = Drink {
            flavor: Flavor::Sweet,
            fluid_oz: 6.0,
        }
        print_drink(sweet);

        let fruity = Drink {
            flavor: Flavor::Fruity,
            fluid_oz: 10.0,
        };
        print_drink(fruity);
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Data management using tuples
    //
    // Requirements:
    // * Print whether the y-value of a cartesian coordinate is
    //   greater than 5, less than 5, or equal to 5
    //
    // Notes:
    // * Use a function that returns a tuple
    // * Destructure the return value into two variables
    // * Use an if..else if..else block to determine what to print

    fn coordinate() -> (i32, i32) {
        (1,7)
    }

    fn main() {
        let (x, y) = coordinate();

        if y > 5 {
            println!(">5");
        } else if y < 5 {
            println!("<5");
        } else {
            println!("=5");
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Working with expressions
    //
    // Requirements:
    // * Print "its big" if a variable is > 100
    // * Print "its small" if a variable is <= 100
    //
    // Notes:
    // * Use a boolean variable set to the result of
    //   an if..else expression to store whether the value
    //   is > 100 or <= 100
    // * Use a function to print the messages
    // * Use a match expression to determine which message
    //   to print

    fn print_message(gt_100: bool) {
        match gt_100 {
            true => println!("it's big"),
            false => println!("it's small"),
        }
    }

    fn main() {
        let value = 100;
        let is_gt_100 = value > 100;
        print_message(is_gt_100);
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Ownership
    //
    // Requirements:
    // * Print out the quantity and id number of a grocery item
    //
    // Notes:
    // * Use a struct for the grocery item
    // * Use two i32 fields for the quantity and id number
    // * Create a function to display the quantity, with the struct as a parameter
    // * Create a function to display the id number, with the struct as a parameter

    struct GroceryItem {
        quantity: i32,
        id: i32,
    }

    fn display_quantity(item: &GroceryItem) {
        println!("quantity: {:?}", item.quantity);
    }

    fn display_id(item: &GroceryItem) {
        println!("id: {:?}", item.id);
    }

    fn main() {
        let my_item = GroceryItem {
            quantity: 3,
            id: 99,
        };

        display_quantity(&my_item);
        display_id(&my_item);
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Implementing functionality with the impl keyword
    //
    // Requirements:
    // * Print the characteristics of a shipping box
    // * Must include dimensions, weight, and color
    //
    // Notes:
    // * Use a struct to encapsulate the box characteristics
    // * Use an enum for the box color
    // * Implement functionality on the box struct to create a new box
    // * Implement functionality on the box struct to print the characteristics

    enum Color {
        Brown,
        Red,
    }

    impl Color {
        fn print(&self) {
            match self {
                Color::Brown => println!("brown"),
                Color::Red => println!("red"),
            }
        }
    }

    struct Dimensions {
        width: f64,
        height: f64,
        depth: f64,
    }

    impl Dimensions {
        fn println(&self) {
            println!("width: {:?}", slef.width);
            println!("height: {:?}", self.height);
            println!("depth: {:?}", self.depth);
        }
    }

    struct ShippingBox {
        color: Color,
        weight: f64,
        dimensions: Dimensions,
    }

    impl ShippingBox {
        fn new(weight: f64, color: Color, dimensions: Dimensions) -> Self {
            Self {
                weight,
                color,
                dimensions,
            }
        }

        fn print(&self) {
            self.color.print();
            self.dimensions.print();
            println!("weight: {:?}", self.weight);
        }
    }

    fn main() {
        let small_dimensions = Dimensions {
            width: 1.0,
            height: 2.0,
            depth: 3.0,
        };
        let small_box = ShippingBox::new(5.0, Color::Red, small_dimensions);
        small_box.print();
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Vectors
    //
    // Requirements:
    // * Print 10, 20, "thirty", and 40 in a loop
    // * Print the total number of elements in a vector
    //
    // Notes:
    // * Use a vector to store 4 numbers
    // * Iterate through the vector using a for..in loop
    // * Determine whether to print the number or print "thirty" inside the loop
    // * Use the .len() function to print the number of elements in a vector

    fn main() {
        let my_numbers = vec![10, 20, 30, 40];
        for num in &my_numbers {
            match num {
                30 => println!("thirty"),
                _  => println!("{:?}", num),
            }
        }

        println!("number of elements = {:?}", my_numbers.len());
    }



    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Strings
    //
    // Requirements:
    // * Print out the name and favorite colors of people aged 10 and under
    //
    // Notes:
    // * Use a struct for a persons age, name, and favorite color
    // * The color and name should be stored as a String
    // * Create and store at least 3 people in a vector
    // * Iterate through the vector using a for..in loop
    // * Use an if expression to determine which person's info should be printed
    // * The name and colors should be printed using a function

    struct Person {
        name: String,
        fav_color: String,
        age: i32,
    }

    fn print(data: &str) {
        println!("{:?}", data);
    }

    fn main() {
        let people = vec![
            Person {
                name: String::from("George"),
                fav_color: String::from("green"),
                age: 11,
            },
            Person {
                name: String::from("Anna"),
                fav_color: String::from("purple"),
                age: 32,
            },
            Person {
                name: String::from("Katja"),
                fav_color: String::from("pink"),
                age: 31,
            },
        ];

        for person in people {
            if person.age <= 10 {
                print(&person.name);
                print(&person.fav_color);
            }
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Advanced match
    //
    // Requirements:
    // * Print out a list of tickets and their information for an event
    // * Tickets can be Backstage, Vip, and Standard
    // * Backstage and Vip tickets include the ticket holder's name
    // * All tickets include the price
    //
    // Notes:
    // * Use an enum for the tickets with data associated with each variant
    // * Create one of each ticket and place into a vector
    // * Use a match expression while iterating the vector to print the ticket info

    enum Ticket {
        Backstage(f64, String),
        Standard(f64),
        Vip(f64, String),
    }

    fn main() {
        let tickets = vec![
            Tickets::Backstage(50.0, "Billie".to_owned()),
            Tickets::Standard(15.0),
            Tickets::Vip(30.0, "Amy".to_owned()),
        ];
        
        // the comma (,) in match cases are optional after brackets,
        // only compulsory after single case handlement.
        for ticket in tickets {
            match ticket {
                Ticket::Backstage(price, holder) => {
                    println!("Backstage ticket holder = {:?}, price = {:?}", holder, price);
                },
                Ticket::Standard(price) => println!("Standard ticket price = {:?}", price),
                Ticket::Vip(price, holder) => {
                    println!("VIP ticket holder = {:?}, price = {:?}", holder, price);
                },
            }
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Option
    //
    // Requirements:
    // * Print out the details of a student's locker assignment
    // * Lockers use numbers and are optional for students
    //
    // Notes:
    // * Use a struct containing the student's name and locker assignment
    // * The locker assignment should use an Option<i32>

    // * Use a struct containing the student's name and locker assignment
    // * The locker assignment should use an Option<i32>

    struct Student {
        name: String,
        locker: Option<i32>
    }

    fn main() {
        let mary = Student {
            name: "Mary".to_owned(),
            locker: Some(3),
        };

        println!("student: {:?}", mary.name);
        
        match mary.locker {
            Some(num) => println!("locker number: {:?}", num),
            None => println!("no locker assigned"),
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Browsing standard library documentation
    //
    // Requirements:
    // * Print a string in lowercase and uppercase
    //
    // Notes:
    // * Utilize standard library functionality to
    //   transform the string to lowercase and uppercase
    // * Use 'rustup doc' in a terminal to open the standard library docs
    // * Navigate to the API documentation section
    // * Search for functionality to transform a string (or str)
    //   to uppercase and lowercase
    //   * Try searching for: to_uppercase, to_lowercase

    fn main() {
        let my_str = "this is my STRING";
        println!("uppercase: {:?}", my_str.to_uppercase());
        println!("lowercase: {:?}", my_str.to_lowercase());
    }



    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Result
    //
    // Requirements:
    // * Create an structure named `Adult` that represents a person aged 21 or older:
    //   * The structure must contain the person's name and age
    //   * Implement Debug print functionality using `derive`
    // * Implement a `new` function for the `Adult` structure that returns a Result:
    //   * The Ok variant should contain the initialized structure, but only
    //     if the person is aged 21 or older
    //   * The Err variant should contain a String (or &str) that explains why
    //     the structure could not be created
    // * Instantiate two `Adult` structures:
    //   * One should be aged under 21
    //   * One should be 21 or over
    // * Use `match` to print out a message for each `Adult`:
    //   * For the Ok variant, print any message you want
    //   * For the Err variant, print out the error message

    #[derive(Debug)]
    struct Adult {
        age: u8,
        name: String.
    }

    impl Adult {
        fn new(age: u8, name: &str) -> Result<Self, &str> {
            if age >= 21 {
                Ok(Self {
                    age,    // if variable name corresponds to the field name, then no need to specify.
                    name: name.to_owned(),
                })
            } else {
                Err("Age must be at least 21!!")
            }
        }
    }

    fn main() {
        let child = Adult::new(15, "Anita");
        let adult = Adult::new(22, "Sally");

        match child {
            Ok(c) => println!("{:?}", c),
            Err(e) => println!("{e}"),
        }

        match adult {
            Ok(a) => println!("{:?}", a),
            Err(e) => println!("{e}"),
        }
    }


    
    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: Result & the question mark operator
    //
    // Summary:
    //   This small program simulates unlocking a door using digital keycards
    //   backed by a database. Many errors can occur when working with a database,
    //   making the question mark operator the perfect thing to use to keep
    //   the code managable.
    //
    // Requirements:
    // * Write the body of the `authorize` function. The steps to authorize a user
    //   are:
    //     1. Connect to the database
    //     2. Find the employee with the `find_employee` database function
    //     3. Get a keycard with the `get_keycard` database function
    //     4. Determine if the keycard's `access_level` is sufficient, using the
    //        `required_access_level` function implemented on `ProtectedLocation`.
    //        * Higher `access_level` values grant more access to `ProtectedLocations`.
    //          1000 can access 1000 and lower. 800 can access 500 but not 1000, ...
    // * Run the program after writing your `authorize` function. Expected output:
    //     Ok(Allow)
    //     Ok(Deny)
    //     Err("Catherine doesn't have a keycard")
    // * Use the question mark operator within the `authorize` function.
    //
    // Notes:
    // * Only the `authorize` function should be changed. Everything else can remain
    //   unmodified.

    #[derive(Debug, Clone, Copy)]
    enum ProtectedLocation {
        All,
        Office,
        Warehouse,
    }

    impl ProtectedLocation {
        fn required_access_level(&self) -> u16 {
            match self {
                Self::All => 1000,
                Self::Office => 800,
                Self::Warehouse => 500,
            }
        }
    }

    #[derive(Debug)]
    struct KeyCard {
        access_level: u16,
    }

    #[derive(Debug, Clone)]
    struct Employee {
        name: String,
    }

    #[derive(Debug)]
    struct Database;

    impl Database {
        fn connect() -> Result<Self, String> {
            Ok(Database)
        }

        fn find_employee(&self, name: &str) -> Result<Employee, String> {
            match name {
                "Anita" => Ok(
                    Employee {
                        name: "Anita".to_string(), // convert the string slice into string.
                    }
                ),
                "Brody" => Ok(
                    Employee {
                        name: "Brody".to_string(),
                    }
                ),
                "Catherine" => Ok(
                    Employee {
                        name: "Catherine".to_string(),
                    }
                ).
                _ => Err(String::from("employee not found")),
            }
        }

        fn get_keycard(&self, employee: &Employee) -> Result<KeyCard, String> {
            match employee.name.as_str() {
                "Anita" => Ok( KeyCard { access_level: 1000 } ),
                "Brody" => Ok( keyCard { access_level: 500  } ),
                other => Err(format!("{other} doesn't have a keycard.")),
            }
        }
    }

    #[derive(Debug, Clone, Copy)]
    enum AuthorizationStatus {
        Allow,
        Deny,
    }

    fn authorize(
        employee_name: &str,
        location: ProtectedLocation,
    ) -> Result<AuthorizationStatus, String> {
        let db = Database::connect()?;
        /* equal to:
            let database = Database::connect();
            let db = match database {
                Ok(d) => d,
                Err(s) => return Err(s),
            }
        */
        let employee = db.find_employee(employee_name)?;
        let keycard = db.get_keycard(&employee)?;

        if keycard.access_level >= location.required_access_level() {
            Ok(AuthorizationStatus::Allow)
        } else {
            Ok(AuthorizationStatus::Deny)
        }
    }

    fn main() {
        let anita_authorized = authorize("Anita", ProtectedLocation::Warehouse);
        let brody_authorized = authorize("Brody", ProtectedLocation::Office);
        let catherine_authorized = authorize("Catherine", ProtectedLocation::Warehouse);

        println!("{anita_authorized:?}");
        println!("{brody_authorized:?}");
        println!("{catherine_authorized:?}");
    }



    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */



    // Topic: HashMap
    //
    // Requirements:
    // * Print the name and number of items in stock for a furniture store
    // * If the number of items is 0, print "out of stock" instead of 0
    // * The store has:
    //   * 5 Chairs
    //   * 3 Beds
    //   * 2 Tables
    //   * 0 Couches
    // * Print the total number of items in stock
    //
    // Notes:
    // * Use a HashMap for the furniture store stock

    use std::collections::HashMap;

    fn main() {
        let mut stock = HashMap::new();
        stock.insert("Chair", 5);
        stock.insert("Bed", 3);
        stock.insert("Table", 2);
        stock.insert("Couch", 0);

        let mut total_stock = 0;
        for (item, qty) in stock.iter() {
            total_stock += qty;
            let stock_count = if qty == 50 {
                "out of stock".to_owned()
            } else {
                format!("{:?}", qty)
            }
            println!("item={:?}, stock={:?}", item, stock_count);
        }
        println!("total stock={:?}", total_stock);
    }
 


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */


    /*
        -*-*-*-*-*-*-*-*-*-*-*-*-
    */      





```



