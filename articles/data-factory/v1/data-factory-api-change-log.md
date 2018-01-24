---
title: "Veri Fabrikası - .NET API değişiklik günlüğü | Microsoft Docs"
description: "Önemli değişiklikler, özellik eklemeler, hata düzeltmeleri vb. açıklar... belirli bir Azure Data Factory için .NET API sürümünde."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 55a08d22c622c89b918d1bfadd0ce34b77c3d408
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - .NET API değişiklik günlüğü
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. 

Bu makale, belirli bir sürümü için Azure Data Factory SDK değişiklikler hakkında bilgi sağlar. Azure Data Factory için en son NuGet paketini bulabilirsiniz [burada](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Sürüm 4.11.0
Özellik ekleme:

* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* Aşağıdaki kopya kaynak türleri eklenmiştir:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Sürüm 4.10.0
* Aşağıdaki isteğe bağlı özellikler için TextFormat eklenmiştir:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* Aşağıdaki kopya kaynak türleri eklenmiştir:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Ekleme [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) AzureMLBatchExecutionActivity özelliği
  * Birden çok web hizmeti girişleri için bir Azure Machine Learning deneme geçirme etkinleştir

## <a name="version-491"></a>Sürüm 4.9.1
### <a name="bug-fix"></a>Hata düzeltmesi
* Webapı tabanlı kimlik doğrulaması için itiraz etme [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Sürüm 4.9.0
### <a name="feature-additions"></a>Özellik ekleme
* Ekleme [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) ve [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) CopyActivity için özellikler. Bkz: [kopyalama hazırlanan](data-factory-copy-activity-performance.md#staged-copy) özelliği hakkında ayrıntılı bilgi için.

### <a name="bug-fix"></a>Hata düzeltmesi
* Bir aşırı yüklemesini tanıtmak [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) geçen yöntemi bir [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) örneği.
* İşareti [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) ve [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) CopySink, isteğe bağlı olarak.

## <a name="version-480"></a>Sürüm 4.8.0
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki isteğe bağlı özellikler kopyalama performansını ayarlama etkinleştirmek için kopyalama etkinliği türü eklenmiştir:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Sürüm 4.7.0
### <a name="feature-additions"></a>Özellik ekleme
* Yeni StorageFormat türü eklenen [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) en iyi duruma getirilmiş satır sütun (ORC) biçiminde dosyaları kopyalamak için türü.
* Ekleme [Bulunan'allowpolybase](https://msdn.microsoft.com/library/mt723396.aspx) ve SqlDWSink PolyBaseSettings özellikleri.
  * SQL Data Warehouse'a veri kopyalamak için PolyBase kullanımını etkinleştirir.

## <a name="version-461"></a>Sürüm 4.6.1
### <a name="bug-fixes"></a>Hata Düzeltmeleri
* Etkinlik pencerelerine listeleme için HTTP isteği giderir.
  * Kaynak grubu adı ve veri fabrikası adı istek yükü kaldırır.

## <a name="version-460"></a>Sürüm 4.6.0
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki özellikler için eklenene [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Veri kümeleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* Aşağıdaki özellikler için eklenene [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Yeni eklenen [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) türü [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) türü verisini olan JSON biçiminde veri kümelerini tanımlayın.

## <a name="version-450"></a>Sürüm 4.5.0
### <a name="feature-additions"></a>Özellik ekleme
* Eklenen [listesinde etkinlik penceresini işlemlerinde](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Varlık türlerine göre (diğer bir deyişle, veri fabrikaları, veri kümelerini, ardışık düzen ve etkinlikleri) filtrelerle etkinlik windows almak için yöntemleri eklendi.
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Aşağıdaki kopya kaynak türleri eklenmiştir:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Sürüm 4.4.0
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki bağlantılı hizmet türü veri kaynakları olarak eklenen ve kopyalama etkinlikler için iç havuzlar:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Bkz: [Azure depolama SAS bağlantılı hizmet](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) kavramsal bilgi ve örnekler için.

## <a name="version-430"></a>Sürüm 4.3.0
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki bağlantılı hizmet türleri başlattıysanız edilmiş kopyalama etkinlikler için veri kaynakları olarak eklendi:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Bkz: [Data Factory kullanarak HDFS veri taşıma](data-factory-hdfs-connector.md) kavramsal bilgi ve örnekler için.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Bkz: [taşıma Azure Data Factory kullanarak veri öğesinden ODBC veri depolarını](data-factory-odbc-connector.md) kavramsal bilgi ve örnekler için.

## <a name="version-420"></a>Sürüm 4.2.0
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki yeni etkinlik türü eklenmiştir: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Etkinliği hakkında daha fazla ayrıntı için bkz: [kaynak güncelleştirme etkinliği kullanarak Azure ML güncelleştirme modelleri](data-factory-azure-ml-batch-execution-activity.md).
* Yeni bir isteğe bağlı özellik [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) eklendi [AzureMLLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) ve [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) özellikler eklenmiştir [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) sınıfı.
* Data Factory hizmetinin istemci çağrılarını zaman aşımlarını yapılandırılmasına izin verin.

## <a name="version-410"></a>Sürüm 4.1.0'da
### <a name="feature-additions"></a>Özellik ekleme
* Aşağıdaki bağlantılı hizmet türleri eklenmiştir:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Aşağıdaki etkinlik türlerini eklenmiştir:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Aşağıdaki veri türleri eklenmiştir:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Kopyalama etkinliği aşağıdaki kaynak ve havuz türleri eklenmiştir:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>4.0.1 sürümünü
### <a name="breaking-changes"></a>Yeni değişiklikler
Aşağıdaki sınıflar yeniden adlandırılmıştır. 4.0.0 bırakmadan önce yeni adlarını sınıflarının özgün adlarını yoktu.

| İçinde 4.0.0 adı | İçinde 4.0.1 adı |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Sürüm 4.0.0
### <a name="breaking-changes"></a>Yeni değişiklikler
* Aşağıdaki sınıflar/arabirimleri yeniden adlandırılmıştır.

| Eski adı | Yeni bir ad |
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

* **Listesi** yöntemleri disk belleğine alınmış sonuçlar şimdi döndürür. Yanıt boş olmayan içeriyorsa **NextLink** özelliği, istemci uygulaması gereken tüm sayfaları geri döndürülene kadar sonraki sayfaya getirme devam etmek.  Örnek aşağıda verilmiştir:

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
* **Liste** ardışık düzen API yalnızca tam Ayrıntılar yerine bir ardışık düzen özetini döndürür. Örneği için bir ardışık düzen Özet etkinlikleri yalnızca adını ve türünü içerir.

### <a name="feature-additions"></a>Özellik ekleme
* [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) sınıfı, iki yeni özellikleri destekler **Sliceıdentifiercolumnname** ve **SqlWriterCleanupScript**, Azure SQL Data Warehouse ıdempotent kopyaya desteklemek için. Bkz: [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) bu özellikleri hakkında ayrıntılı bilgi için makalenin.
* Saklı yordam Azure SQL Database ve Azure SQL Data Warehouse kaynaklarına karşı kopyalama etkinliği bir parçası olarak çalışan artık destekler. [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) ve [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) sınıfları, aşağıdaki özelliklere sahiptir: **SqlReaderStoredProcedureName** ve **StoredProcedureParameters**. Bkz: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#sqlsource) ve [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) Azure.com üzerindeki makaleler için bu özellikleri hakkında ayrıntılı bilgi.  
