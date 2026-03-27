---
layout: default
title: GOV.UK Accessibility Skill
---

# GOV.UK Accessibility Skill — Example

This is an example Agent Skill for accessibility. It activates automatically when Copilot is working on Nunjucks templates, HTML, or SCSS. It provides richer WCAG 2.2 AA guidance than can comfortably fit in an instruction file, and complements the [Accessibility Advisor agent]({{ "/pages/agents/accessibility-advisor" | relative_url }}).

Copy the `govuk-accessibility/` folder into `.github/skills/` in your repository.

## Directory structure

```
.github/skills/
└── govuk-accessibility/
    └── SKILL.md
```

## Example file contents

The content below goes into your `.github/skills/govuk-accessibility/SKILL.md` file:

---

{% raw %}
````markdown
---
name: govuk-accessibility
description: WCAG 2.2 AA compliance and GOV.UK Design System patterns for government services. Use when writing or reviewing Nunjucks templates, HTML, SCSS, or any user-facing UI component. Activates for GOV.UK Frontend projects and government service pages.
license: OGL-UK-3.0
metadata:
  author: defra-digital
  version: "1.0"
compatibility: Requires GOV.UK Frontend 5.x or later
---

# GOV.UK Accessibility

UK government services must meet WCAG 2.2 Level AA under the [Public Sector Bodies Accessibility Regulations 2018](https://www.legislation.gov.uk/uksi/2018/952/made). Apply the guidance below when generating or reviewing any user-facing code.

## Core principle

Prefer native HTML over ARIA. Use semantic elements (`<button>`, `<nav>`, `<main>`, `<fieldset>`, `<table>`) before reaching for `aria-*` attributes. ARIA supplements HTML — it does not replace it.

## GOV.UK Frontend — macro-first approach

Use GOV.UK Frontend macros for all standard components. Do not build custom HTML for components the design system already provides.

✅ Use the macro:
```njk
{{ govukButton({ text: "Continue" }) }}
```

❌ Do not hand-roll:
```html
<button class="govuk-button">Continue</button>
```

Available macros include: `govukButton`, `govukInput`, `govukRadios`, `govukCheckboxes`, `govukSelect`, `govukTextarea`, `govukDateInput`, `govukTable`, `govukDetails`, `govukNotificationBanner`, `govukErrorSummary`, `govukFieldset`, `govukBreadcrumbs`, `govukPagination`, `govukTag`, `govukPanel`.

## Forms

Every form must:

1. Associate every `<input>`, `<select>`, and `<textarea>` with a visible `<label>` using `for`/`id` pairing
2. Group related inputs (radios, checkboxes) in a `<fieldset>` with a `<legend>`
3. Mark required fields with the `required` attribute and indicate them visually in the label text
4. Set `autocomplete` attributes on personal data fields:
   - `name` on full name fields
   - `email` on email fields
   - `tel` on phone fields
   - `postal-code` on postcode fields
   - `bday-day`, `bday-month`, `bday-year` on date of birth inputs
5. Use `govukInput` or `govukRadios` macros rather than bare HTML

## Error handling

When a form submission fails:

1. Show a `govukErrorSummary` at the top of the page before the form
2. Link each error in the summary to the corresponding field using its `id`
3. Show an `errorMessage` on the individual field
4. Move focus to the error summary on page load (GOV.UK Frontend does this automatically via `data-module="govuk-error-summary"`)

```njk
{{ govukErrorSummary({
  titleText: "There is a problem",
  errorList: [
    {
      text: "Enter your email address",
      href: "#email"
    }
  ]
}) }}
```

## Headings and page structure

- One `<h1>` per page — it must be the first visible heading and match the `<title>` tag
- Heading levels form a logical hierarchy — do not skip levels (h1 → h2, not h1 → h3)
- Use landmark regions: `<main>`, `<nav>`, `<header>`, `<footer>`
- The `<title>` element must follow the pattern: `Page name — Service name — GOV.UK`

## Colour and contrast

- Body text must meet 4.5:1 contrast against its background (WCAG 1.4.3)
- Large text (18pt or 14pt bold) must meet 3:1 contrast
- Focus indicators must meet 3:1 contrast against adjacent colours (WCAG 2.4.11 — new in WCAG 2.2)
- Never use colour as the sole indicator — always pair with a text label, pattern, or icon
- Use GOV.UK Frontend colour tokens — do not hard-code hex values for design system colours

## Images

- Decorative images: `alt=""`
- Informative images: meaningful `alt` text describing the content, not the appearance
- Do not use images of text (WCAG 1.4.5)
- Complex images (charts, diagrams): provide extended description in surrounding text or `aria-describedby`

## Links and buttons

- `<button>` for actions (submit, toggle, open modal)
- `<a>` for navigation (goes to another URL)
- Link text must describe the destination — no "click here", "read more", or bare URLs
- Links that open a new tab must warn the user: `Download the accessibility statement (opens in new tab)`

## Dynamic content and JavaScript

- Status messages (success banners, alerts) must use `role="alert"` or `aria-live="polite"` so screen readers announce them without focus moving
- Modal dialogs must trap focus within the dialog and restore focus to the trigger element when closed
- On single-page-style navigation, update the `<title>` and move focus to the main heading
- Use `data-module` attributes for GOV.UK Frontend JavaScript enhancement — do not wire up components manually

## What requires manual testing

Automated tools catch around 30% of accessibility issues. Always test these manually:

- Screen reader announcement of dynamic content (NVDA + Firefox, Narrator + Edge on Windows; VoiceOver + Safari on macOS/iOS)
- Keyboard-only navigation through forms, modals, and interactive components
- Colour contrast in dark mode and high contrast mode (Windows)
- Responsive layout at 320px viewport width (WCAG 1.4.10)
- 400% zoom without horizontal scrolling (WCAG 1.4.4)

Mark any code that requires manual testing with an HTML comment:
```html
<!-- NEEDS SCREEN READER TESTING: focus management on modal close -->
```
````
{% endraw %}

---

[Back to skills index]({{ "/pages/skills" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
