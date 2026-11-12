---
page-id: signing-up
route: /licensing
title: Signing Up
audience: external
status: draft
version: 0.6.0
last-reviewed: 2026-10-14
blocked-reason: "2026-10-14 (later) – All screenshots regenerated from the PM's updated deck showing the new Auth0 branding (L+ logo, midnight-blue background, teal buttons). Slideshow updated to the deck's 19 slides (verify-code and You've Been Invited slides dropped by the PM). verify-email.png no longer shown (not recaptured with new branding) – file left in place, delete on PM OK. Superseded note: 2026-10-14 – GIF replaced with a click-through slideshow (pages/walkthroughs/signup/index.html, iframe; Docsify here doesn't run inline scripts). Slides are a 1920x1080 PowerPoint export of OneDrive 'login gif.pptx' (slides/01–21.jpg) – to update a step, replace that JPG. iframe src is relative to index.html, so it works under both lewiswoolcott.github.io/ and logicplus.smartcoro.com/docs/. Slides 6–9 and 19 and four Auth0 screenshots below predate the 2026-10-12 Auth0 branding (49ff0c419) – awaiting fresh captures. 2026-09-30 – Added the PM's sign-up/user-management walkthrough GIF (images/signing-up/signup-walkthrough.gif, 21 slides, source: OneDrive 'login gif.pptx') and replaced every screenshot with the PM's 2026-09-29 production captures (emails are made-up demo data, no redaction needed). Steps rewritten to match: account creation now uses an emailed verification *code* (not a link) and an Authorize App step; Microsoft path shows a permissions prompt; checkout collects seats, organisation name, tenant name, name, billing address. Old images/signing-up/join-workspace.png is no longer referenced (not deleted – awaiting PM OK). 2026-09-30 – Trials reworked (bb3c146af, v1.2.15–17): the standalone "Free trial" plan card is gone; Solo and Team cards each carry a "Free trial licence" button (LicensePricing.tsx), Enterprise has none in the UI despite the commit title. One Solo + one shared Team/Enterprise trial per purchaser, 30 days card-free, org trials default to 5 seats, purchase during trial bills from trial end (catalog/USE_CASES.md, billingPolicy.ts TRIAL_DAYS). Screenshots choose-licence.png now out of date – still show the old Free trial card; retake. GO-LIVE CONFIRMED 2026-09-25 – production Stripe/Auth0 pipeline is live (verified against __infra/tofu/prod/stripe/*.tf and orchestrator.tf, a real cutover from the prior manual/sandbox setup). Dropped the prior PENDING GO-LIVE flag. Still to do before this leaves draft: the checkout screenshot (images/signing-up/start-checkout.png) is still from a pre-launch Stripe Sandbox test run – redacted of personal data, but shows a visible Sandbox badge and doesn't yet reflect the real annual-billing/GST line items added below; swap for a real production screenshot when convenient. The sign-in screenshot (images/signing-up/join-workspace.png) IS a genuine current production screenshot (2026-09-25). Annual billing and GST-inclusive pricing confirmed live via frontend/src/page/LicensePage/LicensePricing.tsx (not yet independently screenshot-verified). Steps verified against frontend/src/page/LicensePage/LicensePage.tsx (route /licensing) – this same page also serves the Subscription & Billing page's authenticated view, see pages/subscription-billing.md."
---

## Overview

Logic+ Online has three licences – **Solo**, **Team**, and **Enterprise** – covering everyone from an individual to a whole organisation with multiple licensed seats. Solo and Team can both be tried free for 30 days. Sign up for any of them online.

This is separate from being invited to an existing organisation's workspace, where an admin invites you by email and no signup is needed – see [Getting Started](guides/getting-started.md) for that path. See [Subscription & Billing](pages/subscription-billing.md) for how to manage seats and user licences once you're on a Team or Enterprise plan.

**Prefer to watch?** This short walkthrough covers signing up, then inviting your team. It plays automatically – use ◀ ▶, the dots, or click the slide to step through at your own pace:

