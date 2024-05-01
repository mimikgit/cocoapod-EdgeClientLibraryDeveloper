# ``mimik Client Library for iOS``

mimik Client Library for iOS provides a programmatic way to work with the edgeEngine Runtime to access information about the mobile device on which the application is running.

@Metadata {
    @CallToAction(purpose: link, url: "https://github.com/mimikgit/cocoapod-EdgeClientLibrary")
    @PageKind(article)
    @PageColor(purple)
}


## Overview

The purpose of the **mimik Client Library for iOS** is to provide a programmatic way to work with the edgeEngine Runtime to access information about the mobile device on which the application is running, as well as mobile devices running within a cluster of mobile devices that are hosting the edgeEngine Runtime. Also, to allow developers to use edge microservices running within a particular device cluster.

The **mimik Client Library for iOS suite** consists of two sets of cocoapods:

	- EdgeClientLibrary           (EdgeCore + EdgeEngine)
	- EdgeClientLibraryDeveloper  (EdgeCore + EdgeEngineDeveloper)

or four individual cocoapod components:

	- EdgeCore
	- EdgeEngine 
	- EdgeEngineDeveloper
	- EdgeUser


By leveraging the **`EdgeCore`** cocoapod component, developers can build applications compatible with the edgeEngine Runtime.

Additionally, this component provides utility APIs that help developers with core operations such as Authentication, edgeEngine Runtime Setup, deployment of edge microservices and handling of network calls.

Furthermore, the **`EdgeEngine`** and **`EdgeEngineDeveloper`** cocoapod components provides edgeEngine Runtime lifecycle control API, as well as vendoring the actual edgeEngine Runtime binary into the iOS project.

Expanding the client library ecosystem is the **`EdgeUser`** cocoapod component, providing additional user facing API.


## Supported Platforms, Targets
* `iOS Devices running iOS 15+`
* `iOS Simulators running iOS 15+`
* `iOS Mac Catalyst running macOS 12.0`


## Requirements
```
iOS 15.0+
```

## Installation

To integrate **mimik Client Library** components to iOS projects, developers simply add the following lines to their Podfile:

> **_NOTE:_** Developers wanting to use their **developer edge license** from [mimik developer console](https://developer.mimik.com/console) should specify  [EdgeEngineDeveloper](https://github.com/mimikgit/cocoapod-EdgeEngineDeveloper) cocoapod in their `Podfile`.

> **_NOTE:_** Enterprise developers should specify [EdgeEngine](https://github.com/mimikgit/cocoapod-EdgeEngine) cocoapod in their `Podfile` and use the **full edge license** they received from [mimik support](https://developer.mimik.com/support/).


```swift
platform :ios, '15.0'
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/mimikgit/cocoapod-edge-specs.git'

use_frameworks!
inhibit_all_warnings!

def mimik
  pod 'EdgeCore'             # all developers
  pod 'EdgeEngineDeveloper'  # developers with developer edge license
  ### or pod 'EdgeEngine'    # developers with full edge license
end

target '{target}' do
  mimik()
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['ENABLE_BITCODE'] = 'NO'
      config.build_settings['VALID_ARCHS'] = '$(ARCHS_STANDARD_64_BIT)'
      config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '15.0'
      config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
    end
  end
end
```
> **_NOTE:_** Alternativelly, instead of specifying individual cocoapods, developers can also use the **mimik Client Library cocoapods sets**. So for example, instead of EdgeCore and EdgeEngine, they can use **EdgeClientLibrary**. And instead of EdgeCore and EdgeEngineDeveloper, they can use **EdgeClientLibraryDeveloper**. Same notes regarding the edge licenses mentioned above apply.

## Tutorials

After installation, try the following tutorials:

- [Integrating the mimik Client Library into an iOS project](https://devdocs.mimik.com/tutorials/11-index)
- [Working with edgeEngine in an iOS project](https://devdocs.mimik.com/tutorials/12-index)
- [Working with edge microservices in an iOS project](https://devdocs.mimik.com/tutorials/13-index)
- [Creating a Simple iOS Application that Uses an edge microservice](https://devdocs.mimik.com/tutorials/10-index)


## Documentation

`EdgeCore/EdgeClient` API reference documentation can be found  [online](https://mimikgit.github.io/cocoapod-EdgeCore/documentation/edgecore/edgeclient). Alternatively a docc archive file can be downloaded as a [zip file](https://github.com/mimikgit/cocoapod-EdgeCore/tree/main/EdgeCore.doccarchive.zip) and opened locally in Xcode.

`EdgeEngineClient` platform protocol API reference documentation can also be found [online](https://mimikgit.github.io/cocoapod-EdgeCore/documentation/edgecore/edgeengineclient).

`EdgeUserClient` platform protocol API reference documentation can be found [online](https://mimikgit.github.io/cocoapod-EdgeCore/documentation/edgecore/edgeuserclient) as well.

## EdgeClient

### Authentication

- ``EdgeClient/authorizeDeveloper(developerIdToken:edgeEngineIdToken:)``
- ``EdgeClient/authorizeUser(email:password:edgeEngineIdToken:service:)``
- ``EdgeClient/authorizeUser(phoneNumber:edgeEngineIdToken:service:)``
- ``EdgeClient/validateUser(codes:edgeEngineIdToken:service:)``
- ``EdgeClient/signup(email:password:edgeEngineIdToken:service:)``
- ``EdgeClient/validateSignup(codes:password:edgeEngineIdToken:service:)``
- ``EdgeClient/authorizeBackendUse(authorization:service:)``
- ``EdgeClient/authorizeBackendUse(federatedToken:policyId:edgeEngineIdToken:service:)``
- ``EdgeClient/authenticationScopes(serverUrl:)``

### Account Management

- ``EdgeClient/accountInformation(service:authorization:)``
- ``EdgeClient/passwordReset(email:edgeEngineIdToken:service:)``
- ``EdgeClient/validatePasswordReset(codes:newPassword:edgeEngineIdToken:service:)``
- ``EdgeClient/passwordChange(email:currentPassword:newPassword:edgeEngineIdToken:service:)``
- ``EdgeClient/deleteAccount(email:password:edgeEngineIdToken:service:)``
- ``EdgeClient/validateDeleteAccount(codes:password:edgeEngineIdToken:service:)``
- ``EdgeClient/executeDeleteAccount(authorization:service:)``

### Environment

- ``EdgeClient/edgeEngineIdToken()``
- ``EdgeClient/edgeEngineInfo()``
- ``EdgeClient/externalEdgeEngineIsRunning()``
- ``EdgeClient/edgeEngineFullPathUrl()``

### Edge microservices

- ``EdgeClient/deployMicroservice(edgeEngineAccessToken:config:imageTarPath:)``
- ``EdgeClient/deployedMicroservices(edgeEngineAccessToken:)``
- ``EdgeClient/deployedMicroservice(imageId:containerId:edgeEngineAccessToken:)``
- ``EdgeClient/deployedMicroserviceImages(edgeEngineAccessToken:)``
- ``EdgeClient/deployedMicroserviceImage(imageId:edgeEngineAccessToken:)``
- ``EdgeClient/deployedMicroserviceContainers(edgeEngineAccessToken:)``
- ``EdgeClient/deployedMicroserviceContainer(containerId:edgeEngineAccessToken:)``
- ``EdgeClient/updateMicroservice(edgeEngineAccessToken:microservice:imageTarPath:envVariables:)``
- ``EdgeClient/updateMicroserviceEnv(edgeEngineAccessToken:microservice:envVariables:)``
- ``EdgeClient/undeployMicroservice(edgeEngineAccessToken:microservice:)``
- ``EdgeClient/undeployMicroserviceComponent(edgeEngineAccessToken:component:identifier:)``

### EdgeClient Type

- ``EdgeClient/activateExternalEdgeEngine(host:port:)``
- ``EdgeClient/deactivateExternalEdgeEngine()``
- ``EdgeClient/edgeEngineWorkingDirectory()``
- ``EdgeClient/externalEdgeEngineIsActivated()``
- ``EdgeClient/setLoggingLevel(level:module:)``


## EdgeClient/Document
- ``EdgeClient/Document/filenameExtensionFor(type:)``
- ``EdgeClient/Document/filenameExtentionFor(mimeType:)``
- ``EdgeClient/Document/mimeTypeFor(filenameExtension:)``
- ``EdgeClient/Document/mimeTypeFor(type:)``
- ``EdgeClient/Document/uttypeFor(filenameExtension:)``
- ``EdgeClient/Document/uttypeFor(mimeType:)``


## EdgeClient/Log
- ``EdgeClient/Log/log(level:function:line:items:module:marker:privacy:)``
- ``EdgeClient/Log/logDebug(function:line:items:module:marker:privacy:)``
- ``EdgeClient/Log/logError(function:line:items:module:marker:privacy:)``
- ``EdgeClient/Log/logFault(function:line:items:module:marker:privacy:)``
- ``EdgeClient/Log/logInfo(function:line:items:module:marker:privacy:)``
- ``EdgeClient/Log/loggingLevel(module:)``


## EdgeClient/Microservice
- ``EdgeClient/Microservice/basePath()``
- ``EdgeClient/Microservice/fullPathUrl()``
- ``EdgeClient/Microservice/fullPathUrl(withEndpoint:)``
- ``EdgeClient/Microservice/expectedDeployedBasePath(path:clientId:)``
- ``EdgeClient/Microservice/expectedDeployedImageId(imageName:clientId:)``
- ``EdgeClient/Microservice/expectedDeployedContainerId(containerName:clientId:)``
- ``EdgeClient/Microservice/preferredBasePath(path:)``
- ``EdgeClient/Microservice/preferredConfig(imageName:containerName:basePath:edgeEngineFullPathUrl:clientId:envVariables:signatureKey:ownerCode:)``
- ``EdgeClient/Microservice/preferredContainerName(name:)``
- ``EdgeClient/Microservice/preferredImageName(name:)``
- ``EdgeClient/Microservice/validateMicroserviceResponse(response:encapsulatedData:)``


## EdgeClient/Node
- ``EdgeClient/Node/effectiveUrl()``
- ``EdgeClient/Node/preferredNodeName()``


## EdgeClient/Request
- ``EdgeClient/Request/downloadContent(sourceUrl:destinationFileUrl:progressHandler:)``
- ``EdgeClient/Request/downloadImageContent(sourceUrl:)``
- ``EdgeClient/Request/exportVideo(session:outputURL:outFileType:)``
- ``EdgeClient/Request/uploadContent(sourceFileUrl:destinationUrl:mimeType:progressHandler:)``
- ``EdgeClient/Request/authorizedRequest(url:method:authorization:httpHeaders:parameters:contentType:)``
- ``EdgeClient/Request/call(config:)``
- ``EdgeClient/Request/call(config:type:)``
- ``EdgeClient/Request/decode(_:from:)``
- ``EdgeClient/Request/responseError(response:)``
- ``EdgeClient/Request/responseJSON(response:dataKey:)``


## EdgeClient/Service
- ``EdgeClient/Service/healthCheck()``
- ``EdgeClient/Service/urlComponents()``
- ``EdgeClient/Service/versionCheck(requireMatch:)``


## EdgeEngineClient protocol
> **_NOTE:_**  To activate the EdgeEngineClient protocol API developers have to add [EdgeEngine](https://github.com/mimikgit/cocoapod-EdgeEngine) or [EdgeEngineDeveloper](https://github.com/mimikgit/cocoapod-EdgeEngineDeveloper) cocoapod to their `Podfile`.

- ``EdgeEngineClient/startEdgeEngine(parameters:)``
- ``EdgeEngineClient/stopEdgeEngine()``
- ``EdgeEngineClient/restartEdgeEngine()``
- ``EdgeEngineClient/resetEdgeEngine()``
- ``EdgeEngineClient/edgeEngineIsRunning()``
- ``EdgeEngineClient/edgeEngineLifecycleIsManaged()``
- ``EdgeEngineClient/edgeEngineParameters()``
- ``EdgeEngineClient/manageEdgeEngineLifecycle(manage:)``
- ``EdgeEngineClient/expectedEdgeEngineVersion()``
- ``EdgeEngineClient/setCustomPort(number:)``

## EdgeUserClient protocol
> **_NOTE:_**  To activate the EdgeUserClient protocol API developers have to add [EdgeUser](https://github.com/mimikgit/cocoapod-EdgeUser) cocoapod to their `Podfile`.

- ``EdgeUserClient/addUserProfileNotificationsConsent(service:authorization:consent:)``
- ``EdgeUserClient/deleteUserProfileNotificationsConsent(service:authorization:consentId:)``
- ``EdgeUserClient/findUser(service:authorization:email:)``
- ``EdgeUserClient/updateUserProfile(service:authorization:update:)``
- ``EdgeUserClient/updateUserProfileAttributes(service:authorization:attributes:)``
- ``EdgeUserClient/updateUserProfileProperties(service:authorization:parameters:)``
- ``EdgeUserClient/userProfile(service:authorization:)``

- ``EdgeUserClient/addressSuggestions(service:authorization:address:maxSuggestions:language:countries:geoLocation:geoFence:)``
- ``EdgeUserClient/places(service:authorization:address:maxPlaces:language:countries:geoLocation:geoFence:)``

- ``EdgeUserClient/acceptFriendship(service:authorization:friendId:)``
- ``EdgeUserClient/cancelFriendship(service:authorization:friendId:)``
- ``EdgeUserClient/friendList(service:authorization:)``
- ``EdgeUserClient/friends(service:authorization:)``
- ``EdgeUserClient/receivedFriendRequests(service:authorization:)``
- ``EdgeUserClient/receivedFriendRequestList(service:authorization:)``
- ``EdgeUserClient/requestFriendship(service:authorization:friendId:)``
- ``EdgeUserClient/sentFriendRequests(service:authorization:)``
- ``EdgeUserClient/sentFriendRequestList(service:authorization:)``


## EdgeProvider

### Beams

- ``EdgeProvider/Beams/beamTokens(userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:)``
- ``EdgeProvider/Beams/beams(userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:)``
- ``EdgeProvider/Beams/create(beam:userAccessToken:edgeEngineFullPathUrl:edgeAccessToken:microservice:ownerCode:)``
- ``EdgeProvider/Beams/deleteBeam(id:userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:)``
- ``EdgeProvider/Beams/deleteBeamToken(id:userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:)``
- ``EdgeProvider/Beams/openBeam(id:userAccessToken:edgeEngineFullPathUrl:edgeAccessToken:microservice:ownerCode:)``
- ``EdgeProvider/Beams/updateBeam(id:userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:status:)``
- ``EdgeProvider/Beams/updateBeamToken(id:userAccessToken:edgeEngineFullPathUrl:microservice:ownerCode:status:)``

### Drive

- ``EdgeProvider/Drive/create(file:userAccessToken:edgeEngineFullPathUrl:driveMicroservice:driveOwnerCode:)``
- ``EdgeProvider/Drive/delete(file:userAccessToken:edgeEngineFullPathUrl:driveMicroservice:driveOwnerCode:)``
- ``EdgeProvider/Drive/fileWith(localId:userAccessToken:edgeEngineFullPathUrl:microservice:)``
- ``EdgeProvider/Drive/files(userAccessToken:edgeEngineFullPathUrl:microservice:)``

### Clusters

- ``EdgeProvider/Clusters/accountNodes(userAccessToken:edgeEngineFullPathUrl:microservice:edgeAccessToken:)``
- ``EdgeProvider/Clusters/friendNodes(userAccessToken:edgeEngineFullPathUrl:edgeAccessToken:microservice:)``
- ``EdgeProvider/Clusters/friendNodesList(userAccessToken:edgeEngineFullPathUrl:edgeAccessToken:microservice:)``
- ``EdgeProvider/Clusters/nearbyNodes(userAccessToken:edgeEngineFullPathUrl:microservice:edgeAccessToken:)``
- ``EdgeProvider/Clusters/nodePresenceCheck(nodeId:userAccessToken:edgeEngineFullPathUrl:edgeAccessToken:microservice:)``


## mimik client libraries

Don't forget to checkout all mimik client libraries [available on Github](https://github.com/search?q=cocoapod-Edge)

Cocoapods sets:

 * [EdgeClientLibrary = (EdgeCore + EdgeEngine)](https://github.com/mimikgit/cocoapod-EdgeClientLibrary)
 * [EdgeClientLibraryDeveloper = (EdgeCore + EdgeEngineDeveloper)](https://github.com/mimikgit/cocoapod-EdgeClientLibraryDeveloper)
 
Individual cocoapods:
 
 * [EdgeCore](https://github.com/mimikgit/cocoapod-EdgeCore)
 * [EdgeEngine](https://github.com/mimikgit/cocoapod-EdgeEngine)
 * [EdgeEngineDeveloper](https://github.com/mimikgit/cocoapod-EdgeEngineDeveloper)
 * [EdgeUser](https://github.com/mimikgit/cocoapod-EdgeUser)


## Author

[mimik Technology, Inc.](https://mimik.com)

More details about how the edgeEngine platform revolutionizes computing with the hybrid-cloud approach are at [mimik Developer Documentation](https://devdocs.mimik.com).


## License

Developers can get their developer edge license by following this [tutorial](https://devdocs.mimik.com/tutorials/02-index).

For details about a full edge license please contact [mimik support](https://mimik.com/contact-us/).
