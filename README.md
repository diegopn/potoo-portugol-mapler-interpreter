# Portugol Mapler Interpreter

Independent Rust interpreter for the Portugol Mapler syntax documented by the
[Projeto-Mapler Portugol repository](https://github.com/Projeto-Mapler/portugol)
and the [Mapler examples](https://portugol.sourceforge.io/exemplos/).

It implements the documented variable types and vectors, `escrever`/`ler`,
arithmetic and logical expressions, `se`/`senao`, `para`/`enquanto`/`repita`,
and declared modules. It exposes source-located diagnostics, type checking,
runtime limits, input/output hooks, and debugger events for host applications.

The original Mapler IDE stores source files as `.txt`. This interpreter does
not define an official file extension. The `.mapler` suffix used by Potoo
follows [DesignLiquido's Mapler project](https://github.com/DesignLiquido/mapler)
and is not attributed to the original IDE; syntax and behavior follow
Projeto-Mapler's documentation above.

The crate exports a Rust library and includes a standalone terminal command-line runner. Install Rust
1.93 or later, clone this repository, and add the crate to a Rust application
using a Git dependency pinned to commit `147b5b88bd4aa8833bc7966e048d6f835fc3c7ce`:

```toml
[dependencies]
potoo-mapler-interpreter = { git = "https://github.com/diegopn/potoo-portugol-mapler-interpreter.git", rev = "147b5b88bd4aa8833bc7966e048d6f835fc3c7ce" }
```

Implement `RuntimeIo` to provide input and output, then call `execute(source,
&mut io)`. The public API and host callbacks are documented in the crate
source. For a local checkout, run the interpreter's tests with `cargo test`.

## Original language references

- [Mapler user manual](https://portugol.sourceforge.io/manuais.html)
- [Projeto-Mapler syntax reference](https://github.com/Projeto-Mapler/portugol)
- [Mapler examples](https://portugol.sourceforge.io/exemplos/)

These are the original language references for compatibility work. This repository provides an independent Rust implementation.

## Source example

```mapler
variaveis
inicio
  escrever "Olá, Mapler!";
fim.
```

## Development

Run these commands from the repository root:

```sh
cargo fmt --all -- --check
cargo check
cargo test
```

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).

## Project policies

- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Standalone command-line runner

The `potoo-mapler` command runs one source file without Potoo IDE. For example:

```sh
potoo-mapler example.mapler
```

The terminal runner provides standard input/output and continues through debugger events without an interactive debugger.

## Install a prebuilt runner

Download the archive for your system and `SHA256SUMS` from the [latest GitHub Release](https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/latest). Binary releases are not published yet. The planned first release will be tagged `v0.1.0`; its release workflow is configured to produce these asset names:

| System | Archive |
| --- | --- |
| Linux x86_64 | `potoo-portugol-mapler-interpreter-v0.1.0-x86_64-unknown-linux-gnu.tar.gz` |
| Windows x86_64 | `potoo-portugol-mapler-interpreter-v0.1.0-x86_64-pc-windows-msvc.zip` |
| macOS Intel | `potoo-portugol-mapler-interpreter-v0.1.0-x86_64-apple-darwin.tar.gz` |


Current CI and release targets are Linux x86_64, Windows x86_64, and macOS Intel (x86_64). Apple silicon (arm64) is not supported in this release; support may be added in a future version.

### Linux x86_64

```sh
asset=potoo-portugol-mapler-interpreter-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
release=https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/download/v0.1.0
curl -fLO "$release/$asset"
curl -fLO "$release/SHA256SUMS"
sha256sum "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "potoo-portugol-mapler-interpreter-v0.1.0-x86_64-unknown-linux-gnu/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
potoo-mapler example.mapler
```

Add it for the current shell with `export PATH="$HOME/.local/bin:$PATH"`; place that line in your shell profile to keep it for future sessions.

### Windows x86_64 (PowerShell)

```powershell
$asset = "potoo-portugol-mapler-interpreter-v0.1.0-x86_64-pc-windows-msvc.zip"
$release = "https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/download/v0.1.0"
Invoke-WebRequest "$release/$asset" -OutFile $asset
Invoke-WebRequest "$release/SHA256SUMS" -OutFile SHA256SUMS
Get-FileHash ".\$asset" -Algorithm SHA256 # compare with the matching SHA256SUMS entry
Expand-Archive ".\$asset" -DestinationPath .\potoo-portugol-mapler-interpreter
.\potoo-portugol-mapler-interpreter\potoo-portugol-mapler-interpreter-v0.1.0-x86_64-pc-windows-msvc\potoo-mapler.exe .\example.mapler
```

To run `potoo-mapler.exe` from any folder, add its extracted directory to your user `PATH` in Windows Environment Variables.

### macOS Intel

This release supports Intel Macs only. Apple silicon is not supported in this version.

```sh
asset=potoo-portugol-mapler-interpreter-v0.1.0-x86_64-apple-darwin.tar.gz
release=https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/download/v0.1.0
curl -fLO "$release/$asset"
curl -fLO "$release/SHA256SUMS"
shasum -a 256 "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
potoo-mapler example.mapler
```

Add it for the current shell with `export PATH="$HOME/.local/bin:$PATH"`; place that line in `~/.zprofile` to keep it for future macOS sessions.

## Install from source

With Rust 1.93 or later and network access, Cargo can install the command directly from its tagged source:

```sh
cargo install --git https://github.com/diegopn/potoo-portugol-mapler-interpreter.git --rev 147b5b88bd4aa8833bc7966e048d6f835fc3c7ce --bin potoo-mapler
```

## Use as a Rust library

```toml
[dependencies]
potoo-mapler-interpreter = { git = "https://github.com/diegopn/potoo-portugol-mapler-interpreter.git", rev = "147b5b88bd4aa8833bc7966e048d6f835fc3c7ce" }
```

The library API lets Rust applications provide their own `RuntimeIo` implementation.

## DOI

A stable Figshare DOI will be added when the first public release is published.
