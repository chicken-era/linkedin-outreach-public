---
name: linkedin-outreach-public
description: Turn a customer-discovery thesis into a vetted LinkedIn contact list, send connection requests without a note, and draft tailored follow-up messages in one spreadsheet. Asks the user for their target markets, outreach details, and preferences first.
---

# LinkedIn customer discovery outreach

Use when the user wants to find people on LinkedIn for customer discovery or expert calls, send them connection requests, and have a tailored follow-up message ready for when they accept.

Order is always: SETUP -> GATHER -> SHEET -> CONNECT -> FINISH. The agent only ever sends connection requests, always without a note. It never sends messages, InMails, or emails.

## What the user needs (tell them this if anything is missing)

1. **A Claude plan that supports skills and file creation** (code execution), so Claude can build the spreadsheet.
2. **Google Chrome with the Claude in Chrome extension** installed, signed in to the same Claude account, and connected to this conversation. Chrome must stay open while the skill runs.
3. **Signed in to LinkedIn in that Chrome.** The skill uses the user's own LinkedIn session; it never asks for or types a password.
4. **Permission for Claude in Chrome to act on linkedin.com** when the extension asks.

If the user isn't sure how to set something up, point them to https://support.claude.com (search "Claude in Chrome" and "skills") rather than guessing menu paths.

## Preflight check (run first, silently, before Setup)

1. **Browser tools.** Look for tools named `mcp__claude-in-chrome__*`. If only `enable__mcp__claude-in-chrome` is present, call it first. If they're deferred, load them with one ToolSearch call (tabs_context_mcp, tabs_create_mcp, navigate, javascript_tool, computer, get_page_text, find, tabs_close_mcp). Then call `tabs_context_mcp` with createIfEmpty: true.
   - If no Chrome tools exist or the call fails (extension not connected), stop and tell the user requirement 2 in plain words, then wait.
2. **LinkedIn session.** Navigate a new tab to https://www.linkedin.com/feed/. If the page shows a sign-in form, ask the user to sign in to LinkedIn in that Chrome window themselves, then continue. Never enter credentials.
   - If the extension asks for site permission, tell the user to allow linkedin.com and wait.
3. **Spreadsheet.** Check that code execution works and `openpyxl` imports (install it with pip if missing). If no file creation is available, produce the same tables as CSV, or as a Sheets artifact if the session offers one.
4. If everything passes, tell the user in one line that setup looks good and go to Setup.

**Fallback if the browser can't be connected:** offer to still do Setup, pick the companies, and build the sheet with a LinkedIn search link per company and the message drafts, so the user can find and connect with people by hand.

## Phase 0: SETUP (ask before doing anything)

Ask these in one short round (use a multiple-choice question tool if available; the user can always type their own answer). Skip any the user already answered in their request. Don't start searching until the required ones are answered.

**Required**
1. **Your name and how to sign** - full name for the intro line and the name to sign with (e.g. "Maria Lopez", signs "Maria").
2. **Who you are / affiliation** - e.g. "MBA student at Stanford in a venture design class", "founder of an early-stage startup", "PhD researcher at MIT".
3. **Purpose of the calls** - what you're trying to learn (e.g. "how hospitals choose remote monitoring tools", "where on-device AI hits its limits").
4. **Target markets / segments** - the industries, product types, or customer groups to search (e.g. hearables, wearables, smart cameras, industrial sensors). Ask for 3-6.
5. **How many contacts** - total people to find (suggest 30). Ask whether to include a few expert contacts (vendors, consultants, researchers) and how many (suggest 3).

