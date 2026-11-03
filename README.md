# Docházka – mobilní zadávání do Jarvisu

Jedna statická stránka (PWA) pro rychlé zadávání docházky z mobilu.
Zapisuje přímo do Jarvisu (jarvis.genlive.pro) – do stejné databáze a stejné tabulky
`time_entries`, kterou používá Jarvis. Záznamy se v Jarvisu objeví hned, nic se nesynchronizuje.

## Co umí

- **Timer** – start jedním ťuknutím na naposledy použitý projekt + druh práce, stop, úprava běžícího.
- **Kalendář měsíce** – u každého dne odpracované hodiny, listování do historie.
- **Přidání záznamu na libovolný den** – výběr dne v kalendáři → „Přidat záznam“ →
  ťuknout na „Naposledy“ (projekt + druh práce) → ťuknout na časový blok (8–16, …) → Uložit.
  Přes půlnoc to pozná samo (20:00–02:00 = konec další den).
- **Návrhy z akcí** – dny, kdy jsi přiřazený na akci a nemáš záznam, se ukážou jako návrh
  (oranžová tečka v kalendáři). „Potvrdit“ = hotový záznam. Stejná logika jako v Jarvisu /dochazka.
- **Úprava, kopírování a mazání** záznamů. Při úpravě je povinná poznámka (stejně jako v Jarvisu).
- Kontrola překryvu s jinými záznamy (stejná databázová funkce jako Jarvis).

## Jak to funguje

- Přihlášení stejným účtem jako do Jarvisu (Supabase Auth). Používá veřejný klíč,
  který Jarvis posílá každému prohlížeči; práva řeší databáze (RLS) stejně jako u Jarvisu.
- Žádný vlastní server ani databáze. Stačí hostovat 8 statických souborů z této složky.
- Knihovna `@supabase/supabase-js@2.76.1` (stejná verze jako v Jarvisu) z jsDelivr, s kontrolou integrity.

## Nasazení

Teď běží na GitHub Pages z větve `gh-pages`: https://adamstys.github.io/dochazka/
(změny je potřeba nahrát do `main` i `gh-pages`).

Jde to i na libovolný jiný statický hosting přes HTTPS, např.:

- subdoména na serveru, kde běží Jarvis (Plesk), např. `dochazka.genlive.pro`,
- GitHub Pages, Netlify, Cloudflare Pages.

Pak v mobilu otevřít adresu a „Přidat na plochu“ (iPhone: Sdílet → Přidat na plochu).

### Přihlášení

- **E-mail + heslo** – stejné jako do Jarvisu.
- Kdo chodí do Jarvisu přes Google a heslo nemá, nastaví si ho v Jarvisu přes
  „Zapomněli jste heslo?“ (jarvis.genlive.pro/auth/forgot) nebo v profilu → „Nastavit heslo“.
  Přihlášení přes Google tu není: fungovalo by až po přidání adresy této stránky do Supabase
  projektu Jarvisu (Authentication → URL Configuration → Redirect URLs).

## Soubory

| Soubor | K čemu |
|---|---|
| `index.html` | celá aplikace |
| `manifest.webmanifest`, `icon*.png`, `apple-touch-icon.png`, `icon.svg` | instalace na plochu |
| `sw.js` | otevření aplikace i bez signálu (data se ukládají jen online) |
