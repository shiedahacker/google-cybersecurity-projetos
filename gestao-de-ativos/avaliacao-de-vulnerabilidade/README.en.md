[Português](README.md) · **English**

# Vulnerability assessment of a publicly exposed database

> **Context:** Google Cybersecurity Professional Certificate (Assets, Threats, and Vulnerabilities course)
> **Skills:** vulnerability assessment, qualitative risk analysis, NIST SP 800-30 Rev. 1, security control recommendations

## Project description

In this activity, I was a newly hired analyst at an e-commerce company. The company stores its data on a remote database server, because many employees work from home in different countries and use this data to find new customers.

The problem is that this database has been open to the public since the company started, three years ago. My job was to write a vulnerability assessment report explaining the risks of this to the decision makers and what can be done to protect the server.

The completed report is in [`vulnerability-assessment-en.pdf`](vulnerability-assessment-en.pdf). There is also a Portuguese version in [`avaliacao-de-vulnerabilidade.pdf`](avaliacao-de-vulnerabilidade.pdf).

## The system

| Item | Detail |
|---|---|
| Hardware | Powerful CPU and 128 GB of memory |
| Operating system | Linux (latest version) |
| Database | MySQL |
| Network | IPv4, connected to other servers |
| Current security | SSL/TLS encrypted connections |
| Scope | System access controls, from June to August |

## Risk assessment

I used NIST SP 800-30 Rev. 1 to choose the threat sources and threat events. The risk score is likelihood times severity, each one from 1 to 3.

| Threat source | Threat event | Likelihood | Severity | Risk |
|---|---|---|---|---|
| Hacker | Obtain sensitive information via exfiltration | 3 | 3 | 9 |
| Employee | Alter/Delete critical information | 2 | 3 | 6 |
| Competitor | Conduct Denial of Service (DoS) attacks | 2 | 2 | 4 |

**Why these scores.** Data theft by a hacker got the highest score because the database is open on the internet, so it is very likely someone will try, and the damage would be big. Data changes by an employee are less likely but would also be serious, since right now there is no control over who can change or delete information. A DoS attack from a competitor would take the server down for a while, but the data would not be lost, so I gave it a lower severity.

## Remediation strategy

1. Remove public access to the database and only allow authorized connections, for example through a VPN.
2. Use multi-factor authentication (MFA) for all users.
3. Apply the principle of least privilege, so only the people who need it can change or delete data.
4. Keep SSL/TLS with valid certificates and use PKI.
5. Apply defense in depth with a firewall, IDS and access logging (AAA).

## What I learned

- One configuration mistake, like leaving the database open, can make many different attacks possible.
- A qualitative assessment depends a lot on the judgment of the person doing it. That is why it is important to explain in the report how I got to each score.
- Not every threat comes from outside. The risk of an employee deleting or changing data also needs to be in the analysis.
- For the report to be useful to managers, the recommendations need to be practical and connected to the risks I found.
