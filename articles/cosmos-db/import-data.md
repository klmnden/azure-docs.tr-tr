---
title: Azure Cosmos DB için veritabanı geçiş aracı
description: MongoDB, SQL Server, Tablo depolama, Amazon DynamoDB, CSV ve JSON dosyaları da dahil olmak üzere çeşitli kaynaklardan Azure Cosmos DB’ye verileri geçirmek için açık kaynak Azure Cosmos DB veri geçişi araçlarının nasıl kullanılacağını öğrenin. CSV’yi JSON’a dönüştürme.
author: deborahc
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 02/22/2019
ms.author: dech
ms.openlocfilehash: 023b344d796ea5297cda202e7baa2f0e0ef5eebd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61058375"
---
# <a name="use-data-migration-tool-to-migrate-your-data-to-azure-cosmos-db"></a>Verilerinizi Azure Cosmos DB'ye geçirmek için Veri geçişi aracını kullanma

Bu öğreticide, çeşitli kaynaklardan Azure Cosmos DB koleksiyonlarına ve tablolarına verileri içeri aktarabilen Azure Cosmos DB Veri Geçişi aracının kullanımına ilişkin yönergeler sağlanmaktadır. JSON dosyaları, CSV dosyaları, SQL, MongoDB, Azure tablo depolama, Amazon DynamoDB ve hatta Azure Cosmos DB SQL API koleksiyonları içeri aktarabilirsiniz. Koleksiyonlar ve tabloları Azure Cosmos DB ile kullanmak için bu verileri geçirin. Veri Geçişi aracı, tek bir bölüm koleksiyonundan SQL API’si için çok bölümlü bir koleksiyona geçirme işlemi sırasında da kullanılabilir.

Azure Cosmos DB ile hangi API’yi kullanacaksınız?

