[![Package tests](https://github.com/mason-org/mason-registry/actions/workflows/package-tests.yaml/badge.svg)](https://github.com/mason-org/mason-registry/actions/workflows/package-tests.yaml?query=branch%3Amain)
![Platforms](https://img.shields.io/badge/platform-linux%20macOS%20windows-blue)
[![Sponsors](https://img.shields.io/github/sponsors/williamboman?style=flat-square)](https://github.com/sponsors/williamboman)

# mason-registry

> Repository of versioned package definitions for [mason.nvim][mason].

# Table of Contents

<!--toc:start-->
- [The anatomy of a package](#the-anatomy-of-a-package)
- [Package specification](#package-specification)
  - [`name`](#name)
  - [`description`](#description)
  - [`homepage`](#homepage)
  - [`licenses`](#licenses)
  - [`languages`](#languages)
  - [`categories`](#categories)
  - [`source`](#source)
  - [`bin`](#bin)
<!--toc:end-->

> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT
> RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14][bcp14],
> [RFC2119][rfc2119], and [RFC8174][rfc8174] when, and only when, they appear in all capitals, as shown here.

# The anatomy of a package

Packages are defined following a [well-defined
specification](#package-specification). Package definitions are hosted as
separate YAML files that MUST be located at
`packages/<package-name>/package.yaml`.

Package sources are identified via a [purl][purl]-compatible identifier. Each
package identifier MUST contain a version component specifying the latest
available version. Package versions are automatically kept up-to-date via
[Renovate][renovate].

# Package specification

The following is a rough outline of the package definition schema:

```json5
{
  name: string,
  description: string,
  homepage: string,
  licenses: string[],
  languages: string[],
  categories: string[],
  source: {
    id: string,
    [key: string]: any,
  },
  bin?: {
    [executable: string]: string
  }
}
```

## `name`

The package name MUST be unique. The name of a package MUST follow the
following naming scheme:

1. If the upstream package name is sufficiently unambiguous, or otherwise
   widely recognized, that name MUST be used.
1. If the upstream package provides a single executable with a name that is
   sufficiently unambiguous, or otherwise widely recognized, the name of the
   executable MUST be used.
1. If either the package or executable name is ambiguous, a name where a
   clarifying prefix or suffix is added SHOULD be used.
1. As a last resort, the name of the package should be constructed to best
   convey its target language and scope, e.g. `json-language-server` for a JSON
   language server.

## `description`

Short description of the package. The description SHOULD be sourced from the
upstream package directly.

## `homepage`

The homepage of the package. The homepage SHOULD be a public website if
available, otherwise it MUST be a URL to the source code.

The URL MUST be a [well-formed URL][rfc1738]. The URL scheme MUST be either
`http` or `https`.

## `licenses`

List of licenses associated with this package. MUST contain at least one entry.

The license MUST be a [SPDX-compatible](https://spdx.org/licenses/) license
identifier.

Examples:

- `MIT`
- `Apache-2.0`
- `GPL-3.0-only`

## `languages`

The languages the package targets. MAY be empty. A language is an arbitrary
string (e.g., `"Rust"`). The casing of the string MUST be the same as other
references to the same language in other package definitions, i.e. it's an
error if package A specifies `Javascript` and package B specifies `JavaScript`.

## `categories`

The categories the package belongs to. MAY be empty. If not empty, each entry
MUST be one of:

- `Compiler`
- `DAP`
- `Formatter`
- `LSP`
- `Linter`
- `Runtime`

## `source`

The source of the package. The `source` entry contains all necessary
information to properly install the package. At the very minimum it MUST
contain an `id` property. The `id` property MUST be a [purl][purl]-compatible
package identifier. The purl identifier MUST contain a version component.

The source object MAY contain additional properties to support installation.

Examples:

```yaml
source:
  id: pkg:npm/typescript-language-server@2.0.0
```

```yaml
source:
  id: pkg:github/rust-lang/rust-analyzer@2022-12-05
```

## `bin`

The executables the package provides. The key is the canonical name of the
executable, and the value is either (i) a relative path to the executable from
the package directory, or (ii) an expression that delegates path resolution
(e.g., `npm:typescript-language-server` or `cargo:rust-analyzer`).

Example:

```yaml
bin:
  typescript-language-server: npm:typescript-language-server
  rust-analyzer: bin/rust-analyzer
```

[bcp14]: https://tools.ietf.org/html/bcp14
[mason]: https://github.com/williamboman/mason.nvim
[purl]: https://github.com/package-url/purl-spec
[renovate]: https://github.com/renovatebot/renovate
[rfc1738]: https://www.rfc-editor.org/rfc/rfc1738
[rfc2119]: https://tools.ietf.org/html/rfc2119
[rfc8174]: https://tools.ietf.org/html/rfc8174
