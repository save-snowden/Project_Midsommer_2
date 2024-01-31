<h1 id="cross-site-scripting-xss-attack-originating-from-update-profile-improper-file-upload-validation">Cross-Site Scripting (XSS) attack originating from /update-profile (improper file upload validation)</h1>
<h3 id="severity-high">Severity: High</h3>
<h2 id="issue-description">Issue Description</h2>
<p>Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted web sites. XSS attacks occur when an attacker uses a web application to send malicious code, generally in the form of a browser side script, to a different end user. The consultant identified that the update profile picture is vulnerable to cross site scripting, it is possible to upload an image with a MIME type of <strong>text\html</strong> this is then stored on the user’s profile as an XSS payload, the outline below demonstrates the steps taken to exploit and reproduce.</p>
<h2 id="risk-rating">Risk Rating</h2>
<ul>
<li>
<p>Risk: High</p>
</li>
<li>
<p>Difficulty to Exploit: Medium</p>
</li>
<li>
<p>Authentication : No</p>
</li>
</ul>
<h2 id="affected-urls">Affected URLs</h2>
<ul>
<li>
<p><a href="https://example.com/update-profile">https://example.com/update-profile</a></p>
</li>
<li>
<p><a href="https://example.com/file/upload">https://example.com/file/upload</a></p>
</li>
</ul>
<h2 id="steps-to-reproducepoc">Steps to Reproduce/PoC</h2>
<p>The following steps indicate a proof of concept outlined in three(3) steps to reproduce and execute the issue.</p>
<p><strong>Step 1:</strong></p>
<p>Navigate to <a href="https://example.com/update-profile">https://example.com/update-profile</a> and select edit as shown in screenshot attached labelled step1.jpg.</p>
<p><strong>Step 2:</strong></p>
<p>Modify the profile image request with a local proxy, in this case the consultant is using Burp Suite. Change the Content type from image to text/html as shown in the post request:</p>
<p><a href="https://ibb.co/pLQRMDJ"><img src="https://i.ibb.co/crwCZ5D/Screenshot-2024-01-31-011149.png" alt="Screenshot-2024-01-31-011149" border="0"></a></p>
<p>When this is sent, the following response is shown:</p>
<p><a href="https://ibb.co/DWvMhnN"><img src="https://i.ibb.co/r0zpYJg/Screenshot-2024-01-31-011243.png" alt="Screenshot-2024-01-31-011243" border="0"></a></p>
<p><strong>Step 3:</strong></p>
<p>The file has been uploaded to Application X and is hyperlinked to from the profile page as shown in step 3.jpg. By simply following the link to the image which in this case is:</p>
<p><a href="https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg">https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg</a></p>
<p>The payload is executed as shown in attached screenshot labelled step3.jpg, thus this demonstrates the issue is stored cross site scripting.</p>
<h2 id="affected-demographic">Affected Demographic</h2>
<p>This issue will affect all users on the site who view the profile of the attacker, when the image is rendered the payload is executed instead of a profile image. Additionally when the malicious user posts anything on the forums the payload will execute.</p>
<h2 id="recommendation">Recommendation</h2>
<p>Insure that file upload checks the MIME type of content being uploaded, for additional security implement server side content checking to ensure file headers match that of the file extension. Additionally make sure that all user input is treated as dangerous do not render any HTML tags.</p>
<h2 id="references">References</h2>
<p>For more information on remediation steps check out reference <a href="https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)">[2]</a>.</p>
<ul>
<li>
<p>[1] <a href="https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)">OWASP XSS Explained</a></p>
</li>
<li>
<p>[2] <a href="https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet">OWASP XSS Prevention Cheat Sheet</a></p>
</li>
</ul>

