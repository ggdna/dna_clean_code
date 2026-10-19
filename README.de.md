# dna_clean_code

[English](README.md) | Deutsch

Gibt einem Repo die Regeln, nach denen Code geschrieben und getestet wird,
sprachneutral und für Dart und TypeScript, und Skills, die ihn prüfen und
reviewen.

## Motivation

Die Clean-Code- und Test-Konventionen lagen früher in der
ggsuite-Umbrella-Schicht, nur ggsuite-Repos bekamen sie. Diese Schicht
löst sie heraus, damit jede Organisation dieselben Regeln erben kann. Was
in jeder Sprache gilt, steht einmal da; die Besonderheiten von Dart und
TypeScript stehen in eigenen Guides daneben. Die Skills prüfen ein Repo
und reviewen seine Änderungen gegen genau diese Regeln.

## Verwendung

Als Dev-Dependency eintragen und einmal initialisieren:

```bash
pnpm add -D @ggdna/dna-clean-code   # TypeScript projects
dart pub add dev:dna_clean_code     # Dart projects
gg dna init
```

`gg dna init` legt einen Test ab, der die DNA bei jedem Testlauf
instanziiert und prüft. Er legt die Guides in `doc/guides/` und
`doc/guides/for-ai/` ab und die Skills in `.claude/skills/`. Meist kommt
die Schicht über die Umbrella-Schicht der Organisation ins Repo, z. B.
`dna_ggsuite` oder `dna_tssuite`.

Tippe `/code` oder `/test` in Claude Code, um das Repo gegen den Code-
oder den Test-Guide prüfen zu lassen; beide listen zuerst die Befunde auf
und beheben nur, was du bestätigst. Tippe `/review-light` oder
`/review-heavy`, um die aktuellen Änderungen reviewen zu lassen.

Eine spätere Schicht kann die Guides anpassen, ohne sie zu forken: Eine
Datei am selben Pfad ersetzt einen Guide ganz, eine `.overrides.md`-Datei
daneben, z. B. `code-guide-typescript.overrides.md`, ersetzt nur die
Abschnitte, die sie nennt. Die Guides markieren ihre austauschbaren
Abschnitte mit `@`-Tags: `@layout`, `@imports`, `@api`, `@example` und
`@naming` im Code-Guide, `@nesting`, `@examples` und `@coverage` im
Test-Guide, `@performance` und `@security` im Review-Guide.

## Guides

- `dna/doc/guides/code-guide.md` — Paketaufbau, Lizenzkopf, Reihenfolge
  der Member, API-Form, Example-Konstruktoren, Kommentar-Landmarken,
  Dokumentation, kleine Funktionen, Benennung, alles nur einmal sagen
- `dna/doc/guides/code-guide-dart.md`,
  `dna/doc/guides/code-guide-typescript.md` — was das in der jeweiligen
  Sprache heißt, samt den Pflicht-Lints
- `dna/doc/guides/test-guide.md` — den Quellbaum spiegeln, beim Example
  anfangen, echte Typen statt Mocks, auf Strukturen prüfen, 100 Prozent
  Coverage
- `dna/doc/guides/test-guide-dart.md`,
  `dna/doc/guides/test-guide-typescript.md` — dasselbe pro Sprache
- `dna/doc/guides/for-ai/ai-review-guide.md` — die Review-Checkliste, die
  ein Agent für jede geänderte Datei abarbeitet, und die interaktive
  Fix-Schleife
- `dna/doc/guides/for-ai/review-heavy-guide.md` — die Review-Checkliste
  plus `/simplify` und `/code-review`

## Skills

- `/code` — prüft den Code gegen den Code-Guide und den Guide der Sprache
  des Repos
- `/test` — prüft die Tests gegen den Test-Guide und den Guide der
  Sprache des Repos und startet den Commit-Check des Repos
- `/review-light` — reviewt die aktuellen Änderungen; folgt dem
  AI-Review-Guide
- `/review-heavy` — dasselbe, plus `/simplify` und `/code-review`; folgt
  dem Review-Heavy-Guide

## Umfang

Die Schicht legt nur die Regeln fest und prüft sie. Der Commit-Check, der
die Coverage durchsetzt, die Lint-Konfiguration und die CI-Pipeline
kommen aus dem Repo und aus anderen Schichten.

## Schichten

Die Schicht ist orthogonal: `dna/_dna.json` nennt keine Eltern-Schichten.
Clean Code steht für sich und wird von der Umbrella-Schicht jeder
Organisation geerbt.

## Variablen

- `dnaCopyrightHolder` — der Name im Lizenzkopf jeder Datei;
  `mustOverride` in `dna/_dna.json` markiert ihn, daher setzt ihn jedes
  Repo, das diese Schicht einbindet, in seiner eigenen `dna/_vars.json`

## Entwicklung

Der Ordner `dna/` ist handgeschriebene Quelle und wird nie generiert. Das
Repo instanziiert seine eigene DNA — nach Änderungen `dart test` laufen
lassen; vorher committen, denn eine Datei, die die DNA überschreiben
würde, darf keine uncommittete Arbeit enthalten.

Bearbeite die Dateien unter `dna/`, nicht ihre Kopien in `doc/` und
`.claude/`, die der Test überschreibt.

## Lizenz

MIT — siehe [LICENSE](LICENSE).
