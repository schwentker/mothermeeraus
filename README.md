# Mother Meera US (Hugo Site)

## 1. Project Overview

Mother Meera US is a Hugo-based migration and redesign of the existing WordPress site at `https://mothermeerausa.org/`.

The project has three practical goals:

- preserve essential information about Mother Meera and darshan
- publish clear event information for US-based darshan gatherings
- support a restrained media and connect layer without turning the site into a campaign

Stack:

- Hugo
- GitHub
- Cloudflare Pages

Why Hugo:

- fast static output
- simple content model built from Markdown and templates
- low operational overhead
- reliable delivery through static hosting and CDN caching

Why migrate from WordPress:

- WordPress carries theme, plugin, and admin complexity that this site does not need
- the new site benefits from simpler publishing, fewer moving parts, and cleaner markup
- a static site is easier to optimize for search, AI interpretation, and long-term maintenance

## 2. Core Intent

This site is built as a three-layer system:

1. Presence
   Mother Meera and darshan are the center of the site. Visitors should be able to understand what the site is about without already knowing the vocabulary.
2. Movement
   Events are the time-based layer. They carry dates, locations, contacts, and structured details that change over time.
3. Field
   Media and Connect provide context around the core material. They should support understanding and participation without overwhelming the primary informational role.

This is not a marketing site. It is designed to prioritize clarity, trust, and accessibility over persuasion.

## 3. Key Concept: Darshan

Homepage definition:

> Darshan is the time when each person comes before Mother Meera individually and receives her blessing in silence. People usually wait quietly, approach when called, sit or kneel briefly, and then move on so others may come forward.

Full explanation:

- [`/darshan/`](./content/darshan/_index.md)

This definition is central because the site does not work without it. Most pages either explain darshan directly, support access to it through event information, or provide context around it through media and contact information.

## 4. Information Architecture

Main sections:

- `About`
  Background on Mother Meera and the purpose of the site.
- `Darshan`
  Plain explanation of what darshan is, what happens, and what visitors should expect.
- `Events`
  Structured listings for confirmed darshan dates, places, and local contact details.
- `Media`
  Photo and video records with descriptive text and external media references.
- `Connect`
  Social links, volunteering, and donations in a restrained informational frame.
- `Contact`
  Direct contact information for questions, corrections, and logistics.

## 5. Content Model (Hugo)

### Events

Events are structured content, not free-form posts. They should carry practical information that can also be expressed in schema markup.

Typical fields include:

- `event_start`
- `event_end`
- `eventStatus`
- `location`
- `organizer`
- `offers`
- city and address details
- local contact details

### Media

Media distinguishes between photo and video entries.

- photos use metadata plus a lightweight preview image when available
- videos use external hosting and embed references, such as YouTube
- large original media files should not be committed to Git

### Cities (future)

Cities are planned as a reusable content layer for durable location-level information. This should eventually separate recurring local context from one-off event announcements.

Example event front matter:

```toml
+++
title = "Example Darshan Event"
date = 2026-04-13T00:00:00-07:00
draft = true
description = "Structured event content for a darshan gathering."
event_start = "2026-09-18T18:00:00-07:00"
event_end = "2026-09-18T21:00:00-07:00"
featured = true
eventStatus = "https://schema.org/EventScheduled"

[location]
type = "Place"
name = "Example Retreat House"

[location.address]
streetAddress = "123 Example Way"
addressLocality = "San Francisco"
addressRegion = "CA"
postalCode = "94110"
addressCountry = "US"

[organizer]
type = "Organization"
name = "Mother Meera US"
url = "https://mothermeera.us/"
email = "events@example.org"

[offers]
type = "Offer"
url = "https://mothermeera.us/events/example-darshan-event/"
price = "0"
priceCurrency = "USD"
availability = "https://schema.org/InStock"
+++
```

## 6. Repository Structure

Key folders:

- `/content`
  Markdown content for pages, events, media, and future cities.
- `/layouts`
  Hugo templates for page rendering, list views, single views, partials, and schema output.
