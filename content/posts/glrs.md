+++
title = "Improving the Rust OpenGL experience with macros"
date = 2024-01-21

[extra]
toc = true
+++

## Brains are faillible

Much to the dismay of innocent programmers new and old, graphics programming on
the GPU is inherently painful.
This is easily traced back to the hidden state and optimizations of the OpenGL
constructs the programmer has to be mindful of when using the API.

So, what do we do to cope with that? We write abstractions. Lots and lots of
abstractions, with the drawbacks and additional maintenance time that
inevitably come with them.

But abstractions are usually still limited to slightly type-checked APIs and
nicer runtime errors, meaning the developer still has to be very careful about
many things.

Unfortunately, internal OpenGL state errors are not the only issues one runs
into: anyone who has worked with graphics APIs could probably tell you stories
of debugging buffer data for hours on end. Be it mismatches between CPU and GPU
padding of structs or improper offsets in OpenGL call, data passing errors are
all too common.

## Macros, but the procedural kind

Macros are nice. Always have been. Sure they're a bit wonky and limited in C,
but they're still our friends most of the time.

For those unaware, Rust takes macros a step further. Even the "regular" Rust
macros, called declarative macros, are extremely powerful in comparison to
their C counterpart. Probably the most immediately striking differences between
Rust declarative macros and C pre-processor macros is the control meta type
over the input values and the ability for repetition. It's too broad of a topic
for me to cover in this post, but I'd recommend checking them out if you're
still unsure about learning Rust.

The other type of macro in Rust is the procedural macro. Instead of being a
straight-forward AST replacement like declarative macros, procedural macros
are defined in regular Rust, allowing you to manipulate the AST at compile-time
with regular Rust.
This extremely powerful tool is what enables many of Rust's best features.
Derives? That's procedural macros. Attributes? Procedural macros as well. That
one crate with the insane JSX-like syntax macro you learned about a month ago
but haven't gotten around to using? Again, procedural macros.

## A statically checked future

I recently started working on a crate called [glrs]. The objective? Build Rust
structs at compile-time from their definitions in GLSL files, with all the
proper layout-aware padding one might expect.

[glrs]: https://crates.io/crates/glrs

This crate is a work in progress, but has already served me well for academic
projects. From this simple use case, I realized I could likely statically check
and use much more from my shaders: uniforms, SSBOs, attributes, etc...
I plan to start working on the crate in the coming month and hope to create a
better experience for Rust graphics programming, hopefully outweighing the
negative aspects of the workarounds necessary to interact with the OpenGL API.
