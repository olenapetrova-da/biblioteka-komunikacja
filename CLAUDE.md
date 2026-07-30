# CLAUDE.md — kontekst projektu

Ten plik zawiera kontekst dla przyszłych sesji Claude Code pracujących nad tym
projektem. Przeczytaj go przed rozpoczęciem pracy.

## Cel projektu

Automatyzacja komunikacji z czytelnikami biblioteki (organizacja NGO, Polska).
Biblioteka korzysta z systemu bibliotecznego **MAK+**, który nie ma API —
jedyny sposób pozyskania danych czytelników to ręczny eksport do CSV przez
pracownika biblioteki.

Projekt ma:
1. Importować i czyścić eksport CSV z MAK+.
2. Łączyć dane w Google Sheets (baza pośrednia).
3. Wysyłać e-maile do czytelników (zmiany godzin otwarcia, nowości w
   zbiorach) przez Gmail / Google Apps Script.
4. Przygotowywać treści o nowościach do **ręcznej** publikacji na
   Instagramie (na razie brak API — Instagram Graph API jako możliwy
   kierunek w przyszłości).

## Na początku każdej sesji

1. Przeczytaj `docs/project-scope-i-solution-design.md` — zrozum zakres projektu i architekturę rozwiązania
2. Przeczytaj backlog w Notion: [Kolejność wykonania](https://app.notion.com/p/58bbcef8c6c544e8b550b2019ffe809b?v=3ad94a8e216281e1b36c000c3205a4b0) — sprawdź aktualny status i co jest w toku
3. Przeczytaj `docs/ADR.md`, jeśli istnieje — zrozum decyzje już podjęte i ich uzasadnienie
4. Przeczytaj `docs/RODO.md`, jeśli istnieje — zrozum ustalony proces zgody i rezygnacji (opt-out), nie zmieniaj go bez potwierdzenia
5. Zapytaj, nad którym zadaniem pracujemy, albo potwierdź kolejne zadanie wg pola `Order` w Notion

## Stos technologiczny

- **Google Apps Script** — główna logika (import, czyszczenie, wysyłka); kod `.gs`/`.js` docelowo w `src/`; wdrażane przez `clasp`, brak lokalnego hostingu
- **Google Sheets** — baza pośrednia na start, arkusz `subscribers` jako źródło prawdy o statusie zgody (struktura: [schemas/subscribers-sheet.md](schemas/subscribers-sheet.md)), niezależna od danych z MAK+. Możliwa przyszła migracja do **Supabase**, jeśli skala/potrzeby wzrosną (patrz ADR, jeśli decyzja zapadnie)
- **Gmail / Apps Script MailApp** — wysyłka e-maili; limit 100 odbiorców/dzień na koncie konsumenckim, 1500/dzień na Google Workspace (weryfikacja kwalifikacji do Google Workspace for Nonprofits jest w toku — zadanie #2 w backlogu)
- **Looker Studio** — analityka, nie na obecnym etapie; MVP docelowo obejmie wielkość listy, rezygnacje, dostarczalność, łączy się natywnie z Google Sheets
- **clasp** — synchronizacja kodu Apps Script z tym repozytorium
- **MAK+** — system biblioteczny bez API; dane wyłącznie przez ręczny eksport CSV; format dokumentowany w `schemas/`
- **Instagram** — brak dostępu do API konta na razie; automatyzacja przygotowuje tylko gotową treść (kod w `integrations/instagram-content` pisany tak, by dało się go łatwo podłączyć pod API w przyszłości, bez zakładania, że ono już istnieje), publikacja ręczna

## Ograniczenia

- **Brak budżetu** — wszystkie rozwiązania muszą mieścić się w darmowych warstwach (Google Workspace/Gmail free tier, Google Sheets, Apps Script). Unikać usług płatnych (n8n Cloud, Mailchimp itp.), chyba że jawnie uzgodnione.
- **Brak API do MAK+** — import danych to zawsze ręczny eksport CSV wykonywany przez pracownika biblioteki. Kod musi zakładać ręczny, nieregularny import, a nie zautomatyzowane pobieranie.
- **Brak API do Instagrama (na razie)** — przygotowanie treści = generowanie tekstu/grafik do ręcznej publikacji.
- **Wymogi RODO są priorytetem** — patrz [docs/RODO.md](docs/RODO.md). Kluczowe zasady:
  - Zgoda na maile (opt-in) i możliwość rezygnacji (opt-out) w każdej wiadomości — to wymóg, nie opcja.
  - Status zgody na newsletter o nowościach wymaga ustalenia (zadanie #1 w backlogu) przed uruchomieniem tej wysyłki; komunikacja o godzinach otwarcia traktowana jako operacyjna.
  - Status zgody jest śledzony **osobno** od danych z MAK+ (MAK+ nie jest źródłem prawdy o zgodzie — patrz [schemas/subscribers-sheet.md](schemas/subscribers-sheet.md)).
  - Żadne prawdziwe dane czytelników nie trafiają do repozytorium (patrz `.gitignore` i [SECURITY.md](SECURITY.md)).

## Styl kodu

- Kod Apps Script (`.gs`/`.js` w `src/`): **komentarze i nazwy funkcji publicznych po polsku**, żeby był zrozumiały dla przyszłych wolontariuszy/opiekunów projektu bez zaplecza technicznego po angielsku. Nazwy zmiennych mogą być po angielsku, jeśli to bardziej naturalne (konwencje JS), ale komentarze wyjaśniające "dlaczego" — po polsku.
- Brak komentarzy opisujących oczywiste "co" — tylko nieoczywiste "dlaczego" (ograniczenia MAK+, wymogi RODO, limity Gmail/Apps Script).

## W trakcie pracy

- Przed podjęciem nieoczywistej decyzji technicznej — przedstaw opcje i zapytaj o potwierdzenie
- Jeśli podejmiesz decyzję samodzielnie (bo była oczywista), od razu dodaj wpis do `docs/ADR.md`
- Nigdy nie commituj: prawdziwych danych czytelników, plików `.csv` z eksportu MAK+, `.env` — zgodnie z `.gitignore` (patrz `SECURITY.md`)
- Przed napisaniem/testowaniem logiki wysyłki e-maili sprawdź, z jakiego typu konta korzystamy (konsumenckie gmail.com = 100 odbiorców/dzień, Google Workspace = 1500/dzień) — patrz `docs/project-scope-i-solution-design.md` pkt 6.1
- Po ukończeniu zadania zaznacz je jako zrobione w Notion i zrób commit

## Po ukończeniu zadania

- Zaktualizuj Notion (oznacz jako Done, przenieś kolejne zadanie do "In Progress")
- Dodaj wpis do `docs/ADR.md`, jeśli podjęto jakieś decyzje
- Commit z jasnym komunikatem: prefiks `feat:`, `fix:`, `chore:` lub `docs:`
- Powiedz mi, że zadanie jest zrobione i jaki jest następny krok

## Status projektu

Etap: **szkielet repozytorium** (dokumentacja + struktura, kod w budowie zgodnie z kolejnością w Notion).

Kolejne kroki wynikają bezpośrednio z pola `Order` w bazie Notion (patrz link wyżej), m.in.:
- Uzupełnienie rzeczywistej struktury kolumn eksportu z MAK+.
- Implementacja importu/czyszczenia danych w Apps Script.
- Implementacja formularza zgody i mechanizmu opt-out.
- Implementacja wysyłki e-maili przez Gmail (limity wysyłki!).
- Konfiguracja clasp i pierwszego wdrożenia.

Notion jest źródłem prawdy o statusie zadań — jeśli ten dokument i Notion się rozjadą, wygrywa Notion; zaktualizuj tę sekcję przy okazji, jeśli zauważysz rozbieżność.
