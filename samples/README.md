# Dane przykładowe (samples/)

Ten folder zawiera **wyłącznie zanonimizowane, fikcyjne dane testowe**,
używane do rozwoju i testowania importu/logiki projektu.

## Zasady

- **Zabronione jest umieszczanie tu prawdziwych danych czytelników** —
  nawet tymczasowo, nawet do "szybkiego testu". Patrz
  [SECURITY.md](../SECURITY.md).
- Wszystkie adresy e-mail w plikach przykładowych używają domeny
  `example.com` (zarezerwowanej przez IANA do celów dokumentacyjnych —
  nigdy nie dostarcza prawdziwej poczty).
- Wszystkie imiona/nazwiska są fikcyjne.
- Pliki w tym folderze są celowo **wyjątkiem** w `.gitignore` (patrz
  wpisy `!samples/*-przyklad.csv`) — dozwolone są tylko pliki z sufiksem
  `-przyklad.csv`, jawnie oznaczone jako dane testowe. Nowe pliki z
  prawdziwymi/eksportowanymi danymi (np. `*-real.csv`, `*-export.csv`)
  pozostają zablokowane.

## Zawartość

- [mak-eksport-przyklad.csv](mak-eksport-przyklad.csv) — przykład zgodny
  ze szkicem [schemas/mak-export-csv.md](../schemas/mak-export-csv.md).
- [subscribers-przyklad.csv](subscribers-przyklad.csv) — przykład zgodny
  z [schemas/subscribers-sheet.md](../schemas/subscribers-sheet.md).

> Struktura kolumn w obu plikach to placeholder — do zaktualizowania, gdy
> rzeczywista struktura eksportu MAK+ zostanie potwierdzona.
