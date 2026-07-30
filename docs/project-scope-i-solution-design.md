# Automatyzacja komunikacji z czytelnikami biblioteki
## Project Scope & Solution Design

**Status:** Draft v0.1
**Data:** 2026-07-30
**Właściciel projektu:** [uzupełnić]

---

## 1. Cel projektu

Biblioteka korzysta z systemu bibliotecznego MAK+, który nie udostępnia otwartego API, ale pozwala na eksport danych do CSV. Celem projektu jest zbudowanie lekkiej, bezbudżetowej automatyzacji, która wykorzysta ten eksport do:

1. Informowania czytelników e-mailem o zmianach godzin otwarcia biblioteki (na początku każdego miesiąca).
2. Informowania czytelników e-mailem o nowościach w zbiorach (w ciągu miesiąca, gdy się pojawiają).
3. Przygotowania treści o nowościach do publikacji na Instagramie biblioteki.

Projekt ma być zgodny z RODO, w szczególności w zakresie podstawy prawnej przetwarzania danych kontaktowych do celów innych niż podstawowa obsługa konta czytelnika.

## 2. Zakres

### W zakresie (in scope)
- Import i czyszczenie danych czytelników z eksportu CSV z MAK+ (proces ręczny — brak API).
- Przechowywanie statusu zgody na komunikację e-mailową, niezależnie od danych w MAK+.
- Mechanizm rezygnacji (opt-out) z otrzymywania wiadomości.
- Wysyłka e-maili: godziny otwarcia + nowości w zbiorach.
- Przygotowanie treści o nowościach gotowej do ręcznej publikacji na Instagramie.
- Podstawowa analityka: rozmiar listy, liczba rezygnacji, skuteczność dostarczania.
- Dokumentacja procesu zgodności z RODO.

### Poza zakresem (out of scope, na tym etapie)
- Automatyczna publikacja na Instagramie przez API (brak dostępu do konta z uprawnieniami API).
- Integracja API z MAK+ (system jej nie udostępnia).
- Płatne narzędzia (Mailchimp, n8n Cloud) — do rozważenia w przyszłości, gdy pojawi się budżet.
- Migracja do Supabase — zaplanowana jako możliwy krok w przyszłości, nie w MVP.
- Segmentacja odbiorców / personalizacja treści (poza podstawowym podziałem wg zgody).

## 3. Kontekst i ograniczenia

| Ograniczenie | Wpływ na projekt |
|---|---|
| Brak API w MAK+ | Import danych wyłącznie przez ręczny eksport CSV, wykonywany cyklicznie (planowane: raz w miesiącu) |
| Brak budżetu | Wykluczone narzędzia płatne na starcie (Mailchimp, n8n Cloud); wybrano Google Apps Script + Google Sheets |
| Brak dostępu do API Instagrama | Publikacja pozostaje ręczna; automatyzacja przygotowuje tylko gotową treść |
| Status zgody RODO na newsletter niejasny | Wymaga weryfikacji w regulaminie/klauzuli informacyjnej biblioteki przed pierwszą wysyłką o nowościach |

## 4. RODO — podstawy prawne i zgody

- Czytelnicy wyrażają zgodę RODO przy rejestracji w bibliotece; zakres tej zgody wymaga weryfikacji (czy obejmuje komunikację o charakterze informacyjnym poza obsługą konta).
- **Do ustalenia przed uruchomieniem wysyłki o nowościach:** czy istniejąca zgoda wystarcza, czy potrzebne jest potwierdzenie (np. jednorazowy e-mail z prośbą o potwierdzenie zgody na newsletter — double opt-in), czy zbieranie nowej zgody przy najbliższym kontakcie z czytelnikiem.
- Komunikacja o **godzinach otwarcia** traktowana jako informacja operacyjna, spójna z podstawowym celem przetwarzania (obsługa czytelnika).
- Każdy e-mail musi zawierać czytelną możliwość rezygnacji (opt-out).
- Status zgody (wyrażona/wycofana, data, źródło) przechowywany osobno od danych z MAK+, w arkuszu/bazie zarządzanej przez projekt.
- Szczegółowy proces opisany docelowo w `docs/RODO.md` w repozytorium projektu.

## 5. Solution Design — architektura rozwiązania

### 5.1 Przepływ danych (MVP)

```
MAK+ (system biblioteczny)
   │  eksport ręczny, cyklicznie (~1x/miesiąc)
   ▼
CSV → Google Drive (folder wejściowy)
   │
   ▼
Google Apps Script
   │  - parsowanie CSV
   │  - czyszczenie i deduplikacja danych
   │  - scalenie z istniejącym statusem zgody
   ▼
Google Sheets ("subscribers" — źródło prawdy o zgodach)
   │
   ├──► Gmail / Apps Script MailApp
   │       - wysyłka: godziny otwarcia (comiesięcznie)
   │       - wysyłka: nowości w zbiorach (wg potrzeby)
   │       - link/formularz rezygnacji zapisujący z powrotem do arkusza
   │
   └──► Looker Studio
           - dashboard: wielkość listy, rezygnacje, dostarczalność

Treść o nowościach (tekst + grafika)
   │  przygotowana automatycznie lub półautomatycznie
   ▼
Publikacja ręczna na Instagramie (do czasu ew. dostępu do API)
```

### 5.2 Stos technologiczny

