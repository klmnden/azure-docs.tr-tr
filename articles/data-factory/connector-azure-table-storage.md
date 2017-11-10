---
title: Veri kopyalama/Data Factory kullanarak Azure Table Storage | Microsoft Docs
description: "Desteklenen kaynak depoları Azure tablo depolaması (veya) Table Storage veri kopyalamak için desteklenen havuz depoları Data Factory kullanarak öğrenin."
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
ms.date: 11/07/2017
ms.author: jingwang
ms.openlocfilehash: 7b1ee6afc3cb3d55e2abd1bcf742610e7dcc92ea
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="copy-data-to-or-from-azure-table-using-azure-data-factory"></a>Veri ya da Azure Data Factory kullanarak Azure tablosundan kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-table-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-table-storage.md)

Bu makalede kopya etkinliği Azure Data Factory'de veri ve Azure tablosundan kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Azure Table Storage Bağlayıcısı](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen kaynak veri deposundan Azure tabloya veri kopyalama veya verileri Azure tablosundan tüm desteklenen havuz veri deposuna kopyalamak. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure Table bağlayıcı her ikisini de kullanarak veri kopyalamayı destekleyen **hesap anahtarı** ve **hizmet SAS** (paylaşılan erişim imzası) kimlik doğrulamaları.

## <a name="get-started"></a>başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Azure Table Storage Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

### <a name="using-account-key"></a>Hesap anahtarı kullanma

Veri Fabrikası için Azure Storage ile genel erişim sağlayan hesap anahtarı kullanarak bir Azure Storage bağlı hizmeti oluşturabilirsiniz. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| connectionString | ConnectionString özelliği için Azure depolama alanına bağlanmak için gereken bilgileri belirtin. Bu alan SecureString işaretleyin. |Evet |
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

### <a name="using-service-sas-authentication"></a>Hizmet SAS kimlik doğrulaması kullanma

Bir Azure depolama bağlantılı hizmeti bir paylaşılan erişim imzası (data factory depolama alanındaki tüm/özel kaynakları kısıtlanmış/zaman sınırlı erişim sağlayan SAS), kullanarak da oluşturabilirsiniz.

