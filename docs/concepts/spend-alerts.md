---
icon: material/bell-outline
---

# Spend alerts

Chakshu can post to a Slack channel when this month's spend crosses a share of the monthly cap, and send a short weekly digest.

## Set up

1. In Slack, create an incoming webhook for the channel you want.
2. In Chakshu, open **Settings**, then **Spend alerts**.
3. Paste the webhook URL into **Slack incoming webhook**. It's stored encrypted.
4. Under **Alert at (% of the monthly cap)**, list the percentages, separated by commas. The default is `50, 80, 100`.
5. Tick or untick **Weekly digest on Mondays**, save, and press the test button to check the message arrives.

Spend alerts need a [monthly cap](../reference/configuration.md). Without one there's nothing to measure against, so only the digest is sent.

## Spend alerts

The first time spend crosses each percentage in a calendar month (UTC), Chakshu posts once: how much has been spent of the cap, where it's heading by month end at the current rate, the day the cap would be reached, and a link to the Cost view. Each percentage is announced once a month.

## Weekly digest

On Mondays Chakshu posts last week's reviews, pull requests and spend, findings posted, fixed and dismissed, fixes by severity, the skill with the most dismissed findings, and the repositories that spent the most. It contains no names.
