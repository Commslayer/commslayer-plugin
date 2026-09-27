---
name: ai-agent-guidance
description: Draft, edit and test guidance for the Commslayer AI agent. Use when the merchant wants to turn a policy into AI agent guidance, change how the AI agent handles a customer situation, fix guidance that misfires or hands over too often, decide whether something belongs in guidance, knowledge, general rules, tone or actions, or test guidance in the AI Playground. Load it before calling guidance_create, guidance_update, settings_update_ai_agent or ai_actions_update.
---

# Commslayer AI agent guidance

Help the merchant turn their policy into clear AI agent guidance with a title, a specific trigger and concise instructions.

Read existing guidance, general rules, tone settings, the enabled actions and their conditions before writing anything:

1. `guidance_list`, then `guidance_get` for any guidance that covers the same situation.
2. `settings_get_ai_agent` for general rules and tone.
3. `ai_agent_capabilities` for what the agent can look up and change on its own.
4. `ai_actions_list`, then `ai_actions_get` for the conditions of each action the policy needs.

## How the AI agent is set up

Each part of the setup has one job. Most bad setups come from putting something in the wrong place.

| Part | What it's for | Read with | Change with |
| -- | -- | -- | -- |
| Knowledge | Facts and policies: delivery times, return rules, product details | `knowledge_search_articles`, `articles_get`, `response_examples_list` | `articles_create`, `articles_update`, `response_examples_create`, `response_examples_update` |
| General rules | Brand and compliance rules that apply to every reply | `settings_get_ai_agent` | `settings_update_ai_agent` |
| Tone | How the agent sounds on each channel, including greetings and sign offs | `settings_get_ai_agent` | `settings_update_ai_agent` |
| Guidance trigger | When a guidance applies: the customer's intent | `guidance_list`, `guidance_get` | `guidance_create`, `guidance_update` |
| Guidance instructions | What the agent should do in that situation | `guidance_get` | `guidance_create`, `guidance_update` |
| Actions | What the agent can change in connected systems. Action conditions are requirements the action enforces itself | `ai_actions_list`, `ai_actions_get` | `ai_actions_update` |
| Automations and labels | Routing and reporting for the team | `automation_rules_list`, `labels_list` | `automation_rules_create`, `labels_create` |

The platform decides when the agent replies, waits, hands over or closes a ticket, and it runs identity and order ownership checks before sensitive actions. None of this belongs in text settings. Writing it there overrides the platform, and the agent follows the text even when it doesn't fit the conversation.

## Knowledge

Knowledge holds the merchant's facts and policies. It comes from several sources:

* Help centre articles. These are customer facing, so write them for customers.
* Text snippets. Internal facts the agent can use but customers never see. The `response_examples_*` tools read and write them.
* Files: PDF, DOCX, CSV, TXT, Google Docs and Google Sheets.
* Shopify products, pages and collections.
* Canned responses marked for AI use. Canned responses are reply templates for human agents first.

Before writing guidance, search knowledge with `knowledge_search_articles` and `knowledge_search_canned_responses`. Article search returns summaries; use `articles_get` to read the full policy when access allows. If you can't read the policy, ask the merchant for it rather than guessing.

The agent has built in knowledge search. Guidance can tell it to search for and follow a policy, such as "Search knowledge for the damage policy." This doesn't need a separate action.

Do:

* Put customer facing policy in articles and internal facts in text snippets.
* Keep offers and the conditions for them in guidance, not knowledge. Deciding which offer to make, and when, is a step the agent takes.
* Fix a gap in facts by adding or updating knowledge, not by writing guidance.
* Ask the merchant about missing or conflicting policy.

Don't:

* Treat canned responses as policy. They're written for human agents, often with blanks like "[tracking link]" or details that are out of date. Skip templates with placeholders and confirm anything old with the merchant.
* Delete canned responses. Human agents rely on them.

Canned responses marked for AI use are fine once audited. Review each one the agent reads: fill or remove blanks, replace variables from other helpdesks, and update old prices or policy. Turn off AI use on templates that don't answer customers, such as proactive outreach or internal notes. Human agents keep using all of them.

## General rules

Do:

* Keep them to what must hold in every reply: banned words, claims the agent must not make, market specific wording, brand name spelling.
* Point out general rules that conflict with what the merchant wants, instead of writing guidance around them.

Don't:

* Add handover, closing, greeting, sign off or fallback behaviour. A blanket rule overrides every guidance. For example, "hand over any message under 8 words" sends "thanks" and bare order numbers to a person.
* Add rules that only apply to one situation. Write those as guidance.

## Tone

Do:

* Describe voice per channel in the tone fields: warmth, length, formality, greetings, sign offs.

Don't:

* Repeat tone inside guidance or general rules.

## Guidance triggers

Do:

* Describe one customer intent in one or two sentences, the way you'd brief a new support agent: "Customer wants to cancel their subscription."
* Keep one guidance per intent. Handle variations as scenarios in the instructions.

Don't:

* List phrases the customer might type ("cancel my sub", "stop sending", "I want out"). The agent recognises intent. Keyword lists miss paraphrases and match unrelated messages.
* Name or number other guidances, or add "do not match" sections. If two guidances overlap, merge them.
* Write catch all or holding triggers ("any message on this channel", "interim until dedicated guidance exists").
* Put details that change the response, such as order status, timing or region, in the trigger. They belong in the instructions as scenarios.

If the trigger is longer than the instructions, it's doing the instructions' job. Before saving, check the trigger for keyword lists, quoted customer phrases and references to other guidances, and rewrite it if you find any.

## Guidance instructions

Do:

* Write conditional steps: if this, then that.
* Tell the agent to search knowledge for the relevant policy.
* Use the actions the policy needs. Escalate only when a person has to decide or act.
* Apply labels when the team needs them for routing or reporting.
* Ask for ordinary troubleshooting details, such as which item is damaged or a photo. This is different from identity verification.

Don't:

* Set ticket status: resolve, close, snooze, pending, "never resolve".
* Script replies ("send exactly", "verbatim") or write placeholders like "[first name]". The agent copies them into replies.
* Add identity or order ownership verification, or ban lookups the agent would normally run.
* Repeat conditions an action already enforces.
* Treat example policies as merchant defaults.

For finished guidance to compare against, read [references/examples.md](references/examples.md). The policies there are examples, not defaults for every store.

## Actions

Do:

* Check which actions are on before writing guidance.
* If the policy needs an action that's off, ask the merchant to turn it on.

Don't:

* Write a handover around a disabled action, or promise the customer something the agent can't do.

## Before and after saving

* Leave the [Template] guidances in place, enabled or disabled. Don't delete or edit them.
* Read existing guidance first. For a small change, update it in place. For a bigger rewrite, keep the old guidance on, test the new one in the playground, then enable the new one and disable the old one. Never leave two versions live, and delete old versions once nothing points to them.
* Run `guidance_lint` on the trigger and instructions. `guidance_create` and `guidance_update` refuse text that breaks an error rule, so fix every error and read every warning.
* Test in the playground against relevant past conversations, or create a new test conversation. Make sure the intended guidance is active, then review the reply, the matched guidance and the action log, and suggest improvements. [references/testing.md](references/testing.md) has the tool steps.
* Confirm with the merchant which channels the guidance should run on.
* A week after launch, check `reports_ai_agent_guidance`. Review any guidance with a handover rate above about 80%.
