# Home, Kassa en Refunds koppelen aan de live omgeving

Nu draait alles in de testomgeving (repository `Stocktelling-test-omgeving`, Supabase-testproject).
Dit stappenplan zet de nieuwe modules later live. **Nog niet uitvoeren.**

## Hoe het opgebouwd is

- Alle modules lezen hun Supabase-gegevens uit één bestand: `gedeeld/config.js`.
  De code zelf bevat geen projectadressen of sleutels.
- Home en Kassa gebruiken enkel tabellen die live al bestaan (`hotels`, `editors`, `hotel_months`, `locks`).
  De kassa bewaart haar gegevens in de maandgegevens van de stocktelling, onder `kassa`.
- Refunds heeft twee nieuwe tabellen nodig (`refunds`, `hotel_info`). Die maak je met `database/refunds.sql`.
- `stocktelling/index.html` is dezelfde stocktelling als live (de knop "Maand afsluiten" werkt hier wel meteen bij).

## Stappen

1. **Back-up van live.** Supabase live project > Database > Backups, of exporteer de tabellen.
2. **Database live voorbereiden.** Open het live project > SQL Editor, plak `database/refunds.sql` en klik op Run.
   Het script voegt enkel nieuwe tabellen toe en raakt de stocktelling-gegevens niet. Het mag opnieuw uitgevoerd worden.
3. **Bestanden naar de live repository** (`Stocktelling`) kopiëren:
   - de mappen `home/`, `stocktelling/`, `kassa/`, `refunds/`, `gedeeld/`, `database/`
   - `index.html` uit de hoofdmap (stuurt door naar `home/`; oude links en app-icoontjes blijven werken)
   - `README.md` en dit bestand
4. **`gedeeld/config.js` in de live repository** moet de **live** gegevens bevatten:
   ```js
   var SUPABASE_URL = "https://gvmnjkwjznilaulagasg.supabase.co";
   var SUPABASE_ANON_KEY = "sb_publishable_njkHHOnAfSNyhnpuBSeJJQ_rvqfv0fr";
   ```
   (Dat zijn de gegevens uit het huidige live `config.js`. Kopieer dus **niet** de test-`config.js`.)
5. **Oude bestanden in de live hoofdmap opruimen** zodra de nieuwe versie werkt:
   `config.js`, `supabase-adapter.js`, `js/supabase.min.js` en het ongebruikte bestand `supabase`.
   `sw.js`, `manifest.webmanifest` en de icoontjes blijven in de hoofdmap staan.
6. **Testen op live:** aanmelden als beheerder en als hotel, elke module openen, een test-refund invullen,
   afsluiten, uitbetalen en daarna verwijderen.

## Eén module apart live zetten

Dat kan ook. Bijvoorbeeld enkel Refunds: stap 2, en daarna enkel de map `refunds/` (met `gedeeld/` als die er nog niet is) kopiëren.
Op Home staat dan nog geen knop zolang `home/` niet live staat; de module is wel rechtstreeks bereikbaar via `.../refunds/`.
