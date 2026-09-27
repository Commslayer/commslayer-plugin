# Testing guidance in the AI Playground

The AI Playground runs the AI agent against test conversations without replying to real customers. Guidance with the status `testing` is active in the playground only. Guidance with the status `enabled` is active in the playground and in live conversations.

## Pick the conversations to test

* Find past conversations for the same situation with `conversations_search` or `conversations_list`. Pick a few that went well and a few that went wrong.
* Add at least one edge case the policy mentions, such as an order outside the delivery window or a customer who already sent photos.
* If no past conversation fits, write a short customer message that a real customer would send.

## Test new guidance or a bigger rewrite

1. Save the guidance with `guidance_create` and the status `testing`. For a rewrite, create the new version as a separate guidance and leave the old one as it is.
2. Replay past conversations with `playground_batch_test`, or start one with `playground_simulate` and `source_conversation_id`. For a new message, use `playground_simulate` with `initial_message`.
3. Read each result with `playground_get`. Check the reply, the guidance each customer message matched, and the actions and handovers in the activity log.
4. Continue a conversation with `playground_send_message` to test follow-ups, such as the customer sending the photo that the agent asked for.
5. Change the guidance with `guidance_update`, run `guidance_lint`, and test again until the replies are right.
6. Confirm the channels with the merchant. Then set the new guidance to `enabled` and the old one to `disabled` right after it, so two versions are never live together.

## Test a change to live guidance with drafts

When `guidance_draft_save` is in your tool list, change enabled guidance through a draft. Customers keep the published guidance until you publish the draft.

1. Save the change with `guidance_draft_save`.
2. Read the recent conversations this guidance handled with `guidance_draft_get`, and replay some of them with `guidance_draft_test`.
3. Read the replies with `guidance_draft_get` after a few minutes. Compare each draft reply with what resolved the conversation before.
4. Publish with `guidance_draft_publish` when the merchant agrees, or remove the draft with `guidance_draft_discard`.

## What to check in each result

* The intended guidance matched. If a different guidance matched, the triggers overlap: merge them or make the triggers more specific.
* The reply follows the policy and doesn't promise something the agent can't do.
* Actions ran when the policy needs them, and the agent didn't ask for verification that the action already does.
* The agent handed over only when a person has to decide or act.

Tell the merchant what you tested, what the agent did and what you suggest changing.
