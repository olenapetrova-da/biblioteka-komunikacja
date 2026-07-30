# Integracja: instagram-content

## Kontekst

Obecnie **brak API/automatycznej publikacji na Instagramie** — publikacja
jest zawsze **ręczna** (osoba odpowiedzialna za social media kopiuje
przygotowaną treść i publikuje ją samodzielnie w aplikacji Instagram).

Ten moduł ma za zadanie **przygotować gotową treść** (tekst posta o
nowościach w zbiorach, ew. sugestie grafiki), a nie publikować ją
automatycznie.

## Proces (docelowy, na razie bez kodu)

1. Na podstawie danych o nowościach w zbiorach (źródło danych — **do
   ustalenia**: czy z eksportu MAK+, czy ręczne wprowadzanie przez
   bibliotekarza) generowany jest szkic treści posta.
2. Treść trafia do miejsca łatwo dostępnego dla osoby publikującej — np.
   osobny arkusz Google Sheets lub Google Doc (**do ustalenia**).
3. Osoba odpowiedzialna za Instagram ręcznie kopiuje treść i publikuje
   post, ewentualnie dobierając zdjęcie.

## Status implementacji

- [ ] Brak kodu — tylko opis procesu.
- [ ] TODO: ustalić źródło danych o nowościach (MAK+? ręczne wejście?).
- [ ] TODO: ustalić format/miejsce przygotowanej treści (arkusz? dokument?).
- [ ] TODO: zaprojektować szablon treści posta (długość, hashtagi, ton).

## Przyszłość: Instagram Graph API

Jeśli w przyszłości pojawi się potrzeba i zasoby na automatyzację
publikacji, naturalnym kierunkiem jest **Instagram Graph API** (wymaga
konta biznesowego/twórcy powiązanego ze stroną na Facebooku oraz procesu
weryfikacji aplikacji przez Meta). To wykracza poza obecny zakres
(brak budżetu/czasu na proces weryfikacji), ale moduł jest wydzielony
jako osobna integracja właśnie po to, żeby dało się go podmienić na
automatyczną publikację bez zmiany reszty systemu.
