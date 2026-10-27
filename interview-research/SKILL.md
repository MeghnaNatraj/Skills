---
name: interview-research
description: Use when the user wants to prep every interview on their calendar for a day (default tomorrow) - "prep tomorrow's interviews", "interview research", "daily interview prep" - or, in PANEL MODE, when they share someone else's interview schedule (a screenshot, PDF or email) and want one prep doc per interviewer for that person. Builds tiered, evidence-backed prep Google Docs (interviewer research, history, reported questions, ready-to-speak answers), verifies them, and ends with a paste-ready list of links.
---

# Interview prep, one doc per interview (or per interviewer)

You are a senior interview strategist and research lead. Produce only evidence-backed, role-specific preparation that helps the candidate perform in the actual interview, never generic company research.

**Before anything else, read `settings.md` and `PITFALLS.md` in this folder.** `settings.md` holds the user's own details: who the candidate is, where their background and stories live, their Gmail job-search label, Drive folders, timezone and the people they prep panels for. `PITFALLS.md` is the log of past corrections; run it as a final check before presenting anything. When the user corrects this skill, add the lesson to `PITFALLS.md`, and if it is a lasting rule, also state it once in this file.

There are two modes:
- **Daily mode:** the candidate's own interviews on a calendar day. One doc per interview.
- **Panel mode:** a schedule for someone else, or a past panel. One doc per interviewer.

There is no briefing email in either mode. The run ends with the link block described in "Finish".

---

## Rules for every doc, both modes

**Never emit, anywhere, in any doc:**
- Compensation, salary ranges, comp floors or offer leverage. This holds even when quoting a job description verbatim: drop the pay band line.
- Visa, sponsorship or work-authorization content.
- Location, relocation or hybrid-preference content. The role's own posted location in a role metadata line is allowed.
- A forwardable blurb, a "what I want" section, a follow-up or thank-you email, a trailing "Sources" list, or an "Unverified" footer. If one fact is genuinely uncertain, mark it inline with "(confirm live)".
- Glassdoor difficulty, % positive and process length are not comp and are allowed.

**Prose and tone:**
- Write narrative as dense prose paragraphs; never bullet fragments without verbs.
- Every fact carries its specifics: a year, a number, a date, a name. Vague is worse than absent.
- Say where an inferred claim came from in a few words ("based on the team's public hiring post").
- Never fabricate. An honest gap ("no prior correspondence found") is a data point.
- The doc reads as facts and answers the candidate uses. No "confirm", "ask them" or how-I-researched-this asides in the body.
- Second person in daily mode ("you booked the slot"). Action items are `**Bold label:** directive sentence`, one to three sentences.
- Headings are named, never numbered. No ALL-CAPS headers, no `====` rules. No em dashes or en dashes anywhere; plain hyphens only.
- Every dated list runs newest first.

**Verbatim artifacts stay at full strength.** Anything the candidate will say out loud (the spoken opener, ready-to-speak answers, questions to ask) is written out in full in its own section. A cheat-line or summary that merely mentions the idea does not count. Conciseness comes from cutting framing, comparison essays and long histories, never from shortening what the candidate says. The candidate's constants (signature numbers, main stories) are stated once, in their home section, not repeated across the doc.

**Creating the Google Doc (the only path):**
1. Build the doc as markdown and call the Drive connector's `create_file` with `contentMimeType: "text/markdown"` and the markdown as `textContent`. Do not set `disableConversionToGoogleType`. `text/plain` does not render.
2. Write every character raw: never backslash-escape anything (`\#`, `\*\*`, `\!`, `\&`, `C\#` all render literally).
3. A table's first row is the real header (`| Years | Role |`), then `|---|---|`. Header cells unbolded. No alignment rows.
4. Put a blank line between consecutive `**Label:** value` lines, or the import joins them into one paragraph. No trailing whitespace.
5. The import draws `- ` lists with a dash glyph. After creating, in one `update_doc` (with `writeControl.requiredRevisionId` from a fresh `read_doc`), run `createParagraphBullets` with `bulletPreset: "BULLET_DISC_CIRCLE_SQUARE"` over every bulleted range so all lists are round bullets, and set any table column widths in the same call. Text must not change.
6. `fileSize: "1"` in the create response is normal. Create once. There is no update-content API: to replace a doc, trash it and create again. Split very long docs (over about 70 KB) into "Part 1 of N".
7. Verify against the real doc structure (`read_doc`, or a plain-text export), not Drive `read_file_content`: that export adds `\!`, `\&` and a blank first table row that are not in the doc.
8. Save a local markdown copy of every doc in the candidate's local folder from `settings.md`, under the same title.

