---
title: Azure Cosmos DB veritabanı geçiş aracını | Microsoft Docs
description: Açık kaynak Azure Cosmos DB veri geçiş araçları Azure Cosmos DB MongoDB, SQL Server, tablo depolama, Amazon DynamoDB, CSV ve JSON dosyaları dahil olmak üzere çeşitli kaynaklardan veri almak için nasıl kullanılacağını öğrenin. CSV JSON dönüştürme.
keywords: json, veritabanı Geçiş Araçları, CSV'ye Dönüştür csv için json
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: ''
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 317e5f2696635d28b5dbab302e45960af9c8aee2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB: Veri geçiş aracı

Bu öğretici, Azure Cosmos DB koleksiyonları ve tablolar halinde veriler çeşitli kaynaklardan içeri aktarabilirsiniz Azure Cosmos DB Veri Taşıma aracını kullanma hakkında yönergeler sağlar. JSON dosyaları, CSV dosyaları, SQL, MongoDB, Azure Table depolama, Amazon DynamoDB ve hatta Azure Cosmos DB SQL API koleksiyonları alabilir ve verileri koleksiyonları ve tablolar için Azure Cosmos DB ile kullanmak geçirilir. Veri Geçiş Aracı, tek bir bölüm koleksiyondan çok bölümlü bir koleksiyon için SQL API'yi geçirirken kullanılabilir.

