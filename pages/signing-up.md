---
page-id: signing-up
route: /licensing
title: Signing Up
audience: external
status: draft
version: 0.7.0
last-reviewed: 2026-10-28
blocked-reason: "2026-10-28 – Sign-up flow rewritten for PR #532 (improve sign-up flow) and PR #544 (native Auth0 trial): plans and pricing now live on the public website (VITE_WEBSITE_PLANS_URL) – the in-app pricing/plan cards, Solo trial, seat picker and checkout organisation/tenant questions for trials are gone; trials are Team-only (5 seats, 30 days, no card, no checkout) created automatically at /start-trial with the workspace named from the email domain; paid purchases go through /buy (Pay monthly / Pay annually) into Stripe Checkout. Verified against BuyPage.tsx, TrialSignupPage.tsx, SelectAccount.tsx, LoginForm.tsx, VerifyEmail.tsx, LicensePage.tsx, CheckoutService.ts and the catalog/frontend READMEs. NOT verified: current prices on the website (price column removed from this page), whether Solo can still be trialled from the website, and the Auth0-hosted screenshots (kept, not recaptured). The embedded walkthrough slideshow and the choose-licence / welcome / your-licences / logged-in screenshots show the previous flow, so they are removed or commented out pending a PM refresh. Earlier: 2026-10-14 (later) – All screenshots regenerated from the PM's updated deck showing the new Auth0 branding (L+ logo, midnight-blue background, teal buttons). Slideshow updated to the deck's 19 slides (verify-code and You've Been Invited slides dropped by the PM). verify-email.png no longer shown (not recaptured with new branding) – file left in place, delete on PM OK. Superseded note: 2026-10-14 – GIF replaced with a click-through slideshow (pages/walkthroughs/signup/index.html, iframe; Docsify here doesn't run inline scripts). Slides are a 1920x1080 PowerPoint export of OneDrive 'login gif.pptx' (slides/01–21.jpg) – to update a step, replace that JPG. iframe src is relative to index.html, so it works under both lewiswoolcott.github.io/ and logicplus.smartcoro.com/docs/. Slides 6–9 and 19 and four Auth0 screenshots below predate the 2026-10-12 Auth0 branding (49ff0c419) – awaiting fresh captures. 2026-09-30 – Added the PM's sign-up/user-management walkthrough GIF (images/signing-up/signup-walkthrough.gif, 21 slides, source: OneDrive 'login gif.pptx') and replaced every screenshot with the PM's 2026-09-29 production captures (emails are made-up demo data, no redaction needed). Steps rewritten to match: account creation now uses an emailed verification *code* (not a link) and an Authorize App step; Microsoft path shows a permissions prompt; checkout collects seats, organisation name, tenant name, name, billing address. Old images/signing-up/join-workspace.png is no longer referenced (not deleted – awaiting PM OK). 2026-09-30 – Trials reworked (bb3c146af, v1.2.15–17): the standalone "Free trial" plan card is gone; Solo and Team cards each carry a "Free trial licence" button (LicensePricing.tsx), Enterprise has none in the UI despite the commit title. One Solo + one shared Team/Enterprise trial per purchaser, 30 days card-free, org trials default to 5 seats, purchase during trial bills from trial end (catalog/USE_CASES.md, billingPolicy.ts TRIAL_DAYS). Screenshots choose-licence.png now out of date – still show the old Free trial card; retake. GO-LIVE CONFIRMED 2026-09-25 – production Stripe/Auth0 pipeline is live (verified against __infra/tofu/prod/stripe/*.tf and orchestrator.tf, a real cutover from the prior manual/sandbox setup). Dropped the prior PENDING GO-LIVE flag. Still to do before this leaves draft: the checkout screenshot (images/signing-up/start-checkout.png) is still from a pre-launch Stripe Sandbox test run – redacted of personal data, but shows a visible Sandbox badge and doesn't yet reflect the real annual-billing/GST line items added below; swap for a real production screenshot when convenient. The sign-in screenshot (images/signing-up/join-workspace.png) IS a genuine current production screenshot (2026-09-25). Annual billing and GST-inclusive pricing confirmed live via frontend/src/page/LicensePage/LicensePricing.tsx (not yet independently screenshot-verified). Steps verified against frontend/src/page/LicensePage/LicensePage.tsx (route /licensing) – this same page also serves the Subscription & Billing page's authenticated view, see pages/subscription-billing.md."
---

## Overview

Logic+ Online has three licences – **Solo**, **Team**, and **Enterprise** – covering everyone from an individual to a whole organisation with multiple licensed seats. You can try Logic+ free for 30 days with a **Team trial**, or buy a plan online.

Plans and current pricing are shown on the Logic+ website – choose **Free trial** or **Buy** there and you're brought into Logic+ to finish signing up.

This is separate from being invited to an existing organisation's workspace, where an admin invites you by email and no signup is needed – see [Getting Started](guides/getting-started.md) for that path. See [Subscription & Billing](pages/subscription-billing.md) for how to manage seats and user licences once you're on a Team or Enterprise plan.

