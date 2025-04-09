---
title: "Content Security Policy"
url: /howto/security/csp/
weight: 80
description: By employing a content security policy (CSP) in your app, you can protect it from malicious content which might try to take advantage of the app's trusted web page context.
aliases:
    - /howto/security/using-mobile-capabilities/csp/
---

## Introduction

By employing a content security policy (CSP) in your app, you can protect it from malicious content which might try to take advantage of the app's trusted web page context. A rigorous CSP allows you to control which resources are loaded in the app.

A web app (including progressive web apps) can be made more strict and secure by setting its CSP to `default-src: self`. By doing so, only resources from the same domain can be loaded and no resources can be loaded inline (such as Base64 images or inline JavaScript).

For more background information on CSPs, see [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) from Mozilla.

{{% alert type="warning" %}}
Currently, some of Mendix's pluggable widgets are not fully compliant with CSP. If used with strict CSP, these widgets can result in CSP errors in the console or broken flows. Please refer to [widget's security documentation](/appstore/widgets/security/content-security-policy/) page for more details.
{{% /alert %}}

## Setup

In order to be able to use the strictest setting of a CSP (`default-src: self`) you must make some changes in your application. See the sections below for guidance.

### Updating the Theme Folder

To upgrade your theme directory to latest version, complete the following steps:

1. Rename your current theme directory. For example, you can use *theme_backup* as the new name.
1. Download the new theme files from this GitHub link: [theme.zip](https://github.com/mendix/atlas/releases/download/atlasui-theme-files-2024-01-25/atlasui-theme-files.zip). Extract the downloaded file into the root of your Mendix app folder. The folder structure should be similar to the previous folder, (meaning the Mendix app root, then the theme, and then the web and native folders).
1. After extracting the new theme files, restore your custom styling from the backup by copying over the new theme folder files. You will see the main changes enacted to make things compatible with strict CSP involve the `login.html` file and one JavaScript file for the toggled password.

### Changing the Theme

Create a new file to contain the Dojo configuration in your theme folder (*theme/web/appSetup.js*) with the following configuration:

```js
window.dojoConfig = {
    // Default Dojo config
	isDebug: false,
	useCustomLogger: true,
	async: true,
	baseUrl: "mxclientsystem/dojo/",
	cacheBust: "{{cachebust}}",
	rtlDirect: "index-rtl.html",

    // CSP Dojo config
	has: {
        "csp-restrictions": true
    },
	blankGif: "mxclientsystem/dojo/resources/blank.gif"
};

if (!document.cookie || !document.cookie.match(/(^|;) *originURI=/gi))
	document.cookie = "originURI=/login.html" + (window.location.protocol === "https:" ? ";SameSite=None;Secure" : "");
```

Create a second file to contain the script for unsupported browsers (*theme/web/unsupported-browser.js*):

```js
// Redirect to unsupported browser page if opened from browser that doesn't support Symbols
if (typeof Symbol !== "function") {
    var homeUrl = window.location.origin + window.location.pathname;
    var appUrl = homeUrl.slice(0, homeUrl.lastIndexOf("/") + 1);
    window.location.replace(appUrl + "unsupported-browser.html");
}
```

Finally, the *theme/web/index.html* file needs to be changed to use these files directly. If you lack this file, please follow the [Customizing index.html (Web)](/howto/front-end/customize-styling-new/#custom-web) section of *Customize Styling*.

In *theme/web/index.html* do the following:

1. Remove the line with the `{{unsupportedbrowsers}}` tag
1. Remove the `<script>` tag with the `dojoConfig` inside
1. At the top of the `<head`> tag, add a reference to the `unsupported-browser.js` script:

    ```js
    <html>
        <head>
            <script src="unsupported-browser.js"></script>
            ...
        </head>
        ...
    </html>
    ```

1. In the `<body>` tag, add a reference to the `appSetup.js` script before `mxui.js` is loaded:

    ```js
    <html>
        <body>
            ...
            <div id-"content"></div>
            <script src="appSetup.js"></script>
            <script src="mxclientsystem/mxui/mxui.js?{{cachebust}}"></script>
        </body>
    </html>
    ```

Lastly, ensure you are not using any external fonts by checking your theme's styling to confirm all of the fonts are loaded locally.

#### Testing Your Changes Locally

To check that your changes are working locally, you can add a custom `Content-Security-Policy` header in your [configuration](/refguide/configuration/#headers).

After redeploying your app locally, it should function as normal. If your app does not load or if there are errors, check that you have completed all steps listed above.

After you finish testing locally, remember to remove the line of code in the `head` tag.

### Enabling the Header in the Cloud

In Mendix v10.12 we have added a new way of setting the CSP header. It can now be set using the *Headers* custom runtime setting. This is the recommended way of setting the CSP header. The old way of setting the CSP header using the *HTTP Headers* is deprecated and will be removed in the future. A JSON configuration can be used containing the CSP header and its value. The JSON configuration should look like this:

```json
{
  "Content-Security-Policy": "default-src 'self';"
}
```

In versions before 10.12, follow the instructions in the [HTTP Headers](/developerportal/deploy/environments-details/#http-headers) section of *Environment Details*.

### Nonces

The CSP header can also contain nonces. Nonces are used to allow specific inline scripts to run while still maintaining a strict CSP. We only recommend using nonces for experts who are familiar with CSP and its implications.

{{% alert type="warning" %}}
Only supported using the `Headers` custom runtime setting. The `HTTP Headers` setting does not support nonces.
{{% /alert %}}

To use nonces, all HTML files in your theme folder need to be updated to include the nonce template tag for `<style>` and `<script>` tags. For example:

```html
<script
  src="mxclientsystem/mxui/mxui.js?{{cachebust}}"
  nonce="{{NONCE}}"
></script>
```

The header itself also needs to be updated to include the nonce. The header should look like this:

```json
{
  "Content-Security-Policy": "default-src 'self'; script-src 'self' 'nonce-{{NONCE}}';"
}
```

## CSP for your marketplace module

If you are creating a marketplace module in which you are serving HTML files, you likely want to ensure your module uses the proper CSP headers and tags. For this, we provide the following runtime APIs from Mx11 onwards:

* `Configuration#getHeader`, can be used to get the current CSP header, e.g. `Core.getConfiguration().getHeader("Content-Security-Policy")`. Use this to get the template for the CSP header. You can also use this to determine whether you need to template nonces in your HTML files.
* `IMxRuntimeResponse#addContentSecurityPolicy`, can be used to add the configured CSP header to the response.
* `IMxRuntimeResponse#getNonce`, used to get a unique nonce for the current response. This can be used to template the nonce in your HTML files.
