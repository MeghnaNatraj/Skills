# PITFALLS, interview-research

Past corrections. Read before generating and run as a final check before presenting. Add new ones at the bottom as Trigger, Wrong, Correct. Lasting rules are also stated once in SKILL.md.

## Only list the reported problems that match the interview
**Trigger:** Building "Reported questions & process" from a company problem catalogue.
**Wrong:** Citing all 97 problems in the catalogue as proof of coverage when 8 matched the interview.
**Correct:** Filter to the problems that match this interview's format and topic; do not mention or summarize the rest.

## A pay band inside a quoted JD is still comp
**Trigger:** Quoting a job description verbatim.
**Wrong:** Keeping "Salary range listed: ..." because it is part of the posting.
**Correct:** Drop the pay band line; quote responsibilities and requirements only.

## Aggregator interview sites mix up companies
**Trigger:** Pulling reported questions from interview-guide aggregator sites.
**Wrong:** Quoting their "verbatim" questions as firsthand.
**Correct:** Treat them as low confidence; many are AI-generated and blend same-named companies (an AI startup confused with a bootcamp and a scaffolding firm of the same name). Trust 1point3acres problem pages, Blind threads and curated banks; flag confidence inline.

## Prep docs read like research notes
**Trigger:** Writing an interviewer or company prep doc.
**Wrong:** "Confirm who the role reports to", "Whether your call is for this role is unconfirmed. Ask.", an "interview loop not discoverable" note, an intro explaining how questions were ranked.
**Correct:** The doc reads as facts and answers the candidate uses, with no confirm, ask or method asides. Answers lead with the bold question. The candidate is the one being interviewed, so questions to ask never probe reporting lines.

## Panel docs too thin on the interviewer
**Trigger:** Writing per-interviewer panel docs.
**Wrong:** A LinkedIn summary, one line of public activity, a career table, then generic questions to ask placed after the answers.
**Correct:** Use every LinkedIn activity tab for the last 12 months with dates and use those signals; explain what their programs and products actually are; say how they will probe and why; ground the hooks in them; put technical "Questions to ask" above "Questions they will ask".

## Panel doc layout
**Trigger:** Reviewing the first panel-mode docs.
**Wrong:** A 300-word biography paragraph at the top; public activity out of date order; numbered lists; a two-column career table with equal widths although the first column holds only years; career years as "2021 to 2026" bullets.
**Correct:** Section 1 is only the LinkedIn line, then "Why it matters". Every dated list newest first. Round bullets only. Career is a `| Years | Role |` table, years like "2019-2022", Years column 80 pt.

## Panel docs ran long
**Trigger:** First panel-mode test run.
**Wrong:** About 4,000 words per doc, mostly long Role and Company sections repeated in every doc.
**Correct:** Cap The Role at 6 bullets and The Company at 10, reuse them across the panel, target about 3,000 words.

## Things that look broken but are not
**Trigger:** Verifying a created Google Doc.
**Wrong:** Reporting `\!`, `\&` or a blank first table row as render failures after reading the doc with Drive `read_file_content`.
**Correct:** Those come from that export. Verify with Docs `read_doc` or a plain-text export.

## The markdown import makes dash bullets
**Trigger:** Creating a doc whose lists must be round bullets.
**Wrong:** Assuming `- ` lists render as round bullets.
**Correct:** They render with a dash glyph. Re-apply `BULLET_DISC_CIRCLE_SQUARE` with `createParagraphBullets` after creating.

## Empty LinkedIn detail pages
**Trigger:** Pulling an interviewer's education, languages or volunteering.
**Wrong:** Recording "not found" when a `/details/` page comes back empty.
**Correct:** Read the same section from the main profile page, or with JavaScript from the rendered page.

## Repairing a doc already created (fallback only)
**Trigger:** A doc must be fixed in place in Chrome instead of re-created.
**Wrong:** `navigator.clipboard.writeText` (reports success but leaves the clipboard empty on docs.google.com), synthetic Cmd+C, live-typing markdown, or pausing between copy and paste while a clipboard manager is running.
**Correct:** Clear the doc, copy with a hidden textarea and `document.execCommand('copy')`, then right-click and choose "Paste from Markdown" immediately, with no screenshot in between. Format-only: the word count must not drop.

## Public activity without links
**Trigger:** Writing the Public activity section of a panel doc.
**Wrong:** Dated bullets with no way to open the post.
**Correct:** Each bullet's date links to the item. LinkedIn's activity page has no post IDs in the page and blocks its internal API, and public posts are not indexed by web search, so capture each link with "Copy link to post" while `navigator.clipboard.writeText` is intercepted in the page (see SKILL.md panel mode, input 4).

