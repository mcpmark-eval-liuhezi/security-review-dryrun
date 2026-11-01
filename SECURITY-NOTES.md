# Log4Shell Security Notes (CVE-2021-44228)

## Summary

Log4Shell is a critical remote code execution vulnerability in Apache Log4j 2, an widely used Java logging framework. It was discovered by Chen Zhaojun of the Alibaba Cloud Security Team and disclosed in December 2021. An attacker who can control log messages or log message parameters can exploit JNDI features to execute arbitrary code loaded from LDAP and other JNDI-related endpoints.

## CVE Identifier

- **CVE-2021-44228** — JNDI lookup can be exploited to execute arbitrary code loaded from an LDAP server
- Related CVEs: CVE-2021-45046, CVE-2021-45105, CVE-2021-44832

## Severity

- **CVSS 3.x Score: 10.0 CRITICAL** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H) — per NVD
- Some sources (e.g., AWS桿 AWSLinux advisories) cite CVSS 3.1 9.8 (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H); NVD's official base score is 10.0.
- CVSS v2: 9.3 (AV:N/AC:M/Au:N/C:C/I:C/A:C)

## Affected Versions

AffectedLog4j 2 versions (per Apache Logging Security page):

- `[2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0)`
- Inclusive range commonly stated: **2.0-beta9 through 2.14.1**
- Only the `log4j-core` JAR is impacted; applications using only `log4j-api` without `log4j-core` are not affected.

## Fixed Versions

- **2.15.0** for Java 8 and later (addresses CVE-2021-44228; fix incomplete in non-default configurations — CVE-2021-45046)
- **2.16.0** for Java 8+ (addresses CVE-2021-45046)
- **2.17.1** for Java 8+ (recommended final fix; also addresses CVE-2021-45105 and CVE-2021-44832)
- 2.12.4 for Java 7
- 2.3.2 for Java 6

## Remediation Summary

1. **Upgrade** log4j-core to >= 2.17.1 (Java 8+), >= 2.12.4 (Java 7), or >= 2.3.2 (Java 6). Use dependency constraints to prevent resolution of vulnerable versions.
2. **Detect** vulnerable deployments via `mvn dependency:tree | grep log4j`, Gradle dependencies report, `snyk log4shell`, OWASP Dependency-Check, Trivy, and GitHub Dependabot.
3. **Hardening** if upgrade is not immediate: set `log4j2.formatMsgNoLookups=true` (Log4j 2.10.0+), or remove `JndiLookup.class` from the classpath (`zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class`) and restart the JVM. JNDI lookups are limited to the `java:` protocol by default since 2.16.0.
4. **Automated scanning**: enable Dependabot alerts, OWASP Dependency-Check in CI (fail-on-severity=Critical), Trivy for container images, and Snyk for transitive dependency detection.

## Sources

- NVD CVE-2021-44228: https://nvd.nist.gov/vuln/detail/cve-2021-44228
- Apache Logging Services Security Page: https://logging.apache.org/security.html
- AWS Linux Advisory (CVSS details): https://explore.alas.aws.amazon.com/CVE-2021-44228.html
- Snyk Log4Shell Remediation Cheat Sheet: https://snyk.io/blog/log4shell-remediation-cheat-sheet/
- Sonatype Find and Fix Log4j: https://help.sonatype.com/en/find-and-fix-log4j.html
- Gradle Blog: https://blog.gradle.org/log4j-vulnerability

## Tracking

This write-up supports the parent tracking issue "Log4Shell remediation checklist" (issue #5) with sub-issues for each remediation action.