**Recommended (offer defaults)**
6. **Target titles / roles** - who makes or influences the decision you care about (e.g. firmware engineer, head of hardware, product manager, clinical lead). If unsure, propose titles from their purpose.
7. **Companies to include or exclude** - e.g. exclude competitors or vendors as customers.
8. **Location preference** - e.g. SF Bay Area, Boston, "anywhere" (default: anywhere, prefer the user's region).
9. **Warm-connection preferences** - their school(s) for alumni preference; prefer 2nd-degree connections (default: yes).
10. **Outreach message** - paste their own template, OR just confirm name + intention and the agent drafts one from the default template below. Show the user one filled example and get a yes before using it for everyone.
11. **Connect automatically or review first?** - default: build the sheet, then connect to everyone on it. If the user picks "review first", stop after the sheet and wait for an explicit yes naming who to connect with.
12. **Existing lists to skip** - any earlier contact sheet to dedupe against, and their existing segment names to reuse.
13. **Found By** - name to put in the Found By column (default: the user's first name).

Save the answers as the run's settings and use them everywhere below. Don't invent anything the user didn't give; ask.

## Segments

Use the user's own segment names when they have them. Otherwise, create one segment per target market from setup. Put vendors / competitors under "Industry Players" and academics under "Researchers".

For each segment, write a hypothesis as 3-4 short testable bullets, grounded in the user's purpose. Example (for an edge-AI hardware thesis, hearables segment):
- Battery life is a top buying factor and the battery is tiny.
- Always-on audio AI uses a large share of that battery.
- Teams would add more AI features if inference used less energy.
- Changing chips is slow and costly, which may limit adoption.

## Phase 1: GATHER (bulk, from search results only)

1. Pick companies across the user's target markets (roughly half the contact count, 1-2 people each). Mix startups and large companies, respect location preference and exclusions.
2. Reuse the tab from the preflight check (or open a new one). Only reuse the user's own tabs if they ask.
3. For each company, navigate to `https://www.linkedin.com/search/results/people/?keywords=<Company>%20<title keywords>` using the user's target titles. Navigate must be its own call; then extract with javascript_tool:
```js
await new Promise(r=>setTimeout(r,1500));const seen=new Map();document.querySelectorAll('main a[href*="/in/"]').forEach(a=>{const u=a.href.split('?')[0];if(seen.has(u))return;let el=a,best=null;for(let i=0;i<10&&el;i++){el=el.parentElement;if(!el)break;const us=new Set([...el.querySelectorAll('a[href*="/in/"]')].map(x=>x.href.split('?')[0]));if(us.size>1)break;best=el}const t=(best||a).innerText.replace(/\s*\n+\s*/g,' | ');if(/•/.test(t))seen.set(u,t.replace(/ \| (Message|Connect|Follow).*/,'').replace(/, United States/,'').slice(0,110))});[...seen].map(([u,t])=>u.replace('https://www.linkedin.com/in/','')+' :: '+t).join('\n')
```
4. Keep real people whose headline names the company and a target title. Prefer the user's location, 2nd-degree connections, and alumni of their schools. Never guess a company the headline doesn't show; skip unclear results and note them.
5. Dedupe by LinkedIn URL against earlier lists.

## Phase 2: SHEET

Build one .xlsx (Arial) and send it to the user with Stage = "To contact". Then either go to CONNECT (default) or stop for review, per setup answer 11. Also stop and ask if someone doesn't clearly match the targets.

**Sheet "Contacts"**, exactly these columns:
| Contact Name | Company / Role | Segment | Found By | Stage | Email / LinkedIn |
- Company / Role = "<profile title> at <company>".
- Stage: To contact -> Contacted (request sent). Later stages are the user's.

**Sheet "Messages"**: Contact Name | LinkedIn message (send after they accept). Never sent by the agent.

**Sheet "New Segments"**: Segment | Hypothesis (bullets as above).

## Phase 3: CONNECT (paced, resumable)

For each person: navigate to the profile, then run this in javascript_tool:
```js
const sl=ms=>new Promise(r=>setTimeout(r,ms));await sl(2000);
const Q=s=>{const o=[...document.querySelectorAll(s)];document.querySelectorAll('*').forEach(h=>{if(h.shadowRoot)o.push(...h.shadowRoot.querySelectorAll(s))});return o};
const name=document.title.split(' | ')[0].replace(/^\(\d+\)\s*/,'').trim();const first=name.split(' ')[0];
const top=document.querySelector('main section');const head=top?top.innerText.slice(0,500):'';
const isInv=e=>{const a=e.getAttribute('aria-label')||'';return a.startsWith('Invite '+first)&&a.endsWith('to connect')};
let r;
if(!top) r='no top card';
else if(/\bPending\b/.test(head)) r='already pending';
else if(/·\s*1st/.test(head)) r='already 1st';
else{let b=[...top.querySelectorAll('[aria-label]')].find(isInv);
 if(!b){const more=[...top.querySelectorAll('button')].find(e=>/^More/.test((e.getAttribute('aria-label')||e.innerText||'').trim()));
  if(more){more.click();await sl(1000);b=Q('[aria-label]').find(e=>isInv(e)&&e.getClientRects().length&&!e.closest('aside'));}}
 if(!b) r='NO CONNECT OPTION';
 else{b.click();let s;for(let i=0;i<10&&!s;i++){await sl(600);s=Q('button').find(e=>/without a note/i.test(e.innerText||''));}
  if(!s) r='NO SEND BUTTON';
  else{s.click();let ok=false;for(let i=0;i<8&&!ok;i++){await sl(500);ok=/\bPending\b/.test(document.querySelector('main section').innerText.slice(0,600));}r=ok?'SENT':'clicked send, unconfirmed';}}}
name+': '+r
```
Notes:
- The "Add a note?" dialog lives in a shadow root (#interop-outlet); plain document queries miss it.
- On NO SEND BUTTON / NO CONNECT OPTION / unconfirmed, take a screenshot: the dialog or menu is usually just slow. Click "Connect" in the More menu, then "Send without a note", and confirm via the Pending button or the "Invitation sent" toast. Some profiles keep a Follow button after sending; the toast is the proof.
- If javascript_tool times out (page frozen), re-navigate to the same profile and finish with screenshot + clicks.
- Send at most ~40 requests per session. Stop and tell the user if LinkedIn shows a limit, CAPTCHA, or an email-required prompt. Never solve CAPTCHAs.
- The Sent invitations page (https://www.linkedin.com/mynetwork/invitation-manager/sent/) only shows the most recent ~20; use it as a spot check and rely on each profile's Pending state for the rest.
- Set Stage = "Contacted" for confirmed people and close the tab.

## Message template

Use the user's own template if they gave one. Otherwise fill this default (Mom Test structure: vision, framing, weakness, pedestal, ask) with their setup answers, and confirm one example with them first:

```
Hi <First>, thanks for connecting.

I'm <User full name>, <affiliation>. We're trying to understand <segment-specific decision this person makes, from the user's purpose>, and public sources don't tell us how those tradeoffs get made in practice.

As <a/an/the> <Title> at <Company>, you're close to exactly those decisions, so your experience would help us a lot. We're not selling anything.

Would you be open to a 20 minute chat sometime this week or next?

Best,
<User sign-off name>
```

Rules:
- Tailor the "trying to understand" line to each person's segment and role.
- For expert contacts (vendors, consultants), use a variant like: "what limits customers run into when they try to <do the thing>, and public sources don't show where those projects actually get stuck" + "you see many of those projects up close".
- Use "the" for unique roles (CEO, Head of X, Director of X), "a/an" otherwise; reword awkward titles into a natural phrase.
- Don't pitch a product, don't claim familiarity beyond what's on the profile, ask about past experience rather than hypotheticals. Drop "We're not selling anything" if it isn't true for the user.

## Finish

Send the final spreadsheet (Stage updated), say how many requests were confirmed sent, and list anyone skipped and why.

## Responsible use

This skill acts in the user's own LinkedIn account. Users are responsible for following LinkedIn's User Agreement and professional community policies. Keep batches small, target only people relevant to a genuine discovery purpose, and never use it for spam or mass selling.
