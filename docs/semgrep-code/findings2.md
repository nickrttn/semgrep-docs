---
slug: findings2
append_help_link: true
title: Findings, v2
hide_title: true
description: Learn how to use Semgrep Code's Findings page in Semgrep Cloud Platform to manage your scan results.
tags:
  - Semgrep Cloud Platform
  - Semgrep Code
  - Team & Enterprise Tier
---

import MoreHelp from "/src/components/MoreHelp"
import DisplayTaintedDataIntro from "/src/components/concept/_semgrep-code-display-tainted-data.mdx"
import DisplayTaintedDataProcedure from "/src/components/procedure/_semgrep-code-display-tainted-data.mdx"

# View findings in Semgrep Cloud Platform

<ul id="tag__badge-list">
{
Object.entries(frontMatter).filter(
    frontmatter => frontmatter[0] === 'tags')[0].pop().map(
    (value) => <li class='tag__badge-item'>{value}</li> )
}
</ul>

Semgrep Code generates a **finding** when a rule matches a piece of code in your codebase. You can view your findings in Semgrep Cloud Platform on the [**Findings** page](https://semgrep.dev/orgs/-/findings).

:::info
You've set up Semgrep, scanned your codebase, and are ready to view your Semgrep Code results.
:::

![Semgrep Cloud Platform Findings page](/img/app-findings-overview.png)<br />

To access the [**Findings** page](https://semgrep.dev/orgs/-/findings):

1. [Log into Semgrep Cloud Platform](https://semgrep.dev/login)
2. Click **[Code](https://semgrep.dev/orgs/-/findings)** to navigate to the **Findings** page.

## Findings page structure

The **Findings** page consists of:

- The filter panel, which you can use to filter for specific findings
- Information about findings identified by Semgrep Code. Each  finding in the list includes:
  - The name and description of the rule used to generate the finding
  - The name of the project, as well as links to the Git branch and source code, where Semgrep Code identified the finding

### Group findings

By default, Semgrep groups all of the findings by the rule Semgrep used to match the code. The rules that reported the highest number of findings are at the top of the page. This helps you see which rules have the highest efficiency and which generate a lot of noise in your results.

![Screenshot of the Findings page with findings grouped by rule](/img/app-findings.png)<br />
Screenshot of the Findings page with findings grouped by rule.

To view findings individually, toggle **Group by Rule** to **No grouping** using the drop-down menu in the header. Findings are displayed based on the date they were found, with the most recent finding listed at the top.

![Screenshot of the Group by Rule option](/img/cloud-platform-findings-no-grouping.png)<br />
Screenshot of the No grouping option.

### Filter findings

Semgrep offers multiple filters you can use to narrow down your results. The following criteria are available for filtering:

| Filter                 | Description  |
| ---------------------  | ------------ |
| **Projects**           | Filter by codebases connected to Semgrep Cloud Platform. |
| **Status**             | Filter the triage state of a finding. Refer to the [following table](#triaging-findings) to understand triage states. |
| **Category**           |  Filter by the type of security issue or vulnerability the rule detects, such as `security`, `correctness`, and `maintainability`. You can select more than one category at a time. |
| **Severity**           | Filter by the severity of a finding. Possible values: <ul><li>Low</li><li>Medium</li><li>High</li></ul> |
| **Component**          | Filter by Semgrep Assistant component tags. Semgrep Assistant uses GPT-4 to categorize a finding based on its function, such as payments, user authentication, and infrastructure. |
| **Confidence**         | Filter by the likelihood of the rule to detect true positives. The higher the confidence, the more true positives the rule may detect. |
| **Recommendation**     | Filter by recommendation offered by Semgrep Assistant's auto-triage feature. Possible values: <ul><li>Fix</li><li>Ignore</li></ul> |
| **Action**             | Filter by monitoring, commenting, or blocking rules in your Policies. |
| **Rule**               | Filter by rules included in your Policies page. You can select more than one rule or ruleset for filtering. |
| **Ruleset**            | Filter by the ruleset name where rules that match the code belong. More than one rule or ruleset can be selected for filtering. |
| **Branch**             | Filter by findings in different Git branches. |

:::tip
You can identify findings categorized under **Security** by their badge.
![Screenshot of security badge](/img/findings-security-badge.png)
:::

### Display findings reported in a specific timeframe

By default, the **Findings** page displays your results from all time. To display findings reported during a specific timeframe, click the <i class="fa-solid fa-calendar-days"></i> **All time** button and select the preferred period. The following options are available:

  - All time
  - Last 1 year
  - Last 1 month
  - Last 7 days
  - Last 1 day

### View findings details about a specific finding

To view in-depth information about a specific finding:

1. Select the finding whose details you want to view details:
    - If the default **Group by Rule** is enabled, click <i class="fa-regular fa-window-restore"></i> **Details** icon on the card of the finding.
        ![Click View details if Group by Rule is enabled](/img/cloud-platform-findings-group-by-rule-view-details.png)<br />
    - If **No grouping** view is enabled, click the **header hyperlink** on the card of the finding. In the example on the screenshot below, it is the **detected-generic-api-key**.
        ![Click View details if No grouping is enabled](/img/cloud-platform-findings-no-grouping-view-details.png)<br />

#### Add notes to findings

To **add notes** to the activity history of a finding:

1. Select a finding where you want to view details or add notes, and then do one of the following actions:
    - If the default **Group by Rule** is enabled, click <i class="fa-regular fa-window-restore"></i> **Details** icon on the card of the finding.
        ![Click View details if Group by Rule is enabled](/img/cloud-platform-findings-group-by-rule-view-details.png)<br />
    - If **No grouping** view is enabled, click the **header hyperlink** on the card of the finding. In the example on the screenshot below, it is the **detected-generic-api-key**.
        ![Click View details if No grouping is enabled](/img/cloud-platform-findings-no-grouping-view-details.png)<br />
2. View, or add the notes in the **Activity** section. To add a new note, click plus **New note**.
    ![Semgrep Cloud Platform finding details page](/img/cloud-platform-finding-details.png)<br />
    Finding details page.

## Dataflow traces

<DisplayTaintedDataIntro />

### View dataflow traces

<DisplayTaintedDataProcedure />

## Data retention

The retention period for findings are as follows:

| Retention period | Tier availability |
| ---------------  | ----------------- |
| 5 years          | Team/Enterprise   |

## Further reading

- Learn how to [triage and remediate Semgrep Code findings](/semgrep-code/triage-remediation).
- See [Semgrep Assistant for Semgrep Code](/semgrep-code/semgrep-assistant-code) for information on receiving GPT-4-powered security recommendations when reviewing your findings.

<MoreHelp />