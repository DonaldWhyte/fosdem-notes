# Demistifying Rust Parsing

Think of parsing as code as data &mdash; it contains extractable metainfo.

This can be used for more than compiling the source code! It can also be used for static analysis.

Compilation pipeline -- code to tokens, create token trees, produce ASTs.

AST = abstract syntax tree / data structure .

AST can be used for code generation! Produce Rust AST and use it to convert the Rust code to another language.

Or scan library code, extract all pub functions and generate FFI / glue code to generate bindings to Rust code for other languages.

Useful for linting too! Can also transform / refactor your code.

Macros actually operate on tokens in a similar fashion to ASTs. 

### How do you Uue ASTs in Rust?

`libsyntax`, which is internal part of rustc compiler. Availability only in _nightly_.

`syntex` is a port of `libsyntax` that can work on stable branch of Rust compiler. However, this is deprecated...only really used in legacy libraries.

A non-deprecated and stable crate is `syn`.

Generate Rust code using `ast::Builder`. Uses builder pattern to construct tokens one-by-one, sequentially.

### Quasi-quotation

Code generated based on a  template. `quote` crate provides handy macros. Easier that using `ast::Builder` directoy.

### Generate External Language Bindings

Can do this as well, by scanning Rust code and genearting Java/python/etc. equivalents.

### The Role of the Macros

Macros provide a subset of the full AST builder. But, they can be used in a way that AST building can't be used. Macros can rewrite code being compiled while it's being compiled. e.g. macros take an AST and transform it into another AST.

AST builders can't do that.
