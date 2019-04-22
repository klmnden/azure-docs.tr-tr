---
title: "Hızlı Başlangıç: Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma"
description: Bu hızlı başlangıçta, .NET standart SDK'sını kullanarak Azure Veri Gezgini içinde (yükle) alabilen öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/18/2018
ms.openlocfilehash: 6a068c45a13bd45a09ed51fd154b5842938e0c5e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59044679"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-net-standard-sdk-preview"></a>Hızlı Başlangıç: Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma

Azure Veri Gezgini (ADX), günlük ve telemetri verilerini için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. ADX iki istemci kitaplığı için .NET Standard sağlar: bir [kitaplığı alma](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Ingest.NETStandard) ve [veri Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard). Bu kitaplıklar verileri bir kümeye almanıza (yüklemenize ve kodunuzdan verileri sorgulamanıza olanak tanır. Bu hızlı başlangıçta, önce tek kümesinde bir tablo ve veri eşlemesi oluşturursunuz. Bu küme için bir alma sıra ve sonuçları doğrulayın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

* [Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="install-the-ingest-library"></a>Alma Kitaplığı'nı yükleyin

```
Install-Package Microsoft.Azure.Kusto.Ingest.NETStandard
```

## <a name="authentication"></a>Authentication

Azure Veri Gezgini uygulamanın kimliğini doğrulamak için AAD kiracı kimliğinizi kullanır. Kiracı kimliğinizi bulmak için aşağıdaki URL'yi kullanın ve *YourDomain* yerine kendi etki alanınızı yazın.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'ye tıklayın; ilk satır aşağıdaki gibidir. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Bu örnekte kiracı kimliği `6babcaad-604b-40ac-a9d7-9fd97c0b779f` değeridir.

Bu örnek, kümeye erişmek için bir AAD kullanıcısı ve kimlik doğrulaması için parola kullanır. Ayrıca, AAD uygulama sertifikası ve AAD uygulama anahtarını da kullanabilirsiniz. Ayarlamak için değerler `tenantId`, `user`, ve `password` bu kodu çalıştırmadan önce.

```csharp
var tenantId = "<TenantId>";
var user = "<User>";
var password = "<Password>";
```

## <a name="construct-the-connection-string"></a>Bağlantı dizesi oluşturun
Şimdi bağlantı dizesini hazırlayın. Sonraki bir adımda hedef tabloyu ve eşlemeyi oluşturursunuz.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var database = "<DatabaseName>";

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };
```

## <a name="set-source-file-information"></a>Kaynak dosya bilgilerini ayarlama

Kaynak dosya yolunu ayarlayın. Bu örnekte, Azure Blob Depolama'da barındırılan bir örnek dosya kullanılır. **StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

```csharp
var blobPath = "https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
```

## <a name="create-a-table-on-your-test-cluster"></a>Test kümenizde tablo oluşturma
Adlı bir tablo oluşturun `StormEvents` verileri şemasını eşleşen `StormEvents.csv` dosya.

```csharp
var table = "StormEvents";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("StartTime", "System.DateTime"),
                Tuple.Create("EndTime", "System.DateTime"),
                Tuple.Create("EpisodeId", "System.Int32"),
                Tuple.Create("EventId", "System.Int32"),
                Tuple.Create("State", "System.String"),
                Tuple.Create("EventType", "System.String"),
                Tuple.Create("InjuriesDirect", "System.Int32"),
                Tuple.Create("InjuriesIndirect", "System.Int32"),
                Tuple.Create("DeathsDirect", "System.Int32"),
                Tuple.Create("DeathsIndirect", "System.Int32"),
                Tuple.Create("DamageProperty", "System.Int32"),
                Tuple.Create("DamageCrops", "System.Int32"),
                Tuple.Create("Source", "System.String"),
                Tuple.Create("BeginLocation", "System.String"),
                Tuple.Create("EndLocation", "System.String"),
                Tuple.Create("BeginLat", "System.Double"),
                Tuple.Create("BeginLon", "System.Double"),
                Tuple.Create("EndLat", "System.Double"),
                Tuple.Create("EndLon", "System.Double"),
                Tuple.Create("EpisodeNarrative", "System.String"),
                Tuple.Create("EventNarrative", "System.String"),
                Tuple.Create("StormSummary", "System.Object"),
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="define-ingestion-mapping"></a>Veri alımı eşlemesini tanımlama

Gelen CSV veri tablosunu oluştururken kullanılan sütun adlarını eşleyin.
Sağlama bir [CSV sütun eşleme nesnesi](/azure/kusto/management/tables#create-ingestion-mapping) Bu tablo üzerinde

```csharp
var tableMapping = "StormEvents_CSV_Mapping";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCsvMappingCreateCommand(
            table,
            tableMapping,
            new[]
            {
                new CsvColumnMapping { ColumnName = "StartTime", Ordinal = 0 },
                new CsvColumnMapping { ColumnName = "EndTime", Ordinal = 1 },
                new CsvColumnMapping { ColumnName = "EpisodeId", Ordinal = 2 },
                new CsvColumnMapping { ColumnName = "EventId", Ordinal = 3 },
                new CsvColumnMapping { ColumnName = "State", Ordinal = 4 },
                new CsvColumnMapping { ColumnName = "EventType", Ordinal = 5 },
                new CsvColumnMapping { ColumnName = "InjuriesDirect", Ordinal = 6 },
                new CsvColumnMapping { ColumnName = "InjuriesIndirect", Ordinal = 7 },
                new CsvColumnMapping { ColumnName = "DeathsDirect", Ordinal = 8 },
                new CsvColumnMapping { ColumnName = "DeathsIndirect", Ordinal = 9 },
                new CsvColumnMapping { ColumnName = "DamageProperty", Ordinal = 10 },
                new CsvColumnMapping { ColumnName = "DamageCrops", Ordinal = 11 },
                new CsvColumnMapping { ColumnName = "Source", Ordinal = 12 },
                new CsvColumnMapping { ColumnName = "BeginLocation", Ordinal = 13 },
                new CsvColumnMapping { ColumnName = "EndLocation", Ordinal = 14 },
                new CsvColumnMapping { ColumnName = "BeginLat", Ordinal = 15 },
                new CsvColumnMapping { ColumnName = "BeginLon", Ordinal = 16 },
                new CsvColumnMapping { ColumnName = "EndLat", Ordinal = 17 },
                new CsvColumnMapping { ColumnName = "EndLon", Ordinal = 18 },
                new CsvColumnMapping { ColumnName = "EpisodeNarrative", Ordinal = 19 },
                new CsvColumnMapping { ColumnName = "EventNarrative", Ordinal = 20 },
                new CsvColumnMapping { ColumnName = "StormSummary", Ordinal = 21 },
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="queue-a-message-for-ingestion"></a>Veri alımı için bir iletiyi kuyruğa alma

Bir ileti blob depolamadan veri çekmek üzere sıraya ve bu verileri ADX alma.

```csharp
var ingestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/";
var ingestConnectionStringBuilder =
    new KustoConnectionStringBuilder(ingestUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };

using (var ingestClient = KustoIngestFactory.CreateQueuedIngestClient(ingestConnectionStringBuilder))
{
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.csv,
            CSVMappingReference = tableMapping,
            IgnoreFirstRecord = true
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
}
```

## <a name="validate-data-was-ingested-into-the-table"></a>Doğrulama veri tablosu içine alınan

Alma zamanlamak ve ADX veri yükleme kuyruğa alınan alımı için beş ila on dakika bekleyin. Ardından aşağıdaki kodu çalıştırarak `StormEvents` tablosundaki kayıtların sayısını alın.

```csharp
using (var cslQueryProvider = KustoClientFactory.CreateCslQueryProvider(kustoConnectionStringBuilder))
{
    var query = $"{table} | count";

    var results = cslQueryProvider.ExecuteQuery<long>(query);
    Console.WriteLine(results.Single());
}
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
