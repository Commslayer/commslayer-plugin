# Commslayer

Commslayer is an affordable helpdesk and AI support agent for Shopify stores. Email, live chat, Instagram, Messenger, WhatsApp, TikTok and phone calls all land in one inbox, with the customer's Shopify orders next to every conversation. The AI agent answers customers on all of those channels.

Connect Commslayer to Claude and run your support from the chat:

- Reply to customers, and assign, label, snooze or close tickets
- Search your whole support history by topic, channel, label or date
- Train your AI agent: edit its guidance and actions, then test the changes before customers see them
- See what the AI agent couldn't answer and write the missing answers
- Turn support threads into help center articles
- Set up saved replies, macros, automation rules and labels
- Check the Shopify order, customer and subscription behind a ticket (read-only)
- Ask for reports on volume, response times, CSAT and AI agent performance

You can see and change the same things your Commslayer role allows. Requires a Commslayer account. Works on every Commslayer plan.

## What it adds

- **Commslayer connector.** Claude can read and work with your tickets, contacts, help center, knowledge, reports and AI agent settings through the Commslayer MCP server.
- **AI agent guidance skill.** When you ask Claude to write, change or test guidance, it loads the skill first. The skill tells Claude to read your existing guidance, rules, tone and actions before it writes, where each kind of instruction belongs, how to write triggers and instructions, and how to test the result in the AI Playground before customers see it.

## Set it up

1. Install the plugin. In Claude Code, you can also install it from this repository:

   ```
   claude plugin marketplace add Commslayer/commslayer-plugin
   claude plugin install commslayer@commslayer
   ```

2. Connect the Commslayer connector and sign in with your Commslayer account. Pick the account to connect if you have more than one.
3. Ask Claude, for example: "Turn our returns policy into AI agent guidance and test it on last week's return requests."

## Data

The plugin has no code of its own. The connector sends requests to `https://app.commslayer.com/api/v1/mcp` and receives data from your Commslayer account, with the permissions of the Commslayer user who signed in. Sign-in uses OAuth, and Claude never sees your password. The skill is plain instructions and sends nothing.

- Privacy policy: https://www.commslayer.com/privacy-policy
- Terms of service: https://www.commslayer.com/terms-of-service
- Help: https://docs.commslayer.com/en/articles/1768296514-connect-claude-and-chatgpt-via-mc
