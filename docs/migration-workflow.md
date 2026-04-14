# Mother Meera USA Migration Workflow

This workflow converts content from `https://mothermeerausa.org/` into the Hugo structure for `https://mothermeera.us/`.

## Goal

Move WordPress content into Hugo as clean Markdown, remove WordPress-specific artifacts, and place each page into the new site structure:

- About
- Darshan
- Events
- Media
- Connect
- Contact

## 1. Inventory The Existing WordPress Site

Start with a content inventory before exporting anything.

Primary source areas currently visible on the site:

- Mother Meera
  - About Mother Meera
  - Mother's Work
  - Asking for Help
  - Inner Contact
  - Japa
- Darshan
  - What is Darshan?
  - Receiving Darshan
  - Darshan Details
  - Meditations Online
- Reservations
  - Make a Reservation
  - Darshan Schedule
- Get Involved
  - Volunteer
  - Locations
  - Stay Connected
  - Blog
- Donate
- Contact / footer information

Create a migration sheet with these columns:

- `wp_title`
- `wp_slug`
- `wp_type`
- `target_section`
- `target_path`
- `keep / merge / rewrite / omit`
- `notes`

## 2. Export WordPress Content As XML

Preferred source export:

1. In WordPress admin, go to `Tools > Export`.
2. Export `All content` as WordPress XML (WXR).
3. Save the file in a source-only directory outside Hugo content generation.

Recommended local staging:

```text
data/
  source/
    mothermeerausa-wxr.xml
```

Fallback if admin export is unavailable:

- Use the WordPress REST API for pages and posts.
- Use the public sitemap for URL discovery.
- Use direct HTML capture only as a last fallback, because it creates more cleanup work.

## 3. Parse XML Into A Structured Intermediate Format

Do not transform WXR directly into final Hugo content in one step. First normalize it into structured records.

Each record should capture:

- `title`
- `slug`
- `date`
- `status`
- `content_html`
- `excerpt_html`
- `source_url`
- `wp_type`
- `menu_order`
- `parent`

Intermediate output can be JSON or CSV. JSON is usually easier for nested WordPress fields.

## 4. Convert HTML To Markdown

For each record:

1. Read the WordPress HTML body.
2. Convert HTML to Markdown.
3. Preserve semantic elements where useful:
   - headings
   - paragraphs
   - lists
   - blockquotes
   - links
4. Keep tables only when they convey structured facts. Otherwise rewrite them as lists.

Recommended conversion approach:

- Use `pandoc` or a purpose-built converter to go from HTML to Markdown.
- Keep links intact.
- Preserve line breaks only when they represent real structure.

## 5. Remove WordPress Artifacts

Clean every converted file before mapping it into Hugo.

Remove or normalize:

- shortcodes
- embed wrappers
- `wp-image-*` classes
- alignment classes such as `alignleft`, `aligncenter`, `alignright`
- inline `style=""` attributes
- empty paragraphs
- repeated spacer blocks
- “read more” fragments
- footer or theme boilerplate
- “Proudly powered by WordPress”
- JavaScript-dependent UI fragments

Normalize:

- smart quotes and entities
- excessive line breaks
- heading hierarchy
- inconsistent capitalization
- bare URLs

## 6. Rewrite Into The Hugo Information Architecture

Map WordPress content into the new Hugo structure instead of preserving the old navigation literally.

### Suggested mapping

`Mother Meera` pages -> `content/about/`

- `About Mother Meera`
- `Mother's Work`
- `Asking for Help`
- `Inner Contact`
- `Japa`

`Darshan` pages -> `content/darshan/`

- `What is Darshan?`
- `Receiving Darshan`
- `Darshan Details`
- `Meditations Online`

`Reservations` -> `content/events/` or `content/connect/`

- `Make a Reservation`
  - likely becomes event logistics copy or an external registration note
- `Darshan Schedule`
  - becomes the main events landing page plus structured event entries when dates exist

`Get Involved` -> `content/connect/`

- `Volunteer`
- `Stay Connected`
- donation-related explanatory copy

`Locations` -> future `content/cities/`

- do not force this live if the cities system is not ready
- migrate the content into draft city entries or a staging document first

`Blog` / recent updates -> evaluate case by case

- keep only material that still belongs on the new site
- avoid importing time-bound WordPress updates that no longer serve the Hugo structure

`Media` -> `content/media/`

- convert photo/video pages into metadata-driven entries
- do not copy large media binaries into Git

`Contact` -> `content/contact/`

- retain only current, confirmed contact information

## 7. Create Hugo Front Matter During Import

Use the new archetypes, not ad hoc front matter.

### Standard pages

```toml
+++
title = "Page Title"
date = 2026-04-13T00:00:00-07:00
draft = true
description = ""
summary = ""
+++
```

### Events

Use the structured event archetype with:

- `event_start`
- `event_end`
- `eventStatus`
- `location`
- `organizer`
- `offers`

### Media

Use metadata entries with:

- `mediaType`
- `thumbnail`
- `source_url`
- `youtube_id` or `embed_url`

### Cities

Store draft city records for future rollout rather than mixing city-level information into event pages.

## 8. Editorial Cleanup Pass

After conversion, rewrite each file for the new site voice.

Required editorial rules:

- remove promotional language
- prefer observational phrasing
- define meaning plainly
- preserve factual information
- shorten long WordPress-era introductions
- break long pages into scannable sections

Every migrated page should include at least one clear explanatory sentence that states what the page is about or what the reader should understand from it.

## 9. Asset Handling

Do not import large media files into Git.

Workflow:

1. Extract referenced image and video URLs.
2. Decide which assets are still needed.
3. Store selected media outside the Git repo.
4. Reference assets from Hugo content using remote URLs or managed static paths.

Use Git only for:

- Markdown
- front matter
- lightweight static assets
- templates and styles

## 10. QA Before Publish

For each migrated batch:

1. Run `hugo -D`.
2. Check internal links.
3. Check headings and summaries.
4. Check JSON-LD output on event pages.
5. Confirm each page lands in the correct section.
6. Confirm no WordPress UI or theme artifacts remain.
7. Confirm media entries do not require local large files.

## 11. Migration Order

Use this order to reduce rework:

1. Core pages
   - homepage source copy
   - About
   - Darshan
   - Contact
2. Connect pages
   - Volunteer
   - Stay Connected
   - donation support language
3. Events
   - schedule language
   - reservation guidance
   - structured event entries
4. Media
   - videos first
   - then photos with captions and alt text
5. Cities
   - stage as draft content only
6. Blog or archive decisions
   - migrate only if content still serves the new site

## 12. Definition Of Done

A page is considered migrated only when:

- the content is in Markdown
- WordPress artifacts are removed
- front matter matches the Hugo content model
- the page is assigned to the correct section
- the language matches the new tone
- links and metadata are checked
- the page builds cleanly in Hugo