Paylaşılan erişim imzası (SAS) depolama hesabınızdaki kaynaklara yetkilendirilmiş erişim sağlar. Bir istemci depolama hesabındaki nesnelere süre ve belirtilen bir izin kümesi ile belirli bir süre için hesap erişim tuşlarınızı paylaşmak zorunda kalmadan sınırlı izinleri vermek sağlar. SAS depolama kaynağı için kimlik doğrulamalı erişim için gerekli tüm bilgileri kendi sorgu parametrelerini kapsayan bir URI değil. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun Oluşturucusu veya yöntem SAS geçirmek gerekir. SAS hakkında ayrıntılı bilgi için bkz: [paylaşılan erişim imzaları: SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory artık yalnızca destekler **hizmet SAS** ancak hesap SAS. Bkz: [türleri, paylaşılan erişim imzaları](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) bu iki tür ve nasıl oluşturulacağıyla ilgili ayrıntılar için. Azure Portalı'ndan generable SAS URL veya Depolama Gezgini desteklenmeyen bir hesap SAS, değil.
>

Hizmet SAS kimlik doğrulaması kullanmak için aşağıdaki özellikleri desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| sasUri | Blob, kapsayıcı ya da tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI belirtin. Bu alan SecureString işaretleyin. |Evet |
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

Oluştururken bir **SAS URI'sini**, aşağıdaki noktaları göz önünde bulunduruyor:

- Ayarlama uygun okuma/yazma **izinleri** (okuma, yazma, okuma/yazma) bağlantılı hizmet, veri fabrikası nasıl kullanıldığına göre nesneler üzerinde.
- Ayarlama **sona erme saati** uygun şekilde. Azure Storage nesnelere erişimi ardışık düzen etkin süresi içinde dolmaz emin olun.
- URI gereksinimleri temelinde sağ tablo düzeyinde oluşturulmalıdır.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure tablosu veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ Azure tablodan veri kopyalamak için veri kümesi için tür özelliği ayarlamak **AzureTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **AzureTable** |Evet |
| tableName |Bağlantılı hizmet başvurduğu Azure tablo veritabanı örneğinde tablonun adı. |Evet |

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

Azure Table gibi şemasız veri depoları için Data Factory hizmetinin şema aşağıdaki yollardan biriyle oluşturur:

1. Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin bu yapısı şema olarak geliştirir. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bir null değer için sağlanır.
2. Kullanarak verilerin yapısını belirtmezseniz **yapısı** Data Factory veri kümesi tanımı özelliğinde verileri ilk satırını kullanarak şema oluşturur. Bu durumda, ilk satırın tam şema içermiyorsa, bazı sütunları kopyalama işlemi sonucunda eksik.

Bu nedenle, şemasız veri kaynakları için en iyi uygulama verileri kullanarak yapısı belirtmektir **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure Table kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-table-as-source"></a>Azure tablo kaynağı olarak

Verileri Azure tablosundan kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzureTableSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **AzureTableSource** |Evet |
| azureTableSourceQuery |Verileri okumak için özel Azure Tablo sorgusu kullanın. Sonraki bölümdeki örneklere bakın. |Hayır |
| azureTableSourceIgnoreTableNotFound |Swallow tablosunun özel durum var olup olmadığını gösterir.<br/>İzin verilen değerler: **True**, ve **False** (varsayılan). |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri

Azure tablo sütunu dize türü ise:

```json
"azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', <datetime parameter>)"
```

Azure tablo sütunu datetime türü ise:

```json
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', <datetime parameter>, <datetime parameter>)"
```

### <a name="azure-table-as-sink"></a>Havuz olarak Azure tablo

Verileri Azure tablosundan kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzureTableSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **AzureTableSink** |Evet |
| azureTableDefaultPartitionKeyValue |Havuzu tarafından kullanılan varsayılan bölüm anahtar değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütun adını belirtin. Belirtilmezse, "AzureTableDefaultPartitionKeyValue" Bölüm anahtarı olarak kullanılır. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Hayır |
| azureTableInsertType |Azure tabloya veri ekleme modu. Bu özellik, var olan satırları eşleşen bölüm ve satır anahtarlarla çıkış tablosuna birleştirilmiş veya değiştirilmesi değerlerine sahip olup olmadığını denetler. <br/><br/>İzin verilen değerler: **birleştirme** (varsayılan), ve **Değiştir**. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır ve girdide var olmayan satır çıkış tablosuna hiçbiri seçeneği siler. Bu ayarları (birleştirme ve Değiştir) nasıl çalıştığı hakkında bilgi edinmek için [ekleme veya birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [ekleme veya değiştirme varlık](https://msdn.microsoft.com/library/azure/hh452242.aspx) Konular. |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı) |Hayır (varsayılan değeri: 10000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler.<br/>İzin verilen değerler: timespan. Örnek: "00: 20:00" (20 dakika) |Hayır (varsayılan olarak 90 saniye - depolama istemcinin varsayılan zaman aşımı) |

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

Bir kaynak sütun azureTablePartitionKeyName hedef sütun kullanmadan önce "Çeviricisi" özelliğini kullanarak bir hedef sütun eşleme.

Aşağıdaki örnekte, kaynak sütunu DivisionID hedef sütun DivisionID eşlenir.

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

Başlangıç/bitiş Azure Table veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Azure Table veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

Taşınırken veri & Azure tablosundan aşağıdaki [Azure tablo hizmeti tarafından tanımlanan eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure tablo OData türleri .NET türü ve tersi yönde kullanılır.

| Azure tablo veri türü | Veri Fabrikası geçici veri türü | Ayrıntılar |
|:--- |:--- |:--- |
| Edm.Binary |Byte] |Bir bayt dizisi en çok 64 KB. |
| Edm.Boolean |bool |Bir Boole değeri. |
| Edm.DateTime |Tarih saat |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen bir 64-bit değeri. Desteklenen tarih/saat aralığı 1 Ocak 1601 gece 12:00 gece ' başlar (C.E.) UTC. Aralık 9999 31 Aralık sona erer. |
| Edm.Double |Çift |Bir 64-bit kayan noktalı değeri. |
| Edm.Guid |GUID |128-bit bir genel benzersiz tanımlayıcısı. |
| Edm.Int32 |Int32 |Bir 32 bit tamsayı. |
| Edm.Int64 |Int64 |64 bitlik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlu değer. Dize değerlerini en çok 64 KB olabilir. |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).