| Warstwa | Narzędzie | Uzasadnienie |
|---|---|---|
| Import/logika | Google Apps Script | Bezpłatny, natywna integracja z Sheets/Gmail/Drive, zgodny z umiejętnościami zespołu |
| Baza pośrednia | Google Sheets | Zero kosztów, łatwa integracja z Looker Studio, wystarczająca przy obecnej skali |
| Wysyłka e-mail | Gmail / Apps Script MailApp | Bezpłatne w ramach limitów Google |
| Analityka | Looker Studio | Bezpłatne, natywne połączenie z Sheets, znane narzędzie zespołu |
| Kontrola wersji | GitHub + clasp | Wersjonowanie kodu Apps Script, dokumentacja RODO w repo |
| Automatyzacja pomocnicza (opcja) | Make.com (darmowy plan) | Rezerwa, gdyby logika w Apps Script stała się zbyt rozgałęziona |

### 5.3 Ścieżka rozwoju (poza MVP)

- **Mailchimp / Brevo / MailerLite** — rozważyć, gdy pojawi się budżet lub limity darmowego Apps Script/Gmail staną się niewystarczające; wymaga weryfikacji aktualnych limitów przed decyzją.
- **Supabase (Postgres)** — rozważyć przy wzroście skali danych lub potrzebie bardziej złożonej analityki/audytu zgód; Looker Studio i Power BI łączą się z Postgresem bez zmiany warstwy analitycznej.
- **API Instagrama** — do wdrożenia, jeśli/gdy dostęp administracyjny do konta zostanie ustalony organizacyjnie.

## 6. Otwarte pytania / ryzyka

| # | Pytanie / ryzyko | Właściciel | Status |
|---|---|---|---|
| 1 | Czy obecna zgoda RODO obejmuje newsletter o nowościach, czy potrzebna jest osobna zgoda? | [do ustalenia] | Otwarte |
| 2 | Jaki jest dokładny format kolumn eksportu CSV z MAK+? | [do ustalenia] | Otwarte |
| 3 | Kto docelowo będzie miał dostęp do konta Instagram z uprawnieniami do API? | [do ustalenia] | Otwarte |
| 4 | ~~Limity wysyłki Gmail/Apps Script — czy wystarczą przy obecnej wielkości bazy czytelników?~~ | — | **Zamknięte** — patrz 6.1 |

### 6.1 Ustalenie: limity wysyłki Gmail/Apps Script

Przy obecnej bazie 1100 czytelników:

| Typ konta | Limit dzienny (odbiorcy) | Wystarcza na 1100 czytelników w 1 dzień? |
|---|---|---|
| Zwykłe konto gmail.com (konsumenckie) | 100/dzień | Nie — wymagałoby rozłożenia wysyłki na ~11 dni |
| Google Workspace | 1500/dzień | Tak |

**Rekomendacja:** sprawdzić, czy biblioteka kwalifikuje się do **Google Workspace for Nonprofits** (bezpłatny program dla organizacji non-profit, zwykle po weryfikacji przez TechSoup lub odpowiedni program w Polsce). To rozwiązuje limit wysyłki bez naruszania budżetu projektu i jest preferowaną ścieżką nad rozkładaniem wysyłki na wiele dni.

**Do zrobienia:** zweryfikować status kwalifikacji biblioteki do Google Workspace for Nonprofits jako jeden z pierwszych kroków wdrożenia.

## 7. Kryteria sukcesu (MVP)

- Comiesięczna wysyłka o godzinach otwarcia działa bez ręcznej interwencji poza eksportem CSV.
- Czytelnik może zrezygnować z otrzymywania wiadomości jednym kliknięciem, a status zapisuje się automatycznie.
- Dashboard w Looker Studio pokazuje aktualną wielkość listy i liczbę rezygnacji.
- Proces zgodności z RODO jest udokumentowany i możliwy do przedstawienia w razie kontroli.

## 8. Analityka — Looker Studio (rozszerzalność)

Dashboard w Looker Studio (patrz pkt 5.2) w MVP obejmuje podstawowe metryki: wielkość listy, liczbę rezygnacji, dostarczalność (patrz pkt 7). Dashboard ma być zaprojektowany jako **rozszerzalny** — kolejne wykresy analityczne zostaną zdefiniowane w osobnym etapie, po zebraniu wymagań.

**Do ustalenia w kolejnym kroku:** lista dodatkowych wykresów/metryk (np. trendy w czasie, porównania miesiąc do miesiąca, segmentacja czytelników, skuteczność poszczególnych kampanii). Ponieważ Looker Studio łączy się bezpośrednio z Google Sheets, dodawanie nowych wykresów w przyszłości nie powinno wymagać zmian w warstwie danych — jedynie w strukturze arkusza, jeśli nowa metryka wymaga dodatkowej kolumny/logiki zliczania.

## 9. Kolejne kroki

1. Ustalić podstawę prawną / zakres zgody na newsletter o nowościach (pkt 6.1).
2. Wykonać pierwszy eksport CSV z MAK+ i udokumentować jego strukturę w `schemas/`.
3. Zaimplementować import + czyszczenie danych w Apps Script.
4. Zaimplementować mechanizm opt-out.
5. Skonfigurować pierwszą wysyłkę testową na małej grupie.
6. Zbudować podstawowy dashboard w Looker Studio.
7. Zweryfikować kwalifikację biblioteki do Google Workspace for Nonprofits.
8. Zebrać i zdefiniować listę dodatkowych wykresów analitycznych dla Looker Studio (poza MVP).
