A library for the Raspberry Pi Sense HAT LED Screen
====================================================

[![crates.io](https://img.shields.io/crates/v/sensehat-screen.svg)](https://crates.io/crates/sensehat-screen)
[![docs](https://docs.rs/sensehat-screen/badge.svg)](https://docs.rs/sensehat-screen)
[![Build Status](https://travis-ci.org/saibatizoku/sensehat-screen-rs.svg?branch=master)](https://travis-ci.org/saibatizoku/sensehat-screen-rs)


The [Raspberry Pi Sense HAT](https://www.raspberrypi.org/products/sense-hat/) has an 8×8 RGB LED matrix that provides its own driver for the Linux framebuffer.

This library provides a thread-safe, strong-typed, high-level API for the LED matrix, treating it as you would any other screen on a Linux box.

# Requirements

This crate supports Rust stable version v1.26.0 and higher, and is tested on nightly continuously.

# Changelog

For a detailed summary of how changes are incorporated into the code, we keep a [CHANGELOG](./CHANGELOG.md).

# Usage

To use this crate with the default features, add this to your `Cargo.toml`:
```cargo
[dependencies]
sensehat-screen = "0.1"
```

or, to manually specify the features::

```cargo
[dependencies]
sensehat-screen = { version = "0.1", default-features = false, features = ["fonts"] }
```

Then you can use it with your crate:

```rust
extern crate sensehat_screen

use sensehat_screen::{FontCollection, FrameLine, PixelColor, Screen};
```

# Documentation

You can find the documentation for the latest release at: [https://docs.rs/sensehat-screen](https://docs.rs/sensehat-screen).

It contains a general description of the types contained in the library, it is a good to place to get familiar with the methods available.


# Examples

## Source Code Examples

You can find working examples in the source code.

* [Blinking RGB shapes](./examples/blink.rs)
* [A letter from ゆにち (Yunichi) to Toño](./examples/letter.rs)
* [Rotate demo](./examples/rotate.rs)
* [Offset demo](./examples/offset.rs)
* [Clip demo](./examples/clip.rs)

## Basic Example

The following program shows how to:

* Open the framebuffer file-descriptor for the LED matrix screen (`screen`)
* Define a pixel color (`red_pixel`)
* Define a slice of pixel colors that represents the screen (`all_64_pixels`)
* Turn that slice into a valid pixel frame
* Show the frame on the screen

```rust
extern crate sensehat_screen;

use sensehat_screen::{PixelFrame, PixelColor, Screen};

fn main() {
    let mut screen = Screen::new("/dev/fb1")
        .expect("Could not open the framebuffer for the screen");

    let red_pixel = PixelColor::new(255, 0, 0); // The pixel color's RGB components are each in the range of 0 <= c < 256.

    let all_64_pixels = &[red_pixel; 64];   // A single vector of 8 x 8 = 64 pixel colors (rows are grouped by chunks of 8)

    let all_red_screen = PixelFrame::from_pixels(&all_64_pixels); // a screen frame

    screen.write_frame(&all_red_screen.frame_line()); // show the frame on the LED matrix
}
```


# Features

## `default` features

By default, the `basic`, and `linux-framebuffer` features are included.

## `basic` features

A set of features that don't require the hardware. This is mostly code that you will want to use if you are writing a simulator/emulator/etc. It includes, the `fonts`, `offset`, `rotate`, `clip`, `scroll`, and `serde-support` features.

### `fonts`

In `default`. A collection of legacy 8x8 fonts, renderable on the LED matrix.

See [this](https://en.wikipedia.org/wiki/Endianness#Current_architectures) for more information.

### `offset`

In `default`. Support for offsetting the `PixelFrame` left/right/top/bottom. Requires `clip`.

### `rotate`

In `default`. Support for rotating `PixelFrame`s by 90-degree steps.

### `clip`

In `default`. Support for combining, and clipping two `PixelFrame`s onto a single frame.

### `scroll`

In `default`. Support for joining a collection of `PixelFrame`s into a single `Scroll`. Requires `clip`.

### `serde-support`

In `default`. Enables support for serialization/deserialization with `serde`.

## `linux-framebuffer` feature

In `default`. Use the Linux framebuffer to write to the LED matrix.

# Extra features

## `big-endian`

Uses big-endian format, suitable for non-AMD64/x86-64 processors. This is used when encoding/decoding 16-bit RGB565 to/from 24-bit RGB.

Feature Wish List
=================
* [X] `linux-framebuffer` - In `default`. Use the Linux framebuffer to write to the LED matrix.
* [X] `fonts` - In `default`. A collection of legacy 8x8 fonts, renderable on the LED matrix.
* [X] `offset` - In `default`. Support for offsetting the `PixelFrame` left/right/up/down.
* [X] `rotate` - In `default`. Support for rotating `PixelFrame`s by 90-degree steps.
* [X] `clip` - In `default`. Support for combining, and clipping two `PixelFrame`s onto a single frame.
* [X] `scroll` - In `default`. Support for joining a collection of `PixelFrame`s into a single `Scroll`. Requires `clip`.
* [X] `serde-support` - In `default`. Enables support for serialization/deserialization with `serde`.
* [X] `big-endian` - Uses big-endian format, suitable for non-AMD64/x86-64 processors.

# Contribute

Please do contribute! Issues and pull requests are welcome.

Thank you for your help improving software one changelog at a time!
