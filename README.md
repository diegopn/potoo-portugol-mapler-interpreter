<p align="center">
  <img src="https://diegopn.github.io/potoo-site/src/assets/images/language-interpreter-icon.svg?v=1c651ca7" alt="Potoo IDE language interpreter icon" width="120" />
</p>

<h1 align="center">Portugol Mapler Interpreter</h1>

<p align="center">An independent Rust interpreter library and CLI for documented Portugol Mapler syntax.</p>

<p align="center">
  <a href="https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/latest">Download</a> ·
  <a href="https://diegopn.github.io/potoo-site/">Website</a> ·
  <a href="https://diegopn.github.io/potoo-site/manual/mapler/">User manual</a>
</p>

<p align="center">
  <a href="https://www.rust-lang.org/"><img alt="Rust" src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white"></a>
  <a href="https://doc.rust-lang.org/cargo/"><img alt="Cargo" src="https://img.shields.io/badge/Cargo-DEA584?style=for-the-badge&logo=rust&logoColor=white"></a>
  <img alt="Rust library" src="https://img.shields.io/badge/Rust_library-4A86CF?style=for-the-badge">
  <img alt="CLI" src="https://img.shields.io/badge/CLI-6A4C93?style=for-the-badge">
</p>

Originally developed as part of the Potoo IDE project, this interpreter is an independent Rust software package intended to be distributed separately from the IDE. It can be embedded as a library in other Rust projects or run as a standalone command-line tool on Linux, macOS, and Windows.

The interpreter supports documented variable types and vectors, `escrever`/`ler`, arithmetic and logical expressions, control flow, and modules. The original Mapler IDE stores source files as `.txt` and defines no official extension; `.mapler` is a Potoo convention based on DesignLiquido’s project, not the original IDE. Syntax and behavior follow Projeto-Mapler’s documentation.

## Original language references

- [Mapler user manual](https://portugol.sourceforge.io/manuais.html)
- [Projeto-Mapler syntax reference](https://github.com/Projeto-Mapler/portugol)
- [Mapler examples](https://portugol.sourceforge.io/exemplos/)

These are the original language references for compatibility work. This repository provides an independent Rust implementation.

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

## Support

If this interpreter is useful in your work, you can support its continued development:

<p align="left">
  <a href="https://github.com/sponsors/diegopn"><img alt="Become a sponsor" src="https://img.shields.io/badge/Become_a_sponsor-EA4AAA?style=for-the-badge&logo=githubsponsors&logoColor=white"></a>
</p>

## Project policies

- [Code of conduct](CODE_OF_CONDUCT.md)
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Standalone command-line runner

The `potoo-mapler` command runs one source file without Potoo IDE. For example:

```sh
potoo-mapler example.mapler
```

The terminal runner provides standard input/output and continues through debugger events without an interactive debugger.

## Install a prebuilt runner

The v0.1.1 release provides native CLI packages for Linux x86_64 GNU, Windows x86_64 MSVC, Intel macOS x86_64, and Apple silicon macOS arm64. Download your system's archive and `SHA256SUMS` from the [latest GitHub release](https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases/latest), then run the matching commands below from the folder containing both files. Each archive contains `potoo-mapler` (or `potoo-mapler.exe` on Windows) and the repository license. The package does not include a sample file; save the source example above as `example.mapler` or use your own file.

### Linux x86_64 GNU

```sh
set -euo pipefail
archive="potoo-portugol-mapler-interpreter-v0.1.1-x86_64-unknown-linux-gnu.tar.gz"
grep -F "  $archive" SHA256SUMS | sha256sum --check -
tar -xzf "$archive"
mkdir -p "$HOME/.local/bin"
install -m 755 "${archive%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
export PATH="$HOME/.local/bin:$PATH"
potoo-mapler example.mapler
```

To keep the command available in new Linux sessions, add the `export PATH="$HOME/.local/bin:$PATH"` line to your shell profile.

### Windows x86_64 MSVC (PowerShell)

```powershell
$archive = "potoo-portugol-mapler-interpreter-v0.1.1-x86_64-pc-windows-msvc.zip"
$line = Get-Content .\SHA256SUMS | Where-Object { $_.EndsWith("  $archive") } | Select-Object -First 1
if (-not $line) { throw "No SHA256SUMS entry found for $archive" }
$expected = ($line -split '\s+')[0]
$actual = (Get-FileHash -LiteralPath $archive -Algorithm SHA256).Hash.ToLowerInvariant()
if ($actual -ne $expected) { throw "SHA-256 mismatch for $archive" }
$installDir = Join-Path $env:USERPROFILE ".local\bin"
New-Item -ItemType Directory -Force $installDir | Out-Null
Expand-Archive -LiteralPath $archive -DestinationPath $installDir
$exe = Join-Path $installDir "potoo-portugol-mapler-interpreter-v0.1.1-x86_64-pc-windows-msvc\potoo-mapler.exe"
& $exe .\example.mapler
```

To run `potoo-mapler.exe` from any folder, add `$env:USERPROFILE\.local\bin\potoo-portugol-mapler-interpreter-v0.1.1-x86_64-pc-windows-msvc` to your user `Path` environment variable in Windows Settings.

### Intel macOS x86_64

```sh
set -euo pipefail
archive="potoo-portugol-mapler-interpreter-v0.1.1-x86_64-apple-darwin.tar.gz"
grep -F "  $archive" SHA256SUMS | shasum -a 256 -c -
tar -xzf "$archive"
mkdir -p "$HOME/.local/bin"
install -m 755 "${archive%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
export PATH="$HOME/.local/bin:$PATH"
potoo-mapler example.mapler
```

### Apple silicon macOS arm64

```sh
set -euo pipefail
archive="potoo-portugol-mapler-interpreter-v0.1.1-aarch64-apple-darwin.tar.gz"
grep -F "  $archive" SHA256SUMS | shasum -a 256 -c -
tar -xzf "$archive"
mkdir -p "$HOME/.local/bin"
install -m 755 "${archive%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
export PATH="$HOME/.local/bin:$PATH"
potoo-mapler example.mapler
```

On macOS, add the `export PATH="$HOME/.local/bin:$PATH"` line to `~/.zprofile` to keep the command available in future sessions.


## Install from source

With Rust 1.93 or later and network access, Cargo can install the command directly from its tagged source:

```sh
cargo install --git https://github.com/diegopn/potoo-portugol-mapler-interpreter.git --tag v0.1.1 --bin potoo-mapler
```

## Use as a Rust library

```toml
[dependencies]
potoo-mapler-interpreter = { git = "https://github.com/diegopn/potoo-portugol-mapler-interpreter.git", rev = "147b5b88bd4aa8833bc7966e048d6f835fc3c7ce" }
```

The library API lets Rust applications provide their own `RuntimeIo` implementation.

## DOI and citation

DOI: [10.6084/m9.figshare.34914246](https://doi.org/10.6084/m9.figshare.34914246)

Suggested reference (ABNT NBR 6023:2018):

> NASCIMENTO, Diego Pereira do. *Portugol Mapler interpreter v0.1.0*. 2026. Programa de computador. Disponível em: https://doi.org/10.6084/m9.figshare.34914246. Acesso em: 4 nov. 2026.