* **[SQL API’si](documentdb-introduction.md)** : Verileri içeri aktarmak için Veri Geçişi aracında sağlanan kaynak seçeneklerinin herhangi birini kullanabilirsiniz.
* **[Tablo API’si](table-introduction.md)** : Verileri içeri aktarmak için Veri Geçişi aracını veya AzCopy’yi kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Cosmos DB Tablo API’si ile kullanılmak üzere verileri içeri aktarma](table-import.md).
* **[Azure Cosmos DB'nin MongoDB API'si](mongodb-introduction.md)**  -veri geçiş aracı şu anda desteklemiyor Azure Cosmos DB'nin API'sini MongoDB için bir kaynak veya hedef olarak. İçine veya dışına Azure Cosmos DB koleksiyonları veri geçirmek istiyorsanız, başvurmak [MongoDB verilerini Cosmos veritabanı Azure Cosmos DB API'si ile MongoDB için geçirme](mongodb-migrate.md) yönergeler için. SQL API’si ile kullanılmak üzere MongoDB’den Azure Cosmos DB SQL API koleksiyonlarına verileri dışarı aktarmak için Veri Geçişi aracını kullanmaya devam edebilirsiniz.
* **[Gremlin API](graph-introduction.md)**  -veri geçiş aracı şu anda bir Gremlin API hesapları için desteklenen içeri aktarma aracı değildir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Veri Geçişi aracını yükleme
> * Farklı veri kaynaklarından verileri içeri aktarma
> * Azure Cosmos DB’den JSON’a dışarı aktarma

## <a id="Prerequisites"></a>Önkoşullar

Bu makaledeki yönergeleri izlemeden önce aşağıdakileri yaptığınızdan emin olun:

* **Yükleme** [Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) veya üzeri.

* **Aktarım hızını artırın:** Veri geçiş süresi, aktarım hızı, tek bir koleksiyon için ayarlama miktarı veya koleksiyonları kümesi göre değişir. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Azure portalında aktarım hızını artırma hakkında daha fazla bilgi için bkz. [performans düzeyleri](performance-levels.md) ve [fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/cosmos-db/) Azure Cosmos DB'de.

* **Azure Cosmos DB kaynaklarını oluşturan:** Veri geçişi başlatmadan önce tüm koleksiyonlarınız Azure portalından önceden oluşturun. Veritabanı düzeyinde aktarım hızına sahip bir Azure Cosmos DB hesabına geçirmek için Azure Cosmos DB koleksiyonları oluştururken bir bölüm anahtarı sağlayın.

## <a id="Overviewl"></a>Genel Bakış

Veri Geçişi aracı, aşağıda örnekleri verilen çeşitli kaynaklardan Azure Cosmos DB’ye veri içeri aktaran bir açık kaynak çözümüdür:

* JSON dosyaları
* MongoDB
* SQL Server
* CSV dosyaları
* Azure Tablo depolama
* Amazon DynamoDB
* HBase
* Azure Cosmos DB koleksiyonları

İçeri aktarma aracı bir grafiksel kullanıcı arabirimi (dtui.exe) içerse de, komut satırından (dt.exe) da tetiklenebilir. Aslında, bir içeri aktarma kullanıcı Arabirimi aracılığıyla kurduktan sonra ilişkili komut çıktısı için bir seçenek yoktur. İçeri aktarma sırasında hiyerarşik ilişkileri (alt) oluşturmak için SQL Server veya CSV dosyaları gibi tablo kaynak verileri dönüştürebilirsiniz. Kaynak seçenekleri, her bir kaynaktan içeri aktarma için örnek komutlar, hedef seçenekler ve içeri aktarma sonuçlarını görüntüleme hakkında daha fazla bilgi edinmek için devamını okuyun.

## <a id="Install"></a>Yükleme

Geçiş aracı kaynak koduna GitHub’da [bu depodan](https://github.com/azure/azure-documentdb-datamigrationtool) erişilebilir. Çözümü yerel olarak indirip derleyebilir veya [önceden derlenmiş bir ikiliyi indirip](https://aka.ms/csdmtool) daha sonra aşağıdakilerden birini çalıştırabilirsiniz:

* **Dtui.exe**: Aracı'nın grafik arabirimi sürümü
* **Dt.exe**: Komut satırı aracı sürümü

## <a name="select-data-source"></a>Veri kaynağı seçme

Aracı yükledikten sonra verilerinizin içeri aktarılmasına sıra gelir. Ne tür verileri içeri aktarmak istiyorsunuz?

* [JSON dosyaları](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB Dışarı aktarma dosyaları](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV dosyaları](#CSV)
* [Azure Tablo Depolama](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [Blob](#BlobImport)
* [Azure Cosmos DB koleksiyonları](#SQLSource)
* [HBase](#HBaseSource)
* [Azure Cosmos DB toplu içeri aktarma](#SQLBulkTarget)
* [Azure Cosmos DB sıralı kayıt içeri aktarma](#SQLSeqTarget)

## <a id="JSON"></a>JSON dosyalarını içeri aktarma

JSON dosyası kaynak alma seçeneği alma bir veya daha fazla tek belge JSON dosyaları veya her bir dizi JSON belgeleri olan JSON dosyaları sağlar. JSON dosyalarını içeri aktarmak için klasör eklerken, yinelemeli olarak alt klasörlerdeki dosyaları aranıyor seçeneğiniz vardır.

![JSON dosya kaynağı seçeneklerinin ekran görüntüsü: Veritabanı geçişi araçları](./media/import-data/jsonsource.png)

Aşağıda, JSON dosyalarını içeri aktarmak için bazı komut satırı örnekleri verilmiştir:

```console
#Import a single JSON file
dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

#Import a directory of JSON files
dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

#Import a directory (including sub-directories) of JSON files
dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

#Import a directory (single), directory (recursive), and individual JSON files
dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

#Import a single JSON file and partition the data across 4 collections
dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500
```

## <a id="MongoDB"></a>MongoDB’den içeri aktarma

> [!IMPORTANT]
> MongoDB için Azure Cosmos DB API'si ile yapılandırılan bir Cosmos hesabına içe aktarıyorsanız, aşağıdaki adımları [yönergeleri](mongodb-migrate.md).

MongoDB kaynak alma seçeneği ile tek bir MongoDB koleksiyonundan içeri aktarma, isteğe bağlı olarak bir sorgu kullanarak belgeleri filtreler ve belge yapısı bir yansıtma kullanarak değiştirin.  

![MongoDB kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/mongodbsource.png)

Bağlantı dizesi, standart MongoDB biçimindedir:

`mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen MongoDB örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

İçinden verilerin içeri aktarılacağı koleksiyonun adını girin. İsteğe bağlı olarak belirttiğinizde veya bir sorgu için bir dosya sağlayın `{pop: {$gt:5000}}`, ya da projeksiyon gibi `{loc:0}`, hem filtrelemeyi ve aldığınız veri şekli.

Aşağıda, MongoDB’den içeri aktarılacak bazı komut satırı örnekleri verilmiştir:

```console
#Import all documents from a MongoDB collection
dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

#Import documents from a MongoDB collection which match the query and exclude the loc field
dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500
```

## <a id="MongoDBExport"></a>MongoDB dışarı aktarma dosyalarını içeri aktarma

> [!IMPORTANT]
> MongoDB desteği ile bir Azure Cosmos DB hesabına içe aktarıyorsanız, aşağıdaki adımları [yönergeleri](mongodb-migrate.md).

MongoDB dışarı aktarma JSON dosyası kaynak içeri aktarıcı seçeneği, mongoexport yardımcı programından üretilen bir veya daha fazla JSON dosyasını içeri aktarmanıza olanak sağlar.  

![MongoDB dışarı aktarma kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/mongodbexportsource.png)

MongoDB dışarı aktarma JSON dosyaları içeri aktarma için klasör eklerken, yinelemeli olarak alt klasörlerdeki dosyaları aranıyor seçeneğiniz vardır.

Aşağıda, MongoDB dışarı aktarma JSON dosyalarından içeri aktarma işlemi yapmak için bir komut satırı örneği verilmiştir:

```console
dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500
```

## <a id="SQL"></a>SQL Server’dan içeri aktarma

SQL kaynak içeri aktarıcı seçeneği, tek bir SQL Server veritabanından içeri aktarma işlemi yapmanıza ve isteğe bağlı şekilde bir sorgu kullanarak içeri aktarılacak kayıtları filtrelemenize olanak sağlar. Ayrıca bir iç içe geçirme ayırıcısı belirterek de belge yapısını değiştirebilirsiniz (kısa bir süre sonra bununla ilgili daha fazla bilgi verilecektir).  

![SQL kaynak seçeneklerinin ekran görüntüsü: veritabanı geçişi araçları](./media/import-data/sqlexportsource.png)

Bağlantı dizesinin biçimi, standart SQL bağlantı dizesi biçimindedir.

> [!NOTE]
> Bağlantı dizesi alanında belirtilen SQL Server örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

İçeri aktarma sırasında hiyerarşik ilişkiler (alt belgeler) oluşturmak için iç içe geçirme ayırıcısı özelliği kullanılır. Aşağıdaki SQL sorgusunu dikkate alın:

`select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'`

Aşağıdaki (kısmi) sonuçları döndürür:

![SQL sorgusu sonuçlarının ekran görüntüsü](./media/import-data/sqlqueryresults.png)

Address.AddressType ve Address.Location.StateProvinceName gibi diğer adlara dikkat edin. İç içe geçirme ayırıcısı olarak '.' öğesi belirtildiğinde içeri aktarma aracı içeri aktarma sırasında Address ve Address.Location alt belgelerini oluşturur. Aşağıda, Azure Cosmos DB’de elde edilen bir belge örneği verilmiştir:

*{"id": "956", "Name": "Daha hassas satış ve hizmet", "Adres": {"AddressType": "Ana ofis", "Adressatırı1": "#500-75 O'Connor Sokak", "Konum": {"City": "Ottawa", "StateProvinceName": "Ontario"} "PostalCode": "K4B 1S2", "CountryRegionName": "Kanada"}}*

Aşağıda, SQL Server’dan içeri aktarılacak bazı komut satırı örnekleri verilmiştir:

```console
#Import records from SQL which match a query
dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

#Import records from sql which match a query and create hierarchical relationships
dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500
```

## <a id="CSV"></a>CSV dosyalarını içeri aktarma ve CSV’yi JSON’a dönüştürme

CSV dosyası kaynak içeri aktarıcı seçeneği, bir veya daha fazla CSV dosyasını içeri aktarmanıza olanak sağlar. CSV dosyaları içeri aktarma için klasör eklerken, yinelemeli olarak alt klasörlerdeki dosyaları aranıyor seçeneğiniz vardır.

![CSV kaynak seçeneklerinin ekran görüntüsü: CSV’den JSON’a](media/import-data/csvsource.png)

SQL kaynağına benzer şekilde, içeri aktarma sırasında hiyerarşik ilişkiler (alt belgeler) oluşturmak için iç içe geçirme ayırıcısı özelliği kullanılabilir. Aşağıdaki CSV üst bilgisi satırını ve veri satırlarını dikkate alın:

![CSV örnek kayıtlarının ekran görüntüsü: CSV’den JSON’a](./media/import-data/csvsample.png)

DomainInfo.Domain_Name ve RedirectInfo.Redirecting gibi diğer adlara dikkat edin. İç içe geçirme ayırıcısı olarak '.' öğesi belirtildiğinde içeri aktarma aracı içeri aktarma sırasında DomainInfo ve RedirectInfo alt belgelerini oluşturur. Aşağıda, Azure Cosmos DB’de elde edilen bir belge örneği verilmiştir:

*{"DomainInfo": {"Etki_alanı_adı": "ACUS.GOV", "Domain_Name_Address": "https:\//www.ACUS.GOV"}, "Federal kurum": "Amerika Birleşik Devletleri yönetim konferansı", "RedirectInfo": {"Yönlendirme": "0", "Redirect_Destination": ""}, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*

İçeri aktarma Aracı (tırnak içine alınmış değerler her zaman dize olarak kabul edilir), tırnak işareti olmayan değerleri CSV dosyaları için tür bilgilerini çıkarmasına dener.  Türler şu sırayla tanımlanır: sayı, tarih saat, boole.  

CSV içeri aktarımı ile ilgili dikkat edilecek iki şey daha vardır:

1. Varsayılan olarak, tırnak içine alınmayan değerler her zaman sekmeler ve boşluklar için kırpılırken, tırnak içine alınan değerler olduğu gibi korunur. Tırnak içine alınan değerleri kırp onay kutusu veya /s.TrimQuoted komut satırı seçeneği ile bu davranış geçersiz kılınabilir.
2. Varsayılan olarak, tırnak içine alınmayan null değer, null değer olarak değerlendirilir. Tırnak içine alınmayan NULL değeri dize olarak değerlendir onay kutusu veya /s.NoUnquotedNulls komut satırı seçeneği ile bu davranış geçersiz kılınabilir (başka bir deyişle, tırnak içine alınmayan null değer, "null" dize olarak değerlendirilir).

Aşağıda, CSV içeri aktarımı için bir komut satırı örneği verilmiştir:

```console
dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500
```

## <a id="AzureTableSource"></a>Azure Tablo depolamadan içeri aktarma

Azure Tablo depolama kaynak içeri aktarıcı seçeneği, tek bir Azure Tablo depolama tablosundan içeri aktarma işlemi yapmanıza olanak sağlar. İsteğe bağlı olarak, içeri aktarılacak tablo varlıklarını filtreleyebilirsiniz.

Azure tablo Depolama'dan Azure Cosmos DB tabloları ve varlıkları tablo API'si ile kullanmak için içeri aktarılan veri çıkış. İçeri aktarılan veriler ayrıca koleksiyonları ve belgeleri SQL API ile kullanılmak için çıkış olabilir. Ancak, tablo API'si yalnızca komut satırı yardımcı programı hedef olarak kullanılabilir. Veri Geçiş Aracı kullanıcı arabirimini kullanarak tablo API'sine dışarı aktaramazsınız. Daha fazla bilgi için bkz. [Azure Cosmos DB Tablo API’si ile kullanılmak üzere verileri içeri aktarma](table-import.md).

![Azure Tablo depolama kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/azuretablesource.png)

Azure Tablo depolama bağlantı dizesinin biçimi şöyledir:

`DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen Azure Tablo depolama örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

İçinden içeri aktarma yapılacak Azure tablosunun adını girin. İsteğe bağlı olarak bir [filtre](../vs-azure-tools-table-designer-construct-filter-strings.md) belirtebilirsiniz.

Azure Tablo depolama kaynağı içeri aktarıcı seçeneği aşağıdaki ek seçenekleri içerir:

1. İç Alanları Dahil Et
   1. Tümü: Tüm iç alanları (PartitionKey, RowKey ve Timestamp) dahil et
   2. Hiçbiri: Tüm iç alanları dışla
   3. RowKey: Yalnızca RowKey alanını dahil et
2. Sütun Seç
   1. Azure tablo depolama filtreleri projeksiyonlar desteklemez. Yalnızca belirli Azure Tablo varlığı özelliklerini içeri aktarmak istiyorsanız bunları Sütun Seç listesine ekleyin. Diğer tüm varlık özellikleri yoksayılır.

Aşağıda, Azure Tablo depolamadan içeri aktarılacak bir komut satırı örneği verilmiştir:

```console
dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500
```

## <a id="DynamoDBSource"></a>Amazon DynamoDB’den içeri aktarma

Amazon DynamoDB kaynak alma seçeneği, tek bir Amazon DynamoDB tablodan içeri aktarmanıza olanak sağlar. İsteğe bağlı olarak içeri aktarılacak varlıkları filtre uygulayabilirsiniz. İçeri aktarımın mümkün olduğunca kolay şekilde ayarlanması için birçok şablon sağlanır.

![Amazon DynamoDB kaynağı seçeneklerinin ekran görüntüsü: veritabanı geçişi araçları](./media/import-data/dynamodbsource1.png)

![Amazon DynamoDB kaynağı seçeneklerinin ekran görüntüsü: veritabanı geçişi araçları](./media/import-data/dynamodbsource2.png)

Amazon DynamoDB bağlantı dizesinin biçimi şöyledir:

`ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen Amazon DynamoDB örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

Aşağıda, Amazon DynamoDB’den içeri aktarım için bir komut satırı örneği verilmiştir:

```console
dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500
```

## <a id="BlobImport"></a>Azure Blob depolama alanından içeri aktarma

JSON dosyası, MongoDB dışarı aktarma dosyası ve CSV dosyası kaynak içeri aktarıcı seçenekleri, Azure Blob depolama alanından bir veya daha fazla dosyayı içeri aktarmanıza olanak sağlar. Bir Blob kapsayıcı URL’si ve Hesap Anahtarı belirttikten sonra, içeri aktarılacak dosyaları seçmek için bir normal ifade sağlayın.

![Blob dosya kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/blobsource.png)

Aşağıda, Azure Blob depolama alanından JSON dosyalarını içeri aktarmak için komut satırı örneği verilmiştir:

```console
dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest
```

## <a id="SQLSource"></a>SQL API koleksiyonundan içeri aktarma

Azure Cosmos DB kaynağı içeri aktarıcı seçeneği, bir veya daha fazla Azure Cosmos DB koleksiyonundan verileri içeri aktarmanıza ve isteğe bağlı şekilde bir sorgu kullanarak belgeleri filtrelemenize olanak sağlar.  

![Azure Cosmos DB kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/documentdbsource.png)

Azure Cosmos DB bağlantı dizesinin biçimi şöyledir:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

Azure Cosmos DB hesabı bağlantı dizesi açıklandığı gibi Azure portalının anahtarlar sayfasından alabilirsiniz [bir Azure Cosmos DB hesabını yönetme](manage-account.md). Ancak, veritabanı adını şu biçimde bağlantı dizesine eklenmesi gerekir:

`Database=<CosmosDB Database>;`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen Azure Cosmos DB örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

Tek bir Azure Cosmos DB koleksiyonundan içeri aktarmak için, içinden verilerin içeri aktarılacağı koleksiyonun adını girin. Birden fazla Azure Cosmos DB koleksiyonundan içeri aktarmak için bir veya daha fazla koleksiyon adlarını eşleştirmek için normal bir ifade girin (örneğin, collection01 | collection02 | collection03). İsteğe bağlı olarak belirtin veya bir sorgu hem filter hem de aldığınız veri şekli için için bir dosya sağlayın.

> [!NOTE]
> Normal ifade karakterleri adı olan tek bir koleksiyondan alıyorsanız normal ifadeler, koleksiyon alanın kabul olduğundan, bu karakterler uygun şekilde kaçınılmalıdır.

Azure Cosmos DB kaynağı içeri aktarıcı seçeneği aşağıdaki gelişmiş seçenekleri içerir:

1. İç alanları şunlardır: Azure Cosmos DB belge Sistem özellikleri içerir (örneğin, _rid, _ts) verme gerekip gerekmediğini belirtir.
2. Hata durumunda yeniden deneme sayısı: Azure Cosmos DB bağlantısını geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme sayısını belirtir.
3. Yeniden deneme aralığı: Azure Cosmos DB bağlantısını geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme arasında beklenecek süreyi belirtir.
4. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler, DirectTcp, DirectHttps ve Gateway seçenekleridir. Doğrudan bağlantı modları daha hızlıyken, ağ geçidi modu ise yalnızca 443 numaralı bağlantı noktasını kullandığından güvenlik duvarıyla daha kolay kullanılabilir.

![Azure Cosmos DB kaynağı gelişmiş seçeneklerinin ekran görüntüsü](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> İçeri aktarma aracı, varsayılan olarak DirectTcp bağlantı moduna geçer. Güvenlik duvarıyla ilgili sorun yaşarsanız, Ağ Geçidi bağlantı moduna geçin; bu yalnızca 443 numaralı bağlantı noktasını gerektirir.

Aşağıda, Azure Cosmos DB’den içeri aktarılacak bazı komut satırı örnekleri verilmiştir:

```console
#Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

#Migrate data from more than one Azure Cosmos DB collection to a single Azure Cosmos DB collection
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

#Export an Azure Cosmos DB collection to a JSON file
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500
```

> [!TIP]
> Azure Cosmos DB Veri İçeri Aktarma Aracı, [Azure Cosmos DB Öykünücüsü](local-emulator.md)’nden verilerin içeri aktarımını da destekler. Yerel bir öykünücüden verilerini içeri aktarırken uç noktayı `https://localhost:<port>` olarak ayarlayın.

## <a id="HBaseSource"></a>HBase’den içeri aktarma

HBase kaynak içeri aktarıcı seçeneği, bir HBase tablosundan verileri içeri aktarmanıza ve isteğe bağlı olarak verileri filtrelemenize olanak sağlar. İçeri aktarımın mümkün olduğunca kolay şekilde ayarlanması için birçok şablon sağlanır.

![HBase kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/hbasesource1.png)

![HBase kaynağı seçeneklerinin ekran görüntüsü](./media/import-data/hbasesource2.png)

HBase Stargate bağlantı dizesinin biçimi şöyledir:

`ServiceURL=<server-address>;Username=<username>;Password=<password>`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen HBase örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

Aşağıda, HBase’den içeri aktarım için bir komut satırı örneği verilmiştir:

```console
dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport
```

## <a id="SQLBulkTarget"></a>SQL API’sine içeri aktarma (Toplu İçeri Aktarma)

Azure Cosmos DB Toplu içeri aktarıcı, verimlilik için bir Azure Cosmos DB saklı yordamını kullanarak kullanılabilir kaynak seçeneklerden herhangi birinden içeri aktarma işlemi yapmanıza olanak sağlar. Tek bölümlenmiş bir Azure Cosmos DB koleksiyonu için içeri aktarma Aracı'nı destekler. Ayrıca, veri üzerinde birden fazla tek bölümlenmiş Azure Cosmos DB koleksiyonu ile bölümlenmiş parçalı alma destekler. Verileri bölümleme hakkında daha fazla bilgi için bkz. [Azure Cosmos DB'de bölümleme ve ölçeklendirme](partition-data.md). Araç, saklı yordam oluşturur, yürütür ve sonra hedef koleksiyonlardan saklı yordamı siler.  

![Azure Cosmos DB toplu seçeneklerinin ekran görüntüsü](./media/import-data/documentdbbulk.png)

Azure Cosmos DB bağlantı dizesinin biçimi şöyledir:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

[Azure Cosmos DB hesabını yönetme](manage-account.md) bölümünde açıklandığı gibi, Azure portalının Anahtarlar sayfasından Azure Cosmos DB hesabı bağlantı dizesi alınabilse de, bağlantı dizesinin sonuna veritabanı adının şu biçimde eklenmesi gerekir:

`Database=<CosmosDB Database>;`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen Azure Cosmos DB örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

Tek bir koleksiyona içeri aktarmak için, içinden verilerin içeri aktarılacağı koleksiyonun adını girin ve Ekle düğmesine tıklayın. Birden fazla koleksiyona içeri aktarmak üzere ayrı ayrı her bir koleksiyon adı girin veya birden fazla koleksiyon belirtmek için aşağıdaki sözdizimini kullanın: *collection_prefix*[Başlangıç dizini - bitiş dizini]. Yukarıda sözü edilen söz dizimi kullanılarak birden fazla koleksiyon belirtirken, aşağıdaki yönergeleri göz önünde bulundurun:

1. Yalnızca tamsayı aralığı adı desenleri desteklenir. Örneğin, collection[0-3] belirtildiğinde şu koleksiyonlar oluşturulur: collection0, collection1, collection2, collection3.
2. Kısaltılmış bir sözdizimi kullanabilirsiniz: collection[3], 1. adımda belirtilen aynı koleksiyon kümesini oluşturur.
3. Birden fazla değiştirme sağlanabilir. Örneğin, collection[0-1] [0-9], başında sıfır olan 20 koleksiyon adı oluşturur (collection01, ..02, ..03).

Koleksiyon adları belirtildikten sonra, koleksiyonların istediğiniz aktarım hızını seçin (400 RU - 10.000 RU). En iyi içeri aktarma performansı için daha yüksek bir aktarım hızı seçin. Performans düzeyleri hakkında daha fazla bilgi için bkz. [Azure Cosmos DB’de performans düzeyleri](performance-levels.md).

> [!NOTE]
> Performans aktarım hızı ayarı yalnızca koleksiyon oluşturma için geçerlidir. Belirtilen koleksiyon zaten varsa, aktarım hızı değişiklik olmaz.

Birden fazla koleksiyona içeri aktardığınızda, içeri aktarma aracı karma tabanlı parçalama destekler. Bu senaryoda, bölüm anahtarı olarak kullanmak istediğiniz belge özelliği belirtin. (Bölüm anahtarı boş bırakılırsa rastgele hedef koleksiyonlarındaki parçalı belgelerdir.)

İsteğe bağlı olarak, içeri aktarma sırasında Azure Cosmos DB belge kimliği özelliği olarak içeri aktarma kaynaktaki hangi alanın kullanılması gerektiğini belirtebilirsiniz. Bu özellik belgeleri yoksa, içeri aktarma aracı kimliği özellik değeri bir GUID oluşturur.

İçeri aktarma sırasında kullanılabilen birçok gelişmiş seçenek vardır. İlk olarak, araç varsayılan bir toplu içeri aktarma saklı yordamı (BulkInsert.js) içerse de, kendi içeri aktarma saklı yordamınızı belirtmeyi seçebilirsiniz:

 ![Azure Cosmos DB toplu ekleme sproc seçeneğinin ekran görüntüsü](./media/import-data/bulkinsertsp.png)

Ayrıca veri türlerini içeri aktarırken (örneğin, SQL Server veya MongoDB’den) üç içeri aktarma seçeneği arasından seçim yapabilirsiniz:

 ![Azure Cosmos DB tarih saat içeri aktarma seçeneklerinin ekran görüntüsü](./media/import-data/datetimeoptions.png)

* Dize: Dize değeri olarak Sürdür
* Epoch: Kalıcı bir dönem sayı değeri
* Her ikisi: Hem dize hem de sayı değerlerini dönem kalıcı hale getirin. Bu seçenek, örneğin bir alt oluşturur: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

Azure Cosmos DB Toplu içeri aktarıcı aşağıdaki ek gelişmiş seçenekleri içerir:

1. Toplu iş boyutu: Araç, varsayılan olarak 50 için bir toplu iş boyutu.  İçeri aktarılacak belgeler büyükse, toplu iş boyutunu küçültün. Aksine, içeri aktarılacak belgeler küçükse, toplu iş boyutunu büyütün.
2. En fazla betik boyutu (bayt): Aracı varsayılan en fazla betik boyutu 512 KB olarak.
3. Otomatik kimliği üretimini devre dışı bırakın: İçeri aktarılacak her belgenin Kimliği alanı varsa, ardından bu seçeneğin belirlenmesi, performansı artırabilirsiniz. Benzersiz bir kimliği alanı eksik belgeleri içe aktarılmaz.
4. Varolan belgeleri güncelleştirin: Varolan belgeleri Kimliğiyle çelişiyor değiştirerek değil aracı varsayılanlarını. Bu seçeneğin belirlenmesi, kimlikleriyle eşleşen ile varolan belgeleri üzerine yazmasını sağlar. Bu özellik, mevcut belgeleri güncelleştiren zamanlanmış veri geçişleri için yararlıdır.
5. Hata durumunda yeniden deneme sayısı: Genellikle geçici hatalar (örneğin, ağ bağlantı kesintisi) sırasında Azure Cosmos DB bağlantı yeniden deneme işlemleri belirtir.
6. Yeniden deneme aralığı: Azure Cosmos DB bağlantısını geçici hataları (örneğin, ağ bağlantı kesintisi) durumunda yeniden deneme arasında beklenecek süreyi belirtir.
7. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler, DirectTcp, DirectHttps ve Gateway seçenekleridir. Doğrudan bağlantı modları daha hızlıyken, ağ geçidi modu ise yalnızca 443 numaralı bağlantı noktasını kullandığından güvenlik duvarıyla daha kolay kullanılabilir.

![Azure Cosmos DB toplu içeri aktarma gelişmiş seçeneklerinin ekran görüntüsü](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> İçeri aktarma aracı, varsayılan olarak DirectTcp bağlantı moduna geçer. Güvenlik duvarıyla ilgili sorun yaşarsanız, Ağ Geçidi bağlantı moduna geçin; bu yalnızca 443 numaralı bağlantı noktasını gerektirir.

## <a id="SQLSeqTarget"></a>SQL API’sine içeri aktarma (Sıralı Kayıt İçeri Aktarma)

Azure Cosmos DB sıralı kaydı alma kayıt kayıt temelinde bir kullanılabilir kaynak seçeneğinden almanızı sağlar. Saklı yordam kotasına ulaşmış olan mevcut bir koleksiyona içeri aktarma işlemi yapıyorsanız bu seçeneği belirleyebilirsiniz. (Tek bölümlü ve birden çok bölüm) tek bir Azure Cosmos DB koleksiyonu için içeri aktarma Aracı'nı destekler. Ayrıca, verileri birden fazla tek bölümlü veya birden çok bölüm Azure Cosmos DB koleksiyonunda yapabildiği bölümlenmiş parçalı alma destekler. Verileri bölümleme hakkında daha fazla bilgi için bkz. [Azure Cosmos DB'de bölümleme ve ölçeklendirme](partition-data.md).

![Azure Cosmos DB sıralı kayıt içeri aktarma seçeneklerinin ekran görüntüsü](./media/import-data/documentdbsequential.png)

Azure Cosmos DB bağlantı dizesinin biçimi şöyledir:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

Azure Cosmos DB hesabı için bağlantı dizesini açıklandığı gibi Azure portalının anahtarlar sayfasından alabilirsiniz [bir Azure Cosmos DB hesabını yönetme](manage-account.md). Ancak, veritabanı adını şu biçimde bağlantı dizesine eklenmesi gerekir:

`Database=<Azure Cosmos DB Database>;`

> [!NOTE]
> Bağlantı dizesi alanında belirtilen Azure Cosmos DB örneğinin erişilebilir olduğundan emin olmak için Doğrula komutunu kullanın.

Tek bir koleksiyonu içeri aktarmak için verileri içine aktarmak için koleksiyon adı girin ve ardından Ekle düğmesine tıklayın. Birden fazla koleksiyona içeri aktarmak üzere ayrı ayrı her bir koleksiyon adı girin. Birden fazla koleksiyon belirtmek için aşağıdaki söz dizimini kullanabilirsiniz: *collection_prefix*[Başlangıç dizini - bitiş dizini]. Yukarıda sözü edilen söz dizimi aracılığıyla birden fazla koleksiyon belirtirken, aşağıdaki yönergeleri göz önünde bulundurun:

1. Yalnızca tamsayı aralığı adı desenleri desteklenir. Örneğin, collection[0-3] belirtildiğinde şu koleksiyonlar oluşturulur: collection0, collection1, collection2, collection3.
2. Kısaltılmış bir sözdizimi kullanabilirsiniz: collection[3], 1. adımda belirtilen aynı koleksiyon kümesini oluşturur.
3. Birden fazla değiştirme sağlanabilir. Örneğin, collection[0-1] [0-9], başında sıfır olan 20 koleksiyon adı oluşturur (collection01, ..02, ..03).

Koleksiyon adları belirtildikten sonra, koleksiyonların istediğiniz aktarım hızını seçin (400 RU - 250.000 RU). En iyi içeri aktarma performansı için daha yüksek bir aktarım hızı seçin. Performans düzeyleri hakkında daha fazla bilgi için bkz. [Azure Cosmos DB’de performans düzeyleri](performance-levels.md). Aktarım hızı 10.000 RU’da büyük olan koleksiyonlara içeri aktarma işlemi için bölüm anahtarı gerekir. 250.000’den fazla RU’nuz olmasını seçerseniz, hesabınızın artırılması için portalda bir istekte bulunmanız gerekir.

> [!NOTE]
> Aktarım hızı ayarı yalnızca koleksiyon veya veritabanı oluşturma için geçerlidir. Belirtilen koleksiyon zaten varsa, aktarım hızı değişiklik olmaz.

Birden fazla koleksiyona içeri aktarırken, içeri aktarma aracı karma tabanlı parçalama destekler. Bu senaryoda, bölüm anahtarı olarak kullanmak istediğiniz belge özelliği belirtin. (Bölüm anahtarı boş bırakılırsa rastgele hedef koleksiyonlarındaki parçalı belgelerdir.)

İsteğe bağlı olarak, içeri aktarma sırasında Azure Cosmos DB belge kimliği özelliği olarak içeri aktarma kaynaktaki hangi alanın kullanılması gerektiğini belirtebilirsiniz. (Belgeler, bu özelliği yoksa, daha sonra içeri aktarma aracı bir GUID kimliği özellik değeri oluşturur.)

İçeri aktarma sırasında kullanılabilen birçok gelişmiş seçenek vardır. İlk olarak, veri türlerini içeri aktarırken (örneğin, SQL Server veya MongoDB’den) üç içeri aktarma seçeneği arasından seçim yapabilirsiniz:

 ![Azure Cosmos DB tarih saat içeri aktarma seçeneklerinin ekran görüntüsü](./media/import-data/datetimeoptions.png)

* Dize: Dize değeri olarak Sürdür
* Epoch: Kalıcı bir dönem sayı değeri
* Her ikisi: Hem dize hem de sayı değerlerini dönem kalıcı hale getirin. Bu seçenek, örneğin bir alt oluşturur: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

Azure Cosmos DB: Sıralı kayıt içeri aktarıcı aşağıdaki ek gelişmiş seçenekleri içerir:

1. Paralel istekler sayısı: Aracın iki paralel istekler için varsayılan olarak ayarlanır. İçeri aktarılacak belgeler küçükse, paralel istek sayısını yükseltin. Bu sayı çok fazla yükseltilirse içeri aktarma sırasında hız sınırlama oluşabilir.
2. Otomatik kimliği üretimini devre dışı bırakın: İçeri aktarılacak her belgenin Kimliği alanı varsa, ardından bu seçeneğin belirlenmesi, performansı artırabilirsiniz. Benzersiz bir kimliği alanı eksik belgeleri içe aktarılmaz.
3. Varolan belgeleri güncelleştirin: Varolan belgeleri Kimliğiyle çelişiyor değiştirerek değil aracı varsayılanlarını. Bu seçeneğin belirlenmesi, kimlikleriyle eşleşen ile varolan belgeleri üzerine yazmasını sağlar. Bu özellik, mevcut belgeleri güncelleştiren zamanlanmış veri geçişleri için yararlıdır.
4. Hata durumunda yeniden deneme sayısı: Genellikle geçici hatalar (örneğin, ağ bağlantı kesintisi) sırasında Azure Cosmos DB bağlantı yeniden deneme işlemleri belirtir.
5. Yeniden deneme aralığı: Azure Cosmos DB bağlantısını geçici hatalar (örneğin, ağ bağlantı kesintisi) sırasında yeniden deneniyor arasında beklenecek süreyi belirtir.
6. Bağlantı modu: Azure Cosmos DB ile kullanmak için bağlantı modunu belirtir. Kullanılabilir seçenekler, DirectTcp, DirectHttps ve Gateway seçenekleridir. Doğrudan bağlantı modları daha hızlıyken, ağ geçidi modu ise yalnızca 443 numaralı bağlantı noktasını kullandığından güvenlik duvarıyla daha kolay kullanılabilir.

![Azure Cosmos DB sıralı kayıt içeri aktarma gelişmiş seçeneklerinin ekran görüntüsü](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> İçeri aktarma aracı, varsayılan olarak DirectTcp bağlantı moduna geçer. Güvenlik duvarıyla ilgili sorun yaşarsanız, Ağ Geçidi bağlantı moduna geçin; bu yalnızca 443 numaralı bağlantı noktasını gerektirir.

## <a id="IndexingPolicy"></a>Dizin oluşturma ilkesi belirtme

Geçiş aracının, içeri aktarma sırasında Azure Cosmos DB SQL API koleksiyonları oluşturmasına izin verdiğinizde koleksiyonların dizin oluşturma ilkesini belirtebilirsiniz. Azure Cosmos DB Sıralı kayıt seçenekleri ve Azure Cosmos DB Toplu içeri aktarmanın gelişmiş seçenekler bölümünde Dizin Oluşturma İlkesi bölümüne gidin.

![Azure Cosmos DB Dizin Oluşturma İlkesi gelişmiş seçeneklerinin ekran görüntüsü](./media/import-data/indexingpolicy1.png)

Dizin Oluşturma İlkesi gelişmiş seçeneğini kullanarak, bir dizin oluşturma ilkesi dosyası seçebilir, kendiniz bir dizin oluşturma ilkesi girebilir veya varsayılan şablonlar kümesi içinden seçim yapabilirsiniz (dizin oluşturma ilkesi metin kutusuna sağ tıklayarak).

Aracın sağladığı ilke şablonları şunlardır:

* Varsayılan. Bu ilke, dizelere yönelik eşitlik sorguları gerçekleştirirken en iyisidir. ORDER BY, aralık ve eşitlik sorguları için numaraları kullanırsanız de çalışır. Bu ilkenin, Aralıktan daha düşük dizin depolama yükü vardır.
* Aralık. Bu ilke, sayılar ve dizeler, ORDER BY, aralık ve eşitlik sorguları kullandığınızda en iyisidir. Bu ilkenin, Varsayılan veya Karmadan daha yüksek dizin depolama yükü vardır.

![Azure Cosmos DB Dizin Oluşturma İlkesi gelişmiş seçeneklerinin ekran görüntüsü](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Bir dizin oluşturma ilkesini belirtmezseniz varsayılan ilkenin geçerli olduğu. Dizin oluşturma ilkeleri hakkında daha fazla bilgi için bkz: [Azure Cosmos DB dizin oluşturma ilkeleri](index-policy.md).

## <a name="export-to-json-file"></a>JSON dosyasına dışarı aktarma

Azure Cosmos DB JSON verici, kullanılabilir kaynak seçeneklerden herhangi biri bir dizi JSON belgelerini içeren bir JSON dosyası vermenize olanak sağlar. Sizin için dışarı aktarma Aracı'nı işler. Alternatif olarak, elde edilen geçiş komut görüntüleyip komutu kendi başınıza çalıştırmak seçebilirsiniz. Sonuçta elde edilen JSON dosyası, yerel olarak veya Azure Blob depolama alanında depolanabilir.

![Azure Cosmos DB JSON yerel dosya dışarı aktarma seçeneğinin ekran görüntüsü](./media/import-data/jsontarget.png)

![Azure Cosmos DB JSON Azure Blob depolama alanı dışarı aktarma seçeneğinin ekran görüntüsü](./media/import-data/jsontarget2.png)

İsteğe bağlı olarak elde edilen JSON prettify tercih edebilirsiniz. Bu eylem içeriği yaparken daha fazla sonuç belgesi boyutunu artıracaktır insan tarafından okunabilir.

* Standart JSON dışarı aktarma

  ```JSON
  [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]
  ```

* Prettified JSON dışarı aktarma

  ```JSON
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
  ```

Aşağıda, Azure Blob depolama alanından JSON dosyasını dışarı aktarmak için komut satırı örneği verilmiştir:

```console
dt.exe /ErrorDetails:All /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB database_name>" /s.Collection:<CosmosDB collection_name>
/t:JsonFile /t.File:"blobs://<Storage account key>@<Storage account name>.blob.core.windows.net:443/<Container_name>/<Blob_name>"
/t.Overwrite
```

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Gelişmiş yapılandırma ekranında, hataların yazılmasını istediğiniz günlük dosyasının konumunu belirtin. Bu sayfa için aşağıdaki kurallar geçerlidir:

1. Ardından dosya adı belirtilmezse, sonuçları sayfasında tüm hatalar döndürülür.
2. Bir dizin olmadan dosya adı sağlanırsa, geçerli ortam dizininde dosya oluşturulur (veya dosyanın üzerine yazılır).
3. Mevcut bir belirlerseniz dosyasını ve ardından dosyanın üzerine yazılır, ekleme seçeneği yoktur.
4. Daha sonra tüm hata iletilerinin mi, kritik hata iletilerinin mi günlüğe kaydedileceğini yoksa hiçbir hata iletisinin günlüğe kaydedilmeyeceğini mi seçin. Son olarak, ekrandaki aktarım iletisinin ilerleme durumunun ne sıklıkla güncelleştirileceğine karar verin.

   ![Gelişmiş yapılandırma ekranının görüntüsü](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Ayarları içeri aktarma ve görüntüleme komut satırı onaylayın

1. Kaynak bilgileri, hedef bilgileri ve Gelişmiş Yapılandırma belirttikten sonra geçiş özeti gözden geçirin ve görüntülemek veya isterseniz sonuçta elde edilen geçiş komutu kopyalayın. (Kopyalama komutu alma işlemlerini otomatik hale getirmek kullanışlıdır.)

    ![Özet ekranının ekran görüntüsü](./media/import-data/summary.png)

    ![Özet ekranının ekran görüntüsü](./media/import-data/summarycommand.png)

2. Kaynak ve hedef seçeneklerden memnun kaldığınızda **İçeri Aktar**’a tıklayın. İçeri aktarma devam ettikçe, geçen süre, aktarılan sayı ve hata bilgileri (Gelişmiş yapılandırma bölümünde bir dosya adı sağlamadıysanız) güncelleştirilir. Tamamlandıktan sonra, sonuçları dışarı aktarabilirsiniz (örneğin, içeri aktarma hatalarıyla ilgilenmek için).

    ![Azure Cosmos DB JSON dışarı aktarma seçeneğinin ekran görüntüsü](./media/import-data/viewresults.png)

3. Yeni bir alma işlemi ayrıca, tüm değerlerini sıfırlama veya var olan ayarları tutmak da başlatılabilir. (Örneğin, bağlantı dizesi bilgilerini, kaynak ve hedef seçimi ve daha fazla korumak seçebilirsiniz.)

    ![Azure Cosmos DB JSON dışarı aktarma seçeneğinin ekran görüntüsü](./media/import-data/newimport.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri tamamladınız:

> [!div class="checklist"]
> * Veri Geçişi aracını yüklediniz
> * Farklı veri kaynaklarından verileri içeri aktardınız
> * Azure Cosmos DB’den JSON’a dışarı aktarma işlemi yaptınız

Şimdi, sonraki öğreticiye devam edebilir ve Azure Cosmos DB kullanarak verilerin nasıl sorgulanacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
>[Veriler nasıl sorgulanır?](../cosmos-db/tutorial-query-sql-api.md)
