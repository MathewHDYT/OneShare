---
layout: default
title: Keywords
parent: Rust
grand_parent: Notes
---

## Keywords
Contains all keywords currently reserved by Rust and their respective use.

### Raw identifiers
Keywords can still be used as function names and so on, when adding r#, to the keyword name both when defining and then when using it.

```rust
fn r#match() -> bool {
    return true;
}

fn main() {
    assert!(r#match());
} 
```

### Keyword list
**KEYWORD**  | **USE**                                                              |
-------- | ---------------------------------------------------------------- |
as       | Perform primitive casting, disambiguate the specific trait containing an item, or rename items in use statements                                                             |
await    | Suspend execution until the result of a Future is ready          |
break    | Exit a loop immediately                                          |
const    | Define constant items or constant raw pointers                   |
continue | Continue to the next loop iteration                              |
crate    | In a module path, refers to the crate root                       |
dyn      | Dynamic dispatch to a trait object                               |
else     | Fallback for if and if let control flow constructs               |
enum     | Define an enumeration                                            |
extern   | Link an external function or variable                            |
false    | Boolean false literal                                            |
fn       | Define a function or the function pointer type                   |
for      | Loop over items from an iterator, implement a trait, or specify a higher-ranked lifetime                                                                      |
if       | Branch based on the result of a conditional expression           |
impl     | Implement inherent or trait functionality                        |
in       | Part of for loop syntax                                          |
let      | Bind a variable                                                  |
loop     | Loop unconditionally                                             |
match    | Match a value to patterns                                        |
mod      | Define a module                                                  |
move     | Make a closure take ownership of all its captures                |
mut      | Denote mutability in references, raw pointers, or pattern bindings                                                                      |
pub      | Denote public visibility in struct fields, impl blocks, or modules                                                                       |
ref      | Bind by reference                                                |
return   | Return from function                                             |
Self     | A type alias for the type we are defining or implementing        |
self     | Method subject or current module                                 |
static   | Global variable or lifetime lasting the entire program execution |
struct   | Define a structure                                               |
super    | Parent module of the current module                              |
trait    | Define a trait                                                   |
true     | Boolean true literal                                             |
type     | Define a type alias or associated type                           |
union    | Define a union; is only a keyword when used in a union declaration                                                                   |
unsafe   | Denote unsafe code, functions, traits, or implementations                                                               |
use      | Bring symbols into scope                                         |
where    | Denote clauses that constrain a type                             |
while    | Loop conditionally based on the result of an expression          |
abstract | No functionality yet                                             |
become   | No functionality yet                                             |
box      | No functionality yet                                             |
do       | No functionality yet                                             |
final    | No functionality yet                                             |
macro    | No functionality yet                                             |
override | No functionality yet                                             |
priv     | No functionality yet                                             |
try      | No functionality yet                                             |
typeof   | No functionality yet                                             |
unsized  | No functionality yet                                             |
virtual  | No functionality yet                                             |
yield    | No functionality yet                                             |
