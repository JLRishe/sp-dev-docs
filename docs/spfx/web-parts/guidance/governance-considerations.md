---
title: SharePoint Framework solutions governance considerations
ms.date: 09/25/2017
ms.prod: sharepoint
---


# SharePoint Framework solutions governance considerations

Using the SharePoint Framework organizations can easily build solutions that make better use of capabilities available in SharePoint and Office 365. Additionally, by default, SharePoint Framework solutions work across the different devices even including the SharePoint mobile app. In order to benefit of SharePoint Framework solutions, organizations should have an actionable governance plan covering the most important considerations.

## Anatomy of SharePoint Framework solutions

![Diagram illustrating the composition of SharePoint Framework solutions](../../../images/guidance-governance-spfx-structure-schema.png)

SharePoint Framework solutions consist of two parts: code (often referred to as web part bundle), deployed to a URL, and an .sppkg file that contains web part manifest with a URL pointing to the location where the web part code is deployed. There are no particular restrictions with regards to where the code is deployed to, as long as users working with the web part can access the web part code. Organizations can choose for example to have their web parts deployed to the [Office 365 public CDN](https://dev.office.com/blogs/office-365-public-cdn-developer-preview-release), [Azure storage](../get-started/deploy-web-part-to-cdn.md) or a privately owned web server.

## Web part code hosting location

The first and foremost thing that organizations should know, before deploying SharePoint Framework solutions, is where the code of the solution is deployed to. SharePoint Framework solutions are executed as a part of the page in the context of the current user. As a result, whatever the user can do, the web part's code can do as well. In contrary to SharePoint add-ins, there is no separate permission scope applied to SharePoint Framework solutions. This is why SharePoint administrators should treat SharePoint Framework solutions as high-trust solutions - the same way they treated farm solutions on-premises. The location where the web part's code is deployed is important for a number of reasons.

### Is the code hosting location supported by the organization

SharePoint Framework doesn't impose any restrictions with regards to where the solution's code is deployed to. As a result, developers and vendors could deploy the code to a range of locations within or outside of the organization's IT department. Different organizations might have different requirements with regards to servers that they use varying from access policies to SLAs. Before deploying a SharePoint Framework solution package, organizations should ensure, that the server that is used to host the code is a known server approved to be used by the organization.

### Who manages the code hosting location

SharePoint Framework solutions execute as a part of the page in the context of the current user. While an organization could perform a code review before deploying a solution package, in order to verify that the code can be trusted, it also has to be able to ensure the integrity of the code as long as it's deployed to the tenant. Organizations should have a clear understanding of who manages the hosting location, who and under what circumstances can modify the files and what the process of approving updates looks like. Establishing this information upfront not only helps organizations control the update process but also lowers the risk of deploying malicious code.

### What is the SLA for the hosting location

When organizations use Office 365 and SharePoint Online, they rely on the SLA provided by Microsoft. SharePoint Framework solutions, that extend the standard capabilities provided with SharePoint and Office 365, should be deployed to servers that meet or exceed the SLA provided by Microsoft. That way organizations can ensure that they will be able to truly benefit of the added values of their customizations.

## What libraries are used by the solution

When building client-side solutions, developers can choose from a variety of libraries such as React, Angular, jQuery or Knockout to name a few. Using an existing JavaScript library makes it easier for developers to build rich solutions. There are big differences between how the different libraries work, and often specific knowledge is required to fully understand how to build a solution using the particular library.

Once released to your production tenant, you have to be sure that your support organization, which could be your own IT department or a contracted third party, is capable of supporting the solution. In order to do this, the support organization should have at least a basic understanding of the library used to build that solution. Also, the more different libraries you use across your tenant, the harder it will be to support the different solutions. Selecting one or two libraries to use in your organization, helps you lower the operational costs. Before deploying a solution to your production tenant, you should ensure that the solution is using only libraries supported in your organization.

## Does the solution use external scripts and if so, where does it load them from

When using existing JavaScript libraries developers can either choose to include them in the web part code bundle or load them from a URL. Loading libraries from URLs allows developers to optimize SharePoint Framework solutions for performance. Because libraries are loaded from a URL, they don't need to be included in the web part bundle which decreases its size making it load faster. Additionally, by referencing the same libraries across the whole tenant, SharePoint Framework solutions will be loading faster by reusing the previously downloaded scripts from the local cache.

There are no restrictions with regards to where the existing libraries can be loaded from and it's important that you know from which servers external scripts are being loaded. Together with the web part code, these scripts run in the context of the current user and can do whatever the current user can. It's therefore important that you trust these scripts and their integrity. Some organizations have strict policies related to loading resources from public CDNs and you should ensure that the solution and its resources meet your organizational policies.

### Is the hosting location optimized for performance

Loading existing libraries from a URL instead of embedding them in the web part bundle is the first step in speeding up the loading time of SharePoint Framework solutions. To get the most out of it, you want to ensure, that the server hosting the different scripts is correctly configured for optimal performance. It should for example serve all files compressed and the longer it allows proxies and clients to cache the files, the longer users will be able to load these scripts from their local cache, significantly speeding up loading SharePoint pages with web parts on them.

## Approve SharePoint Framework solutions for deployment

SharePoint Framework solutions are deployed to a tenant centrally through the App Catalog. Your organization should have a plan in place describing who is allowed to deploy and approve SharePoint Framework packages. This is important, because this plan should include the responsibility of verifying that the packages that are being deployed are secure and meet the organizational policies. SharePoint Framework solutions run in browser in the context of the current user and, unlike SharePoint add-ins, always have the same permissions as the currently signed-in user. Before deploying and approving a SharePoint Framework solution for use in your organization, its origin and other criteria mentioned earlier in this article should be carefully examined.

In order to verify, that the particular SharePoint Framework solution meets your organizational policies, organizations should review the contents of the .sppkg package that you want to deploy and closely examine the contents of the referenced scripts and the location where they are hosted. This step can be performed manually or it could be automated partly for example by using third party tooling. [SharePoint Customization Analysis Framework](https://rencore.com/products/#spcaf) (SPCAF) is an example of third party solution that significantly simplifies the process of analyzing the contents of SharePoint Framework solutions and verifying, that they meet your organizational security and governance requirements.

## SharePoint Framework solutions and no-script sites

In Office 365 organizations can use the no-script setting to disable script-based customizations in SharePoint Online. Organizations can configure the no-script setting either for the whole tenant or for a particular site collections. Based on the criteria from the organizational policies, administrators can use the no-script setting to disable customizations built for example using the script editor web part or a user custom action.

The no-script setting is meant for organizations to apply an additional layer of control and security to either the whole tenant or specific site collections. Customizing SharePoint using script embedding and injecting is not without risks and particularly on sites containing sensitive information should be thoroughly evaluated.

In the past developers used script embedding and injecting techniques for building powerful SharePoint customizations. In some cases, these customizations relied on specific page structure and when it changed, the particular customization would not work correctly anymore. To guide developers to build more robust solutions, the SharePoint engineering team decided, that all modern sites should have the no-script setting enabled. This means that embedding and injecting scripts on these sites is not possible and using the SharePoint Framework is currently the only option to customize these sites. The expectation is, that all modern sites in the future will use the no-script setting and alternatives to script embedding and injecting will become available for developers to support the different scenarios.
