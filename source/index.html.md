---
title: elfCLOUD API Reference

language_tabs:
  - json
  - shell

toc_footers:
  - <a href="https://secure.elfcloud.fi/">elfCLOUD Web Home</a>
  - <a href="https://my.elfcloud.fi/">My elfCLOUD</a>
  - <a href="mailto:support@elfcloud.fi">Contact support</a>

#includes:
#  - errors

search: true
---

# Introduction

elfCLOUD back-end API documentation has been collected on this page. The documentation discusses the API connectivity, authentication and authorization, session management and the actual API objects and functions.

The back-end API is divided into two parts. There is a JSON API for managing the account, subscription, folders and files and all the administrative things. The DataItem API is used for file uploads and downloads, we call it **store** to write to the cloud and **fetch** to read from the cloud.

Many account management operations can already be done through the JSON API. Some things like adding and modifying users can currently only be done on the My elfCLOUD web site but these will later on be supported via the API as well enabling different management tools interaction.

# Operating Principle

elfCLOUD is a high security cloud based service for storing and controlled sharing of all your valuable information, media and other digital content. Files stored in elfCLOUD vaults are accessible for authenticated and authorized users from anywhere in the world, unless you choose otherwise. You can easily store photos, videos, other media files, documents, password and credit card records from your favorite tools or, for example, database backups; whatever you value and want to secure.

We promise that your data is never uploaded to the network until it has been locally encrypted. Further, the encryption keys are never created on or transmitted to our servers, so that your data could be decrypted on the server side. Additionally, we continue to work to bring you fine-grained access control mechanisms and secure key distribution tools so that you can decide and control, who you distribute the keys to and share access to your information with.

## Secure Cloud

Typically, a cloud storage providing service performs the encryption and decryption on the provider’s servers. This is done either by storing the encryption key on the provider’s server or by transferring the key to the server for the duration of the cipher processing. Provider held encryption key can also be password protected, letting the provider’s server use the key whenever the user enters her password to use the service. In this scenario, the provider may or may not maintain a saved copy of the user’s key password.

Be it any of the above or a mixture of the approaches, when the encryption and decryption is performed on the provider side, ultimately the end user is placing ultimate trust on the provider. For example, a simple password gaining you access to a browser based file repository, from anywhere in the Internet, means that the provider can decrypt your files at will.

In other terms, the possibility of a severe data leakage is greatly increased, when the provider infrastructure alone holds everything needed to turn your data into readable unencrypted data.

We at elfCLOUD believe in a different approach. In the elfCLOUD model, the encryption key is never created on or transferred to the elfCLOUD servers in a way that it would enable decryption of customer files. In fact, the cipher keys are never transferred to the elfCLOUD servers at all, unless you choose to utilize our corporate targeted elfCLOUD Cloud Assisted Cryptographic Key Distribution mechanism, but even then, the keys are strongly protected and not usable when being transmitted through the cloud infrastructure. Encryption keys are created, stored and handled solely on the customer terminals and devices.

## Secure Storage For Your Data

Main principle and starting point for all elfCLOUD operations is that all information is securely encrypted on the client computer or device before the information is sent to the network. The information is encrypted on the user’s computer, mobile phone, tablet or other device before it is transmitted even to the customer’s local corporate network. Data in its readable format is processed only on the end user’s device, data is always locally encrypted, transferred and stored in fully encrypted format and then locally decrypted for use of authorized end users.

The elfCLOUD infrastructure and the elfCLOUD server side supports use of any encryption algorithm. Current official elfCLOUD client libraries and client applications support AES encryption with variable key sizes up to 256 bits.

The user can still opt, on data item basis, to not use encryption at all, if the stored information is not security classified and the user wishes to optimize performance for larger quantities of data, or wants to enable sharing of the data without key distribution. However, according to elfCLOUD design principles, disabling encryption has to always be an active choice from the user’s side and will never be default operation mode of the clients.

## Complete and permanent deletion of data

All information is really, truly, deleted and without unnecessary delay. Some providers have gained questionable fame by keeping “deleted files” for several years after the user deleted the files or even terminated their whole accounts. Further, even if we didn’t delete the files, it would be technically impossible to access any of the content because everything has been encrypted locally on the client side and the keys have never even visited the elfCLOUD servers in a usable form.

One of our promises is that deleted information will be gone forever.

## Safe Transferring of Your Files And Other Data

All communication between end users’ computers and the elfCLOUD servers is fully TLS/SSL encrypted (Wikipedia article) on the logical transport layer of the elfCLOUD communications stack. This makes it literally impossible for any unauthorized thirdparty to eavesdrop on the conversions between the customer and the elfCLOUD servers. TLS/SSL encryption covers all elfCLOUD API usage, user authentication, vault management, file transfers, even browsing the elfCLOUD web site. The elfCLOUD APIs and the My elfCLOUD application are not even available without the encryption, so you can rest assured that any working client application actually uses encrypted network connections.

All elfCLOUD interfaces are based on the HTTPS protocol, the secure internet protocol used by practically all Internet banks and other sites dealing with data of sensitive nature. The use of standards based HTTP/SSL protocol, on its standard port tcp/443, makes it well reachable and accessible from those even more restricted corporate networks that have a strict outbound network traffic policy in place.

Many strict security policies deny HTTPS outbound connections to previously unknown IP addresses. elfCLOUD servers’ public IP addresses are publicly available information and any upcoming changes are informed well ahead of time for customers subscribed to these notifications. This makes it easy for environments with highly controlled outbound connections to efficiently utilize the elfCLOUD services.

## Open Infrastructure

The elfCLOUD infrastructure does not restrict its end users to a specific set of client applications, file formats or data structures. The elfCLOUD vaults allow you to define any data structure whatsoever. You can choose to have hierarchies, we have built a native support for a tree model. In addition to the free organizing of your data, you can also freely choose the structure and contents of your files, the data items. Our vaults and clusters are analogous to folder on a filesystem and the data items analogous to regular files, except that the elfCLOUD files are transparently secure and available from anywhere, for your private or shared use.

Any application storing data into a elfCLOUD vault can choose freely how the data will be saved, whatever approach suits the best. Adapting existing applications to use elfCLOUD storage is very straightforward. In its simpliest form, saving to a elfCLOUD vault is even easier than saving to a local file. Plus, you get all the benefits of cloud storage without any security concerns, including not having to worry about backing up the data, distributing it between company network segments and thirdparties, let alone maintain concerns about continuously protecting your data from leakage.

### No vendor lock-in

Using elfCLOUD for your secure cloud purposes does not lock you in with us or our service offering. All of your information can at any time be completely exported via the My elfCLOUD service. You can take a vault export for your external backup purposes, or if you simply prefer to keep a local copy just in case. In case of really large volumes of data, we offer a possibility for delivering you a local copy on a transportable physical media. Of course, your data will always be exported in the exact strongly encrypted format as you have stored it.

All data stored in a elfCLOUD vault can be transferred to customer’s own environment, where encryption can be removed and the data imported to the appropriate target system. Because of the client side encryption always being the elfCLOUD default setting, we will not be able to deliver offline backup medias in plaintext format. Decryption must be done by the customer, optionally by using the tools provided by elfCLOUD. As our encryption scheme is standards based, it will also be possible to perform the decryption by using other compatible tools such as the OpenSSL suite.

## Open source

All official elfCLOUD client applications and client libraries are published with open source code. This enables interested users and the community in general to ensure, even by a source code review, that the encryption keys or unencrypted data is never transmitted outside the client device. Further, the open source client packages and libraries make it possible for developers to develop their own elfCLOUD compatible client applications in a faster schedule. elfCLOUD client applications and client API libraries are licensed free of charge to be used as starting points for any other applications. For more information, please refer to our developer pages and the download page.

## Open elfCLOUD server side interfaces

Our open cloud or server side interfaces make writing your own elfCLOUD client implementations relatively straightforward, eliminating the guesswork from how the client-server interface works. If you prefer not to use any of the existing elfCLOUD client libraries, or one does not yet exist for your target platform, please feel free to implement your own to the extent that you require for your application to function.

The cloud side interfaces are based on industry standard protocols and notations, making it possible and reasonable to utilize already existing software components for implementing the API counterparts. This will directly accelerate elfCLOUD integration also in cases where a custom integration is preferred. The interfaces are described on the developer pages.

## Backups and redundant storage systems

elfCLOUD cloud architecture has been designed from the scratch with security in mind. And not just in mind, but as the top most priority. elfCLOUD vault data is handled only on redundant storage systems and disk surfaces part of the elfCLOUD server infrastructure. In addition to continuous mirroring and distribution of active server disk arrays, all data stored in the elfCLOUD vaults is frequently and repeatedly backed up to physically distinct locations. All elfCLOUD servers are located in Finland and we never transfer your data to abroad (however, due to the nature of Internet, we of course cannot influence the physical route the IP packets take between user workstations and our servers, but then again, when you keep encryption enabled, the data will never leave your computer without proper protection).

# Developer Info

elfCLOUD offers all application developers open elfCLOUD programming interfaces and fully open source elfCLOUD client libraries to be used in any proprietary applications. The client libraries can be used as they are, or customized as needed. The use of the client libraries does not obligate the developer to publish his or her own application’s source code. However, the use of elfCLOUD client library for non-elfCLOUD related purposes is not allowed.

The elfCLOUD client libraries act as reference client implementations. Developers may build their own client implementations by using the library code as a starting point, or even integrate directly to our HTTPS (JSON/RAW) interfaces with a completely proprietary client side implementation. Version updates of the server side interfaces will be kept downwards compatible as much as possible to ensure compatibility of clients implemented against earlier releases.

Secure storing of data to the elfCLOUD service is easy and provides a location, time and device independent, military grade secured, way of storing your application data. Developers can provide their application’s users a seamless use experience, as the end users can access their data securely from anywhere, anytime.

## Developer Registration

Developer accounts can be registered by following this link:

<a href="https://my.elfcloud.fi/register/developer">https://my.elfcloud.fi/register/developer</a>

Developer account users will be able register new client applications, access pre-release versions of the elfCLOUD server interfaces and other new features of the platform.

Sometimes the pre-release features can still be ongoing verification phases and dev-logins may be redirected to non-production servers if you use pre-release API levels.

<aside class="warning">
Developer accounts are not recommended for production grade continued data persistence, especially not if you intend to use Beta APIs! The recommended approach is that you create a developer account and only use it to register your client and API key and conduct your API test use. Then have a separate production account (home or corporate) for all serious use. Same API keys will work for any account once registered.
</aside>

## Integrate with elfCLOUD

Any developer can easily configure their applications to utilize elfCLOUD storage services by following these steps:

