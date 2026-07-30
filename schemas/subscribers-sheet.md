# Schemat: arkusz "subscribers"

Arkusz `subscribers` jest **jedynym źródłem prawdy o zgodzie** na
otrzymywanie e-maili. Jest niezależny od danych importowanych z MAK+ —
patrz [docs/RODO.md](../docs/RODO.md) sekcja 5.

> Ten dokument opisuje docelową strukturę arkusza. Implementacja (kod
> Apps Script obsługujący ten arkusz) to kolejny krok — na razie tylko
> struktura.

## Kolumny

| Kolumna | Typ | Opis |
|---|---|---|
| `id_subskrybenta` | tekst (UUID) | wewnętrzny, stabilny identyfikator wiersza — niezależny od id z MAK+, żeby zmiany w MAK+ nie gubiły historii zgody |
| `email` | tekst | adres e-mail czytelnika (klucz łączący z danymi z MAK+, jeśli e-mail jest wspólny) |
| `imie` | tekst (opcjonalne) | do personalizacji maili — **placeholder**, do potwierdzenia czy potrzebne |
| `status_zgody` | enum: `brak_zgody` / `aktywna` / `wycofana` | **domyślnie `brak_zgody`** dla każdego nowego rekordu, niezależnie od źródła |
| `data_zgody` | data (ISO 8601, np. `2026-07-30`) | kiedy zgoda została wyrażona; puste, jeśli `status_zgody` = `brak_zgody` |
| `data_wycofania` | data (ISO 8601) | kiedy zgoda została wycofana; puste, jeśli zgoda nigdy nie była aktywna lub wciąż jest aktywna |
| `zrodlo_zgody` | tekst | skąd pochodzi zgoda, np. `formularz_papierowy`, `formularz_online`, `email_potwierdzajacy` — **nigdy** `import_mak` (import z MAK+ sam w sobie nie jest zgodą) |
| `typ_zgody` | enum: `godziny_otwarcia` / `nowosci` / `oba` | na wypadek przyszłego rozdzielenia zgód na różne typy komunikacji — patrz `docs/RODO.md` sekcja 2 |
| `id_czytelnika_mak` | tekst (opcjonalne) | powiązanie z rekordem w eksporcie MAK+, jeśli potrzebne do łączenia danych — **nie jest to podstawa zgody** |
| `token_wypisu` | tekst (unikalny) | token używany w linku "wypisz się" w mailach — pozwala na wycofanie zgody bez logowania się |
| `data_ostatniej_wysylki` | data (ISO 8601, opcjonalne) | pomocnicze pole do przyszłej analityki / unikania zbyt częstej wysyłki |
| `uwagi` | tekst (opcjonalne) | wolne pole na notatki administracyjne |

## Zasady

1. **Nowy e-mail z importu MAK+ → nowy wiersz w `subscribers` ze
   `status_zgody = brak_zgody`.** Nigdy automatyczne ustawienie `aktywna`.
2. Zmiana na `aktywna` następuje wyłącznie w wyniku jawnej akcji czytelnika
   (np. wypełnienie formularza zgody) — patrz `docs/RODO.md` sekcja 3.
3. Zmiana na `wycofana` następuje w wyniku kliknięcia linku wypisu (lub
   innego jawnego żądania) — patrz `docs/RODO.md` sekcja 4. Wysyłka maili
   musi zawsze filtrować po `status_zgody = aktywna`.
4. `token_wypisu` powinien być unikalny i niemożliwy do odgadnięcia
   (losowy identyfikator), żeby link wypisu nie mógł być użyty do wypisania
   innej osoby.
5. Usunięcie rekordu z MAK+ (np. czytelnik przestał być aktywny) **nie**
   powoduje automatycznej zmiany `status_zgody` — patrz `docs/RODO.md`.

## Przykładowe dane testowe

Zanonimizowany przykład zgodny z tym schematem: patrz
[samples/subscribers-przyklad.csv](../samples/subscribers-przyklad.csv).
