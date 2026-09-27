# Commslayer

Commslayer is a helpdesk and AI support agent for Shopify stores. This plugin connects Claude to your Commslayer account and teaches it how to write guidance for the Commslayer AI agent, so the guidance it drafts follows the same rules the Commslayer team uses.

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
