# AIF Forge

## What this is

AIF Forge builds conversational agents that are wired into real systems, and a factory that
builds those agents.

The agents are Telegram-based consultation bots. They run in production, they talk to paying
users every day, and they are boring in exactly the way production software should be boring:
they restart cleanly, they keep their memory, they refuse the questions they should refuse, and
they say what they are when asked.

The factory is the longer project. Today an agent is assembled from a shared core and deployed by
hand from a blueprint. The goal is that the blueprint alone is the input — that adding a new
agent type is a specification, not a week of work. That part is under construction, and this
README will not pretend otherwise.

## What runs today

Six consultation agents serve end users across two tenants: three domains, each in a generic
build and a curated build. The curated builds carry a domain expert's voice, their editorial
canon, and their red lines; the generic builds carry the same machinery without them. Same core,
different persona layer — that separation is the whole point of a factory.

Alongside them run a maintenance agent that answers technical questions about the fleet itself,
four older single-purpose assistants from earlier generations, a monitoring agent, a relay that
connects an operator to their channel, and a scheduled publisher that drafts channel posts for
human approval before anything goes out.

Two internal services sit behind the agents: a billing service handling subscriptions, and an
ephemeris service doing real astronomical computation. Neither is an agent. Both exist because
the agents needed answers that a language model cannot produce honestly.

## The thesis: integrations over conversation quality

Conversation quality is close to solved. Any competent model, given a decent persona prompt,
will hold a warm and coherent conversation. Competing there is competing on the commodity.

What is not solved is everything the conversation touches.

An astrology agent that computes planetary positions from a real ephemeris is doing something
categorically different from one that improvises them, even when the two answers read the same.
A numerology agent that refuses to build a chart it has no valid method for is more useful than
one that fills the gap with confident text. An agent that remembers your birth data across
sessions, that knows when your subscription lapsed, that hands you a crisis helpline instead of
a horoscope when you say the wrong thing at 2am — that agent is made of integrations, not of
prose.

So the work concentrates there: real computation instead of plausible numbers, persistent
per-user memory, payment state the agent actually knows about, consent and data-deletion paths,
and hard limits that hold under pressure. The persona is the last layer, not the first.

The same principle applies to what an agent says about itself. Every agent discloses that it is
an AI when asked directly, names the human curator behind the editorial voice as a person rather
than a brand, and does not blur the difference between itself and a human practitioner. This is
not a disclaimer in the footer. It is behaviour, and behaviour has to be measured.

## How we know it works

Claims about agent behaviour are worth exactly as much as the test behind them.

Identity and disclosure are checked by an automated scan that puts the same questions to every
production bot, against the deployed prompt and the deployed model, and scores the answers with
deterministic rules rather than a model acting as judge. Because sampling is not deterministic, a
single clean run does not count; a fix is only confirmed by consecutive clean runs. Because the
rules themselves can drift, every report records the version of the rules that produced it, and
raw answers are stored so that a rule change can be re-checked against past data — including a
negative control that must still catch the failures the fix was written for.

That discipline started as a checklist item and turned into a tool. Several of the tools here
started that way.

## Honest limits

This is a small operation. The agents are real and in production; the factory around them is
partly built. Some of what is described here as a system is still a well-organised set of
scripts. Where that is true, the repositories say so.

Public repositories under this organisation are the parts that stand on their own. Client
deployments, editorial content, and user data are not here and will not be.
