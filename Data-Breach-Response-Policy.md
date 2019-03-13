This document covers Restyled's plan for responding to unauthorized exposure of sensitive data.

## Ownership and Responsibility

As Restyled is a single-developer project, all matters will be handled by the following contact:

- Patrick Brisbin <support@restyled.io>, <pbrisbin@gmail.com>
- PGP Key: [C35EC89C33868FEC](https://files.pbrisbin.com/public.key)

## Responsible Disclosure

Any parties who suspect a vulnerability or data breach should send an encrypted email to the above address.

## Sensitive Data

The following is data that Restyled stores and considers sensitive. If any such data becomes accessible to an unauthenticated / unauthorized user, that will be considered a _Data Breach_:

- Private repository contents (i.e. source code), whole or in "diff" form
- Private user email addresses
- Any secrets or tokens which grant access to the above

**NOTE**: Repository names, commit shas, Pull Request numbers, Pull Request descriptions, and GitHub user or organization names are not considered sensitive for the purposes of this response policy.

## Expectations

In the event of a Data Breach,

- All running services will be stopped to prevent further access
- Impacted users will be notified by email within 48 hours
- security@github.com will be notified within 48 hours

## Non-Data-Breach Vulnerabilities

Restyled routinely monitors for patched vulnerabilities in the software and infrastructure it depends on. Most such vulnerabilities are not at risk of causing a Data Breach. In such cases, they will be patched without user-notification or data-breach response activities and listed below.

| Description | Action |
| --- | --- |
| [CVE-2019-5736](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) runc through 1.0-rc6, as used in Docker before 18.09.2 and other products, allows attackers to overwrite the host runc binary | Re-created restyle machines with docker-18.09.2 |


