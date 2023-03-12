# Usercentrics

Bypassing the Usercentrics cookie prompt seems to require the following to be set:

**localStorage**
- uc_settings
- uc_user_interaction

These can be set near the start of a test in WebPageTest via the _Inject Script_ field at the bottom of the _Advanced_ tab.

The DevTools snippet below can be used extracts relevant data from an existing Chrome session and then generate an appropriate script.

I'd recommend generating the script using a fresh Guest profile in Chrome first to avoid capturing anything that may be personal.

1. Launch fresh Guest window in Chrome

2. Open a site that uses Usercentrics as it's CMP

3. Accept cookies (could reject, or experiment with different options too)

4. Launch DevTools

5. Either paste script below into the console or save it as a devtools snippet, and then execute it


```
// Local storage items
var UCSettings = localStorage.getItem('uc_settings');
var UCUserInteraction = localStorage.getItem('uc_user_interaction');

// Generate WPT Script
var output = `localStorage.setItem('uc_settings', '${UCSettings}');\n` +
 `localStorage.setItem('uc_user_interaction', '${UCUserInteraction}');\n`;

console.log(output);

copy(output);
```

6. The script will generate another script that looks something like this (it will also copy it to the clipboard)

7. In WebPageTest…

8. Enter the page you want test

9. Paste the generated script into the _Inject Script_ text box

8. Hit start

If all's gone well when the test completes the filmstrip shouldn't contain a consent popup, and the waterfall should have all the third-parties and ads (although some third-parties and ad providers don't serve their content to AWS locations etc)

Here's two tests using https://www.baywa.de/ 

Without injected script: https://www.webpagetest.org/result/230312_AiDc21_5P8/ 
With script: https://www.webpagetest.org/result/230312_AiDc99_5N6/

Comparison: https://www.webpagetest.org/video/compare.php?tests=230312_AiDc21_5P8,230312_AiDc99_5N6
