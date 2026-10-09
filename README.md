# language-prisma

Syntax highlighting en snippets voor [Prisma](https://www.prisma.io)-schemabestanden (`.prisma`) in de editor [Pulsar](https://pulsar-edit.dev), de opvolger van Atom.

Het pakket bevat een TextMate-grammatica (`source.prisma`) die gebaseerd is op de officiële grammatica uit de Prisma VS Code-extensie ([prisma/language-tools](https://github.com/prisma/language-tools)). Die is omgezet naar CSON en uitgebreid met standaard scope-namen, zodat gewone Pulsar-thema's alles netjes inkleuren.

## Functies

- **Blokken**: `model`, `enum`, `type` (composite types), `view`, `datasource`, `generator` en `types`, met de bloknaam gemarkeerd als typenaam.
- **Velden**: veldnamen, scalaire types (`String`, `Boolean`, `Int`, `BigInt`, `Float`, `Decimal`, `DateTime`, `Json`, `Bytes`, `Unsupported("...")`) en verwijzingen naar andere modellen/enums.
- **Modifiers**: optioneel `?` en lijst `[]`.
- **Veldattributen**: `@id`, `@unique`, `@default`, `@relation`, `@map`, `@updatedAt`, `@ignore`, enzovoort.
- **Blokattributen**: `@@id`, `@@unique`, `@@index`, `@@map`, `@@ignore`, `@@schema`, `@@fulltext`.
- **Native types**: `@db.VarChar(255)`, `@db.Uuid`, `@db.Text`, ... (namespace en type apart gekleurd).
- **Argumenten**: benoemde argumenten zoals `fields:`, `references:`, `onDelete:`, `name:`, `map:`, `sort:`, `type:`; referential actions (`Cascade`, `SetNull`, ...) en `Asc`/`Desc`.
- **Functies**: `env()`, `autoincrement()`, `now()`, `uuid()`, `cuid()`, `ulid()`, `nanoid()`, `dbgenerated()`, `sequence()`, `auto()`.
- **Waarden**: strings met escapes (`\"`, `\n`, `é`), getallen, `true`/`false`/`null`, arrays.
- **Configuratie**: `sleutel = waarde`-paren in `datasource`- en `generator`-blokken.
- **Enums**: enumwaarden, inclusief `@map` en `@@map`.
- **Commentaar**: `//` (gewoon commentaar), `///` (documentatiecommentaar) en `/* ... */`.
- **Editor-instellingen**: `Cmd+/` zet `// ` voor regels, automatisch inspringen binnen `{ }`, 2 spaties per inspringniveau.
- **Snippets**: zie hieronder.

## Installatie in Pulsar (macOS)

### 1. Shell-commando's installeren

Open Pulsar en kies in de menubalk **Pulsar → Install Shell Commands**. Daarna zijn in de terminal de commando's `pulsar` en `ppm` beschikbaar (`ppm` is de package manager van Pulsar; `pulsar -p` doet precies hetzelfde als `ppm`).

Controleer het in een nieuw terminalvenster:

```sh
ppm --version
```

### 2. Het pakket linken

```sh
cd ~/automagisch/pulsar-prisma-syntax-grammar
ppm link
```

`ppm link` maakt een symlink van deze map naar `~/.pulsar/packages/language-prisma`. Wijzigingen in deze map zijn dus direct actief in Pulsar (na een reload).

### 3. Pulsar herladen en testen

1. Kies **View → Developer → Reload Window** (of herstart Pulsar).
2. Open een `.prisma`-bestand, bijvoorbeeld `spec/fixtures/schema.prisma` uit deze map.
3. Rechtsonder in de statusbalk hoort **Prisma** te staan.
4. Staat daar iets anders (bijv. "Plain Text")? Druk op `ctrl+shift+L` en kies **Prisma** in de grammaticalijst.

### Alternatief: handmatig symlinken

Zonder `ppm` kun je de symlink ook zelf maken:

```sh
mkdir -p ~/.pulsar/packages
ln -s ~/automagisch/pulsar-prisma-syntax-grammar ~/.pulsar/packages/language-prisma
```

Herlaad daarna Pulsar zoals in stap 3.

### Controleren of het pakket actief is

Ga naar **Pulsar → Settings → Packages** en zoek op `language-prisma`. Het pakket staat onder *Community Packages* en moet ingeschakeld zijn.

### De-installeren

```sh
cd ~/automagisch/pulsar-prisma-syntax-grammar
ppm unlink
```

Heb je handmatig gelinkt, verwijder dan de symlink: `rm ~/.pulsar/packages/language-prisma`. Herlaad Pulsar daarna.

## Snippets

Typ een prefix in een `.prisma`-bestand en druk op `Tab`. Met `Tab` spring je vervolgens naar het volgende invulveld.

| Prefix        | Resultaat                                                   |
|---------------|-------------------------------------------------------------|
| `model`       | `model Name { id Int @id @default(autoincrement()) }`       |
| `enum`        | `enum Name { VALUE }`                                       |
| `datasource`  | PostgreSQL-datasource met `url = env("DATABASE_URL")`       |
| `generator`   | `generator client { provider = "prisma-client-js" }`        |
| `relation`    | Relatieveld met `@relation(fields: [...], references: [...])` plus het foreign-key-veld |
| `id`          | `id Int @id @default(autoincrement())`                      |
| `iduuid`      | `id String @id @default(uuid())`                            |
| `timestamps`  | `createdAt DateTime @default(now())` en `updatedAt DateTime @updatedAt` |
| `@@index`     | `@@index([field])`                                          |
| `@@map`       | `@@map("table_name")`                                       |

Alle snippets zijn ook te vinden via het commandopalet (`Cmd+Shift+P` → *Snippets: Available*).

## Problemen oplossen

- **Geen kleuren / statusbalk toont niet "Prisma"**: herlaad het venster (**View → Developer → Reload Window**). Kies anders handmatig de grammatica met `ctrl+shift+L`.
- **Conflict met een ander Prisma-pakket**: als er al een ander pakket voor Prisma geïnstalleerd is (bijv. `prisma` of een ander `language-prisma`), kunnen twee grammatica's om `.prisma`-bestanden strijden. Schakel het andere pakket uit of verwijder het via **Settings → Packages** en herlaad Pulsar.
- **`ppm link` zegt dat het pakket al bestaat**: er staat al iets op `~/.pulsar/packages/language-prisma`. Bekijk het met `ls -la ~/.pulsar/packages/` en verwijder of hernoem het oude item.
- **`ppm` of `pulsar` niet gevonden**: voer stap 1 opnieuw uit en open een nieuw terminalvenster. `pulsar -p link` werkt als alternatief voor `ppm link`.
- **Kleuren wijken af**: de uiteindelijke kleuren hangen af van je syntax-thema. Met **Editor: Log Cursor Scope** (commandopalet) zie je welke scopes op de cursorpositie gelden.

## Projectstructuur

```
grammars/prisma.cson            TextMate-grammatica (source.prisma)
settings/language-prisma.cson   Editor-instellingen (commentaar, inspringen)
snippets/language-prisma.cson   Snippets
spec/fixtures/schema.prisma     Voorbeeldschema dat alle features gebruikt
```

## Credits en licentie

De grammatica is afgeleid van de TextMate-grammatica in [prisma/language-tools](https://github.com/prisma/language-tools) (`packages/vscode/syntaxes/prisma.tmLanguage.json`), © Prisma Data, Inc., uitgebracht onder de Apache License 2.0.

Dit pakket valt eveneens onder de [Apache License 2.0](LICENSE). Zie [NOTICE](NOTICE) voor de herkomst en de aangebrachte wijzigingen.
