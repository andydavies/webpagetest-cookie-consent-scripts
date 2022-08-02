# LiveRamp Privacy Manager

Bypassing the LiveRamp consent prompt requires a number of localStorage values to be set:

- euconsent-v2
- cconsent-v2
- addtl_consent
- gdpr-dau
- gdpr-last-interaction
- gdpr-config-version
- gdpr-auditId
- geo-location

These can be set near the start of a test in WebPageTest via the _Inject Script_ field at the bottom of the _Advanced_ tab.

The DevTools snippet below can be used extracts relevant data from an existing Chrome session and then generate an appropriate script.

I'd recommend generating the script using a fresh Guest profile in Chrome first to avoid capturing anything that may be personal.

1. Launch fresh Guest window in Chrome

2. Open a site that uses LiveRamp as its CMP (look for `*.privacymanager.io`)

3. Accept cookies (could reject, or experiment with different options too)

4. Launch DevTools

5. Either paste script below into the console or save it as a devtools snippet, and then execute it

```
const items = ["euconsent-v2","cconsent-v2","addtl_consent","gdpr-dau","gdpr-last-interaction","gdpr-config-version","gdpr-auditId","geo-location"]
const commands = items.map(key=>`localStorage.setItem('${key}','${localStorage.getItem(key)}');`).join('\n');

console.log(commands);
copy(commands);
```

6. The script will generate another script that looks something like this (it will also copy it to the clipboard)

```
localStorage.setItem('euconsent-v2','{"data":"...","expire":1693128301069}');
localStorage.setItem('cconsent-v2','{"data":"...","expire":1693128301078}');
localStorage.setItem('addtl_consent','{"data":"...","expire":1693131139654}');
localStorage.setItem('gdpr-dau','{"data":true,"expire":1659518701769}');
localStorage.setItem('gdpr-last-interaction','{"data":1659432301.061,"expire":1693128301061}');
localStorage.setItem('gdpr-config-version','{"data":25,"expire":1693128301061}');
localStorage.setItem('gdpr-auditId','{"data":"...","expire":1693128289025}');
```

7. In WebPageTest…

8. Enter the page you want test

9. Paste the generated script into the _Inject Script_ text box under the _Advanced_ tab

8. Hit start

If all's gone well when the test completes the filmstrip shouldn't contain a consent popup, and the waterfall should have all the third-parties and ads (although some third-parties and ad providers don't serve their content to AWS locations etc)

Here's two tests using https://www.ibtimes.co.uk/  

Without injected script: https://www.webpagetest.org/result/220802_BiDc7F_7H5/
With script: https://www.webpagetest.org/result/220802_BiDcTM_7HB/1

Comparison: https://www.webpagetest.org/video/compare.php?tests=220802_BiDcTM_7HB,220802_BiDc7F_7H5