<iframe src="pages/walkthroughs/signup/index.html" title="Sign up, subscription and user management walkthrough" style="width:100%;max-width:960px;aspect-ratio:16/10;border:1px solid #d9e0e6;border-radius:8px" loading="lazy"></iframe>

## Steps

### 1. Choose a licence

Go to **https://logicplus.smartcoro.com/licensing** and choose a licence type – **Free trial licence** to try Solo or Team, or **Start Solo** / **Start Team** / **Start Enterprise** to buy.

![Choose the right licence for you](images/signing-up/choose-licence.png)

| Plan | Monthly price | Notes |
|---|---|---|
| Solo | $150 AUD/month | One named user, private personal workspace, automatic monthly renewal, self-service billing |
| Team | $250 AUD/seat/month | Shared organisation workspace, flexible licensed seats, email invitations, organisation membership |
| Enterprise | $350 AUD/seat/month | Everything in Team, plus enterprise organisation access, company email domain association, multiple organisation workspaces |

**Free trials.** The Solo and Team cards each have a **Free trial licence** button: 30 days, no card required, and the trial ends automatically – you're only charged if you choose to buy. You can have one Solo trial and one Team trial. A Team trial is for your organisation and starts with five seats. If you buy a plan during its trial, billing starts when the trial ends, not straight away. Enterprise doesn't have a self-service trial – contact **support@lware.com** if you'd like to evaluate it.

A **Pay annually** toggle switches every paid plan to annual billing at a 15% discount (billed as one annual charge rather than monthly). Every price shown already **includes GST** – there's no tax added on top at checkout, and each plan card shows exactly how much of its price is GST.

### 2. Create an account, or sign in with Microsoft

On the **Welcome to Logic+** screen, choose **Create account**, or **Sign in with Microsoft** if you have a Microsoft work or school account. (Already have a Logic+ account? Choose **Log in**.)

![Welcome to Logic+ – log in, create an account, or sign in with Microsoft](images/signing-up/welcome.png)

**Creating a new account:**

1. Enter your email address and click **Continue**.
2. Choose a password and click **Continue**. It needs at least 8 characters, including at least three of: lower-case letters, upper-case letters, numbers and special characters.
3. Check your email for a verification code, enter it, and click **Continue**. (No email? Click **Resend**.)
4. When asked to authorise the Logic+ application, click **Accept**.

![Create your account – enter your email](images/signing-up/create-account-email.png ':size=320')
![Create your account – set your password](images/signing-up/enter-password.png ':size=320')
![Authorise the Logic+ application](images/signing-up/authorise-app.png ':size=320')

**Signing in with Microsoft:**

1. Enter your Microsoft email address and click **Next**.
2. Review the permissions Logic+ is asking for, and click **Accept**.

![Microsoft sign-in](images/signing-up/microsoft-sign-in.png ':size=360')
![Microsoft permissions requested](images/signing-up/microsoft-permissions.png ':size=360')

### 3. Choose your licence again and check out

Once you're signed in, the **Your licences** page asks for your licence type again – choose the same plan.

![Your licences – choose your plan](images/signing-up/your-licences.png)

At checkout:

1. For Team or Enterprise, set the number of **seats** (a Team trial includes up to five).
2. Enter your **organisation name**, a **tenant name** (the short name shown in the workspace selector at the top of Logic+), your **name** and your **billing address**.
3. Click **Start trial** (or the equivalent button for a paid plan). For a trial, the total due today is $0.

![Checkout – seats, organisation, tenant and billing details](images/signing-up/start-checkout.png)

### 4. You're in

After checkout you're logged in to Logic+ Online automatically, in your new organisation's workspace. For Team or Enterprise, you're its **Owner**. **Save the link** so you can get back in later.

![Logged in to your organisation's workspace](images/signing-up/logged-in.png)

Next steps:

- Check you're in the right workspace before creating projects – see [Choosing a workspace](guides/getting-started.md#choosing-a-workspace).
- Invite your team – see [Managing seats](pages/subscription-billing.md#managing-seats).

## If you run into trouble

Contact **support@lware.com** with any issues, questions, or feedback.
