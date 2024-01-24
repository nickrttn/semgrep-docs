---
slug: rules
append_help_link: true
title: Rules
hide_title: true
description: Learn about Semgrep Secrets rules.
tags:
  - Semgrep Secrets
---


## Semgrep Secrets rule sample and structure

The following example detects a leaked GitHub PAT:

```yaml
rules:
- id: github_pat
  message: >-
    To revoke the token, visit the `Active tokens` page in the organization settings screen: `https://github.com/organizations/<ORGRANIZATION>/settings/personal-access-tokens/active`.
    From here, select the token, and revoke it using the drop down field and selecting `"Revoke"`.
  severity: ERROR
  metadata:
    likelihood: LOW
    impact: HIGH
    confidence: HIGH
    category: security
    subcategory:
    - vuln
    cwe:
    - 'CWE-798: Use of Hard-coded Credentials'
    cwe2020-top25: true
    cwe2021-top25: true
    cwe2022-top25: true
    owasp:
    - A07:2021 - Identification and Authentication Failures
    references:
    - https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures
    secret_type: GitHub
    technology:
    - secrets
  languages:
  - regex
  patterns:
  - pattern-regex: (?<REGEX>\b((ghp|gho|ghu|ghs|ghr|github_pat)_[a-zA-Z0-9_]{36,255})\b)
  - focus-metavariable: $REGEX
  - metavariable-analysis:
      analyzer: entropy
      metavariable: $REGEX
  - pattern-not-regex:
      (?i:a{5,}|b{5,}|c{5,}|d{5,}|e{5,}|f{5,}|g{5,}|h{5,}|i{5,}|j{5,}|k{5,}|l{5,}|m{5,}|n{5,}|o{5,}|p{5,}|q{5,}|r{5,}|s{5,}|t{5,}|u{5,}|v{5,}|w{5,}|x{5,}|y{5,}|z{5,}|0{5,}|abcde|abc123|abcd123|abcde123|abcdef123|example|sample|12345|cafecafe|deadbeef|deadb33f|asdfasdf|00112233|000111222|000011112222|aabbccdd|aaabbbccc|aaaabbbbcccc|00112233|000111222|000011112222|aabbccdd|aaabbbccc|aaaabbbbcccc|your[a-z_-]{0,}(?:cred|key|pass|pat|token))
  validators:
  - http:
      request:
        headers:
          Authorization: Bearer $REGEX
          Host: api.github.com
          User-Agent: Semgrep
        method: GET
        url: https://api.github.com/user
      response:
        - match:
          - status-code: '200'
          result:
            validity: valid
        - match:
          - status-code: '404'
          result:
            validity: invalid
```

The following sections describe new and existing keys in the context of a Secrets rule.

#### Subkeys under the `metadata` key

These subkeys provide context to both you and other end-users, as well as to Semgrep.

```yaml
  ...
  metadata:
    ...
    secret_type: GitHub
    technology:
    - secrets
  ...
```
| Key | Description |
| -------  | ------ |
| `secret_type`  | Defines the name of the service or the type of secret. When writing a custom validator, set this value to a descriptive name to help identify it when triaging secrets. Examples of secret types include "Slack," "Asana," and other common service names. |
| `technology` | Set this to `secrets` to identify the rule as a Secrets rule. |
   
#### Subkeys under the `patterns` key

These subkeys identify the token to analyze in a given match.

```yaml
  ...
  patterns:
  ...
  - focus-metavariable: $REGEX
  - metavariable-analysis:
      analyzer: entropy
      metavariable: $REGEX
  ..
```

| Key | Description |
| -------  | ------ |
| `focus_metavariable`  | This key enables the rule to define a metavariable upon which Semgrep can perform further analysis, such as entropy analysis. |
| `metavariable_analysis`  | Under `metavariable_analysis`, you can define additional keys: `analyzer` and `metavariable`, which specifies the kind of analysis Semgrep performs and on what variable.  |

:::tip
For more information, see the rule syntax for [<i class="fa-regular fa-file-lines"></i> Focus metavariable](/writing-rules/rule-syntax/#focus-metavariable).
:::

#### Subkeys under the `validators` and `http` keys

The `validators` key uses a list of keys to define the validator function. In particular, the `http` key defines how the rule forms a request object and what response is expected for valid and invalid states. Although there are some rules that do not use a `validators` key, most Secrets rules make use of it. 

```yaml
  ...
  validators:
  - http:
      request:
        headers:
          Authorization: Bearer $REGEX
          Host: api.github.com
          User-Agent: Semgrep
        method: GET
        url: https://api.github.com/user
      response:
        - match:
          - status-code: '200'
          result:
            validity: valid
        - match:
          - status-code: '404'
          result:
            validity: invalid
```

| Key | Description |
| -------  | ------ |
| `request`  | This key and its subkeys describe the request object and the URL to send the request object to. |
| `response`  | This key and its subkeys determine **validation status**. Semgrep Secrets identifies a validation status through HTTP status code **and** other key-value pairs. For example, a rule may require both a 200 status code **and** a `"message": "ok"` in the response body for the matching secret to be considered **Confirmed valid**. |

## Differences between Semgrep Secrets and Semgrep Registry rules

The Semgrep Registry includes SAST rules that can detect secrets to a certain extent. You can run these rules in Semgrep Code (Semgrep's SAST analyzer), or even write your own custom secret-detecting SAST rules, but with the following differences:

* Semgrep Code does not run a validator function against these rules, resulting in less accurate results.
    * Because the results are less accurate, these rules are not suitable as a criteria to block a PR or MR.
* The UI for Semgrep Code is tailored to SAST triage, and does not include filtering functions for valid or invalid tokens.
* Existing Semgrep Pro rules that detect secrets are transitioning from Semgrep Code to Semgrep Secrets. By transitioning these rules, improvements, such as validator functions, can be added to the rules when they are run in Semgrep Secrets.
* You can write your own custom validator functions and run them in Semgrep Secrets for your own custom services or use cases.