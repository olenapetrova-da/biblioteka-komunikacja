# Automatyzacja komunikacji z czytelnikami biblioteki

Projekt organizacji pozarządowej (NGO) automatyzujący komunikację
e-mailową i przygotowanie treści na Instagram dla czytelników biblioteki
korzystającej z systemu **MAK+**.

> **Status:** wczesny etap — szkielet repozytorium. Logika biznesowa
> (import, wysyłka, generowanie treści) nie jest jeszcze zaimplementowana.
> Zobacz [CLAUDE.md](CLAUDE.md) po szczegółowy kontekst i ograniczenia.

## Co robi projekt

1. **Import danych z MAK+** — MAK+ nie ma API, więc dane czytelników
   pozyskiwane są przez ręczny eksport CSV (patrz
   [integrations/mak-export](integrations/mak-export/README.md)).
2. **Czyszczenie i łączenie danych** w Google Sheets — dane z MAK+ są
   łączone z osobno prowadzoną listą zgód RODO
   (patrz [schemas/subscribers-sheet.md](schemas/subscribers-sheet.md)).
3. **Wysyłka e-maili** (zmiany godzin otwarcia, nowości w zbiorach) przez
   Gmail / Google Apps Script — tylko do czytelników z aktywną zgodą
   (patrz [integrations/gmail-send](integrations/gmail-send/README.md)).
4. **Przygotowanie treści o nowościach** do ręcznej publikacji na
   Instagramie (patrz
   [integrations/instagram-content](integrations/instagram-content/README.md)).

Wszystko działa zgodnie z RODO — patrz [docs/RODO.md](docs/RODO.md).

## Stos technologiczny

- Google Apps Script (główna logika)
- Google Sheets (baza pośrednia; możliwa przyszła migracja do Supabase)
- Looker Studio (analityka — przyszłość)
- [clasp](https://github.com/google/clasp) (synchronizacja Apps Script ↔ repo)

## Struktura repozytorium

```
docs/           dokumentacja (w tym RODO.md)
schemas/        struktura danych (CSV z MAK+, arkusz subscribers)
samples/        przykładowe/zanonimizowane dane testowe (NIGDY prawdziwe dane)
integrations/   opis integracji: mak-export, gmail-send, instagram-content
src/            docelowo: kod Apps Script synchronizowany przez clasp
```

## Jak uruchomić (na obecnym etapie)

Projekt nie zawiera jeszcze działającego kodu. Docelowo:

1. Zainstaluj [clasp](https://github.com/google/clasp):
   `npm install -g @google/clasp`
2. Zaloguj się: `clasp login`
3. Podłącz projekt Apps Script (po jego utworzeniu w Google Drive):
   `clasp clone <scriptId>` lub `clasp create` w folderze `src/`
   — **TODO**: uzupełnić po utworzeniu właściwego projektu Apps Script.
4. Skopiuj `.env.example` do `.env` i uzupełnij lokalne identyfikatory
   (np. Sheet ID) — `.env` nigdy nie trafia do repozytorium.

## Jak zrobić eksport z MAK+ (szkic — do uzupełnienia)

> **TODO:** poniższe kroki to placeholder. Trzeba je uzupełnić o rzeczywisty
> proces eksportu używany w danej bibliotece (moduł MAK+, dokładna ścieżka
> menu, format pliku).

1. Zaloguj się do modułu administracyjnego MAK+.
2. Wygeneruj eksport listy czytelników (moduł czytelnicy / wypożyczenia —
   do potwierdzenia, który moduł zawiera potrzebne dane).
3. Zapisz plik CSV — **nie commituj go do repozytorium** (patrz
   [.gitignore](.gitignore) i [SECURITY.md](SECURITY.md)).
4. Wgraj plik do folderu importu w Google Drive / arkusza Google Sheets
   (docelowy mechanizm importu — do zaimplementowania).

Oczekiwana struktura kolumn: patrz
[schemas/mak-export-csv.md](schemas/mak-export-csv.md) (placeholder — do
uzupełnienia rzeczywistą strukturą).

## RODO

Zobacz [docs/RODO.md](docs/RODO.md) — opis podstawy prawnej przetwarzania,
proces zgody i wycofania zgody, oraz zasady retencji danych.

## Zgłaszanie problemów z bezpieczeństwem/danymi

Zobacz [SECURITY.md](SECURITY.md).

## Współpraca

Zobacz [CONTRIBUTING.md](CONTRIBUTING.md).
