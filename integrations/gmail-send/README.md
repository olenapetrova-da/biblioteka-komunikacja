# Integracja: gmail-send

## Kontekst

Wysyłka e-maili do czytelników (zmiany godzin otwarcia, nowości w
zbiorach) odbywa się przez **Gmail za pośrednictwem Google Apps Script**
(`GmailApp` / `MailApp`), bez dodatkowych usług płatnych (brak budżetu —
patrz [CLAUDE.md](../../CLAUDE.md)).

## Zasady (wynikające z RODO — patrz docs/RODO.md)

1. **Wysyłka wyłącznie do adresów ze `status_zgody = aktywna`** w arkuszu
   `subscribers` — nigdy bezpośrednio z danych MAK+.
2. **Każdy e-mail musi zawierać link/instrukcję wypisania się**, działający
   bez logowania, oparty o `token_wypisu` z arkusza `subscribers`.
3. Kliknięcie linku wypisu ustawia `status_zgody = wycofana` i zapisuje
   `data_wycofania` — natychmiast, bez opóźnień.
4. Treść maila nie powinna zawierać zbędnych danych osobowych poza tym, co
   konieczne do personalizacji (np. imię, jeśli dostępne).

## Ograniczenia techniczne (Gmail / Apps Script)

- **Limity wysyłki Gmail przez Apps Script**: konto Google (darmowe)
  ma dzienny limit wysyłanych maili (rząd wielkości: ok. 100/dzień dla
  zwykłego konta Gmail, więcej dla Google Workspace — **do zweryfikowania
  aktualnego limitu w dokumentacji Google w momencie implementacji**, bo
  limity się zmieniają).
- Przy większej liczbie subskrybentów niż dzienny limit, wysyłka będzie
  wymagać rozłożenia w czasie (np. wysyłka partiami przez kilka dni) —
  **do zaprojektowania w kolejnym etapie**.
- Apps Script ma też limit czasu wykonania pojedynczego uruchomienia
  (6 minut dla zwykłych kont) — przy dużej liczbie odbiorców wysyłka może
  wymagać podziału na wiele uruchomień (np. przez trigery czasowe).

## Status implementacji

- [ ] Brak kodu — tylko opis procesu i ograniczeń.
- [ ] TODO: zaprojektować szablon maila (godziny otwarcia / nowości).
- [ ] TODO: zaimplementować generowanie i obsługę `token_wypisu`
      (link wypisu obsługiwany przez `doGet` w Apps Script Web App).
- [ ] TODO: zaimplementować logikę wysyłki wsadowej z uwzględnieniem
      limitów Gmail.
- [ ] TODO: zdecydować, czy potrzebny jest log wysyłek (np. do pola
      `data_ostatniej_wysylki` w `subscribers`) — bez przechowywania treści
      maili z danymi osobowymi w logach.

## Przyszłość

Jeśli skala wzrośnie ponad to, co realistycznie obsłuży Gmail/Apps Script
za darmo, rozważyć migrację do dedykowanej usługi mailingowej z warstwą
darmową (do oceny w przyszłości, poza obecnym zakresem).
