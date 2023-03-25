---
layout: pattern
title: How to add Richer Install UI
date: 2023-03-27
authors:
  - ajara
description: |
  Add an app store-like install experience to your app by adding the description and screenshots fields to your manifest.
height: 800
static:
  - sw.js
  - assets/manifest.json
  - icons/favicon.png
  - assets/screens/squoosh-riui-desktop.jpg
  - assets/screens/squoosh-riui-desktop1.jpg
  - assets/screens/squoosh-riui-phone.jpg
  - assets/screens/squoosh-riui-phone1.jpg
---

App stores provide a space for developers to showcase their apps before installation, with screenshots and text information that help the user make the choice to install the app. Richer install UI provides a similar space for web app developers to invite their users to install their app, directly from the browser. This enhanced UI is available in Chrome for Android and desktop environments.

TODO:ajara Add with and without image

To get the richer install UI dialog instead of the regular small default prompt, developers need to add the fields `description` and `screenshots` to their web manifest.

Below are the specific requirements and recommendations for those fields:

## Description

The `description` member describes the application in the installation prompt, to invite the user to keep the app. Only 300 characters are allowed in the description, and longer descriptions are truncated and an ellipsis is appended (as you can see in [this example](https://glitch.com/edit/#!/richerinstall-longer-description)).

```json
{
…
"description": "Compress and compare images with different codecs
right in your browser."
}
```

The description appears at the top of the installation prompt.

You may have noticed from the screenshots that installation dialogs also list the app's origin. Origins that are too long to fit the UI are truncated, this is also known as eliding and is used
as a [security measure to protect users](https://chromium.googlesource.com/chromium/src/+/master/docs/security/url_display_guidelines/url_display_guidelines.md#eliding-urls).

## Screenshots

Screenshots really add the 'richer' part to the new install ui and we strongly recommend their use. In your manifest you add the `screenshots` member, which takes an array that requires at least one image and Chrome will display up to eight. An example is shown below.

```json
 "screenshots": [
    {
     "src": "source/image1.gif",
      "sizes": "640x320",
      "type": "image/gif",
      "form_factor": "wide",
      "label": "Wonder Widgets"
    }
]
```

In practice that produces something like this:

Screenshots must follow these criteria:

- Width and height must be at least 320px and at most 3,840px.
- The maximum dimension can't be more than 2.3 times as long as the minimum dimension.
- All screenshots with the _same form factor_ value must have _identical aspect ratios_.
- Only JPEG and PNG image formats are supported.
- Only eight screenshots will be shown. If more are added, the user agent simply ignores them.

Also, you need to include the size and type of the image so it renders correctly. [See this demo](https://glitch.com/edit/#!/richerinstall-screenshot?path=manifest.json%3A14%3A24).

The `form_factor` indicates to the browser whether the screenshot should appear on desktop (`wide`) or mobile environments (`narrow`)

## Further reading

- [Richer PWA installation UI](https://developer.chrome.com/blog/richer-pwa-installation/)
- [Richer install UI available for desktop](https://developer.chrome.com/blog/richer-install-ui-desktop/)

## Demo