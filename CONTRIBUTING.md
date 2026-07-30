# Współpraca

## Stan obecny

Projekt jest na razie prowadzony jednoosobowo. Ten dokument zostawia miejsce
na przyszłych współpracowników (np. wolontariuszy technicznych NGO), gdy
projekt się rozwinie.

## Zasady na przyszłość (szkic)

- **Nigdy nie commituj prawdziwych danych czytelników** — patrz
  [SECURITY.md](SECURITY.md) i `.gitignore`. To zasada nadrzędna nad
  wszystkimi innymi.
- Zmiany w logice dotyczącej zgód RODO (opt-in/opt-out) wymagają
  szczególnej uwagi — patrz [docs/RODO.md](docs/RODO.md). W razie
  wątpliwości pytaj przed zmianą.
- Kod Apps Script: komentarze po polsku (patrz [CLAUDE.md](CLAUDE.md) —
  sekcja "Styl kodu").
- Zmiany w `schemas/` (struktura danych) powinny być uzgodnione, zanim
  wpłyną na `src/` — schemat jest kontraktem między eksportem z MAK+,
  arkuszem subskrybentów i wysyłką maili.
- Przed wprowadzeniem nowej integracji (np. API Instagrama, migracja do
  Supabase) — opisz ją najpierw w `integrations/` lub `docs/`, potem
  implementuj.

## Zgłaszanie zmian

- **TODO**: doprecyzować proces (branch/PR) gdy dołączy więcej niż jedna
  osoba. Na razie zmiany trafiają bezpośrednio na główną gałąź.

## Kontakt

- **TODO**: dane kontaktowe osoby/organizacji odpowiedzialnej za projekt.
