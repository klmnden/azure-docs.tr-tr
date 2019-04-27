---
title: Data Factory kullanarak Azure tablo depolama ve veri kopyalamak | Microsoft Docs
description: Data Factory kullanarak Azure tablo depolama için desteklenen kaynak depolarını veya tablo depolama desteklenen havuz depolarına veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: 7ef8f80f44c921cc1f2524351c8acb78ebd713bf
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57434802"
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure tablo depolama ve veri kopyalamak
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-table-connector.md)
> * [Geçerli sürüm](connector-azure-table-storage.md)

Bu makalede, kopyalama etkinliği Azure Data Factory ve Azure tablo Depolama'dan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen kaynak veri deposundan tablo Depolama'ya veri kopyalayabilirsiniz. Ayrıca, tablo Depolama'yı desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Paylaşılan erişim imzası kimlik doğrulamaları özellikle hesap anahtarı ve hizmetini kullanarak veri kopyalama bu Azure tablo bağlayıcısını destekler.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, tablo depolama için belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

### <a name="use-an-account-key"></a>Bir hesabı anahtarını kullanın

Hesap anahtarı kullanarak bir Azure depolama bağlı hizmeti oluşturabilirsiniz. Data factory depolama ile küresel erişim sağlar. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureTableStorage**. |Evet |
| connectionString | ConnectionString özelliği için depolama alanına bağlanmak için gereken bilgileri belirtin. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Hesap anahtarı Azure Key Vault ve çekme koyabilirsiniz `accountKey` yapılandırma bağlantı dizesini dışında. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" tür bağlı hizmeti kullanıyorsanız, hala olarak desteklenmektedir-olduğu sırada bu yeni "AzureTableStorage" kullanmak için bağlı hizmet türü ileriye dönük önerilir.

**Örnek:**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
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

**Örnek: hesap anahtarı Azure Key Vault'ta depolama**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;"
            },
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması kullanma

Bir depolama bağlı hizmeti, paylaşılan erişim imzası kullanarak da oluşturabilirsiniz. Data factory ile depolama tüm/belirli kaynaklara erişim kısıtlanmış/zamana bağlı sağlar.

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir istemci belirli bir süre için ve belirli bir izin kümesi ile sınırlı depolama hesabınızdaki nesnelere izinleri vermek için kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası için depolama kaynak kimliği doğrulanmış erişim için gerekli tüm bilgileri sorgu parametrelerini kapsayan bir URI'dir. Paylaşılan erişim imzası ile depolama kaynaklarına erişmek için istemci yalnızca uygun oluşturucu veya yöntemi için paylaşılan erişim imzasını geçmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları: Paylaşılan erişim imzası modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Data Factory artık destekler hem de **hizmet paylaşılan erişim imzaları** ve **hesabı paylaşılan erişim imzaları**. Bu iki tür ve bunları oluşturmak nasıl hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları türleri](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). 

> [!TIP]
> Depolama hesabınız için hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutlarını yürütebilirsiniz. Yer tutucuları değiştirmeniz ve gerekli izni verin.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Paylaşılan erişim imzası kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureTableStorage**. |Evet |
| sasUri | Paylaşılan erişim imzası URI'si tablo için SAS URI'sini belirtin. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Ayrıca, otomatik döndürme yararlanın ve belirteç bölümünü kaldırmak için Azure Key Vault'ta SAS belirteci koyabilirsiniz. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanının kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" tür bağlı hizmeti kullanıyorsanız, hala olarak desteklenmektedir-olduğu sırada bu yeni "AzureTableStorage" kullanmak için bağlı hizmet türü ileriye dönük önerilir.

**Örnek:**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<account>.table.core.windows.net/<table>?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: hesap anahtarı Azure Key Vault'ta depolama**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<account>.table.core.windows.net/<table>>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI'si oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Uygun okuma/yazma izinleri (okuma, yazma, okuma/yazma) bağlı hizmet, data factory'de nasıl kullanıldığına göre nesneler üzerinde ayarlayın.
- Ayarlama **süre sonu** uygun şekilde. İşlem hattının etkin dönemini içinde depolama nesnelerine erişim süresi sona ermiyor emin olun.
- URI, ihtiyaca göre sağ tablo düzeyinde oluşturulmalıdır.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Azure tablosu veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure tablosu ve veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzureTable**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **AzureTable**. |Evet |
| tableName |Bağlı hizmetini ifade eder tablo Depolama veritabanı tablosunun adı. |Evet |

**Örnek:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Table storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması

Azure tablo gibi şemasız veri depoları için veri fabrikası şemayı aşağıdaki yollardan biriyle algılar:

* Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği Data Factory veri kümesi tanımında, bu yapı şema olarak geliştirir. Bu durumda, bir satır bir sütun için bir değer içermiyorsa null değeri için sağlanır.
* Kullanarak verilerin yapısını belirtmezseniz **yapısı** özelliği, Data Factory veri kümesi tanımında, verilerin ilk satırı kullanarak şemayı algılar. Bu durumda, ilk satır, tam şema içermiyorsa, bazı sütunları kopyalama işleminin sonucunda eksik.

Şemasız veri kaynakları için veri yapısını kullanarak belirtmek için en iyi yöntem olacaktır. **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure tablo kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-table-as-a-source-type"></a>Azure tablo olarak bir kaynak türü

Verileri Azure tablosundan kopyalamak için kopyalama etkinliği kaynak türü ayarlayın. **AzureTableSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **AzureTableSource**. |Evet |
| azureTableSourceQuery |Verileri okumak için özel tablo depolama sorgu kullanın. Aşağıdaki bölümdeki örneklere bakın. |Hayır |
| azureTableSourceIgnoreTableNotFound |Özel durum yok tablo izin verilip verilmeyeceğini belirtir.<br/>İzin verilen değerler **True** ve **False** (varsayılan). |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri

Azure tablo sütun datetime türü ise:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Azure tablo sütun dize türünde ise:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

İşlem hattı parametresini kullanırsanız, önceki örneklere göre doğru biçimde datetime değerine dönüştürün.

### <a name="azure-table-as-a-sink-type"></a>Bir havuz türü olarak Azure tablosu

Azure tablo için verileri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **AzureTableSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **AzureTableSink**. |Evet |
| azureTableDefaultPartitionKeyValue |Havuz tarafından kullanılan varsayılan bölüm anahtarı değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, "AzureTableDefaultPartitionKeyValue" Bölüm anahtarı olarak kullanılır. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Hayır |
| azureTableInsertType |Azure tabloya veri eklemek için modu. Bu özellik, var olan satır bölüm ve satır anahtarları eşleşen çıktı tablosu değiştirildi veya birleştirilmiş değerlerine sahip olup olmadığını denetler. <br/><br/>İzin verilen değerler **birleştirme** (varsayılan) ve **değiştirin**. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır. Her iki seçeneği giriş bulunmayan çıkış tablosundaki satırları siler. Birleştirme ve Değiştir ayarları nasıl çalıştığı hakkında bilgi edinmek için [Ekle ya da birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [eklemek veya varlık yerine](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler.<br/>İzin verilen değerler şunlardır: tamsayı (satır sayısı). |Hayır (varsayılan: 10.000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler.<br/>Zaman aralığı izin verilen değerler. Bir örnek verilmiştir "00: 20:00" (20 dakika). |Hayır (varsayılan değer 90 saniye cinsinden depolama istemcinin varsayılan zaman aşımı) |

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

Kullanarak bir kaynak sütun için bir hedef sütun eşleme **"translator"** hedef sütunun azureTablePartitionKeyName kullanabilmeniz için önce özelliği.

Aşağıdaki örnekte, kaynak sütunu DivisionID hedef sütunu DivisionID eşlenir:

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

## <a name="data-type-mapping-for-azure-table"></a>Azure tablosu için veri türü eşlemesi

Gelen ve Azure tablo verileri kopyaladığınızda, aşağıdaki eşlemeler Azure tablo veri türleri arasından veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemelerini nasıl hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Ne zaman, veri taşıma Azure tablosundan aşağıdaki [Azure Table tarafından tanımlanan eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) .NET türüne ve Azure tablo OData türlerinden kullanılır.

| Azure tablo veri türü | Veri Fabrikası geçici veri türü | Ayrıntılar |
|:--- |:--- |:--- |
| Edm.Binary |bayt] |Bir bayt dizisi en fazla 64 KB. |
| Edm.Boolean |bool |Bir Boole değeri. |
| Edm.DateTime |DateTime |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen bir 64-bit değeri. Desteklenen tarih/saat aralığı 1 Ocak 1601 M.S. gece yarısı başlar. (C.E.), UTC. Aralığın 31 Aralık 9999 sonlandırır. |
| Edm.Double |double |Bir 64-bit kayan nokta değeri. |
| Edm.Guid |Guid |128 bit genel benzersiz tanımlayıcı. |
| Edm.Int32 |Int32 |Bir 32 bit tamsayı. |
| Edm.Int64 |Int64 |Bir 64-bit tamsayı. |
| Edm.String |String |UTF-16 kodlu bir değer. Dize değerleri, en fazla 64 KB olabilir. |

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
