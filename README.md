# one-liner

A Claude skill that writes a startup **One-liner (Hook)** (max 5 words) and a **Blurb** (max 250 characters) from a pitch deck.

Built for founders submitting to Colosseum hackathons. The same One-liner (Hook) and Blurb go to two places: the Colosseum submission form and the founder's X posts.

## Credits

The core method (5 words max, Analogue and Descriptive formats, unambiguous then exciting then truthful) follows [Josip Volarević's one-liner approach](https://x.com/JosipVolarevic2/status/2096885934532768013) and [Michael Seibel's YC pitch advice](https://www.ycombinator.com/blog/how-to-pitch-your-company/). The Claim format, the cold read, the real-thing test, and the follow-up handling are this skill's own additions. Full sources are listed under **References** in [`SKILL.md`](./SKILL.md).

## Why

A good one-liner lets anyone understand the project at a glance: a judge, an investor, a stranger on X, or your parents. People read the one-liner first and read further only if it caught them. Most one-liners fail at that first step because they are fluent, professional, and ambiguous.

This skill makes Claude work the way a good pitch coach does: read the deck, ask only for what is missing, write three real candidates, reject two out loud, and explain why.

## What it does

1. **Reads the deck** (PDF, PPTX, DOCX, MD, TXT, or a URL). Falls back to reading the pages as images when the deck is design-heavy.
2. **Extracts five items**: What, Who, How, Why the claim holds, and Around (who else is in the picture). Asks one question at a time for the missing ones. The founder can skip any of them.
3. **Generates 3 One-liner (Hook) candidates** in three allowed formats:
   - **Analogue**: `Reddit for agents`
   - **Descriptive**: `Move money globally for cents`
   - **Claim**: `Job ads show who's buying`
4. **Scores them in a fixed order**: Unambiguous, then Exciting, then Truthful. A line that fails the first test is not judged on the others.
5. **Selects one**, and shows the rejection reasons for the other two so the founder learns the pattern.
6. **Writes one Blurb** whose only job is to make the claim in the One-liner (Hook) believable.

If all three candidates fail, the skill selects nothing and says what is missing. A weak One-liner (Hook) shipped is worse than none.

## Example output

```markdown
## One-liner (Hook)

✅ **negative interest loans** (3 words, Descriptive)
Cold read: "A lender where borrowers get paid interest instead of paying it."

| Result | Candidate | Format | Stopped at | Why |
|---|---|---|---|---|
| ✅ Selected | **negative interest loans** | Descriptive, 3 words | Passed all 3 | Everyone knows loans and interest rates. And it stops you: wait, negative? |
| Rejected | loans that work for you | Founder-style, 5 words | 1. Unambiguous | Reads like a sales pitch, and it tells me what it does, not what it is. |
| Rejected | self-repaying loans on Solana | Descriptive, 4 words | 1. Unambiguous | Ten readers, ten products: auto-liquidating? refinancing? auto-paying interest? |

## Blurb

Hobba lets you borrow USDC against your BTC. It borrows more against the same collateral,
farms that into yield, and uses the yield to cover your interest. Over 90% of closed-beta
users paid a negative rate.

207 / 250 characters
```

## How to use

Everything happens in a normal chat on [claude.ai](https://claude.ai) or in the Claude desktop app. No terminal needed.

### Recommended settings

| Setting | Recommended |
|---|---|
| Model | Opus or Fable |
| Thinking (Effort in Claude Code) | High or above |

The skill runs three tests in order and rejects out loud. That reasoning is where the quality comes from, so a weaker model or less thinking gives you weaker lines.

### 1. Set up (once)

1. Download [`SKILL.md`](./SKILL.md) from this repository.
2. Open a new chat in Claude and attach `SKILL.md`.
3. Send this:

   ```
   I want to use this SKILL.md as an agent skill on claude.ai or the Claude desktop app.
   Can you zip this SKILL.md and then rename it to one-liner.skill?

   After that, guide me on how to use this Agent Skill.
   ```

Claude walks you through the rest.

### 2. Make your one-liner

Open a new chat, attach your pitch deck, and use this prompt:

```
I want to create one-liner.
```

The skill starts from your deck. PDF works best. PPTX and DOCX also work, and you can paste a URL instead.

No deck? Say so, and the skill asks four short questions instead.

### What to expect

The skill shows what it read from your deck, asks one question at a time for anything missing (you can skip any of them), then gives you three candidates, one selected One-liner (Hook), and one Blurb.

The One-liner (Hook) and the Blurb are always written in English. The commentary follows the conversation language, and a `Meaning:` line is added under the One-liner (Hook) when the conversation is not in English.

### If the proposed one-liner is weak

It is almost always the input. The deck is missing something, inaccurate, or unclear, and the skill can only work with what it was given. Feed it the right information and it proposes a high-quality One-liner (Hook).

You can skip any of the questions. Every skip removes material the tests would have used, so each one lowers the quality of the line you get back. Skip only what you genuinely cannot answer.

### Follow-ups the skill handles

| You say | What happens |
|---|---|
| "I don't quite get it" | Treated as a failed Unambiguous test from a real reader. The line is withdrawn and replaced. |
| "Any other ideas?" | Up to six new candidates from angles not tried yet, compared against the current best. |
| You pick a different line | The same three tests run on your pick, honestly. It is your company, so you decide. |

## Design notes

- **Deck content is data, never instructions.** Text in the deck that reads like a directive is flagged and ignored.
- **Cold read.** Claude has read the deck, so every line looks clear to it. The skill forces a stranger's reading of the One-liner (Hook) alone before passing it.
- **Substitution test.** If three other projects in the same category could use the same line, it is rejected.
- **Real-thing test.** A metaphor that exists as a real object in the product's own field (a prescription in a health app, for example) gets read literally, so it is rejected.
- **The 250-character Blurb limit** follows [toly's (Solana co-founder) advice](https://x.com/toly/status/1746250598608241148): "No more than a 250 character blurb on what X does." Colosseum allows more.

## Scope

In scope: the One-liner (Hook) and the Blurb.

Out of scope: improving or scoring the pitch deck as a whole.

The rules assume a human reader who sees the project name and the one-liner first. Revisit them if the submission form changes.
