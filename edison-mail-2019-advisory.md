# Edison Mail 2019 Disclosure

The Carve Systems team coordinates its disclosure with vendors when at all possible. This advisory is for Edison Mail Client version:

- Android – 1.6.1
- iOS – 1.9.16

Carve Systems consultants found that the Edison Mail Client on both iOS and Android would execute arbitrary JavaScript stored inside of HTML emails. Furthermore, consultants also found that it was possible to leverage this functionality to read and upload files off of a user’s Android device and onto other – additional details can be found at: Abusing WebViews to Steal Files via Email.

# Disclosure Timeline

- 3/20/19 – Initial Contact sent via vendor’s contact page
- 3/27/19 – Vendor replies to Carve Systems team asking for details.
- 3/28/19 – Carve Systems shares initial advisory contents with Vendor.
- 3/29/19 – Vendor indicates that the issue will be resolved in the next release towards the end of April or early May.
- 6/3/19 – Carve Systems contacts vendor asking when fix will be implemented. Carve Systems is told that the iOS version has been fixed, but the android version has not.
- 6/4/19 – Carve Systems works with vendor to delay public posting for 30 days
- 6/17/19 – Vendor contacts Carve Systems stating that the issue has been resolved on Android.

Advisory Contents

Details about these findings were shared with Edison mail on March 28, 2019.

The slightly modified version of the initial email is shared below:
Versions Tested:

- Android version 1.6.1 (92)
- iOS 1.9.16 (828)

##Details

The Edison mail mobile client executes any JavaScript contained within the body of an email containing HTML content. This code execution vulnerability can be triggered with minimal user interaction. On iOS, the vulnerability requires that a user reply or forward the email – on occasions simply opening the email also triggers the payload; this is unreliable. However, Android will execute the remote code anytime the email body is rendered, as such opening an email, replying to an email, or forwarding an email will execute the code.

As an example consider the following HTML email:

EMAIL BODY
<script>
  alert("Edison Mail has detected your phone is out of storage, please visit http://fakesite.com to get more storage");
</script>

This email will create a normal looking email, but once the script tag’s code has been executed a popup will appear. Although benign, the above could be used to craft a convincing phishing campaign. For a more malicious example, see the following issue.
Replication Steps:

1. Using a service that allows sending of HTML emails, such as https://putsmail.com/tests/new, send the following example to an iOS or Android device using Edison Mail.

EMAIL BODY
  <script>
    alert("Edison Mail has detected your phone is out of storage, please visit http://fakesite.com to get more storage");
  </script>

2. On Android simply opening the email should trigger a pop up. On iOS open the email and attempt to reply or forward the email to trigger the popup.

Remote code execution can be leveraged to remotely read contents off a user’s devices.

Versions Tested: Android version 1.6.1 (92)

## Details

The remote code execution vulnerability discussed above is executed within a WebView on Android. This WebView uses overly permissive settings, which allow an attacker to not only execute code within the WebView, but also read the contents of files on the device. As the WebView is running under the Edison mail application, it has access to both files stored on the user’s external storage as well as the application’s internal storage.

As an example consider the following:

EMAIL BODY
<script>
  function alertFile(filepath){
    var fileRequest = new XMLHttpRequest();
    fileRequest.onreadystatechange = function(){
      alert(this.responseText);
    }
    fileRequest.open("GET", filepath, false);
    fileRequest.send();
  }
  alertFile("file:///data/data/com.easilydo.mail/shared_prefs/EdoChatAccount.xml");
  alertFile("file:///sdcard/test.txt");
</script>

Note: In order to have this example completely function, a file on the SD card with the name test.txt must first be created.

The above example will only read and display the contents of the file to the user, however it is would be possible to modify the above code snippet to not only read the file’s content, but to also upload the file’s content to remote server. Additionally, in order for a user to retrieve files from a user’s device they must know the file path ahead of time. However, multiple files with known static paths exist. For example, if the data/data/com.easilydo.mail/files/Email.realm file is read and upload, an attacker could get a copy of all cached emails sent from/to the user.

Using a service that allows sending of HTML emails, such as https://putsmail.com/tests/new, send the following example to an iOS or Android device using Edison Mail.

EMAIL BODY
  <script>
    function alertFile(filepath){
      var fileRequest = new XMLHttpRequest();
      fileRequest.onreadystatechange = function(){
        alert(this.responseText);
      }
      fileRequest.open("GET", filepath, false);
      fileRequest.send();
    }
    alertFile("file:///data/data/com.easilydo.mail/shared_prefs/EdoChatAccount.xml");
    alertFile("file:///sdcard/test.txt");
  </script>

    On Android simply opening the email should trigger various popups. The first one may be blank.
    Hitting OK, should show the contents of the EdoChatAccount.xml file.
    If present, hitting OK should show the contents of the test.txt file.

