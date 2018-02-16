---
title: "Veri Fabrikası kullanarak ve Azure Table depolama veri kopyalamak | Microsoft Docs"
description: "Veri Fabrikası kullanarak Azure Table depolama için desteklenen kaynak depoları veya desteklenen havuz depolarına Table storage veri kopyalamak öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 41e2117e14f336d33f5d6f4e1f446e32a6886079
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Table depolama gelen ve giden veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - genel olarak kullanılabilir](v1/data-factory-azure-table-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-table-storage.md)

Bu makalede kopya etkinliği Azure Data Factory'de ve Azure Table depolama veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [tablo depolama Bağlayıcısı sürüm 1](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tablo depolama alanına tüm desteklenen kaynak veri deposundan verileri kopyalayabilirsiniz. Ayrıca, verileri Table storage'tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Paylaşılan erişim imzası kimlik doğrulamaları özellikle, hesap anahtarı ve hizmeti kullanarak veri kopyalama bu Azure tablo bağlayıcısını destekler.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını tablo depolama alanına tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

### <a name="use-an-account-key"></a>Hesap anahtarı kullanın

Azure depolama bağlı hizmeti hesap anahtarı kullanarak oluşturabilirsiniz. Genel erişim ile veri fabrikası depolama sağlar. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AzureStorage**. |Evet |
| connectionString | ConnectionString özelliği için depolama alanına bağlanmak için gereken bilgileri belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-service-shared-access-signature-authentication"></a>Kullanım hizmeti paylaşılan erişim imzası kimlik doğrulaması

Depolama bağlantılı hizmeti bir paylaşılan erişim imzası kullanarak da oluşturabilirsiniz. Veri Fabrikası depolama alanındaki tüm/özel kaynakları kısıtlanmış/zaman sınırlı erişim sağlar.

Paylaşılan erişim imzası temsilci depolama hesabınızdaki kaynaklara erişim sağlar. Bir istemci belirli bir süredir ve belirtilen bir izin kümesi ile sınırlı depolama hesabındaki nesnelere izinleri vermek için kullanabilirsiniz. Hesap erişim tuşlarınızı paylaşmak gerekmez. Paylaşılan erişim imzası depolama kaynağı için kimlik doğrulamalı erişim için gerekli tüm bilgileri kendi sorgu parametrelerini kapsayan bir URI değil. Paylaşılan erişim imzası ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun oluşturucunun ya da yöntemi paylaşılan erişim imzası geçirmek gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları: paylaşılan erişim imzası modelini anlamanıza](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Veri Fabrikası artık yalnızca hizmet paylaşılan erişim imzaları ancak hesap paylaşılan erişim imzaları destekler. Bu iki türleri ve bunları oluşturma hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları türlerini](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). Paylaşılan erişim Azure portal ya da Azure Storage Gezgini üretilen imza desteklenmeyen bir hesap paylaşılan erişim imzası URL'dir.

> [!TIP]
> Depolama hesabınız için hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutları çalıştırabilirsiniz. Yer tutucuları değiştirin ve gerekli izni verin.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Hizmet paylaşılan erişim imzası kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AzureStorage**. |Evet |
| sasUri | Blob, kapsayıcı ya da tablo gibi depolama kaynakları için paylaşılan erişim imzası URI belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Bağlantılı hizmet (okuma, yazma, okuma/yazma), veri fabrikası nasıl kullanıldığına göre nesneler üzerinde uygun okuma/yazma izinleri ayarlayın.
- Ayarlama **sona erme saati** uygun şekilde. Depolama nesnelere erişimi ardışık düzen etkin süresi içinde süresi sona ermiyor emin olun.
- URI gereksinimleri temelinde sağ tablo düzeyinde oluşturulmalıdır.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Azure tablosu veri kümesi tarafından desteklenen özellikler listesini sağlar.

Veri ve Azure tablosundan kopyalamak için veri kümesi türü özelliğini ayarlayın **AzureTable**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak **AzureTable**. |Evet |
| tableName |Bağlantılı hizmet başvurduğu tablo Depolama veritabanı örneğinde tablonun adı. |Evet |

**Örnek:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası

Azure Table gibi şemasız veri depoları için Data Factory şema aşağıdaki yollardan biriyle oluşturur:

