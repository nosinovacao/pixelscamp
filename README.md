# NOS Inovação APIs

![NOS Logo](https://github.com/nosinovacao/pixelscamp/blob/master/images/logo.png)

Welcome to the NOS Inovação APIs!
In this page you can find all the information you need to develop and integrate your __awesome application__ with our available data through these APIs.

## OData Protocol

The REST APIs relating the EPG & VOD content are fully compatible with the OData v3 protocol.
This protocol provides an abstraction with some common use-cases to consume data from a REST API.

Some IDEs (e.g. Visual Studio) have client-code generators to consume this type of services. The /$metadata endpoint provides information relating the service schema.

For more information about all the possibilities that this protocol provides, please check the [documentation][1].

## EPG (Electronic Program Guide)

__API Endpoint__: [http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/][2]

This is an OData REST API that makes available NOS electronic programming guide for a 15 days time window (6 days in the past, current day and 7 days in the future).

### Domain Model

The TV Guide repository is composed mainly by: __Channels__, __Events__ and __Programs__. A given __Channel__ have a list of __Events__, which have information about the duration and start time in which a specific __Program__ is aired in the TV. Therefore, some __Programs__ exist only once in the repository, but are shared by multiple __Events__ (e.g. a movie is aired several times in TVCines, but the metadata of that __Program__ exists only once).

Entities:
* Channel
* Event
* Program
* Category
* DailyUpdate

![EPG Domain Model](https://github.com/ctorrao/pixelscamp/blob/master/images/EPG_DomainModel_v1_0.png)

### Example Requests

#### Service Schema Metadata

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/\$metadata"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/$metadata)

#### All TV Channels

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Channel?\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Channel?$format=json)

#### TV Guide for RTP1 TV Channel for day 2016-10-07

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Event?\$format=json&\$filter=ServiceId%20eq%20%275%27%20and%20UtcBeginDate%20ge%20datetime%272016-10-04T00:00:00Z%27%20and%20UtcEndDate%20lt%20datetime%272016-10-05T00:00:00Z%27"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Event?$format=json&$filter=ServiceId%20eq%20%275%27%20and%20UtcBeginDate%20ge%20datetime%272016-10-04T00:00:00Z%27%20and%20UtcEndDate%20lt%20datetime%272016-10-05T00:00:00Z%27)

#### TV Guide for RTP1 TV Channel for day 2016-10-07 (including Program entity in the response)

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Event?\$format=json&\$filter=ServiceId%20eq%20%275%27%20and%20UtcBeginDate%20ge%20datetime%272016-10-04T00:00:00Z%27%20and%20UtcEndDate%20lt%20datetime%272016-10-05T00:00:00Z%27&\$expand=Program"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Event?$format=json&$filter=ServiceId%20eq%20%275%27%20and%20UtcBeginDate%20ge%20datetime%272016-10-04T00:00:00Z%27%20and%20UtcEndDate%20lt%20datetime%272016-10-05T00:00:00Z%27&$expand=Program)

#### Specific Program Detail

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Program('1395116')?\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/Program('1395116')?$format=json)

## VOD (Video-on-Demand)

__API Endpoint__: [http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/][3]

This is an OData REST API that makes available NOS video-on-demand catalogue and contents, which includes all the assets (e.g. movies) available to be purchased or viewed by the NOS clients. This catalogue also includes the contents of the NPLAY subscription.

### Domain Model

The VOD repository is composed mainly by a tree of __Items__, which can be typified as __Categories__ or __Packages__. The __Package__ has all the metadata information in their __TitleAsset__/__TitleMetaData__, and has all the purchase information in __OfferMetaData__.

Entities:
* CatalogItem
* Item
* TitleAsset

![VOD Domain Model](https://github.com/ctorrao/pixelscamp/blob/master/images/VOD_DomainModel_v1_0.png)

### Example Requests

#### Service Schema Metadata

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/\$metadata"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/$metadata)

#### Root CatalogItem(Type=Category)

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem?\$filter=IsRoot%20eq%20true&\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem?$filter=IsRoot%20eq%20true&$format=json)

#### CatalogItem(Type=Category) with Children(Type=Category)

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem('ott.1092710789')/ChildCatalogItems?\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem('ott.1092710789')/ChildCatalogItems?$format=json)

#### CatalogItem(Type=Category) with Children(Type=Package)

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem('ott.245048737')/ChildCatalogItems?\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/CatalogItem('ott.245048737')/ChildCatalogItems?$format=json)

#### Item(Type=Package) TitleAsset Detail

```bash
$ curl -XGET "http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/Item('TVOD_051518_LUS_XMENAPOC_1045332_CIPK08522BC27F0C4C67')/zon.vod.central.odata.api.model.Package/TitleAsset?\$format=json"
```
[Test in Browser](http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/Item('TVOD_051518_LUS_XMENAPOC_1045332_CIPK08522BC27F0C4C67')/zon.vod.central.odata.api.model.Package/TitleAsset?$format=json)

## Search

__API Endpoint__: [http://nos-brpx-search.northeurope.cloudapp.azure.com][4]

This is an Elasticsearch instance that contains both the EPG and VOD indexes. Each entity specified in the VOD and EPG domain models maps to an Elasticsearch type.

If you are only interested in searching **VOD** movies and **EPG** programs, the only types that concern you are:
* **program** for epg programs
* **titleasset** for vod movies

For more information, refer to the Elasticsearch Search api documentation: https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html

### Example requests

#### Generic search

* Executes a generic search against all indexes and types.
* This is the most generic search form. 
* You will have to parse the result **index** and **type** to get the result context.

`$ curl -XGET 'http://nos-brpx-search.northeurope.cloudapp.azure.com/_search?q=batman'`

#### Search EPG programs

```bash
# Find an EPG program that contains the 'batman' keyword within its properties
$ curl -XGET 'http://nos-brpx-search.northeurope.cloudapp.azure.com/_all/program/_search?q=batman'
```

#### Search VOD movies

```bash
# Find a VOD movie (TitleAsset) that contains the 'batman' keyword within its properties
$ curl -XGET 'http://nos-brpx-search.northeurope.cloudapp.azure.com/_all/titleasset/_search?q=batman'
```

#### Search both VOD movies and EPG programs

* If you want you may combine additional types in the query
* Each type is separated by a comma `,`

```bash
# Find either a VOD or EPG match
$ curl -XGET 'http://nos-brpx-search.northeurope.cloudapp.azure.com/_all/titleasset,program/_search?q=batman'
```

## Support

Please contact us in [![Slack](https://github.com/nosinovacao/pixelscamp/blob/master/images/slack.png)][5] if you need some support relating to these APIs.

[1]: http://www.odata.org/documentation/odata-version-3-0/
[2]: http://nos-brpx.northeurope.cloudapp.azure.com/EPGRepositories/EPGCatalog.svc/
[3]: http://nos-brpx.northeurope.cloudapp.azure.com/VODRepositories/VODCatalog.svc/
[4]: http://nos-brpx-search.northeurope.cloudapp.azure.com
[5]: https://pixelscamp.slack.com/messages/nos/