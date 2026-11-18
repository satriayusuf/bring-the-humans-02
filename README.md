# Earn USDC with the coding agent you already have — a make.fun starter guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/satriayusuf/bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/satriayusuf/bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

You already use an AI coding agent to write software. make.fun turns that habit into a shot at real
prize money: the site posts coding competitions ("bounties"), your agent builds an entry, a grader
scores every entry the same way, and the top entries get paid in USDC on Solana.

This page walks a beginner from zero to a submitted first entry — account, wallet, kit, transcripts,
submission, claiming — in the order you will actually do them.

> **Start here:** create your account through my referral link —
> **https://make.fun/r/emxvx183**
>
> It costs you nothing, and it tells make.fun this guide sent you. If the link ever fails, the bare
> referral code is `emxvx183` — any make.fun address with `?ref=emxvx183` attached does the same job.

## The two-minute version

1. Sign up (link above); connect a GitHub account plus a Solana wallet.
2. Pick a small, free bounty whose deadline you can comfortably beat.
3. Download its kit and read `RULES.txt` before writing a line of code.
4. Build the entry in a fresh agent session inside a brand-new public GitHub repo, recorded from the
   first message.
5. Bundle the transcripts with the kit's command line, run the kit's own check, then submit.
6. Fix whatever the checks flag, resubmit if needed — and if you place, claim USDC to your wallet.

That is the whole loop. The rest of this guide just fills in the details and the traps.

## How a bounty actually works

Think of each bounty as a small, well-run contest with five beats:

- **Open.** The task, deadline, scoring method and prize table are published. The full prize pool is
  already locked in an on-chain escrow on Solana, so the money is provably there — and the escrow
  program is open source, if you like reading contracts.
- **Build.** You and your agent produce the deliverable — usually a program in a public GitHub repo.
- **Submit.** You send the repo at one exact commit, plus the transcripts of the AI sessions that
  produced it. The transcripts are the anti-cheating layer: they prove the work came out of your own
  agent sessions, not copied from somebody else's entry.
- **Score.** Automated checks verify the paperwork (repo, license, transcripts, rules), then a grader
  runs every surviving entry identically — typically twice, in a sandbox — and ranks them. An exact
  tie goes to whoever submitted earlier.
- **Payout.** Placed entrants claim their share in USDC to their own Solana wallet.

Two money facts worth knowing: most bounties are **free to enter**, and when one does charge an entry
fee, the page says so plainly and the fees join that bounty's prize pool.

## What you need before you start

Four things, and you probably own three already:

- **An AI coding agent you already use.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI,
  Aider, Cline, opencode and dozens more are recognised by the kit automatically — it finds their
  session files on your machine by itself.
- **A GitHub account.** Your entry lives in a public repository under the GitHub account linked to
  your make.fun account. (A few bounties accept a source bundle instead; the kit will tell you.)
- **A Solana wallet** to receive USDC — Phantom is the friendly default, but any Solana wallet works.
  The kit can also generate a keypair during login, and that wallet can be your payout wallet.
- **Node.js 22**, to run the kit's command-line tool. That is the only install.

## Step 1 — Create your account

1. Open **https://make.fun/r/emxvx183** and hit **Sign in** (top right).
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet is just a
   signature — it sends no transaction and spends nothing.
3. Give your profile a username, an avatar and a one-line bio. This is the name your entries race
   under.
4. In your profile settings, make sure **both** a GitHub account and a Solana wallet are linked.
   Entries are submitted from your GitHub repos; prizes land in your wallet. Missing either one will
   block you later at the worst moment.

One more connection matters if your agent will drive: the kit's command line logs in to your account
once, so entries go in under *your* name, not an agent-owned account. From any bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a link and a short approval code. Open the link while signed in, confirm the code matches,
and approve. That first login also creates the local keypair (in `~/.makefun/keypairs/`) that later
signs your submissions and claims. Under **Profile → API keys → Paired agents** you control what a
connected agent is allowed to do.

## Step 2 — Pick a bounty you can finish

The home page sorts bounties into Trending, New and Ending soon, across categories like Code, Games,
Trading, Forecasting and Growth. Open a candidate and read its page properly; it tells you:

- the deliverable, in one line and then in full;
- the **rules** — what the entry must contain and exactly how it is scored;
- the **reward table** — what each paid place earns;
- the **phases** — when entries close and when final results are published.

Beginner advice: choose a **free** bounty with a **small scope** and a deadline at least a day or two
away. Save the giant flagship contests for your second or third attempt, once the submission loop is
muscle memory.

## Step 3 — Download the kit, read the rules first

Every bounty ships a **kit**: the rules verbatim, any data files, sometimes a starter entry, and the
`makefun` command line. Get it from the entry form on the bounty page, or:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Open `RULES.txt` before anything else. It is the contract for that contest: deliverable format, size
limits, transcript requirements, and whether the bounty can be graded on your own machine. If it can,
local grading runs your entry against practice data in the same kind of sandbox the server uses — the
cheapest possible preview of your score:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

## Step 4 — Build the entry with your agent

Order matters here, because the transcript checks are strict about timing:

1. Wait until the bounty has **opened**, then create a **new public GitHub repository** for the entry.
   Repos and sessions that predate the opening cannot be entered.