## The same facts told in several sections
**Trigger:** Reviewing a finished panel doc (a past panel test).
**Wrong:** The interviewer's startup acquisition story told five times (Why it matters, career table, brush-up, twice in The Company); his job history repeated in Why it matters; a paper and an ML platform described in both the brush-up and The Company; one LinkedIn post used in activity, a rapport hook and a question; a probe list in "How they will interview" that duplicated "Questions they will ask"; a rapport hook citing an article that could not be verified.
**Correct:** One home per fact (SKILL.md panel mode). Why it matters gives takeaways only, "How they will interview" is one paragraph with no probe list, The Company leaves out anything in the brush-up, and hooks only cite verified items. Do not add a "why they ask this" line to each question.

## Speculative filler in "Why it matters"
**Trigger:** Trimming "Why it matters" in a panel doc.
**Wrong:** "Every system he owns ... turns raw member and job events into features, scores or allocations, which is exactly what a data coding problem simulates" and "He came to machine learning from production systems engineering, so he will judge code by whether it would survive real data at scale". The user called these wrong and unnecessary.
**Correct:** Keep only sourced facts (their seat, what the interview tests, concrete track record). No guesses about how the interviewer thinks or judges, no generic links between their work and the interview. One bullet is fine.

## Panel doc header and wording
**Trigger:** Reviewing the top of a panel doc.
**Wrong:** A "Why it matters" section; an H1 with the candidate's name; the word "round" anywhere; a file name with the full weekday.
**Correct:** H1 `{Interviewer}, {Company}`, then bold-label lines LinkedIn, Role, Topic, When (date with year, time range, time zone, length, place), What it tests. Say "interview" or "session", never "round". File name `{N}. Wed, April 3, 3:00pm: {Interviewer}, {Company}`. After rewriting the header in place, re-bold the "Public activity (LinkedIn):" label, which the edit can strip.

## Header spacing
**Trigger:** Laying out the panel doc header.
**Wrong:** A blank line after the title and between every label line.
**Correct:** Title, LinkedIn, Role, Topic and When sit together with no blank lines; one blank line, then What it tests, then one blank line before Public activity (the user's own edit of a past doc).

## Rapport hooks that read as desperate
**Trigger:** Writing rapport hooks.
**Wrong:** "Mentioning that you read the February 2017 launch post and noticed the 46-hour to 7.5-hour scaling result shows you prepared for him specifically"; "so a brief, genuine note about shipping something real for users lands well"; "which gives an honest reason to enjoy the probabilistic framing he will use"; "it is a genuine link to his current area".
**Correct:** One plain shared point per hook (their work, your parallel), with no advice on how it will come across and no signaling of preparation.

## Old awards and old projects
**Trigger:** Linking the candidate's background to an interviewer (rapport hooks, answers).
**Wrong:** Citing school competition results from a decade earlier and a six-year-old student project in a prep doc.
**Correct:** Use only the candidate's recent work. No school-era awards or rankings, no projects older than about five years. Drop a hook rather than reach back for one.

## Background sections the user cut
**Trigger:** Reviewing a panel doc (a past panel test).
**Wrong:** Separate "What their work means (brush up)", "How they will interview" and "Rapport hooks" sections.
**Correct:** Drop all three. Their useful facts (the interviewer's systems with dates and numbers, scale figures, known model choices, likely probes) go inside "Questions to ask" and the answers in "Questions they will ask".

## Header lines that read like citations
**Trigger:** Writing "What the interview tests".
**Wrong:** "machine learning fundamentals applied to Acme data: 2018 Glassdoor reports for this role cite logistic and linear regression and how a decision tree works, and the recruiter's March 12 prep email lists ..."
**Correct:** Plain English, one or two short sentences, no sources: "Machine learning basics. Expect to explain how logistic regression, decision trees, Bayesian models and neural networks work, and when you would use each."

## Unsourced framing in the header
**Trigger:** Writing "What the interview tests".
**Wrong:** "Machine learning basics, applied to Acme data." No source said the questions used Acme data; it was added to sound tailored.
**Correct:** State only what the sources say about the interview. If a qualifier has no source, leave it out.

## Questions to ask too technical and unlabeled
**Trigger:** Writing "Questions to ask" (a past panel test).
**Wrong:** Unlabeled bullets stacked with insider terms and multi-part questions: "How is vertical intent predicted today: a single multi-class model over all verticals, or per-vertical binary models, and how much does personalization ... contribute?"
**Correct:** Bold topic label first, then a plain question: "**Search intent:** When someone types a query, how does search decide whether they want people, jobs or companies, and how much does their own profile help?"

## Vague Themes with unlinked sources
**Trigger:** Closing the Public activity section when the interviewer barely posts (a past panel test).
**Wrong:** "His public record lives off LinkedIn, in the company engineering blog posts he co-authored (2016 and 2017) and two open-source GitHub repositories. Use those as the hooks; he is a builder who documents systems rather than posting." No links, a missed 2015 post, hook advice and a personality guess.
**Correct:** "He rarely posts on LinkedIn. His public work is on his former employer's engineering blog and GitHub. He co-wrote [Post one](url) (September 2015), [Post two](url) (February 2016) and [Post three](url) (February 2017). The code for both projects is public: [Project A](url) and [Project B](url)." Every named item is hyperlinked text, checked for authorship.
