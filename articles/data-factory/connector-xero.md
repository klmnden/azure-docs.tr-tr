---
title: Azure Data Factory (Beta) kullanarak Xero veri kopyalama | Microsoft Docs
description: "Desteklenen havuz veri depolarına Xero bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin."
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
ms.date: 02/12/2018
ms.author: jingwang
ms.openlocfilehash: 458ad702b510c0fd01ab63541b2026b8a9a06e91
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="copy-data-from-xero-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak Xero verilerini

Bu makalede kopya etkinliği Azure Data Factory'de Xero verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve geri bildirim sağlayın. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Xero veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Xero bağlayıcı destekler:

- Xero [özel uygulama](https://developer.xero.com/documentation/getting-started/api-application-types) ancak ortak değil uygulama.
- "Rapor" dışındaki tüm Xero tabloları (API uç noktaları). 

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Xero bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Xero bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Xero** | Evet |
| konak | Xero sunucusu uç noktası (`api.xero.com`).  | Evet |
| consumerKey | Xero uygulama ile ilişkili tüketici anahtarı. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| privateKey | Özel anahtar, Xero özel uygulamanız için oluşturulan .pem dosyasından bkz [ortak/özel anahtar çifti oluşturma](https://developer.xero.com/documentation/api-guides/create-publicprivate-key). .Pem dosyasını UNIX satır endings(\n) dahil olmak üzere tüm metni içerir, aşağıdaki örneğe bakın.<br/>Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için ana bilgisayar adı sunucunun sertifikasında gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucusunun kimliğini doğrulamak belirtir. Varsayılan değer true olur.  | Hayır |

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

.Pem dosyasını UNIX satır endings(\n) dahil olmak üzere tüm metni içerir.

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Xero veri kümesi tarafından desteklenen özellikler listesini sağlar.

Xero verileri kopyalamak için kümesine tür özelliği ayarlamak **XeroObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Xero kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="xero-as-source"></a>Kaynak olarak Xero

Xero verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **XeroSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **XeroSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Contacts"`. | Evet |

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

Xero sorgu belirtirken aşağıdakileri unutmayın:

- Karmaşık öğeler sahip tablolar için birden çok tablo bölünür. Örneğin, banka işlemleri sahip bir karmaşık veri yapısı "LineItems" veri banka hareket eşlenmiş şekilde tabloya `Bank_Transaction` ve `Bank_Transaction_Line_Items`, ile `Bank_Transaction_ID` birbirine bağlamak için yabancı anahtar olarak.

- Xero veri iki şemaları kullanılabilir: `Minimal` (varsayılan) ve `Complete`. Tam şeması, istenen sorgu yapmadan önce ek veriler (örneğin kimliği sütun) gerektiren önkoşul arama tabloları içerir.

Aşağıdaki tablolarda, aynı bilgilerin Minimal ve tam şemada vardır. API çağrılarının sayısını azaltmak için en az şeması (varsayılan) kullanın.

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
- Complete.Contacts_Contact_ kişi 
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
Kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