1. Sign up for a My elfCLOUD developer account or logon with an existing developer account.
2. In My elfCLOUD, go to My Profile -> Development and register a new client type by clicking Add. You need to give a name and a short description for your client application. Additionally, you need to define the primary [vault type](#client-types,-api-keys-and-vault-types) that your application will handle.
3. Submit the form and My elfCLOUD will reserve a unique API key that your client will need to use when authenticating on the server.
4. Develop your application, allow for the end user to give her elfCLOUD username and password in the application settings or use your application own credentials depending on your setup.
5. Be prepared to provide the encryption key (and an initialization vector if you prefer) to be used for encrypting the data items. The key length will depend on the encryption mode used, for AES256 the key is 32 bytes and the Initialization Vector is 16 bytes of raw binary data. Alternatively, you can use the elfCLOUD Backend APIs to fetch user specific PKI keys and content keychain backup from the cloud similar to how elfCLOUD mobile clients work.
6. Use a suitable elfCLOUD client library (or your own implementation against the Backend APIs) for storing and retrieving data from the cloud. When your application opens a server session, it’ll need to provide the API key to identify the client type, as well as end user’s username and password to authorize the access.
7. Your application can now access vaults that the end user has access to, and that have a vault type identified by your client application identity as specified by the API key.

# Client libraries

elfCLOUD client libraries have currently been implemented on Python, Java, C++ and Objective-C. These are open sourced and licensed under the Apache 2.0 license. Latest versions are available by contacting support@elfCLOUD, you should currently prepare for some packaging delay though. (The ones you might find on Github are old and should not be used for anything new).

# Client apps

## elfCLOUD Clients

elfCLOUD client applications are available for download from the <a href="https://secure.elfcloud.fi/">elfCLOUD site</a>. Clients that are only available for paid subscriptions are available after subscribing. Mobile apps are available from Google Play Store and the Apple AppStore.

Note that for developers and administrator users, especially in terminal environments, elfCLOUD Weasel is a Python based command line tool that can be used e.g. from Bash scripts to access the cloud vaults. For example with Weasel, you could encrypt and push server backups to the cloud quite easily.

## FUSE Client

The FUSE Client allows you to mount elfCLOUD vaults to mountpoints in your local filesystem (Linux, OS X).

elfCLOUD FUSE Client implementation has been contributed by Tuukka Pasanen, Ilmi Solutions Oy. Please refer to the Github page for usage instructions and bug reports.

Link to the Github repository:<br>
<a href="https://github.com/illuusio/elfcloud-fuse">https://github.com/illuusio/elfcloud-fuse</a>

Compiled Linux packages:<br>
<a href="http://download.opensuse.org/repositories/home:/illuusio:/elfcloud/">http://download.opensuse.org/repositories/home:/illuusio:/elfcloud/</a>

Mac OS X distribution:<br>
<a href="https://europa.ilmi.fi/opensource/elfcloud/macosx/elfcloud-fuse-15.06.dmg">https://europa.ilmi.fi/opensource/elfcloud/macosx/elfcloud-fuse-15.06.dmg</a>

## API Levels

So far three versions of the public API have been created. Versions 1.0 and 1.1 are deprecated and the documentation has been removed from the site. These are still functional to support legacy applications, but all applications must eventually be updated to use the API 1.2 version.

API 1.2 is the current level and has been extended with new functions and with new parameters to existing functions without breaking backwards compatibility to the original API 1.2 specification. Version 1.3 will introduce some changes to how data items are referred to as versioning, transactions and account level recycle bins will be introduced.

Client defines the used API level by embedding the version number into the server URL, see [Connections and URLs](#connection-and-urls). A client must retain the used API level throughout its session, i.e. calling some functions from different API levels than others is not supported. The latest API level always supports the broadest functionality set.

All elfCLOUD server production instances always implement the latest and prior still supported API levels. For larger upcoming API changes, beta API call points may be provided for developer use.

<aside class="notice">
API levels 1.0 and 1.1 have been deprecated and shall not be used for any new implementations. For the most part they are still supported but will be removed in future updates.
</aside>

# Examples

> Example request

```json
{
  "method": "remove_dataitem",
  "params": {
    "parent_id": 35,
    "name": "Unnecessary file.doc"
  }
}
```

```shell
curl -X POST --cookie-jar cookies.tmp --cookie cookies.tmp --data '{"method": "remove_dataitem", "params": { "parent_id": 1, "name": "Unnecessary file.doc"}}' --header "Content-type: application/json; charset: utf-8" https://api.elfcloud.fi/1.2/json
```

> Example response

```json
{
  "result": null,
  "id": null
}
```

```shell
{"result": null, "id": null}
```

On the right, there are JSON examples for all API calls and Shell/curl examples for selected API calls for getting started easily. Note that the curl command line specifies a file to be used as cookie storage (`cookies.tmp`) in order to be able to maintain session state.

Going forward we'll add examples for Python, Java and maybe C++ how our client libraries can be used. Depending on your platform, you can choose to integrate against the raw server APIs or embed the client libraries that already implement crypto and the API use.

Most API functions that perform the requested operation (and don't create or get any objects) return a null result when successful. Functions used for creating or modifying something, for example a Vault, will return the Vault instance JSON object.

Examples aim to show complete requests and responses that can be directly used for testing, of course you need to use containers and dataitems that your authenticated user actually has access to.

# Connection and URLs

The interface is based on a typical HTTP dialogue between the elfCLOUD client and the elfCLOUD server. The client always connects to the server.

The HTTP connection may utilize keep-alive, but this is not required. The HTTP connection is always TLS encrypted and the client can ensure that it is dealing with an authentic elfCLOUD server by verifying the certificate.

There are two distinct APIs call points available: JSON API and DataItem API. The Data Item API is used to create, update and read data items (files) contained in elfCLOUD vaults and clusters (folders). Through the JSON API you can list, add, rename and remove vaults and clusters, list and delete data items as well as perform many account management operations such as user groups, organization units PKI certificate management operations.

Please note that all transactions are performed within the authorization context of the user you authenticate as. Further, the API key of the connecting client will determine the types of vaults (top level folders) available, as well as the types of vaults that can be created.

The JSON API URL on the elfCLOUD servers is `https://api.elfcloud.fi/M.m/json` where M.m is the API version number such as `1.2`.

Typical usage flow is to first use the JSON API to authenticate (call `auth`), list some vaults, clusters and dataitems to navigate in the storage tree (call `list_vaults`, `list_contents` and others), perform file transfers with the Data Item API and finally terminate the session through the JSON API (call `term`).

Using the Data Item API requires an authenticated HTTP session, so a JSON API `auth` call must be performed before the Data Item API Store and Fetch calls can be performed. Data Item API URLs on the elfCLOUD servers are `https://api.elfcloud.fi/M.m/store` for the Data Item Store request and `https://api.elfcloud.fi/M.m/fetch` for the Data Item Fetch request.

# Session Authentication

> To authenticate (see [auth](#auth) function for details)

```json
{
  "method": "auth",
  "params": {
    "username": "admin@demo.fi",
    "auth_method": "password",
    "auth_data": "TheCorrectPassword",
    "apikey": "atk8vzrhnc2by4f"
  }
}
```

```shell
curl -X POST --cookie-jar cookies.tmp --cookie cookies.tmp --data '{"method": "auth", "params": {"username": "admin@demo.fi", "auth_method": "password", "auth_data": "TheCorrectPassword", "apikey": "atk8vzrhnc2by4f"}}' --header "Content-type: application/json; charset: utf-8" https://api.elfcloud.fi/1.2/json
```

> And the response will be something like

```json
{
  "id": null,
  "result": {
    "client": {
      "description": "Standard elfCLOUD client",
      "name": "elfCLOUD",
      "types": [
        "fi.elfcloud.backup",
        "fi.elfcloud.datastore"
      ]
    },
    "role": [
      {"user-role": "ectm.mycontact.accept"},
      {"user-role": "ectm.mycontact.revoke"},
      {"user-role": "account.administrator"},
      {"user-role": "ectm.acctrust.accept"},
      {"user-role": "ectm.acctrust.revoke"},
      {"user-role": "ectm.eimgmt.admin"},
      {"user-role": "ectm.eimgmt.signer"},
      {"user-role": "account.users.manage"},
      {"user-role": "account.groups.manage"},
      {"user-role": "account.organizationtree.manage"},
      {"user-role": "ectm.acctrust.invite"},
      {"user-role": "ectm.mycontact.invite"},
      {"user-role": "ectm.keyshare.internal.all"},
      {"user-role": "ectm.keyshare.external.all"}
    ],
    "license": [
      {
        "count": 20,
        "license": "ECTM",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECMA",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECFS",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECSB",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      }
    ],
    "user": {
      "id": 269,
      "email": "support@elfcloud.fi",
      "name": "admin",
      "lastname": "",
      "account": {
        "id": 172,
        "company_name": "demo.fi",
        "name": "demo.fi",
        "type": "corporate"
      },
      "organization_unit": null,
      "eula_accepted": true,
      "firstname": "",
      "telephone": null,
      "lang": "fi"
    },
    "account_admin": {
      "id": 269,
      "email": "support@elfcloud.fi",
      "name": "admin",
      "lastname": "",
      "account": {
        "id": 172,
        "company_name": "demo.fi",
        "name": "demo.fi",
        "type": "corporate"
      },
      "organization_unit": null,
      "eula_accepted": true,
      "firstname": "",
      "telephone": null,
      "lang": "fi"
    }
  }
}
```

```shell
{"result": {"account_admin": {"lang": "en", "account": {"company_name": "demo.fi", "type": "corporate", "id": 172, "name": "demo.fi"}, "telephone": "+3581122334455", "name": "admin", "firstname": "Administrator", "eula_accepted": true, "lastname": "Demo Corporation", "organization_unit": null, "id": 269, "email": "support@elfcloud.fi"}, "client": {"name": "elfCLOUD", "types": ["fi.elfcloud.backup", "fi.elfcloud.datastore"], "description": "Standard elfCLOUD client"}, "role": [{"user-role": "ectm.mycontact.accept"}, {"user-role": "ectm.mycontact.revoke"}, {"user-role": "account.administrator"}, {"user-role": "ectm.acctrust.accept"}, {"user-role": "ectm.acctrust.revoke"}, {"user-role": "ectm.eimgmt.admin"}, {"user-role": "ectm.eimgmt.signer"}, {"user-role": "account.users.manage"}, {"user-role": "account.groups.manage"}, {"user-role": "account.organizationtree.manage"}, {"user-role": "ectm.acctrust.invite"}, {"user-role": "ectm.mycontact.invite"}, {"user-role": "ectm.keyshare.internal.all"}, {"user-role": "ectm.keyshare.external.all"}], "user": {"lang": "en", "account": {"company_name": "demo.fi", "type": "corporate", "id": 172, "name": "demo.fi"}, "telephone": "+3581122334455", "name": "admin", "firstname": "Administrator", "eula_accepted": true, "lastname": "Demo Corporation", "organization_unit": null, "id": 269, "email": "support@elfcloud.fi"}, "license": [{"count": 20, "valid_till": "2015-02-27T23:59:59.457050+00:00", "license": "ECTM"}, {"count": 20, "valid_till": "2015-02-27T23:59:59.457050+00:00", "license": "ECMA"}, {"count": 20, "valid_till": "2015-02-27T23:59:59.457050+00:00", "license": "ECFS"}, {"count": 20, "valid_till": "2015-02-27T23:59:59.457050+00:00", "license": "ECSB"}]}, "id": null}
```

> To invalidate (see [term](#term) function for details) your session

```json
{
  "method": "term",
  "params": {
  }
 }
```

```shell
curl -v -X POST --cookie-jar cookies.tmp --cookie cookies.tmp --data '{"method": "term", "params": {}}' --header "Content-type: application/json; charset: utf-8" https://api.elfcloud.fi/1.2/json
```

> Server response:

```json
{"result": {}, "id": null}
```

```shell
{"result": {}, "id": null}
```

The API session management is based on an HTTP cookie (`elfcloud.session.id`). Upon processing the authentication request, elfCLOUD server issues the cookie and returns it to the client in the the HTTP response headers with a Set-Cookie header. The client application is then required to include the HTTP cookie in all subsequent requests sent to elfCLOUD servers.

The session will timeout when there is no activity for a longer time (currently 3 hours for the API sessions, note this is quite long session timeout so for example a mobile application can preserve resources by not having to refresh/poll/re-authenticate all the time).

If the session has expired and the application wants to resume use, it must re-authenticate and start over.

<aside class="notice">
For API key explanation please read the next section carefully!
</aside>

# Password Complexity

User logon passwords have the following complexity criteria:

- At least 8 characters long
- Must contain at least one lower case character in a-z 
- Must contain at least one upper case character in A-Z
- Must contain at least one decimal digit 0-9 or non-alphanumeric (other than a-z, A-Z, 0-9 or underscore (_)) character
- When changing, must be different than the previous password

# Data Management

elfCLOUD offers a data structure and data hierarchy neutral data storage for all your information. Therefore, you can save all the important information that you prefer to preserve such as documents, photos, videos, passwords, etc.

Thanks to our high security standards, we can guarantee that your data cannot be accessed by anyone else, including the elfCLOUD staff, all ISPs and other providers along the route. You can subscribe to even several terabytes of capacity, more will always be available, when your data storage needs grow. However, you are not restricted by fixed size subscription packages, you can order exactly the amount of capacity that you need and scale it from there.

Saving your existing files to elfCLOUD data vaults is just the beginning. Developers can easily integrate elfCLOUD storage to any application, regardless of the nature or platform of the application and the data it processes. When an end-user uses a elfCLOUD compatible application, use of the high security vaults is completely transparent. The application will ideally perform just as if it were saving the data locally, automatic and silent. The applications can freely organize their vault structures how they see it best serving their needs.

## Storage Concepts

elfCLOUD defines data storage concepts of a vault, a cluster and a data item. These are created and manipulated over the offered HTTP/SSL based cloud side interfaces (HTTP/JSON and HTTP/RAW). The interface specifications are open and freely available through our developer pages. The storage concepts are described below.

Information is organized into vaults, that may contain independent clusters or hierarchical cluster trees. The data is stored as data items, which can be placed in any vault or cluster container. Each of these concepts, their characteristic and possible use is explained below in their respective sections.

## Managing the Data Storage

elfCLOUD offers two HTTPS based interfaces for primary management of the data storage. The structure is created and maintained over a request/response HTTPS interface using JSON formatted messages. This is called the elfCLOUD JSON API.

Actual data items containing user data and their meta information are read and written to over another HTTPS interface, optimized for fast request processing. This is called the elfCLOUD Data Item API and uses custom HTTP request and response headers for coordinating the transaction while the HTTP body is reserved for the data. The data payload is always transmitted in raw encrypted format.

Both interfaces can be utilized by using one of the elfCLOUD client applications or any other application built to support the elfCLOUD APIs. For application developers, we offer the choice between using ready made elfCLOUD client libraries for straightforward object-oriented data processing or developing their own client implementations against our open elfCLOUD HTTPS interfaces. The client libraries are available for several popular platforms and programming languages, including Python, Java, C++ and Objective-C.

All approaches, whether custom built or our standard supported client, are referred to as client applications when we talk about client applications performing actions against the elfCLOUD cloud interfaces.

elfCLOUD accounts, vault permissions, capacity and backups are managed on the My elfCLOUD site.

## Vault

In order to save any data, at least one vault must exist. Vaults can contain any number of clusters and data items as vault top level's immediate child nodes. So data items can be stored either directly on the vault level or into any of the clusters created inside the vault. Vaults can be thought as folders in the elfCLOUD account's root path.

All elfCLOUD account types (corporate, private consumer and developer) can contain an unlimited amount of vaults.

## Vault Owner

Vaults are owned by the user that created them. Vault owner and the account administrator always has all possible permissions to the vault and all the clusters and data items that the vault contains. The owner can always read and modify the vault contents and edit the vault's permissions. They can view and search the service audit history log, modify user permissions and generally manage the elfCLOUD account on the vault. Any user can create new vaults for user with the client applications.

## Vault Permissions

Vault's permissions define the read, write and administrative access to each vault. Read permission allows viewing the vault's contents. Having read and write access to a vault means that the user can view and modify the raw encrypted information of the data items. Whether the user is in possession of the corresponding encryption keys, therefore being able to decrypt and view the information, is a completely independent matter. Therefore, it is ultimately the combination of vault access and having the specific encryption keys that controls the access to the saved information.

When a new vault is created, the default privileges are granted only the vault owner and the account administrator (see below). Vault permissions to other internal and external users must be explicitly granted.

<aside class="notice">
Remember: Being able to read data items does not mean being able to view the information. For decrypting the data, yet another layer of security exists, corresponding cryptographical key must be in user's possession.
</aside>

### Read

- Vault is visible in the vault listing (assuming client API key matching the vault type in question)
- Browsing the vault’s structure, ability to view cluster names, data item names and meta information (indexing)
- Reading the raw binary, encrypted, data item contents

### Write

- Permission to create new data items into vaults and clusters
- Permission to modify existing data items within the vault
- Removing data items from the vault
- Ability to create and remove clusters within the vault

Vault level permissions that can be granted only to internal users (users of the same account):

### Permissions view

- Viewing of the vault’s permissions, showing which permissions are granted to which users, including internal and external users

### Permissions modify

- Permission to modify vault’s permissions, grant to and revoke permissions from any internal or external user (account administrator and vault owner excluded; permissions of these two special users cannot be altered)
- Permission to add or remove internal account users from vault’s access list (however, account administrator role is required to add new users to the account)
- Permission to add to and remove external users from the vault’s access list, and to grant read/write access to any external user

Vault permissions can generally be granted to users belonging to the same elfCLOUD account. External users, who may or may not be existing elfCLOUD users, can be granted external access by sending them a temporary and one-time use elfCLOUD vault access token.

It is worth noting that eventhough vault owner and the account administrator can always read the raw contents of the vault’s data items, the information may be, and usually is, encrypted by client applications with such cipher keys, that the administrator users do not have. This is beneficial, when for example an IT employee has set up a vault for corporate’s management to use and is responsible for maintaining a local backup copy of the vault. In such a scenario, confidentiality of the information is effortlessly guaranteed with discretionary encryption key distribution.

<aside class="notice">
elfCLOUD is an ideal solution for guaranteeing information confidentiality also inside an organization, not only keeping it safe from lurking outsiders.
</aside>

## Account Capacity

elfCLOUD vaults always belong to the elfCLOUD account, whose user has created the vault in question. The storage capacity consumption of each vault contributes to the total consumption of that account. Storage capacity subscriptions are always made on account level, so vault owners do not individually need to manage any subscription information. The account administrator will be able to view percentage of total subscription capacity consumed by each vault.

## Clusters

elfCLOUD clusters are analogous to folders in computer filesystems and are used to group data items into logical collections. Clusters can form hierarchies without any explicit limitation of tree depth. First cluster(s) of a vault must always be created on the vault top level, just like you would create a Documents folder in the root of a disk drive. Further clusters can be created as equal siblings or as child nodes of an existing cluster.

## Data Items

Data items are the entities containing the actual end user data, analogous to a file in a traditional filesystem. Each data item has a name, that must be unique within the context of the parent container entity (vault or cluster). When observing the uniqueness of the name, both clusters and data items in the same parent container are taken into account. Data item store and fetch requests use the data item name for identifying the data item.

In addition to regular fetch and store (read and write) operations, that cover the entire data item, the elfCLOUD API supports random access to fragments of existing data items. Naturally, fetching byte ranges from existing data items is supported as well.

The information stored into the data items, its format and structure thereof, is completely up to the client applications. Application developers set their applications to use the elfCLOUD Data Item API (HTTP/RAW).

elfCLOUD internal accounting, data structures and intelligent storage algorithms guarantee flexible data management for logical data item sizes ranging from zero bytes to several terabytes of data.

# Managing Permissions

elfCLOUD user permissions are defined on vault basis. Possible vault level permissions and their meaning are presented above. Upon creating a new vault, the vault owner and the account administrator of the same elfCLOUD account are the only users having any access to the vault. Of course, this can also be a single user acting in both roles, if the account administrator is creating new vaults.

It is important to notice, that vault read access will give a user the cluster and data item meta information and the raw encrypted data. Only this combined with the valid encryption key for these data items, will render the information contents readable for the user. Local client performed encryption is a significant layer of security in keeping the information confidential, and cannot be bypassed through mere vault permissions.

## Account administrator user

All elfCLOUD accounts have a designated account administrator user account, created during the registration process. The concept of the account administrator is the same for all account types. Additional users can be made administrators on the My elfCLOUD / Users page.

Account administrator has the privilege to maintain the elfCLOUD account’s user accounts from within the My elfCLOUD site. The administrator can create, modify (including password reset) and remove user accounts. The administrator can also view all contained vaults’ basic information and control the vault access control lists. The account administrator has same privileges to each vault as their corresponding vault owners. This makes it possible for the accont admin to substritute the vault owner in all management tasks, such as taking manual backups and granting access when appropriate.

Vault rename and export (manual export for example to backup to end user’s own local storage) functions are only allowed for elfCLOUD account adminstrator and vault owners.

## Internal Account Users

Users under the same elfCLOUD account can be assigned read, write, permissions view and permissions modify permissions on vault basis, as explained above. It is further possible to finegrain the access rules by controlling allowed client IP addresses and times of access.

## Granting Access to External Users

A user that is allowed to modify a vault’s access permissions, can grant external users (that is, users belong to other accounts) to access the vault by adding rules to the vault access control list. The external user must be linked to the current user's account via either an account level trust relationship (Account Trust) or a person level trust relationship (`My Contact` as shown in Beaver). Please consider that any user, be she administrator or not, can initiate or confirm person level trust relationships with other users. It is the Vault ACL Edit permission that controls who can distribute access to that vault.

# Usage Monitoring

The elfCLOUD server maintains a usage and audit log of all transactions on the elfCLOUD accounts (users, vaults, subscriptions) as well as storage concepts (vaults, clusters, data items) related transactions. Account administrator has the privilege to view the account level audit information. When viewing the audit log, it is possible to filter the entries by selecting certain search criterium. An audit log entry is created from each user authentication and all the performed transactions in the context of the authenticated usage session. Audit log is maintained all the way to the data item level, but the actual data items contents are not traced or saved on the server.

# Clients, API Keys, Vault Types

Each vault has a type attribute, the vault type. This is a string value that defines the purpose of the vault. Technically it does not enforce any specific structure, but is more of a label that implies the expected structure of the vault's contents and the intended use of the vault. The purpose is to only show an application vaults that it will understand how to handle (maintaining data integrity and structural sense of the vault).

For example, let's say a Vendor Company Corporation (vendorcompany.com) is going to develop a elfCLOUD enabled Password Management application. The vendor will register a new client type through My elfCLOUD, and will define `org.vendorcompany.pim-utility` as an allowed vault type string for this client. Now, My elfCLOUD allocates an API key for this specific client application. The Vendor develops the application such that it will use the allocated API key along its end user's credentials when it authenticates on the elfCLOUD servers.

The application will then create all its vaults with the vault type `org.vendorcompany.pim-utility`. This is allowed, because the API key is used to lookup the client registration, where the vault type was defined. The elfCLOUD itself won't enforce any specific vault structure as a result of the vault type used, the string merely helps applications to find the vaults they know how to handle. Other applications, that bind with an API key that does not map to this vault type, cannot query or access these vaults.

It is worth noting, that a client can support multiple vault types. A larger application could create separate vaults for its configuration data, one for transactional customer data and third for reporting data. Each vault can have its own vault type and still be accessed from a single client application.

Equally, same vault type string can be registered for multiple client types. There can be another PIM application, that supports the same vault structure (if for nothing else, then to import data from another vendor's application); in this case using the same vault type might be sensible.

While a client's API key is meant to be used only by the application's developer and is usually not visible to the end-user, it is not ultimately secret and is not any guarantee of restricted vault access. Rather, this is accomplished through user authentication and client side encryption.

# Accepting EULA

> This is the error message when auth is otherwise successful but the user has not accepted the latest required EULA level.

```json
{
  "error":{
    "message":"User has not accepted EULA",
    "code":"ECAuthException",
    "id":204
  }
}
```
Each end-user must logon to the My elfCLOUD web application once before they can utilize the API interfaces. Or, if the client is capable of recognizing this error message, is to present the elfCLOUD EULA to the user and should the user accept the terms, issue the respective API call to confirm EULA acceptance.

When an API authentication is attempted with credentials of an user who has not accepted the service terms, the following exception is returned in the JSON response body.  This is done for every user, not account!

# JSON API

Request body must consist of a well-formatted JSON block, a dictionary. It will must have two keys, `method` and `params`. All understood and successfully processed requests return a JSON dictionary with `result` and `id` elements. The `id` element is reserved for future use and is set to `null` for now. The `result` element data type and contents will depend on the API function called, often its value is a JSON dictionary or JSON array type. If the request is not understood or an exception condition is raised, the response will be an [error message](#exception-handling).

HTTP Request Content-type must be set to `text/json; charset=utf-8`. Content-length header must always be present. The method key defines the elfCLOUD API method that is being called, such as `auth` for an authentication request. The `params` key is a dictionary container, that must contain at least the required parameters of the called method, if any.

The elfCLOUD API is designed to be <a href="http://www.jsonrpc.org/specification">JSON-RPC 2.0</a>-compliant with following exceptions in the request body:

    jsonrpc-element can be omitted. Then server will also omit the jsonrpc element from response.
    id-element can be omitted. Then response will be have id-element with value null.

All existing JSON-RPC 2.0 libraries will work as long as they implements the support for HTTP Cookies, which is mandatory requirement when using elfCLOUD API. All available methods can be found at the API Reference section.

## Exception handling

> Couple of example JSON API exception responses

```json
{
  "error":{
    "message":"Client authorization failure.",
    "data":"ECClientException",
    "code":101
  }
}

{
  "error": {
    "message": "Permission denied.",
    "code": 105,
    "data": "ECClientException"}
}
```

The exception handling follows the JSON-RPC 2.0 -specification, and method call returns special kind of response when an error occurs. The response contains an error-object and it has two required and one optional members:

Field | Type | Description
----- | -----| -----------
code | integer | Error code
message | string | User-friendly error message, which should be used, when presenting error messages for end-user. Error message language will follow user's profile setting (My elfCLOUD).
data (Optional) | string | Used for additional details, typically an exception class name is provided. Client implementations should rely on the `code` field.  

# Data Item API

The Data Item API is used to create, update and read data items contained in elfCLOUD vaults and clusters. Using the Data Item API requires an authenticated HTTP session, so a JSON API `auth` call must be performed before the Data Item API Store and Fetch calls can be performed.

The Data Item API only uses HTTP request and response headers for request definition and result information. The HTTP request and response body payload is dedicated to the data item contents (file data).

The same HTTP cookie as provided by the `auth` call is valid for both the JSON API and the Data Item API.

## HTTP headers

The following elfCLOUD specific header extensions are used in HTTP Requests:

Header | Description
------ | -----------
X-ELFCLOUD-STORE-MODE | Define mode of the Data Item Store operation. Possible values: NEW, REPLACE, APPEND, PATCH (see below)
X-ELFCLOUD-KEY | Key (base64-encoded) of the data item to be created/modified, unique within the container context.
X-ELFCLOUD-PARENT | Unique numeric identifier of the elfCLOUD container (vault or cluster) containing the data item.
X-ELFCLOUD-OFFSET | Used only in PATCH mode. Defines the byte offset (signed integer in printable string representation) of the current data item content, where the write operation should start from. Value of 0 marks the beginning of the data item, any positive value indicates the number of bytes forward from the beginning of the data item, any negative value indicates the number of bytes backwards from the end of the current data item.
X-ELFCLOUD-META | String of elfCLOUD client specified meta information. Server supports header values up to 8000 characters in length. With the STORE request, the server will persistently store the meta information. On the FETCH request, it will return the stored information unmodified. X-ELFCLOUD-META header is only processed by the elfCLOUD client libraries. Server will store and return the header value untouched. The header consists of KEY:VALUE pairs. Any colons in the value fields are escaped by a backslash '\'. VALUE can be empty, resulting in back-to-back colons in the meta header. Header will still remain parseable, as the keys are never empty and therefore confusion with the termination marker should not happen. Header always terminates with the double-colon :: termination marker.
X-ELFCLOUD-HASH | MD5 hash of the fetch/store message payload. In fetch, the header is present in the server's HTTP response. In Store, the header must be present in client's HTTP request. Representation is 32 characters long hexadecimal string with lowercase alphabets. The hash should be used on both sides to verify integrity of the data transferred. For store requests, server will return an error code if the provided hash does not match the calculated value.
Content-Length | Length of the data being stored as a byte/octet count.
Content-Type | Always "application/octet-stream" to indicate unprocessed 8-bit binary content.

Store mode | Description
---------- | -----------
NEW | Store new data item with a new unique X-ELFCLOUD-KEY value to the pointed X-ELFCLOUD-PARENT container. If X-ELFCLOUD-KEY already exists in the container, the request will yield an exception.
REPLACE | Identical to new, but always replaces possibly existing data item.
APPEND | Append request contained data at the end of the currently existing data item. Creates a new data item if the X-ELFCLOUD-KEY specifies a non-existent key.
PATCH | Store request contained data to existing data item by overwriting an existing byte range of the same size as the request contained data. Writing is started at the offset defined by the X-ELFCLOUD-OFFSET header. If (X-ELFCLOUD-OFFSET + Content-Length) is greater than current length of the data item, the resulting data item will grow accordingly. Non-existent X-ELFCLOUD-KEY and X-ELFCLOUD-OFFSET value pointing beyond the current length of the existing data item will result in an error condition.

Use of data item description and tags is client application dependent. Client application must skip and preserve unknown key-value pairs and value contents, i.e. when updating data item data.

META header version 1 defines the following fields shown in the example:
`v1:ENC:AES256:KHA:a0ffcd...:DSC:nnn...:TGS:aaa,bbb,...:CHA:a0ffcd...::`

The v1 META string starts with `v1:` followed by any number of `KEY:VALUE:` pairs, ends with an extra `:`. **Only version 1 is currently defined and will be deprecated in the future**. The HTTP header META will be replaced by a JSON based meta information handling scheme.

Key | Value
--- | -----
ENC | ENCRYPTION METHOD USED. "NONE" / "AES256" / "AES192" / "AES256"
KHA | Key HAsh, ((10000)*MD5 hash) of the cipher key used. Required when ENC!=NONE.
DSC | User specified description of the data item
TGS | User specified content tags (comma delimited). Only a-z A-Z 0-9 _ - [space] and åäöÅÄÖ are allowed.
CHA | Content HAsh, MD5 hash of the original plaintext content, lowercase hexadecimal. Content hash is calculated by the encrypting client before encryption. Servers obviously cannot calculate this, but the value can be used by (other) clients to compare contents against a local copy without downloading.

Specific header extensions that are used in HTTP Responses:

Header | Description
------ | -----------
X-ELFCLOUD-RESULT | The result header will contain elfCLOUD server's response code. OK - Successful completion of the request. 'ERROR: Error message' - An error occurred and the details are provided after the colon.
X-ELFCLOUD-ITEM-LENGTH | Byte count of the associated data item after processing this request. The X-ELFCLOUD-ITEM-LENGTH header is only present when X-ELFCLOUD-RESULT value is OK. Data item contents (and length thereof) is guaranteed to be unmodified when the operation results in an error condition.

## Storing to cloud

> Example store request:

```http
POST /1.2/store HTTP/1.1
Host: api.elfcloud.fi
Content-Length: 94
Content-Type: application/octet-stream
X-ELFCLOUD-STORE-MODE: NEW
X-ELFCLOUD-PARENT: 32
X-ELFCLOUD-KEY: dXNlci5wcm9maWxlLmltYWdl
X-ELFCLOUD-HASH: 00bad54411073aaeacd20abc19c97950
X-ELFCLOUD-META: v1:ENC:AES256::

< 94 bytes of binary data >
```

>Example response:

```http
HTTP/1.1 200 OK
Content-Length: 0
Date: Wed, 21 Mar 2012 13:22:37 GMT
Server: api.elfcloud.fi
X-ELFCLOUD-RESULT: OK
X-ELFCLOUD-ITEM-LENGTH: 94
Content-Type: application/octet-stream
```

This is a basic example of storing a new file to the cloud.

Please refer to Data Item API description for more details.

In the example, file is saved to folder with id 32. The name of the file is user.profile.image and it has been Base64 encoded. File size is 94 bytes.

Base64Encode("user.profile.image") = dXNlci5wcm9maWxlLmltYWdl

## Fetching from cloud

> Example request:

```http
GET /1.2/fetch HTTP/1.1
Host: api.elfcloud.fi
Content-Length: 0
X-ELFCLOUD-PARENT: 32
X-ELFCLOUD-KEY: dXNlci5wcm9maWxlLmltYWdl
```

> Example response:

```http
HTTP/1.1 200 OK
Content-Length: 94
Date: Wed, 21 Mar 2012 13:22:37 GMT
Server: api.elfcloud.fi
X-ELFCLOUD-RESULT: OK
Content-Type: application/octet-stream
X-ELFCLOUD-HASH: 00bad54411073aaeacd20abc19c97950
X-ELFCLOUD-META: v1:ENC:AES256::

< 94 bytes of binary data >
```

This is a basic example of fetching a file from the cloud.

Please refer to Data Item API description for more details. The fetch request does not use the HTTP body. In the HTTP response, the body is dedicated to the returned file payload.

In the example, file is read from cloud folder with id 32. The name of the file is user.profile.image and it has been Base64 encoded. The elfCLOUD servers will return the file identical to what was stored.

Base64Encode("user.profile.image") = dXNlci5wcm9maWxlLmltYWdl

# API Objects

## Account

> Account JSON object sample

```json
{
  "id": 172,
  "company_name": "demo.fi",
  "name": "demo.fi",
  "type": "corporate"
}
```

Models an elfCLOUD account. Account has 1-n [users](#user).

Account is a dictionary type with the following keys:

Variable         | Type       | Description
---------------- | ----       | -----------
id               | integer    | Unique ID of the elfCLOUD account
company_name     | string     | Real name of the company, corporate accounts only (optional for user to input). Value is null for other account types.
name             | string     | Account name, corporate accounts only, used in login name after the @. Value is null for other account types.
type             | string     | Possible values: "corporate", "home" or "developer"

## Client

> Client JSON object sample

```json
{
  "description": "Standard elfcloud.fi client",
  "name": "elfcloud.fi",
  "types": [
    "fi.elfcloud.backup",
    "fi.elfcloud.datastore"
  ]
}
```

Models an elfCLOUD client application. Each `auth` session is bound to a single client type.

Client is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
name             | string     | Short name of the client application (used in audit log)
description      | string     | Long name or description of the client application
types            | array      | String array of [vault types](#clients,-api-keys,-vault-types) allowed for the client

To register new client types, see [developer info](#developer-info).

## Pending

Documentation migration in progress

## DataItem

> Example DataItem with no content encryption applied

```json
{
  "md5sum": "e3c3aba53d68a14f98e0ce1c278f661d",
  "dataitem_id": 967309,
  "last_accessed_date": "2015-01-26T19:38:55.500924+00:00",
  "modified_date": "2015-01-26T19:38:49.766284+00:00",
  "name": "elfcloud.fi-Beaver-S14-r16-20140814.jar",
  "locks": [
  ],
  "meta": "v1:CHA:e3c3aba53d68a14f98e0ce1c278f661d:ENC:NONE::",
  "embedded_data": null,
  "size": 9643547,
  "parent_id": 40058
}
```

The DataItem object models a single stored data item, usually a file.

The `md5sum` element contains an MD5 hash of the byte stream stored at the back-end. If the file was encrypted before storing, this will be a hash of the encrypted data. As explained in the meta header section, the CHA-element (Content HAsh) of the meta header contains a client calculated MD5 of the file data BEFORE encryption for fast comparison of whether an update is needed. In this case, as the example file was not encrypted (ENC:NONE), the CHA value matches the `md5sum` value calculated on the server side.

Documentation migration in progress

## Datetime string

Documentation migration in progress

## Object Share

Documentation migration in progress

## Public Link

Documentation migration in progress

## Permissions list

Documentation migration in progress

## User

> User JSON object sample

```json
{
  "id": 269,
  "email": "support@elfcloud.fi",
  "name": "admin",
  "lastname": "",
  "account": {
    "id": 172,
    "company_name": "demo.fi",
    "name": "demo.fi",
    "type": "corporate"
  },
  "organization_unit": null,
  "eula_accepted": true,
  "firstname": "",
  "telephone": null,
  "lang": "fi"
}
```

Models an elfCLOUD user. User has 1-1 relationship to an [account](#account).

User is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
id               | integer    | Unique ID of the elfCLOUD user
email            | string     | E-mail address of the user
name             | string     | Username, used in login. For corporate accounts this is unique within the account. For home and developer accounts the name is globally unique.
firstname        | string     | First name of the user (nullable)
lastname         | string     | Last name of the user (nullable)
account          | [account](#account) | Account of the user
organization_unit | integer | Unique ID of the [organization_unit](#organizationunit) the user is mapped to (nullable) (corporate only)
eula_accepted    | boolean    | Whether the user has accepted required EULA level
lang             | string     | User language preference, currently "fi" or "en"

## UserGroup

Documentation migration in progress

## Datetime

Datetime is a timestamp string such as: `2015-01-31T20:40:32.871917+00:00`.

The back-end server always expects and returns UTC time (+00:00) when datetime field is used in the JSON API.

## Container Vault 

> Vault JSON object sample

```json
{
  "id": 40066,
  "last_accessed_date": "2015-01-31T20:40:32.871917+00:00",
  "dataitems": 13,
  "as_web_share_root": true,
  "modified_date": "2015-02-06T22:54:51.035197+00:00",
  "name": "Some demo files",
  "permissions": [
    "rename",
    "read",
    "aclwrite",
    "remove",
    "write",
    "aclread"
  ],
  "owner": {
    "id": 269,
    "email": "support@elfcloud.fi",
    "name": "admin",
    "lastname": "",
    "account": {
      "id": 172,
      "company_name": "demo.fi",
      "name": "demo.fi",
      "type": "corporate"
    },
    "organization_unit": null,
    "eula_accepted": true,
    "firstname": "",
    "telephone": null,
    "lang": "fi"
  },
  "descendants": 1,
  "vault_type": "fi.elfcloud.backup",
  "type": "vault",
  "size": 486276398
}
```

Models an elfCLOUD vault (a top level folder). Each [user](#user) can own and have access to 0-n vaults.

Vault is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
id               | integer    | Unique ID of the elfCLOUD vault
last_accessed_date | [datetime](#datetime) | Timestamp of vault last access (UTC)
dataitems | integer | Number of direct dataitems in the folder
as_web_share_root | boolean | Whether a web share link is valid with this container as root
modified_date | [datetime](#datetime) | Timestamp of vault last modification (UTC)
name | string | Name of the vault (unique within account of the vault owner)
permissions | array | String array of authentication user's permissions to this container, see [permission list](#permissions-list) for more
owner | user | Owner [user](#user) of the vault
descendants | integer | Number of direct sub-folders (clusters) in the folder
vault_type | string | Vault type string, must be allowed by authenticated client (API key) to view
type | string | Always "vault"
size | integer | Total storage consumed by this vault instance in bytes

Documentation migration in progress

## Container Cluster

> Example Cluster JSON object

```json
{
  "id": 40062,
  "last_accessed_date": "2015-01-29T19:58:34.010517+00:00",
  "dataitems": 1,
  "as_web_share_root": false,
  "modified_date": "2015-01-28T18:41:12.161528+00:00",
  "name": "Another subfolder",
  "permissions": [
    "rename",
    "read",
    "aclwrite",
    "remove",
    "write",
    "aclread"
  ],
  "descendants": 0,
  "type": "cluster",
  "size": 24663,
  "parent_id": 40058
}
```

Clusters are non-top-level folders. They are very similar to [vaults](#container-vault) but lack some attributes such as the owner. Clusters always have a parent node in the folder tree.

Documentation migration in progress

## Message

Documentation migration in progress

## PublicLink

Documentation migration in progress

## ObjectShare

Documentation migration in progress

## OrganizationUnit

> Organization Unit JSON object sample

```json
{
  "organization_units": [],
  "unit_info": {
    "manager_id": 271,
    "name": "Development",
    "unit_id": 37,
    "members": []
  }
}
```

Models an organization unit. An [account](#account) can have 0-n top level organization units, intended to model a corporation's organization structure. Each unit can have 0-n immediate child unit. A [user](#user) can be mapped to 0-1 organization units of her own account.

Please see [get_address_book](#get_address_book) function description for more details.

OrganizationUnit is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
unit_info.name   | string     | Name of the organization unit
unit_info.manager_id | integer | User ID of the manager user of this organization unit
user_info.unit_id | integer | Unique ID of the organization unit
user_info.members | array | Integer array of User IDs of the users mapped to this organization unit
organization_units | array | Array of OrganizationUnit objects of all immediate child OUs

Documentation migration in progress

## Trust

Documentation migration in progress

## ObjectLock

Documentation migration in progress

## Subscription

Documentation migration in progress

## ACLRule

Documentation migration in progress

## ACLRuleSet

Documentation migration in progress

## Role

> Role JSON object sample

```json
{
  "user-role": "ectm.mycontact.accept"
}
```

Models an elfCLOUD security role. Roles are managed by the back-end and can be mapped to accounts, users, user groups, organization units and storage containers (vaults and clusters).

Role is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
user-role        | string     | Role identifier (optional)
account-role     | string     | Role identifier (optional)
container-role   | string     | Role identifier (optional)

User roles are security privileges that the user is allowed to perform, such as `ectm.acctrust.accept` giving the user permission to accept a received account level trust relationship invitation and bind the elfCLOUD account to the trust.

Roles assigned to user groups and organization units are propagated to the user level.

More information about existing and planned roles will be added here.

## License

> License JSON object sample

```json
{
  "count": 20,
  "license": "ECTM",
  "valid_till": "2014-11-04T07:21:00.576867+00:00"
}
```

Models an elfCLOUD subscription license. Valid licenses are returned by the elfCLOUD back-end upon authenticating. The purpose of the license information is primarily to allow the client applications to disable or hide user interface elements that the user is not allowed/licensed to use. The back-end servers will  validate all received requests against the currently valid set of licenses.

License is a dictionary type with the following keys:

User             | Type       | Description
---------------- | ----       | -----------
count            | intege     | Number of licenses issued, 1 if not relevant
license          | string     | License code (`ECTM` = elfCLOUD Trust Management, `ECMA` = elfCLOUD Mobile Access, `ECFS` = elfCLOUD Folder Synchronization, `ECSB` = elfCLOUD Secure Backup)
valid_till       | string     | Current validity period expiration time

Licenses are bundled sets of functionality available to the users of the account. Licenses are always considered on account level.

More information about existing license types will be added here.

# API Functions

Supported API functions are defined here. These are quite many and will be later on splitted under separate main level heading for easier navigation.

## auth

> To authenticate

```json
{
  "method": "auth",
  "params": {
    "username": "admin@demo.fi",
    "auth_method": "password",
    "auth_data": "CorrectPassword",
    "apikey": "atk8vzrhnc2by4f"
  }
}
```

> Example response for OK auth


```json
{
  "id": null,
  "result": {
    "client": {
      "description": "Standard elfcloud.fi client",
      "name": "elfcloud.fi",
      "types": [
        "fi.elfcloud.backup",
        "fi.elfcloud.datastore"
      ]
    },
    "role": [
      {"user-role": "ectm.mycontact.accept"},
      {"user-role": "ectm.mycontact.revoke"},
      {"user-role": "account.administrator"},
      {"user-role": "ectm.acctrust.accept"},
      {"user-role": "ectm.acctrust.revoke"},
      {"user-role": "ectm.eimgmt.admin"},
      {"user-role": "ectm.eimgmt.signer"},
      {"user-role": "account.users.manage"},
      {"user-role": "account.groups.manage"},
      {"user-role": "account.organizationtree.manage"},
      {"user-role": "ectm.acctrust.invite"},
      {"user-role": "ectm.mycontact.invite"},
      {"user-role": "ectm.keyshare.internal.all"},
      {"user-role": "ectm.keyshare.external.all"}
    ],
    "license": [
      {
        "count": 20,
        "license": "ECTM",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECMA",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECFS",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      },
      {
        "count": 20,
        "license": "ECSB",
        "valid_till": "2014-11-04T07:21:00.576867+00:00"
      }
    ],
    "user": {
      "id": 269,
      "email": "support@elfcloud.fi",
      "name": "admin",
      "lastname": "",
      "account": {
        "id": 172,
        "company_name": "demo.fi",
        "name": "demo.fi",
        "type": "corporate"
      },
      "organization_unit": null,
      "eula_accepted": true,
      "firstname": "",
      "telephone": null,
      "lang": "fi"
    },
    "account_admin": {
      "id": 269,
      "email": "support@elfcloud.fi",
      "name": "admin",
      "lastname": "",
      "account": {
        "id": 172,
        "company_name": "demo.fi",
        "name": "demo.fi",
        "type": "corporate"
      },
      "organization_unit": null,
      "eula_accepted": true,
      "firstname": "",
      "telephone": null,
      "lang": "fi"
    }
  }
}
```

All JSON sessions must start with the client sending an authentication request. For any other JSON request method, there must be a valid session token with a successful authentication performed.

An attempt to execute authentication requiring methods without a valid authenticated user’s session context will yield an authorization exception.

When an API authentication is attempted with credentials of an user who has not accepted the service terms, then server will also yield an authorization exception.

### Request

Sending a `auth` request is an attempt to authenticate the user and bring the session to an authenticated state. New session cookie will be set upon successful authentication.

Parameter | Type | Description
--------- | ---- | -----------
auth_method | string | Method of authentication, currently "password"
username | string | Full username for login (home users: username, corporate users: username@account)
auth_data | string | For `auth_method` "password", this is the user's actual login password
apikey | string | API key registered for the client application in use

### Response and action

Session is brought to authenticated state and further actions can be performed.

Successful: Element `result` is a dictionary with following top level elements:

Error: Exception response is returned

Return Variable  | Type       | Description
---------------- | ----       | -----------
client           | [client](#client) | Client application bound to the session
role             | array      | Array of [role](#role) objects
license          | array      | Array of [license](#license) objects
user             | [user](#user) | Authenticated user
account_admin    | [user](#user) | Administrator user of the own account (any one of them)

The `account_admin` variable was initially added so that the client can compare if the authenticated user is the admin user of their account. However, later on it was made possible to have multiple admin level users so comparison with these values is not so straightforward anymore if there are multiple admins in use. Hence the admin information is now mostly useful for showing the user who to contact in case of problems or lack of access.

### Permissions and session state

Everyone is allowed to call this function. Session must be in non-authenticated state.

## term

> To invalidate your session

```json
{
  "method": "term",
  "params": {
  }
}
```

```shell
curl -v -X POST --cookie-jar cookies.tmp --cookie cookies.tmp --data '{"method": "term", "params": {}}' --header "Content-type: application/json; charset: utf-8" https://api.elfcloud.fi/1.2/json
```

> Server response:

```json
{"result": {}, "id": null}
```

```shell
{"result": {}, "id": null}
```

When calling this method with authenticated client session, it expires the current session. After that, the client will need to re-authenticate or exception will be raised.

In a case where the client wishes to change the user identity it is logged in as, the client will first need to issue the `term` call and then `auth` with the new credentials.

### Permissions and session state

Everyone is allowed to call this function. Session must be in authenticated state.

### Request

Sending a `term` request indicates that the client wishes to immediately invalidate the session and logout.

Parameter | Type | Description
--------- | ---- | -----------
This function does not take any parameters.

### Response and action

Session is terminated.

Successful: Element `result` is an empty dictionary `{ }`.

Error: Exception response is returned

Return Variable  | Type       | Description
---------------- | ----       | -----------
This function does not have any return variables.

## list_vaults

> Basic list of all vaults user has visibility to

```json
{
  "method": "list_vaults",
  "params": {
  }
}
```

> Response with couple of own account's vaults

```json
{
  "id": null,
  "result": [
    {
      "id": 40066,
      "last_accessed_date": "2015-01-31T20:40:32.871917+00:00",
      "dataitems": 13,
      "as_web_share_root": true,
      "modified_date": "2015-02-06T22:54:51.035197+00:00",
      "name": "Some demo files",
      "permissions": [
        "rename",
        "read",
        "aclwrite",
        "remove",
        "write",
        "aclread"
      ],
      "owner": {
        "id": 269,
        "email": "support@elfcloud.fi",
        "name": "admin",
        "lastname": "",
        "account": {
          "id": 172,
          "company_name": "demo.fi",
          "name": "demo.fi",
          "type": "corporate"
        },
        "organization_unit": null,
        "eula_accepted": true,
        "firstname": "",
        "telephone": null,
        "lang": "fi"
      },
      "descendants": 1,
      "vault_type": "fi.elfcloud.backup",
      "type": "vault",
      "size": 486276398
    },
    {
      "id": 40058,
      "last_accessed_date": "2015-01-29T19:58:34.010517+00:00",
      "dataitems": 4,
      "as_web_share_root": true,
      "modified_date": "2015-01-28T18:41:12.161528+00:00",
      "name": "Web Share Vault",
      "permissions": [
        "rename",
        "read",
        "aclwrite",
        "remove",
        "write",
        "aclread"
      ],
      "owner": {
        "id": 269,
        "email": "support@elfcloud.fi",
        "name": "admin",
        "lastname": "",
        "account": {
          "id": 172,
          "company_name": "demo.fi",
          "name": "demo.fi",
          "type": "corporate"
        },
        "organization_unit": null,
        "eula_accepted": true,
        "firstname": "",
        "telephone": null,
        "lang": "fi"
      },
      "descendants": 3,
      "vault_type": "fi.elfcloud.backup",
      "type": "vault",
      "size": 170509011
    }
  ]
}
```

The `list_vaults` function is essential for the storage navigation. It identifies and lists all the vaults (top level folders) that the authenticated user has access to. The vaults can belong to any account.

Specific variations of the `list_vaults` are supported, i.e. to filter by vault type by specifying the `vault_type` parameter. Vaults can also be requested grouped by scope (account level vaults, user's own vaults and other vaults) by setting `grouped` parameter to true.

### Response and action

List of accessible vaults matching the given search criteria is returned.

Successful: When no vault grouping was requested, the element `result` is a flat array of [Vault](#container-vault) elements.

Error: Exception response is returned

### Permissions and session state

Everyone is allowed to call this function. Session must be in authenticated state.

Documentation migration in progress

## add_vault

> Example request

```json
{
  "params": {
    "vault_type": "fi.elfcloud.datastore", 
    "name": "NewVault"
  },
  "method": "add_vault"
}
```

> Example response

```json
{
    "id": null, 
    "result": {
        "size": 0, 
        "modified_date": null, 
        "last_accessed_date": null, 
        "name": "NewVault", 
        "descendants": 0, 
        "vault_type": "fi.elfcloud.datastore", 
        "permissions": [
            "read", 
            "write", 
            "aclread", 
            "aclwrite", 
            "rename", 
            "remove"
        ], 
        "type": "vault", 
        "dataitems": 0, 
        "id": 39
    }
}
```

Creates a new vault (top level folder) and assigns the vault ownership to the authenticated user. The vault is visible only to its owner and administrator users of the account. For any other users needing acces to the vault, you need to set explicit permissions with the [modify_permissions](#modify_permissions) call.


### Request

Parameter|Required|Type|Description
---|---|---|---
vault_type|yes|string|Defines the type of the vault to be created. Possible vault   types are determined by the API key used while authenticating the session. For further   explanation on the vault types and API keys please refer to [Clients, API Keys, Vault    Types](#clients,-api-keys,-vault-types) section.
name|yes|string|Specifies what the new vault will be titled. The vault name must be unique in the account context.

### Response and action

Returns created [vault](#container-vault) object.

### Permissions and session state

Authenticated session state is required to call this function. All users are currently allowed to create new vaults.

## rename_vault

> Example request

```json
{
  "params": {
    "vault_name": "New Vault Name",
    "vault_id": 39
  },
  "method": "rename_vault"
}
```

> Example response

```json
{
    "id": null, 
    "result": {
        "size": 0, 
        "modified_date": null, 
        "last_accessed_date": null, 
        "name": "New Vault Name", 
        "descendants": 0, 
        "vault_type": "fi.elfcloud.democorp.sample", 
        "permissions": [
            "read", 
            "write", 
            "aclread", 
            "aclwrite", 
            "rename", 
            "remove"
        ], 
        "type": "vault", 
        "dataitems": 0, 
        "id": 39
    }
}
```

Vault renaming through the `rename_vault` method. Only allowed for the vault owner and the account administrator.

### Request

Parameter|Required|Type|Description
---|---|---|---
vault_id|yes|int|Vault identifier
vault_name|yes|string|A new name for the vault

### Response and action

Returns renamed [vault](#container-vault) object.

### Permissions and session state

Authenticated session state and 'rename' permission for the vault are required to call this function.

## remove_vault

> Example request

```json
{
  "params": {
    "vault_id": 39
  },
  "method": "remove_vault"
}
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Vault owner and account administrator can permanently remove a vault with the `remove_vault` method. This means permanently deleting all data items, the complete cluster tree and all data items contained within these clusters. There is no way to restore the data deleted (except through elfcloud.fi maintainers backup restore, for a short time after the delete). Being a programmatic interface, there are no confirm dialogs before the deletion actually happens.

<aside class="warning">
In practice, all the data will be IRREVERSIBLY LOST from the elfcloud.fi servers. It is a elfcloud.fi policy not to retain any customer data after the data has been removed by the customer, or after subscription has ended. After the backup cycle rotates there will be no more copies remaining. Apart from insufficient privileges, there are no reasons for declining vault deletion.
</aside>

### Request

Parameters|Type|Description
----|----|---
vault_id|int|Identifier of the vault called for removal

### Response and action

Returns null values for `id` and `result`.

### Permissions and session state

Authenticated session state and 'remove' permission for the vault are required to call this function.

## list_clusters

> Example request

```json
{
  "params": {
    "parent_id": 40
  },
  "method": "list_clusters"
}
```

> Example response

```json
{
    "id": null, 
    "result": [
        {
            "size": 0, 
            "modified_date": null, 
            "last_accessed_date": null, 
            "name": "New Cluster", 
            "parent_id": 40, 
            "descendants": 0, 
            "permissions": [
                "read", 
                "write", 
                "aclread", 
                "aclwrite", 
                "rename", 
                "remove"
            ], 
            "type": "cluster", 
            "dataitems": 0, 
            "id": 41
        }
    ]
}
```

A vault can contain an unlimited depth cluster tree. The tree can be navigated by listing clusters with the `list_clusters` API call. You define the parent element, which can be a vault or another cluster, and the server returns a list of all clusters contained by the parent element.

Note that the list is not recursive, and you will need to make another request to query further clusters inside the listed clusters.

### Request

Parameter|Type|Description
---|---|---
parent_id|int|Identifier for the parent cluster or vault which cluster list is queried

### Response and action

Returns a list of clusters contained in the parent cluster or vault.

### Permissions and session state

Authenticated session state and 'read' permission for vault or cluster is required to call this function.

## add_cluster

> Example request

```json
{
  "params": {
    "parent_id": 40,
    "name": "New Cluster"
  },
  "method": "add_cluster"
}
```

> Example response

```json
{
    "id": null, 
    "result": {
        "size": 0, 
        "modified_date": null, 
        "last_accessed_date": null, 
        "name": "New Cluster", 
        "parent_id": 40, 
        "descendants": 0, 
        "permissions": [
            "read", 
            "write", 
            "aclread", 
            "aclwrite", 
            "rename", 
            "remove"
        ], 
        "type": "cluster", 
        "dataitems": 0, 
        "id": 41
    }
}
```

Creates a new cluster (folder) inside a vault or another cluster.

### Request

Parameter|Type|Description
---|---|---
parent_id|int|Identifier of the parent vault or cluster
name|string|Name for the newly created cluster

### Response and action

Returns the new cluster.

### Permissions and session state

Authenticated session state and 'write' permission for vault or cluster is required to call this function.

## rename_cluster

> Example request

```json
{
  "params": {
    "cluster_id": 35,
    "name": "NewNameCluster"
  },
  "method": "rename_cluster"
}
```

> Example response

```json
{
    "id": null, 
    "result": {
        "size": 0, 
        "modified_date": null, 
        "last_accessed_date": null, 
        "name": "NewNameCluster", 
        "parent_id": 30, 
        "descendants": 1, 
        "permissions": [
            "read", 
            "write", 
            "aclread", 
            "aclwrite", 
            "rename", 
            "remove"
        ], 
        "type": "cluster", 
        "dataitems": 0, 
        "id": 35
    }
}
```

Cluster renaming through the `rename_cluster` method.

### Request

Parameter|Required|Type|Description
---|---|---|---
cluster_id|yes|int|Identifier for the cluster to be renamed
name|yes|string|New name for the specified cluster

### Response and action

Returns the renamed cluster.

### Permissions and session state

Authenticated session state as a parent vault owner or an account administrator with 'rename' permissions is required to call this function.

## remove_cluster

> Example request

```json
{
  "params": {
    "cluster_id": 35
  },
  "method": "remove_cluster"
}
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Clusters can be removed with the `remove_cluster` API method. All child clusters and data items stored in the cluster will be removed as well.

### Request

Parameter|Required|Type|Description
---|---|---|---
cluster_id|yes|int|Identifier for the cluster to be removed

### Response and action

Returns null values for `id` and `result` after a successful cluster removal.

### Permissions and session state

Authenticated session state and 'remove' permissions are required to call this function.

## list_dataitems

> Example request

```json
{
  "params": {
    "parent_id": 30
  },
  "method": "list_dataitems"
}
```

> Example response

```json
{
    "id": null, 
    "result": [
        {
            "embedded_data": null,
            "locks": [],
            "dataitem_id": 197,
            "modified_date": "2012-09-11T10:57:55.877266+00:00", 
            "name": "DataItem 2", 
            "md5sum": "5e18dc2e8a01bbde8dfc1e76aa62698d", 
            "parent_id": 30, 
            "last_accessed_date": "2012-09-12T07:30:05.132943+00:00", 
            "meta": "v1:CHA:76c6fbb298c72a80e92f661f59bb6d16....", 
            "size": 10240
        }, 
        {
            "embedded_data": null,
            "locks": [],
            "dataitem_id": 194,
            "modified_date": "2012-09-11T11:11:44.180638+00:00", 
            "name": "DataItem 1", 
            "md5sum": "53f11718d9bf02f256ba6037d7bbb73f", 
            "parent_id": 30, 
            "last_accessed_date": "2012-09-12T07:30:05.032721+00:00", 
            "meta": "v1:ENC:AES256:TGS:....", 
            "size": 9
        }
    ]
}
```

Data items are primarily created and modified through the Data Item API which is not JSON based. However, API calls for listing data items and querying their attributes are available in the JSON API.

API function `list_dataitems` provides a listing of data items in the named parent container. The response contains all data items that the authenticated user has at least read access to. The data item dictionary contains fields regarding last access and modification of the data item, locks currently placed on the data item, data item tags, description and information on the used encryption mechanism. The embedded_data element of the data item dictionary is used only with the `list_contents` API call to speed up fetching very small data items as part of the data item listing, this helps in avoiding separate Data Item API fetch call to download the item contents.

### Request

Parameter|Required|Type|Description
---|---|---|---
parent_id|yes|int|Identifier of the vault or cluster, whose contained data items are to be listed
names|no|array|Method to return only data items that match with the given names array, empty array returns all data items

### Response and action

Returns a list of queried data items and their values.

### Permissions and session state

Authenticated session state and 'read' permission for the named vault or cluster are required to call this function.

## update_dataitem

> Example request

```json
{
  "params": {
    "parent_id": 30,
    "meta": "v1:ENC:NONE::",
    "name": "Backups-20110812.tar"
  },
  "method": "update_dataitem"
}
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Data item's meta information can be updated with `update_dataitem` method.

<aside class="notice">
The new meta must be formatted and escaped by the X-ELFCLOUD-META rules as seen above at the <a href="#http-headers">HTTP Headers section</a>.
</aside>

### Request

Parameter|Required|Type|Description
---|---|---|---
parent_id|yes|int|Identifier of the data item's parent vault or cluster
name|yes|string|Name of the data item

### Response and action

Returns null values for `id` and `result`.

### Permissions and session state

Authenticated session state and 'write' permission for the parent vault or cluster are required to call this function.

## relocate_dataitem

> Example request

```json
{
    "params": {
        "new_name": "DataItemName2",
        "parent_id": 15,
        "name": "OldName",
        "new_parent_id": 16
    },
    "method": "relocate_dataitem"
}
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Data item relocating with an optional renaming.

### Request

Parameter|Required|Type|Description
---|---|---|---
parent_id|yes|int|Identifier of the data item's parent vault or cluster
name|yes|string|Name of the data item for relocating
new_parent_id|yes|int|Identifier of the data item's relocation target vault or cluster
new_name|?|string|A new name for the relocated data item

### Response and action

Returns null values for `id` and `result` after successful relocating.

### Permissions and session state

Authenticated session state and 'write' permission for the new vault or cluster are required to call this function.

## rename_dataitem

> Example request

```json
{
  "params": {
    "parent_id": 35,
    "name": "DataItem1",
    "new_name": "New name for dataitem"
  },
  "method": "rename_dataitem"
} 
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Data item renaming.

### Request

Parameter|Required|Type|Description
---|---|---|---
parent_id|yes|int|Identifier of the data item's parent vault or cluster
name|yes|string|Name of the data item to be renamed
new_name|yes|string|New name for the specified data item

### Response and action

Returns null values for `id` and `result` after successful renaming.

### Permissions and session state

Authenticated session state and 'write' permission for the parent vault or cluster are required to call this function.

## remove_dataitem

> Example request

```json
{
  "params": {
    "parent_id": 35,
    "name": "DataItem1",
  },
  "method": "remove_dataitem"
}
```

> Example response

```json
{
    "id": null, 
    "result": null
}
```

Data items can be deleted with the `remove_dataitem` JSON API method call.

### Request

Parameter|Required|Type|Description
---|---|---|---
parent_id|yes|int|Identifier of the data item's parent vault or cluster
name|yes|string|Name of the data item to be removed

### Response and action

Returns null values for `id` and `result` after a successful removal.

### Permissions and session state

Authenticated session state and 'write' permission for the parent vault or cluster are required to call this function.

## list_contents

> List sub-folders and dataitems for a container

```json
{
  "method": "list_contents",
  "params": {
    "parent_id": 40058
  }
}
```

> Example response

```json
{
  "id": null,
  "result": {
    "clusters": [
      {
        "id": 40059,
        "last_accessed_date": null,
        "dataitems": 0,
        "as_web_share_root": false,
        "modified_date": null,
        "name": "Sub-folder 1",
        "permissions": [
          "rename",
          "read",
          "aclwrite",
          "remove",
          "write",
          "aclread"
        ],
        "descendants": 1,
        "type": "cluster",
        "size": 0,
        "parent_id": 40058
      },
      {
        "id": 40060,
        "last_accessed_date": null,
        "dataitems": 0,
        "as_web_share_root": false,
        "modified_date": null,
        "name": "Clusters are non-top-level folders!",
        "permissions": [
          "rename",
          "read",
          "aclwrite",
          "remove",
          "write",
          "aclread"
        ],
        "descendants": 0,
        "type": "cluster",
        "size": 0,
        "parent_id": 40058
      },
      {
        "id": 40062,
        "last_accessed_date": "2015-01-29T19:58:34.010517+00:00",
        "dataitems": 1,
        "as_web_share_root": false,
        "modified_date": "2015-01-28T18:41:12.161528+00:00",
        "name": "Another subfolder",
        "permissions": [
          "rename",
          "read",
          "aclwrite",
          "remove",
          "write",
          "aclread"
        ],
        "descendants": 0,
        "type": "cluster",
        "size": 24663,
        "parent_id": 40058
      }
    ],
    "dataitems": [
      {
        "md5sum": "743745fb4d415d14901a8c46af8c0bbf",
        "dataitem_id": 967308,
        "last_accessed_date": "2015-01-28T20:10:45.395025+00:00",
        "modified_date": "2015-01-26T19:12:37.759438+00:00",
        "name": "2008_BMW_K_1200S._V139114037_.jpg",
        "locks": [],
        "meta": "v1:CHA:743745fb4d415d14901a8c46af8c0bbf:ENC:NONE::",
        "embedded_data": null,
        "size": 24663,
        "parent_id": 40058
      },
      {
        "md5sum": "e3c3aba53d68a14f98e0ce1c278f661d",
        "dataitem_id": 967309,
        "last_accessed_date": "2015-01-26T19:38:55.500924+00:00",
        "modified_date": "2015-01-26T19:38:49.766284+00:00",
        "name": "elfcloud.fi-Beaver-S14-r16-20140814.jar",
        "locks": [],
        "meta": "v1:CHA:e3c3aba53d68a14f98e0ce1c278f661d:ENC:NONE::",
        "embedded_data": null,
        "size": 9643547,
        "parent_id": 40058
      },
      {
        "md5sum": "d3a270c17a3142b9f906851647d0fe8b",
        "dataitem_id": 9222310,
        "last_accessed_date": "2015-01-26T20:19:16.558364+00:00",
        "modified_date": "2015-01-26T19:39:29.070509+00:00",
        "name": "se_19000014_linux64.tar.gz",
        "locks": [],
        "meta": "v1:CHA:d3a270c17a3142b9f906851647d0fe8b:ENC:NONE::",
        "embedded_data": null,
        "size": 160501524,
        "parent_id": 40058
      }
    ]
  }
}
```

The `list_contents` is a convenient function that returns all sub-folders and dataitems for the given folder (vault or cluster).



Documentation migration in progress

## subscription_info

> Querying current subscription information

```json
{
  "method": "subscription_info",
  "params": {}
}
```

> Example response

```json
{
  "id": null,
  "result": {
    "current_subscription": {
      "id": 1323,
      "end_date": "2014-11-04T07:21:00.576867+00:00",
      "status": "active",
      "storage_quota": 536870912000,
      "renewal_type": "auto",
      "subscription_type": {"name": "paid"},
      "length": {
        "id": 0,
        "name": "n/a"
      },
      "start_date": "2013-11-04T07:21:00.576867+00:00"
    }
  }
}
```

Documentation migration in progress

Note: A request has been made to add current storage quota usage to the `current_subscription` element.

## user_accept_eula

> Example request

```json
{
    "method": "user_accept_eula",
    "params": {
    }
}
```

> Example response

```json
{"result": null, "id": null}
```

Each end-user must accept the EULA before they can utilize the API interfaces.

## get_organization_structure

> Requesting organization structure info

```json
{
  "method": "get_organization_structure",
  "params": {
    "account_id": 172
  }
}
```

> Example response

```json
{
  "id": null,
  "result": {
    "organization_units": [
      {
        "organization_units": [],
        "unit_info": {
          "manager_id": 271,
          "name": "Sales",
          "unit_id": 7,
          "members": [
            273,
            8585
          ]
        }
      }
    ],
    "account": {
      "id": 172,
      "company_name": "demo.fi",
      "name": "demo.fi",
      "type": "corporate"
    }
  }
}

```

Organization structure can be requested from user's own account or any other account that has an established account trust relationship with the user's account. The information contains account basic information as well as organization unit structure and members.

Combined with the [address book](#get_address_book) information, phone book level visibility is achieved within the elfCLOUD account and among all trusted accounts. This essentially makes content key and folder permission management very straightforward for the end users because the recipients of content keys and permissions can be selected from an easy-to-use address book even when the other user is under a distinct account.

Documentation migration in progress

## organization_unit_add

```json
{
  "method": "organization_unit_add",
  "params": {
    "manager_id": 271,
    "name": "Development",
    "parent_id": null
  }
}
```

```json
{
  "id": null,
  "result": {
    "organization_units": [],
    "unit_info": {
      "manager_id": 271,
      "name": "Development",
      "unit_id": 37,
      "members": []
    }
  }
}
```

Documentation migration in progress

## organization_unit_update

Documentation migration in progress

## organization_unit_delete

Documentation migration in progress

## get_address_book

> Request address book, users and groups

```json
{
  "method": "get_address_book",
  "params": {
    "account_id": 172,
    "section": [
      "internal_users",
      "internal_groups"
    ]
  }
}
```

> Example response

```json
{
  "id": null,
  "result": {
    "internal_groups": [
      {
        "hidden_external": false,
        "name": "Myynti",
        "group_id": 335,
        "hidden_internal": false,
        "parent_group_id": null,
        "external_members": [],
        "internal_members": [172]
      }
    ],
    "internal_users": [
      {
        "id": 269,
        "email": "support@elfcloud.fi",
        "name": "admin",
        "lastname": "Administrator",
        "account": {
          "id": 172,
          "company_name": "demo.fi",
          "name": "demo.fi",
          "type": "corporate"
        },
        "organization_unit": null,
        "eula_accepted": true,
        "firstname": "Demo",
        "telephone": null,
        "lang": "fi"
      },
      {
        "id": 273,
        "email": "info@elfcloud.fi",
        "name": "demo.user",
        "lastname": "Demo Family",
        "account": {
          "id": 172,
          "company_name": "demo.fi",
          "name": "demo.fi",
          "type": "corporate"
        },
        "organization_unit": null,
        "eula_accepted": false,
        "firstname": "User",
        "telephone": "",
        "lang": "fi"
      }
    ]
  }
}
```

Get address book. `account_id` is optional, returns own account if omitted. Query allowed only for own and trusted accounts. For `get_address_book` requests on external accounts, only internal-users section is returned. External account's groups and external users are not shown.

Please note that a user group can also contain users from other accounts, defined in the `external_members` attribute.

Group item `external_members`: `account_id` refers to either 1) trusted account, 2) account of an external user individually bound to internal user's contacts.

Invited externals are always visible to account level administrators / management role holders.

Unless a group is hidden from general browsing of account users, any internal user can see all members of a group, including external users.

Invalid external users are not generally visible to other users of the account, unless they are members of a non-hidden group.

## get_external_relations

> Request My Contacts and Account Trusts

```json
{
  "method": "get_external_relations",
  "params": {
    "section": [
      "my_contacts",
      "account_trusts"
    ]
  }
}
```

> Example response

```json
{
  "id": null,
  "result": {
    "my_contacts": [
      {
        "id": 37,
        "email": "support@elfcloud.fi",
        "name": "demo.admin",
        "lastname": "Administrator",
        "account": {
          "id": 14,
          "company_name": "elfcloud.fi Demo Corporation",
          "name": "democorp.elfcloud.fi",
          "type": "corporate"
        },
        "organization_unit": null,
        "eula_accepted": true,
        "firstname": "Demo",
        "telephone": "+3584000",
        "lang": "fi"
      }
    ],
    "account_trusts": [
      {
        "id": 14,
        "company_name": "elfcloud.fi Demo Corporation",
        "name": "democorp.elfcloud.fi",
        "type": "corporate"
      }
    ]
  }
}
```

Documentation migration in progress

## send_external_invite

> Example request

```json
{
    "params": {
        "recipient_email": "tuomas.tonteri@iki.fi",
        "trust_type": "account" | "user",
        "pin_code": "3029102",
        "invite_message": "Hello, welcome to be my trusted partner",
        "trust_initiator_comment": "My internal/private notes for this trust
relationship",
        "send_me_copy": true
 },
    "method": "send_external_invite"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": null
}
```

Server sends an invite email to the indicated recipient. Email will contain a server generated token (128 characters long hexstring split into 4 lines), the PIN code is not in the email. PIN code is stored on the server, and the user must copy-paste the token to Beaver, enter the PIN and Beaver will call `accept_invite`.

Trust type "account" can only be sent and accepted by authorized role. Type "user" invite can be sent by anybody unless corporate policy prohibits this.

Parameter `send_me_copy` will cause same invitation email, with the authorization token, to be sent to the inviting user. Subject line will have (copy) prefix, otherwise the message is fully identical.

## accept_invite

> Example request

```json
{
    "params": {
        "token":
"bf96f2fc44a6462b69e73c6a625400b700ae8a2c23a400f4dd73849276383f22312d0b33108207d060013
5c27d74f6b072f3ab525ae93661440dd704558cb845",
        "pin_code": "3029102"
    },
    "method": "accept_invite"
}
```

> Example response

```json
{"result": null, "id": null}
```

> Example response, when token is not valid, PIN code does not match or PIN code attempts are out

```json
{"error": {"message": "Permission denied.", "code": 105, "data": "ECClientException"}}
```

Accept invite. Invitations are given three PIN code attempts. The count is not shown to the recipient. The call to `accept_invite` will verify the input and silently increase the PIN code guess attempt count. When the input is correct, the trust relationship is formed.

## revoke_trust

> Example request

```json
{
    "params": {
        "trust_id": 150,
  "revoke_comment": "User given reason for cancelling the trust relationship, for
audit log."
 },
    "method": "revoke_trust"
}
```

> Example response

```json
{
    "id": null,
    "result": null
}
```

Revoke trust. Trust can be revoked at any time by an administrative user with role XXX on either side of the trust relationship. Same applies for My Contact and Account Trust relationships. Operation is successful and trust is removed when the response is a non-exception null value.

## identity_reset

> Example request

```json
{
    "method": "identity_reset",
    "params": {
        "account_id": 43,
        "user_id": null
    }
}
```

Resetting personal or enterprise authority identity. Give only `account_id` OR `user_id`, depending on which type of keys you want to delete. This will delete public key, private key and public key waiting for signing.

### Request

Parameter|Required|Type|Description
---|---|---|---
account_id|no|int|Identifier for the account which keys are called for resetting
user_id|no|int|Identifier for the accont which keys are called for resetting

### Response and action

### Permissions and session state

Authenticated session state is required to call this function, can only be performed in context of authenticated user's account. Account admin can delete any user's keys.

## identity_update_keys

> Example request

```json
{
    "method": "identity_update_keys",
    "params": {
        "account_id": 41,
        "key_public_format": "pem",
        "key_public":
"----------------------BEGIN-PUBLIC-KEY---------------------------\ne0392cr209i3,ir90,
3e091ie0i1209,e290e,09ei01ei,0129e,00,i0e9i2,0ei,0ei,290xi\ne229ix10,e09e21ie92i9,i9ii
i9012eijd0r983kyrqurs2ur389uk392sueu239ekusu32e293eu2e\n3es2ues2389ekus2389euk298eus23
89k2es29e3u938euk29euks398k298euk398kesu3eu239se23e\n---------------------------------
--------------------------------------------\n",
        "key_private_format": "pem.aes256.b64",
        "key_private":
"229ix1dqwqwd1ie92i9,i9iii9012eijd0r983kyrqurs2ur389uk392sueu239ekusu32e293ekW="
    }
}
```

> Example response

```json
{
    "id": null,
    "result": null
}
```

Uploading user or enterprise authority keys.

Use cases:

* EA uploads new or replacement EA keys
* user uploads new or replacement keys (public key is queued for EA-signing.
* EA publishes signed user public key

All key updates replace possibly existing conflicting keys.

Parameters `user_id` and `account_id` are **mutually exclusive** and indicate the user or account level EA to update. Regular user can only update her own keys, still, `user_id` shall be given to indicate desired functionality. Regular user must update both keys at the same time.

EA user should only update other users' public key (replace with signed certificate) (this is not currently controlled).

`key_public_format` must be "pem" when user uploads her own new unsigned public key. `key_private_format` must be "pem.aes256.b64". Public keys are stored in plain PEM, private key as PEM + AES256 encryption + base 64 encoding.

When EA signer uploads user's signed public certificate, key_public_format must be "crt.pem" and certificate is stored in plain PEM format.

## identity_user_sign_requests

> Example request

```json
{
    "method": "identity_user_sign_requests",
    "params": {
    }
}
```

> Example response

```json
{
    "id": null,
    "result": [
        {
        "user": User,
        "key_public_format": "pem",
        "key_public":
"----------------------BEGIN-PUBLIC-KEY---------------------------\ne0392cr209i3,ir90,
3e091ie0i1209,e290e,09ei01ei,0129e,00,i0e9i2,0ei,0ei,290xi\ne229ix10,e09e21ie92i9,i9ii
i9012eijd0r983kyrqurs2ur389uk392sueu239ekusu32e293eu2e\n3es2ues2389ekus2389euk298eus23
89k2es29e3u938euk29euks398k298euk398kesu3eu239se23e\n---------------------------------
--------------------------------------------\n"
        }*
    ]
}
```

Get public keys waiting for signing. When EA uploads a certificate for a user, the pending public key is automatically removed by the server.

## identity_whoami

> Example request

```json
{
    "method": "identity_whoami"
}
```

> Example response

```json
{"result": {"client": {"name": "tuomas-test-app", "types": ["mx.tto.storage"],
"description": "tuomas dev test app"}, "user": {"lang": "fi", "account":
{"company_name": "Tuomas Corporation", "type": "corporate", "id": 41, "name":
"tuomas-corp"}, "telephone": "", "name": "tuomas-adm", "firstname": "Corporate",
"eula_accepted": true, "lastname": "Admin", "id": 52, "email":
"tuomas.tonteri@elfcon.net"}}, "id": null}
```

Call to get user info of the currently active user.

## identity_get_public_key

> Example request

```json
{
    "params": {
        "user_id": 100,
        "account_id": 90
    },
    "method": "identity_get_public_key"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": {
        "key_public_format": "crt.pem",
        "key_public": "keydata"
    }
}
```

Get public key of the specified user (of account) or account level EA.

## identity_get_private_key

> Example request

```json
{
    "params": {
        "key_owner": "own" | "ea"
    },
    "method": "identity_get_private_key"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": {
        "key_private_format": "pem.aes256.b64",
        "key_private": "keydata"
    }
}
```

Get own or account level EA's private key.

## submit_to_user_inbox

> Example request

```json
{
    "method": "submit_to_user_inbox",
    "params": {
        "recipient_user_id": 52,
        "recipient_account_id": 21,
        "item_type": "content_key",
        "item_data": "Base64 encoded end encrypted message data",
        "item_data_sig": "Base64 encoded item_data signature"
    }
}
```

> Example response

```json
{
    "id": null,
    "result": null
}
```

Sharing content cipher keys with other users. Submission is only allowed to My Contact or a user of same or trusted account, but not to the sender.

For key sharing the XML shall contain a well formatted key exchange package:

* name (filename)
* description
* encryption method / key length
* key data, encrypted with the recipient public key
* hash of encrypted data
* signature for s(hash) by sender private key

It is possible to attach file attachment to mails (via STORE), but this is not needed for key exchange, as the whole KEY XML can be embedded into the item_data (it can hold up to 20480 bytes).

## inbox_list

> Example request

```json
{
  "method": "inbox_list",
  "params": {
    "include_status": ["new"]
  }
}
```

> Example response

```json
{
  "id": null,
  "result": [ ]
}
```

User's own inbox is always returned. Field time_received is null if the message has not been marked read with the inbox_mark_as_read call.

## inbox_delete

> Example request

```json
{
    "method": "inbox_delete",
    "params": {
          "mail_ids": [ 1, ... ]
    }
}
```

> Example response

```json
{
    "id": null,
    "result": {
        "deleted_mails": [ 1, ... ]
    }
}
```

Delete inbox item. Input array items must contain identifiers of messages in authenticated user's mailbox to be deleted.

Response array deleted_mails lists identifiers of deleted messages. These are deleted for good.

## inbox_mark_as_read

> Example request

```json
{
    "method": "inbox_mark_as_read",
    "params": {
          "mail_ids": [ 1, ... ]
    }
}
```

> Example response

```json
{
    "id": null,
    "result": {
        "marked_as_read": [ 1, ... ]
    }
}
```

Mark inbox items as read. Input array items must contain identifiers of messages in authenticated user's mailbox to be marked as read. Response array `marked_as_read` identifiers of processed messages. The `time_received` field is set to UTC now() and is returned in subsequent `list_inbox` calls.

## group_add

> Example request

```json
{
    "params": {
        "parent_group_id": 1,
        "name": "Nerdy Hackers",
        "hidden_internal": true,
        "hidden_external": true,
        "internal_members": [ user_id* ], 
        "external_members": [ { "account_id": 7, "user_id": 8 }* ]
    },
    "method": "group_add"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": Group
}
```

Add group.

NOTE: Server must verify that external account/user pair is visible to the requesting user. Viewing group then returns User objects, which would make it possible to scan other accounts.

## group_members_add

> Example request

```json
{
    "params": {
        "group_id": 11,
        "internal_members": [ user_id* ],
        "external_members": [ { "account_id": 7, "user_id": 8 }* ]
    },
    "method": "group_members_add"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": Group
}
```

Add members into existing group. In case of external members, account_id must refer to trusted account or the specific member must be an invited external.

## group_members_delete

> Example request

```json
{
    "params": {
        "group_id": 11,
        "internal_members": [ user_id* ],
        "external_members": [ { "account_id": 7, "user_id": 8 }* ]
    },
    "method": "group_members_delete"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": Group
}
```

Delete member(s) from a group.

## group_update

> Example request

```json
{
    "method": "group_update",
    "params": {
     "group_id": 112,
     "parent_id": null,
     "name": "Vaihdettu",
     "hidden_internal": null,
     "hidden_external": null,
     "internal_members": [ 70, 0, 56 ],
     "external_members": [ { "account_id": 0, "user_id": 0 } ]
    }
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": Group
}
```

Modify group: rename, move, visibility and complete members change. All parameters except `group_id` and `parent_id` can be set to null when no change is desired.

`group-members-add` and `group-update`: `external_members` can only be added when the external is in My Contacts of the requesting user, or in the case of `group-update`, same external already exists as a group member.

When moving group under new parent, the `parent_id` must not be a child of the updated group. When move is not wanted, please provide the original `parent_id` value. Value null moves the group to top level.

## group_delete

> Example request

```json
{
    "params": {
        "group_id": 1
    },
    "method": "group_delete"
}
```

> Example response

```json
{
    "id": null, (do something with this)
    "result": null
}
```

Call to delete group and its all child groups.

## user_set_organization_unit

> Example request

```json
{
  "method": "user_set_organization_unit",
  "params": {
    "users": [
      8585,
      273
    ],
    "unit_id": 7
  }
}
```

> Example response

```json
{
  "id": null,
  "result": [
    {
      "user_id": 8585,
      "organization_unit": 7
    },
    {
      "user_id": 273,
      "organization_unit": 7
    }
  ]
}
```

Assign users to given organization unit (will be removed from existing OU, set unit to null to just remove users from current OU).

Response lists user id and new organization unit id of all modified user records. Users that were given in request but are missing from response were not found or do not require change.

## get_property

> Example request #1

```json
{
    "params": {
        "name": "userconfig.xml",
        "data": "base64 encoded public key encrypted contents"
    },
    "method": "set_property"
}
```

> Example response #1

```json
{
    "id": null,
    "result": null
}
```

> Example request #2

```json
{
    "params": {
        "name": "userconfig.xml"
    },
    "method": "get_property"
}
```

> Example response #2

```json
{
    "id" : null,
    "result" : {
        "data" : "unmodified data returned"
    }
}
```

Client applications can store variable properties on the server side. All API properties default to user's private properties and the get_property call always returns the latest property value set (also there is no history of values on the server side either).

Beaver stores user's keyring (or entire userconfig) as a single property, allowing keyring cloud backup and restore.

## set_property

Documentation migration in progress

## lock_object

Documentation migration in progress

## unlock_object

Documentation migration in progress

## modify_lock

Documentation migration in progress

## list_locks

Documentation migration in progress

## create_link

Documentation migration in progress

## modify_link

Documentation migration in progress

## list_links

Documentation migration in progress

## list_permissions

Documentation migration in progress

## modify_permissions

Documentation migration in progress

## list_roles

Documentation migration in progress

## set_owner

Documentation migration in progress

## get_dataitem_by_id

Documentation migration in progress

## get_container_by_id

Documentation migration in progress

## duplicate_dataitem

Documentation migration in progress

## get_container_by_id

> Fetch multiple container details by ID array

```json
{
  "method": "get_container_by_id",
  "params": {"container_ids": [
    33749,
    40041
  ]}
}
```

> Example response

```json
{
  "id": null,
  "result": [
    {
      "id": 33749,
      "exception": "Container not found or no permission to view"
    },
    {
      "id": 40041,
      "last_accessed_date": null,
      "dataitems": 3,
      "as_web_share_root": false,
      "modified_date": "2014-08-26T21:58:53.358206+00:00",
      "name": "Sync-admin@demo.fi",
      "permissions": [
        "rename",
        "read",
        "aclwrite",
        "remove",
        "write",
        "aclread"
      ]
    }
  ]
}        
```

This function enables fetching multiple container objects in a single call.

Documentation migration in progress


## check_name_availability

> Example request to verify if the username is valid and available in the global consumer user namespace

```json
{
    "method": "check_name_availability",
    "params": {
      "user_name": "donald470",
      "account_name": null
    }
}
```

> Example response indicating the user name is not valid or not available

```json
{
    "id": null,
    "result": {
      "name_availability": false
    }
}
```

> Example request to verify if the account name is available for a new corporate account

```json
{
    "method": "check_name_availability",
    "params": {
      "user_name": null,
      "account_name": "example.com"
    }
}
```

> Example response indicating the given account name is available for a new account creation

```json
{
    "id": null,
    "result": {
      "name_availability": true
    }
}
```

> Example request to verify if the username is available within the corporate account

```json
{
    "method": "check_name_availability",
    "params": {
      "user_name": "admin",
      "account_name": "example.com"
    }
}
```

> Example response indicating the user can be added to the given new or existing account

```json
{
    "id": null,
    "result": {
      "name_availability": true
    }
}
```

This request can be used by authorized clients to enquire whether the given user name, account name or a combination of both is valid and available to be registered. Please note that for consumer users the elfCLOUD login name consists of only the user name which has to be globally unique. For corporate accounts, login name is formed as user name @ account name where the account name must be globally unique amongst all account names but the user names are only unique within the account specific namespace.

### Request

This API function accepts the following parameters:

Parameter    | Type         | Required | Description
------------ | ------------ | -------- | -----------
user_name    | string       | no      | User name to be verified. Can be omitted when account_name is given.
account_name | string       | no      | Account name to be verified. Can be omitted when user_name is given.

When `user_name` is null, the request will verify the validity and availability of the given account name for a new corporate account to be created.

When `account_name` is null, the request is interpreted as a consumer account enquiry. The given user name is verified for global uniqueness, availability and validity.

When both `user_name` and `account_name` are provided, the request will verify the uniqueness of the given user name within the namespace of the given corporate account. If the named corporate account does not exist and the name is valid, then also the user name will be available unless it contains illegal characters.

When a field can be given a null value, the field can also be omitted.

### Response and action

*Successful*: Element `result` is a dictionary with following top level elements:

Return Variable         | Type         | Description
----------------------- | ------------ | -------------
name_availability 	    | boolean      | `true` when name is available for use, `false` when the name is invalid or already reserved

*Error*: Exception response is returned

Exception               | Code         | Reason
----------------------- | ------------ | -------------
ECClientException       | 105          | Client is not authorized to perform the request

### Permissions and session state

This request does not have session state requirements. The call is privileged and the client must be properly authorized to perform this action.



## register_account

> Example request to register a new corporate account

```json
{
    "method": "register_account",
    "params": {
      "account": {
        "account_name": "example.com",
        "company_name": "Example Corporation",
        "business_number": "FI12345678",
		"country": "FI",
		"company_identifier_non_eu": "US Business ID AZ123456789"
      },
      "administrator": {
        "user_name": "admin",
        "password": "abc!1234",
        "subscribe_newsletter": false,
		"email_address": "john.smith@example.org"
      },
      "subscription": {
        "license_count": 3,
		"annual_payment": true
      }
    }
}
```

> Example response indicating the account has been created with the given details

```json
{
    "id": null,
    "result": {
      "account_id": 1234,
      "authorization": "1bced851ef7592d68bad72524025d01f93be604b46813b4964b896938d547b1e"
    }
}
```

This request can be used by authorized clients to create (register) new elfCLOUD accounts. The account will be created with one user, the administrator. The account will have the designated number of licenses. The account will be activated and the 30 day trial period started when the payment card information has been added to it. If the payment card is not added after a while, the account will be deleted and the reserved account name is released for other use.

### Request

This API function accepts the following parameters:

Parameter                          | Type         | Required | Description
---------------------------------- | ------------ | -------- | -----------
account                            | dict         | no       | Container dictionary for corporate account details. Can be omitted for consumer account registration.
account.account_name               | string       | yes      | Name of the corporate account to be created.
account.company_name               | string       | yes      | Name of the corporation being registered. No length requirements, can be an empty string.
account.business_number            | string       | yes      | Business ID of the corporation being registered. No length requirements, can be an empty string.
account.country                    | string       | yes      | Country code where the customer is located, required for private and corporate accounts (ISO-3166-1 alpha-2)
account.company_identifier_non_eu  | string       | no       | Business ID of a non-EU corporation
administrator                      | dict         | yes      | Container dictionary for the user to be added to the account.
administrator.user_name            | string       | yes      | User name of the administrator user being created.
administrator.password             | string       | yes      | Password of the administrator user being created.
administrator.subscribe_newsletter | boolean      | yes      | Whether the user wants to subscribe to the elfCLOUD newsletter.
subscription                       | dict         | yes      | Container dictionary for subscription related attributes.
subscription.license_count         | int          | yes      | Number of licenses to start the trial period with, 1-100 range.
subscription.annual_payment        | boolean      | yes      | Whether the account trial is created with annual rather than monthly payment term.

When the `account` dictionary is omitted, the call registers a consumer account. For corporate accounts this dictionary and all it’s fields are required.

### Response and action

*Successful*: Element `result` is a dictionary with following top level elements:

Return Variable         | Type         | Description
----------------------- | ------------ | -------------
account_id              | int          | Account ID of the created account. This will be needed to supplement the account with the Stripe payment card token.

*Error*: Exception response is returned. In the error response, there is no result object but an error object instead (see the Error section for details).

Exception               | Code         | Reason
----------------------- | ------------ | -------------
ECClientException       | 105          | Client is not authorized to perform the request

### Permissions and session state

This request does not have session state requirements. The call is privileged and the client must be properly authorized to perform this action.


## set_payment_card_token

> Example request to bind a Stripe payment card token to the newly created account

```json
{
    "method": "set_payment_card_token",
    "params": {
      "account_id": 1234,
      "authorization": "1bced851ef7592d68bad72524025d01f93be604b46813b4964b896938d547b1e",
      "payment_card_token": "tok_a0b0c0d0e0f0a0b0c0d0e0f0a0b0c0d0e0f0"
    }
}
```

> Example response indicating the token was processed successfully

```json
{
    "id": null,
    "result": {
	    "status": "Account updated"
    }
}
```

This request can be used by authorized clients to attach an unused Stripe payment card token to an elfCLOUD account.

During a new account registration process, the `account_id` and `authorization` parameters are returned by the `register_account` call previously done. For existing accounts in authenticated account management state, the account_id and authorization fields may be omitted.

### Request

This API function accepts the following parameters:

Parameter          | Type         | Required         | Description
------------------ | ------------ | ---------------- | -----------
account_id         | int          | during reg.      | Account ID of an existing account. During registration this value is got from the register_account call response.
authorization      | string       | during reg.      | Authorization key to be able to configure the account. This value is got from the register_account call response.
payment_card_token | string       | yes              | Card token received from the Stripe interface after adding the card.

### Response and action

*Successful*: Element `result` is a dictionary with following top level elements:

Return Variable         | Type         | Description
----------------------- | ------------ | -------------
status                  | string       | "Account updated" indicates the card token was successfully processed.

*Error*: Exception response is returned. In the error response, there is no result object but an error object instead (see the Error section for details).

Exception               | Code         | Reason
----------------------- | ------------ | -------------
ECClientException       | 105          | Client is not authorized to perform the request

### Permissions and session state

Everyone is allowed to call this function. Session must be in authenticated state.





## validate_eu_vat

> Example request to validate corporate EU VAT Identifier

```json
{
	"method": "validate_eu_vat",
	"params": {
		"country_code": "FI",
		"vat_code": "23540122",
		"company_name": "elfGROUP Kyberturvallisuuspalvelut Oy"
	}
}
```

> Example response indicating the VAT code is valid and matches the given company name

```json
{
	"vat_code_valid": true,
	"company_name_matches": true
}
```

This request can be used by authorized clients to validate VAT codes for new EU based elfCLOUD registrations.

### Request

This API function accepts the following parameters:

Parameter                          | Type         | Required | Description
---------------------------------- | ------------ | -------- | -----------
country_code                       | string       | yes      | Country code of the corporation (ISO-3166-1 alpha-2)
vat_code                           | string       | yes      | EU format of the VAT code without the country prefix, e.g. FI23540122 becomes 23540122
company_name                       | string       | yes      | Corporation name to be validated against the given VAT ID

### Response and action

*Successful*: Element `result` is a dictionary with following top level elements:

Return Variable         | Type         | Description
----------------------- | ------------ | -------------
vat_code_valid          | boolean      | True if the given country_code + vat_code combination forms a valid EU VAT ID
company_name_matches    | boolean      | True if the given company_name matches a valid EU VAT ID. The field is omitted if vat_code_valid == False

*Error*: Exception response is returned. In the error response, there is no result object but an error object instead (see the Error section for details).

Exception               | Code         | Reason
----------------------- | ------------ | -------------
ECClientException       | 105          | Client is not authorized to perform the request (rate limit reached or special privileges required)




### Permissions and session state

This request does not have session state requirements. The call is privileged and the client must be properly authorized to perform this action.


## password_change

> Change logon password

```json
{
  "method": "password_change",
  "params": {
    "old_password": "Current565",
    "new_password": "NewBetter!Pas\"sword2"
  }
}
```

> Example response on successful change

```json
{
  "id": null,
  "result": null
}
```

The `password_change` function allows the authenticated user to change his/her logon password. The currently valid password must be provided in the `old_password` field. The usual password complexity requirements apply for the `new_password` field.

### Request

This API function accepts the following parameters:

Parameter    | Type         | Required | Description
------------ | ------------ | -------- | -----------
old_password | string       | yes      | Current valid password
new_password | string       | yes      | New password that meets the [complexity requirements](#password-complexity)

### Response and action

Authenticated user's logon password has been changed. The current authenticated sessions continue normally but the new password is immediately effective for further authentications.

*Successful*: Element `result` has `null` value.

*Error*: Exception response is returned.

Exception               | Code         | Reason
----------------------- | ------------ | -------------
ECSecurityException     | 1100         | User is not valid or active, might be pending EULA approval
ECSecurityException     | 1101         | Password does not meet complexity requirements
ECClientException       | 105          | Old password is incorrect
ECClientException       | 101          | No session

### Permissions and session state

Everyone is allowed to call this function. Session must be in authenticated state.
