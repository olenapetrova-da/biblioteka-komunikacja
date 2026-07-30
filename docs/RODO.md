# RODO — ochrona danych osobowych czytelników

Ten dokument opisuje, jak projekt spełnia wymogi RODO (ogólnego
rozporządzenia o ochronie danych, UE 2016/679). To dokument roboczy —
sekcje oznaczone **TODO** wymagają uzupełnienia przez organizację (np.
dokładna nazwa administratora danych, dane kontaktowe IOD, jeśli
wyznaczony).

## 1. Administrator danych

**TODO:** pełna nazwa organizacji (NGO), adres, dane kontaktowe do spraw
związanych z ochroną danych (e-mail/IOD, jeśli dotyczy — organizacje
przetwarzające dane na dużą skalę mogą być zobowiązane do wyznaczenia
Inspektora Ochrony Danych; biblioteka NGO małej skali zwykle nie musi, ale
warto to zweryfikować).

## 2. Jakie dane przetwarzamy i po co

| Cel przetwarzania | Dane | Podstawa prawna (wstępnie) |
|---|---|---|
| Informacje operacyjne (np. zmiana godzin otwarcia) | e-mail, ew. imię | Do ustalenia — patrz sekcja 3 |
| Informacje o nowościach w zbiorach (charakter zbliżony do marketingu/informacji handlowej) | e-mail, ew. imię | Zgoda (art. 6 ust. 1 lit. a RODO) + zgoda wg ustawy o świadczeniu usług drogą elektroniczną |
| Powiązanie z kontem czytelnika w MAK+ (jeśli potrzebne do personalizacji) | identyfikator czytelnika z MAK+ | Do ustalenia — patrz sekcja 3 |

### Dlaczego to nie jest oczywiste

