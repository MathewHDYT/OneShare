---
layout: default
title: Cargo
parent: Rust
grand_parent: Notes
---

## Cargo
### Useful Commands
| Command                    | Does                                                                                                                |
|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| `cargo --version`          | Returns the currently installed version number of rust if installed (comes with cargo preinstalled)	               |
| `cargo new <projectname>`  | Creates a new project and git repo with the given name. <br/> **Add --vcs=git to not create a new git repo**        |
| `cargo build`              | Creates an executable file in **target/debug/<projectname>.exe**. <br/> Run with **.\target\debug\<projectname>.exe**. <br/> To build for release use **--release**                                                                                                                                      |
| `cargo run`                | Both compiles code and runs the resulting executable                                                                |
| `cargo update`             | Searches for newer version of the specified dependencies and updates the **Cargo.lock** file accordingly. <br/> (To update to the next minor version 0.X.0, the **Cargo.lock** file has to be changed by hand)                                                         |
| `cargo doc --open`         | Locally builds a documentation of all included dependencies and opens them in the browser                           |

### Cargo.toml
Is the build file for rust and includes the package name, version and edition. As well as optional dependencies and everything that dependency is fetched from [Crates.io](https://crates.io/) and built as well when we build our package.

```toml
[package]
name = "example_project"
version = "0.1.0"
edition = "2021"
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
[dependencies]
rand = "^0.8.3"
```

`"0.8.3" = "^0.8.3"`: All version from **0.8.3** up to but not including **0.9.0** (*might contain breaking API changes*)

### Cargo.lock
When rebuilding a project, this file will be used to get the saved versions instead of trying to figure out the newest versions again. Meaning the package will stay in the specified version until it is explicitly upgraded. With `cargo update` explained above.
