# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Landing page + blog for **Tao**, an AI productivity assistant app. Built on an Envato "Davies" HTML template, converted to Eleventy (11ty) with Nunjucks templating. All content is in French with tutoiement.

## Commands

- `npm run dev` — Start dev server (Eleventy --serve on port 8080)
- `npm run build` — Build to `_site/`
- `sass assets/scss/app.scss assets/css/styles.css --watch` — Compile SCSS (if editing styles)

## Architecture

**Static site generator:** Eleventy 3.x with Nunjucks (`.njk`) as the only template format.

**Config:** `.eleventy.js` — passthrough copies `assets/` and `404.html`. Only `.njk` files are processed.

**Global data:** `_data/site.json` — name, email, tagline, social links, location. Accessed as `{{ site.name }}` etc. in all templates.

### Layouts

- `_includes/layouts/base.njk` — Landing page layout. Includes preloader, mobile-menu, all JS/CSS.
- `_includes/layouts/blog-base.njk` — Blog layout. Uses blog-specific header/footer/mobile-menu. No preloader.

### Pages (root `.njk` files)

- `index.njk` → `index.html` — Landing page. Includes all section partials sequentially.
- `blog.njk` → `blog.html` — Blog listing page (permalink set explicitly).
- `article.njk` → `article.html` — Single article template (permalink set explicitly).

### Section Partials (`_includes/sections/`)

**Landing page sections** (included by `index.njk` in order):
`header.njk` → `scroll-header.njk` → `hero.njk` → `selected-work.njk` → `about.njk` → `services.njk` → `brands.njk` → `awards.njk` → `testimonials.njk` → `pricing.njk` → `faq.njk` → `cta.njk` → `footer.njk`

**Blog-specific partials:**
`blog-header.njk`, `blog-footer.njk`, `blog-mobile-menu.njk`, `blog-sidebar.njk`

**Shared partials** (included by base layouts):
`preloader.njk`, `mobile-menu.njk`

### Two Header Systems

1. **`header.njk`** — Landing page initial header (`tf-header style-2`). Simple vertical nav list, visible in hero only.
2. **`scroll-header.njk`** — Fixed header (`tf-header m-0`) that appears via JS when user scrolls past `#workScroll`. Numbered nav (01/ACCUEIL through 06/CONTACT).
3. **`blog-header.njk`** — Same numbered nav as scroll-header, sticky, used on blog pages.

### Key Patterns

- **Data-driven loops:** Services, FAQs, testimonials, menu items, brands are defined as Nunjucks `{% set %}` arrays and iterated with `{% for %}`. Edit data in the `{% set %}` block, not the HTML.
- **Infinite scroll marquee:** Uses `infiniteSlide` JS class with `data-clone` attribute for horizontal scrolling sections (hero title, brands, pricing header).
- **Bootstrap accordion:** Used in services and FAQ sections with `data-bs-toggle="collapse"`.
- **SVG text branding:** `assets/images/item/davies-fill.svg` and `davies-stroke.svg` render "Tao.app" as SVG `<text>` elements in the hero.
- **Brand logos:** `assets/images/brands/*.svg` — downloaded from Simple Icons, used in the brands marquee.

### CSS/JS (not to be edited directly)

- Compiled CSS: `assets/css/styles.css` (from SCSS at `assets/scss/app.scss`)
- Template JS: Bootstrap, Swiper, GSAP, ScrollTrigger, SplitText, Odometer, infinityslide, main.js
- Legacy blog HTML files (`blog-*.html`) still exist in root but are NOT processed or copied to output.

## Important Notes

- Only `.njk` files are processed by Eleventy. Raw `.html` files in root are ignored unless explicitly passed through.
- The hero section relies on the photo element (`davies-main.jpg`) for layout spacing — removing it collapses the hero. Currently removed with flex layout compensation.
- Blog pages use a different mobile menu system (Bootstrap offcanvas `#mobileMenu`) vs landing page (custom `.offcanvas-menu` with `.open-mb-menu`/`.close-mb-menu` JS).
- All nav links from blog pages use `index.html#sectionId` format to link back to landing page sections.
