# n8n-instagram-comment-to-dm-Automated-DMs-
# Instagram Comment-to-DM Automation (n8n)

An n8n workflow that automatically sends a DM to anyone who comments a trigger keyword (for example "desmos") on your Instagram posts or reels. The DM contains your link, and the workflow can also reply under the comment so people know to check their inbox.

## Features

- Triggers on keyword comments (case-insensitive, with a configurable keyword list)
- Sends a private reply DM with your link through the Instagram API
- Optional public reply under the comment ("Sent you a DM!")
- Ignores your own comments to prevent loops
- Skips comments it has already handled, so nobody gets duplicate DMs
- Handles Meta's webhook verification
- Optional follow-up: sends a file or media after the user replies to the DM

## Requirements

- An n8n instance (cloud or self-hosted)
- An Instagram **Business or Creator** account
- A Meta developer app with these permissions:
  - `instagram_business_basic`
  - `instagram_business_manage_comments`
  - `instagram_business_manage_messages`
- An Instagram access token

## Setup

1. In n8n, create a new workflow, click the three dots, choose **Import from file**, and select `instagram-comment-to-dm.json`.
2. Create a **Data Table** named `ig_processed_comments` with these columns: `comment_id`, `commenter_id`, `commenter_username`, `media_id`, `matched_keyword` (text), `processed_at` (date), `media_sent` (boolean). Select it in the Data Table nodes.
3. Create a credential with your Instagram access token and select it in the HTTP Request nodes.
4. In the **Verify Token Matches?** node, replace `YOUR_VERIFY_TOKEN` with a secret string of your choice.
5. In the **Config** node, set your link, keywords, and numeric Instagram account ID.
6. Publish the workflow.
7. In the Meta App Dashboard, go to Instagram, then API setup, then Configure webhooks. Paste the **Production** webhook URL from the Webhook node, enter the same verify token, click **Verify and save**, and subscribe to the `comments` field.

## Testing

Comment your keyword on one of your posts from another account (it must be added as a tester while your Meta app is in Development mode). Check the **Executions** tab in n8n to see the run.

## Limitations

- A private reply can only be sent once per comment, within 7 days of the comment.
- The first DM can only be text. Files and media are sent after the user replies.
- Access tokens expire (long-lived tokens last 60 days), so refresh yours regularly.

## Security

Never commit your access token to a public repo. This workflow stores credentials in n8n, not in the JSON file.

## License

MIT
