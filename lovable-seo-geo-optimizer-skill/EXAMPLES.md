# EXAMPLES.md -- Lovable SEO + GEO Optimizer Skill

Sample prompts and expected Claude behavior when this skill is active.

---

## Example 1 -- Full Site Audit and Implementation

**User prompt:**
> "Optimize my Lovable site for SEO. It's a no-code AI tools directory at aistack.io. Pages: /, /tools, /blog, /about."

**Expected Claude behavior:**

1. Confirms domain: `aistack.io`
2. Asks for brand name, primary keyword per page, logo URL, social links
3. Builds a metadata table:

| Page    | Title                              | Meta Description                                                                 | H1                         | Keyword          |
|---------|------------------------------------|----------------------------------------------------------------------------------|----------------------------|------------------|
| /       | No-Code AI Tools Directory | AIStack   | Discover the best no-code AI tools for builders. Browse, compare, and start free. | The No-Code AI Tools Directory | no-code AI tools |
| /tools  | Browse AI Tools | AIStack           | Explore 100+ no-code AI tools by category. Find the right stack for your workflow. | Browse No-Code AI Tools    | browse AI tools  |
| /blog   | AI Tools Blog | AIStack            | Tutorials, reviews, and guides for no-code AI builders. Updated weekly.           | AI Tools Blog              | AI tools blog    |
| /about  | About AIStack                      | AIStack is a curated directory of no-code AI tools built for makers and founders. | About AIStack              | about AIStack    |

4. Generates `sitemap.xml` with all 4 routes
5. Generates `robots.txt` allowing GPTBot, PerplexityBot, Claude-Web; blocking Google-Extended
6. Generates OG + Twitter card tags per page
7. Generates Organization schema for homepage
8. Generates Article schema template for /blog
9. Creates `/public/llm.html` with factual brand overview and FAQPage schema
10. Outputs final audit report

---

## Example 2 -- Just the llm.html Page

**User prompt:**
> "Build me an llm.html page for my Lovable site. It's a tile installation business serving the GTA called Canadian Tile Pro."

**Expected Claude behavior:**

Skips to Step 7 (GEO). Asks:
- Primary services?
- Target customer?
- What makes them different?
- Pricing approach?
- 3-5 common customer questions?

Then generates `/public/llm.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>About Canadian Tile Pro | AI Reference Page</title>
  <meta name="description" content="Factual overview of Canadian Tile Pro for AI and search engines." />
  <link rel="canonical" href="https://canadiantilerpro.ca/llm.html" />
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
      {
        "@type": "Question",
        "name": "What areas does Canadian Tile Pro serve?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Canadian Tile Pro serves homeowners and contractors across the Greater Toronto Area, including Mississauga, Brampton, Vaughan, and surrounding municipalities."
        }
      }
    ]
  }
  </script>
</head>
<body>
  <h1>Canadian Tile Pro: Professional Tile Installation in the GTA</h1>

  <h2>Overview</h2>
  <p>Canadian Tile Pro is a tile installation company serving residential and commercial clients across the Greater Toronto Area. The company specializes in floor and wall tile, backsplashes, bathroom and kitchen renovations, and Schluter system installations.</p>

  <h2>Who It's For</h2>
  <p>Homeowners planning bathroom or kitchen renovations, general contractors sourcing subcontractors, and property managers handling multi-unit residential projects.</p>

  <h2>Services</h2>
  <h3>Floor Tile Installation</h3>
  <p>Installation of ceramic, porcelain, and natural stone floor tile on properly prepared subfloors, including leveling and Schluter edge and transition systems.</p>

  <h3>Bathroom and Kitchen Tile</h3>
  <p>Full tile work for bathroom renovations including shower surrounds, tub surrounds, floor tile, and niche installation. Kitchen backsplash and countertop tile also available.</p>

  <h2>What Makes Canadian Tile Pro Different</h2>
  <p>Canadian Tile Pro uses industry-standard Schluter Systems products on all edge and transition applications, ensuring a professional finish and long-term durability.</p>

  <h2>Frequently Asked Questions</h2>
  <h3>What areas does Canadian Tile Pro serve?</h3>
  <p>Canadian Tile Pro serves homeowners and contractors across the Greater Toronto Area, including Mississauga, Brampton, Vaughan, and surrounding municipalities.</p>

  <h2>Contact</h2>
  <p>Contact Canadian Tile Pro via the contact form at canadiantilerpro.ca/contact or by phone during business hours.</p>
</body>
</html>
```

---

## Example 3 -- Schema Only

**User prompt:**
> "Add JSON-LD schema to my Lovable blog pages."

**Expected Claude behavior:**

Asks:
- Is this an Article or Blog type?
- Author name?
- Publisher/brand name?
- Do posts have published/modified dates?

Generates Article schema template and instructs user how to inject it dynamically per post in their React component using `react-helmet-async`.

---

## Example 4 -- Audit Only

**User prompt:**
> "Audit my Lovable site for SEO issues. I think I'm missing some things."

**Expected Claude behavior:**

Asks for page list and domain. Then produces a checklist-based audit report:

```
AUDIT SUMMARY
=============
Pages audited: 4 (/, /tools, /blog, /about)
Pages fully compliant: 1 (/)
Issues found:
  - /tools: missing meta description
  - /tools: H1 does not contain primary keyword
  - /blog: no Article schema
  - /about: OG image missing
  - All pages: no robots.txt found at /public/robots.txt
  - All pages: no sitemap.xml found
  - All pages: no llm.html found

Recommended fixes (in priority order):
  1. Add robots.txt and sitemap.xml (crawlability foundation)
  2. Fix missing and duplicate meta descriptions
  3. Add Article schema to /blog
  4. Create unique OG images per page
  5. Create /llm.html for AI discoverability
```

---

## Non-Triggering Examples

These prompts should NOT trigger this skill:

- "How does SEO work?" -- general knowledge question, answer directly
- "Write a blog post optimized for the keyword tile installation Toronto" -- content writing task, not a site implementation task
- "What's the difference between H1 and H2?" -- general knowledge

This skill is for implementation and audit tasks on actual Lovable projects, not for explaining SEO concepts in the abstract.
