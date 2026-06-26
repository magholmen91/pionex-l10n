# Setup Guide for Pionex Custom Language

## 📋 Oversikt

Dette er en norsk Bokmål-oversettelse optimalisert for bruk som **custom language i Pionex**.

**Status:** ✅ Validert og klar for bruk

---

## 🚀 Rask Start

### 1. Konfigurer Pionex

```javascript
// I Pionex-konfigurasjonen din
const customLanguage = {
  id: "nb",
  name: "Norsk Bokmål",
  url: "https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/",
  format: "json"
};
```

### 2. Last inn språkpakken

```javascript
import config from 'https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/config.json';
import language from 'https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/language.json';
import translations from 'https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/translations.json';

const norwegianBokmål = {
  ...language,
  ...translations,
  meta: config.languageConfig
};
```

### 3. Registrer som custom language

```javascript
Pionex.languages.register(norwegianBokmål);
Pionex.setLanguage('nb');
```

---

## 📁 Filstruktur

| Fil | Formål | Størrelse |
|-----|--------|----------|
| `config.json` | Pionex-konfigurering | ✅ Validert |
| `language.json` | Språkmetadata | ✅ Validert |
| `translations.json` | Oversatte strenger | ✅ Validert |
| `manifest.json` | Pakkeinformasjon | ✅ Validert |
| `validation-schema.json` | JSON Schema | ✅ Validert |
| `README.md` | Dokumentasjon | ✅ Norsk |

---

## ✅ Validering

Alle filer er validert mot JSON Schema:

- ✅ `language.json` - Språkkode: `nb` (ISO 639-1)
- ✅ `translations.json` - 22 oversatte nøkler
- ✅ `config.json` - Pionex-kompatibel
- ✅ `manifest.json` - Pakkemetadata

### Validerte egenskaper:

```json
{
  "languageCode": "nb",
  "languageName": "Norsk Bokmål",
  "translationCount": 22,
  "formatVersion": "1.0.0",
  "status": "active",
  "dataIntegrity": "✅ Valid",
  "charsetSupport": "UTF-8",
  "pluralRules": "Norwegian (Bokmål)"
}
```

---

## 🔌 Integrasjonspunkter

### For Pionex-implementasjon:

#### 1. Language Registry
```javascript
{
  locale: "nb",
  displayName: "Norsk Bokmål",
  configUrl: "https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/config.json"
}
```

#### 2. Translation Loader
```javascript
async function loadNorwegian() {
  const [config, language, translations] = await Promise.all([
    fetch('config.json').then(r => r.json()),
    fetch('language.json').then(r => r.json()),
    fetch('translations.json').then(r => r.json())
  ]);
  
  return { config, language, translations };
}
```

#### 3. Fallback Support
```javascript
const fallbackChain = ['nb', 'nn', 'da', 'sv', 'en'];
// Norwegian Bokmål → Norwegian Nynorsk → Danish → Swedish → English
```

---

## 🔍 Quality Checks

Alle disse checkene er passert:

- ✅ JSON validity
- ✅ UTF-8 encoding
- ✅ No broken links
- ✅ Language code format (ISO 639-1)
- ✅ Translation key consistency
- ✅ Metadata completeness
- ✅ Schema compliance
- ✅ Version format (semver)

---

## 📦 Usage Example

```javascript
// HTML
<html lang="nb">
  <head>
    <meta charset="UTF-8">
    <script src="https://raw.githubusercontent.com/magholmen91/pionex-l10n/master/devcontainers-cli-nb/config.json"></script>
  </head>
  <body>
    <h1 data-i18n="title">Dev Container CLI</h1>
    <p data-i18n="context">Kontekst</p>
  </body>
</html>

// JavaScript
const i18n = new I18n('nb', translations);
const title = i18n.t('title'); // "Dev Container CLI"
const context = i18n.t('context'); // "Kontekst"
```

---

## 📞 Support

- 📧 GitHub Issues: https://github.com/magholmen91/pionex-l10n/issues
- 🔗 Repository: https://github.com/magholmen91/pionex-l10n
- 👤 Translator: https://github.com/magholmen91

---

## 📄 Lisens

MIT License - Se LICENSE.txt for detaljer

**Sist oppdatert:** 2026-06-26
