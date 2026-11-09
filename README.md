# linkedin-outreach-public

A Claude skill for customer discovery. You describe what you're trying to learn and who you want to talk to. Claude then:

1. Finds relevant people on LinkedIn, by company and job title.
2. Tracks them with Contact Name, Company / Role, Segment, Found By, Stage, LinkedIn URL, and a tailored message. You can keep this in a downloadable spreadsheet or in an app you already use, like Notion, Google Sheets, Airtable, or a CRM.
3. Sends them connection requests without a note, from your own LinkedIn account.
4. Writes a tailored follow-up message for each person, ready to send once they accept. It never sends messages for you.

## Before you start

You need:
- A Claude plan that supports skills.
- One browser Claude can drive:
  - **Claude in Chrome**: the Chrome extension, connected to your Claude account. It uses your existing LinkedIn sign-in in Chrome.
  - **Claude desktop app**: its built-in browser pane. Sign in to LinkedIn there once. *(Should work; less tested than Chrome so far.)*
- To be signed in to LinkedIn in that browser.
- Optional: your tracking app (Notion, Google Sheets, Airtable, HubSpot...) connected to Claude as a connector, if you want contacts kept there.

Help: https://support.claude.com (search "Claude in Chrome", "skills", or "connectors").

## Install

Download this repo (green **Code** button → **Download ZIP**) and add the `linkedin-outreach-public` folder (it contains `SKILL.md`) as a skill in Claude.

## Use

Open a new chat and say something like:

> Use the linkedin-outreach-public skill to find 30 people for customer discovery.

Claude first checks which browser it can use, and that you're signed in to LinkedIn. Then it asks for:
- your name and affiliation
- what you want to learn
- your target markets and job titles
- how many contacts you want
- your outreach message (or it drafts one from a proven template and shows you an example first)
- where to track contacts

## Responsible use

The skill acts in your own LinkedIn account. You're responsible for following LinkedIn's User Agreement. Keep batches small (it caps at about 40 requests per session), contact only people relevant to a genuine discovery purpose, and never use it for spam or mass selling.
