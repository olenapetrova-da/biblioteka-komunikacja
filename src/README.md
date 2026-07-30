# src/ — kod Apps Script

Ten folder jest docelowym miejscem na kod Google Apps Script
(pliki `.gs`/`.js` oraz `appsscript.json`), synchronizowany z projektem
Apps Script w Google Drive za pomocą [clasp](https://github.com/google/clasp).

## Status

**Pusty placeholder.** Na obecnym etapie projekt nie zawiera jeszcze
logiki biznesowej — patrz [CLAUDE.md](../CLAUDE.md) sekcja "Status
projektu".

## Plan (kolejne kroki)

1. Utworzyć projekt Apps Script (samodzielny lub powiązany z arkuszem
   Google Sheets) w Google Drive.
2. `clasp login` + `clasp clone <scriptId>` (lub `clasp create`), żeby
   zsynchronizować `appsscript.json` i pierwsze pliki z tym repozytorium.
3. Struktura plików (propozycja, do potwierdzenia przy implementacji):
   - `import-mak.gs` — import i czyszczenie eksportu CSV z MAK+.
   - `subscribers.gs` — logika arkusza zgód (dodawanie, opt-out).
   - `mailing.gs` — wysyłka e-maili przez Gmail.
   - `instagram-content.gs` — generowanie treści o nowościach.

`.clasp.json` (zawiera `scriptId` konkretnego wdrożenia) jest ignorowany
przez git — patrz `.gitignore` — żeby nie wiązać repozytorium na sztywno
z jednym środowiskiem/kontem Google.
