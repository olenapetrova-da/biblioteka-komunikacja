# Schemat: eksport CSV z MAK+

> **PLACEHOLDER — do uzupełnienia rzeczywistą strukturą kolumn.**
> Poniższa struktura to wstępny szkic oparty na typowej zawartości eksportu
> czytelników z systemu bibliotecznego. **Nie jest potwierdzona** — trzeba
> ją zweryfikować na podstawie faktycznego pliku wyeksportowanego z MAK+ w
> danej bibliotece (nazwy kolumn, kodowanie znaków, separator, format dat
> mogą się różnić).

## Do ustalenia przed implementacją importu

- [ ] Dokładne nazwy i kolejność kolumn w eksporcie (nagłówek CSV).
- [ ] Kodowanie pliku (MAK+ może eksportować w Windows-1250 lub UTF-8 —
      **do sprawdzenia**, żeby polskie znaki nie były uszkodzone).
- [ ] Separator kolumn (przecinek `,` czy średnik `;` — w polskich
      lokalizacjach Excela częściej średnik).
- [ ] Format daty (np. `DD.MM.RRRR` czy `RRRR-MM-DD`).
- [ ] Czy eksport zawiera e-mail czytelnika bezpośrednio, czy trzeba go
      łączyć z osobnego źródła.
- [ ] Czy eksport zawiera dane wrażliwe, których nie wolno przechowywać
      dłużej niż to konieczne (np. pełny adres zamieszkania) — do
      zminimalizowania na etapie importu.

## Szkic oczekiwanych kolumn (do potwierdzenia/nadpisania)

| Kolumna (placeholder) | Opis | Uwagi |
|---|---|---|
| `id_czytelnika` | wewnętrzny identyfikator czytelnika w MAK+ | prawdopodobnie numer karty bibliotecznej |
| `nazwisko` | nazwisko czytelnika | dane osobowe — minimalizować użycie |
| `imie` | imię czytelnika | dane osobowe — minimalizować użycie |
| `email` | adres e-mail | **nie jest dowodem zgody na maile** — patrz [docs/RODO.md](../docs/RODO.md) |
| `telefon` | numer telefonu | prawdopodobnie niepotrzebny do tego projektu — do potwierdzenia, czy w ogóle importować |
| `data_waznosci_karty` | data ważności karty bibliotecznej | do potwierdzenia, czy istotne dla logiki wysyłki |
| `status_konta` | aktywne / zablokowane / wygasłe | do potwierdzenia dokładnych wartości |

## Zasady importu (niezależnie od finalnej struktury kolumn)

1. Import z MAK+ **nigdy** nie tworzy ani nie zmienia statusu zgody w
   arkuszu `subscribers` — patrz
   [subscribers-sheet.md](subscribers-sheet.md) i
   [docs/RODO.md](../docs/RODO.md) sekcja 5.
2. Plik CSV źródłowy nie powinien być przechowywany dłużej niż to konieczne
   do importu (patrz retencja w `docs/RODO.md`) i **nigdy nie trafia do
   repozytorium** (patrz `.gitignore`).
3. Import powinien być idempotentny — wielokrotne wgranie tego samego
   eksportu nie powinno duplikować rekordów ani nadpisywać statusu zgody.

## Przykładowy plik testowy

Zanonimizowany przykład zgodny z tym szkicem: patrz
[samples/mak-eksport-przyklad.csv](../samples/mak-eksport-przyklad.csv).
