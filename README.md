cargo-geiger ☢
===============

A program that list statistics related to usage of unsafe Rust code in a Rust
crate and all its dependencies.

This cargo plugin is based on the code from two other projects:
<https://github.com/icefoxen/cargo-osha> and
<https://github.com/sfackler/cargo-tree>.


Usage
-----

1. `cargo install cargo-geiger`
2. Navigate to the same directory as the Cargo.toml you want to analyze.
3. `cargo geiger`


Output example
--------------

![Example output](https://user-images.githubusercontent.com/3704611/42893683-54f16930-8ab5-11e8-87a5-785fe4a1d5d9.png)


Why even care about unsafe Rust usage?
--------------------------------------

When and why to use unsafe Rust is out of scope for this project, it is simply
a tool that provides information to aid auditing and hopefully to guide
dependency selection. It is however the opinion of the author of this project
that __libraries choosing to abstain from unsafe Rust usage when possible should
be promoted__.

This project is an attempt to create pressure against __unnecessary__ usage of
unsafe Rust in public Rust libraries.


Why the name?
-------------

<https://en.wikipedia.org/wiki/Geiger_counter>

Unsafe Rust and ionizing radiation have something in common, they are both
inevitable in some situations and both should preferably be safely contained!


Known issues
------------

- Unsafe code inside macros are not detected. Needs macro expansion(?).
- Unsafe code generated by `build.rs` are probably not detected.
- Will continue on syn parse errors. Should default to exit on the first error(?).
- More on the github issue tracker.


Roadmap
-------

- There should be no false negatives. All unsafe code should be identified.
- An optional whitelist file at the root crate level to specify crates that are
  trusted to use unsafe (should only have an effect if placed in the root
  project).
- Refactoring and general cleanup.
- Replace all remaining panics with Result based errors handling.
- Additional output formats.


Changelog
---------

### Unreleased
 - Merge pull request #40 from jiminhsieh/rust-2018. Use Rust 2018 edition.

### 0.4.2
 - Merge pull request #38 from anderejd/updated-deps. Updated deps and fixed
   build errors.

 - __BUGFIX__: Merge pull request #33 from ajpaverd/windows_filepaths.
   Canonicalize file paths from walker.

### 0.4.1
 - Merge pull request #28 from alexmaco/deps_upgrade. fix build on rust 1.30:
   upgrade petgraph to 0.4.13

 - Merge pull request #29 from alexmaco/invalid_utf8_source. fix handling
   source files with invalid utf8: lossy conversion to string

### 0.4.0
 - Filters out tests by default. Tests can still be included by using
   `--include-tests`. The test code is filted out by looking for the attribute
   `#[test]` on functions and `#[cfg(test)]` on modules.

### 0.3.1
 - Some bugfixes related to cargo workspace path handling.
 - Slightly better error messages in some cases.

### 0.3.0
 - Intercepts `rustc` calls and reads the `.d` files generated by `rustc` to
   identify which `.rs` files are used by the build. This allows a crate that
   contains `.rs` files with unsafe code usage to pass as "green" if the unsafe
   code isn't used by the build.
 - Each metric is now printed as `x/y`, where `x` is the unsafe code used by the
   build and `y` is the total unsafe usage found in the crate.
 - Removed the `--compact` output format to avoid some code complexity. A new
   and better compact mode can be added later if requested.

### 0.2.0
 - (alexmaco) Table based default output format. Old format still available by
   `--compact`.

### 0.1.x
 - Initial experimental versions.
 - Mostly README.md updates.



