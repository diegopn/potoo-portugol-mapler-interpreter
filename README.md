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

The interpreter supports documented variable types and vectors, `escrever`/`ler`, arithmetic and logical expressions, control flow, and modules. The original Mapler IDE stores source files as `.txt` and defines no official extension; `.mapler` is a Potoo convention based on DesignLiquido’s project, not the original IDE. Syntax and behavior follow Projeto-Mapler’s documentation.

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

Prebuilt binaries are published on the [GitHub Releases page](https://github.com/diegopn/potoo-portugol-mapler-interpreter/releases). Choose a version and the package for your system there. Packages are currently available for Linux x86_64, Windows x86_64 (MSVC), and Intel macOS (x86_64). The current release does not include an Apple silicon (arm64) package.

Download only the package for your system and the `SHA256SUMS` file, and save both in the same folder. Before extracting or running the executable, compare the package's SHA-256 hash with its corresponding entry in `SHA256SUMS`. Run the commands below from that folder. Replace `example.mapler` with the path to a source file you want to run; the release package does not include a sample file.

### Linux x86_64

```sh
asset=$(ls -1 potoo-portugol-mapler-interpreter-v*-x86_64-unknown-linux-gnu.tar.gz | head -n 1)
sha256sum "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
export PATH="$HOME/.local/bin:$PATH"
potoo-mapler example.mapler
```

To keep the command available in new Linux sessions, add the `export PATH="$HOME/.local/bin:$PATH"` line to your shell profile.

### Windows x86_64 (PowerShell)

```powershell
$archive = Get-ChildItem -Filter "potoo-portugol-mapler-interpreter-v*-x86_64-pc-windows-msvc.zip" | Select-Object -First 1
Get-FileHash $archive.FullName -Algorithm SHA256 # compare with the matching SHA256SUMS entry
Expand-Archive $archive.FullName -DestinationPath .\potoo-mapler
$exe = Get-ChildItem .\potoo-mapler -Filter "potoo-mapler.exe" -Recurse | Select-Object -First 1
& $exe.FullName .\example.mapler
```

To run `potoo-mapler.exe` from another folder, add the directory containing the executable to your user `PATH`.

### Intel macOS

The published macOS package targets Intel Macs (x86_64). There is no prebuilt Apple silicon package in the current release.

```sh
asset=$(ls -1 potoo-portugol-mapler-interpreter-v*-x86_64-apple-darwin.tar.gz | head -n 1)
shasum -a 256 "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-mapler" "$HOME/.local/bin/potoo-mapler"
export PATH="$HOME/.local/bin:$PATH"
potoo-mapler example.mapler
```

Add the `export PATH="$HOME/.local/bin:$PATH"` line to `~/.zprofile` to keep the command available in future macOS sessions.


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

## Support

If this interpreter is useful in your work, you can support its continued development:

<p align="left">
  <a href="https://github.com/sponsors/diegopn"><img alt="Become a sponsor" src="https://img.shields.io/badge/Become_a_sponsor-EA4AAA?style=for-the-badge&logo=githubsponsors&logoColor=white"></a>
</p>

## DOI and citation

DOI: [10.6084/m9.figshare.34914246](https://doi.org/10.6084/m9.figshare.34914246)

Suggested reference (ABNT NBR 6023:2018):

> NASCIMENTO, Diego Pereira do. *Portugol Mapler interpreter v0.1.0*. 2026. Programa de computador. Disponível em: https://doi.org/10.6084/m9.figshare.34914246. Acesso em: 4 nov. 2026.
