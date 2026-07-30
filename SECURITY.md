# Bezpieczeństwo i ochrona danych

## Zasada nadrzędna

**Nigdy nie commituj prawdziwych danych czytelników do tego repozytorium.**

Dotyczy to w szczególności:
- eksportów CSV z MAK+ (dowolnej wersji, dowolnego fragmentu),
- adresów e-mail, numerów kart bibliotecznych, imion i nazwisk czytelników,
- zrzutów ekranu arkuszy Google Sheets zawierających prawdziwe dane,
- logów zawierających dane osobowe (np. treść wysłanych maili z realnymi
  adresami odbiorców).

Repozytorium jest miejscem na **kod i strukturę**, nie na dane. Prawdziwe
dane czytelników żyją wyłącznie w Google Sheets / MAK+, objęte kontrolą
dostępu Google Workspace, nigdy w git.

`.gitignore` blokuje pliki `.csv`, pliki `.env`, oraz pliki/foldery
oznaczone sufiksem `-real` / `-export`, ale **to zabezpieczenie pomocnicze,
nie zwalnia z odpowiedzialności** — sprawdzaj `git status` i `git diff`
przed każdym commitem.

## Co zrobić, jeśli prawdziwe dane trafiły do repozytorium

1. Nie panikuj, ale działaj szybko — jeśli repo jest publiczne lub ma
   wielu współpracowników, dane mogły zostać już pobrane.
2. Usuń dane z historii git (nie tylko z bieżącego commita) —
   w razie wątpliwości poproś o pomoc, zanim wykonasz operacje
   nieodwracalne (np. force-push, przepisanie historii).
3. Jeśli repozytorium było publiczne lub dostępne dla osób trzecich,
   rozważ, czy doszło do naruszenia ochrony danych osobowych w rozumieniu
   RODO (art. 33/34) — patrz [docs/RODO.md](docs/RODO.md).
4. Zgłoś incydent osobie odpowiedzialnej za projekt / ochronę danych w
   organizacji. **TODO**: uzupełnić dane kontaktowe osoby odpowiedzialnej
   za RODO w organizacji, gdy będą znane.

## Zgłaszanie problemów bezpieczeństwa

- **TODO**: docelowy kanał zgłoszeń (na razie brak — projekt jednoosobowy).
  Gdy dołączą kolejne osoby, uzupełnić np. dedykowany adres e-mail lub
  prywatne zgłoszenie (nie publiczny issue), jeśli problem dotyczy
  wycieku danych osobowych.

## Dostęp i uprawnienia

- **TODO**: opisać, kto ma dostęp do arkusza Google Sheets z danymi
  czytelników i do konta Gmail używanego do wysyłki, gdy zostanie to
  ustalone.
- Klucze/identyfikatory (np. Sheet ID) trzymamy w `.env` (lokalnie, poza
  repozytorium) — patrz `.env.example`.
