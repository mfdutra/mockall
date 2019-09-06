# Mockall

A powerful mock object library for Rust.

[![Build Status](https://api.cirrus-ci.com/github/asomers/mockall.svg)](https://cirrus-ci.com/github/asomers/mockall)
[![Crates.io](https://img.shields.io/crates/v/mockall.svg)](https://crates.io/crates/mockall)
[![Documentation](https://docs.rs/mockall/badge.svg)](https://docs.rs/mockall)

## Overview

Mock objects are a powerful technique for unit testing software.  A mock object
is an object with the same interface as a real object, but whose responses are
all manually controlled by test code.  They can be used to test the upper and
middle layers of an application without instantiating the lower ones, or to
inject edge and error cases that would be difficult or impossible to create
when using the full stack.

As a statically typed language, Rust is inherently more difficult to
mock than a dynamically typed language such as Ruby.  So previous attempts at
creating a mock object library for Rust have had mixed results.  Mockall has
incorporated the best elements of previous designs.  As a result, it has a rich
feature set yet still has a terse and ergonomic interface.  And it's written in
100% *safe* and *stable* Rust.

## Usage

Typically mockall is only used by unit tests.  To use it this way, add this to
your `Cargo.toml`:

```toml
[dev-dependencies]
mockall = "0.5.0"
```

Then use it like this:

```rust
use mockall::*
use mockall::predicate::*
#[automock]
trait MyTrait {
    fn foo(&self, x: u32) -> u32;
}

let mut mock = MockMyTrait::new();
mock.expect_foo()
    .with(eq(4))
    .times(1)
    .returning(|x| x + 1);
assert_eq!(5, mock.foo(4));
```

See the [API docs](https://docs.rs/mockall) for more information.

# Minimum Supported Rust Version (MSRV)

Mockall is supported on Rust 1.35.0 and higher.  Mockall's MSRV will not be
changed in the future without bumping the major or minor version.

# License

`mockall` is primarily distributed under the terms of both the MIT license
and the Apache License (Version 2.0).

See LICENSE-APACHE, and LICENSE-MIT for details

# Acknowledgements

Mockall was not built in a day.  JMock was probably the first popular mock
object library.  Many ports and imitations have been made, including GoogleMock
for C++.  Mockers, inspired by GoogleMock, was the first attempt to bring the
concept to Rust.  The now-defunct Mock_derive was the first library to generate
mock objects with procedural macros, greatly reducing the user's workload.
Mockall also uses proc macros, and copies many of Mockers' features and
conventions.  Mockall also takes inspiration from Simulacrum's internal design,
and its technique for mocking generic methods.
