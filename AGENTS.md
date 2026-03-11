# FeedLint — Agent Interface

## What it does

FeedLint validates RSS 2.0 and Atom 1.0 feed XML against the spec. Given raw feed XML, it returns a list of errors, warnings, and informational notices — including Apple Podcasts namespace requirements.

## Current surface

**Browser-side only (Tier 1 probe).** The validation logic runs in JavaScript via DOMParser. There is no server API yet.

To use programmatically today, replicate the validation logic in `index.html` (see the `validateFeed()`, `validateRSS2()`, `validateAtom()`, and `validatePodcast()` functions).

## Planned MCP tool (Tier 2)

```
tool: validate_feed
input:
  xml: string          # raw RSS or Atom XML content
output:
  valid: boolean       # true if no errors (warnings allowed)
  format: string       # "rss2" | "atom" | "unknown"
  errors: Issue[]      # blocking spec violations
  warnings: Issue[]    # non-blocking best-practice violations
  infos: Issue[]       # informational suggestions

Issue:
  level: "error" | "warn" | "info"
  msg: string          # human-readable description
  path: string         # XML path to the offending element
  fix: string          # suggested correction
```

## What it validates

**RSS 2.0:**
- Required channel elements: title, link, description
- Required item elements: title or description
- Recommended: guid, pubDate, language, lastBuildDate
- Apple Podcasts namespace: itunes:author, itunes:image, itunes:explicit, itunes:category
- Episode enclosures: url, type, length attributes
- Date formats: RFC 822

**Atom 1.0:**
- Required feed elements: title, id, updated
- Required entry elements: title, id, updated
- Recommended: author, link[rel=self], content or summary
- Date formats: ISO 8601

## Pricing

Free. No API key required for the browser tool.
Future MCP tool: free tier (10 validations/day), paid ($5/mo, unlimited).

## Discovery

- Live at: https://feedlint.radicchio.page
- GitHub: https://github.com/ruemic/feedlint
- Support: https://bitterdesk.com
