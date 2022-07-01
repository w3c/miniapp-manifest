# [Self-Review Questionnaire: Security and Privacy](https://w3ctag.github.io/security-questionnaire/)

## 01.  What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

This specification enables developers to identify, describe and configure a MiniApp using a JSON document. This manifest file, available in a MiniApp package, aims to help user agents, marketplaces, and other services identify, classify, and configure the app. For this reason, the metadata,  including descriptive information about the app, the platform supported, and the environmental and operational setup, is publicly exposed. 

Manifest members do not include information about users or their preferences. However, during the execution phase, MiniApp user agents may register and store any data, including the user's setup preferences (e.g., color schemes, window configuration, and privacy preferences). 

## 02. Do features in your specification expose the minimum amount of information necessary to enable their intended uses?

The information required by the manifest enables MiniApp user agents to run the apps and present helpful information to the end-users. The information exposed by the document is public and based on the requirements extracted from the existing use cases and scenarios.

## 03. How do the features in your specification deal with personal information, personally-identifiable information (PII), or information derived from them?

No personal information is required. All the information declared in a manifest refers to the app and its configuration. It does not contain personal information or user preferences that could help identify or track users. However, the `req_permissions` member includes the list of powerful capabilities required by the app's regular operation. A concrete and systematic rejection of concrete features could help profile users, so the platform should guarantee the management of the user's preferences with no public exposition. 

## 04. How do the features in your specification deal with sensitive information?

No sensitive information is included in the manifest. 

## 05. Do the features in your specification introduce new state for an origin that persists across browsing sessions?

MiniApp user agents must manage the user's preferences, in concrete the explicit permissions to use sensitive system's features (e.g., geolocation and device's sensors) and the look-and-feel options. They must avoid publication of this data, offering the possibility to store and remind the user's preferences across sessions. 

User agents may also persist information about the app, including its versioning details, for updating purposes. No other information related to the manifest should be stored across sessions.

## 06. Do the features in your specification expose information about the underlying platform to origins?

The  [`req_permissions` member](https://www.w3.org/TR/miniapp-manifest/#req_permissions-member) includes the powerful capabilities required by the apps, like hardware devices such as sensors. This information demanded by the app is public but does not indicate that the underlying platform supports them.

## 07. Does this specification allow an origin to send data to the underlying platform?

Yes, the manifest can specify the potential features that could involve sending data to the underlying platform (i.e., use of APIs, storage, etc.) in a generic way through the  [`req_permissions` member](https://www.w3.org/TR/miniapp-manifest/#req_permissions-member). 

The [`pages` member](https://www.w3.org/TR/miniapp-manifest/#pages-member) describes the routes of the MinApp components, including a map of pages and the file structure of the resources within the MiniApp container. The [`icons` member](https://www.w3.org/TR/miniapp-manifest/#icons-member) includes the path to an image stored in the package that depicts the app. 

The user agents may install the app resources and access the underlying platform, preserving the sandboxed environment.      

Still to define the security policy to fetch external resources described in the manifest. Under discussion in [issue #42](https://github.com/w3c/miniapp-manifest/issues/42).

## 08. Do features in this specification enable access to device sensors?

The manifest can specify the features that a MiniApp uses during its operation, involving access to sensors and other sensitive information. This declaration does not imply the use of the service. User agents should implement security mechanisms and guarantee the user's privacy minimizing the potential fingerprinting surface based on these services and sensors.

## 09. Do features in this specification enable new script execution/loading mechanisms?

This manifest is an extension of the Web App manifest, so it is expected to be treated only as descriptive data.

## 10. Do features in this specification allow an origin to access other devices?

Although the manifest may indicate services that may access other devices via network connections or directly linking devices (e.g., via Bluetooth, NFC, or USB), the user agent platform should address these potential vulnerabilities in each concrete case.

## 11. Do features in this specification allow an origin some measure of control over a user agent's native UI?

The manifest allows MiniApp user agents, as in Web apps, to install apps using native UI dialogs. During the installation process, the user agent should show clear and transparent information about the app (i.e., icon, name, description, etc.). This information allows end-users to make a conscious decision before installing it.

User agents should also provide a mechanism for users to remove an installed app. 

The manifest's [`window` member](https://www.w3.org/TR/miniapp-manifest/#window-member) defines the appearance of the MiniApp, including the look and feel of the navigation bar and other features that modify the user agent's native UI like `fullscreen` or `orientation`. User agents must offer intuitive mechanisms to close the apps, reverting the changes that affected the native UI.

## 12. What temporary identifiers do the features in this specification create or expose to the web?

No temporary identifiers are created under the scope of this specification.

> 13. How does this specification distinguish between behavior in first-party and third-party contexts?

Still to define the security policy to fetch external resources described in the manifest. Under discussion in [issue #42](https://github.com/w3c/miniapp-manifest/issues/42).

## 14. How do the features in this specification work in the context of a browser's Private Browsing or Incognito mode?

N/A.

## 15. Does this specification have both "Security Considerations" and "Privacy Considerations" sections?

Yes ([Privacy](https://www.w3.org/TR/miniapp-manifest/#privacy-considerations) and [Security](https://www.w3.org/TR/miniapp-manifest/#security-considerations) sections).

## 16. Do features in your specification enable origins to downgrade default security protections?

The manifest can specify the required access to powerful features that might affect privacy and security (i.e., use of APIs, storage, etc.) through the  [`req_permissions` member](https://www.w3.org/TR/miniapp-manifest/#req_permissions-member). The user agents handle these requests, preserving the secure management of these resources and services.

## 17. How does your feature handle non-"fully active" documents?

The manifest does not specify anything related to this topic.

## 18. What should this questionnaire have asked?
...
