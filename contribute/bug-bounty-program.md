# Bug Bounty

## Introduction

As in any of our network upgrade, X-Cash 2.0 has been designed to add innovative features to the network. Despite rigorous checks and a period of extensive tests, there will always be potential issues that we need to address. Not all bugs can lead to security issues, but assessing and answering them quickly can only make the core code of X-Cash better. For this reason, the team has decided to launch a bug bounty program that will allow software engineers, security researchers or any other contributors to help in identifying and solving bugs.

{% hint style="info" %}
The actual Bug Bounty program is designed for the alpha test of the DPoPS. The program will be adjusted for the next test phases of the DPoPS, then we will inaugurate a permanent version of the bug bounty program.
{% endhint %}

## Software in scope

* **XCASH\_DPOPS -** [**GitHub**](https://github.com/X-CASH-official/XCASH_DPOPS)\*\*\*\*
* **XCASH\_DPOPS\_Delegates\_Website -** [**GitHub**](https://github.com/X-CASH-official/XCASH_DPOPS_delegates_website)\*\*\*\*

{% hint style="info" %}
Other programs will be added to the next phases of the test, and we will extend the scope for the permanent bug bounty program.
{% endhint %}

{% hint style="danger" %}
Monero Legacy code from 12.0 and below is not in scope for this bug bounty program. Any forks, public blockchain or applications built on top of X-Cash are not covered either by the current bounty program.
{% endhint %}

## Qualifying vulnerabilities

To be considered as valid, a vulnerability must lead to one or several of the following results:

* Causes the software to crash
* Causes the software to run in a significantly deteriorated mode \(slow, non responsive ... etc.\)
* Causes the software to use an overload of memory
* Triggers unauthorized operations with an account
* Generates non-intentional transactions on the network
* Generates an unexpected behaviour with regards to the consensus protocol
* Leads to additional coin supply generation

The above conditions remains indicative and any out-of-scope vulnerabilities will be assessed on a case-by-case basis. The X-Cash Foundation team member remains responsible for accepting or rejecting vulnerabilities at their own discretion although a justification will be provided.

{% hint style="danger" %}
Manually manipulating and/or provoking issue through DDoS will not be recognized as a qualified vulnerability.
{% endhint %}

## Assessing the severity of vulnerabilities

### Common Vulnerability Scoring System

To assess the severity of a given vulnerability, we rely on the use of the [Common Vulnerability Scoring System \(CVSS\)](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator) which is a free and open industry standard. When filling a vulnerability report, one can also provide a self-assessment of the scoring. However, the final assessment of the vulnerability remains at the discretion of the team.

### Vulnerability Compensation

| **Severity** | **Critical** | **High** | **Moderate** | **Low** | **Very Low** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **CVSS** | &gt; 8.0 | 6.0 - 8.0 | 4.0 - 6.0 | 2.0 - 4.0 | 0 - 2.0 |
| **Minimum** | $500 | $250 | $100 | $25 | $5 |
| **Maximum** | $1,000 | $500 | $250 | $100 | $25 |

## Vulnerability submission process and terms

### Submission process

When a vulnerability has been identified, **open an issue or a pull request on the related GitHub repository with the following information**:

1. A title summarizing the issue
2. A detailed description of the vulnerability
3. Steps to reproduce the vulnerability
4. Any code that could support the understanding of the issues \[optional\]
5. What are the potential impacts of the vulnerability
6. Suggested fix \[optional\]

{% hint style="warning" %}
If the vulnerability is considered critical \(CVSS&gt;8.0, leading to monetary supply generation or from reporter self-assessment\), please send the report directly to **security@xcash.foundation**, we will assess the severity and advise if a non-disclosure period is required.
{% endhint %}

### Confirmation process

Once the vulnerability has been confirmed, accepted and assessed by the team, a final report will be created and shared. The report will be also added to the bug bounty tracker, unless a non-disclosure period is specified.

### Payment process

The final payment will be processed in the next 7 business days following the issue's acceptation. All payments are processed in XCASH unless specified otherwise using the spot market rate with a floor at 0.00002 USD/XCASH. Bounties are paid until the full depreciation of bounty pool allocation. The initial funding for the bug bounty pool has been set at **500,000,000 XCASH** and will be reviewed on a regular basis.

## Additional legal terms

The X-Cash team remains in the right to ask for additional identification details in order to be able to process the payment. Please note you will qualify for a reward only if you were the first person to alert us to a previously unknown flaw. We will update you on the progress of your report when it is accepted, validated, fixed and when the bounty is paid.

This is not a competition, but rather an experimental and discretionary reward program and the X-Cash team remains in the right to cancel it at any time and the decision as to whether or not to pay a reward has to be entirely at the team discretion. Any testing must not violate any law, or disrupt or compromise any data that is not the hacker's own.

Any activities conducted in a manner consistent with this policy will be considered authorized conduct and the X-Cash Foundation will not initiate legal action against the hacker. If legal action is initiated by a third party against the hacker or in connection with activities conducted under this policy, the X-Cash Foundation will take steps to make it known that actions were conducted in compliance with this policy.

