# Architecture Decision Records (ADR)

Ten dokument zawiera zapis decyzji technicznych podjętych w trakcie projektu — dlaczego wybrano dane rozwiązanie, jakie były alternatywy i jakie ma to konsekwencje. Nowe wpisy dodawane są na górze (najnowsze pierwsze), z kolejnym numerem ADR.

Wpis dodaje się zawsze, gdy: (a) podjęto decyzję techniczną bez wcześniejszego pytania w czacie, bo była oczywista/jednoznaczna, albo (b) decyzja zapadła po dyskusji i warto ją utrwalić, żeby nie wracać do tego samego pytania w przyszłości.

---

## Format wpisu (szablon)

```
## ADR-XXX: [Krótki tytuł decyzji]

**Data:** RRRR-MM-DD
**Status:** Proponowane / Zaakceptowane / Zastąpione przez ADR-YYY

**Kontekst**
Jaki problem/pytanie wywołało potrzebę decyzji.

**Rozważane opcje**
1. Opcja A — krótki opis
2. Opcja B — krótki opis

**Decyzja**
Którą opcję wybrano i w jednym zdaniu dlaczego.

**Konsekwencje**
Co to oznacza w praktyce (pozytywne i negatywne skutki, ograniczenia na przyszłość).
```

---

## Przykładowy wpis

## ADR-000: Google Sheets jako baza pośrednia zamiast Supabase (przykład)

**Data:** 2026-07-30
**Status:** Zaakceptowane

**Kontekst**
Potrzebna baza pośrednia do przechowywania danych czytelników i statusu zgody RODO, zasilana ręcznym eksportem CSV z MAK+. Do wyboru: Google Sheets lub Supabase (Postgres).

**Rozważane opcje**
1. Google Sheets — zero kosztów, natywna integracja z Apps Script i Looker Studio
2. Supabase (Postgres) — bardziej skalowalne, lepsza audytowalność zgód, ale wymaga większego nakładu wdrożeniowego

**Decyzja**
Start z Google Sheets jako MVP, ze świadomą ścieżką migracji do Supabase w przyszłości, jeśli pojawi się potrzeba głębszej analityki lub większej skali danych.

**Konsekwencje**
Zero dodatkowych kosztów i infrastruktury na start. W przyszłości migracja nie wymaga zmiany warstwy analitycznej (Looker Studio łączy się też z Postgresem), tylko warstwy przechowywania danych.

---

<!-- Kolejne wpisy dodawaj powyżej tej linii, z rosnącym numerem ADR-001, ADR-002 itd. -->
