# rust_intro

- **tools**:
    - `rustc`: rust compiler, built over llvm, awesome compile time error/warning reporting
    - `cargo`: behaves both as a package manager (eg. conda) and build configurator (eg. cmake), makes use of "cargo.toml" to manage the project & dependent packages (aka `crate` in rust)
        - `cargo new hello_world [--bin, --lib]`: create a executable or library
        - `cargo [build, publish]`: build/publish project
        - `cargo [test, bench]`: run tests/benchmarks
        - `cargo [search, install, update, uninstall]`: manage dependencies
        
- **declarations**:
    - **variables**: All variables are const by defult, `mut` keyword makes variable mutable. Python like type declarations. Basic types are passed by value.
    ```
    let x = 3   // same as auto x = 3 in c++
    let x: bool = true;  // type hints, implicitly infered when possible
    let x = 34;   // type isize
    let x: u8 = 34u8;   // unsigned int8
    let x = 34i64;   // signed long long int
    let x = 34f32;   // float
    let x = 34f64;   // double
    let mut y = x as u64  // casting
    ```
    
    - **functions**: Type declarations is mandatory, except for void returns. 
    ```
    fn sqr_in_place(a: &i64) { // takes a reference to the value
        *a = a * a;
    }
    fn sqr(a: f32) -> f32 {
        a * a  // `return` keyword can be avoided with skipping the semicolon
    }
    ```
    
    - **structs**: 
    member type declaraion is mandatory, class methods are defined externally in impl context.
    All structs are passed by reference by default. If you skip & (reference) when instantiating a struct, then it will be treated as a std::unique_ptr, can't use after move. No constructor/destructor.
    ```
    struct S {
        field1: i32,
        field2: i32
    }
    
    impl S {
       fn get_field1(&self) {  // self stands for c++ this, no type hints are necessary
          // self.field1 += 1; --> error, self is const here, can't increment
       }
       fn inc(mut& self) { 
           self.field1 += 1;
       }
       fn make_S(a: i32, b: i32) -> S {  // if you skip self, then method is static
           return S{field1: a, field2: b};
       }
       fn make_empty_S() -> S {
           return Self::make_S(0, 0);  // Self stands for implemented class, i.e. S in this case
           // return S{0, 0}; --> the same as above
       }
    }
    ```
    - tuples:
    ```
    fn foo() -> (i32, str) {
        return (3, "asd")
    }
    fn bar() {
       let f = foo()
       println!("{}", f.0)  // Note how to access a tuple
       x, y = foo()  // unfolding
       println!("{num} {text}", num=x, text=y)
    }
    ```
    
    - **enums**:
    ```
    enum E1 {
        Var1,
        Var2,
        Var3
    }
    ```
    
- **control flow**: optional paranthesis on conditions, braces are mandatory
    - `if`: 
    ```
    if n < 0 {
        print!("{} is negative", n);
    } else if n > 0 {
        print!("{} is positive", n);
    } else {
        print!("{} is zero", n);
    } 
    ```

    - loops:
    ```
    for i in 0..all.len() {
        println!("{}: {}", i, all[i]);
    }
    ```
    ```
    for (i, a) in all.iter().enumerate() {
        println!("{}: {}", i, a);
    }
    ```
    ```
    for a in all.iter() {
        println!("{}", a);
    }
    ```
    ```
    let mut x = 10;
    while x > 0 {
        println!("Current value: {}", x);
        x -= 1;
    }
    ```
    
    - `match`: substitute for `switch` in c++, but more talented
    ```
    match x {
        0 | 1 | 10 => println!("x is one of zero, one, or ten"),
        y if y < 20 => println!("x is less than 20, but not zero, one, or ten"),
        y if y == 200 => println!("x is 200 (but this is not very stylish)"),
        _ => {}  // default case
    }
    ```
    
- **memory management**:
