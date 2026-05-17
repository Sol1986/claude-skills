---
name: lovable-seo-geo-optimizer-skill
description: Audit and fully implement SEO and GEO (AI discoverability) across a Lovable project. Use this skill whenever someone wants to optimize their Lovable site for search engines or AI tools, improve Google rankings, add structured data/schema, create a sitemap or robots.txt, add Open Graph tags, build an llm.html page for AI discovery, or says things like "optimize my Lovable site", "add SEO to my project", "make my site discoverable by AI", "set up schema markup", "improve my Lighthouse score", or "help ChatGPT/Perplexity find my site". Always use this skill -- do not attempt to implement SEO from memory. Even if the request seems narrow ("just add meta tags"), use this skill to ensure nothing is missed.
---

# Lovable SEO + GEO Optimizer

A complete implementation guide for making a Lovable-hosted site fully optimized for Google ranking, social sharing, and AI discovery (ChatGPT, Perplexity, Claude).

Treat SEO like code: implement, verify, and enforce best practices across every page.

---

## STEP 0 -- Gather Context Before Starting

Before writing any code, ask the user for:

1. **Site URL** (or Lovable project name)
2. **Primary pages** -- list all public routes (e.g., /, /about, /services, /blog)
3. **Brand name** and primary keyword(s) per page
4. **Domain** (e.g., mysite.com) -- needed for canonical tags, sitemap, OG URLs
5. **Logo URL** -- needed for Organization schema
6. **Social profile URLs** -- Twitter/X, LinkedIn, etc.
7. **GSC verification code** (if they have one -- optional)

Do not proceed to implementation without at least items 1-4.

---

## STEP 1 -- Foundation (Crawlability)

### 1a. /public/sitemap.xml

Generate a static sitemap. If the project uses dynamic routes, instruct the user to automate updates via a build script or Lovable plugin.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://DOMAIN/</loc>
    <lastmod>YYYY-MM-DD</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://DOMAIN/about</loc>
    <lastmod>YYYY-MM-DD</lastmod>
    <priority>0.8</priority>
  </url>
  <!-- main pages = 0.8, blog/tutorial/agent pages = 0.6 -->
</urlset>
```

Priority rules:
- Homepage: 1.0
- Main pages (/about, /services, /contact): 0.8
- Blog/tutorial/agent pages: 0.6

### 1b. /public/robots.txt

```txt
User-agent: *
Allow: /

User-agent: GPTBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Claude-Web
Allow: /

User-agent: Google-Extended
Disallow: /

User-agent: CCBot
Disallow: /

# Never block assets
Allow: /assets/
Allow: *.js
Allow: *.css

Sitemap: https://DOMAIN/sitemap.xml
```

### 1c. Canonical Tags

Add to every page's `<head>`. In Lovable/React, inject via `react-helmet` or the project's existing head manager:

```html
<link rel="canonical" href="https://DOMAIN/page-slug" />
```

Rules:
- Self-referencing only
- No trailing slashes (pick one convention and enforce it sitewide)
- Always use the primary domain (no www vs non-www mismatch)

### 1d. URL Hygiene Checklist

Audit existing routes. Flag any that use:
- Query params (?id=123) -- rewrite to /slug
- Underscores -- replace with hyphens
- Uppercase letters -- enforce lowercase
- Generic names (/page1) -- rename to keyword-based paths

---

## STEP 2 -- On-Page SEO

Apply per-page. Build a metadata table before writing code:

| Page | Title (<60 chars) | Meta Description (140-160 chars) | H1 | Primary Keyword |
|------|-------------------|-----------------------------------|----|-----------------|
| /    | ...               | ...                               | ...| ...             |

### 2a. Page Titles

Format: `Primary Keyword | Brand Name`

Examples:
- `AI Agents for Tile Showrooms | GroutJoynt`
- `MindStudio Tutorials | Sol Builds`

Max 60 characters. Each page must be unique.

### 2b. Meta Descriptions

Format: Benefit-driven statement + soft CTA.

Example:
`Learn how to build AI-powered workflows in MindStudio with no code. Step-by-step tutorials for builders at every level. Start free today.`

Max 160 characters. Unique per page.

### 2c. Heading Structure

Each page must have exactly ONE `<h1>` containing the primary keyword. Hierarchy must be logical:

```
H1 (primary keyword)
  H2 (subtopic)
    H3 (detail)
  H2 (subtopic)
