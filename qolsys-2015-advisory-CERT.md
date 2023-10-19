# Qolsys IQ Panel contains multiple vulnerabilities

[Published via CERT Coordination Center](https://www.kb.cert.org/vuls/id/573848)

Vulnerability Note VU#573848
Original Release Date: 2015-10-29 | Last Revised: 2015-10-29

## Overview

All firmware versions of Qolsys IQ Panel contain hard-coded cryptographic keys, do not validate signatures during software updates, and use a vulnerable version of Android OS.

## Description

Qolsys IQ Panel is an Android OS-based touch screen controller for home automation devices and functions. All firmware versions contain the following vulnerabilities.

CWE-321: Use of Hard-coded Cryptographic Key - CVE-2015-6032

Qolsys IQ Panel contains multiple hard-coded cryptographic keys. With these keys it may be possible for attackers to sign malicious code that would then be accepted as valid by affected devices.

CWE-347: Improper Verification of Cryptographic Signature - CVE-2015-6033

Qolsys IP Panel fails to properly validate cryptographic signatures for software updates before installing them. Malicious updates provided by an attacker may be accepted as valid by affected devices.

CWE-937: OWASP Top Ten 2013 Category A9 - Using Components with Known Vulnerabilities

Qolsys IP Panel uses an outdated version of Android OS with known vulnerabilities. An attacker may be able to leverage vulnerabilities affecting Android 2.2.1 to compromise affected devices.

The CVSS score below is for CVE-2015-6033.

## Impact

A remote, unauthenticated attacker may be able to inject malicious firmware or software updates that will be accepted as valid by affected devices. It may be possible to leverage known vulnerabilities affecting Android OS 2.2.1 compromise affected devices.

## Solution

The CERT/CC is currently unaware of a practical solution to this problem. The vendor has indicated that they will release QOL 1.5.1 to address these issues in November 2015, but until then, users should consider the following workaround.

Restrict access

As a general good security practice, only allow connections from trusted hosts and networks. Since the nature of these vulnerabilities means that malicious updates can be made to appear valid, users should consider disabling automatic updates altogether. Users should confirm the source of any update before applying it manually.

CVSS Metrics
Group 	Score 	Vector
Base 	7.6 	AV:N/AC:H/Au:N/C:C/I:C/A:C
Temporal 	6.8 	E:POC/RL:U/RC:C
Environmental 	5.1 	CDP:N/TD:M/CR:ND/IR:ND/AR:ND

## References

    http://www.qolsys.com/
    https://cwe.mitre.org/data/definitions/321.html
    https://cwe.mitre.org/data/definitions/347.html
    https://cwe.mitre.org/data/definitions/937.html
    https://www.cvedetails.com/vulnerability-list/vendor_id-1224/product_id-19997/version_id-103819/Google-Android-2.2.1.html

## Acknowledgements

Thanks to Roman Faynberg from Carve Systems for reporting this vulnerability.

This document was written by Joel Land.

## Other Information

CVE IDs: 	[CVE-2015-6032](https://nvd.nist.gov/vuln/detail/CVE-2015-6032), [CVE-2015-6033](https://nvd.nist.gov/vuln/detail/CVE-2015-6033)
Date Public: 	2015-10-29
Date First Published: 	2015-10-29
Date Last Updated: 	2015-10-29 16:14 UTC
Document Revision: 	23 