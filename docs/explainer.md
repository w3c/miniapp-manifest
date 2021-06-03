# MiniApp Manifest explainer

> Note: This document serves as a supplementary explanation of the [MiniApp Manifest](https://w3c.github.io/miniapp-manifest/) spec. If there is any inconsistency with the spec, you should consider the spec to be authoritative.

## Authors

Shouren Lan, Zhiqiang Yu, Xiaofeng Zhang, Yongjing Zhang

## 1. Introduction

### What is this?

MiniApp Manifest defines a JSON-based profile file that provides developers with a centralized place to put essential information associated with a MiniApp ([What is MiniApp?](https://w3c.github.io/miniapp/white-paper/#what-is-miniapp)). In fact, every MiniApp must have a manifest file which describes essential information about MiniApp to build tools, host platform, users, and app market.

### Why should we care?

Manifest, as the manifest file of MiniApp, includes configuration information that is necessary for application development, release, installation and running, such as desktop display, application ID, version management, version upgrade, permission statement and UX configuration, etc.

## 2. Manifest Design

To ensure the overall design requirements of [MiniApp Packaging](https://w3c.github.io/miniapp/specs/packaging/), [MiniApp Widget](https://w3c.github.io/miniapp-widget/), and [MiniApp Addressing](https://w3c.github.io/miniapp-addressing/) are met, the manifest file of MiniApp should contain basic information of the application, page routing scope, window configuration information and widget configuration information, etc.

### Key Considerations

#### Why is the basic metadata needed?

* Home screen display: Show the name and icon of the application to users by `name` and `icon`.
* Locales: Set different languages ​​and text directions to meet the localization requirement by `lang` and `dir`.
* Version management: Show version information of MiniApp, control application and device compatibility to users by `version_name`.
* Version upgrade: Provide maintainability and security of MiniApp by `version_code`.
* Permission statement: Declare the necessary permissions required for running MiniApp, such as geolocation, storage, camera, etc.

#### Why are `pages` and `window` needed?

* The [pages](https://w3c.github.io/miniapp-manifest/#pages-member) array needs to cover all the pages included in the MiniApp and specify a reasonable page jump range and home page settings.
* The [window](https://w3c.github.io/miniapp-manifest/#window-member) object needs to cover the basic elements of the MiniApp window, such as the status bar, navigation bar, title and window style, etc.

#### Why is `widgets` needed?

Widgets can be embedded in various local applications, and directly display the content that the user is most concerned about in an interactive way to better meet the user's requirements.

## 3. Sample

So far only a basic subset of MiniApp Manifest is specified. It will be gradually updated and supplemented according to the demands of different scenarios.

```json
{
  "dir": "ltr",
  "lang": "en-US",
  "app_id": "org.example.miniApp1",
  "name": "My MiniApp Demo",
  "short_name": "Demo X",
  "version_name": "1.0.0",
  "version_code": 1,
  "description": "A Simple MiniApp Demo",
  "icons": [
    {
      "src": "common/icon/icon.png",
      "sizes": "48x48"
    }
  ],
  "min_platform_version": "1.0.0",
  "pages": [
    "pages/index/index",
    "pages/detail/detail"
  ],
  "window": {
    "navigation_bar_text_style": "black",
    "navigation_bar_title_text": "Demo",
    "navigation_bar_background_color": "#f8f8f8",
    "background_color": "#ffffff",
    "fullscreen": false
  },
  "widgets": [
    {
      "name": "widget",
      "path": "widgets/index/index",
      "min_platform_version": "1.0.0"
    }
  ]
}
```

## 4. Extension of the Web App Manifest
The MiniApp manifest is developed in the way that it covers the most common practices in the  target ecosystems like 'Mini Program' [[1]](https://smartprogram.baidu.com/developer/index.html)[[2]](https://open.alipay.com/channel/miniIndex.htm)[[3]](https://mp.weixin.qq.com/cgi-bin/wx) and [Quick App](https://www.quickapp.cn/), while trying to be aligned (compatible) as much as possible with other ongoing web standard development. In this sense, the MiniApp Manifest follows the recommendations of the <a href="https://w3ctag.github.io/design-principles/#extend-manifests">Web Platform Design Principles</a> to extend the <a href="https://www.w3.org/TR/appmanifest/">Web App Manifest</a>. Therefore common elements (e.g. `dir`, `lang`) or counterparts (e.g. `window.orientation` vs. `orientation`) can be found in both manifest specifications. A detailed comparison given in [Appendix A](#a-miniapp-manifest-comparison-with-web-app-manifest).

On the other hand, MiniApp manifest has different assumptions on the hosting platforms and the form of application from those of Web App Manifest, so there are aspects that are not matched:

1. **Platform difference:** MiniApp needs to cover the cases that the application hosting platforms are not based on the web/browser, such as a native OS or a hosting application running on top of the OS. The software architecture is more like a native application rather than a web application (although it leverages some web technologies). Therefore, a MiniApp manifest needs to manage the compatibility more strictly between its application version and the platform version than that in the web environment. Member attributes such as `version_name`, `version_code` and `min_platform_version` are specified for that purpose, but are not necessary for a web application. And strict permission management by the `req_permissions` attribute is also needed to control the access to local resources (sensitive data and functions) via the hosting platform.
2. **Application form difference:** A MiniApp typically consists of a set of pages and/or widgets. A MiniApp page/widget is similar to a web page in the sense of developing techniques (e.g. JS, CSS), but is different in many other ways like the life-cycle, the layout, components and system APIs. More importantly, these pages/widgets are organically composed views/activities of the MiniApp rather than independent web pages. The MiniApp manifest needs to provide means to organize and configure them into a common look and feel (e.g. navigation bar, scrolling behavior, width adaptation) by the attributes like `window`, `pages` and `widgets`. Moreover, web app manifests are deployed in an HTML page using a `link` element, but MiniApp manifests are a part of the MiniApp [package](https://w3c.github.io/miniapp-packaging/), and does not depend on HTML.

As both work are still under development, it worths further evaluating the possibilities of alignment from each side. From MiniApp perspective, further study could be investigating the usability of some unmapped member attributes (such as `categories`, `screenshots`) of Web App Manifest in the context of MiniApp.



## A. MiniApp Manifest comparison with Web App Manifest
The following table mainly describes the comparison between the manifest attributes in Mini App and Web App. "-" in the table means that there is no such attribute in the corresponding manifest.

[Mini App Manifest](https://w3c.github.io/miniapp-manifest/)| [Web App Manifest](https://www.w3.org/TR/appmanifest/) |	Comparison 
:---    |:--        |:---
dir	    |   dir     |	Same
lang    |	lang    |	Same
appID	|-          |	MiniApp only
name	|name       |	Same
short_name	|short_name|	Same
description	|description|	Same
icons	|icons	| Same
version_name	|-	| MiniApp only
version_code	|-	| MiniApp only
min_platform_version	|-	|MiniApp only
pages	|scope	| Similar. `pages` lists the local URIs to the app pages of a MiniApp while the `scope` in Web App can vary from a local URL to a remote URL.  
pages.[0]	|start_url	| Different but comparable. The first element of `pages` represents the starting page in MiniApp instead of an explicit URL.
window  |-  | MiniApp only. `window` contains a set of attributes for the default configuration of MiniApp pages and widgets. Most of them are unique to MiniApp except for a few that are mappable to Web App Manifest.(See below.)
window.background_color	|background_color	|Similar. MiniApp accepts only RGB hex value for now.
window.orientation	|orientation	|Same
window.fullscreen	|display=fullscreen	|Same but in different formation (boolean vs enum). 
widgets	|-	|MiniApp only
req_permissions	|-	|MiniApp only
\-	|theme_color	|Web App only.  (For further study in MiniApp)
\-	|iarc_rating_id	| Web App only. (For further study in MiniApp)
\-	|related_applications	| Web App only (For further study in MiniApp)
\-	|prefer_related_applications	| Web App only (For further study in MiniApp)
\-	|categories	| Web App only.  (For further study in MiniApp)
\-	|screenshots	| Web App only.  (For further study in MiniApp)
\-	|shortcuts	| Web App only  (For further study in MiniApp)

## References & acknowledgements

In the process of writing the MiniApp Manifest specification, many people have given valuable comments, thank you all.

The following name list will be continuously updated, in the order of alphabetical.

* Chun Wang
* Changjun Yang
* ...