```

Never skip heading levels. Never use headings for styling.

### 2d. Semantic HTML

Replace generic div-only layouts with:

```html
<header>...</header>
<nav>...</nav>
<main>
  <section>...</section>
  <article>...</article>
</main>
<footer>...</footer>
```

### 2e. Internal Linking

Each page should have 3-5 contextual internal links using keyword-rich anchor text.

Bad: `Click here` | Good: `Learn about AI agent workflows`

Every page must be reachable within 3 clicks from the homepage.

### 2f. Image Optimization

For every `<img>`:
- Add `alt="descriptive text with keyword"`
- Add explicit `width` and `height` attributes
- Use WebP format where possible
- Compress to under 200KB
- Add `loading="lazy"` on below-fold images
- Add `fetchpriority="high"` on hero/LCP image

---

## STEP 3 -- Structured Data (Schema / JSON-LD)

Inject all schema as `<script type="application/ld+json">` blocks in `<head>`.

### 3a. Organization Schema (Homepage only)

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "BRAND_NAME",
  "url": "https://DOMAIN",
  "logo": "https://DOMAIN/logo.png",
  "description": "ONE_SENTENCE_DESCRIPTION",
  "sameAs": [
    "https://twitter.com/HANDLE",
    "https://linkedin.com/company/HANDLE"
  ]
}
```

### 3b. Product/Service Schema (Services or AI Agents page)

```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "SERVICE_NAME",
  "description": "DESCRIPTION",
  "provider": {
    "@type": "Organization",
    "name": "BRAND_NAME"
  }
}
```

### 3c. Article Schema (Blog and Tutorial pages)

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "ARTICLE_TITLE",
  "author": {
    "@type": "Person",
    "name": "AUTHOR_NAME"
  },
  "datePublished": "YYYY-MM-DD",
  "dateModified": "YYYY-MM-DD",
  "publisher": {
    "@type": "Organization",
    "name": "BRAND_NAME",
    "logo": "https://DOMAIN/logo.png"
  }
}
```

### 3d. FAQPage Schema (Key pages + llm.html)

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "QUESTION",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "ANSWER"
      }
    }
  ]
}
```

Schema must exactly match visible on-page content. Do not add schema for content that is not displayed.

---

## STEP 4 -- Social Preview Tags

Add to every important page's `<head>`. Each page needs a UNIQUE OG image (1200x630px).

```html
<!-- Open Graph -->
<meta property="og:title" content="PAGE_TITLE" />
<meta property="og:description" content="PAGE_DESCRIPTION" />
<meta property="og:image" content="https://DOMAIN/og/page-slug.png" />
<meta property="og:url" content="https://DOMAIN/page-slug" />
<meta property="og:type" content="website" />

<!-- Twitter / X Card -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="PAGE_TITLE" />
<meta name="twitter:description" content="PAGE_DESCRIPTION" />
<meta name="twitter:image" content="https://DOMAIN/og/page-slug.png" />
```

No generic fallback images. Each page must have its own.

---

## STEP 5 -- Performance

Target Lighthouse scores: Performance 90+, SEO 100.

### Implementation checklist:

- [ ] `loading="lazy"` on all below-fold images
- [ ] `fetchpriority="high"` on LCP/hero image
- [ ] `<link rel="preload">` for hero image and critical fonts
- [ ] Defer non-critical JS with `defer` or `async`
- [ ] Minify CSS and JS (Lovable handles this at build time -- confirm it is enabled)
- [ ] Avoid layout shift: set explicit width/height on all images and embeds
- [ ] Remove unused CSS (check for dead Tailwind classes)

