# Insert Code HTML to HEAD/FOOTER — PrestaShop Module

![Version](https://img.shields.io/badge/version-1.1.3-blue.svg)
![PrestaShop](https://img.shields.io/badge/PrestaShop-1.7.x%20|%208.x%20|%209.x-orange.svg)
![License](https://img.shields.io/badge/license-AFL--3.0-green.svg)
![PHP](https://img.shields.io/badge/PHP-7.2+-purple.svg)

> **Moduł do wstrzykiwania własnego kodu HTML/JS/CSS do sekcji `<head>` oraz przed znacznikiem `</body>` na każdej stronie frontendu sklepu PrestaShop.**

---

## 📋 Spis treści

- [Opis](#-opis)
- [Funkcjonalności](#-funkcjonalności)
- [Wymagania](#-wymagania)
- [Instalacja](#-instalacja)
- [Konfiguracja](#-konfiguracja)
- [Przykłady użycia](#-przykłady-użycia)
- [Dokumentacja techniczna](#-dokumentacja-techniczna)
- [Rozwiązywanie problemów](#-rozwiązywanie-problemów)
- [Changelog](#-changelog)
- [Wkład w rozwój](#-wkład-w-rozwój)
- [Licencja](#-licencja)
- [Kontakt](#-kontakt)

---

## 📝 Opis

**Insert Code HTML to HEAD/FOOTER** to lekki, bezpieczny moduł dla PrestaShop, który umożliwia administratorom sklepu wstrzykiwanie dowolnego kodu HTML, JavaScript lub CSS bezpośrednio do:

- Sekcji `<head>` każdej strony frontendu (np. meta tagi, style, skrypty analityczne)
- Obszaru tuż przed zamknięciem znacznika `</body>` (np. chatboty, pixel śledzący, skrypty lazy-load)

Moduł został zaprojektowany z myślą o:

- ✅ **Bezpieczeństwie** — walidacja tokenów CSRF, escapowanie SQL, base64 transport
- ✅ **Kompatybilności** — działa z PrestaShop 1.7.x, 8.x i 9.x
- ✅ **Wydajności** — zerowy narzut, brak dodatkowych tabel w bazie danych
- ✅ **Diagnostyce** — panel weryfikacji zapisu z informacjami o błędach SQL

Moduł jest aktywnie rozwijany i testowany w środowisku produkcyjnym sklepu [KEDAR-WIHA.pl](https://kedar-wiha.pl) — autoryzowanego dystrybutora narzędzi WIHA w Polsce.

---

## ✨ Funkcjonalności

### 🔹 Wstrzykiwanie kodu

- Dodawanie kodu do `<head>` — globalnie na wszystkich podstronach
- Dodawanie kodu przed `</body>` — idealne dla skryptów ładowanych asynchronicznie
- Wsparcie dla dowolnego HTML/JS/CSS — bez ograniczeń formatu

### 🔹 Bezpieczeństwo i stabilność

- **Base64 transport** — obejście restrykcji WAF/ModSecurity przy zapisie złożonych fragmentów kodu
- **Walidacja CSRF** — trzy-poziomowa weryfikacja tokena administracyjnego
- **Escapowanie SQL** — użycie canonicalnych helperów `Db::insert()` / `Db::update()` z single-quoted delimiterami
- **Verify-after-save** — automatyczna weryfikacja, czy zapisane dane są identyczne z wprowadzonymi

### 🔹 Diagnostyka

- Panel diagnostyczny wyświetlający:
  - Metodę transportu (base64 / raw)
  - Rozmiar danych wejściowych i zapisanych w bazie
  - Status operacji zapisu dla HEAD i FOOTER
  - Szczegóły błędów SQL (jeśli wystąpią)

### 🔹 Zarządzanie cache

- Automatyczne czyszczenie cache Smarty po zapisie konfiguracji
- Możliwość ręcznego czyszczenia cache z poziomu panelu konfiguracyjnego

### 🔹 Kompatybilność z motywami

- Działa z motywem **Optima 3.3.0** (roadthemes)
- Kompatybilny z hookami `displayHeader` i `displayBeforeBodyClosingTag`
- Nie koliduje z modułami Creative Elements, POS Themes ani innymi popularnymi rozszerzeniami

---

## ⚙️ Wymagania

| Komponent        | Wymagana wersja                            |
| ---------------- | ------------------------------------------ |
| **PrestaShop**   | 1.7.0.0 – 9.99.99                          |
| **PHP**          | 7.2 lub nowszy (rekomendowane 8.0+)        |
| **MySQL**        | 5.6+ lub MariaDB 10.1+                     |
| **Smarty**       | 3.1+ (domyślnie w PrestaShop)              |
| **Przeglądarka** | Nowoczesna (Chrome, Firefox, Edge, Safari) |

> ⚠️ **Uwaga:** Moduł nie tworzy własnych tabel w bazie danych — wykorzystuje istniejącą tabelę `ps_configuration`.

---

## 📦 Instalacja

### Metoda 1: Przez Back Office (zalecana)

1. Pobierz najnowszą wersję modułu z [GitHub Releases](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/insertcode-html-headfooter-prestashop/releases)
2. W panelu administratora przejdź do: **Moduły → Menedżer modułów → Prześlij moduł**
3. Wybierz pobrany plik `.zip` i kliknij **Zainstaluj**
4. Po instalacji przejdź do: **Moduły → Menedżer modułów → Front Office Features → Insert Code HTML to HEAD/FOOTER → Konfiguruj**

### Metoda 2: Manualna (FTP/SFTP)

1. Rozpakuj archiwum modułu
2. Prześlij folder `insertcodeheadfooter` do katalogu `/modules/` na serwerze
3. W panelu administratora przejdź do: **Moduły → Menedżer modułów**
4. Znajdź moduł na liście i kliknij **Zainstaluj**

### Weryfikacja instalacji

Po poprawnej instalacji:

- ✅ Moduł pojawi się w sekcji **Front Office Features**
- ✅ Hooki `displayHeader` i `displayBeforeBodyClosingTag` zostaną zarejestrowane
- ✅ Klucze konfiguracyjne `ICHF_HEAD_CODE` i `ICHF_FOOTER_CODE` zostaną utworzone w tabeli `ps_configuration`

---

## ⚙️ Konfiguracja

### Panel administracyjny

Po wejściu w konfigurację modułu zobaczysz dwa edytory kodu:

#### 🔹 Kod dla sekcji `<head>`

```html
<!-- Przykład: Google Tag Manager -->
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');</script>
<!-- End Google Tag Manager -->
```

#### 🔹 Kod przed `</body>`

```html
<!-- Przykład: Facebook Pixel -->
<!-- Facebook Pixel Code -->
<script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
  n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)}(window, document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', '123456789');
  fbq('track', 'PageView');
</script>
<noscript><img height="1" width="1" style="display:none"
src="https://www.facebook.com/tr?id=123456789&ev=PageView&noscript=1"
/></noscript>
<!-- End Facebook Pixel Code -->
```

### Zapisywanie konfiguracji

1. Wklej kod do odpowiedniego pola
2. Kliknij przycisk **Zapisz ustawienia**
3. System automatycznie:
   - Zweryfikuje token CSRF
   - Zakoduje dane (base64, jeśli wykryto znaki specjalne)
   - Zapisze konfigurację w bazie danych
   - Wykona weryfikację "verify-after-save"
   - Wyczyści cache Smarty
4. Zobaczysz komunikat potwierdzający lub panel diagnostyczny z szczegółami

### 🔐 Bezpieczeństwo formularza

Moduł implementuje trzy-poziomową walidację tokena:

1. Ukryte pole `ichf_admin_token` w formularzu POST
2. Parametr `token` w URL (kompatybilność wsteczna)
3. Weryfikacja sesji zalogowanego administratora

---

## 💡 Przykłady użycia

### 🔹 Dodanie custom CSS do `<head>`

```html
<style>
  /* Custom styles dla KEDAR-WIHA.pl */
  .product-availability .in_stock {
    color: #1A7A3C !important;
    font-weight: 600;
  }
  .badge-vde {
    background: #8B0000;
    color: #fff;
    padding: 2px 8px;
    border-radius: 3px;
    font-size: 11px;
    font-weight: 700;
  }
</style>
```

### 🔹 Dodanie skryptu lazy-load obrazów

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  if ('loading' in HTMLImageElement.prototype) {
    const images = document.querySelectorAll('img[loading="lazy"]');
    images.forEach(img => {
      img.src = img.dataset.src;
    });
  } else {
    const script = document.createElement('script');
    script.src = 'https://cdnjs.cloudflare.com/ajax/libs/lozad.js/1.16.0/lozad.min.js';
    script.onload = function() {
      const observer = lozad();
      observer.observe();
    };
    document.body.appendChild(script);
  }
});
</script>
```

### 🔹 Dodanie meta tagów SEO

```html
<!-- Custom meta tags dla lepszego SEO -->
<meta name="author" content="KEDAR-WIHA.pl — Autoryzowany Dystrybutor WIHA Polska">
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

### 🔹 Dodanie chatbotu przed `</body>`

```html
<!-- Chatbot widget -- before </body> -->
<script type="text/javascript" id="chatbot-widget" async
  src="https://widget.chatbot.example.com/embed.js?account=12345">
</script>
```

---

## 🛠️ Dokumentacja techniczna

### Struktura modułu

```
insertcodeheadfooter/
├── insertcodeheadfooter.php      # Główny plik modułu (v1.1.3)
├── config.xml                    # Metadane modułu
├── logo.png                      # Ikona modułu w Back Office
├── views/
│   ├── templates/
│   │   ├── admin/
│   │   │   └── configure.tpl     # Formularz konfiguracyjny
│   │   └── hook/
│   │       ├── head_code.tpl     # Template dla hooka displayHeader
│   │       └── footer_code.tpl   # Template dla hooka displayBeforeBodyClosingTag
├── install.sql                   # Dokumentacja instalacji SQL
└── index.php                     # Zabezpieczenie dostępu do katalogu
```

### Hooki wykorzystywane przez moduł

| Hook                          | Opis                              | Renderowany w motywie Optima         |
| ----------------------------- | --------------------------------- | ------------------------------------ |
| `displayHeader`               | Wstrzykuje kod do sekcji `<head>` | `_partials/head.tpl`, linia 86       |
| `displayBeforeBodyClosingTag` | Wstrzykuje kod przed `</body>`    | `layout-both-columns.tpl`, linia 161 |

### Klucze konfiguracyjne

| Klucz               | Opis                             | Wersja  |
| ------------------- | -------------------------------- | ------- |
| `ICHF_HEAD_CODE`    | Kod wstrzykiwany do `<head>`     | v1.1.0+ |
| `ICHF_FOOTER_CODE`  | Kod wstrzykiwany przed `</body>` | v1.1.0+ |
| `INSERTCODE_HEAD`   | Legacy key (auto-migrowany)      | v1.0.0  |
| `INSERTCODE_FOOTER` | Legacy key (auto-migrowany)      | v1.0.0  |

### Mechanizm zapisu do bazy danych

Moduł używa canonicalnych helperów PrestaShop `Db::getInstance()->insert()` oraz `Db::getInstance()->update()`, które:

- Automatycznie escapują wartości przy użyciu single-quoted SQL delimiters
- Obsługują parametryzowane zapytania
- Zapewniają kompatybilność z różnymi wersjami MySQL/MariaDB

```php
// Przykład zapisu (uproszczony)
$db->update(
    'configuration',
    ['value' => $userCode, 'date_upd' => $now],
    '`id_configuration` = ' . $existingId,
    1
);
```

### Diagnoza problemów z zapisem

Jeśli zapis się nie powiedzie, panel diagnostyczny wyświetli:

- `head_save_error` / `footer_save_error` — komunikat błędu z `Db::getMsgError()`
- `head_db_bytes` vs `head_input_bytes` — porównanie rozmiaru danych
- `transport_head` — informacja, czy użyto base64 czy raw

---

## 🔍 Rozwiązywanie problemów

### ❌ Kod nie pojawia się na stronie

1. **Sprawdź cache Smarty**
   
   - Przejdź do: **Zaawansowane → Wydajność**
   - Kliknij **Wyczyść cache**
   - Lub użyj przycisku "Wyczyść cache" w konfiguracji modułu

2. **Sprawdź, czy hooki są zarejestrowane**
   
   ```sql
   SELECT * FROM ps_hook_module 
   WHERE id_module = (SELECT id_module FROM ps_module WHERE name = 'insertcodeheadfooter');
   ```

3. **Sprawdź wartość w bazie danych**
   
   ```sql
   SELECT name, value, date_upd 
   FROM ps_configuration 
   WHERE name IN ('ICHF_HEAD_CODE', 'ICHF_FOOTER_CODE');
   ```

### ❌ Błąd przy zapisie konfiguracji

1. **Sprawdź panel diagnostyczny** — wyświetla szczegóły błędu SQL
2. **Sprawdź uprawnienia do bazy danych** — użytkownik DB musi mieć prawa `INSERT`/`UPDATE` do tabeli `ps_configuration`
3. **Sprawdź logi PrestaShop** — `/var/logs/prod-*.log` lub `/var/logs/dev-*.log`

### ❌ Konflikty z innymi modułami

Moduł został zaprojektowany tak, aby **nie kolidować** z innymi rozszerzeniami:

- Używa unikalnych nazw hooków i template'ów
- Nie nadpisuje istniejących plików motywu
- Renderuje kod jako ostatni element w danym hooku (dzięki priorytetowi domyślnemu)

Jeśli występuje konflikt:

1. Sprawdź kolejność ładowania modułów w `ps_hook_module`
2. Tymczasowo wyłącz inne moduły korzystające z tych samych hooków
3. Skontaktuj się z supportem — dodamy mechanizm priorytetów w przyszłej wersji

### ❌ Problemy z WAF / ModSecurity

Jeśli Twój hosting blokuje zapis kodu zawierającego znaki `<`, `>`, `"`, `'`:

- ✅ Moduł automatycznie wykryje sytuację i użyje **base64 transport**
- ✅ Kod zostanie zakodowany przed wysłaniem i rozkodowany po stronie serwera
- ✅ Jeśli problem persistuje, skontaktuj się z administratorem hostingu w sprawie reguł ModSecurity

---

## 📜 Changelog

### v1.1.3 — 2026-04-07

- 🔧 **FIX:** Naprawiono regresję SQL z v1.1.2 — zastąpiono raw SQL z double-quoted delimiterami na canonicalne helpery `Db::insert()` / `Db::update()` z single-quoted delimiterami
- 🔧 **FIX:** Dodano przechwytywanie i wyświetlanie błędów SQL z `Db::getMsgError()` w panelu diagnostycznym
- 🔧 **FIX:** `install()` pomija inicjalizację kluczy, jeśli już istnieją — reinstall nie nadpisuje istniejącej konfiguracji
- 📝 **Docs:** Zaktualizowano dokumentację techniczną o szczegóły mechanizmu escapowania SQL

### v1.1.2 — 2026-04-06

- 🔄 **Refactor:** Zastąpiono `Configuration::updateValue()` bezpośrednimi zapytaniami SQL przez `Db::getInstance()` dla większej kontroli
- ✅ **Feature:** Dodano weryfikację "verify-after-save" — porównanie bajtów wejściowych z zapisanymi w DB
- 🐛 **Fix:** Usunięto potencjalne duplikaty powiadomień przy wielokrotnym zapisie
- 📊 **Diagnostics:** Rozszerzono panel diagnostyczny o metodę zapisu i rozmiary danych

### v1.1.1 — 2026-04-05

- 🐛 **Fix:** Naprawiono renderowanie kodu w Smarty — dodano `{literal}` w template'ach hooków
- 🧹 **Cleanup:** Usunięto nieużywane zmienne i poprawiono komentarze

### v1.1.0 — 2026-04-04

- 🚀 **Feature:** Dodano **base64 transport** dla kodu — obejście restrykcji WAF/ModSecurity
- 🔐 **Security:** Wdrożono trzy-poziomową walidację tokena CSRF
- 🔄 **Migration:** Automatyczna migracja kluczy z v1.0.0 (`INSERTCODE_*`) na v1.1.0 (`ICHF_*`)
- 📦 **Refactor:** Przeniesiono logikę zapisu/odczytu do dedykowanych metod prywatnych

### v1.0.0 — 2026-04-01

- 🎉 **Initial release** — podstawowa funkcjonalność wstrzykiwania kodu
- ✅ Rejestracja hooków `displayHeader` i `displayBeforeBodyClosingTag`
- ✅ Prosty formularz konfiguracyjny w Back Office
- ✅ Zapis konfiguracji przez `Configuration::updateValue()`

---

## 🤝 Wkład w rozwój

Chcesz przyczynić się do rozwoju modułu? Świetnie! 🎉

### Zgłaszanie błędów

1. Sprawdź, czy problem nie został już zgłoszony w [Issues](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/insertcode-html-headfooter-prestashop/issues)
2. Otwórz nowy issue z szablonem **Bug Report**
3. Dołącz:
   - Wersję PrestaShop i PHP
   - Kroki do reprodukcji błędu
   - Zrzuty ekranu / logi błędów (jeśli dostępne)

### Propozycje nowych funkcji

1. Otwórz issue z szablonem **Feature Request**
2. Opisz dokładnie, jaką funkcjonalność chciałbyś dodać i dlaczego
3. Jeśli możesz, zaproponuj rozwiązanie techniczne

### Pull Requests

1. Forknij repozytorium
2. Utwórz branch z nazwą opisującą zmianę: `feature/base64-transport`, `fix/sql-escaping`, itp.
3. Wprowadź zmiany, pamiętając o:
   - ✅ Standardach kodowania PrestaShop
   - ✅ Komentarzach w języku angielskim (kod) / polskim (dokumentacja)
   - ✅ Testach manualnych na PrestaShop 1.7, 8.x i 9.x
4. Otwórz PR z opisem zmian i odniesieniem do issue (jeśli dotyczy)

### Standardy kodu

- PHP: zgodność z [PrestaShop Coding Standards](https://devdocs.prestashop-project.org/8/modules/concepts/coding-standards/)
- Nazewnictwo: `camelCase` dla metod, `UPPER_CASE` dla stałych
- Komentarze: PHPDoc dla metod publicznych, inline comments dla złożonej logiki
- Bezpieczeństwo: zawsze escapuj dane przed zapytaniami SQL, waliduj input

---

## 📄 Licencja

Ten moduł jest udostępniany na licencji **Academic Free License 3.0 (AFL-3.0)**.

```
Copyright (c) 2024-2026 KEDAR-WIHA.pl

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

Pełny tekst licencji: [AFL-3.0](https://opensource.org/licenses/AFL-3.0)

---

## 📬 Kontakt

**Autor:** KEDAR-WIHA.pl  
**Agencja deweloperska:** [PB-MEDIA — Strony | Sklepy | Marketing](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing)

| Kanał         | Kontakt                                                                                                           |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| 🌐 Strona WWW | [kedar-wiha.pl](https://kedar-wiha.pl)                                                                            |
| ✉️ E-mail     | [info@kedar-wiha.pl](mailto:info@kedar-wiha.pl)                                                                   |
| 📞 Telefon    | +48 575 838 766                                                                                                   |
| 💬 WhatsApp   | [+48 575 838 766](https://wa.me/48575838766)                                                                      |
| 🐛 Issues     | [GitHub Issues](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/insertcode-html-headfooter-prestashop/issues) |

> ⚠️ **Ważne:** Moduł jest rozwijany na potrzeby sklepu KEDAR-WIHA.pl. Wsparcie komercyjne dostępne wyłącznie dla klientów agencji PB-MEDIA.

---

## 🔗 Przydatne linki

- 📘 [Dokumentacja PrestaShop — tworzenie modułów](https://devdocs.prestashop-project.org/)
- 🎨 [Motyw Optima 3.3.0 — dokumentacja](https://ecolife.posthemes.com/doc/)
- 🛠️ [Brandbook KEDAR-WIHA.pl](https://github.com/piotroq/kedar-wiha-ps9/blob/main/docs/KEDAR-WIHA-Brandbook.md)
- 📦 [Repozytorium modułów KEDAR-WIHA.pl](https://github.com/piotroq/MODULES/)

---

> **KEDAR-WIHA.pl** — Autoryzowany Dystrybutor Narzędzi WIHA w Polsce  
> *„Narzędzia dla specjalistów — jakość, którą czujesz w każdym użyciu."*

---

*README wygenerowany dla wersji modułu **1.1.3** — ostatnia aktualizacja: **2026-04-07***
