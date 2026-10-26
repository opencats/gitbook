# Using the software

This guide covers the released OpenCATS 0.12.0 workflows. The interface now uses Bootstrap 5.3; the core Company, Contact, Job Order, Candidate, Activity and Calendar records remain familiar.

Start with [Companies, contacts, job orders, and candidates](companies-contacts-job-orders-and-candidates.md), then use [Screens In-Depth](screens-in-depth.md) for navigation and reporting.

## Changes to expect in 0.12.0

* **Forms and dialogs:** refreshed styling and layouts across the main modules, login, installer and Career Portal. Use the labelled controls; positions can vary with screen width and local customisations.
* **Rich text:** SunEditor replaces CKEditor for formatted content. Check the preview before sending an email or publishing a job description, especially when pasting formatted text or using older templates.
* **Charts:** Hiring Overview, job-order pipeline and EEO charts use Chart.js. Associated tables expose exact values. The recruiting-summary PDF shows a numeric summary instead of a graph image.
* **Dates:** administrators can select US, UK or ISO (`YYYY-MM-DD`) in Settings → Administration → Localization. Existing installations keep their chosen format.
* **Exports:** CSV exports through the export module require Site Administrator access. Other actions depend on your access level and the records you can see.
* **Career Portal:** returning-candidate verification uses CAPTCHA and a server-side session. Persistent “Remember my information” authentication has been removed.

## Lists and tags

Lists group records that users add to them. They are not live searches that automatically gain or lose members when candidate details change. Review membership and ownership regularly; a list's name does not guarantee that its records are still current. Tags are record labels, not a replacement for activity history or job-order pipeline status.

## Reading reports

Home Hiring Overview counts submission, interview and placement status-change events in four calendar periods. Repeated transitions can count more than once, and the current period is incomplete. Do not read these as distinct-candidate totals.

Job-order pipeline chart categories are cumulative and can overlap. They are not separate shares of a total. Use the pipeline table to inspect the individual candidates.

For shipped changes and upgrade considerations, see the [0.12.0 release notes](https://github.com/opencats/OpenCATS/releases/tag/v0.12.0). Future roadmap items are not necessarily available in this release.
