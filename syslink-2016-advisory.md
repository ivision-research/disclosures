# SysLINK M2M Modular Gateway contains multiple vulnerabilities


Carve Systems consultants performed, and continue to perform, research on a number of IoT devices. The Carve Systems team coordiantes its disclosure with vendors when at all possible. This advisory concerns SysLINK M2M Modular Gateway. The consultants discovered numerous security findings and disclosed these findings to the vendor.

This advisory covers three CVEs:

- [CVE-2016-2331](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-2331) – Default password
- [CVE-2016-2332](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-2332) – Unauthenticated remote arbitrary command execution
- [CVE-2016-2333](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-2333) – Hardcoded encryption keys

[Published via CERT Coordination Center](https://www.kb.cert.org/vuls/id/822980)

Vulnerability Note VU#822980
Original Release Date: 2016-04-22 | Last Revised: 2016-04-22

## Overview

The SysLINK SL-1000 M2M (Machine-to-Machine) Modular Gateway contains multiple vulnerabilities.

## Description

According to the researcher, the SysLINK SL-1000 M2M Modular Gateway contains multiple vulnerabilities:

CWE-259: Use of Hard-coded Password - CVE-2016-2331

By default, the device's web interface uses a default password across all devices and does not prompt the administrator to change that password.

CWE-77: Improper Neutralization of Special Elements used in a Command ('Command Injection') - CVE-2016-2332

The device's web interface runs as the root user and is vulnerable to command injection via an authenticated POST request to flu.cgi. The parameter "5066" (dnsmasq) is vulnerable to injection. Commands are constructed and run as the root user.

CWE-321: Use of Hard-coded Cryptographic Key - CVE-2016-2333

The device uses an encryption key that is hard-coded and believed to be static across the entire device population.

The CERT/CC has been unable to confirm these vulnerabilities with the vendor. It is also unclear if models other than the SL-1000 are affected.

## Impact

An unauthenticated remote attacker with knowledge of the password may obtain root access to the device.

## Solution

Apply an update

According to the reporter, affected users should update to firmware version 01A.8 which addresses these issues. CERT/CC has reached out to Systech to confirm this information.

Additionally, affected users may consider the following workarounds and mitigations:

Restrict Network Access

As a general good security practice, only allow connections from trusted hosts and networks. Consult your firewall product's manual for more information.

CVSS Metrics
Group   Score   Vector
Base    10  AV:N/AC:L/Au:N/C:C/I:C/A:C
Temporal    9.5     E:F/RL:U/RC:C
Environmental   7.1     CDP:ND/TD:M/CR:ND/IR:ND/AR:ND

## Acknowledgements

Thanks to Roman Faynberg and Jeremy Allen of Carve Systems for reporting this vulnerability.

This document was written by Garret Wassermann. 

## Disclosure Timeline

- 5/6/15 Reported to CERT/CC
- 5/11/15 Received response from US CERT asking us to contact the vendor directly
- 5/12/15 Emailed vendor directly
- 3/4/16 CERT/CC assigns CVEs (CVE-2016-2331, CVE-2016-2332, CVE-2016-2333)
- 3/24/16 Systech confirms fixes; ships new unit to Carve for testing
- 4/22/16 CERT/CC publishes http://www.kb.cert.org/vuls/id/822980