- `/static`
  Files copied directly to the built site. This is where `llms.txt`, favicons, and small static assets should live.
- `/data`
  Structured source data for future integrations, migration staging, or shared content records.
- `/archetypes`
  Front matter templates for new content types.
- `/assets`
  Source assets such as CSS that Hugo processes into the final site.
- `/public`
  Generated output from `hugo`. This is build output, not source content.
- `/docs`
  Internal project documentation such as migration workflow notes.

Hugo generates static HTML from Markdown content and templates. Content in `/content` is combined with templates in `/layouts`, processed assets, and static files to produce the final site in `/public`.

## 7. Development Setup

Install Hugo:

```bash
brew install hugo
```

Run the local server:

```bash
hugo server
```

Run a production build:

```bash
hugo
```

Useful development build when working with drafts:

```bash
hugo -D
```

Build output goes to `/public`.

## 8. Deployment

Deployment is intended for Cloudflare Pages.

Recommended setup:

- GitHub repository as the source
- automatic deploys on push to `main`
- build command: `hugo`
- output directory: `public`

This keeps deployment simple and matches the static delivery model of the site.

## 9. Content Workflow

### Add a new event

Create a new event from the archetype:

```bash
hugo new events/example-event.md
```

Then fill in:

- title and description
- start and end dates
- location name and address
- organizer details
- offers block, even if free
- local contact or event reference

### Update a standard page

Edit the relevant Markdown file in `/content`, for example:

- `content/about/_index.md`
- `content/darshan/_index.md`
- `content/connect/_index.md`

### Add or update media

- create a media entry in `/content/media`
- use external hosting or remote preview URLs when needed
- avoid committing large image or video files into Git

### Why large media is not stored in Git

Large media makes the repository slower to clone, harder to review, and less stable over time. The source of truth for this project should be content, structure, and presentation, not binary storage.

## 10. AEO + SEO Strategy

This site is optimized for both search engines and AI systems.

Core elements:

- JSON-LD for `Person`
  Used to describe Mother Meera as the central entity.
- JSON-LD for `Event`
  Used on event pages to express date, place, organizer, status, and offers in machine-readable form.
- FAQ schema
  Used for concise repeated explanations of core visitor questions.
- `llms.txt`
  A lightweight guidance file for AI systems interpreting the site.

The goal is not keyword inflation. The goal is clean structure, factual copy, and machine-readable context.

## 11. Design Principles

Principles:

- minimal and contemplative
- image-led but always supported by semantic text
- serif typography
- neutral palette
- generous spacing
- no marketing language

Good tone:

- “Darshan is the time when each person comes before Mother Meera individually and receives her blessing in silence.”
- “Confirmed dates will appear here as event details are added.”
- “Donations help cover ordinary practical costs when they arise.”

Bad tone:

- “Experience a profound transformational blessing.”
- “Join our growing spiritual movement today.”
- “Do not miss this life-changing event.”

The site should remain calm, precise, and grounded.

## 12. Connect Layer

The Connect layer includes:

- social links
- volunteering
- donations

Its role is supportive, not promotional. The guiding principle is invitation rather than persuasion. Social and volunteer pathways should be practical. Donations should be visible enough to find and quiet enough not to dominate the page.

## 13. Future Directions

Possible next steps:

- CMS integration through Decap CMS or Tina
- city-level content and routing
- multilingual support
- a richer media system with transcripts, captions, and better indexing
- stronger structured data coverage for FAQs and media entries

## 14. Contributing

How to propose changes:

- open an issue or pull request
- describe the content or design problem clearly
- explain how the change improves clarity, structure, or maintainability

Contribution standards:

- preserve factual, non-promotional tone
- prefer shorter, clearer sentences
- do not introduce marketing language
- keep semantic HTML and structured content intact
- avoid adding large binary assets to the repository

## 15. License / Credits

License: TBD

Credits:

- Source content and reference structure: `https://mothermeerausa.org/`
- Site rebuild and static implementation: Mother Meera US Hugo project