---

## STEP 6 -- Mobile Optimization

- No horizontal overflow (`overflow-x: hidden` on body as a failsafe, but fix root cause)
- Minimum body font size: 16px
- All tap targets: minimum 48x48px
- Test at 375px, 390px, and 414px viewport widths
- Use `clamp()` for responsive typography where appropriate

---

## STEP 7 -- GEO (AI Discoverability)

### 7a. Create /public/llm.html

This page is plain HTML with minimal JS. It is written for AI crawlers (ChatGPT, Perplexity, Claude-Web) to read and quote from.

Structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>About BRAND_NAME | AI Reference Page</title>
  <meta name="description" content="Factual overview of BRAND_NAME for AI and search engines." />
  <link rel="canonical" href="https://DOMAIN/llm.html" />
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [ /* FAQ items here */ ]
  }
  </script>
</head>
<body>
  <h1>BRAND_NAME: What We Do</h1>

  <h2>Overview</h2>
  <p>BRAND_NAME is a [TYPE OF COMPANY] that helps [TARGET AUDIENCE] [ACHIEVE OUTCOME] by [METHOD].</p>

  <h2>Who It's For</h2>
  <p>...</p>

  <h2>Products and Services</h2>
  <h3>SERVICE_NAME</h3>
  <p>...</p>

  <h2>What Makes Us Different</h2>
  <p>...</p>

  <h2>Pricing Approach</h2>
  <p>...</p>

  <h2>Frequently Asked Questions</h2>
  <h3>QUESTION?</h3>
  <p>ANSWER.</p>

  <h2>Contact</h2>
  <p>...</p>
</body>
</html>
```

Writing style for llm.html:
- Factual, direct, quotable
- No marketing fluff or vague superlatives
- Short answers first, then explanation
- Use definitions: "TERM: A brief explanation of what this means."

Example: "GroutJoynt is a platform that helps tile showrooms increase in-store conversion by improving customer confidence during design consultations."

---

## STEP 8 -- Google Search Console Prep

Add a placeholder in the site's `<head>` for the GSC verification tag:

```html
<!-- Google Search Console verification -->
<!-- <meta name="google-site-verification" content="PASTE_CODE_HERE" /> -->
```

Confirm sitemap URL is correct and ready to submit: `https://DOMAIN/sitemap.xml`

---

## STEP 9 -- Final Audit

After implementation, produce an audit report covering:

**Completeness check (per page):**
- [ ] Unique title tag
- [ ] Unique meta description
- [ ] Canonical tag
- [ ] One H1 with primary keyword
- [ ] OG + Twitter card tags with unique image
- [ ] Relevant schema block

**Site-wide:**
- [ ] /sitemap.xml accessible and valid
- [ ] /robots.txt accessible, allows GEO bots, blocks Google-Extended
- [ ] /llm.html accessible, plain HTML, indexed
- [ ] All images have ALT text
- [ ] No broken internal links
- [ ] GSC verification placeholder present

**Report format:**

```
AUDIT SUMMARY
=============
Pages audited: X
Pages fully compliant: X
Issues found:
  - [page] missing [element]
  - [page] title too long (XX chars)
Improvements made:
  - Added sitemap.xml with X URLs
  - Created llm.html with Organization + FAQPage schema
  - Added OG tags to X pages
  - ...
```

---

## Reference: Lovable-Specific Notes

- Inject `<head>` tags via `react-helmet-async` or the `index.html` template depending on project setup
- For SPA routing: ensure the server (or Netlify/Vercel config) returns the correct HTML for each route -- crawlers do not execute JS by default
- Static files (sitemap.xml, robots.txt, llm.html) go in `/public/` so Lovable serves them at the root
- For dynamic blog/article pages: generate static routes at build time if possible; avoid client-only rendering for SEO-critical content
- Test crawlability using Google Search Console's URL Inspection tool or `curl -A "Googlebot" https://DOMAIN/page`