Wysyłka informacji **czysto operacyjnych** (np. "biblioteka będzie zamknięta
w te dni") do osób, które są aktywnymi czytelnikami, można argumentować jako
**prawnie uzasadniony interes** administratora (art. 6 ust. 1 lit. f) —
mieści się w zwykłej obsłudze usługi bibliotecznej. Natomiast wysyłka
informacji o **nowościach w zbiorach** ma charakter zbliżony do komunikacji
marketingowej/informacyjnej i w praktyce polskiej (ustawa o świadczeniu
usług drogą elektroniczną, art. 10) wymaga **odrębnej zgody** na
otrzymywanie informacji handlowych drogą elektroniczną.

**Decyzja projektowa (rekomendacja, do potwierdzenia przez organizację):**
żeby uniknąć rozróżniania dwóch reżimów prawnych dla dwóch typów maili
(co komplikuje logikę i ryzyko pomyłki), projekt traktuje **całą
komunikację e-mailową jako wymagającą zgody (opt-in)** — zarówno
informacje o godzinach otwarcia, jak i o nowościach. Jest to podejście
bardziej ostrożne niż wymagane minimum, ale prostsze do wdrożenia i
utrzymania poprawnie w małym zespole bez działu prawnego.

Schemat arkusza subskrybentów (patrz
[schemas/subscribers-sheet.md](../schemas/subscribers-sheet.md)) przewiduje
pole `typ_zgody`, więc w przyszłości można rozdzielić zgody na poszczególne
typy komunikacji, jeśli organizacja zdecyduje się na bardziej granularne
podejście.

## 3. Zbieranie zgody

**TODO:** opisać dokładny proces zbierania zgody, gdy zostanie ustalony.
Możliwe kanały (do wyboru/uzupełnienia):
- formularz papierowy przy zapisie do biblioteki (checkbox zgody, osobno
  od zgody na przetwarzanie danych czytelnika przez MAK+),
- formularz online (np. Google Forms) linkowany np. na stronie/w bibliotece,
- zgoda potwierdzona mailowo (double opt-in) po zebraniu adresu e-mail.

Zasada: **zgoda na maile nigdy nie jest domniemana z faktu bycia
czytelnikiem biblioteki w MAK+.** Sam fakt posiadania karty bibliotecznej
nie oznacza zgody na komunikację e-mailową — dlatego status zgody jest
śledzony w osobnym arkuszu `subscribers`, niezależnym od danych
importowanych z MAK+ (patrz sekcja 5).

## 4. Wycofanie zgody (opt-out)

- Każdy e-mail wysyłany w ramach projektu **musi** zawierać czytelny i
  działający sposób rezygnacji (link/instrukcję wypisania się).
- Wycofanie zgody musi być **co najmniej tak proste**, jak jej wyrażenie
  (wymóg RODO — art. 7 ust. 3).
- Po wycofaniu zgody:
  - pole `status_zgody` w arkuszu `subscribers` zmienia się na wartość
    oznaczającą wycofanie,
  - zapisywana jest `data_wycofania`,
  - dana osoba **nie otrzymuje kolejnych maili**, niezależnie od tego, czy
    nadal figuruje w danych z MAK+.
- **TODO**: dokładny mechanizm techniczny wypisania (link z unikalnym
  tokenem obsługiwany przez Apps Script — do zaimplementowania w kolejnym
  etapie).

## 5. Rozdzielenie danych MAK+ od statusu zgody

To kluczowy wymóg tego projektu: **dane z MAK+ (kto jest czytelnikiem) i
status zgody na maile (kto zgodził się na komunikację) to dwa osobne
źródła prawdy.**

- Import z MAK+ dostarcza listę czytelników (i ewentualnie ich e-maile),
  ale **nigdy sam w sobie nie tworzy ani nie zmienia statusu zgody**.
- Arkusz `subscribers` (patrz
  [schemas/subscribers-sheet.md](../schemas/subscribers-sheet.md)) jest
  jedynym źródłem prawdy o zgodzie. Nowy e-mail z eksportu MAK+ pojawia się
  w arkuszu `subscribers` ze statusem zgody domyślnie **"brak zgody"**
  (nigdy automatycznie "zgoda aktywna").
- Usunięcie/zmiana rekordu w MAK+ (np. czytelnik już nieaktywny) **nie
  wycofuje automatycznie zgody** — to wymaga świadomej decyzji czytelnika
  (opt-out) lub administratora.

## 6. Retencja danych

**TODO:** dokładne okresy retencji do ustalenia z organizacją. Wstępna
propozycja (do zatwierdzenia):

- **Dane w arkuszu `subscribers` osoby z aktywną zgodą**: przechowywane
  tak długo, jak zgoda jest aktywna.
- **Dane osoby, która wycofała zgodę**: adres e-mail powinien zostać
  usunięty (lub zanonimizowany) po okresie **TODO (np. 30/90 dni)**
  potrzebnym do potwierdzenia skuteczności wypisania, chyba że dłuższe
  przechowywanie jest wymagane do celów dowodowych (np. potwierdzenie, że
  dana osoba faktycznie się wypisała — wtedy przechowuje się minimalny
  zestaw danych: zhashowany e-mail + data wypisania, nie treść komunikacji).
- **Surowe pliki eksportu CSV z MAK+**: nie powinny być przechowywane
  dłużej niż to konieczne do zaimportowania danych do arkusza — usuwać po
  udanym imporcie (nigdy nie trzymać trwale poza MAK+, tym bardziej nie w
  repozytorium — patrz `.gitignore` i [SECURITY.md](../SECURITY.md)).

## 7. Prawa osób, których dane dotyczą

Czytelnicy mają prawo do (art. 15–22 RODO):
- dostępu do swoich danych,
- sprostowania,
- usunięcia ("prawo do bycia zapomnianym"),
- ograniczenia przetwarzania,
- przenoszenia danych,
- sprzeciwu,
- wycofania zgody w dowolnym momencie (patrz sekcja 4).

**TODO:** opisać, jak organizacja obsługuje takie żądania w praktyce
(np. adres e-mail kontaktowy, czas realizacji — RODO wymaga odpowiedzi
bez zbędnej zwłoki, standardowo w ciągu miesiąca).

## 8. Bezpieczeństwo danych

- Dostęp do arkusza Google Sheets i konta Gmail ograniczony do osób, które
  faktycznie potrzebują dostępu (zasada minimalizacji dostępu).
- Żadne dane osobowe czytelników nie trafiają do repozytorium git —
  patrz [SECURITY.md](../SECURITY.md).
- **TODO**: opisać proces zgłaszania i obsługi ewentualnego naruszenia
  ochrony danych (art. 33/34 RODO) — kto jest powiadamiany, w jakim czasie
  (72h do organu nadzorczego, jeśli ryzyko naruszenia praw i wolności osób).

## 9. Rejestr zmian tego dokumentu

- **TODO**: prowadzić krótką historię istotnych zmian w podejściu do RODO
  (np. zmiana podstawy prawnej, zmiana okresu retencji), żeby było wiadomo,
  kiedy i dlaczego coś się zmieniło.
