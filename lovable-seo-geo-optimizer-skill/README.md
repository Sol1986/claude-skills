# Lovable SEO + GEO Optimizer Skill

A Claude skill that audits and fully implements SEO and GEO (AI discoverability) across a Lovable-hosted website. No shortcuts -- full implementation only.

---

## What This Skill Does

This skill walks Claude through a complete, structured SEO and GEO implementation for any Lovable project. It covers:

- **Crawlability** -- sitemap.xml, robots.txt, canonical tags, clean URLs
- **On-Page SEO** -- titles, meta descriptions, heading structure, semantic HTML, internal linking, image optimization
- **Structured Data** -- JSON-LD schema for Organization, Service, Article, and FAQPage
- **Social Previews** -- Open Graph and Twitter/X card tags with unique images per page
- **Performance** -- Lighthouse 90+ targets, lazy loading, LCP preload, CLS fixes
- **Mobile** -- tap targets, font sizes, responsive layouts
- **GEO (AI Discoverability)** -- /llm.html page written for ChatGPT, Perplexity, and Claude-Web crawlers
- **Google Search Console** -- verification placeholder and sitemap submission prep
- **Final Audit** -- per-page compliance checklist and summary report

---

## Who It's For

Lovable builders, no-code developers, and solopreneurs who want their sites to rank on Google and be discoverable by AI tools like ChatGPT, Perplexity, and Claude.

---

## How to Use

1. Install the skill into your MindStudio or Claude skill library
2. Open a conversation with Claude
3. Say something like: "Optimize my Lovable site for SEO" or "Add GEO to my project"
4. Claude will ask for your site URL, page list, brand name, and domain
5. Claude follows the 9-step implementation guide from SKILL.md

---

## Trigger Phrases

This skill activates on phrases like:

- "Optimize my Lovable site for SEO"
- "Add SEO to my project"
- "Make my site discoverable by AI"
- "Set up schema markup"
- "Create a sitemap and robots.txt"
- "Add Open Graph tags"
- "Build an llm.html page"
- "Improve my Lighthouse score"
- "Help ChatGPT / Perplexity find my site"

---

## File Structure

```
lovable-seo-geo-skill/
├── SKILL.md        -- Full implementation guide (Claude reads this)
├── README.md       -- This file
├── skill.json      -- Skill metadata
└── EXAMPLES.md     -- Sample inputs and outputs
```

---

## Requirements

- A Lovable project with at least one public route
- Access to the project's source files or Lovable editor
- Domain name (for canonical tags, sitemap, OG URLs)
- Optional: Google Search Console account for verification

---

## Version

1.0.0 -- Initial release
