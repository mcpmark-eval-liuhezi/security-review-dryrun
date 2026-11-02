# Modern Classics — Quarterly Security Digest: Library & Supply-Chain Bugs Every Developer Should Know

## Apache Log4j "Log4Shell" — CVE-2021-44228

- **CVE:** CVE-2021-44228 (disclosed December 2021)
- **GitHub Advisory Database ID:** GHSA-jfh8-c2jp-5v3q (Reviewed; published to the Advisory Database Dec 10, 2021)
- **Severity / CVSS:** Critical — CVSS 3.1 base score **10.0** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H)
- **First fixed version:** **2.15.0** shipped the first fix; **2.16.0** fully remediates (disables JNDI lookups by default and removes message-lookup support). Backports: 2.3.1, 2.12.2, 2.12.3. Log4j 1.x is EOL and uncorrected.
- **Disclosure year:** 2021 (Dec 10, 2021)
- **Authoritative link:** https://github.com/advisories/GHSA-jfh8-c2jp-5v3q (advisory page; NVD: https://nvd.nist.gov/vuln/detail/CVE-2021-44228)
- **Logo:** ![Apache Log4j](https://upload.wikimedia.org/wikipedia/commons/f/f9/Apache_Log4j_Logo.png)

Log4j versions prior to 2.16.0 are subject to remote code execution via the LDAP JNDI parser. Log4j 2.15.0 contained an earlier fix, but that patch did not disable attacker-controlled JNDI lookups in all situations (tracked additionally as CVE-2021-45046). Only the `org.apache.logging.log4j:log4j-core` package is directly affected.

**What to tell your team:** If you ship Log4j, pin to ≥2.16.0 (2.15.0 is a partial fix) and set `log4j2.formatMsgNoLookups=true` or remove the JndiLookup class as an interim mitigation — untrusted log input is an RCE vector, period.

## OpenSSL "Heartbleed" — CVE-2014-0160

- **CVE:** CVE-2014-0160 (disclosed 2014)
- **GitHub Advisory Database ID:** GHSA-w8r8-w5w4-4w4v (Unreviewed; published to the Advisory Database May 13, 2022; NVD-sourced)
- **Severity / CVSS:** High — CVSS 3.1 base score **7.5** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)
- **First fixed version:** **OpenSSL 1.0.1g** (the 1.0.1 branch fix; 1.0.2-alpha/beta/1.0.2pre also fixed prior to the 1.0.2 stable release in April 2014). Affects OpenSSL 1.0.1 before 1.0.1g.
- **Disclosure year:** 2014 (Apr 7, 2014)
- **Authoritative link:** https://github.com/advisories/GHSA-w8r8-w5w4-4w4v (advisory page; NVD: https://nvd.nist.gov/vuln/detail/CVE-2014-0160; vendor: https://www.openssl.org/news/secadv_20140407.txt)
- **Logo:** ![OpenSSL](https://upload.wikimedia.org/wikipedia/commons/a/a1/OpenSSL_logo.png)

The TLS and DTLS implementations in OpenSSL 1.0.1 before 1.0.1g do not properly handle Heartbeat Extension packets, allowing remote attackers to obtain sensitive information from process memory via crafted packets that trigger a buffer over-read (e.g., reading private keys).

**What to tell your team:** Any OpenSSL 1.0.1 or pre-1.0.2stable release leaks memory contents via malformed heartbeat requests — upgrade to 1.0.1g or 1.0.2+, and audit TLS stacks in embedded/legacy products.

## GNU Bash "Shellshock" — CVE-2014-6271

- **CVE:** CVE-2014-6271 (disclosed 2014)
- **GitHub Advisory Database ID:** GHSA-6hfc-grwp-2p9c (Unreviewed; published to the Advisory Database May 13, 2022; NVD-sourced)
- **Severity / CVSS:** Critical — CVSS 3.1 base score **9.8** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)
- **First fixed version:** **Bash 4.3** (fixed in patch 4.3p2; 4.4 also safe). Note: the original fix was incomplete — a follow-on advisory (GHSA-6493-28fj-f93w, CVE-2014-7169) covers the residual flaw fixed in 4.3p5/4.4.
- **Disclosure year:** 2014 (Sep 24, 2014)
- **Authoritative link:** https://github.com/advisories/GHSA-6hfc-grwp-2p9c (advisory page; NVD: https://nvd.nist.gov/vuln/detail/CVE-2014-6271)
- **Logo:** ![GNU Bash](https://raw.githubusercontent.com/odb/official-bash-logo/master/assets/Logos/Icons/PNG/512x512.png)

GNU Bash through 4.3 processes trailing strings after function definitions in the values of environment variables, allowing remote attackers to execute arbitrary code via a crafted environment (e.g., ForceCommand in OpenSSH sshd, mod_cgi/mod_cgid in Apache HTTP Server, DHCP client scripts).

**What to tell your team:** Exported environment variables are not sandboxed in Bash — anything set across a privilege boundary (sudo, sshd ForceCommand, web-server CGI) can carry function-like trailing content. Update to bash ≥4.3p2 and treat environment renewal as a security boundary.

## xz utils backdoor — CVE-2024-3094

- **CVE:** CVE-2024-3094 (disclosed 2024)
- **GitHub Advisory Database ID:** GHSA-rxwq-x6h5-x525 (Unreviewed; published to the Advisory Database Mar 29, 2024; NVD-sourced)
- **Severity / CVSS:** Critical — CVSS 3.1 base score **10.0** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H)
- **First fixed version:** **xz 5.6.2** (upstream; 5.6.0 and 5.6.1 are affected). The backdoor was removed in 5.6.2; distributions shipped backported fixes earlier.
- **Disclosure year:** 2024 (Mar 29, 2024)
- **Authoritative link:** https://github.com/advisories/GHSA-rxwq-x6h5-x525 (advisory page; NVD: https://nvd.nist.gov/vuln/detail/CVE-2024-3094; CISA alert: https://www.cisa.gov/news-events/alerts/2024/03/29/reported-supply-chain-compromise-affecting-xz-utils-data-compression-library-cve-2024-3094)
- **Logo:** ![XZ Utils](https://upload.wikimedia.org/wikipedia/commons/6/67/XZ_logo_contributed_by_Jia_Tan.png)

Malicious code was discovered in the upstream tarballs of xz starting with version 5.6.0. The tarballs included extra .m4 files with automake instructions that did not exist in the repository; through obfuscation they extract a prebuilt object file from a test archive and modify specific functions while building liblzma, which is then used by software such as sshd.

**What to tell your team:** Trusting built artifacts from a compromised upstream tarball is the failure mode — verify checksums and GPG signatures on source releases, prefer manufacturer backports, and assume any liblzma built from 5.6.0–5.6.1 is hostile.
