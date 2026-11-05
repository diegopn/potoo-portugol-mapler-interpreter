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

## DOI and citation

DOI: [10.6084/m9.figshare.34914246](https://doi.org/10.6084/m9.figshare.34914246)

Suggested reference (ABNT NBR 6023:2018):

> NASCIMENTO, Diego Pereira do. *Portugol Mapler interpreter v0.1.0*. 2026. Programa de computador. Disponível em: https://doi.org/10.6084/m9.figshare.34914246. Acesso em: 4 nov. 2026.
