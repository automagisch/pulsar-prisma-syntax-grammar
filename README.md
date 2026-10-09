# language-prisma-schema

Syntax highlighting and snippets for [Prisma](https://www.prisma.io) schema files (`.prisma`) in [Pulsar](https://pulsar-edit.dev), the community-led successor to Atom.

The package ships a TextMate grammar (`source.prisma`) based on the official grammar from the Prisma VS Code extension ([prisma/language-tools](https://github.com/prisma/language-tools)). It has been converted to CSON and reworked to use standard scope names, so regular Pulsar syntax themes color everything properly.

## Features

- **Blocks**: `model`, `enum`, `type` (composite types), `view`, `datasource`, `generator` and `types`, with the block name highlighted as a type name.
- **Fields**: field names, scalar types (`String`, `Boolean`, `Int`, `BigInt`, `Float`, `Decimal`, `DateTime`, `Json`, `Bytes`, `Unsupported("...")`) and references to other models/enums.
- **Modifiers**: optional `?` and list `[]`.
- **Field attributes**: `@id`, `@unique`, `@default`, `@relation`, `@map`, `@updatedAt`, `@ignore`, and more.
- **Block attributes**: `@@id`, `@@unique`, `@@index`, `@@map`, `@@ignore`, `@@schema`, `@@fulltext`.
- **Native types**: `@db.VarChar(255)`, `@db.Uuid`, `@db.Text`, ... (namespace and type colored separately).
- **Arguments**: named arguments such as `fields:`, `references:`, `onDelete:`, `name:`, `map:`, `sort:`, `type:`; referential actions (`Cascade`, `SetNull`, ...) and `Asc`/`Desc`.
- **Functions**: `env()`, `autoincrement()`, `now()`, `uuid()`, `cuid()`, `ulid()`, `nanoid()`, `dbgenerated()`, `sequence()`, `auto()`.
- **Values**: strings with escapes (`\"`, `\n`, `é`), numbers, `true`/`false`/`null`, arrays.
- **Configuration**: `key = value` pairs in `datasource` and `generator` blocks.
- **Enums**: enum values, including `@map` and `@@map`.
- **Comments**: `//` (regular), `///` (documentation comments) and `/* ... */`.
- **Editor settings**: `Cmd+/` / `Ctrl+/` toggles `// ` comments, auto-indent inside `{ }`, 2 spaces per indentation level.
- **Snippets**: see below.

## Installation

### From the Pulsar Package Registry (recommended)

In Pulsar, open **Settings → Install**, search for `language-prisma-schema` and click **Install**.

Or from the command line:

```sh
ppm install language-prisma-schema
```

(`pulsar -p install language-prisma-schema` does the same thing.)

Open any `.prisma` file; the status bar in the bottom-right corner should read **Prisma**.

### From source (for development)

```sh
git clone https://github.com/automagisch/pulsar-prisma-syntax-grammar.git
cd pulsar-prisma-syntax-grammar
ppm link
```

`ppm link` symlinks this folder into `~/.pulsar/packages`, so changes take effect immediately after reloading Pulsar (**View → Developer → Reload Window**).

To remove the link again, run `ppm unlink` in the same folder.

> **Note (macOS):** if `ppm` or `pulsar` is not found, choose **Pulsar → Install Shell Commands** from the menu bar and open a new terminal window.

### Verifying the package is active

Go to **Settings → Packages** and search for `language-prisma-schema`. It should be listed under *Community Packages* and be enabled.

## Snippets

Type a prefix in a `.prisma` file and press `Tab`. Press `Tab` again to jump to the next placeholder.

| Prefix        | Result                                                      |
|---------------|-------------------------------------------------------------|
| `model`       | `model Name { id Int @id @default(autoincrement()) }`       |
| `enum`        | `enum Name { VALUE }`                                       |
| `datasource`  | PostgreSQL datasource with `url = env("DATABASE_URL")`      |
| `generator`   | `generator client { provider = "prisma-client-js" }`        |
| `relation`    | Relation field with `@relation(fields: [...], references: [...])` plus the foreign-key field |
| `id`          | `id Int @id @default(autoincrement())`                      |
| `iduuid`      | `id String @id @default(uuid())`                            |
| `timestamps`  | `createdAt DateTime @default(now())` and `updatedAt DateTime @updatedAt` |
| `@@index`     | `@@index([field])`                                          |
| `@@map`       | `@@map("table_name")`                                       |

All snippets are also available from the command palette (`Cmd+Shift+P` / `Ctrl+Shift+P` → *Snippets: Available*).

## Troubleshooting

- **No colors / status bar doesn't show "Prisma"**: reload the window (**View → Developer → Reload Window**), or select the grammar manually with `Ctrl+Shift+L`.
- **Conflict with another Prisma package**: if another Prisma grammar package is installed, two grammars may compete for `.prisma` files. Disable or uninstall the other package via **Settings → Packages** and reload Pulsar.
- **`ppm link` says the package already exists**: something is already at that path in `~/.pulsar/packages/`. Check with `ls -la ~/.pulsar/packages/` and remove or rename the old entry.
- **Colors look different than expected**: final colors depend on your syntax theme. Use **Editor: Log Cursor Scope** from the command palette to see which scopes apply at the cursor.

## Project structure

```
grammars/prisma.cson            TextMate grammar (source.prisma)
settings/language-prisma.cson   Editor settings (comments, indentation)
snippets/language-prisma.cson   Snippets
spec/fixtures/schema.prisma     Example schema exercising every feature
```

## Contributing

Issues and pull requests are welcome at [automagisch/pulsar-prisma-syntax-grammar](https://github.com/automagisch/pulsar-prisma-syntax-grammar). If something in your schema isn't highlighted correctly, please include a minimal `.prisma` snippet that reproduces it.

## Credits and license

The grammar is derived from the TextMate grammar in [prisma/language-tools](https://github.com/prisma/language-tools) (`packages/vscode/syntaxes/prisma.tmLanguage.json`), © Prisma Data, Inc., released under the Apache License 2.0.

This package is also licensed under the [Apache License 2.0](LICENSE). See [NOTICE](NOTICE) for attribution and a summary of changes.
