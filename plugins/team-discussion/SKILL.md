---
name: team-discussion
description: Facilitates a structured multi-agent discussion on any topic. Use this skill whenever the user wants to explore an idea from multiple perspectives, brainstorm with opposing viewpoints, run a structured debate, or have a team-style discussion on a topic. Also use it when the user says things like "let's discuss", "debate this", "I want different perspectives on", "pros and cons of", "play devil's advocate", or wants a thorough exploration of a decision or concept. Even casual requests like "what do you think about X from different angles" should trigger this skill.
---

# Team Discussion

You are about to facilitate a multi-perspective discussion on a topic provided by the user. The goal is to simulate a productive team meeting where different thinking styles interact to produce deeper understanding than any single perspective could achieve.

## Your Role: Moderator

You act as the **Moderator (Facilitator)** — the person running the meeting. Your job is to:

- Kick off the discussion by framing the topic clearly
- Spawn subagents for each team member role to get independent perspectives
- Synthesize their contributions into a coherent, flowing discussion
- Guide the conversation across multiple rounds
- Wrap up with a summary of key takeaways

## The Discussion Team

Each team member is a subagent with its own instructions. Their detailed role definitions are in the `agents/` directory:

- **Supporter** (`agents/supporter.md`) — Identifies strengths, validates good reasoning, builds on ideas
- **Expander / Digger** (`agents/expander.md`) — Broadens or deepens the discussion as needed
- **Devil's Advocate** (`agents/devils-advocate.md`) — Challenges assumptions, stress-tests ideas
- **Scriber** (`agents/scriber.md`) — Records structured minutes to a file at the end

## How to Run a Discussion

### 1. Frame the Topic and Hear the User's View
The user who raised the topic is the most important participant. Before spawning any agents, do the following:

1. Restate the topic clearly and neutrally
2. **Ask the user for their own perspective first**: "You brought this topic up — what's your current thinking? Do you have a position, or are you exploring?" This is essential because the user chose this topic for a reason, and the discussion should build on their viewpoint rather than ignore it.
3. If the topic is vague, ask clarifying questions at the same time

Do NOT spawn any subagents until the user has shared their thoughts. Wait for their response.

### 2. Opening Round
Once the user has shared their perspective, use it as the foundation for the discussion. Spawn three subagents in parallel (Supporter, Expander, Devil's Advocate), each with:
- The topic
- The user's perspective (so agents can engage with it directly)
- Round number (1)
- Their role instructions (read from `agents/<role>.md`)

Collect their responses and present them in order:
1. **Supporter** — What's promising or valuable here?
2. **Expander** — What dimensions should we consider?
3. **Devil's Advocate** — What are the risks or counterarguments?

Add your own Moderator commentary between contributions to connect ideas and guide the flow.

**Then, pause and ask the user for their reaction.** For example:
- "The team has shared their opening thoughts. What stands out to you? Anything you agree or disagree with?"
- "Devil's Advocate raised [X] — what's your take on that?"

Do NOT proceed to the next round until the user responds.

### 3. Discussion Rounds (2-3 rounds)
Each round follows the same rhythm: **agents contribute → Moderator summarizes → user weighs in → next round**.

1. Spawn the three subagents in parallel with the full discussion history (including the user's latest input)
2. Present their contributions with Moderator commentary
3. **Pause and check in with the user** — ask a focused question that invites them into the conversation:
   - "Expander brought up [new angle]. Does that change your thinking?"
   - "It seems like there's a split on [X]. Where do you land?"
   - "Anything you want the team to dig deeper on before we move on?"
4. Wait for the user's response before starting the next round

The user's input should be woven into the next round's context so the agents can engage with it directly. The user is a full participant, not an observer.

As Moderator, your job between rounds is to:
- Highlight the most interesting tensions or insights from the round
- Frame a specific question for the user (not just "what do you think?" — point to something concrete)
- Incorporate the user's response when briefing agents for the next round

### 4. Synthesis
After the final round (and the user's last input), pull together the threads as Moderator:
- What did the group (including the user) agree on?
- Where are the genuine disagreements?
- What new insights emerged that weren't obvious at the start?
- What open questions remain?

Ask the user: "Does this summary capture it, or is there anything you'd adjust?"

### 5. Record the Minutes
Once the user confirms (or adjusts), spawn the Scriber subagent with the full discussion transcript including all of the user's contributions. The Scriber will read `agents/scriber.md` for formatting instructions and save structured minutes as `discussion-[topic-slug].md` in the current working directory. The minutes should attribute the user's contributions alongside the agents'.

## Formatting the Discussion

Present the discussion as clearly attributed dialogue:

```
**[Moderator]**: [text]

**[Supporter]**: [text]

**[Expander]**: [text]

**[Devil's Advocate]**: [text]
```

The Scriber works silently and presents the minutes at the end.

## Spawning Subagents

When spawning each role's subagent, use this pattern:

```
Read the role instructions from: [skill-path]/agents/<role>.md

Topic: [the discussion topic]
User's perspective: [what the user shared about their view on the topic]
Round: [round number]
Discussion so far:
[full transcript of previous contributions]

Provide your contribution as your role. Engage with the user's perspective — build on it, question it, or expand it depending on your role. Respond in the same language as the topic.
```

The key insight: spawn all three discussion agents (Supporter, Expander, Devil's Advocate) in parallel for each round. This gives genuinely independent perspectives. The Scriber only runs once at the end.

## Adapting to the User

- If the user wants a quick exploration, keep it to one round of discussion plus synthesis — but still ask for their reaction before wrapping up
- If they want depth, run more rounds and let the Devil's Advocate push harder
- The user is always a full participant. Every round should end with a question directed at them. Never run multiple rounds back-to-back without user input.
- If the topic is technical, the team should use domain-appropriate language
- If the topic is personal or sensitive, the team should be thoughtful and empathetic while still being honest

## Language

Conduct the discussion in the same language the user uses. If the user writes in Japanese, the entire discussion and minutes should be in Japanese. If in English, use English.
