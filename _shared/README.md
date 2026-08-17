# Shared Page Elements

Reference copies of the markup that every page shares. **This directory is not
served or built — it is a clipboard.** We are doing plain HTML with no build step
and no includes, so these blocks get copy-pasted into each new page and kept in
sync by find-and-replace when they change.

When you change a shared element, change it **here first**, then find-and-replace
across every `.html` file at the repo root so the copies never drift.

---

## ⚠️ Read this before building a second page

`index.html` is currently a **single-page application**, not a normal page. All six
sections live in one file as `<main class="page" id="page-...">` blocks, and the
nav switches between them with JavaScript:

```html
<a data-page="research" onclick="showPage('research')">Research</a>
```

There are **37 `showPage()` calls** in `index.html` and only 5 real content links.
Copy this nav onto a second page as-is and every item will be dead: `showPage()`
looks for `#page-research` in the current document, finds nothing, and the click
does nothing at all — no navigation, no error the visitor can see.

So the nav below exists in two versions. Use the right one.

---

## 1. Top bar (UofL strip)

Unchanged across pages. Paste directly after `<body>`.

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

## 2. Header + nav — CURRENT (single-page, JS-driven)

This is what `index.html` uses today. **Only correct for the SPA.**

```html
<!-- Main header -->
<header>
  <div class="container header-inner">
    <div class="brand" onclick="showPage('home')">
      <img class="brand-logo" src="/images/logo.png" alt="EAT Lab — Eating Anxiety Treatment Laboratory and Clinic">
      <span class="brand-uofl">College of Arts &amp; Sciences<br>University of Louisville</span>
    </div>
    <nav class="primary">
      <a class="active" data-page="home" onclick="showPage('home')">About</a>
      <a data-page="research" onclick="showPage('research')">Research</a>
      <a data-page="participate" onclick="showPage('participate')">Participate</a>
      <a data-page="team" onclick="showPage('team')">Our Team</a>
      <a data-page="resources" onclick="showPage('resources')">Resources</a>
      <a data-page="news" onclick="showPage('news')">News</a>
      <button class="cta-button" onclick="showPage('participate')">Join a Study</button>
    </nav>
    <button class="mobile-toggle" onclick="toggleMobile()">
      <span></span><span></span><span></span>
    </button>
  </div>
  <div class="mobile-nav" id="mobileNav">
    <a onclick="showPage('home'); toggleMobile()">About</a>
    <a onclick="showPage('research'); toggleMobile()">Research</a>
    <a onclick="showPage('participate'); toggleMobile()">Participate</a>
    <a onclick="showPage('team'); toggleMobile()">Our Team</a>
    <a onclick="showPage('resources'); toggleMobile()">Resources</a>
    <a onclick="showPage('news'); toggleMobile()">News</a>
  </div>
</header>
```

## 3. Nav — MULTI-PAGE VERSION (for real pages)

Same markup with `onclick="showPage(...)"` swapped for real `href`s. **Not yet
applied to `index.html`** — nothing was changed in Phase 1. Use this when the
first real page is built, and switch `index.html` to it at the same time.

URLs match the approved slugs in the archive repo's `FILENAME-DECISIONS.md`, so
they line up with `vercel.json`:

```html
<nav class="primary">
      <a data-page="home" href="/">About</a>
      <a data-page="research" href="/research-publications">Research</a>
      <a data-page="participate" href="/participate-in-research">Participate</a>
      <a data-page="team" href="/our-team">Our Team</a>
      <a data-page="resources" href="/eating-disorder-resources">Resources</a>
      <a data-page="news" href="/blog">News</a>
      <button class="cta-button" href="/participate-in-research">Join a Study</button>
    </nav>
```

Set `class="active"` on whichever link matches the current page.

Page → URL mapping used above:

| SPA page id | URL |
|---|---|
| `home` | `/` |
| `research` | `/research-publications` |
| `participate` | `/participate-in-research` |
| `team` | `/our-team` |
| `resources` | `/eating-disorder-resources` |
| `news` | `/blog` |

## 4. Footer

Unchanged across pages. Paste immediately before `<script>`.

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
          <li><a onclick="showPage('home')">About</a></li>
          <li><a onclick="showPage('research')">Research</a></li>
          <li><a onclick="showPage('team')">Our Team</a></li>
          <li><a onclick="showPage('news')">News</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Get involved</h5>
        <ul>
          <li><a onclick="showPage('participate')">Participate</a></li>
          <li><a onclick="showPage('resources')">Resources</a></li>
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

---

## Head boilerplate

Every page needs these in `<head>`:

```html
<link rel="stylesheet" href="/styles.css">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Public+Sans:wght@400;500;600;700;800&display=swap">
```

The favicon and apple-touch-icon are still inline base64 `<link>` tags in
`index.html`. They were left alone in Phase 1 — see the note in the commit report.

## Paths

All shared markup uses **root-relative** paths (`/styles.css`, `/images/logo.png`).
That is what Vercel serves, and it keeps the snippets identical on every page
regardless of directory depth. It also means **opening a file directly with
`file://` will not load the CSS or images** — run a local server instead:

```bash
cd ~/Projects/eatlab-website && python3 -m http.server 8000
# then open http://localhost:8000/
```
