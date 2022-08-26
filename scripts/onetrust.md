# [OneTrust](https://www.onetrust.com/)

Bypassing the OneTrust cookie prompt seems to require the following to be set:

**Cookies**
- OptanonConsent
- OptanonAlertBoxClosed

These can be set in WebPageTest via the Advanced Settings → Script → "Enter Script" text field.

The DevTools snippet below can be used extracts relevant data from an existing Chrome session and then generate an appropriate script.

I'd recommend generating the script using a fresh Guest profile in Chrome first to avoid capturing anything that may be personal.

1. Launch fresh Guest window in Chrome

2. Open a site that uses OneTrust as it's CMP

3. Accept cookies (could reject, or experiment with different options too)

4. Launch DevTools

5. Either paste script below into the console or save it as a devtools snippet, and then execute it


```
// Cookies
var cookies = document.cookie.split('; ');
var OptanonConsent = cookies.find(entry => entry.startsWith('OptanonConsent='));
var OptanonAlertBoxClosed = cookies.find(entry => entry.startsWith('OptanonAlertBoxClosed='));

// Generate WPT Script
var output = 
 `setCookie %ORIGIN% ${OptanonConsent}\n` +
 `setCookie %ORIGIN% ${OptanonAlertBoxClosed}\n` +
 `navigate %URL%`;

console.log(output);

copy(output);
```

6. The script will generate another script that looks something like this (it will also copy it to the clipboard) 

```
setCookie https://%HOST% OptanonConsent=isIABGlobal=false&datestamp=Sat+May+22+2021+15%3A42%3A12+GMT%2B0200+(Central+European+Summer+Time)&version=6.15.0&hosts=&consentId=ab1fcb80-a45b-49ca-b5f4-357887f51f7a&interactionCount=1&landingPath=NotLandingPage&groups=C0001%3A1%2CC0002%3A1%2CC0003%3A1%2CC0004%3A1&geolocation=DE%3BBY&AwaitingReconsent=false
setCookie https://%HOST% OptanonAlertBoxClosed=2021-05-04T13:11:41.750Z
navigate %URL%
```

7. In WebPageTest…

8. Enter the page you want test

9. Paste the generated script into Advanced Settings → Script → "Enter Script" text box

8. Hit start

If all's gone well when the test completes the filmstrip shouldn't contain a consent popup, and the waterfall should have all the third-parties and ads (although some third-parties and ad providers don't serve their content to AWS locations etc)

Here's two tests using https://www.netcentric.biz/:
* Without injected script: https://www.webpagetest.org/result/210522_AiDc73_c3abaa493cdc36627a250180c4f8324e/
* With script: https://www.webpagetest.org/result/210522_BiDcFG_64d6f46c9c451e6b751b11d8c1c88c07/
* Comparison: https://www.webpagetest.org/video/compare.php?tests=210522_BiDcFG_64d6f46c9c451e6b751b11d8c1c88c07,210522_AiDc73_c3abaa493cdc36627a250180c4f8324e