<!-- Walkthrough slideshow hidden 2026-10-28: it shows the pre-October sign-up flow. Restore once the PM refreshes it.
**Prefer to watch?** This short walkthrough covers signing up, then inviting your team. It plays automatically – use ◀ ▶, the dots, or click the slide to step through at your own pace:

<iframe src="pages/walkthroughs/signup/index.html" title="Sign up, subscription and user management walkthrough" style="width:100%;max-width:960px;aspect-ratio:16/10;border:1px solid #d9e0e6;border-radius:8px" loading="lazy"></iframe>
-->

## Steps

### 1. Choose a plan on the website

| Plan | Notes |
|---|---|
| Solo | One named user, private personal workspace, automatic monthly renewal, self-service billing |
| Team | Shared organisation workspace, flexible licensed seats, email invitations, organisation membership |
| Enterprise | Everything in Team, plus enterprise organisation access, company email domain association, multiple organisation workspaces |

See the website's plans section for current prices. Paid plans can be billed monthly or annually – paying annually saves 15%.

**Free trial.** A **Team trial** gives you **five licences for 30 days, with no card required**. It ends automatically – you're only charged if you choose to buy. Enterprise doesn't have a self-service trial – contact **support@lware.com** if you'd like to evaluate it.

### 2. Create an account, or sign in

Choosing a trial or **Buy** on the website opens Logic+'s **Create an account** screen (it reads **Sign in or create billing account** when you're buying). Choose:

- **Create an email account** – a new account with an email address and password
- **Continue with an existing account** – if you already have a Logic+ email account
- **Continue with Microsoft account** – for a Microsoft work or school account

**Back to Plans** takes you back to the website. Already a Logic+ user? The **Welcome back** screen has **Log in with email** and **Log in with Microsoft account**.

**Creating a new account:**

1. Enter your email address and click **Continue**.
2. Choose a password and click **Continue**. It needs at least 8 characters, including at least three of: lower-case letters, upper-case letters, numbers and special characters.
3. When asked to authorise the Logic+ application, click **Accept**.

![Create your account – enter your email](images/signing-up/create-account-email.png ':size=320')
![Create your account – set your password](images/signing-up/enter-password.png ':size=320')
![Authorise the Logic+ application](images/signing-up/authorise-app.png ':size=320')

**Signing in with Microsoft:**

1. Enter your Microsoft email address and click **Next**.
2. Review the permissions Logic+ is asking for, and click **Accept**.

![Microsoft sign-in](images/signing-up/microsoft-sign-in.png ':size=360')
![Microsoft permissions requested](images/signing-up/microsoft-permissions.png ':size=360')

### 3. Verify your email

Logic+ emails you a verification link. Click it, then return to Logic+. If you can't find the email, the **Verify your email** screen has a **Resend verification email** button – you'll see *"Verification email sent. Check your inbox."* once it's on its way. You can't start a trial or buy a plan until your email is verified.

### 4a. Starting a trial

Once your email is verified, Logic+ **creates your Team trial automatically** – you'll see *"Creating your Team trial…"* on the **Start your Team trial** screen. There's no checkout and nothing to fill in:

- Your **workspace is named after your email domain** – for example, someone signing up as `joe.emery@lware.com` gets a workspace called "lware", and their name is taken from the first part of their email address.
- You get **five licences for 30 days**, and you're the workspace's **Owner**.

When it's ready you're taken straight to **[All projects](pages/all-projects.md)**. If the screen says your workspace is "still being set up", click **Try again**. Starting the trial again just resumes the same one – you won't get a second.

### 4b. Buying a plan

1. Logic+ asks whether to **Pay monthly** or **Pay annually (save 15%)** (skipped if you already chose on the website).
2. You're taken to a secure **Stripe** checkout. For a new Team or Enterprise organisation, enter your **Organisation name** and **Tenant name** (the short name shown in the workspace selector at the top of Logic+), along with your billing details. Review the total before confirming.
3. After payment, Logic+ activates your plan and you're logged in to your new workspace. If it can't, you'll see *"We could not activate your plan"* with a **Try again** button.

Buying a plan during a trial replaces the trial – your paid plan starts as soon as you pay. Opening checkout and then abandoning it leaves your trial untouched.

Your purchased plans are listed on the **Your plans** page (**/licensing**), where you manage billing and seats – see [Subscription & Billing](pages/subscription-billing.md).

### 5. You're in

For Team or Enterprise, you're the workspace's **Owner**. **Save the link** so you can get back in later.

Next steps:

- Check you're in the right workspace before creating projects – see [Choosing a workspace](guides/getting-started.md#choosing-a-workspace).
- Invite your team – see [Managing seats](pages/subscription-billing.md#managing-seats).
- Give them access to folders and projects – see [Permissions and Access](pages/permissions.md).

## If you run into trouble

Contact **support@lware.com** with any issues, questions, or feedback.
