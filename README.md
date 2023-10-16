# adxl345_driver2

This is a hardware driver for [ADXL345] and [ADXL346] type 3-Axis
Digital Accelerometers written in [Rust] using [embedded-hal] `I2c` and `Spi` traits.
That means it runs on all hardware layers that implement the [embedded-hal] traits.

Although the name says adxl345 the driver should also work with the [ADXL346]
device as well since the only difference between them is the physical packaging
and not the internal workings.

This crate is a fork of the adxl345_driver crate.
This crate still is API compatible with the original adxl345_driver crate.

## Getting Started

You will need to have a recent version of [Rust] installed.

This crate supports all hardware abstraction layers that implement the
[embedded-hal] `I2c` and `Spi` traits. That includes the [rppal] driver for
the Raspberry Pi. But it is not restricted to that platform. Platforms
that are known to work correctly are the Raspberry Pi with [rppal] and the
ESP32 with [esp-idf-hal]. But this crate is not restricted to these HAL layers.

### Using The Crate

You can use `cargo add` to add the driver to your `Cargo.toml`

```sh
cargo add adxl345_driver2
```

or you can manually add the driver to your `Cargo.toml`:

```toml
[dependencies]
adxl345_driver2 = "1"
```

## Examples

You will find examples in the `examples` directory.
The Raspberry Pi I²C and SPI examples are available.

To build the I²C example start by clone this project somewhere on your Raspberry
Pi:

```sh
git clone https://git.bues.ch/git/adxl345_driver2
```

Next execute the follow to build the example:

```sh
cd adxl345_driver2
cargo build --example i2c
```

And finally execute the example:

```sh
sudo ./target/debug/examples/i2c
```

You should see the series of x, y, z values displayed in the terminal if your
device has been hooked up using the primary I²C that the example expects.

Output example:

```console
axis: {'x': 1.6083, 'y': 0.0392, 'z': 8.7868} m/s²
axis: {'x': 1.6867, 'y': 0.1177, 'z': 8.7868} m/s²
axis: {'x': 1.6475, 'y': 0.1177, 'z': 8.8260} m/s²
...
```

## no_std

This crate can be used in `no_std` environments.
Just enable the `no_std` feature, if you want to build without `std` library.

`no_std` currently only disables the implementation of `std::error::Error` for `AdxlError`.

```toml
[dependencies]
adxl345_driver2 = { version = "1", features = ["no_std"] }
```

## TODO

- Most of the driver is currently implemented as `provided` methods in a chain of traits.
  That is more complicated than it needs to be.
  And it also exposes the low level bus access routines to the user API.
  It would be better to have two trait based low level bus access structs for I²C and SPI
  and then implement a driver struct that is generic over this trait.
  The problem with this change is that it is a user visible API change.
  Therefore, we'll keep the old implementation for now.

## Licenses

All code files are available under the `MIT` license.
You can find a copy of the license in the `LICENSE` file.

All additional documentation like this `README` is licensed under a
[CC-BY-SA / Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).

[Rust]: https://www.rust-lang.org/
[embedded-hal]: https://crates.io/crates/embedded-hal
[ADXL345]: https://www.analog.com/media/en/technical-documentation/data-sheets/ADXL345.pdf
[ADXL346]: https://www.analog.com/media/en/technical-documentation/data-sheets/ADXL346.pdf
