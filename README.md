# StoreConnect Add-on: Klaviyo
Integrate [![StoreConnect](https://custom-icon-badges.demolab.com/badge/StoreConnect-fea42a?logo=storeconnect&logoColor=white "StoreConnect")](https://getstoreconnect.com) with [![Klaviyo](https://custom-icon-badges.demolab.com/badge/Klaviyo-black?logo=klaviyo-flag&logoColor=white "Klaviyo")](https://klaviyo.com) to bring your marketing automation to life. With StoreConnect as your eCommerce built on top of Salesforce, the world's #1 CRM, you have customer knowledge and reach like never before.

This integration is not like your typical integration! Say goodbye to locked down, half-baked integrations that you quickly come to regret!

It has the following **key features**:
* Fully **Extensible**
* **Open source** unmanaged package
* **No Apex**, on-demand code changes directly in Flow without Apex
* Uses **Auto-Launch Flow** as a controller for implemention via Flows, Actions, and/or Apex
* **Modifiable** to work with Custom Fields and Custom Objects
* **Flexible Triggers**, compatible with on-demand actions, scheduling, and realtime triggers

## Installation

[![Install in Production](https://custom-icon-badges.demolab.com/badge/Install-Production-fea42a?style=for-the-badge&logo=storeconnect&logoColor=fea42a "Install in Production")](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t2u000000D2lRAAS)

[![Install in Sandbox](https://custom-icon-badges.demolab.com/badge/Install-Sandbox-fea42a?style=for-the-badge&logo=storeconnect&logoColor=fea42a "Install in Sandbox")](https://test.salesforce.com/packaging/installPackage.apexp?p0=04t2u000000D2lRAAS)

### Post-Installation
**Required**
1. Assign the `StoreConnect Add-on: Klaviyo` to any users that need to use the integration or see the `Integration__c` records (required for Flow execution)
2. Klaviyo [Theme Template](./theme/snippets/events/klaviyo.liquid)
3. Add your [Klaviyo Public API Key](https://www.klaviyo.com/settings/account/api-keys) to the Store Variable `klaviyo_public_api_key` of each store using the integration
4. Create your [Klaviyo Private API Key](https://www.klaviyo.com/create-private-api-key), you will need the below minimum access:
    * **Catalogs**: `Full Access`
    * **Events**: `Full Access`
5. Add your Klaviyo Private API Key to Salesforce
    * **Setup** > **Named Credentials** > `Klaviyo` > **External Credential**: `Klaviyo`
    * Under **Principals** > **Actions** > **Edit** > **Add**:
        * **Name**: `PrivateKey`
        * **Value**: *[Private API Key from Klaviyo dashboard]*
6. Turn on the `Klaviyo_ALF_Integration` flow

**Optional**
1. Turn on the `Klaviyo_SF_Sync` flow to enable `Klaviyo Sync` buttons
2. Add the `Klaviyo Sync` button to the desired `Product2` Page Layout(s) and/or List View Layout(s)
3. Add the `Klaviyo Sync` button to the desired `s_c__Product_Category__c` Page Layout(s) and/or List View Layout(s)
4. Turn on the `Klaviyo_RTF_Order_Events` flow or implement the Order Events via a self-defined method, e.g. Apex Trigger, Scheduled Flow, etc.
5. Turn on the `Klaviyo_RTF_Fulfillment_Events` flow or implement the Fulfillment Events via a self-defined method, e.g. Apex Trigger, Scheduled Flow, etc.

### Dependencies
[![StoreConnect](https://custom-icon-badges.demolab.com/badge/StoreConnect-16.3+-fea42a "StoreConnect")](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000FMkeKUAT)

[![Streams](https://custom-icon-badges.demolab.com/badge/Streams-61.0+-blue "Streams")](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3000000B5Z0LEAV)

## Intended Use
This add-on is intended to provide a base level of functionality and is published as an unmanaged Salesforce package for the purposes of flexibility and extensiblity.

Salesforce orgs can differ dramatically from one to the next and all additional packages must take into consideration a shared ecosystem with other packages and compatibility with various Salesforce Clouds. Installation and modification of this package is intended for advanced users, either for Salesforce admins or partners.

Due to the nature of Salesforce being a configurable platform, this add-on has been designed to not override any existing Lightning Record Pages, Page Layouts, Apps, Triggers, or other common customizations, and instead is packaged with the intention of being deployed in a various number of ways dependent on the target org.

## Features

### Platform Integrations
| Klaviyo Objects   | Description                                            | Method           | Deployment |
| ----------------- | ------------------------------------------------------ | ---------------- | - |
| `CatalogCategory` | Synchronizes `s_c__Product_Category__c` records        | Auto-Launch Flow | `s_c__Product_Category__c` button for List/Page Layout |
| `CatalogItem`     | Synchronizes `Product` `s_c__Is_Master__c` records     | Auto-Launch Flow | `Product2` button for List/Page Layout                 |
| `CatalogVariant`  | Synchronizes `Product` non `s_c__Is_Master__c` records | Auto-Launch Flow | `Product2` button for List/Page Layout                 |


| Events             | Description                                                                              | Method           | Deployment |
| -----------------  | ---------------------------------------------------------------------------------------- | ---------------- | - |
| `Order Placed`     | `Order` record where `s_c__Checkout_Step__c == 'complete'`                               | Auto-Launch Flow | Requires specific implementation method with optional conditionals. Recommended via Record-Triggered-Flow |
| `Incomplete Order` | `Order` record where `s_c__Checkout_Step__c == 'complete' or s_c__Abandonded__c == TRUE` | Auto-Launch Flow | Requires specific implementation method with optional conditionals. Recommended via Record-Triggered-Flow |
| `Fulfilled Order`  | `s_c__Shipment__c` record                                                                | Auto-Launch Flow | Requires specific implementation method with optional conditionals. Recommended via Record-Triggered-Flow |

### Website Tracking
You will need to install copy the contents of the provided [Theme Template](./theme/snippets/events/klaviyo.liquid) into a `s_c__Theme_Template__c` record in the required `s_c__Theme__c` record(s) within your org.

There is also an example [Events Theme Template](./theme/snippets/events.liquid) that can be used for rendering the Klaviyo snippet.

For additional Theme knowledge, please see the below resources:
* [Theme Builder](https://help.getstoreconnect.com/documentation/themes/0.11.6/index.html "Theme Builder")
* [Theme Templates](https://help.getstoreconnect.com/documentation/themes/0.11.6/templates/index.html "Theme Templates")
* [Theme Developer Cource](https://storeconnect.academy/students/sign_up?course_id=f39ced7f-5c52-4c70-a768-60f17026e74c "Theme Developer Course")
* [Store Variables](https://help.getstoreconnect.com/documentation/store-variables.html "Store Variables")

| Events             | Description                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| `Viewed Product`   | Triggered when viewing a Product page | `s_c__Theme_Template__c` record                         |
| `Added to Cart`    | Triggered when adding a Product to the Cart via `Buy Now`, `Add to Cart`, or Cart `Update`      |
| `Started Checkout` | Triggered when navigated to `/checkout` page (if using custom checkout then modify this method) |


| Store Variables          | Required  | Description                                                                 |
| ------------------------ | --------- | --------------------------------------------------------------------------- |
| `klaviyo_public_api_key` |    YES    | Log into **Klaviyo** > **Account** > **Settings** > **API Keys** > `Public API Key` |

## Advanced Configuration
This package relies on three (3) values being set via the Custom Settings in `StoreConnect_Add_on_Klaviyo__c`:
* `Named_Credential__c`
* `Platform_Instance_Name__c`
* `Batch_Size__c`

This settings are designed for a single Salesforce:Klaviyo instance mapping to simplify installation. For a 1:many integration, you will need to override the supplied flows that trigger the Auto-Launch Flow controller to supply the instance specific value logic.

## Troubleshooting
Outside of typical Salesforce permission issues that may be org specific, there may be permission issues on the Klaviyo side. If this is the case, adjust the permissions of the Private API Key in Klaviyo to `Full Access` and then restrict down as required to maintain the integration function.

## Disclaimer
This add-on is provided as-is with no warranty or support beyond this document. This package may be updated overtime if critical bugs are reported/detected but is designed to be an unmanaged package for the purposes of the end-user having full control to resolve any and all issues without publisher support.