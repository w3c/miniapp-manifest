# MiniApp Manifest explainer

> Note: This document serves as a supplementary explanation of the [MiniApp Manifest](https://www.w3.org/TR/miniapp-manifest/) spec. If there is any inconsistency with the official spec, you should consider the spec to be authoritative.

## Authors

Shouren Lan, Zhiqiang Yu, Xiaofeng Zhang, Yongjing Zhang

## 1. Introduction

### What is this?

MiniApp Manifest defines a JSON-based profile file that provides developers with a centralized place to set up essential information associated with a MiniApp ([What is MiniApp?](https://w3c.github.io/miniapp/white-paper/#what-is-miniapp)).

The information described in a MiniApp Manifest includes metadata about the app, like app identifiers, denomination, human-readable description, versioning data, and styling information. The MiniApp manifest also configures the routing of the pages and widgets that are part of a MiniApp and other data associated with their running environment, tools, and distribution.

### Why should we care?

The MiniApp Manifest includes information that describes and configures the app, providing developers, publishers, and third parties with advanced  metadata and setup information, useful in the different stages of the app, from development and distribution to installation and execution. 

Members of the manifest include human-readable descriptions (`name`, `description`, `icons`, and `short_name`), version management attributes (`app_id`, `version`, and `platform_version`), look-and-feel configuration (`background_color`, `fullscreen`, `orientation`, `color_scheme`, among others), permission requirements (`req_permissions`), routing and widgets configuration (`pages`, `widgets`).

## 2. Manifest Design

The MiniApp Manifest is defined as a registry of supplementary members for the <a href="https://www.w3.org/TR/appmanifest/">Web Application Manifest</a> and the <a href="https://www.w3.org/TR/manifest-app-info/">Web App Manifest - Application Information</a> specifications. The additional metadata aims at describing the particularities of MiniApps to fulfill the design requirements of the [MiniApp Packaging](https://www.w3.org/TR/miniapp-packaging/), [MiniApp Widget Requirements](https://www.w3.org/TR/miniapp-widget-req/), and [MiniApp Addressing](https://www.w3.org/TR/miniapp-addressing/) specifications. 

### Key Considerations

#### Why is the basic metadata needed?

* __Home-screen display__: Developers and publishers may present the title and app's icon after the installation (shortcut on the home-screen) through `name` and `icons`.
* __Publication in marketplaces__: Publishers may list and display complete information about an app in a marketplace or directory (`app_id`, `name`, `icons`, `description`, and `short_name`).
* __Locales__: Developers may indicate the languages ​​and writing directions to meet their localization requirements (`lang` and `dir`).
* __Version management__: Developers include app versioning data to inform about the evolution of the application and device compatibility through `version` (with `code` and `name`). This versioning metadata would help in the process of distribution and safe execution.
* __Permission statement__: Developers should declare the necessary permissions to use the device's powerful features required in runtime (e.g.,  geolocation, storage, and camera usage) through `req_permissions`. They may also include descriptive texts to clarify the purpose of the request (`reason`).
* __Look-and-feel and User Experience__: Developers may declare information about the styles and viewport configuration (`window`, `background_color`, `design_width`, `orientation`, `fullscreen`, `auto_design_width`, `background_text_style`, `design_width member`), specific configuration for the in-app navigation (`navigation_bar_background_color`, `navigation_bar_text_style`, `navigation_bar_text_style`, `navigation_bar_title_text`, `navigation_style`), and other setup options for maximizing the user experience (`enable_pull_down_refresh`, `on_reach_bottom_distance`). Developers may also offer the possibility to use the predefined `color_scheme` of the platform (`light` and `dark`). 
* __MiniApp Platform__: Developers may define concrete features and setups to specific platforms (members `platform_version`, `min_code`, `release_type`, and `target_code`) and the type of device (e.g., automotive, smartphone, wearable in the `device_type` member). 

#### Why are `pages` and `window` members needed?

* The [`pages`](https://www.w3.org/TR/miniapp-manifest/#pages-member) member is an array that contains all the routes to the components or pages that compose the app. The first page in the array is the entry page of the app.
* The [`window`](https://www.w3.org/TR/miniapp-manifest/#window-member) member is an object that describes the styles and user interface of the app (i.e., `window`, `background_color`, `design_width`, `orientation`, `fullscreen`, `auto_design_width`, `background_text_style`, `design_width member`, `navigation_bar_background_color`, `navigation_bar_text_style`, `navigation_bar_text_style`, `navigation_bar_title_text`, `navigation_style`, `enable_pull_down_refresh`, `on_reach_bottom_distance`)

#### Why is `widgets` member needed?

A [Widget](https://www.w3.org/TR/miniapp-widget-req/) is a particular form of MiniApp page that can be embedded within other applications or environments (i.e., device desktop). The `widgets` member allows users to describe the pages that are part of the MiniApp that can be presented as widgets, including `name` (title of the widget), `path` (path to the corresponding page or component), and `min_code` (minimum platform version required). Using widgets, users can directly display and interact with the app in different scenarios.  

## 3. Sample

The MiniApp Manifest will be gradually updated and supplemented according to the demands of the different use cases and scenarios detected.

```json
{
  "dir": "ltr",
  "lang": "en-US",
  "app_id": "org.example.miniapp1",
  "name": "My MiniApp Demo",
  "short_name": "Demo X",
  "version": {
    "name": "1.0.1",
    "code": 11
  },
  "description": "A Simple MiniApp Demo",
  "icons": [
    {
      "src": "common/icon/icon.png",
      "label": "Red lightning",
      "sizes": "48x48"
    }
  ],
  "platform_version":{
    "min_code": 1,
    "release_type": "Beta1",
    "target_code": 2
  },  
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
      "min_code": "1.0.0"
    }
  ],
  "color_scheme": "light",
  "device_type": [
      "phone",
      "tv",
      "car"
  ]		  
}
```

## 4. Extension of the Web App Manifest

The MiniApp manifest is developed in the way that it covers the most common practices in the  target ecosystems like 'Mini Program' [[1]](https://smartprogram.baidu.com/developer/index.html)[[2]](https://open.alipay.com/channel/miniIndex.htm)[[3]](https://mp.weixin.qq.com/cgi-bin/wx) and [Quick App](https://www.quickapp.cn/), while trying to be aligned (compatible) as much as possible with other existing and future web standards. In this sense, the MiniApp Manifest follows the recommendations of the <a href="https://w3ctag.github.io/design-principles/#extend-manifests">Web Platform Design Principles</a> to extend the <a href="https://www.w3.org/TR/appmanifest/">Web App Manifest</a>. Therefore, this specification extends the Web App Manifest through a defined[extension point](https://www.w3.org/TR/appmanifest/#extensibility), reusing members like `name`, `short_name`, `icons`, `dir`, and `lang`. 

A detailed comparison between the Web App Manifest and the MiniApp Manifest is presented in [Appendix A](#a-miniapp-manifest-comparison-with-web-app-manifest).

On the other hand, MiniApp manifest has different assumptions on the hosting platforms and the form of application from those of Web App Manifest, so there are aspects that are not matched:

1. **Platform difference:** MiniApp needs to cover the cases in that the application hosting platforms are not based on the web/browser, such as a native OS or a hosting application running on top of the OS. The software architecture is more like a native application than a web application (although it leverages some web technologies). Therefore, a MiniApp manifest needs to manage compatibility more strictly between versions of the application and platform than in the web environment. Member attributes such as `device_type`, the `version` object (with the `code` and `name` members), and `platform_version` object (with `min_code`, `release_type`, and `target_code`) are specified for that purpose. However, they are not necessary for a web application. Moreover, strict permission management by the `req_permissions` attribute is also needed to control the access to local resources (sensitive data and powerful functions) via the hosting platform.

2. **Application form difference:** A MiniApp typically consists of pages and/or widgets. A MiniApp page/widget is similar to a web page in developing techniques (e.g., JS, CSS) but is different in many other ways like the life-cycle, the layout, components, and system APIs. More importantly, these pages/widgets are organically composed views/activities of the MiniApp rather than independent web pages. The MiniApp manifest needs to provide means to organize and configure them into a common look and feel (e.g., navigation bar, scrolling behavior, width adaptation) by the attributes like `window`, `pages`, and `widgets`. Moreover, web app manifests are deployed in an HTML page using a `link` element, but MiniApp manifests are included within a MiniApp [package](https://www.w3.org/TR/miniapp-packaging/) and do not depend on HTML binding.

As both works are still under development, it is worth further evaluating the possibilities of alignment from each side. From the MiniApp perspective, further study could be investigating the usability of some unmapped member attributes (such as `categories`, `screenshots`) of Web App Manifest in the context of MiniApp.


## A. MiniApp Manifest comparison with Web App Manifest

The following table mainly compares between the manifest attributes in MiniApp and Web App. The "-" character in the table indicates the absence of such attribute in the corresponding manifest.

[MiniApp Manifest](https://www.w3.org/TR/miniapp-manifest/) | [Web App Manifest](https://www.w3.org/TR/appmanifest/) + [Application Information](https://www.w3.org/TR/manifest-app-info/) |	Comparison 
:---    |:--        |:---
`dir`	    | `dir`       |	Same
`lang`    |	`lang`      |	Same
`app_id`	| `id`        |	Same
`name`	  | `name`       |	Same
`short_name`	| `short_name` |	Same
`description`	| `description` |	Same
`icons`	| `icons`	| Same
`version`	| -	| MiniApp only
`platform_version`	| -	 |MiniApp only
`pages`	| `scope`	| Similar. MiniApp's `pages` member lists the local URIs to the app pages of a MiniApp while the Web App Manifest's `scope` member can contain either a local URL or a remote URL.  
`pages.[0]`	| `start_url`	| Different but comparable. The first element of MiniApp's `pages` represents the starting page the app instead of having an explicit URL like `start_url`.
`window`  | -  | MiniApp only. `window` contains a set of attributes for the default configuration of MiniApp pages and widgets. Most of them are unique to MiniApp except for a few that are mappable to Web App Manifest. (See below.)
`window.background_color`	| `background_color`	| Similar. MiniApp accepts only RGB hex value for now.
`window.orientation`	| `orientation`	| Same
`window.fullscreen`	| `display`=`fullscreen`	| Same but in different formation (boolean vs enum). 
`widgets`	| -	| MiniApp only
`req_permissions`	| -	| MiniApp only
`device_type`	| -	| MiniApp only
`color_scheme`	| `theme_color`	| Similar. MiniApp's `color_scheme` refers to the preferred color scheme (i.e., `light`, `dark`, and `auto`) that is configured by the hosting platform, while `theme_color` may indicate a concrete RGB color as default color theme of the app. 
\-	| `iarc_rating_id`	| Web App only. (For further study in MiniApp)
\-	| `related_applications`	| Web App only (For further study in MiniApp)
\-	| `prefer_related_applications`	| Web App only (For further study in MiniApp)
\-	| `categories`	| Web App only.  (For further study in MiniApp)
\-	| `screenshots`	| Web App only.  (For further study in MiniApp)
\-	| `shortcuts`	| Web App only  (For further study in MiniApp)

## References & acknowledgements

In the process of writing the MiniApp Manifest specification, many people have given valuable comments. Thank you all.

The following name list will be continuously updated in the alphabetical order.

* Chun Wang
* Changjun Yang
* ...
