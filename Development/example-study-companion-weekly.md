# Angular Component

**Component:**
**Module:**
**Date:** 4/22/2026


![screenshot-20260423112200469](/api/file/raw?path=Development%2Fscreenshot-20260423112200469.png)



What the page implements
User story chosen: *"As a student, I want to see all tasks for the week in a clear view, so I can plan my study time and priorities quickly."*

## Composition of approved components:

![screenshot-20260422195658579](/api/file/raw?path=screenshot-20260422195658579.png)

## WCAG 2.2 AA touchpoints covered

All icon-only controls (sl-toggle-button, add-task buttons) carry aria-label + sl-tooltip linked via aria-describedby.

Information never relies on color alone — subject/priority/status each show text + badge color + icon.

Native sl-search-field, sl-select, sl-date-field, sl-tab-group provide keyboard support and screen-reader semantics out of the box.
Responsive layout: toolbar collapses to a single column at ≤ 720px (mobile-first).

sl-panel collapsible — supports "show me only what matters now" for neurodivergent users.