* Verilerin yapısını kullanarak belirtirseniz **yapısı** Data Factory veri kümesi tanımında özelliği bu yapısı şema olarak geliştirir. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bir null değer için sağlanır.
* Kullanarak verilerin yapısını belirtmezseniz **yapısı** Data Factory veri kümesi tanımı özelliğinde verileri ilk satırını kullanarak şema oluşturur. Bu durumda, ilk satırın tam şema içermiyorsa, bazı sütunları kopyalama işlemi sonucunda eksik.

Şemasız veri kaynakları için en iyi uygulama verilerinin yapısını kullanarak belirtmektir **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure Table kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-table-as-a-source-type"></a>Bir kaynak türü olarak Azure tablo

Verileri Azure tablosundan kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzureTableSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **AzureTableSource**. |Evet |
| azureTableSourceQuery |Verileri okumak için özel tablo depolama sorgu kullanın. Aşağıdaki bölümdeki örneklere bakın. |Hayır |
| azureTableSourceIgnoreTableNotFound |Mevcut tablonun özel izin verilip verilmeyeceğini belirtir.<br/>İzin verilen değerler **True** ve **False** (varsayılan). |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri

Azure tablo sütunu datetime türü ise:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Azure tablo sütunu dize türü ise:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

Ardışık Düzen parametreyi kullanırsanız, tarih saat değeri önceki örneklere göre uygun biçimine dönüştürün.

### <a name="azure-table-as-a-sink-type"></a>Havuz türü olarak Azure tablo

Azure tablosuna veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **AzureTableSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak **AzureTableSink**. |Evet |
| azureTableDefaultPartitionKeyValue |Havuzu tarafından kullanılan varsayılan bölüm anahtar değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütun adını belirtin. Belirtilmezse, "AzureTableDefaultPartitionKeyValue" Bölüm anahtarı olarak kullanılır. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Hayır |
| azureTableInsertType |Azure tabloya veri ekleme modu. Bu özellik, var olan satırları eşleşen bölüm ve satır anahtarlarla çıkış tablosuna birleştirilmiş veya değiştirilmesi değerlerine sahip olup olmadığını denetler. <br/><br/>İzin verilen değerler **birleştirme** (varsayılan) ve **Değiştir**. <br/><br> Bu ayar, tablo düzeyi satır düzeyinde uygulanır. Hiçbiri seçeneğini girdide var olmayan satır çıkış tablosuna siler. Birleştirme ve değiştirme ayarları nasıl çalıştığı hakkında bilgi edinmek için [ekleme veya birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [Ekle veya varlığı değiştirme](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler.<br/>Tamsayı (satır sayısı) izin verilen değerler. |Hayır (varsayılan değeri: 10.000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler.<br/>Timespan izin verilen değerler. Örnek "00: 20:00" (20 dakika). |Hayır (varsayılan olarak 90 saniye, depolama istemcinin varsayılan zaman aşımı) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Kullanarak bir hedef sütun için bir kaynak sütun eşleme **"Çeviricisi"** azureTablePartitionKeyName hedef sütun kullanabilmeniz için önce özelliği.

Aşağıdaki örnekte, kaynak sütunu DivisionID hedef sütun DivisionID eşlenir:

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID" Bölüm anahtarı olarak belirtilir.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Azure Table için veri türü eşlemesi

İlk ve son Azure Table veri kopyaladığınızda, aşağıdaki eşlemelerini Azure Table veri türlerinden Data Factory geçici veri türleri için kullanılır. Nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Taşıdığınızda veri Azure Table, aşağıdaki gelen ve giden [Azure Table tarafından tanımlanan eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure tablo OData türleri .NET türü ve tersi yönde kullanılır.

| Azure tablo veri türü | Veri Fabrikası geçici veri türü | Ayrıntılar |
|:--- |:--- |:--- |
| Edm.Binary |Byte] |Bir bayt dizisi en çok 64 KB. |
| Edm.Boolean |bool |Bir Boole değeri. |
| Edm.DateTime |Tarih Saat |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen bir 64-bit değeri. Desteklenen tarih/saat aralığı, 1 Ocak 1601 gece yarısı başlar (C.E.), UTC. Aralık 31 Aralık 9999 sonlandırır. |
| Edm.Double |Çift |Bir 64-bit kayan noktalı değeri. |
| Edm.Guid |Guid |128-bit bir genel benzersiz tanımlayıcısı. |
| Edm.Int32 |Int32 |Bir 32 bit tamsayı. |
| Edm.Int64 |Int64 |64 bitlik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlu değer. Dize değeri, en çok 64 KB olabilir. |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