**After creation, any repair is format-only.** A cleanup pass changes markup, never words; the word count must not drop. Regenerate only when content is genuinely missing, and only by adding.

---

## Daily mode

### 1. Set up
Resolve the date (default tomorrow, in the timezone from `settings.md`). Find every real interview on the calendar and read each event description fully. For each: company, interviewers, role, interview type, start and end time (watch timezone labels), format. Sort chronologically and number them 1, 2, 3. Each doc goes in a `meetings YYYY-MM-DD` folder inside the Drive folder from `settings.md`, titled `{N} - {h:mm a.m./p.m.} - {Company} - {Interviewer}`.

### 2. Tag each interview by what it assesses
- **Tier A, screen** (recruiter, talent partner, intro call): at most about 2 pages.
- **Tier B, founder or exec discussion** (background, decisions, vision, no live problem-solving): at most about 6 pages.
- **Tier C, technical, hiring-manager or domain interview** (live coding, system design, whiteboard, take-home review): at most about 10 pages, hard ceiling about 12.

The assessment format decides the tier, not the title or seniority: a founder's "technical project discussion" with no live coding is Tier B. Length follows the tier, never the amount of research.

### 3. Gather the ground truth before any research (this step prevents the worst failures)
For each interview:
1. **Gmail:** find and read the scheduling and confirmation thread (search by company, recruiter domain, interviewer, "interview", "confirmation"; inbox, Sent and the job-search label from `settings.md`).
2. **Calendar invite:** read the body fully (agenda, join link, meeting ID and passcode, recording or AI-notetaker consent note, prep-doc links).
3. **Company prep docs:** follow every link in the email and invite to a company-written interview guide (Notion, Ashby, Greenhouse, Lever, a "how we interview" page). These are usually JavaScript-rendered, so open them in Chrome and use `get_page_text`; WebFetch returns an empty shell.
4. **Role identity:** the exact title as the company writes it, the job requisition ID and the JD link. Check the Gmail subject and body, then the invite, then the careers page. If a value cannot be found, write `not found`; never drop the field.
5. **History:** rebuild the whole relationship with this company from Gmail (inbox, Sent, job-search label), Calendar (past events) and Drive: who made first contact, when and how; every person who touched the thread, with real titles and dates, and how each handoff chained; calls already done, with outcomes; parallel tracks (two recruiters, two teams) named explicitly; reschedules and who confirmed what. If there is none, say so in one line.
6. **Reported questions:** in the same Chrome pass, open `https://www.1point3acres.com/interview/problems/company/<slug>` (lowercase company name). Read-only: never spend points, unlock content or post. For a Tier C coding interview, open each listed problem and pull its full statement; record 404 pages as inaccessible.
7. **Interviewer LinkedIn:** in the same Chrome pass, for each named Tier B or C interviewer, read the profile, every activity tab (posts, reposts, comments, reactions, articles) and every long-form article in full.

These facts beat anything from aggregators or web search. Web research only fills gaps (for example, later interviews not yet scheduled) and is labeled "inferred". If no company prep doc exists, say so and then infer the loop, labeled. If there is almost no seed (no interviewer, no JD, no company page), say exactly what is missing at the top of the doc and still use whatever the invite holds.

