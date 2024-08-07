# GSA API Standards

This document captures **GSA's recommended best practices, conventions, and standards for Application Programming Interfaces (APIs)**. Projects should start by addressing [API Security](#api-security), followed by implementing the [Mandatory Items](#mandatory-items) and [Mandatory For New Public APIs](#mandatory-for-new-public-apis). After addressing those, review [Other Considerations](#other-considerations) for additional advice.

## Contents
- [About These Standards](#about-these-standards)
- [API Security](#api-security)
- [Mandatory Items for Public APIs](#mandatory-items-for-public-apis)
- [Recommended Items for Public APIs](#recommended-items-for-public-apis)
- [Mandatory Items for Limited Partner APIs](#mandatory-items-for-limited-partner-apis)
- [Recommended Items for Limited Partner APIs](#recommended-items-for-limited-partner-apis)
- [Other Considerations](#other-considerations)
- [API Testing](#api-testing)
- [SOAP Web Services](#soap-web-services)
- [Future Topics](#future-topics)


## About These Standards

These standards are forked from the [18F API Standards](https://github.com/18F/api-standards). They are also influenced by several other sources, including the [White House API Standards](https://github.com/WhiteHouse/api-standards) and best practices from the private sector.

### The standards are a roadmap not a roadblock

These standards are intended to streamline the process for GSA organizations to publish new APIs by providing practical and pragmatic advice. We believe these standards will benefit GSA API development and provide consistency.


### They primarily focus on RESTful APIs
Most of the content in these standards relates to "RESTful" APIs. However, many of the standards are equally appropriate for other types of web service.

A few specific recommendations are provided for [SOAP web services](#soap-web-services), and we encourage the GSA community to share more recommendations.

### They don't look under the covers
Because APIs may be developed with multiple technologies, these standards avoid details internal to the development of the application or unique to a development platform. They generally focus on the "externals" that will be exposed to users.

### API Category Definitions

For the purposes of these standards, we use the following definitions:
  * **Non Public APIs** - Available only to GSA applications and users.
  * **Public APIs** - Available to non-GSA applications and users. Examples include the general public, other agencies, or private companies.
  * **Limited Partner APIs** - Available to two or fewer non-GSA applications or users. Examples include other agencies or private companies.

## API Security
API Security is governed by the GSA IT Security Procedural Guide: API Security CIO-IT Security-19-93. Reference that guide for security related topics such as HTTPS encryption, authentication, and authorization.

## Mandatory Items for Public APIs ##

These are mandatory for GSA public APIs, with exceptions where noted.

### 1. Add Your API To The GSA API Directory
(Public APIs) A directory of GSA public APIs is available at [open.gsa.gov/api](https://open.gsa.gov/api/). Add your API to this directory by posting an issue or pull request in the [GitHub repository](https://github.com/GSA/open-gsa-redesign).

Part of this process is to first complete the neccessary steps to add your GitHub Account to the GSA Organization and then send an email to the CTO team to have your account added to the open-gsa-redesign User Group. The steps are documented on the [Adding API Documentation](https://github.com/GSA/open-gsa-redesign/blob/master/APIDOCS.md) GitHub page.

### 2. Use The api.data.gov Service

(Public APIs) The [api.data.gov service](https://api.data.gov/about/) is an API management service for federal agencies. GSA APIs should use the `api.gsa.gov` base domain with this service.  By having the `api.gsa.gov` base URL as a proxy to developers, this also makes it easier to update and maintain the API in the future since you can update the underlying system and URLs without exposing it to the public.  In some cases, other specific base domains can be established with this service for GSA APIs.

#### Implementing With Your API
Initially, this service can be added as a new version of the URL, and then existing users can be transitioned to the new URL. For help setting this up, contact the api.data.gov team at <api.data.gov@gsa.gov>.

#### Additional Benefits
The api.gsa.gov service also provides:
* API key management
* rate limiting (throttling)
* gathering usage statistics (analytics)

Keys managed by api.data.gov can be re-used with other APIs hosted by this service, which reduces complexity for users. This service also allows the use of a DEMO_KEY for unauthenticated access, without keys, which reduces the "Time To First Hello World" for developers using your API.

_Exceptions: Not required for SOAP APIs. However, it may still provide value to your SOAP API._

### 3. Provide Support For Versioning
All APIs must support versioning. The recommended method of versioning REST APIs is to include a major version number in the URL path. For example "/v1/". An example of this method can be found at: https://gsa.github.io/sam_api/sam/versioning.html.

#### Major and Minor Versions
Major versions (e.g. v1, v2) should be reserved for breaking changes and major releases. Minor versions (eg. 1.1, 2.3) are not required, but can provide additional information about the API. If used, they should not be in the URL, but should be in the HTTP Headers.

#### Breaking Changes (backwards-incompatible)
Any changes made to a specific version of your API should not break your contract with existing users. If you need to make a change that will break that contract, create a new major version.

Examples of Breaking Changes for REST APIs:
- Removing an HTTP method
- Removing or renaming a field in the request or response message
- Removing or renaming a query parameter
- Changing the URL or URL format

Examples of Breaking Changes for SOAP web services:
- Removing an operation
- Renaming an operation
- Changing the parameters (in data type or order) of an operation
- Changing the structure of a complex data type.

#### Non-Breaking Changes (backwards-compatible)
It is not necessary to increment the major API version for non-breaking changes. Non-breaking changes can be reflected in a minor version, if used.

Examples of Non-Breaking Changes for REST APIs:
- Adding an HTTP method
- Adding a field to a request message
- Adding a field to a response message
- Adding a value to an enum
- Adding a query parameter

Examples of Non-Breaking Changes for SOAP web services:
- Adding a new WSDL operation to an existing WSDL document.
- Adding a new XML schema type within a WSDL document that are not contained within previously existing types


#### Support for Previous Versions
Leave at least one previous major version intact. And communicate to existing users to understand when previous versions will be decommissioned.

#### Prototype or Alpha Versions
Use "/v0/" to represent an API that is in prototype or alpha phase and is likely to change frequently without warning.

### 4. Provide Public Documentation
The developer's entry point to your API will likely be the documentation that you provide.

Your API documentation should provide:
* An overview of the contents of the API and the data sources.
* Public APIs should provide production URLs for accessing the API. (Non-public APIs would exclude this.)
* Required parameters and defaults.
* A description of the data that is returned.
* A description of the error codes that are returned, and their meaning.

Additional nice-to-haves include:
* Explanation of key management and a sample key.
* Description of update frequency.
* Interactive documentation to demonstrate sample calls.
* Sample client code for consuming the API in common languages.

### 5. Provide A Feedback Mechanism That Is Clear and Monitored

Have an obvious mechanism for clients to report issues and ask questions about the API. It is critical to respond to issues posted or queries submitted by developers. This demonstrates that the API can be counted on for production usage. If an immediate fix (or even a developer to investigate) is not readily available, respond anyway. Developers will be glad to know when you'll be able to take a look.

When using GitHub for an API's code or documentation, use the associated issue tracker. In addition, publish an email address for direct, non-public inquiries.

If you don't have a support channel specific to your API, you can use the issue tracker at [GSA-APIs](https://github.com/GSA/GSA-APIs/issues). Be sure your support team subscribes to issues there.

### 6. Provide An OpenAPI Specification File
For REST APIs, The API documentation should provide a clear link to the [API's OpenAPI Specification file](https://github.com/OAI/OpenAPI-Specification). This specification was formerly known as the Swagger specification. This specification file can be used by development or testing tools accessing your API.

Using Version 2.0 or later of the specification is recommended. Information about versions can be found here: [OpenAPI Specification Revision History](https://swagger.io/specification/#revisionHistory).

_Exceptions: Not required for SOAP APIs._

### 7. Follow The Standard API Endpoint Design
For REST APIs, An "endpoint" is a combination of two things:

* The verb (e.g. `GET` , `POST`, `PUT`, `PATCH`, `DELETE`)
* The URL path (e.g. `/articles`)

The URL path should follow this pattern for a collection of items:
`(base domain)/{business_function}/{application_name}/{major version}/{plural_noun}`

An example would be:
`https://api.gsa.gov/financial_management/sample_app/v1/vendors`

The URL path for an individual item in this collection would default to:
`(base domain)/{business_function}/{application_name}/{major version}/{plural_noun}/{identifier}`

An example would be:
`https://api.gsa.gov/financial_management/sample_app/v1/vendors/123`

_Exceptions: Not required for SOAP APIs. Not required for APIs that were in progress or production prior to December 2018._


## Recommended Items for Public APIs

### 1. Provide a Testing UI

When you have an OpenAPI specification file (mandatory for public APIs), there are tools that can use this specification file to automatically generate a page where your users can try out your API without too much effort. [Swagger UI is one such tool; here is an example](https://petstore.swagger.io/).

We strongly recommend implementing this item, as it provides a significant enhancement to developer experience and helps ensure proper use of your API. Swagger UI can be implemented on any website, even a static site, with the inclusion of a few JavaScript and CSS files.

[See the Swagger UI project for installation and configuration instructions](https://github.com/swagger-api/swagger-ui).

After downloading Swagger UI, you generally will want a page that loads the Swagger UI and your OpenAPI specification file:

```html
<!-- in the HEAD of the page -->
<link rel="stylesheet" type="text/css" href="swagger-ui.css" >

<!-- in the BODY of the page -->
<div id="swagger-ui"></div>
<script src="swagger-ui-bundle.js"></script>
<script src="swagger-ui-standalone-preset.js"></script>
<script>
  window.onload = function() {
    // openapi.yaml is your OpenAPI specification file (can also be a JSON file)
    const ui = SwaggerUIBundle({
      url: "openapi.yaml",
      dom_id: '#swagger-ui',
      presets: [
        SwaggerUIBundle.presets.apis,
        SwaggerUIBundle.SwaggerUIStandalonePreset,
      ],
      layout: "StandaloneLayout"
    })
    window.ui = ui
  }
</script>
```

Depending on your site, the above is roughly all you need to generate the API testing tool.


## Mandatory Items for Limited Partner APIs


### 1. Provide Public or Protected Documentation

If details of API need to be secured from public viewing, the API documentation can be on a web page that is only accessible to authorized users of the API. This protected documentation may be mentioned on a separate public page, or directly linked from the API Directory.

Your API documentation should provide:
* An overview of the contents of the API and the data sources.
* Required parameters and defaults.
* A description of the data that is returned.
* A description of the error codes that are returned, and their meaning.

Additional nice-to-haves include:
* Explanation of key management and a sample key.
* Description of update frequency.
* Interactive documentation to demonstrate sample calls.
* Sample client code for consuming the API in common languages.

### 2. Provide A Feedback Mechanism That Is Clear and Monitored

On the public or protected documentation page, include a method for users to report issues and ask questions about the API. It is critical to respond to issues posted or queries submitted by developers. This demonstrates that the API can be counted on for production usage. If an immediate fix (or even a developer to investigate) is not readily available, respond anyway. Developers will be glad to know when you'll be able to take a look.

## Recommended Items for Limited Partner APIs

The following items will be beneficial to the users of your APIs. (Full details of these items are linked to the previous section of this document.)

[Add Your API To The GSA API Directory](#1-add-your-api-to-the-gsa-api-directory)
[Use The api.data.gov Service](#2-use-the-apidatagov-service)
[Provide Support For Versioning](#3-provide-support-for-versioning)
[Provide An OpenAPI Specification File](#6-provide-an-openapi-specification-file)
[Follow the Standard API Endpoint Design](#7-follow-the-standard-api-endpoint-design)







## Other Considerations

### Design for common use cases

For APIs that syndicate data, consider several common client use cases:

* **Bulk data.** Clients often wish to establish their own copy of the API's dataset in its entirety. For example, someone might like to build their own search engine on top of the dataset, using different parameters and technology than the "official" API allows. If the API can't easily act as a bulk data provider, provide a separate mechanism for acquiring the backing dataset in bulk, such as posting the full dataset on [data.gov](https://www.data.gov/).
* **Staying up to date.** Especially for large datasets, clients may want to keep their dataset up to date without downloading the data set after every change. If this is a use case for the API, prioritize it in the design.
* **Driving expensive actions.** What would happen if a client wanted to automatically send text messages to thousands of people or light up the side of a skyscraper every time a new record appears? Consider whether the API's records will always be in a reliable unchanging order, and whether they tend to appear in clumps or in a steady stream. Generally speaking, consider the "entropy" an API client would experience.

### Using one's own API

The best way to understand and address the weaknesses in an API's design and implementation is to use it in a production system.

Whenever feasible, design an API in parallel with an accompanying integration of that API.

A few methods to accomplish this include:
* Identifying an internal GSA organization to use your API while also publishing it publicly.
* Creating a web page with a search feature that uses the API.
* Modifying existing web pages or web applications to use the API instead of direct access to the database.

### Developers Are Your End Users
Consider developers who will be using your APIs. Their path to using your API will include discovery and initial investigation, sample API calls, development and testing, deployment and production usage. Consider each of these functions in your documentation, support, and change notification process. Consider performing formal API Usability Testing to understand the developer experience in using your API. More information about this type of testing is available here: [API Usability Testing](https://pages.18f.gov/API-Usability-Testing/).

### Notifications of updates

Have a simple mechanism for clients to follow changes to the API.

Common ways to do this include a mailing list or a persistent issue in the GitHub repository that users can subscribe to. For example: [Notification Thread: Updates to GSA APIs](https://github.com/GSA/GSA-APIs/issues/46)

### Decommission unsupported APIs

If an API can no longer be supported, consider decommissioning the API and removing the documentation. If the API will remain available for historical purposes without support, update the documentation to reflect this.

### Notify current users and update documentation before breaking changes or decommissioning

If an API is going to be decommissioned, retired, or [changed in such a way that current users will be impacted](https://github.com/GSA/api-standards#breaking-changes-backwards-incompatible), it is critical that those users be notified in advance so that they can prepare for the change instead of experiencing an unexpected interruption.  The most common situations where this happens is if an API is going to be decommissioned or if an update to the API will change how the API functions (note that this is where [Versioning](https://github.com/GSA/api-standards#3-provide-support-for-versioning) is important).  If these users aren't notified, their application that consumes the API [may break](https://github.com/GSA/api-standards#breaking-changes-backwards-incompatible) and they may suffer significant consequences from their application's resulting downtime.  Furthermore, experiencing this may seriously degrade the trust that the developer has in GSA's API programs and may cause them to avoid our services going forward if they feel that they cannot rely on them.

In addition to updating the API documentation in advance with a highly visible notice, it is important to use [API analytics](https://github.com/GSA/api-standards#2-use-the-apidatagov-service) to see which API keys have consumed the service in recent months.  You can then use the email addresses associated with each key to send (preferably more than one) notification of the upcoming change.  Note that posting a notice on the API documentation is not sufficient by itself, as many current users will not necessarily revisit the documentatation.  Proactive notification of recent users is essential.

### Use JSON

[JSON](https://en.wikipedia.org/wiki/JSON) is an excellent, widely supported transport format, suitable for many web APIs.

Supporting JSON and only JSON is a practical default for APIs, and generally reduces complexity for both the API provider and consumer.

General JSON guidelines:

* Responses should be **a JSON object** (not an array). Using an array to return results limits the ability to include metadata about results, and limits the API's ability to add additional top-level keys in the future. If multiple results are returned, they can be included as an Array in the JSON object.
* **Don't use unpredictable keys**. Parsing a JSON response where keys are unpredictable (e.g. derived from data) is difficult, and adds friction for clients.
* **Use consistent case for keys**. Whether you use `under_score` or `CamelCase` for your API keys, make sure you are consistent.

### Use a consistent date format

And specifically, [use ISO 8601](https://xkcd.com/1179/), in UTC.

For just dates, that looks like `2013-02-27`. For full times, that's of the form `2013-02-27T10:00:00Z`.

This date format is used all over the web, and puts each field in consistent order -- from least granular to most granular.

### HTTP Response Codes

The following are recommended HTTP Response Codes that API should return. They are based on the [OWASP REST Security Cheat Sheet](https://www.owasp.org/index.php/REST_Security_Cheat_Sheet) from December 2018.

| Return Code  | Message | Description |
| ---  | ----- | --------------------- |
| 200  | OK | Response to a successful REST API action. The HTTP method can be GET, POST, PUT, PATCH or DELETE. |
| 201  | Created | The request has been fulfilled and the resource created. A URL for the created resource is returned in the Location header. |
| 202  | Accepted | The request has been accepted for processing, but processing is not yet complete |
| 400  | Bad Request | The request is malformed, such as a message body format error, missing headers, etc. |
| 401  | Unauthorized | Wrong or no authentication ID/ password provided. |
| 403  | Forbidden | Used when the authentication succeeded but the authenticated user does not have permission to the requested resource. |
| 404  | Not Found | When a non-existent resource is requested. |
| 406  | Unacceptable | The client presented a content type in the Accept header which is not supported by the server API. |
| 405  | Method Not Allowed | The error for an unexpected HTTP method. For example, the REST API is expecting HTTP GET, but HTTP PUT is used. |
| 413  | Payload Too Large | Used to signal that the request size exceeded the given limit (e.g. regarding file uploads and to ensure that the requests have reasonable sizes). |
| 415  | Unsupported Media Type | The requested content type is not supported by the REST service. This is especially effective when you are working primary with JSON or XML media types. |
| 429  | Too Many Requests | The error is used when there may be a DOS attack detected or the request is rejected due to rate limiting. |
| 500  | Internal Server Error | An unexpected condition prevented the server from fulfilling the request. Be aware that the response should not reveal internal information that helps an attacker, e.g. detailed error messages or stack traces. |
| 501  | Not Implemented | The REST service does not implement the requested operation yet |
| 502  | Service Unavailable | The REST service is temporarily unable to process the request. Used to inform the client it should retry at a later time. |

Note: GSA APIs [should be using the api.data.gov service](#3-use-the-apidatagov-service) as a proxy between the client and the API. That service will return additional HTTP codes, prior to the request reaching the base API. Here is a list of those code: [https://api.data.gov/docs/errors/](https://api.data.gov/docs/errors/)

### Pagination

If pagination is required to navigate datasets, use the method that makes the most sense for the API's data.

Common patterns:

* `page` and `per_page`. Intuitive for many use cases. Links to "page 2" may not always contain the same data.
* `offset` and `limit`. This standard comes from the SQL database world, and is a good option when you need stable permalinks to result sets.
* `since` and `limit`. Get everything "since" some ID or timestamp. Useful when it's a priority to let clients efficiently stay "in sync" with data. Generally requires result set order to be very stable.

### Metadata

Include enough metadata so that clients can calculate how much data there is, and how and whether to fetch the next set of results.

Example of how that might be implemented:

```json
{
  "results": [ ... actual results ... ],
  "pagination": {
    "count": 2340,
    "page": 4,
    "per_page": 20
  }
}
```

### Use UTF-8

Just [use UTF-8](http://utf8everywhere.org).

Handle accented characters or "smart quotes" in API output, even if they're not expected.

An API should tell clients to expect UTF-8 by including a charset notation in the `Content-Type` header for responses.

An API that returns JSON should use:

```
Content-Type: application/json; charset=utf-8
```

### Enable CORS

For clients to be able to use an API from inside web browsers, the API must [enable Cross-Original Resource Sharing (CORS)](http://enable-cors.org).

For the simplest and most common use case, where the entire API should be accessible from inside the browser, enabling CORS is as simple as including this HTTP header in all responses:

```
Access-Control-Allow-Origin: *
```

It's supported by [every modern browser](http://enable-cors.org/client.html), and will Just Work in many JavaScript clients.

For additional guidance on CORS, see the GSA IT Security Procedural Guide: API Security CIO-IT Security-19-93.

## API Testing

At its most basic level, API testing is intended to reveal bugs: inconsistencies or deviations from the expected behavior. Continuous testing is also very important to make sure it continues to work when the public has access to it. The risk of putting a bad, and potentially insecure, product on the market is greater than the cost to test it.

**Types of API Testing**
- **Functionality testing** — the API works and does exactly what it’s supposed to do.
- **Reliability testing** — the API can be consistently connected to and lead to consistent results.
- **Load testing** — the API can handle a large amount of calls.
- **Creativity testing** — the API can handle being used in different ways.
- **Security testing** — the API has defined security requirements including authentication, permissions and access controls.
- **Proficiency testing** — the API increases what developers are able to do.
- **API documentation testing** — also called discovery testing, the API documentation easily guides the user.
- **Negative Testing** — checking for every kind of wrong input the user can possibly supply.


## SOAP Web Services
* Provide a WSDL.

  Most platforms will provide this by default out of the box. Leave it active unless you have a strong reason not to. A useful convention is that the WSDL will be available at: {URL Path)?wsdl

* Provide documentation for SOAP web services.

  Users of SOAP web services need documentation, just like REST users.

* See examples of versioning of SOAP web services above.

## Future Topics
Several additional API related topics continue to emerge and will be considered for future updates to these standards.

That list includes:
* Microservices
* Hypermedia and HATEOAS
* Responsive APIs
* GraphQL

### What are we missing?
If you see a future topic we need to consider, take a look at our [contributing page](https://github.com/GSA/api-standards/blob/master/CONTRIBUTING.md) for instructions to share that info.
