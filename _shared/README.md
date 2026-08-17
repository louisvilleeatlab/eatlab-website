# Shared Page Elements

Reference copies of the markup every page shares. **This directory is not served
or built — it is a clipboard.** Plain HTML, no build step, no includes: these
blocks are copy-pasted into each page and kept in sync by find-and-replace.

Change a shared element **here first**, then find-and-replace across every `.html`
file at the repo root so the copies never drift.

To start a new page, copy `page-template.html` rather than pasting these blocks
one at a time.

---

## Current pages

| File | URL | Nav label | `<title>` |
|---|---|---|---|
| `index.html` | `/` | About | EAT Lab — University of Louisville |
| `research-publications.html` | `/research-publications` | Research | Research | EAT Lab |
| `participate-in-research.html` | `/participate-in-research` | Participate | Participate in Research | EAT Lab |
| `our-team.html` | `/our-team` | Our Team | Our Team | EAT Lab |
| `eating-disorder-resources.html` | `/eating-disorder-resources` | Resources | Eating Disorder Resources | EAT Lab |
| `blog.html` | `/blog` | News | Blog & In the Press | EAT Lab |

## How current-page nav indication works

**Nothing is hardcoded per page.** The nav block below is byte-identical in all
six files — verified by hashing it in each. A small script at the bottom of every
page reads `window.location.pathname` and adds `active` (plus `aria-current="page"`)
to the matching link.

This was chosen over hardcoding `class="active"` per file for one reason: it keeps
the shared blocks *identical everywhere*, which is the whole premise of a
copy-paste architecture. The moment the nav differs per page, find-and-replace
stops being safe and every future nav change becomes a six-way manual edit with a
real chance of one page drifting.

**So: when you add a page, you do not touch the nav for the active state.** Add the
link to the nav block, find-and-replace it across all pages, and the script handles
the rest. The only requirement is that the link's `href` exactly matches the
page's URL path.

Trade-offs, stated honestly: the active state is applied by JavaScript, so it does
not appear with JS disabled, and there is a theoretical flash before it applies
(unmeasurable in practice — the script is inline and runs immediately). If you
ever need the active state without JS, switch to hardcoding and update this file.

The script normalises `.html` and `/index` so the highlight is correct whether the
page is reached as `/our-team`, `/our-team.html`, `/`, or `/index.html`.

---

## 1. Top bar (UofL strip)

Paste directly after `<body>`. Identical on every page — it contains no
page-specific state and needed no change for multi-page use.

```html
<!-- UofL bar -->
<div class="top-bar">
  <div class="container">
    <div class="top-bar-left">
      <span class="top-bar-uofl">
        <span class="top-bar-cardinal"></span>
        UNIVERSITY OF LOUISVILLE
      </span>
    </div>
    <div class="top-bar-right">
      <a>Department of Psychological &amp; Brain Sciences</a>
      <a>College of Arts &amp; Sciences</a>
    </div>
  </div>
</div>
```

## 2. Header + nav

Paste after the top bar. Every link is a real `href`; there is no `showPage()`
anywhere in the repo any more.

```html
<!-- Main header -->
<header>
  <div class="container header-inner">
    <a class="brand" href="/">
      <img class="brand-logo" src="/images/logo.png" alt="EAT Lab — Eating Anxiety Treatment Laboratory and Clinic">
      <span class="brand-uofl">College of Arts &amp; Sciences<br>University of Louisville</span>
    </a>
    <nav class="primary">
      <a data-page="home" href="/">About</a>
      <a data-page="research" href="/research-publications">Research</a>
      <a data-page="participate" href="/participate-in-research">Participate</a>
      <a data-page="team" href="/our-team">Our Team</a>
      <a data-page="resources" href="/eating-disorder-resources">Resources</a>
      <a data-page="news" href="/blog">News</a>
      <a class="cta-button" href="/participate-in-research">Join a Study</a>
    </nav>
    <button class="mobile-toggle" onclick="toggleMobile()">
      <span></span><span></span><span></span>
    </button>
  </div>
  <div class="mobile-nav" id="mobileNav">
    <a href="/" onclick="toggleMobile()">About</a>
    <a href="/research-publications" onclick="toggleMobile()">Research</a>
    <a href="/participate-in-research" onclick="toggleMobile()">Participate</a>
    <a href="/our-team" onclick="toggleMobile()">Our Team</a>
    <a href="/eating-disorder-resources" onclick="toggleMobile()">Resources</a>
    <a href="/blog" onclick="toggleMobile()">News</a>
  </div>
</header>
```