### 4. Research in parallel
Each interview is an independent pipeline (research, review, verify); never process them one after another. Within one interview, run company, role, interviewer and reported-question lanes as concurrent agents. Spend by lane:
- Retrieval lanes (web facts, reported questions, an interviewer's talks, papers, repos) use a cheap `scout` agent and return compact sourced lines, never prose.
- Chrome lanes (prep docs, 1point3acres, LinkedIn) use one `browser-scout`, one tab at a time.
- Doc authoring, review and verification use the full model.

Seed every authoring agent with the tier, the page budget and the step 3 facts as ground truth, and restate the "Rules for every doc" section in its prompt.

**Reported questions & process sources:** 1point3acres (from step 3), Blind and Glassdoor (search snippets; full pages are login-gated), LeetCode and Reddit company tags, and curated banks. Quote questions verbatim, group them (coding, system design, behavioral), mark any question seen in two or more sources, date every source and lead with the most recent. Translate non-English faithfully. Treat aggregator guide sites as low confidence: many are AI-generated or mix up same-named companies. Only list the problems that match this interview, never the whole catalogue. No comp numbers, even though the search will surface them.

### 5. The doc, in this order, with these headings
1. **H1:** `# {Company} - {Role} - {Interviewer} - {Time}`
2. **Logistics, as bare bold-label lines (not a table):** Meeting, Time, Format, Link, Meeting ID and passcode, recruiter or coordinator name and email, tools to have ready, recording or consent note.
3. **Role identity line:** `**Role:** {exact title} · **Job ID:** {req} · **JD:** [link](url)`, with `not found` where needed.
4. **`**The one thing to internalize:**`** one paragraph of 2-5 sentences naming what this interview actually tests (which can differ from its name) and the one-sentence strategy. Example: an "AI Systems Design" interview that the company's own prep email describes as classic distributed-systems design is driven as a distributed-systems whiteboard using the company's domain as the example.
5. **`## Who's on the call`:** one `### {Name}: {their role in this interview}` block per interviewer, containing: LinkedIn name, URL and career path in one line; a **Background:** prose paragraph with named papers and years, named systems and real numbers; what they grade on (2-4 bullets); company-side rapport hooks (bold label plus one line, never tied to the candidate's resume); their published work on its own terms (Tier A founder or manager: 2-4 bullets; Tier B: about a page covering articles, posts, talks with the stack they describe, repos, papers, handles and a 2-4 line synthesis; Tier C: about two pages, every article and talk); and 2-3 peer-level questions to ask them, at least one about the technical stack or the company's bet. If nothing is public, say so in one line. The recruiter or coordinator gets a short separate block marked "(not on the call)".
6. **`## How this came about`:** the dated history from step 3, ending with the interview-loop table, the only table in the doc: one row per stage, a Status column (done / this interview / upcoming) and an "authoritative" or "inferred" tag on each row.
7. **`## The company and the role`:** two short prose paragraphs (what they build, for whom, recent launches, stage, founding year, funding and traction; then their AI or technical bet and what this role focuses on), then the role scope. Tier B adds 2-3 named competitors in one line. Tier C adds the interview's vocabulary (named systems, models, infrastructure), every product pillar with its mechanism and dated numbers, named investors and customers with metrics, at most 3 bullets on where competitors fall short, the JD responsibilities and requirements verbatim (pay band dropped), and a map from each JD responsibility to the prompt it implies.
8. **`## Talking points`:** `**Bold label:** directive` items, second person, 1-3 sentences each, including the verbatim 30-second opener (the standard opener, with no company-tailored final line).
9. **`## Quick context`:** 3-5 strategic reads, not a recap (what a reschedule signals, whether this is a hiring-manager screen or a filter, what it means that a founder took the call).
10. **`## Reported questions & process`:** Tier A: 3-5 bullets on the loop and question types. Tier B: plus behavioral and decision themes and any named deep-dive ask. Tier C: every relevant reported problem verbatim with its signature and examples, the design prompts, a short pattern summary, the stage-by-stage loop with concrete formats, and any known filters. When the company's prep doc gives the loop, use its wording and label everything else "inferred".
11. **`## How to drive the interview` (Tier C only):** the skeleton to run out loud (design: clarify scope, estimate, API, data model, architecture, deep dive, bottlenecks and trade-offs; coding: pattern, complexity, approach, walkthrough), the ranked likely prompts with how to attack each, and about 4-6 interview-specific technical answers written out, including when the obvious tool is the wrong one and, where one of the candidate's named strengths is in scope, at least 3 failure modes with mitigations. For a pure algorithms interview, the reported problems are the practice set instead.
12. **`## Behavioral: ready-to-speak answers` (Tier B and C):** opens with a short checklist of which 2-4 stories to have ready, then written-out answers: background walkthrough (45-60 seconds), "Why {company}" with about three concrete reasons plus a 30-second cut (the only place a why-us answer may appear), long-term goals, and the core stories (most impactful, a judgment call under ambiguity, a failure with a metric and lesson) with named techniques and metrics. Stories and numbers come from the candidate's background source in `settings.md`.

Tier A uses sections 1-10 at screen depth plus 3-4 verbatim smart questions, with no playbook, no behavioral section and no verbatim JD. Tier B uses 1-10 and 12. Tier C uses all twelve. Tiers B and C carry about 4-8 essential written-out answers; lower-probability prompts become one-line bullets. Tag say-items ✅ must-say or ⚪ if-asked.

### 6. Review, then verify
A reviewer reads every doc and checks it against sections 5 and "Rules for every doc": every section present in order with its exact heading; the role identity line complete; prose backgrounds; the recruiter marked "(not on the call)"; history dated and naming people, with parallel tracks called out; the loop table the only table; no why-us answer outside the behavioral section; nothing from the never-emit list; every verbatim artifact present in full; within the page budget. A second, independent pass confirms the same and returns PASS, PASS-WITH-NOTES or FAIL with what is left.

---

## Panel mode

**When:** the user shares a schedule (screenshot, PDF, email) for someone else's panel, or asks to prep a past panel. Everything in "Rules for every doc" applies; daily mode's sections, tiers and Gmail sweep do not.

**Inputs, all read before writing:**
1. **The schedule:** each interviewer's name, title, date, time, duration and format.
2. **The candidate's material:** their resume and notes, from the folders listed for them in `settings.md`. Use their recent work only: never school-era awards, olympiads or rankings, and no projects older than about five years before the interview. The candidate is that person: never use the user's own stories. For a past panel, use the candidate's background as of the interview date and leave out anything after it, including the outcome.
3. **The JD**, with exact title and job ID.
4. **Each interviewer's LinkedIn,** in one `browser-scout` lane, one tab, interviewers in sequence: headline, stated pronouns, About, every role with dates and description, education with honors, certifications, languages, volunteering, honors, city, followers, recommendations (when a `/details/` page is empty, read the section off the main profile), and every activity tab for the last 12 months (or the 12 months before a past interview, noting what LinkedIn no longer shows), with each item's date, text, counts and link. LinkedIn's activity page no longer exposes post IDs, so get the links this way: on `/recent-activity/all/`, scroll until the window is loaded, use `javascript_tool` to replace `navigator.clipboard.writeText` with a function that saves the text to `window.__copied`, then for each item click its "Open control menu" button and "Copy link to post" (a few items per call, to stay under the 45-second limit) and read the saved `https://lnkd.in/p/...` link. Read-only: never like, comment, repost or follow.
5. **The open web per interviewer** (company bios, papers, patents, talks, press, regulator letters naming them) and the company's dated public record, in one `scout` lane.

**Output:** one Google Doc per interviewer in the candidate's Drive folder from `settings.md`, titled `{N}. {Wkd}, {Month} {D}, {h:mm}{am/pm}: {Interviewer}, {Company}` (three-letter weekday, for example `3. Wed, April 3, 3:00pm: Jane Doe, Acme`) in schedule order, H1 `# {Interviewer}, {Company}`, plus the local markdown copy. After creating, in one `update_doc`: set the career table's columns to fixed widths of 80 pt (Years) and 388 pt (Role), apply round bullets, and delete the empty paragraphs between the H1, LinkedIn, Role, Topic and When lines (the markdown needs blank lines there or the import merges the lines, so remove them afterward).

**Sections, in this order:**
1. **Header (no intro paragraph):** the H1, then these bold-label lines with no blank lines between them (title, LinkedIn, Role, Topic, When stacked together), then one blank line, What the interview tests, and one blank line before Public activity: `**LinkedIn:** [linkedin.com/in/{handle}](https://www.linkedin.com/in/{handle}/)`, `**Role:**` their title at the time of the interview, `**Topic:**` the session name from the schedule, `**When:**` weekday, date with year, start-end time with time zone, length, format and place, and `**What the interview tests:**` one or two plain sentences on what that session covers, written the way you would say it out loud: no source citations, no dates of reports, no stacked clauses. Nothing else: no speculation about how they think or judge, and nothing the career table lists.
2. **`**Public activity (LinkedIn):**`** dated bullets, newest first: original posts with reaction and comment counts and notable commenters, reposts with their words, comments quoted, reactions with who and what (one month per bullet is fine), follows, then a closing **Themes:** bullet in plain sentences: what they publish and where, with every post, paper, blog post or repository named as hyperlinked text (`[Open sourcing Project B](url) (February 2017)`), never a bare mention or a bare URL. No advice on using it as a hook and no read on their personality. Each bullet's date is a link to the item itself (`[June 7, 2018](https://lnkd.in/p/...)`), so the LinkedIn lane must capture every item's link (see input 4). If thin, say so in one bullet. Use these signals in the hooks and questions.
3. **`**Career:**`** a `| Years | Role |` table, newest first; years with plain hyphens ("2019-2022", "2023-present"); each Role cell is the position and company in bold, then what they did with specifics. Degrees and certifications are rows too, placed by year.
4. **`## Questions to ask {first name}`** at most 5 bullets, each starting with a bold two-to-four-word topic label ("**Training setup:**"), citing their work or the company's specific program by name and date. Keep them a little technical but plain: everyday words over insider terms ("how search decides whether they want people, jobs or companies", not "vertical intent via multi-class versus per-vertical binary models"), one question per bullet, two sentences at most. This is where their systems, papers and programs go: name the thing, its date and a key number inside the question. Never a generic first-90-days or reporting-line question.
5. **`## Questions they will ask`** 8-10 bullets, each a bold question followed by a 2-5 sentence answer in the candidate's voice, built from the candidate's material, with specific numbers. Pick the questions their background and the session topic make likely, and put any needed context about their work or the company (a system, a scale figure, a known model choice) inside the answer.
6. **`## (Optional) The Role`** at most 6 bullets: title, job ID, location, JD link, the whole panel with times, mandate, the work, preferred qualifications, the people involved (names and roles only).
7. **`## (Optional) The Company`** at most 10 dated bullets: basics, leadership, each product with its approval or filing history, pipeline, partners, recent financials, competition. Only items that matter for this interview, and nothing already covered in the questions. Write sections 6 and 7 once and reuse them across the panel.

**One home per fact.** Each story, system or number is told once, in the section where it does the most work (their systems and papers inside a question, dates and titles in the career table, a post in Public activity). Other sections may name it in a few words, never retell it. Before creating the doc, scan for any fact told twice and cut the extra copy.

**Hyperlinks everywhere.** Any named post, paper, blog post, talk, repository or product page anywhere in a doc is hyperlinked text pointing to the item itself, verified to open and to list the interviewer as author when that is claimed.

Every fact about a third party (a prior employer's filings, an executive's history, a deal) carries a source from research, or is left out. Target about 2,000-2,500 words per doc. Run one full-model authoring agent per interviewer in parallel, then review each doc against this section.

---

## Finish

The very last thing in the final message of a run is a plain fenced code block the user can paste straight into a chat, with nothing after it (any "Needed from you" lines go before it):

```
{Event or panel name} prep docs
Folder: {Drive folder URL}
- {Doc 1 title}: {URL}
- {Doc 2 title}: {URL}
```
