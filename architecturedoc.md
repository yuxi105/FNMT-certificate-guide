# Architecture document

Last updated: 7 November 2026

**How to read this file.** Items marked **[Decided]** come from decisions already recorded in the project (mainly Handout 3 and the 4 October handover). Items marked **[Draft — confirm]** are suggestions written for this document that you have not yet approved. Nothing here means code has been written, a prototype exists or the idea is validated.

---

## 1. Venture

**Name:** [Name TBD]. "Spain Digital ID Navigator" was an early working name in a Session 1 draft; it is not a chosen brand.

**Who it is for:** Foreigners working in Spain, EU and non-EU (employees, autónomos, first-time tax filers). Changed on 13 October 2026 from non-EU students; see the session log for why. The page itself still covers only one route: non-EU, passport and TIE, Barcelona. **[Decided]**

**What it does:** A paid interactive website that guides foreigners working in Spain, step by step, through getting their first FNMT digital certificate (software route, in-person identity verification), from preparing their computer to checking the certificate works and backing it up. **[Decided]**

---

## 2. Architectural procedure

Rules this project follows:

1. **No sensitive data, ever.** The website never asks for, stores or transmits identity documents, NIE/passport numbers, passwords, request codes or certificate/private-key files. Users enter these only on official FNMT pages. **[Decided]**
2. **Every procedural instruction has an official source and a check date.** Each step shown to users links to its official source (for example the S1–S11 FNMT pages) with the date it was last checked. Where official wording is unclear or contradictory, the website says so and tells the user what to confirm, rather than guessing. **[Decided]**
3. **Content stays visible and checkable.** All step text, sources and check dates sit in one readable block, not scattered through the code, so each claim can be checked by hand against the sources. (The earlier plan of a questionnaire with if-then rules is not in the MVP; I screen test users myself.) **[Decided]**
4. **Build the thinnest slice for one profile first.** Only one supported path until it is tested: non-EU applicant, valid passport and TIE, Chrome (Windows or Mac), Barcelona, clear English. Anyone outside that profile is told the site does not cover them yet. New profiles are added only when test evidence justifies them. **[Decided]**
5. **AI drafts, I review and own.** AI may write code, content and tests, but I check every procedural claim against official sources and test the prototype's behaviour before any user sees it. Responsibility for approved work stays with me. **[Decided]**
6. **The page learns only through a human gate.** User stuck reports can change what the page shows, but no fix reaches users until I have checked it against an official source and recorded who approved it and when. The code hides any tip without an approver, a source and a check date. **[Decided]** (1 November 2026)

---

## 3. Stack and why

Chosen in Session 4 for the MVP. It fits the constraints: a solo founder with beginner-to-intermediate Python, limited weekly time, and no complex coding project. **[Decided]**

**Stack**

- **One static HTML file** (plain HTML, CSS and a little JavaScript, no framework, no server): the one-page checklist. It opens by double-clicking, is free to host later, and is easy for AI to generate and for me to read.
- **Step content in one clearly marked block inside that file**, so content can be updated after re-checking sources without touching the logic. A separate data file was tried and dropped, because the page broke when the file was missing.
- **Progress saved in the user's own browser** (localStorage), so no accounts or database are needed.
- **Learning loop without a backend (added 1 November 2026):** each stage has an "I'm stuck here" button with multiple-choice questions only (problem, document, computer). The page shows a checked fix if one exists and builds a one-line report the user can copy and send. A separate owner-only page, `review.html`, counts and ranks pasted reports and turns an approved fix into a line for the TIPS list in `index.html`. No model training, no server and no analytics. Since 7 November, `review.html` blocks any fix that mentions a website outside an official list (fnmt.gob.es, agenciatributaria.gob.es, firmaelectronica.gob.es), even when both safety boxes are ticked.
- **Later, only if Guess 3 is tested: payment through a hosted checkout link** from a payment provider, so the site never handles card details.

**Dependencies to avoid, and why**

| Avoid | Why |
|---|---|
| A chatbot or LLM inside the product | Not needed to deliver a fixed checklist; adds cost, unpredictable answers and the risk of confidently wrong procedural advice. **[Decided]** |
| Integration with government systems (FNMT, Cl@ve, appointment systems) | Out of scope, fragile and would bring us close to handling credentials. **[Decided]** |
| User accounts, login and a backend database | Would create personal data to protect and more to build and maintain, with no benefit for a one-off journey. **[Decided]** for the first prototype |
| A mobile app | A browser website is enough; app stores add build and review work. **[Decided]** for the first prototype |
| Heavy frameworks and AI orchestration tools (for example LangChain) | Hide problems, make debugging harder and are unnecessary for a questionnaire plus pages. **[Draft — confirm]** |
| Third-party tracking and advertising scripts | Users are in a sensitive administrative situation; trackers undermine trust and raise privacy obligations. **[Draft — confirm]** |
| Scraping or copying official pages into the site | Copies go stale silently. Link to the official page and summarise with a check date instead. **[Draft — confirm]** |