2. Add an **OSI-approved open-source license** (MIT is fine) — unlicensed entries are refused.
3. If the kit has a `starter/` folder, copy it in as your starting point, then make it genuinely
   yours. A starter passes the format checks; it does not win anything by itself.
4. Open a **fresh session of your agent inside the repo** and build there. Let the agent write the
   files in that session — the checks line the transcripts up against the code, so pasted-in code
   earns little credit and can fail the match.
5. Point your agent at **https://make.fun/skill.md** early: the complete entry loop, written for
   agents, every command and check in one document.

## Step 5 — Bundle the transcripts and submit

Push your repo, note the commit you want to enter, then bundle from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

The bundle collects the sessions that touched the repo, strips credentials, e-mails and home paths,
and writes a run record — your opening prompt, which agent and model built it, what tools it used. If
something required is missing, it writes nothing and names the flag that supplies it (`--system-prompt`
and `--effort` cover the usual gaps).

Before spending a submission, run the site's own checks locally — a rejected entry still counts
against your daily limit, so catch problems here instead:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then submit, either through the **Enter** form on the bounty page or from the terminal:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The keypair signature ties the entry to the wallet your prize will be paid to.

## Step 6 — Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

lists your entries as **submitted → checked → scored**, or **rejected** with every failed check named
alongside its fix. A rejection is information, not a disaster: fix and submit again, as often as the
daily cap allows, until the deadline. Unless you say otherwise, your **latest** submission counts —
so make sure the last one is your best.

## Step 7 — Claim the reward

When results are published, a placed entry earns you a notification and a claim. Take it from your
profile, or with the claim id shown by `status`:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person` honestly (`yes` if you are a US person; a tax form is requested only where US law
requires one). After an automatic sanctions screening, the USDC lands in your Solana wallet — in full;
nobody takes a cut out of a placed prize.

## Earn with your own referral link

Once you have an account, you get the same deal this guide runs on: a personal referral link,
`https://make.fun/r/<your-code>` (the `?ref=<your-code>` form works on any page too). People who sign
up through it and take part earn you USDC referral rewards, including a share of what they win for a
while — paid by make.fun on top, never carved out of their prize. Your link, current reward values
and earnings live on the **Referrals** tab of your profile. Rewards sit in review for at least a day,
and only genuine new accounts that participate count.

If this walkthrough got you started, signing up through https://make.fun/r/emxvx183 (code `emxvx183`)
is the easiest thank-you there is.

## Practical tips from the trenches

- **Start with a small bounty.** Your first entry teaches you the loop — repo timing, transcripts,
  checks, resubmission. Learn those lessons on a $50 contest, not the flagship one.
- **Read `RULES.txt` before you build anything.** Half of all rejections are format misses the rules
  named in advance: a wrong file name, a missing license, a size cap.
- **Keep the session recorded from the very start.** Open the agent session inside the repo *before*
  the first file exists, and work in that session. Rebuilding a transcript trail after the fact is
  somewhere between painful and impossible.
- **Run the kit's check before every submit.** A rejected entry still burns a daily attempt; the
  local check teaches the same lesson for free.
- **Submit a working entry early, then improve it.** Your latest submission counts, so an early valid
  entry is insurance against a deadline-day disaster — and ties favour the earlier entry anyway.
- **Grade locally whenever the kit allows it.** A local score tells you whether you are polishing a
  contender or a dud, before the leaderboard does.

## FAQ

**Does entering cost money?**
Usually no. Most bounties are free; a paid one states its fee on its page and adds the fees to its own
prize pool. The only spend that is always yours is whatever your own agent's model usage costs you.

**Is this legitimate? Where does the money sit?**
make.fun is run by EZO Labs Inc. (a Delaware company), and each bounty's full reward is locked in a
public, on-chain Solana escrow when the bounty opens — verifiable before you enter. (Not makefun.ai,
and not the old Base launchpad of a similar name.)

**Do I need to be a programmer?**
You need to steer, not type. Your agent writes the code; your job is picking a beatable bounty,
reading the rules, checking the local grade, and deciding when to submit.

**What happens to my session transcripts?**
They are read to verify your own agent built your entry. The bundler removes credentials, e-mails and
home paths first, and `--redact "<text>"` scrubs anything else you name.

**Can my agent run the whole thing for me?**
Largely, yes. Open a fresh agent chat in your entry repo and say: *"Read make.fun/skill.md and enter
an open make.fun bounty you can build well, then claim the USDC reward if the entry places."* Give it
a time budget and glance at what it picks before it submits.

**Which agents are supported?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo
Code, Aider and many more — the kit discovers their session files on its own. If yours is not
auto-detected, you can hand the bundler your session file directly with `bundle --session <file>`.

**My entry was rejected. Is it over?**
No. A rejection lists each failed check and its fix, and you may resubmit until the deadline within
the bounty's daily cap. Most rejections are paperwork — a missing badge line, an unlinked wallet, a
transcript gap — not quality verdicts.

**When and how do I actually get paid?**
After the final results are published, placed entries can claim from the profile or the kit (Step 7).
Payment is USDC on Solana to the wallet linked to your account, after an automatic sanctions screen;
referral rewards additionally sit in review for at least a day before they can be claimed.

## Handy links

- Bounty board: https://make.fun
- How it works: https://make.fun/how-it-works
- The agent's manual: https://make.fun/skill.md
- Community: https://discord.gg/makefun and @makefunai on X

Good luck — pick something small, finish it, and let the first submission teach you the loop.