Hangi API Azure Cosmos DB ile kullanacağınız? 
* **[SQL API](documentdb-introduction.md)**  -veri almak için veri geçiş aracı sağlanan kaynak seçeneklerinden herhangi birini kullanabilirsiniz.
* **[Tablo API](table-introduction.md)**  -verileri içe aktarmak için veri geçiş aracı veya AzCopy kullanabilirsiniz. Bkz: [Azure Cosmos DB tablo API ile kullanmak için içeri aktarma verileri](table-import.md) daha fazla bilgi için.
* **[MongoDB API](mongodb-introduction.md)**  -veri geçiş aracı desteklememektedir Azure Cosmos DB MongoDB API'si bir kaynak veya hedef olarak. İçinde veya Azure Cosmos DB MongoDB API koleksiyonlarda dışında veri geçirmek istiyorsanız, başvurmak [Azure Cosmos DB: MongoDB API'si veri geçirmek nasıl](mongodb-migrate.md) ilişkin yönergeler. SQL API ile kullanmak için Azure Cosmos DB SQL API koleksiyonlara adresinden verilerini dışarı aktarmak için veri geçiş aracı kullanmaya devam edebilirsiniz. 
* **[Grafik API'si](graph-introduction.md)**  -veri geçiş aracı değil grafik API'si hesapları için desteklenen alma aracı şu anda. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Veri geçiş aracı yükleme
> * Farklı veri kaynaklarından veri alma
> * Azure Cosmos DB JSON olarak dışarı aktarma

## <a id="Prerequisites"></a>Önkoşullar
Bu makaledeki yönergeleri izlemeden önce aşağıdakilerin yüklü olduğundan emin olun:

* [Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) ya da daha yüksek.

## <a id="Overviewl"></a>Genel bakış
Veri Geçiş Aracı, verileri Azure Cosmos DB bir çeşitli kaynaklardan dahil olmak üzere, içe aktaran bir açık kaynak çözümü olmasıdır:

* JSON dosyaları
* MongoDB
* SQL Server
* CSV dosyaları
* Azure Tablo depolama
* Amazon DynamoDB
* HBase
* Azure Cosmos DB koleksiyonları

Bir grafik kullanıcı arabirimi (dtui.exe) alma aracı içerir, ancak bunu da komut satırından (dt.exe) bulunarak belirlenebilir. Aslında, kullanıcı Arabirimi aracılığıyla içe kurduktan sonra ilişkili komut çıktısı için bir seçenek yoktur. Hiyerarşik ilişkileri (alt) içeri aktarma sırasında oluşturulabilir, tablo kaynak verileri (örn. SQL Server ya da CSV dosyaları) dönüştürülebilir. Kaynak seçenekleri hakkında daha fazla bilgi için her bir kaynak, hedef seçenekleri ve görüntüleme içeri aktarma sonuçları almak için komut satırları örnek okuma tutun.

## <a id="Install"></a>Yükleme
Geçiş Aracı kaynak kodu Github'da üzerinde kullanılabilir [bu havuzda](https://github.com/azure/azure-documentdb-datamigrationtool). Karşıdan yükle ve yerel olarak çözüm derlemek veya [önceden derlenmiş ikili dosya indirme](https://cosmosdbportalstorage.blob.core.windows.net/datamigrationtool/2018.02.28-1.8.1/dt-1.8.1.zip), ya da çalıştırın:

* **Dtui.exe**: grafik arabirim aracı sürümü
* **Dt.exe**: komut satırı aracı sürümü

## <a name="select-data-source"></a>Veri kaynağı seç

Aracı yükledikten sonra verilerinizi almak için zaman yapılır. Ne tür verileri içeri aktarmak istiyor musunuz?

* [JSON dosyaları](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB dışarı aktarma dosyaları](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV dosyaları](#CSV)
* [Azure Tablo Depolama](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [Blob](#BlobImport)
* [Azure Cosmos DB koleksiyonları](#SQLSource)
* [HBase](#HBaseSource)
* [Azure Cosmos DB toplu içeri aktarma](#SQLBulkImport)
* [Azure Cosmos DB sıralı kayıt alma](#DocumentDSeqTarget)


## <a id="JSON"></a>JSON dosyaları alma
JSON dosyası kaynak alma seçeneği alma bir veya daha fazla tek bir belge JSON dosyaları veya JSON dosyaları her bir dizi JSON belgelerini içeren sağlar. İçeri aktarmak için JSON dosyaları içeren klasörleri eklerken, alt klasörler dosyaları aranıyor yinelemeli seçeneğiniz vardır.

![Ekran görüntüsü, JSON dosyası kaynak seçenekleri - veritabanı Geçiş Araçları](./media/import-data/jsonsource.png)

JSON dosyalarını içeri aktarmak için bazı komut satırı örnekleri şunlardır:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>MongoDB alma

> [!IMPORTANT]
> MongoDB için desteği olan bir Azure Cosmos DB hesap alıyorsanız, aşağıdaki adımları [yönergeleri](mongodb-migrate.md).
> 
> 

MongoDB kaynak içeri Aktarıcı seçenek, tek bir MongoDB koleksiyonundan almak ve isteğe bağlı olarak bir sorgu kullanarak belgelere filtre ve/veya bir yansıtma kullanarak belge yapısı değiştirme olanak tanır.  

![Ekran görüntüsü, MongoDB kaynak seçenekleri](./media/import-data/mongodbsource.png)

Bağlantı dizesi standart MongoDB biçimdedir:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Bağlantı dizesi alanına belirtilen MongoDB örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Veri içeri aktarılacak koleksiyonun adını girin. İsteğe bağlı olarak belirttiğinizde veya bir sorgu için bir dosya belirtin (örneğin {pop: {$gt: 5000}}) ve/veya yansıtma (örneğin {loc:0}) hem filtre ve içeri aktarılacak veri şekli.

Adresinden almak için bazı komut satırı örnekleri şunlardır:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>MongoDB dışarı aktarma dosyaları alma

> [!IMPORTANT]
> MongoDB için desteği olan bir Azure Cosmos DB hesap alıyorsanız, aşağıdaki adımları [yönergeleri](mongodb-migrate.md).
> 
> 

MongoDB verme JSON dosyasını kaynak alma seçeneği mongoexport yardımcı programını üretilen bir veya daha fazla JSON dosyalarını içeri aktarmak sağlar.  

![Ekran görüntüsü, MongoDB dışa aktarma kaynak seçenekleri](./media/import-data/mongodbexportsource.png)

Alma için MongoDB verme JSON dosyaları içeren klasörleri eklerken, alt klasörler dosyaları aranıyor yinelemeli seçeneğiniz vardır.

MongoDB verme JSON dosyalarından içeri aktarma için bir komut satırı örnek aşağıda verilmiştir:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>SQL Server'dan alma
SQL kaynak alma seçeneği, tek bir SQL Server veritabanından içeri aktarın ve isteğe bağlı olarak bir sorgu kullanarak içeri aktarılacak kayıtlarını filtrelemek sağlar. Ayrıca, bir iç içe geçmiş ayırıcı (daha ayrıntılı bir dakika içinde) belirterek, belge yapısı değiştirebilirsiniz.  

![Ekran görüntüsü, SQL kaynak seçenekleri - veritabanı Geçiş Araçları](./media/import-data/sqlexportsource.png)

Bağlantı dizesi standart SQL bağlantı dizesi biçimi biçimidir.

> [!NOTE]
> Bağlantı dizesi alanında belirtilen SQL Server örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

İç içe geçmiş ayırıcı özellik alma sırasında hiyerarşik ilişkileri (alt belgeler) oluşturmak için kullanılır. Aşağıdaki SQL sorgusunu göz önünde bulundurun:

*Kimliği, ad, [Address.AddressType] olarak AddressType, AddressLine1 [Address.AddressLine1] olarak, [Address.Location.City] olarak Şehir, StateProvinceName [Address.Location.StateProvinceName] olarak, PostalCode [olarak dönüştürme (BusinessEntityID AS varchar) seçin Address.PostalCode], [Address.CountryRegionName] olarak CountryRegionName Sales.vStoreWithAddresses gelen nerede AddressType 'Ana ofis' =*

Aşağıdaki (kısmi) sonuçları döndüren:

![Ekran görüntüsü, SQL sorgu sonuçları](./media/import-data/sqlqueryresults.png)

Diğer adlar Address.AddressType ve Address.Location.StateProvinceName gibi unutmayın. İç içe geçmiş bir ayırıcı olarak belirterek '.', içeri aktarma aracını içeri aktarma sırasında adresi ve Address.Location belgeler oluşturur. Azure Cosmos DB ortaya çıkan bir belgede bir örneği burada verilmiştir:

*{"ID": "956", "Name": "Yüksekse satış ve"Adres"hizmet": {"AddressType": "Ana ofis", "AddressLine1": "#500 75 O'Connor Sokak", "Konum": {"Şehir": "Ottawa", "StateProvinceName": "Ontario"}, "PostalCode": "K4B 1S2", "CountryRegionName": " Kanada"}}*

SQL Server'dan almak için bazı komut satırı örnekleri şunlardır:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>CSV dosyalarını içeri aktarmak ve CSV JSON biçimine Dönüştür
CSV dosya kaynak içeri Aktarıcı seçeneği, bir veya daha fazla CSV dosyalarını içeri aktarmanıza olanak sağlar. İçeri aktarma için CSV dosyaları içeren klasörleri eklerken, alt klasörler dosyaları aranıyor yinelemeli seçeneğiniz vardır.

![Ekran görüntüsü, CSV kaynak seçenekleri - JSON CSV'ye](media/import-data/csvsource.png)

SQL kaynağına benzer, iç içe geçmiş ayırıcı özellik alma sırasında hiyerarşik ilişkileri (alt belgeler) oluşturmak için kullanılabilir. Aşağıdaki CSV üstbilgisi satır ve veri satırı göz önünde bulundurun:

![Ekran görüntüsü, CSV örnek kayıtlarını - JSON CSV'ye](./media/import-data/csvsample.png)

Diğer adlar DomainInfo.Domain_Name ve RedirectInfo.Redirecting gibi unutmayın. İç içe geçmiş bir ayırıcı olarak belirterek '.', içeri aktarma aracını içeri aktarma sırasında DomainInfo ve RedirectInfo belgeler oluşturur. Azure Cosmos DB ortaya çıkan bir belgede bir örneği burada verilmiştir:

*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*

İçeri aktarma aracını (tırnak içine alınmış değerler her zaman dize olarak kabul edilir) türü bilgileri CSV dosyaları tırnak işareti olmayan değerleri Infer dener.  Türleri, aşağıdaki sırayla tanımlanır: sayı, datetime, Boole değeri.  

CSV Import hakkında dikkat edilecek iki noktalar vardır:

1. Tırnak içine alınmış değerler olarak korunur sırasında varsayılan olarak, tırnak işareti olmayan değerler her zaman sekmeler ve alanları için atılır-değil. Bu davranış kırpma tırnak içine alınmış değerler onay kutusunu veya /s.TrimQuoted komut satırı seçeneği ile geçersiz kılınabilir.
2. Varsayılan olarak, tırnak işareti olmayan bir null, null değeri olarak kabul edilir. Bu davranışı geçersiz kılınabilir (diğer bir deyişle, tırnak işareti olmayan bir null "null" dize olarak işle) ile kabul NULL dize onay kutusunu veya /s.NoUnquotedNulls komut satırı seçeneği olarak tırnak işareti olmayan.

CSV Import için komut satırı bir örnek şudur:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>Azure tablo depolamasından içeri aktarma
Azure Table depolama kaynağı alma seçeneği, tek bir Azure Table depolama tablosundan içeri aktarmanıza olanak sağlar. İsteğe bağlı olarak, içeri aktarılacak tablo varlıkları filtreleyebilirsiniz. 

Azure tablo depolamasından içeri veri çıktı Azure Cosmos DB tabloları ve varlıkları tablo API ile kullanılmak üzere veya koleksiyonlar ve SQL API'yi ile kullanmak için belgelere olabilir. Bununla birlikte; Tablo API yalnızca komut satırı yardımcı programı hedef olarak kullanılabilir, veri Geçiş Aracı kullanıcı arabirimini kullanarak tablo API'sine veremezsiniz. Daha fazla bilgi için bkz: [Azure Cosmos DB tablo API ile kullanmak için içeri aktarma verileri](table-import.md). 

![Azure tablo ekran depolama kaynağı seçenekleri](./media/import-data/azuretablesource.png)

Azure Table depolama bağlantı dizesi biçimi şöyledir:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Bağlantı dizesi alanına belirtilen Azure Table depolama örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Alınacak Azure tablo adını girin. İsteğe bağlı olarak belirtebilir bir [filtre](https://msdn.microsoft.com/library/azure/ff683669.aspx).

Azure Table depolama kaynağı alma seçeneği aşağıdaki ek seçenekler vardır:

1. İç alanlar
   1. Tüm - dahil tüm iç alanlar (PartitionKey, RowKey ve zaman damgası)
   2. Hiçbiri - hariç tüm iç alanları
   3. RowKey - yalnızca RowKey alan dahil et
2. Sütun Seçin
   1. Azure tablo depolama filtrelerini tahminleri desteklemez. Yalnızca belirli Azure tablo varlık özellikleri almak istiyorsanız, bunları seçin sütunlar listesine ekleyin. Diğer tüm varlık özellikleri göz ardı edilir.

Azure tablo depolamasından içeri aktarmak için komut satırı bir örnek şudur:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>Amazon DynamoDB alma
Amazon DynamoDB kaynağı içeri Aktarıcı seçenek, tek tek bir Amazon DynamoDB tablosundan alma ve isteğe bağlı olarak içeri aktarılacak varlıklara filtre olanak tanır. Alma ayarı kadar kolay böylece çeşitli şablonlar sağlanır.

![Amazon DynamoDB ekran kaynak seçenekleri - veritabanı Geçiş Araçları](./media/import-data/dynamodbsource1.png)

![Amazon DynamoDB ekran kaynak seçenekleri - veritabanı Geçiş Araçları](./media/import-data/dynamodbsource2.png)

Amazon DynamoDB bağlantı dizesinin biçimi şöyledir:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Bağlantı dizesi alanına belirtilen Amazon DynamoDB örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Amazon DynamoDB almak için komut satırı bir örnek şudur:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>Azure Blob depolama alanından içeri aktarma
JSON dosyasını, MongoDB dışarı aktarma dosyası ve CSV dosyası kaynak alma seçenekleri, bir veya daha fazla Azure Blob depolama alanından içeri aktarmanıza olanak verin. Bir Blob kapsayıcı URL'si ve hesap anahtarı belirttikten sonra içeri aktarmak üzere dosyaları seçmek için normal bir ifade girin.

![Ekran görüntüsü, Blob dosya kaynağı seçenekleri](./media/import-data/blobsource.png)

Azure Blob depolama alanından JSON dosyaları almak için komut satırı örnek aşağıda verilmiştir:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="SQLSource"></a>Bir SQL API koleksiyonundan alma
Azure Cosmos DB kaynak içeri Aktarıcı seçenek, bir veya daha fazla Azure Cosmos DB koleksiyonlarından verileri alır ve isteğe bağlı olarak bir sorgu kullanarak belgelere filtre olanak tanır.  

![Azure Cosmos DB ekran kaynak seçenekleri](./media/import-data/documentdbsource.png)

Azure Cosmos DB bağlantı dizesi biçimi şöyledir:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Bölümünde açıklandığı gibi Azure Portalı'nın anahtarları sayfasından Azure Cosmos DB hesabı bağlantı dizesi alınabilir [Azure Cosmos DB hesabın nasıl yönetileceği](manage-account.md), veritabanının adı için bağlantı dizesi eklenmesi gerekiyor ancak şu biçimde:

    Database=<CosmosDB Database>;

> [!NOTE]
> Bağlantı dizesi alanına belirtilen Azure Cosmos DB örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Tek bir Azure Cosmos DB koleksiyonundan içeri aktarmak için verileri içe aktarmak için koleksiyonun adını girin. Birden çok Azure Cosmos DB koleksiyonları içeri aktarmak için bir veya daha fazla koleksiyon adlarını eşleştirmek için normal bir ifade girin (örneğin, collection01 | collection02 | collection03). İsteğe bağlı olarak belirtin veya bir dosya filtresi ve Şekil alınacak veri için bir sorgu için sağlayın.

> [!NOTE]
> Adında normal ifade karakterler içeren tek bir koleksiyondan alıyorsanız, normal ifadeler, koleksiyon alanın kabul olduğundan, ardından bu karakterleri uygun şekilde kaçış uygulanmalıdır.
> 
> 

Aşağıdaki gelişmiş seçenekleri Azure Cosmos DB kaynak alma seçeneği vardır:

1. Dahili alanlar şunlardır: Azure Cosmos DB belge sistem özelliklerini dışa aktarma (örneğin, _rid, _ts) dahil gerekip gerekmediğini belirtir.
2. Hata yeniden deneme sayısı: Azure Cosmos DB bağlantısı geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme sayısını belirtir.
3. Yeniden deneme aralığı: nasıl geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda Azure Cosmos DB bağlantısı yeniden deneniyor arasında bekleneceğini belirtir.
4. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler DirectTcp, DirectHttps ve ağ geçidi ' dir. Doğrudan bağlantı modu yalnızca 443 numaralı bağlantı noktasını kullanır gibi ağ geçidi modu daha fazla güvenlik duvarı kolay olsa da, daha hızlıdır.

![Gelişmiş Seçenekleri Azure Cosmos DB ekran kaynağı](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> İçeri aktarma aracını bağlantı modu DirectTcp varsayılan değerdir. Güvenlik Duvarı sorunlarla karşılaşırsanız, yalnızca bir bağlantı noktası 443 gerektirdiği ağ geçidi bağlantı moduna geçer.
> 
> 

Azure Cosmos DB'den almak için bazı komut satırı örnekleri şunlardır:

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> Azure Cosmos DB verileri içe aktarma aracını ayrıca verilerini alma destekler [Azure Cosmos DB öykünücüsü](local-emulator.md). Yerel bir öykünücüsünden verilerini içeri aktarırken, uç nokta kümesine `https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>HBase alma
HBase kaynak alma seçeneği, bir HBase tablodan veri içeri aktarın ve isteğe bağlı olarak verileri filtrelemek sağlar. Alma ayarı kadar kolay böylece çeşitli şablonlar sağlanır.

![Ekran görüntüsü, HBase kaynak seçenekleri](./media/import-data/hbasesource1.png)

![Ekran görüntüsü, HBase kaynak seçenekleri](./media/import-data/hbasesource2.png)

HBase Stargate bağlantı dizesinin biçimi şöyledir:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Bağlantı dizesi alanına belirtilen HBase örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

HBase almak için komut satırı bir örnek şudur:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="SQLBulkTarget"></a>İçeri aktarma SQL API (Toplu içe aktarma)
Azure Cosmos DB toplu içeri Aktarıcı herhangi verimlilik için bir Azure Cosmos DB saklı yordamı kullanarak kullanılabilir kaynak seçeneklerinin almanıza izin verir. Aracı, tek tek bölümlenmiş Azure Cosmos DB koleksiyonuna alma yanı sıra, yapabildiği verileri birden çok tek bölümlenmiş Azure Cosmos DB koleksiyonlar genelinde bölümlenmiş parçalı alma destekler. Veri bölümlendirme hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md). Aracı oluşturur, yürütür ve ardından saklı yordamı hedef collection(s) siler.  

![Azure Cosmos DB ekran toplu seçenekleri](./media/import-data/documentdbbulk.png)

Azure Cosmos DB bağlantı dizesi biçimi şöyledir:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Bölümünde açıklandığı gibi Azure Portalı'nın anahtarları sayfasından Azure Cosmos DB hesabı bağlantı dizesi alınabilir [Azure Cosmos DB hesabın nasıl yönetileceği](manage-account.md), veritabanının adı için bağlantı dizesi eklenmesi gerekiyor ancak şu biçimde:

    Database=<CosmosDB Database>;

> [!NOTE]
> Bağlantı dizesi alanına belirtilen Azure Cosmos DB örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Tek bir koleksiyon almak için Ekle düğmesini tıklatın ve verilerini içeri için koleksiyonun adını girin. Birden çok koleksiyonları içeri aktarmak için ayrı ayrı her koleksiyon adını girin veya birden çok koleksiyon belirtmek için aşağıdaki sözdizimini kullanın: *collection_prefix*[Başlangıç dizini - bitiş dizini]. Daha önce bahsedilen söz dizimi aracılığıyla birden çok koleksiyon belirtirken, aşağıdaki yönergeleri göz önünde bulundurun:

1. Yalnızca tamsayı aralığı adı desenleri desteklenir. Örneğin, aşağıdaki koleksiyonları koleksiyonunu [0-3] belirten oluşturur: Koleksiyon0, collection1, collection2, collection3.
2. Kısaltılmış sözdizimi kullanabilirsiniz: [3] koleksiyonu 1. adımda bahsedilen koleksiyonları aynı kümesini oluşturur.
3. Birden fazla değiştirme sağlanabilir. Örneğin, baştaki sıfırlarla 20 koleksiyon adları [0-1] [0-9] koleksiyonuna oluşturur (collection01,.. 02... 03).

Koleksiyon adları belirledikten sonra istenen üretimini collection(s) (10. 000'rus için 400 RUs) seçin. En iyi alma performans için daha fazla üretilen işi seçin. Performans düzeyleri hakkında daha fazla bilgi için bkz: [Azure Cosmos veritabanı performans düzeyleri](performance-levels.md).

> [!NOTE]
> Performans verimlilik ayar yalnızca koleksiyon oluşturma için geçerlidir. Belirtilen koleksiyon zaten varsa, üretilen iş olduğu olması değiştirilemez.
> 
> 

Birden çok koleksiyonuna içe aktarırken, içeri aktarma aracını karma tabanlı parçalama destekler. Bu senaryoda, bölüm anahtarı olarak kullanmak istediğiniz belgeyi özelliği belirtin (bölüm anahtarı boş bırakılırsa, belgeleri rastgele hedef koleksiyonlarındaki parçalı).

İsteğe bağlı olarak, hangi alma kaynağı alanında (belgeleri bu özellik içermiyorsa, sonra içeri aktarma aracını kimliği özellik değeri olarak bir GUID oluşturur olduğunu unutmayın) alma sırasında Azure Cosmos DB belge kimliği özelliği olarak kullanılması gereken belirtebilir.

İçeri aktarma sırasında birkaç Gelişmiş Seçenekler yok. İlk olarak, Aracı'nı içerir ancak varsayılan toplu alma saklı yordam (BulkInsert.js), kendi depolanan içeri aktarma yordamı belirtmeyi seçebilirsiniz:

 ![Azure Cosmos DB ekran bulk INSERT sproc seçeneği](./media/import-data/bulkinsertsp.png)

Ayrıca, tarih türleri (örneğin, SQL Server veya MongoDB) alırken, üç alma seçenekler arasından seçim yapabilirsiniz:

 ![Azure Cosmos DB ekran tarih saat içeri aktarma seçenekleri](./media/import-data/datetimeoptions.png)

* Dize: bir dize değeri olarak sürdürülemedi
* Dönem: bir dönem sayı değeri kalır
* Her ikisi: hem dize hem de dönem sayı değerlerini kalıcı olmasını sağlar. Bu seçenek, örneğin bir alt oluşturur: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "dönem": 1382390245}

Azure Cosmos DB toplu içeri Aktarıcı Gelişmiş Seçenekler aşağıdaki ek sahiptir:

1. Toplu iş boyutu: Aracı varsayılan olarak bir toplu iş boyutu 50.  İçeri aktarılacak belgeleri büyük toplu iş boyutunu azaltmayı düşünün. İçeri aktarılacak belgeleri küçükse, buna karşılık, toplu iş boyutu yükseltme göz önünde bulundurun.
2. En fazla komut dosyası boyutu (bayt): araç varsayılan olarak en fazla komut dosyası boyutunu 512 KB için.
3. Otomatik kimliği oluşturma devre dışı bırak: içeri aktarılacak her belgenin bir kimlik alanı içeriyorsa, bu seçeneğin belirlenmesi performansını artırabilirsiniz. Benzersiz kimliği alanı eksik belgeleri içeri aktarılmadı.
4. Güncelleştirme mevcut belgeler: varolan belgeleri kimliği çakışmaları ile değiştirerek değil aracı varsayılanlarını. Bu seçeneğin belirlenmesi, varolan belgeleri kimlikleriyle eşleşen ile üzerine sağlar. Bu özellik, varolan belgeleri güncelleştirme zamanlanan veri geçişler için yararlıdır.
5. Hata yeniden deneme sayısı: Azure Cosmos DB bağlantısı geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme sayısını belirtir.
6. Yeniden deneme aralığı: nasıl geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda Azure Cosmos DB bağlantısı yeniden deneniyor arasında bekleneceğini belirtir.
7. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler DirectTcp, DirectHttps ve ağ geçidi ' dir. Doğrudan bağlantı modu yalnızca 443 numaralı bağlantı noktasını kullanır gibi ağ geçidi modu daha fazla güvenlik duvarı kolay olsa da, daha hızlıdır.

![Gelişmiş Seçenekleri Azure Cosmos DB ekran toplu içeri aktarma](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> İçeri aktarma aracını bağlantı modu DirectTcp varsayılan değerdir. Güvenlik Duvarı sorunlarla karşılaşırsanız, yalnızca bir bağlantı noktası 443 gerektirdiği ağ geçidi bağlantı moduna geçer.
> 
> 

## <a id="SQLSeqTarget"></a>İçeri aktarma SQL API (sıralı kayıt içe aktarma)
Azure Cosmos DB sıralı kayıt alma, kayıt kayıt temelinde kullanılabilir kaynak seçeneklerinden herhangi birini alınacak olanak sağlar. Saklı yordamlar kotasına ulaştı varolan bir koleksiyona alıyorsanız bu seçeneği belirleyebilirsiniz. Aracı, verileri birden çok tek bölümlü ve/veya birden çok bölüm Azure Cosmos DB koleksiyonlar genelinde yapabildiği bölümlenmiş parçalı alma yanı sıra tek (tek bölüm ve birden çok bölüm) Azure Cosmos DB koleksiyona Al destekler. Veri bölümlendirme hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md).

![Azure Cosmos DB ekran sıralı kayıt içeri aktarma seçenekleri](./media/import-data/documentdbsequential.png)

Azure Cosmos DB bağlantı dizesi biçimi şöyledir:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Bölümünde açıklandığı gibi Azure Portalı'nın anahtarları sayfasından Azure Cosmos DB hesabı bağlantı dizesi alınabilir [Azure Cosmos DB hesabın nasıl yönetileceği](manage-account.md), veritabanının adı için bağlantı dizesi eklenmesi gerekiyor ancak şu biçimde:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Bağlantı dizesi alanına belirtilen Azure Cosmos DB örneği erişilebildiğinden emin olmak için Doğrula komutunu kullanın.
> 
> 

Tek bir koleksiyon almak için verileri içeri aktarılır ve Ekle düğmesine basın koleksiyonun adını girin. Birden çok koleksiyonları içeri aktarmak için ayrı ayrı her koleksiyon adını girin veya birden çok koleksiyon belirtmek için aşağıdaki sözdizimini kullanın: *collection_prefix*[Başlangıç dizini - bitiş dizini]. Daha önce bahsedilen söz dizimi aracılığıyla birden çok koleksiyon belirtirken, aşağıdaki yönergeleri göz önünde bulundurun:

1. Yalnızca tamsayı aralığı adı desenleri desteklenir. Örneğin, aşağıdaki koleksiyonları koleksiyonunu [0-3] belirten oluşturur: Koleksiyon0, collection1, collection2, collection3.
2. Kısaltılmış sözdizimi kullanabilirsiniz: [3] koleksiyonu 1. adımda bahsedilen koleksiyonları aynı kümesini oluşturur.
3. Birden fazla değiştirme sağlanabilir. Örneğin, baştaki sıfırlarla [0-1] [0-9] koleksiyonuna 20 koleksiyon adları oluşturur (collection01,.. 02... 03).

Koleksiyon adları belirledikten sonra istenen üretimini collection(s) (250.000 RUs için 400 RUs) seçin. En iyi alma performans için daha fazla üretilen işi seçin. Performans düzeyleri hakkında daha fazla bilgi için bkz: [Azure Cosmos veritabanı performans düzeyleri](performance-levels.md). Tüm alma işleme sahip koleksiyonları > 10.000 RUs bölüm anahtarı gerektirir. Birden fazla 250.000 RUs tercih ediyorsanız, artan, hesabınızın olması için portalında bir istek dosya gerekir.

> [!NOTE]
> Üretilen iş ayar yalnızca koleksiyon oluşturma için geçerlidir. Belirtilen koleksiyon zaten varsa, üretilen iş değiştirilmeyecek.
> 
> 

Birden çok koleksiyonuna içe aktarırken, içeri aktarma aracını karma tabanlı parçalama destekler. Bu senaryoda, bölüm anahtarı olarak kullanmak istediğiniz belgeyi özelliği belirtin (bölüm anahtarı boş bırakılırsa, belgeleri rastgele hedef koleksiyonlarındaki parçalı).

İsteğe bağlı olarak, hangi alma kaynağı alanında (belgeleri bu özellik içermiyorsa, sonra içeri aktarma aracını kimliği özellik değeri olarak bir GUID oluşturur olduğunu unutmayın) alma sırasında Azure Cosmos DB belge kimliği özelliği olarak kullanılması gereken belirtebilir.

İçeri aktarma sırasında birkaç Gelişmiş Seçenekler yok. İlk olarak, tarih türleri (örneğin, SQL Server veya MongoDB) alırken, üç alma seçenekler arasından seçim yapabilirsiniz:

 ![Azure Cosmos DB ekran tarih saat içeri aktarma seçenekleri](./media/import-data/datetimeoptions.png)

* Dize: bir dize değeri olarak sürdürülemedi
* Dönem: bir dönem sayı değeri kalır
* Her ikisi: hem dize hem de dönem sayı değerlerini kalıcı olmasını sağlar. Bu seçenek, örneğin bir alt oluşturur: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "dönem": 1382390245}

Azure DB - Cosmos sıralı kayıt içeri Aktarıcı aşağıdaki ek gelişmiş seçenekler vardır:

1. Paralel istek sayısı: araç varsayılan olarak iki paralel istekleri. İçeri aktarılacak belgeleri küçükse, paralel istek sayısı oluşturma göz önünde bulundurun. Bu sayı çok fazla oluşursa alma azaltma karşılaşabilirsiniz olduğunu unutmayın.
2. Otomatik kimliği oluşturma devre dışı bırak: içeri aktarılacak her belgenin bir kimlik alanı içeriyorsa, bu seçeneğin belirlenmesi performansını artırabilirsiniz. Benzersiz kimliği alanı eksik belgeleri içeri aktarılmadı.
3. Güncelleştirme mevcut belgeler: varolan belgeleri kimliği çakışmaları ile değiştirerek değil aracı varsayılanlarını. Bu seçeneğin belirlenmesi, varolan belgeleri kimlikleriyle eşleşen ile üzerine sağlar. Bu özellik, varolan belgeleri güncelleştirme zamanlanan veri geçişler için yararlıdır.
4. Hata yeniden deneme sayısı: Azure Cosmos DB bağlantısı geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme sayısını belirtir.
5. Yeniden deneme aralığı: nasıl geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda Azure Cosmos DB bağlantısı yeniden deneniyor arasında bekleneceğini belirtir.
6. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler DirectTcp, DirectHttps ve ağ geçidi ' dir. Doğrudan bağlantı modu yalnızca 443 numaralı bağlantı noktasını kullanır gibi ağ geçidi modu daha fazla güvenlik duvarı kolay olsa da, daha hızlıdır.

![Gelişmiş Seçenekleri Azure Cosmos DB ekran sıralı kayıt alma](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> İçeri aktarma aracını bağlantı modu DirectTcp varsayılan değerdir. Güvenlik Duvarı sorunlarla karşılaşırsanız, yalnızca bir bağlantı noktası 443 gerektirdiği ağ geçidi bağlantı moduna geçer.
> 
> 

## <a id="IndexingPolicy"></a>Bir dizin oluşturma ilkesini belirtin
İçeri aktarma sırasında Azure Cosmos DB SQL API koleksiyonları oluşturmak geçiş aracı izin verdiğinizde, koleksiyon dizin oluşturma ilkesini belirtebilirsiniz. Gelişmiş Seçenekler bölümünde Azure Cosmos DB sıralı kayıt seçenekleri ve Azure Cosmos DB toplu içeri dizin oluşturma ilkesi bölümüne gidin.

![Ekran Azure Cosmos DB dizin Gelişmiş Seçenekler ilke](./media/import-data/indexingpolicy1.png)

Seçenek Gelişmiş dizin oluşturma İlkesi'ni kullanarak, yapabilir bir dizin oluşturma ilke dosyası seçin, el ile bir dizin oluşturma ilkesi girin veya varsayılan şablonları kümesinden (dizin oluşturma ilkesi metin kutusunda sağ tıklayarak).

Aracı sağlar ilke şablonları şunlardır:

* Varsayılan. Dizeleri eşitlik sorguları gerçekleştirme ve sıralama, aralık ve eşitlik sorguları için numaraları kullanarak bu en iyi bir ilkedir. Bu ilkeyi daha az dizin depolama yükü aralığından daha vardır.
* Aralık. Hem sayılara hem de dizeleri sıralama, aralık ve eşitlik sorguları kullanıyorsanız bu iyi bir ilkedir. Bu ilke varsayılan veya karma daha yüksek bir dizin depolama yükü var.

![Ekran Azure Cosmos DB dizin Gelişmiş Seçenekler ilke](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Bir dizin oluşturma ilkesi belirtmezseniz, varsayılan ilke uygulanır. Dizin oluşturma ilkeleri hakkında daha fazla bilgi için bkz: [Azure Cosmos ilkeleri dizin DB](indexing-policies.md).
> 
> 

## <a name="export-to-json-file"></a>JSON dosyasına dışarı aktarma
Azure Cosmos DB JSON dışarı aktarma, kullanılabilir kaynak seçeneklerinden herhangi birini bir dizi JSON belgeleri içeren bir JSON dosyası vermenize olanak sağlar. Sizin için dışarı aktarma aracı işler veya ortaya çıkan geçiş komut görüntülemek ve kendiniz komutu çalıştırmak seçebilirsiniz. Sonuçta elde edilen JSON dosyasını yerel olarak veya Azure Blob Depolama alanında depolanabilir.

![Ekran Azure Cosmos DB JSON yerel dosya dışa aktarma seçeneği](./media/import-data/jsontarget.png)

![Azure Cosmos DB JSON Azure Blob'ın ekran depolama dışa aktarma seçeneği](./media/import-data/jsontarget2.png)

İsteğe bağlı olarak daha fazla içeriği yaparken sonuç belgesi boyutunu artıracaktır elde edilen JSON prettify seçebileceği okunabilir.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma
Gelişmiş yapılandırma ekranında yazılmış hataları istediğiniz günlük dosyasının konumunu belirtin. Bu sayfa aşağıdaki kurallar geçerli olur:

1. Bir dosya adı sağlanmazsa, tüm hataları sonuçları sayfasında döndürülür.
2. Bir dosya adı olmadan bir dizin sağlanırsa, ardından dosyayı oluşturulduğunda (üzerine geçerli ortam dizinde veya).
3. Var olan belirlerseniz dosyası sonra dosyanın üzerine yazılır, ek bir seçenek yoktur.

Ardından, tüm, günlük isteyip istemediğinizi seçin kritik veya herhangi bir hata iletisi. Son olarak, ne sıklıkta karar aktarımı ileti ile ilerleme durumunu ekran güncelleştirilir.

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Ayarları içeri aktarma ve görünüm komut satırında onaylayın
1. Kaynak bilgileri, hedef bilgileri ve Gelişmiş Yapılandırma belirtme sonra geçiş özeti gözden geçirin ve isteğe bağlı olarak, görünüm/kopyalama ortaya çıkan geçiş komut (komutu kopyalama, içeri aktarma işlemlerini otomatikleştirmek yararlıdır):
   
    ![Özet ekranının ekran görüntüsü](./media/import-data/summary.png)
   
    ![Özet ekranının ekran görüntüsü](./media/import-data/summarycommand.png)
2. Kaynak ve hedef seçeneklerle memnun kaldıktan sonra tıklatın **alma**. İçeri aktarma sürecinde olduğundan geçen süre, aktarılan sayısını ve (Gelişmiş Yapılandırma bir dosya adı kaydetmedi sağlarsanız) hata bilgileri güncelleştirin. Tamamlandıktan sonra sonuçları (örneğin, tüm alma hataları ile mücadele etmek için) dışarı aktarabilirsiniz.
   
    ![Ekran Azure Cosmos DB JSON dışa aktarma seçeneği](./media/import-data/viewresults.png)
3. Yeni bir içeri aktarma, var olan ayarları (örneğin, bağlantı dizesi bilgilerini, kaynak ve hedef seçimi, vb.) tutma ya da tüm değerleri sıfırlama de başlayabilir.
   
    ![Ekran Azure Cosmos DB JSON dışa aktarma seçeneği](./media/import-data/newimport.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri yaptığınızı:

> [!div class="checklist"]
> * Veri Geçiş Aracı yüklü
> * Farklı veri kaynaklarından alınan verileri
> * JSON olarak Azure Cosmos DB dışarı

Şimdi, sonraki öğretici devam etmek ve Azure Cosmos DB kullanarak verileri sorgulamak öğrenin. 

> [!div class="nextstepaction"]
>[Sorgu verileri nasıl?](../cosmos-db/tutorial-query-sql-api.md)
