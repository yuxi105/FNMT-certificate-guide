# Architecture document

Last updated: 24 October 2026

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

---

## 3. Stack and why

Chosen in Session 4 for the MVP. It fits the constraints: a solo founder with beginner-to-intermediate Python, limited weekly time, and no complex coding project. **[Decided]**

**Stack**

- **One static HTML file** (plain HTML, CSS and a little JavaScript, no framework, no server): the one-page checklist. It opens by double-clicking, is free to host later, and is easy for AI to generate and for me to read.
- **Step content in one clearly marked block inside that file**, so content can be updated after re-checking sources without touching the logic. A separate data file was tried and dropped, because the page broke when the file was missing.
- **Progress saved in the user's own browser** (localStorage), so no accounts or database are needed.
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