---

## 4. Tradeoffs consciously accepted

What the project does badly on purpose:

- **Covers almost nobody at first.** Only non-EU applicants with passport and TIE, using Chrome on Windows or Mac, in Barcelona, in English. idCAT, Cl@ve and other FNMT routes are left out. **[Decided]**
- **Cannot fix appointment shortages.** Several interviewees' real obstacle was getting an appointment; the website cannot solve this and will not promise to. **[Decided]**
- **No personal help.** No one-to-one support, legal advice, immigration support or appointment booking, even though interviewees valued people who explained things. **[Decided]**
- **Progress is fragile.** Saving progress in the browser means it is lost if the user clears their browser or switches device. Accepted to avoid accounts and personal data. **[Draft — confirm]**
- **Content is maintained by hand.** Official pages change, and each update needs a manual re-check. Accepted because accuracy matters more than automation at this stage. **[Draft — confirm]**
- **Charges for something that is free.** The certificate and official instructions cost nothing; the product competes with free help (official pages, ISA, friends, ChatGPT) and must say so clearly. **[Decided]**

---

## 5. Open assumptions

What is guessed and not yet proven:

1. **A guidance problem exists.** Foreigners working in Spain who need an FNMT certificate struggle to understand and follow the process, and free help does not solve it. None of the five interviews showed a first-time FNMT setup failure. Online research (10 posts) planned for 24 and 25 October. *(Guess 1, not tested yet)*
2. **The website works on its own.** Users can follow the checklist correctly without personal help and feel less uncertain afterwards. *(Guess 2, not tested)*
3. **People will pay.** Eligible users will pay around €15 despite free alternatives. The €15 is an experimental test price, not a market price. *(Guess 3, not tested)*
4. **Passport plus TIE is accepted** at in-person verification. The official page does not name this exact combination. *(Needs official confirmation)*
5. **The Barcelona AEAT office** (Plaza Doctor Letamendi) can be used by this applicant. Only its listing on the FNMT office map was seen. *(Needs confirmation)*
6. **Download timing.** Official pages disagree (about one hour versus immediately). *(Unresolved)*
7. **Foreigners working in Spain are the right first group** (replaces "non-EU Esade students", changed 13 October). They need the certificate now for taxes and Social Security; students mostly don't. Based on reasoning plus one anecdote so far. *(Not tested)*
8. **A static site with browser-saved progress is enough** for the prototype. *(Draft assumption from this document)*
9. **certificadoelectronico.es leaves a gap.** Checked 24 October: it accepts the TIE (so it competes on my non-EU route) but not the NIE; passports and the EU registration certificate are not mentioned. *(Partly checked)*
10. **People will send stuck reports.** The loop only learns if users copy and send their report by hand. Nobody has done it yet. *(Not tested)*
11. **The ID-check gap may be closing.** Barcelona City Council's page says FNMT now offers video identification for €2.99 + VAT, and a news article says ISTEC accepts NIE + passport by video. Neither is checked on the official provider pages yet. *(Found 31 October, not checked)*

---

## 6. Session log

### 6 October 2026

- Created this architecture document (`architecturedoc.md`) with six sections: venture, architectural procedure, stack, tradeoffs, open assumptions and session log.
- Kept the venture name as **[Name TBD]**; "Spain Digital ID Navigator" recorded only as an earlier working name.
- Carried over existing decisions (scope, no sensitive data, official sources with dates, no chatbot or government integration, human review of AI work) into the rules and tradeoffs.
- Proposed a first stack (static site, rules in a data file, browser-saved progress, hosted payment link) and three extra dependencies to avoid, all marked **[Draft — confirm]**. These are suggestions, not yet your decisions.
- Open follow-up: approve or edit the draft items; this also helps fix the Handout 3 coding-plan inconsistency, which still says the build will be chosen later.

### 6 October 2026 (MVP mockup)

