---
title: 'Hızlı Başlangıç: Azure Veri Gezgini düğümü kitaplığını kullanarak veri alma'
description: Bu hızlı başlangıçta Node.js kullanarak verileri Azure Veri Gezgini'ne almayı (yüklemeyi) öğreneceksiniz.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: 0a23c171d99d46eb29dd589867ce70ca2739ff29
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756458"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>Hızlı Başlangıç: Azure Veri Gezgini düğümü kitaplığını kullanarak veri alma

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini Node için iki istemci kitaplığı sağlar: [alma kitaplığı](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest) ve [veri kitaplığı](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data). Bu kitaplıklar verileri bir kümeye almanıza (yüklemenize ve kodunuzdan verileri sorgulamanıza olanak tanır. Bu hızlı başlangıçta, önce tek kümesinde bir tablo ve veri eşlemesi oluşturursunuz. Ardından veri alımını kümenin kuyruğuna ekler ve sonuçları doğrularsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için Azure aboneliğine ek olarak aşağıdakilere de ihtiyacınız vardır:

* [Test kümesi ve veritabanı](create-cluster-database-portal.md)

* Geliştirme bilgisayarınıza yüklenmiş [Node.js](https://nodejs.org/en/download/)

## <a name="install-the-data-and-ingest-libraries"></a>Veri ve alma kitaplarını yükleme

*azure-kusto-ingest* ve *azure-kusto-data* bileşenlerini yükleyin

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>İçeri aktarma deyimlerini ve sabitlerini ekleme

Sınıfları kitaplıklardan içeri aktarma

```javascript

const KustoConnectionStringBuilder = require("azure-kusto-data").KustoConnectionStringBuilder;
const KustoClient = require("azure-kusto-data").Client;
const KustoIngestClient = require("azure-kusto-ingest").IngestClient;
const IngestionProperties = require("azure-kusto-ingest").IngestionProperties;
const { DataFormat } = require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").IngestionDescriptors;

```
Azure Veri Gezgini uygulamanın kimliğini doğrulamak için Azure Active Directory kiracı kimliğinizi kullanır. Kiracı kimliğinizi bulmak için bkz. [Office 365 kiracı kimliğinizi bulma](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id).

Bu kodu çalıştırmadan önce `authorityId`, `kustoUri`, `kustoIngestUri` ve `kustoDatabase` değerlerini ayarlayın.

```javascript
const cluster = "MyCluster";
const region = "westus";
const authorityId = "microsoft.com";
const kustoUri = `https://${cluster}.${region}.kusto.windows.net:443`;
const kustoIngestUri = `https://ingest-${cluster}.${region}.kusto.windows.net:443`;
const kustoDatabase  = "Weather";
```

Şimdi bağlantı dizesini hazırlayın. Bu örnekte kümeye erişmek için cihaz kimlik doğrulaması kullanılır. Azure Active Directory uygulama sertifikasını, uygulama adını ve kullanıcı adıyla parolasını da kullanabilirsiniz.

Sonraki bir adımda hedef tabloyu ve eşlemeyi oluşturursunuz.

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);
const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);
const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>Kaynak dosya bilgilerini ayarlama

Ek sınıfları içeri aktarın ve veri kaynak dosyası için sabitleri ayarlayın. Bu örnekte, Azure Blob Depolama'da barındırılan bir örnek dosya kullanılır. **StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

```javascript
const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
const filePath = "StormEvents.csv";
const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}`;
```

## <a name="create-a-table-on-your-test-cluster"></a>Test kümenizde tablo oluşturma

`StormEvents.csv` dosyasındaki verilerin şemasıyla eşleşen bir tablo oluşturun. Bu kod çalıştığında, aşağıdaki gibi bir ileti döndürür: *Oturum açmak için bir web tarayıcısı kullanarak https://microsoft.com/devicelogin ve XXXXXXXXX kimliğini doğrulamak için kodu girin*. Adımları izleyerek oturum açın, sonra da dönüp bir sonraki kod bloğunu çalıştırın. Bağlantı kuran sonraki kod blokları için yeniden oturum açmak gerekir.

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = `.create table ${destTable} (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)`;

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="define-ingestion-mapping"></a>Veri alımı eşlemesini tanımlama

Gelen CSV verilerini tablo oluştururken kullanılan sütun adları ve veri türleriyle eşler.

```javascript
const createMappingCommand = `.create table ${destTable} ingestion csv mapping '${destTableMapping}' '[{"Name":"StartTime","datatype":"datetime","Ordinal":0}, {"Name":"EndTime","datatype":"datetime","Ordinal":1},{"Name":"EpisodeId","datatype":"int","Ordinal":2},{"Name":"EventId","datatype":"int","Ordinal":3},{"Name":"State","datatype":"string","Ordinal":4},{"Name":"EventType","datatype":"string","Ordinal":5},{"Name":"InjuriesDirect","datatype":"int","Ordinal":6},{"Name":"InjuriesIndirect","datatype":"int","Ordinal":7},{"Name":"DeathsDirect","datatype":"int","Ordinal":8},{"Name":"DeathsIndirect","datatype":"int","Ordinal":9},{"Name":"DamageProperty","datatype":"int","Ordinal":10},{"Name":"DamageCrops","datatype":"int","Ordinal":11},{"Name":"Source","datatype":"string","Ordinal":12},{"Name":"BeginLocation","datatype":"string","Ordinal":13},{"Name":"EndLocation","datatype":"string","Ordinal":14},{"Name":"BeginLat","datatype":"real","Ordinal":16},{"Name":"BeginLon","datatype":"real","Ordinal":17},{"Name":"EndLat","datatype":"real","Ordinal":18},{"Name":"EndLon","datatype":"real","Ordinal":19},{"Name":"EpisodeNarrative","datatype":"string","Ordinal":20},{"Name":"EventNarrative","datatype":"string","Ordinal":21},{"Name":"StormSummary","datatype":"dynamic","Ordinal":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="queue-a-message-for-ingestion"></a>Veri alımı için bir iletiyi kuyruğa alma

Blob depolamadan verileri çekmek ve bu verileri Azure Veri Gezgini'ne almak için bir iletiyi kuyruğa alın.

```javascript
const defaultProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const ingestClient = new KustoIngestClient(kcsbIngest, defaultProps);
// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties

const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,null, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>Verilerin tabloya eklendiğini doğrulama

Verilerin tabloya alındığını doğrulayın. Kuyruğa eklenen veri alımının, verileri Azure Veri Gezgini'ne alma ve yükleme işlemini zamanlaması için beş ile on dakika arasında bekleyin. Ardından aşağıdaki kodu çalıştırarak `StormEvents` tablosundaki kayıtların sayısını alın.

```javascript
const query = `${destTable} | count`;

kustoClient.execute(kustoDatabase, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="run-troubleshooting-queries"></a>Sorun giderme sorguları çalıştırma

[https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın ve kümenize bağlanın. Son dört saatte hiç veri alımı hatası olup olmadığını görmek için veritabanınızda aşağıdaki komutu çalıştırın. Çalıştırmadan önce veritabanı adını değiştirin.
    
```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

Son dört saatteki tüm veri alım işlemlerinin durumunu görüntülemek için aşağıdaki komutu çalıştırın. Çalıştırmadan önce veritabanı adını değiştirin.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun. Aksi takdirde, veritabanınızda aşağıdaki komutu çalıştırarak `StormEvents` tablosunu temizleyin.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sorgu yazma](write-queries.md)
