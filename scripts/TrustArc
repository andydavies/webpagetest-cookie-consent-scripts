# TrustArc

TrustArc sets the following cookies:

- TAconsentID
- notice_preferences
- cmapi_cookie_privacy
- cmapi_gtm_bl
- notice_gdpr_prefs

These can be set in WebPageTest via the Advanced Settings → Script → "Enter Script" text field.

The DevTools snippet below can be used extracts relevant data from an existing Chrome session and then generate an appropriate script.

I'd recommend generating the script using a fresh Guest profile in Chrome first to avoid capturing anything that may be personal.

1. Launch fresh Guest window in Chrome

2. Open a site that uses TrustArc as it's CMP

3. Accept cookies (could reject, or experiment with different options too)

4. Launch DevTools

Either paste script below into the console or save it as a devtools snippet, and then execute it


```
// Cookies
var cookies = document.cookie.split('; ');
var TAconsentID = cookies.find(entry => entry.startsWith('TAconsentID='));
var notice_preferences = cookies.find(entry => entry.startsWith('notice_preferences='));
var cmapi_cookie_privacy = cookies.find(entry => entry.startsWith('cmapi_cookie_privacy='));
var cmapi_gtm_bl = cookies.find(entry => entry.startsWith('cmapi_gtm_bl='));
var notice_gdpr_prefs = cookies.find(entry => entry.startsWith('notice_gdpr_prefs='));

// Generate WPT Script
var output = 
 `setCookie %ORIGIN% ${TAconsentID}\n` +
 `setCookie %ORIGIN% ${notice_preferences}\n` +
 `setCookie %ORIGIN% ${cmapi_cookie_privacy}\n` +
 `setCookie %ORIGIN% ${cmapi_gtm_bl}\n` +
 `setCookie %ORIGIN% ${notice_gdpr_prefs}\n` +
 `navigate %URL%`;

console.log(output);

copy(output);
```

6. The script will generate another script that looks something like this (it will also copy it to the clipboard) 

```
setCookie %ORIGIN% TAconsentID=3851e98e-1622-4fc4-926b-574bf940ba44
setCookie %ORIGIN% notice_preferences=3:
setCookie %ORIGIN% cmapi_cookie_privacy=permit 1,2,3,4
setCookie %ORIGIN% cmapi_gtm_bl=
setCookie %ORIGIN% notice_gdpr_prefs=0,1,2,3::expressed,eu
navigate %URL%
```

7. In WebPageTest…

8. Enter the page you want test

9. Paste the generated script into Advanced Settings → Script → "Enter Script" text box

8. Hit start

If all's gone well when the test completes the filmstrip shouldn't contain a consent popup, and the waterfall should have all the third-parties and ads (although some third-parties and ad providers don't serve their content to AWS locations etc)

