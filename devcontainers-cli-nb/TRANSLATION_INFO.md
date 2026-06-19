# Oversettingsinformasjon - Dev Container CLI (Norsk Bokmål)

## Oversikt
- **Språk:** Norsk Bokmål (nb)
- **Status:** ✅ Aktiv
- **Sist oppdatert:** 2026-06-19
- **Versjon:** 1.0.0

## Oversettede filer
- ✅ README.md - Fullstendig oversettelse
- ✅ language.json - Metadata
- ✅ translations.json - Translatede nøkler

## Bruk som Pionex Custom Language

### For Pionex-prosjektet:

1. **Konfigurer URL i Pionex:**
   ```
   https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/
   ```

2. **Språk-ID:** `nb` (Norsk Bokmål)

3. **Last inn ressurser:**
   - Bruk `language.json` for metadata
   - Bruk `translations.json` for oversettede strenger
   - Bruk `README.md` for dokumentasjon

### Filstruktur
```
devcontainers-cli-nb/
├── README.md                 # Hovedoversettelse
├── language.json             # Språkmetadata
├── translations.json         # Nøkkel-verdi oversettelser
└── TRANSLATION_INFO.md       # Denne filen
```

## Implementering

### I Pionex:
```javascript
import * as norwegianBokmål from 'https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/language.json';
import * as translations from 'https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/translations.json';
```

## Bidrag og Oppdateringer

Hvis du har forslag til forbedringer:
1. Opprett en issue på https://github.com/magholmen91/pionex-l10n
2. Eller lag en pull request med endringer

## Lisens

Denne oversettingen følger den samme lisensen som originalprosjektet (MIT).