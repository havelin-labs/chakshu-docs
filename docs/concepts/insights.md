# Insights

The **Insights** page shows, for each reviewer and skill, what happened to the inline findings Chakshu posted. Use it to find skills that catch real problems and skills that mostly add noise.

| Column | Meaning |
|---|---|
| Posted | Inline findings posted in the period |
| Fixed | The code under the comment changed and the thread was resolved, by Chakshu or a person |
| Dismissed | Someone resolved the thread without changing the code |
| Open | No verdict yet |
| Fix rate | Fixed ÷ (fixed + dismissed) |

Pick 30, 90 or 365 days at the top.

## When outcomes are recorded

Chakshu reads its review threads when it reviews the pull request again, and once more when the pull request is closed or merged. A verdict never changes once recorded. Findings that only appear in a review's summary (lines outside the diff) have no thread, so they aren't counted.

## Reading the numbers

- **Low fix rate, many findings:** the skill flags things the team doesn't care about. Make its instructions narrower or raise the reviewer's `min_severity`.
- **High fix rate, few findings:** the skill is precise. Consider adding it to other reviewers.
- **Lots of open:** pull requests are being merged without anyone touching those comments. Look at a few runs to see whether the findings were right.
