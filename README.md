# linkedin-outreach-public

A Claude skill for customer discovery. You describe what you're trying to learn and who you want to talk to. Claude then:

1. Finds relevant people on LinkedIn, by company and job title.
2. Builds a spreadsheet: Contact Name, Company / Role, Segment, Found By, Stage, LinkedIn URL.
3. Sends them connection requests without a note, from your own LinkedIn account.
4. Writes a tailored follow-up message for each person, ready to send once they accept. It never sends messages for you.

## Before you start

You need:
- A Claude plan that supports skills and file creation.
- Google Chrome with the **Claude in Chrome** extension installed and connected to your Claude account.
- To be signed in to LinkedIn in that Chrome.

Help: https://support.claude.com (search "Claude in Chrome" and "skills").

## Install

Download the `linkedin-outreach-public` folder (it contains `SKILL.md`) and add it as a skill in Claude.

## Use

Open a new chat and say something like:

> Use the linkedin-outreach-public skill to find 30 people for customer discovery.

Claude first checks that Chrome and LinkedIn are connected. Then it asks for your name, your affiliation, what you want to learn, your target markets and titles, how many contacts you want, and your outreach message (or it drafts one from a proven template and shows you an example first).

## Responsible use

The skill acts in your own LinkedIn account. You're responsible for following LinkedIn's User Agreement. Keep batches small (it caps at about 40 requests per session), contact only people relevant to a genuine discovery purpose, and never use it for spam or mass selling.
