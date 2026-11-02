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

Libovolný statický hosting přes HTTPS, např.:

- subdoména na serveru, kde běží Jarvis (Plesk), např. `dochazka.genlive.pro`,
- GitHub Pages, Netlify, Cloudflare Pages.

Pak v mobilu otevřít adresu a „Přidat na plochu“ (iPhone: Sdílet → Přidat na plochu).

### Přihlášení

- **E-mail + heslo** funguje hned.
- **Google** funguje až po přidání adresy této stránky do Supabase projektu Jarvisu:
  Authentication → URL Configuration → Redirect URLs (např. `https://dochazka.genlive.pro/**`).
  Bez toho Google přesměruje zpět do Jarvisu místo sem.

## Soubory

| Soubor | K čemu |
|---|---|
| `index.html` | celá aplikace |
| `manifest.webmanifest`, `icon*.png`, `apple-touch-icon.png`, `icon.svg` | instalace na plochu |
| `sw.js` | otevření aplikace i bez signálu (data se ukládají jen online) |
