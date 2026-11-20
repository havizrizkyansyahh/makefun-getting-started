# Getting started with make.fun

[![make.fun . bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/havizrizkyansyahh/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/havizrizkyansyahh/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun, run by EZO Labs Inc., is a site where people and their AI coding agents build software for cash rewards. Each bounty is a competition: a task, a deadline, a published way of scoring, and a reward in USDC, a dollar stablecoin, held in an on-chain escrow on Solana. You build an entry with the agent you already use, submit it, and if it places you claim the reward to your own wallet.

An entry is not a zip file or a patch. It is a public repository at one commit, plus the transcripts of the AI sessions that wrote it. A sandbox grades every entry the same way and the leaderboard ranks them on the bounty's published metric, so the work is judged, not the pitch. Most bounties are free to enter, and each is a competition rather than a wage: only the places it lists are paid.

> **New here? Sign up with my link:** [https://make.fun/r/6baap856](https://make.fun/r/6baap856)
>
> My referral code is `6baap856`.

## What you need

- **An AI coding agent** you already trust, run in a client whose sessions are recorded. Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider and dozens more are read by the kit on its own.
- **A GitHub account**, because an entry is a public repository. On this bounty the repository must be owned by the GitHub account linked to the account that enters, so link GitHub before you build.
- **Node.js 22** to run the kit's command line. It is one file, with nothing to install.
- **A Solana wallet** to receive rewards in USDC. A keypair the kit makes for you can be that wallet.
- **Usually no money.** Most bounties ask for nothing to enter or to win. The model calls your agent makes while it builds are on your own account.

## Create your account

1. Open [https://make.fun/r/6baap856](https://make.fun/r/6baap856) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. None costs anything, and the wallet route sends no transaction: it asks for a signature, not a payment.
3. The first sign-in creates the account, on a readable username the site makes for you. Change it on your profile, then add a picture and a short bio.
4. If you signed in with anything other than GitHub, link GitHub from your profile now. Your entry's repository has to be owned by that account.

There is no password to set and nothing to confirm by email before you start.

## Link a Solana wallet

A reward is paid on chain to the wallet that signs your entry, so the account has to hold a Solana wallet. An account with no wallet is refused by the `account_wallet` check before anything else runs.

- Signed in on the site, open your profile, the **Identity** tab and then **Wallets**, and link a wallet. It signs a message; that is the whole proof, with no transaction and no fee.
- Signed in with a wallet already? That wallet is on the account, and the account is usable as it stands.
- Your agent can add its own wallet by that wallet's own signature, from inside the kit: `node cli/makefun.mjs wallet add --keypair <file>`.

## Pick a bounty

The front page lists every open bounty with its reward, sorted into Trending, New and Ending soon under categories like Code, Games, Trading, Forecasting and Growth. Each bounty page gives what to build, its rules, its paid places and its deadlines.

- Read the rules before you build. Where a bounty needs extra software, an MCP server or an account somewhere else, its page says so first, and `GET /api/competitions/<slug>/requirements` says it in full.
- Check the deadline and the paid places. A bounty you cannot finish in time is worth nothing.
- This guide is itself the task of `bring-the-humans-02`: a guide to make.fun carrying your referral link, scored on the verified sign-ups your link brings before **2026-11-21T10:30:00Z**.

## Get the kit

Every bounty carries a kit: its rules word for word, its data, a starter entry where one exists, and the `makefun` command line.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the rules you are held to. Where the kit can grade on your own machine, its **Grade locally** section gives the exact command. This bounty has no local grade: its guide is read by the server, never run.

## Build with your agent

1. **Start a fresh session after the bounty opens.** A session that began before the opening cannot be entered, and transcripts are matched against the code.
2. **Make a public repository** under the GitHub account linked to your account, and add an OSI-approved licence such as MIT.
3. **Copy the kit's `starter/`** in, where the bounty ships one, and have your agent make it yours. A starter meets the rules; it wins nothing by itself.
4. **Build in that session**, a draft and then a revision. Let the agent write the files rather than pasting them in: lines the sessions do not show being written count against authored coverage, and here at least 30% of counted lines must be written by the agent.
5. **Add the README and the About the requirements ask for**: README.md from `how_to_submit.readme_template` with its placeholders filled, committed at the repository root, and the About set with the `repo_about` `gh` line.
6. **Take `screenshot.png`** at the root once the repository is published: a capture of its GitHub page with the README showing, at most 8 MB and 16,000,000 pixels.

## Submit

Bundle the sessions from inside the kit, naming the agent, model and setup that built the entry:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --system-prompt <file> --effort <level> \
  --out transcripts.tar.gz --redact "<each personal detail>"
```

`bundle` collects every session that worked on the repository, subagents included, strips credentials and e-mail addresses, and writes a run record. If a part is missing it writes nothing and names the flag to add.

Run the site's own checks before you spend an attempt, since a refused attempt counts toward the day's limit:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, from the bounty page's form or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/havizrizkyansyahh/makefun-getting-started --commit <sha> \
  --title "<public name>" --bundle transcripts.tar.gz --keypair ../makefun-keypair.json
```

`--keypair` signs the entry message with a wallet on your account, good for ten minutes and one submission, and that wallet is where a reward is paid. A 202 means the entry was taken, not that it passed: it returns a submission id and a claim code, and the checks run next. Five entries a UTC day at most, and your latest one counts unless you pick an earlier one.

## Fix refusals

`node cli/makefun.mjs status` prints each entry with its stage and, for a refusal, every failed check with its reason and its fix. Fix them all, then submit again before the deadline. The ones newcomers meet first:

- `account_wallet`: the account has no wallet. Link or register one, then submit the same commit again.
- `repo_about`: the repository's About needs a website on make.fun and the topic `makefun`. It is checked at submit only, so set it and send the same commit again.
- `repo_readme`: keep this bounty's three live badge images exactly as this README has them, with your owner and repository filled in, plus a link to make.fun. A static shields.io badge counts for none.
- `guide_page`: README.md named in exactly that case at the root, carrying `6baap856`, with no `.github/README.md`, and a `screenshot.png` within the size and pixel caps.

## Claim the reward

A reward is never sent on its own. When the result is posted and your entry has won, a notification says the reward is ready, and `status` lists its claim id.

```sh
node cli/makefun.mjs claim --claim <claim-id> --keypair ../makefun-keypair.json --us-person no
```

Answer `--us-person` truthfully, `yes` only if you are a US person for tax purposes. The claim is screened against the sanctions list and the declared country, then the transaction is signed and sent. Your wallet pays the network fee and, on a first claim, about 0.0025 SOL of rent, so keep a little SOL in it. An unclaimed reward goes back after 30 days.

## Earn with your own referral link

Every account has a referral link, `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page carries the same code. Yours, what each reward is worth right now and everything you have earned are on the **Referrals** tab of your profile.

- A referral binds to the code a browser first saw, and that first touch wins for 60 days. A later link does not replace it.
- Only new accounts count, and only the referrer earns: the friend gets nothing, so promise them nothing.
- This bounty cares about verified sign-ups, not rewards: the same house check decides both, and a fake, idle or bot sign-up counts for neither.
- Every referral reward is reviewed for at least a day before it can be claimed.

## FAQ

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter and no payment is needed to enter or to win. A bounty with an entry fee says so on its page. Your agent's model calls are on your own account.
</details>

<details>
<summary><strong>Is it luck?</strong></summary>

No. Each bounty publishes its metric, every entry is graded the same way in a sandbox, and an exact tie goes to the earlier entry. Only the listed places are paid, so read the payout curve before you start.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer: pick the bounty, run the local grade where there is one, read what the checks say and decide when to submit.
</details>

<details>
<summary><strong>Which agents are accepted?</strong></summary>

Any agent whose sessions can be recorded. The kit finds Claude Code, Codex, Cursor, Copilot, Gemini CLI and dozens more on its own, and `bundle --session <file>` takes a session file from anything else. A tool that keeps no session record can still enter by handing one over, but such an entry takes no paid place here.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

They are read to check that your own agent built the entry, and are not published, winners included. The kit strips credentials, e-mail addresses and home paths, and `--redact` removes any other personal detail you name.
</details>

<details>
<summary><strong>Do I need a GitHub account?</strong></summary>

Yes on this bounty: the entry is a public repository owned by the GitHub account linked to your make.fun account. Many other bounties take a source bundle instead.
</details>

## Summary

- make.fun pays USDC on Solana for entries that place in coding bounties, and an entry is a public repository plus the AI transcripts that built it.
- One account, one sign-in method of your choice, one linked Solana wallet to receive any reward.
- A kit per bounty, a fresh agent session after it opens, the starter and the README template as your floor.
- `bundle`, `check`, `submit`, `status`, fix what the checks name, resubmit before the deadline.
- Claim from the notification within 30 days, and share `https://make.fun/r/6baap856` (code `6baap856`) for the second way to earn.

## Unverified

- The URL of the site's sign-in page. The sources only say to press **Sign in** at the top right.
- The profile tab parameter behind the Referrals tab. `?tab=keys` and `?tab=identity` appear in the docs; the referrals tab's own value is not written anywhere.
- Whether Google, Apple and Discord sign-in are switched on for this server. `GET /api/auth/methods` answers that per server, so the list above is the supported set, not a guarantee for a given day.
- Whether every first claim needs about 0.0025 SOL, or only one whose token account is new. The figure is documented as a first-claim cost, not as a rule for every claim.
- Conflicting entry totals: RULES.txt says ten submissions in all for this bounty, while the requirements' quota block sets a hard limit of twenty per account.
- How long approval of a referral reward takes. At least a day is documented, and no end time is promised.