## 3. Footer

Paste immediately before the script block.

```html
<footer>
  <div class="container">
    <div class="footer-grid">
      <div class="footer-brand">
        <h4>EAT Lab</h4>
        <p>The Eating Anxiety Treatment Laboratory at the University of Louisville. Developing personalized, evidence-based treatments for eating disorders and anxiety.</p>
      </div>
      <div class="footer-col">
        <h5>Explore</h5>
        <ul>
          <li><a href="/">About</a></li>
          <li><a href="/research-publications">Research</a></li>
          <li><a href="/our-team">Our Team</a></li>
          <li><a href="/blog">News</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Get involved</h5>
        <ul>
          <li><a href="/participate-in-research">Participate</a></li>
          <li><a href="/eating-disorder-resources">Resources</a></li>
          <li><a>Donate</a></li>
          <li><a>Prospective students</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Visit</h5>
        <ul>
          <li>Life Sciences Building</li>
          <li>2301 S 3rd St</li>
          <li>Louisville, KY 40292</li>
          <li><a>Directions</a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <div>© 2026 EAT Lab · University of Louisville</div>
      <div class="footer-social">
        <a>Facebook</a>
        <a>Instagram</a>
        <a>Twitter / X</a>
        <a>Email</a>
      </div>
    </div>
  </div>
</footer>
```

## 4. Scripts

Paste immediately before `</body>`. Contains only the mobile-nav toggle and the
active-nav script — both identical on every page.

```html
<script>
// Mobile nav toggle (unrelated to page switching — kept from the SPA).
function toggleMobile() {
  document.getElementById('mobileNav').classList.toggle('open');
}

// Current-page nav indication. Identical on every page — safe to find-and-replace.
(function () {
  var path = window.location.pathname.replace(/\.html$/, '').replace(/\/index$/, '/');
  if (path.length > 1) path = path.replace(/\/$/, '');
  if (!path) path = '/';
  var links = document.querySelectorAll('nav.primary a:not(.cta-button), .mobile-nav a');
  Array.prototype.forEach.call(links, function (a) {
    var href = (a.getAttribute('href') || '').replace(/\.html$/, '');
    if (href === path) {
      a.classList.add('active');
      a.setAttribute('aria-current', 'page');
    }
  });
})();
</script>
```

## 5. Head boilerplate

Everything except `<title>`, `description`, `canonical`, and the `og:` tags is
identical across pages. See `page-template.html` for the fill-in-the-blanks version.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Public+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="icon" type="image/x-icon" sizes="32x32" href="/images/favicon.ico">
<link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon.png">
<meta name="theme-color" content="#2354A2">
<link rel="stylesheet" href="/styles.css">
```

Per-page rules:

- **`<title>`** — `[Page Name] | EAT Lab`. The homepage is the exception:
  `EAT Lab — University of Louisville`.
- **`<meta name="description">`** — 140–160 characters, written from the page's
  own content, not boilerplate.
- **`<link rel="canonical">`** — `https://www.louisvilleeatlab.com` + the page's
  path. This points at the **eventual production domain**, not the `.vercel.app`
  preview, so search engines consolidate on the real address once DNS moves.
- **`og:image`** — `/images/lab-photo.jpg` as the site default, given as an
  absolute URL (Open Graph requires absolute). Override per page when a better
  image exists.

---

## Paths and local preview

All shared markup uses **root-relative** paths (`/styles.css`, `/images/logo.png`,
`/our-team`). That is what Vercel serves and it keeps the snippets identical at any
directory depth.

Two consequences for local testing:

1. Opening a file with `file://` will not load CSS or images.
2. **Plain `python3 -m http.server` will 404 on `/our-team`** — it does not
   implement Vercel's `cleanUrls`, so it only serves `/our-team.html`.

Use this instead — it emulates `cleanUrls` by falling back to `<path>.html`:

```bash
cd ~/Projects/eatlab-website && python3 -c "
import http.server, os
class H(http.server.SimpleHTTPRequestHandler):
    def translate_path(self, p):
        f = super().translate_path(p)
        if not os.path.exists(f) and os.path.exists(f + '.html'): return f + '.html'
        return f
http.server.test(HandlerClass=H, port=8765)
"
```

Then open <http://localhost:8765/>.