- Started the MVP as a clickable mockup, not a working product: design it in Claude Design, then hand it to Claude Code to turn into a static site.
- Chose five mockup screens for the single supported profile: home page (with honest "the certificate is free" note and a placeholder €15 pilot button), four-question eligibility check with an out-of-scope screen, six-stage checklist with progress, a step page with official source, check date and "needs official confirmation" box, and a troubleshooting page.
- Left out of the mockup: real payments, accounts, chatbot, other languages and other routes (Mac, idCAT, Cl@ve).
- Mockup instruction text is placeholder, labelled "Draft, not yet checked" until re-checked against S1–S10.
- Guide and ready-to-paste prompts: `mvp/mvp-mockup-guide.md`.

### 6 October 2026 (Scope sheet)

- Wrote `scope.md`: the MVP is trimmed to one checklist page for one user and one job, testing only Guess 2 (the website works on its own). Home, eligibility, step and troubleshooting screens fold into that page or are dropped; payment, accounts and dashboards stay out. Rule for adding later: only when at least two real test users get stuck at, or ask for, the same thing and rewording the page cannot fix it. **[Draft — confirm]**

### 6 October 2026 (UX tweaks and single file)

- Added a progress bar, a "Next" marker that opens the next unfinished stage on return, and an "I've finished this stage" button. No new screens or features, so the scope sheet is unchanged.
- Merged `steps.js` into `index.html` because the page broke when opened without `steps.js` beside it. The checklist content now sits in one clearly marked block at the top of the script, so it can still be checked and edited by hand.

### 6 October 2026 (Windows or Mac)

- Widened the supported setup from Windows only to Windows or Mac, because two of the three test users from my interviews use a Mac. This follows the scope rule: real users need it, and the Mac route was checked against official FNMT pages (the Configurador page supports macOS; FNMT has a Mac Keychain export guide), both checked 6 October 2026.
- Only two stages change: installing Configurador (Mac security and Rosetta prompts) and backing up (Keychain Access instead of Chrome on Windows). A "Needs official confirmation" box was added because the Mac export guide does not mention Chrome.

### 12 October 2026 (Session 5: defense)

- Copycat test: GPT-6 Astra rebuilt the MVP, including most of its content, in 7 minutes 10 seconds. The code and the public FNMT content are not a defense.
- Chosen defense: **distribution through Esade's International Student Advisors (ISA)**, who already help new students with non-academic paperwork. Esade's International Students Handbook 2026-2027 does not mention the digital certificate.
- First step: email the ISA team to ask whether students ask about the digital certificate and whether they would share the checklist once it has been tested. Draft in `handout5/section4-defense-design.md`.
- No change to the stack or the page for this step. Because the page has no analytics, referrals are counted by asking the ISA and new users, not by tracking.
- New open assumption: the ISA is willing to share an independent student-built guide. Competitor to check: certificadoelectronico.es (own certificate, €9.95 plus VAT, video identification, no office visit); unconfirmed whether it serves non-EU users with an NIE, passport and TIE.

### 13 October 2026 (Session 6: hackathon)

- No building or research happened in class: the session became content plus a short elevator pitch from each team. The MVP page and stack are unchanged.
- Target user changed from non-EU students to **foreigners working in Spain, EU and non-EU** (employees, autónomos, first-time tax filers), after the guest teacher described their own trouble with the FNMT certificate. One anecdote, not proof. **[Decided]**
- The MVP still covers only the non-EU passport + TIE route (rule 4). The EU route (EU registration certificate with NIE plus national ID or passport) is not checked against FNMT sources yet and is added only after checking and finding real EU users who get stuck.
- Assumption 7 (non-EU Esade students are a useful first group) is weakened: students don't need the certificate now. Esade's ISA is a weaker channel for working people; employers' HR teams, gestorías and expat work communities are candidates to check.
- Next step (carried over from the hackathon plan): count Reddit and expat-forum complaints from foreign workers, and check whether certificadoelectronico.es accepts foreigners with an NIE.

### 24 October 2026 (midterm feedback and Summary Document)

- Midterm feedback: the architecture doc I uploaded on 13 October was an older copy (last updated 6 October). This is the correct, current file, with the 13 October entry above and today's entry.
- Why the target changed to foreign workers (13 October), stated properly:
  - My own evidence pointed away from students: none of my five interviews showed a first-time setup failure, and none of the three test students needs the certificate now, so they had no reason to finish the test.
  - Working people need the certificate for tasks they can't avoid: filing their tax return, Social Security procedures and registering as autónomos. This applies to EU workers too.
  - The guest teacher's own trouble with the certificate was the trigger. One anecdote, not proof.
  - So the switch is a better-reasoned guess, not a proven one. The research and the worker test below are how I check it.
