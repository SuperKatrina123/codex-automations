# AI Daily Radar Automation Prompt Template

Use this template when creating the Codex automation. Replace the placeholders before saving the automation.

```text
Run an independent AI Daily Radar workflow. Generate the report, deliver it by Gmail, and archive the final Markdown report to the configured GitHub examples repository.

Config:
- `SOURCE_SKILL`: `aihot` / AI HOT skill by default. This may be replaced with another information-fetching skill if the user wants a different source.
- `SOURCE_NAME`: `AI HOT` by default.
- `LOOKBACK_WINDOW`: past 24 hours.
- `DELIVERY`: connected Gmail account.
- `REPORT_RECIPIENT_EMAIL`: replace this with the email address that should receive the report.
- `GMAIL_LABEL_NAME`: `AI Daily Radar` by default. Replace it if you use another Gmail label.
- `GITHUB_ARCHIVE_REPO`: `https://github.com/SuperKatrina123/codex-automations.git`.
- `GITHUB_ARCHIVE_BRANCH`: `main`.
- `GITHUB_ARCHIVE_PATH`: `examples/ai-daily-radar-YYYY-MM-DD.md`.

Goal:
Fetch the past 24 hours of source items, clean and deduplicate them, filter for AI application-layer new movements, summarize 2-3 actionable trends, and generate one minimal practice plan I can try today. Keep the focus on application-layer trends and practical action, not generic news summaries.

Preflight check:
- Before fetching anything, verify that `SOURCE_SKILL` is installed and available in this Codex environment.
- If `SOURCE_SKILL` is `aihot`, use the existing Aihot/AI HOT skill rules: fetch selected AI HOT items from the past 24 hours; do not use the fixed daily endpoint; do not use raw model memory as a substitute.
- If `SOURCE_SKILL` has been replaced with another information-fetching skill, follow that skill's own instructions and fetch the equivalent past-24-hour source items.
- If no suitable source skill is installed or available, stop the workflow and tell me which skill is missing and how to install or replace it. Do not generate a report from memory or training data.
- Verify that Gmail is connected before delivery.
- If Gmail is unavailable, still generate the Markdown report in this thread and clearly state that Gmail delivery failed.
- Verify that Git can access `GITHUB_ARCHIVE_REPO` before attempting to archive.
- If GitHub archive push is unavailable, still send the Gmail report and clearly state that GitHub archive failed. Do not block Gmail delivery because GitHub push failed.

The report must be Markdown optimized for Gmail readability. Use a strict, stable heading hierarchy so the email does not look messy:

- Use exactly one H1 title at the top: `# AI Daily Radar｜YYYY-MM-DD`
- Use only H2 section titles after that. Do not use H3 titles.
- Prefix every H2 with a two-digit number so sections are easy to scan.
- Do not create ad-hoc headings inside sections. Use tables, numbered lists, or bold labels instead.
- Keep paragraphs short. Avoid long bullet walls.

Required structure:

`# AI Daily Radar｜YYYY-MM-DD`

A short 2-3 line summary block immediately under the H1:
- `时间窗：过去 24 小时`
- `数据源：SOURCE_NAME`
- `应用层精选：N 条`
- `今日主线：一句话说明今天最重要的应用层变化`

`## 01 今日结论`
Use 3 concise bullets: `今天最值得关注` / `对我意味着什么` / `建议今天试什么`. The `建议今天试什么` bullet must match the exercise in `## 05 今天的练习`.

`## 02 趋势雷达`
Use a short numbered list with 2-3 trends. One trend per item. Each item should be one concise paragraph, not nested bullets.

`## 03 今日术语表`
Use a Markdown table with exactly these columns: `术语` / `这是什么` / `为什么重要` / `我该怎么理解或使用`. Explain 3-5 important terms, new tools, technical directions, or concepts from today's sources. Keep each cell concise: 1-2 short sentences, no nested bullets.

`## 04 趋势翻译表`
Use a Markdown table with exactly these columns: `原始信号` / `应用层含义` / `可能机会`. Translate 2-3 raw news signals into application-layer meaning. Keep each cell concise and practical.

`## 05 今天的练习`
Choose one trend and turn it into a 30-90 minute practice task. The exercise must be tightly anchored to today's report, not a generic productivity drill.

Use this exact compact structure:
- `练习锚点`: name the specific trend from `## 02 趋势雷达` and cite 1-2 source signals from `## 06 应用层信号` that justify this exercise.
- `为什么今天做`: explain why this practice follows from today's sources in one sentence.
- `目标`
- `输入材料`
- `步骤`
- `验收标准`

Do not use a table here. Use compact bullets with bold labels only. Steps may be a short numbered list.

`## 06 应用层信号`
List the most relevant source items with source names and URLs. Use a compact numbered list. Each item should include title, source, one-sentence application-layer note, and URL. Do not include more than 12 items unless the day is unusually important.

`## 07 明天观察`
Use 2-3 short bullets about what to watch next.

Then send the resulting Markdown report through the connected Gmail account to `REPORT_RECIPIENT_EMAIL` with subject `AI Daily Radar - YYYY-MM-DD`. After sending, apply the Gmail label `GMAIL_LABEL_NAME` to the sent report email so it is associated with the user's daily radar mailbox label.

After Gmail delivery, archive the same Markdown report to GitHub:

1. Clone or update `GITHUB_ARCHIVE_REPO` on `GITHUB_ARCHIVE_BRANCH` outside any unrelated local project.
2. Write the final Markdown report to `GITHUB_ARCHIVE_PATH`, replacing `YYYY-MM-DD` with the report date.
3. Commit with message `Add AI Daily Radar YYYY-MM-DD`.
4. Push to `GITHUB_ARCHIVE_BRANCH`.
5. If the file already exists with identical content, do not create a new commit; report that the archive is already up to date.

If Gmail sending is unavailable, reply in this thread with the Markdown report and clearly state that Gmail delivery failed. If the label application fails, still consider the report sent, but notify me with the Gmail labeling error.
If GitHub archive push fails, still consider Gmail delivery complete, but notify me with the Git error and the local archive path if one was created.
```
