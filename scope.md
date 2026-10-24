# Scope sheet

Last updated: 24 October 2026

**How to read this file.** This trims the MVP in `architecturedoc.md` and `mvp/mvp-mockup-guide.md` to the thinnest version that still gives real proof. Confirmed by you on 6 October 2026: the MVP tests Guess 2, because Guess 3 depends on it.

---

## What the MVP does

**The assumption it tests (only one):** Guess 2, *the website works on its own*. A newcomer can follow the checklist to a working FNMT certificate without personal help, and feels less uncertain afterwards (`architecturedoc.md`, section 5).

Why this one: it is the only guess the product itself can prove. Guess 1 (a guidance problem exists) is better tested by recruiting and interviews, and Guess 3 (people will pay €15) only matters once Guess 2 holds. Trying to prove all three in one build would make the MVP fatter and the result muddier.

**One user:** a non-EU person working in Barcelona (changed from "applicant" on 13 October 2026, when the target moved to foreign workers) with a valid passport and TIE, using Chrome on a Windows computer or a Mac, reading English, who needs their first FNMT certificate now. Found and screened by you directly, not by the site.

**One job:** get from "I need a digital certificate" to "my certificate works in Chrome and is backed up".

**One screen:** a single checklist page.

- A short line at the top saying who this guide is for (the four eligibility conditions) and that the certificate and official instructions are free.
- The six stages in order: prepare your computer, request your certificate code, verify your identity in person, download the certificate, check it works, back it up safely.
- Each stage opens in place to show its plain-English instructions, the official source link with its "last checked" date, and a yellow "Needs official confirmation" box where FNMT pages are unclear (passport plus TIE, the Barcelona AEAT office, download timing, backing up on a Mac). Where Windows and Mac differ (installing Configurador, backing up), the stage shows one line for each, with its own official source.
- A tick box per stage, saved only in the user's browser, because the journey spans several days (the code, then an in-person appointment, then the download).
- The standing reminder: never type your NIE, passwords or codes on this site.

**The proof:** you watch or debrief each test user. For each one, record three things in your own notes:

1. Did they end with a working, backed-up certificate?
2. Where, if anywhere, did they need help beyond the page?
3. How sure did they feel before and after (a simple 1 to 5 question)?

**How this reconciles with the five-screen mockup guide.** The mockup is still useful for designing how things look. For the real test, the five screens fold into one:

| Mockup screen | In the one-screen MVP |
|---|---|
| Home page | Becomes the two lines at the top of the checklist |
| Eligibility check | Replaced by you screening each test user in person; the page just states who it is for |
| Your checklist | **This is the MVP** |
| Step page | Each step opens inside the checklist instead of on its own page |
| Troubleshooting | Dropped for now; problems are what the test should reveal |

---

## What it deliberately does NOT do

Features thrown away for now, by name:

- **Home / landing page.** No marketing page; test users arrive through you.
- **Eligibility questionnaire** and the "we don't cover you yet" screen. You screen users yourself.
- **Separate step pages.** Steps open inline.
- **Troubleshooting page.** Written later from real problems, not guessed ones.
- **"Join the pilot (€15)" button and any payment.** That is Guess 3, tested later.
- **Accounts, login and any database.** Progress lives in the browser only.
- **Dashboard or analytics.** Your notes from watching users are the measurement.
- **Email capture or waitlist.**
- **Chatbot or AI answers.**
- **Other languages.**
- **Other routes and profiles:** other browsers, EU citizens, idCAT, Cl@ve, other FNMT routes, other cities.
- **Appointment booking or help, and personal one-to-one support.**
- **Any integration with FNMT or government systems.**

**Rule for adding later:** we only add a feature if **at least two real test users get stuck at the same point, or ask for the same thing, and the one checklist page cannot fix it by changing its wording.**

Examples of how the rule applies:

| Feature | Added only if a real user… |
|---|---|
| Troubleshooting section | hits the same problem as another user, after the step text has already been rewritten once |
| Eligibility questionnaire | arrives from outside your own recruiting and turns out not to fit the profile |
| Payment | completes the journey, and Guess 2 holds for most test users, so Guess 3 is worth testing |
| A new profile (for example another browser) | asks for it, and you have checked that route against official sources |
| Accounts or cross-device progress | loses their progress between stages and has to start again |