- Research (Guess 1), 24 and 25 October, with AI help: 21 posts from foreigners stuck on the FNMT certificate, 11 from expat forums (Claude) and 10 from Reddit (ChatGPT, since Reddit blocks Claude). Stuck at: ID check or appointment 6, using it 5, requesting the code 5, computer setup 3, backup 2. Forum posts are old and mostly device problems; Reddit posts are recent (5 in the last 12 months, 6 mention work or taxes) and mostly NIE holders who can't use the video route (needs a DNI) and can't get an appointment. certificadoelectronico.es accepts the TIE but not the NIE (FAQ), so NIE-only and EU users are the gap. Guess 1 is stronger. Possible page change (not made yet): warn NIE holders early and mention walk-ins. Sources: `research-report.md`, `reddit-posts.md`.
- Test with foreign workers (Guess 2), 25 October: contacted 9 foreign people working in Spain (LinkedIn in English and Chinese, email to professors I know); 2 replied, nobody tried the checklist (both already have the certificate). A foreign professor got it twice through lawyers and then an accountant and was glad not to do it. A foreign assistant teacher did it alone but found it "very confusing" and used ChatGPT for the renewal. So the need is real for workers, but they turn to professionals or free AI chat, not a guide; Guess 3 (€15 for a guide) looks weaker. Next: reach users through employers' HR, gestorías or expat work groups.
- Code and documents now in a repository: https://github.com/yuxi105/FNMT-certificate-guide. No commit was made at the end of class on 13 October.
- No change to the MVP page or the stack.

### 1 November 2026 (Session 7: products that learn)

- Competitor map: the strongest competitor is free AI chat (ChatGPT). 2 of the 5 foreign workers who replied used it, 1 paid professionals, and nobody used a guide. My advantage against it is trust (steps checked against official pages, with Barcelona-specific details), shown by the Handout 5 copycat test, which missed every checked detail. Not yet shown with real users.
- New risk: FNMT video identification (€2.99 + VAT) and ISTEC (NIE + passport by video) may shrink the ID-check problem. Added as assumption 11.
- User data: still no page usage data and no tracking. The three behaviors come from what people told me: stuck at the ID check (explicit), handing the hard part to a professional or AI chat (explicit), and praising the page without using it (implicit). The loop is built around **the stage where a user gets stuck**.
- Built the learning loop with Claude Code, in `mvp/site/index.html` and the new `mvp/site/review.html`:
  - Input: "I'm stuck here" in each stage, with multiple-choice answers only (problem, document, computer), and "This helped" or "Still stuck" after a fix is shown.
  - Output: a checked fix for that stage and problem if one exists, plus a "Where others got stuck here" box in the stage, ranked by how many reports asked for each fix.
  - Learning: I paste received reports into `review.html`, which ranks stages and problems and shows the documents, computers and "helped" answers for each.
  - Human safety check: before a fix goes on the page, I check it against an official source and tick two boxes (it matches the source; no personal data, paid links or requests to share codes or files). `review.html` also flags risky wording, and the page hides any tip without an approver, source and date. Cost: about 5 minutes per report and 20 to 30 minutes per new fix, €0.
  - Seeded with two fixes already checked for the page (download timing, Configurador blocked by antivirus) and one hidden draft (FNMT video identification) that waits for checking.
- Uploaded to GitHub on 1 November (`index.html`, new `review.html`, this document). Not done yet: no address for receiving reports (`REPORT_EMAIL` is empty, so users can only copy the report) and no analytics.
- New rule 6 (human gate) and assumption 10 (people will send reports).
- Safety and ethics (Deliverable 5): added a plain-language "How this page learns" note and a "Delete my saved reports" link to the page. Sharing is off by default and reports contain only multiple-choice answers. Bias risk: ranking by report count favours the majority (TIE, Windows, English), so `review.html` splits every problem by document and computer, and each group's "helped" rate is checked. Red line: the loop never publishes a fix only because many people reported it; a person checks every fix against an official source first (rule 6).

### 7 November 2026 (Session 8: workflow and evals)

