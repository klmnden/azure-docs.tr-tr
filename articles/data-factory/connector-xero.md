---
title: Azure Data Factory (Önizleme) ile Xero verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Xero bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: 6793fbcc50711e10231b87fa6e1f11f54f90d325
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445443"
---
# <a name="copy-data-from-xero-using-azure-data-factory-preview"></a>Azure Data Factory (Önizleme) ile Xero veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Xero veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirim sağlayın. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Xero tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Xero bağlayıcı'yı destekler:

- Xero [özel uygulama](https://developer.xero.com/documentation/getting-started/api-application-types) ancak ortak uygulama.
- "Rapor" hariç tüm Xero tabloların (API uç noktaları). 

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Xero bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Xero bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Xero** | Evet |
| host | Xero sunucu uç noktası (`api.xero.com`).  | Evet |
| consumerKey | Xero uygulamayla ilişkili tüketici anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| privateKey | Özel anahtar, Xero özel uygulama için oluşturulan .pem dosyasından bkz [bir ortak/özel anahtar çifti oluşturma](https://developer.xero.com/documentation/api-guides/create-publicprivate-key). Not **numbits 512 ile privatekey.pem oluşturmak** kullanarak `openssl genrsa -out privatekey.pem 512`; 1024 desteklenmiyor. .Pem dosyasından UNIX satırı endings(\n) dahil olmak üzere tüm metni eklemek, aşağıdaki örneğe bakın.<br/><br/>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucu ana bilgisayar adını eşleştirmek için ana bilgisayar adı sunucunun sertifikasında gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "host" : "api.xero.com",
            "consumerKey": {
                 "type": "SecureString",
                 "value": "<consumerKey>"
            },
            "privateKey": {
                 "type": "SecureString",
                 "value": "<privateKey>"
            }
        }
    }
}
```

**Örnek özel anahtar değeri:**

.Pem dosyasını UNIX satırı endings(\n) dahil olmak üzere tüm metni içerir.

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Xero veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Xero verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **XeroObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **XeroObject** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Xero kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="xero-as-source"></a>Xero kaynağı olarak

Xero verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **XeroSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **XeroSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Contacts"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Xero sorguyu oluştururken aşağıdakileri unutmayın:

- Karmaşık öğeleri içeren tablolar için birden çok tablo bölünür. Örneğin, banka işlemleri sahip bir karmaşık veri yapısı "Lineıtems" banka işlem verilerini eşlenmiş şekilde tablosuna `Bank_Transaction` ve `Bank_Transaction_Line_Items`, ile `Bank_Transaction_ID` birbirine bağlamak için yabancı anahtar olarak.

- Xero veri iki şemaları kullanılabilir: `Minimal` (varsayılan) ve `Complete`. İstenen sorgu yapmadan önce ek veriler (örn: sütun kimliği) gerektiren önkoşul çağrı tablolar tam şema var.

Aşağıdaki tablolarda, aynı bilgileri en az ve tam şemada vardır. API çağrılarının sayısını azaltmak için en az bir şema (varsayılan) kullanın.

- Bank_Transactions
- Contact_Groups 
- Kişiler 
- Contacts_Sales_Tracking_Categories 
- Contacts_Phones 
- Contacts_Addresses 
- Contacts_Purchases_Tracking_Categories 
- Credit_Notes 
- Credit_Notes_Allocations 
- Expense_Claims 
- Expense_Claim_Validation_Errors
- Faturalar 
- Invoices_Credit_Notes
- Invoices_ ödemeler 
- Invoices_Overpayments 
- Manual_Journals 
- Fazla ödeme 
- Overpayments_Allocations 
- Ödemeler 
- Prepayments_Allocations 
- Giriş 
- Receipt_Validation_Errors 
- Tracking_Categories

Aşağıdaki tablolarda, yalnızca tam şemasıyla sorgulanabilir:

- Complete.Bank_Transaction_Line_Items 
- Complete.Bank_Transaction_Line_Item_Tracking 
- Complete.Contact_Group_Contacts 
- Complete.Contacts_Contact_ kişiler 
- Complete.Credit_Note_Line_Items 
- Complete.Credit_Notes_Line_Items_Tracking 
- Complete.Expense_Claim_ ödemeler 
- Complete.Expense_Claim_Receipts 
- Complete.Invoice_Line_Items 
- Complete.Invoices_Line_Items_Tracking
- Complete.Manual_Journal_Lines 
- Complete.Manual_Journal_Line_Tracking 
- Complete.Overpayment_Line_Items 
- Complete.Overpayment_Line_Items_Tracking 
- Complete.Prepayment_Line_Items 
- Complete.Prepayment_Line_Item_Tracking 
- Complete.Receipt_Line_Items 
- Complete.Receipt_Line_Item_Tracking 
- Complete.Tracking_Category_Options

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
