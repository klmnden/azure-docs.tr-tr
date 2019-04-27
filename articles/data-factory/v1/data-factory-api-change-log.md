---
title: Data Factory - .NET API'si değişiklik günlüğü | Microsoft Docs
description: Bozucu değişiklikleri, özellik eklemeleri ve hata düzeltmeleri vb. açıklar... belirli bir Azure Data Factory .NET API sürümünde.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 863f3500c84eeab1c3dac19141cd334fc6961694
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60567257"
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - .NET API'si değişiklik günlüğü
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Bu makalede, Azure Data Factory SDK için belirli bir sürümde değişiklikleri hakkında bilgi sağlar. Azure Data Factory için en son NuGet paketini bulabilirsiniz [burada](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Sürüm 4.11.0
Özellik eklemeleri:

* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* Aşağıdaki kopyalama kaynak türleri eklenmiştir:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Sürüm 4.10.0
* TextFormat için aşağıdaki isteğe bağlı özellikler eklenmiştir:
  * [skipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [firstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* Aşağıdaki kopyalama kaynak türleri eklenmiştir:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Ekleme [Webserviceınputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) AzureMLBatchExecutionActivity özelliği
  * Azure Machine Learning deneme hizmeti girişleri birden çok web geçirme etkinleştir

## <a name="version-491"></a>Sürüm 4.9.1
### <a name="bug-fix"></a>Hata düzeltmesi
* WebApi tabanlı kimlik doğrulaması için kullanımdan [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Sürüm 4.9.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Ekleme [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) ve [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) CopyActivity özellikleri. Bkz: [kopyalama aşamalı](data-factory-copy-activity-performance.md#staged-copy) özelliği hakkında ayrıntılı bilgi için.

### <a name="bug-fix"></a>Hata düzeltmesi
* Bir aşırı yüklemesini tanıtmak [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) gereken yöntemini bir [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) örneği.
* İşareti [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) ve [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) CopySink, isteğe bağlı olarak.

## <a name="version-480"></a>Sürüm 4.8.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Kopyalama performansını ayarlama etkinleştirmek için kopyalama etkinliği türü için aşağıdaki isteğe bağlı özellikler eklenmiştir:
  * [parallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Sürüm 4.7.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Eklenen yeni StorageFormat türü [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) en iyi duruma getirilmiş satır irdelemenizde (ORC) dosyaları kopyalamak için yazın.
* Ekleme [Bulunan'allowpolybase](https://msdn.microsoft.com/library/mt723396.aspx) ve SqlDWSink PolyBaseSettings özellikleri.
  * PolyBase, SQL veri ambarı'na veri kopyalamak için kullanılmasını sağlar.

## <a name="version-461"></a>4.6.1 sürümü
### <a name="bug-fixes"></a>Hata Düzeltmeleri
* Etkinlik pencerelerini listelemek için HTTP isteği düzeltir.
  * İstek yükü, kaynak grubu adı ve veri fabrikasının adını kaldırır.

## <a name="version-460"></a>Sürüm 4.6.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki özellikler eklenmiştir [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [expirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Veri kümeleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* Aşağıdaki özellikler eklenmiştir [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Yeni eklenen [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) türü [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) verilerini JSON biçiminde olan veri kümeleri tanımlamak için tür.

## <a name="version-450"></a>Sürüm 4.5.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Eklenen [etkinlik penceresi için işlemleri listele](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Varlık türleri üzerinde (diğer bir deyişle, veri fabrikası, veri kümeleri, işlem hatları ve etkinlikler) göre filtreler ile etkinlik pencereleri almak için yöntemler eklendi.
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Aşağıdaki kopyalama kaynak türleri eklenmiştir:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Sürüm 4.4.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türü, veri kaynakları olarak eklenen ve için kopyalama etkinliği iç havuzları:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Bkz: [Azure depolama bağlı hizmeti SAS](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) kavramsal bilgiler ve örnekler için.

## <a name="version-430"></a>Sürüm 4.3.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türleri haven olan veri kaynakları için kopyalama etkinliği olarak eklenir:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Bkz: [HDFS Data Factory ile veri taşıma](data-factory-hdfs-connector.md) kavramsal bilgiler ve örnekler için.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Bkz: [taşıma Azure Data Factory ile veri öğesinden ODBC veri depoları](data-factory-odbc-connector.md) kavramsal bilgiler ve örnekler için.

## <a name="version-420"></a>Sürüm 4.2.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki yeni bir etkinlik türü eklenmiştir: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Etkinlik hakkında daha fazla ayrıntı için bkz: [kaynak güncelleştirme etkinliği kullanarak güncelleştirme Azure ML modelleri](data-factory-azure-ml-batch-execution-activity.md).
* Yeni bir isteğe bağlı özellik [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) eklendi [AzureMLLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) ve [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) özellikler eklenmiştir [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) sınıfı.
* Data Factory hizmetinin istemci çağrılarını zaman aşımlarını yapılandırılmasına izin verin.

## <a name="version-410"></a>Sürüm 4.1.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Aşağıdaki etkinlik türlerini eklenmiştir:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Aşağıdaki kaynak ve havuz türleri kopyalama etkinliği için eklenmiştir:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Sürüm 4.0.1'in
### <a name="breaking-changes"></a>Yeni değişiklikler
Aşağıdaki sınıfları yeniden adlandırıldı. 4.0.0 yayımlamadan önce yeni adları sınıfları özgün adlarını yoktu.

| İçinde 4.0.0 adı | İçinde 4.0.1'in adı |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Sürüm 4.0.0
### <a name="breaking-changes"></a>Yeni değişiklikler
* Aşağıdaki sınıfı/arabirimi yeniden adlandırıldı.

| Eski adı | Yeni ad |
|:--- |:--- |
| ITableOperations |[IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Tablo |[Veri kümesi](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| TableProperties |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TableTypeProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters |[DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| TableCreateOrUpdateResponse |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| TableListResponse |[DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* **Listesi** yöntemleri disk belleğine alınmış sonuçlar artık döndürür. Boş olmayan bir yanıtı içeriyorsa **NextLink** özelliği, istemci uygulaması gereken tüm sayfaları geri döndürülene kadar sonraki sayfaya getiriliyor. devam etmek.  Örnek aşağıda verilmiştir:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Liste** API işlem hattı yalnızca bir işlem hattı yerine tam ayrıntıları özetini döndürür. Örneği için bir işlem hattı özetindeki etkinlikleri yalnızca adı ve türü içerir.

### <a name="feature-additions"></a>Özellik eklemeleri
* [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) sınıfı, iki yeni özellikleri destekler **Sliceıdentifiercolumnname** ve **SqlWriterCleanupScript**, Azure SQL veri ıdempotent Kopyala desteklemek için Ambarı. Bkz: [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md) makalede bu özellikleri hakkında ayrıntılar için.
* Saklı yordam, kopyalama etkinliği bir parçası olarak Azure SQL veritabanı ve Azure SQL veri ambarı kaynaklarına karşı çalıştırılması artık desteklenmektedir. [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) ve [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) sınıfları aşağıdaki özelliklere sahiptir: **SqlReaderStoredProcedureName** ve **StoredProcedureParameters**. Bkz: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#sqlsource) ve [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) Azure.com makalelerde bu özellikleri hakkında ayrıntılar için.  
