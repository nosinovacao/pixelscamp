# NOS Inovação APIs

Welcome to the NOS Inovação APIs!
In this page you can find all the information you need to develop and integrate your __awesome application__ with our available data through these APIs.

## OData Protocol

The REST APIs relating the EPG & VOD content are fully compatible with the OData v3 protocol.
This protocol provides an abstraction with some common use-cases to consume data from a REST API.

Some IDEs (e.g. Visual Studio) have client-code generators to consume this type of services. The /$metadata endpoint provides information relating the service schema.

For more information about all the possibilities that this protocol provides, please check the [documentation][1].

## EPG (Electronic Program Guide)

__API Endpoint__: [http://TBD/EPGRepositories/EPGCatalog.svc/][2]

This is an OData REST API that makes available NOS electronic programming guide for a 15 days time window (6 days in the past, current day and 7 days in the future).

### Domain Model

* Channel
* Event
* Program
* Category
* DailyUpdate

![EPG Domain Model](https://github.com/ctorrao/pixelscamp/blob/master/images/EPG_DomainModel_v1_0.png)

### Example Requests

_TBD_

## VOD (Video-on-Demand)

__API Endpoint__: [http://TBD/VODRepositories/VODCatalog.svc/][3]

This is an OData REST API that makes available NOS video-on-demand catalogue and contents, which includes all the assets (e.g. movies) available to be purchased or viewed by the NOS clients. This catalogue also includes the contents of the NPLAY subscription.

### Domain Model

* CatalogItem
* Item
* TitleAsset

![VOD Domain Model](https://github.com/ctorrao/pixelscamp/blob/master/images/VOD_DomainModel_v1_0.png)

### Example Requests

_TBD_

## Support

Please contact us if you need some support relating to these APIs.

[1]: http://www.odata.org/documentation/odata-version-3-0/
[2]: http://TBD/EPGRepositories/EPGCatalog.svc/
[3]: http://TBD/VODRepositories/VODCatalog.svc/