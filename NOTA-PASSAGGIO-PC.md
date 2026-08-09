# Nota passaggio PC (10 agosto 2026)

Lavoro di traduzione EN/IT chiuso e pushato. Tutto sta su `ercole-dev`, un branch per repo.

## Cosa è stato pushato

| Repo | Branch | Commit | Contenuto |
|---|---|---|---|
| posteat-react-web | `ercole-dev` (nuovo) | `2cf0a796` | i18next EN/IT, LanguageSwitcher, validazioni yup localizzate, via il widget Google Translate |
| posteat-react-web-admin | `ercole-dev` | `4289c22` | idem per la dashboard admin, piu rename rotta `users/[uuid].tsx` in `users/$uuid.tsx` |
| posteat-vendor-web | `ercole-dev` | `ae61726` | idem per la dashboard vendor |
| posteat-laravel-api | `ercole-dev` | `4126eb6`, `c8b61b6` | `DemoDataSeeder` per i test locali, piu tre script usa e getta per il flusso scontrino |
| posteat-react-native-mobile | `ercole-dev` | `2f56107`, `641874c` | fix refetch catalogo rewards, `impact_action` reso testuale, script `avvia-app.sh` |

Attenzione a react-web: prima non aveva un `ercole-dev` sul remote. L'ho creato partendo da `develop`, quindi il branch contiene i 189 file di i18n che su `develop` non ci sono ancora.

## Come allinearsi sull'altro PC

```bash
cd <cartella-posteat>
for r in posteat-react-web posteat-react-web-admin posteat-vendor-web \
         posteat-laravel-api posteat-react-native-mobile; do
  git -C $r fetch origin
  git -C $r checkout ercole-dev
  git -C $r pull
done
```

Prima di far girare qualcosa, reinstalla le dipendenze dove sono cambiate:

- `posteat-react-web-admin`: `npm install` (nel commit sono aggiornati sia `package-lock.json` sia `yarn.lock`)
- `posteat-vendor-web`: `yarn install` (aggiunte `i18next` e `react-i18next`, `yarn.lock` aggiornato)
- `posteat-react-web`: niente di nuovo da installare, il commit toglie solo `i18next-browser-languagedetector` da `package.json`

## Cosa è rimasto fuori dai commit

Roba locale, non serve portarla dietro:

- `.claude/` in admin e vendor
- `vite.config.ts.timestamp-*.mjs` in admin, artefatti temporanei di Vite
- `package-lock.json` non tracciato in vendor, avanzo di un `npm install` sbagliato visto che quel repo usa yarn
- `nul` nella root di laravel-api, file spazzatura di Windows
- `.yarn/install-state.gz` ovunque, stato locale di yarn

## Punti aperti

1. **react-web**: `ercole-dev` non è ancora rientrato in `develop`. Serve una PR: https://github.com/Post-Eat/posteat-react-web/pull/new/ercole-dev
2. **Copertura traduzioni**: la lista di cosa manca sta in `TRANSLATION_GUIDE.md` nella root di lavoro. Restano fuori sidebar, footer, messaggi di validazione form lato admin e buona parte della dashboard vendor.
3. **laravel-api**: `DemoDataSeeder` è volutamente commentato in `DatabaseSeeder`. Si lancia a mano e solo in locale con `php artisan db:seed --class=DemoDataSeeder`. È idempotente, rilanciarlo non duplica niente.
4. **Script scontrino**: `seed_demo.php`, `seed_restaurant.php` e `seed_run.php` stanno nella root di laravel-api. Sono appoggi temporanei per il test end to end, da togliere quando l'OCR vero (Gemini) sostituisce lo stub.