- Workflow map (5 steps, no AI model): 1 follow the stage (fixed rule: my checked text), 2 say what's wrong (user, multiple choice), 3 find a checked fix (fixed rule: TIPS list), 4 report back (user), 5 learn and publish (me, `review.html`). Step I trusted least: step 3, because only 2 of 11 specific problems have an approved fix.
- Test set: 15 cases in `handout8/FNMT-guide-error-analysis.xlsx`. 10 normal (7 based on real posts from the October research) and 5 attacks aimed where text actually enters the system: messages sent with reports, a forged report line, an EU user, 25 fake reports (Red Line), and a shared computer. Each case has "what a good answer must do" (drafted with AI).
- Run 1 (page as it was): 6 of 10 normal cases OK, 2 of 5 attacks handled. Errors per step: 1: 4, 2: 1, 3: 1, 4: 0, 5: 1. The weak step was step 1, not step 3: the page never said where it stops (which surname to type, moving to a new computer, using the certificate after stage 5, EU users). Security findings: `review.html` let a fix with a fake FNMT look-alike domain through without a warning; the unchecked video ID draft is readable in the page source.
- Fix (step 1 only): header now says who the page is not checked for (EU citizens as the example) and links the official FNMT route and NIE pages, and says where the guide stops. Stage 2 has a "Which surname to type" box (needs official confirmation). Stage 5 says the guide ends there and points to the official Autofirma page. Stage 6 explains moving to a new computer with the official FNMT import pages. New sources S12 to S15.
- Guardrail (worst attack, case 12): `review.html` blocks "Make the tip" when a fix mentions any website that is not on the official list, including addresses without https:// and look-alikes such as fnmt.gob.es.fake-site.com.
- Run 2 (same 15 cases): 9 of 10 normal, 4 of 5 attacks, step 1 errors 4 to 0. Nothing that worked before broke (cases 2, 5, 6 and 7 gave identical output; a fix with no outside website can still be made).
- Still open: case 4 (no stuck choice for "the send button does nothing", a step 2 problem left for the next round), case 15 (draft tips sit in the public page code; move drafts out of `index.html`), and `review.html` can't tell 25 copied reports from 25 people.
- To do before uploading to GitHub (rule 5): I open the four new sources (S12 to S15) myself and confirm the new lines match them. The check dates were set from an AI fetch of the pages on 7 November; the two import pages were found by title, not read in full.

### 15 November 2026 (Session 9: AI judge and product-market fit)

- My judging rules (Deliverable 1), written from my own PASS/FAIL and reasons on the 15 Class 8 cases (11 PASS, 4 FAIL: cases 4, 5, 6, 15). A good answer must: 1 give the user a specific next step for their exact problem that gets them unstuck (an office, a map or "check with the office" without an answer does not count); 2 use only official government sources checked by me; 3 be honest about who and what the site covers, and give users close to the covered route (TIE in process, passport plus visa) an officially confirmed answer; 4 show nothing about one user to anyone else and keep unchecked draft tips out of the page and its code; 5 never guess. Red Line unchanged. Rules are worded so they also work when the site covers more Spanish digital identity tools.
- Judge prompt (Deliverable 2): `handout9/judgeprompt.md`, with three judged examples (case 7 PASS, case 4 FAIL, case 12 PASS). Versions kept as `judgeprompt-v1.md` to `judgeprompt-v3.md`. The judge ran in a fresh ChatGPT chat per case on the other 12 cases, with the same inputs every round (`handout9/round1-inputs.md`).
- Judge check (Deliverable 3): failures caught 1, 2, then 3 of 3; passes agreed 8 of 9 in all rounds. Round 2 made rule 1 stricter; round 3 extended rule 3. The judge's remaining disagreement is case 3 (the surname box marked "needs official confirmation"), which it fails every time. My Class 7 metrics (fix success rate, fix coverage) need real stuck reports, so the judge cannot measure them, and I have no reports yet. I don't trust the judge to check the page every week yet: 12 cases with 3 failures is too few.
- Users (Deliverable 4): survey (Sean Ellis question plus 3 open questions and 2 to mark who answered) sent on 14 November to my October contacts and a LinkedIn contact, and posted in one Facebook group and two subreddits. All three posts were removed by moderators. 1 answer: an EU teacher working in Barcelona, a personal contact, "Somewhat disappointed" (0% very disappointed). She got the certificate alone with the official FNMT site and found it "very straightforward". Place on the scale: below light. Two other working foreigners pay someone else to do it.
- Decisions: the target user stays foreigners working in Spain, but I have no evidence yet that any of them needs the guide. Next round should reach first-timers with a deadline (her guess: people who don't speak Spanish or Catalan, applying for the first time) through a channel that trusts me, not open groups.
- To check before adding to the page: her tip to keep the password created at application, because it is needed to download the certificate. The page doesn't mention it; I confirm it on the official FNMT page first (rule 6).
- AI grader (ChatGPT): Clear rules 4, Checked judge 4, Honest about users 5. Its missing rule: separate whether a fallback is safe from whether it solves the problem, and treat "needs official confirmation" as unverified advice.
