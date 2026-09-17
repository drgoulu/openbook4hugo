# OpenBook 4 Hugo

A Hugo Module reproducing the features of the WordPress [OpenBook Book Data](https://github.com/goulu/openbook) plugin.

OpenBook fetches book metadata, cover images, author information, external library links (WorldCat, Google Books, LibraryThing, BookFinder), and COinS citation metadata directly from [Open Library](https://openlibrary.org/) at build time.

## Installation

### As a Hugo Module (Recommended)

1. Initialize Hugo modules in your site (if not already done):
   ```bash
   hugo mod init my-site
   ```

2. Add `openbook4hugo` to your `hugo.yaml` or `config.yaml`:
   ```yaml
   module:
     imports:
       - path: github.com/goulu/openbook4hugo
   ```

3. Include OpenBook CSS in your `<head>` layout (or partial):
   ```html
   {{ partial "openbook/css.html" . }}
   ```
   *Alternatively, OpenBook CSS is also available at `/css/openbook.css`.*

---

## Usage

Use the `openbook` shortcode anywhere in your Markdown content:

### 1. By ISBN
```markdown
{{< openbook "9780880294188" >}}
{{< openbook booknumber="ISBN:9780880294188" templatenumber="1" >}}
```

### 2. By Open Library ID (OLID)
```markdown
{{< openbook "OLID:OL22263607M" >}}
{{< openbook booknumber="OLID:OL22263607M" templatenumber="5" >}}
```

### 3. By LCCN or OCLC
```markdown
{{< openbook booknumber="LCCN:93005405" >}}
{{< openbook booknumber="OCLC:28723707" >}}
```

---

## Templates

OpenBook supports 5 built-in display templates, matching the WordPress plugin:

### Template 1 (Default: Rich Card with Medium Cover & Links)
```markdown
{{< openbook booknumber="ISBN:9780880294188" templatenumber="1" >}}
```
* Displays medium book cover on the left, book title (linked to Open Library), author(s), publisher, publication year, links to WorldCat, Google Books, LibraryThing, and BookFinder, plus embedded COinS citation metadata.

### Template 2 (Compact: Small Cover & Title)
```markdown
{{< openbook booknumber="ISBN:9780880294188" templatenumber="2" >}}
```
* Displays small thumbnail with title and first author.

### Template 3 (Large Cover Centered with Links)
```markdown
{{< openbook booknumber="ISBN:9780880294188" templatenumber="3" >}}
```
* Displays large centered book cover and external library links.

### Template 4 (Title Link Only)
```markdown
{{< openbook booknumber="ISBN:9780880294188" templatenumber="4" >}}
```
* Displays the book title as a direct link to its Open Library entry.

### Template 5 (Academic Citation Format)
```markdown
{{< openbook booknumber="ISBN:9780880294188" templatenumber="5" >}}
```
* Formats as an academic citation: `Author(s) (Year). Title. Publisher.` with COinS metadata (ideal for references and bibliographies).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `booknumber` (or `0`) | String | *Required* | ISBN, OLID, LCCN, or OCLC identifier |
| `templatenumber` (or `1`) | String | `"1"` | Template number (`1` to `5`) |
| `revisionnumber` (or `v`) | String | `""` | Open Library revision version number |
| `publisherurl` | String | `""` | Optional link for the publisher name |

---

## Caching

OpenBook caches remote Open Library requests using Hugo's resource cache to avoid querying the remote API on every site rebuild:

- **Automatic Monthly Cache**: Cache keys are generated using the current year and month format (`2006-01`), making cached data valid for **one month** before automatically fetching fresh data.
- **Custom Cache Period**: You can optionally customize the format in your site configuration (`hugo.yaml`):
  ```yaml
  params:
    openbook:
      cache_format: "2006-01" # Default: monthly. Use "2006-01-02" for daily, "2006" for yearly.
  ```
- **Hugo File Cache Configuration**: Hugo persists remote resources in its `getresource` file cache. You can also configure its duration in `hugo.yaml`:
  ```yaml
  caches:
    getresource:
      dir: ":cacheDir/:project"
      maxAge: 720h # 30 days (1 month)
  ```
- **Force Refresh**: To force a fresh download of all remote data, run Hugo with `--ignoreCache`:
  ```bash
  hugo --ignoreCache
  ```

---

## Compatibility with WordPress Shortcodes

If you are migrating content from WordPress that used `[openbook booknumber="..." templatenumber="5"]`, you can either convert them to `{{< openbook booknumber="..." templatenumber="5" >}}` or use Hugo shortcode syntax.

## License

GPLv2 or later (same as OpenBook WordPress plugin).
