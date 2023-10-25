# Resources for Arduino Development in Rust

## Libraries

why so many

There are a couple libraries that you might use.

They are:

- [`avr-hal`]: Hardware Abstraction Layer for AVR microcontrollers and common boards (for example Arduino)
- [`avr-rust`]/[`ruduino`] Reusable components for the Arduino Uno.

These three projects all effectively do the same thing at different abstraction levels.

### [`avr-hal`]

This is my recommendation for developing an Arduino application in Rust. [`avr-hal`] has the low level control needed for real projects but provides an easy to write and reasonably safe API to interact with the system.

What is **HAL**?

> HAL Design Patterns
>
> This is a set of common and recommended patterns for writing hardware abstraction layers (HALs) for microcontrollers in Rust. These patterns are intended to be used in addition to the existing Rust API Guidelines when writing HALs for microcontrollers.

Simply, HALs are abstractions over embedded hardware. The most basic is the [embdedded-hal](https://github.com/rust-embedded/embedded-hal) crate which defines many traits applicable to most embedded systems. [`avr-hal`] is a HAL that abstracts many AVR boards like Arduino boards.

In my opinion, this seems like the *correct* solution to Arduino Rust development. Additionally, [`avr-hal`] has lots of [examples](https://github.com/Rahix/avr-hal/tree/main/examples).

### [`ruduino`]

[`ruduino`] seems to be a separate, more high level take compared to avr-hal. While there is still access to registers, serial, and timers, etc., the overall feel is very rustian and hand-holdy.

For example, a timer configuration:

```rust
const DESIRED_HZ_TIM1: f64 = 2.0;
const TIM1_PRESCALER: u64 = 1024;
const INTERRUPT_EVERY_1_HZ_1024_PRESCALER: u16 =
    ((ruduino::config::CPU_FREQUENCY_HZ as f64 / (DESIRED_HZ_TIM1 * TIM1_PRESCALER as f64)) as u64 - 1) as u16;

timer1::Timer::new()
    .waveform_generation_mode(timer1::WaveformGenerationMode::ClearOnTimerMatchOutputCompare)
    .clock_source(timer1::ClockSource::Prescale1024)
    .output_compare_1(Some(INTERRUPT_EVERY_1_HZ_1024_PRESCALER))
    .configure();
```

Timer becomes a struct that can be have it's properties modified using method chaining. In contrary, avr-hal gives you organized register definitions but has you write each register individually.

Additionally it appears the [`ruduino`] is only for the Arduino Uno.

### [`avr-rust`]

[`avr-rust`] refers to the fork of the Rust compiler to add AVR support. Nowadays, AVR support is included in Rust nightly so these projects are deprecated e.g. [rust-legacy-fork](https://github.com/avr-rust/rust-legacy-fork).

## Projects

- [CHIP-8 interpreter by Gergo Erdi](https://github.com/gergoerdi/rust-avr-chip8-avr)
- [Keyboard firmware by Wez Furlong](https://github.com/wez/flutterby-rs)
- [Demo for Arduboy](https://github.com/simon-i1-h/arduboy-hello-rs)
- [Dockerized avr-rust toolchain by Douglas Campos](https://github.com/qmx/docker-avr-rust)
- [LED Blink Example](https://github.com/avr-rust/blink/)

[`avr-hal`]: https://github.com/Rahix/avr-hal/tree/main
[`avr-rust`]: https://github.com/avr-rust
[`ruduino`]: https://github.com/avr-rust/ruduino
