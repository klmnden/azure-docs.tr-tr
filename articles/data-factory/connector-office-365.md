---
title: Azure Data Factory (Önizleme) kullanarak Office 365 veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Office 365'ten bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: jingwang
ms.openlocfilehash: 5d2d5948d817cbe80d00b74ef104ebaffcb511fb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995821"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory-preview"></a>Azure Data Factory (Önizleme) kullanarak Azure'da Office 365'ten veri kopyalama 

Azure Data Factory, Office 365'te kurumsal veri ölçeklenebilir bir şekilde Azure'a Kiracı ve analiz uygulamaları oluşturun ve bu değerli veri varlıklarını üzerinde göre içgörü zengin getirmenize olanak sağlar. Privileged Access Management ile tümleştirme için değerli seçkin verileri Office 365'te güvenli erişim denetimi sağlar.  Microsoft Graph veriler üzerinde daha fazla bilgi için Bağlan, lütfen [bu bağlantıyı](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki).

Bu makalede, kopyalama etkinliği Azure Data Factory'de Office 365'ten veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Şimdilik, tek bir kopyalama etkinliği içinde yalnızca yapabilecekleriniz **Office 365'ten veri kopyalama [Azure Blob Depolama](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), ve [Azure Data Lake depolama 2. nesil ](connector-azure-data-lake-storage.md) JSON biçiminde** (setOfObjects yazın). Office 365 veri depolarının veya başka biçimlerde diğer türleri yüklemek istiyorsanız, daha fazla veri hiçbirine yüklemek için sonraki kopyalama etkinliği ile ilk kopyalama etkinliği zincirleyebilirsiniz [desteklenen ADF hedef depoları](copy-activity-overview.md#supported-data-stores-and-formats) (bakın" bir havuz olarak desteklenen"tablosundaki"desteklenen veri depoları ve biçimler").

>[!IMPORTANT]
>- Veri fabrikasını ve havuz veri deposu içeren Azure aboneliğinin, Office 365 kiracısı olarak aynı Azure Active Directory (Azure AD) kiracısı altında olması gerekir.
>- Kopyalama etkinliği için kullanılan Azure Integration Runtime bölge sağlamak hem de Office 365 Kiracı kullanıcının posta kutusuna bulunduğu bölgede hedefi olan. Başvuru [burada](concepts-integration-runtime.md#integration-runtime-location) Azure IR konumu nasıl belirlendiğini öğrenmek. Başvurmak [burada tablo](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#data-regions) desteklenen Office bölgeleri ve karşılık gelen Azure bölgelerinin listesi için.
>-  Office 365 verilerini yüklüyorsanız **Azure Blob Depolama** hedef olarak kullandığınızdan emin olun **[hizmet sorumlusu kimlik doğrulaması](connector-azure-blob-storage.md#service-principal-authentication)** bağlı tanımlarken Azure Blob depolama alanına hizmet ve kullanmayan [hesap anahtarı](connector-azure-blob-storage.md#account-key-authentication), [paylaşılan erişim imzası](connector-azure-blob-storage.md#shared-access-signature-authentication) veya [kimliklerini Azure kaynakları için yönetilen](connector-azure-blob-storage.md#managed-identity) kimlik doğrulamaları.
>-  Office 365 verilerini yüklüyorsanız **Azure Data Lake depolama Gen1** hedef olarak kullandığınızdan emin olun [ **hizmet sorumlusu kimlik doğrulaması** ](connector-azure-data-lake-store.md#use-service-principal-authentication) tanımlarken Azure Data Lake depolama Gen1 ve kullanmayan bağlı hizmeti [yönetilen Azure kaynaklarında kimlik doğrulaması için kimlik](connector-azure-data-lake-store.md#managed-identity).

## <a name="prerequisites"></a>Önkoşullar

Azure'da Office 365'ten veri kopyalamak için aşağıdaki önkoşul adımlarını tamamlamanız gerekir:

- Office 365 Kiracı yöneticiniz kolaylaşmasına eylemleri açıklandığı gibi tamamlayın [burada](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/On-boarding).
- Oluşturun ve Azure Active Directory'de bir Azure AD web uygulaması yapılandırın.  Yönergeler için [bir Azure AD uygulaması oluştur](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Office 365 için bağlı hizmetini tanımlamak için kullanacağınız aşağıdaki değerleri not edin:
    - Kiracı kimliği Yönergeler için [Kiracı kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    - Uygulama kimliği ve uygulama anahtarı.  Yönergeler için [uygulama kimliği ve kimlik doğrulama anahtarını Al](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key).
- Azure AD web uygulaması sahibi olarak veri erişim isteği yapacak kullanıcı kimliğini ekleyin (Azure AD web uygulaması > Ayarlar > sahipleri > Ekle sahibi). 
    - Kullanıcı kimliği, içinden verileri alma ve Konuk kullanıcı olmamalıdır Office 365 kuruluşta olması gerekir.

## <a name="approving-new-data-access-requests"></a>Onaylama yeni veri erişim istekleri

Bu ilk kez kullanıyorsanız, bu bağlam için verileri isteyen (veri erişimi tablo olmasından, hangi hedef hesap, içine yüklenen verilerdir ve hangi kullanıcı kimliğini veri yapıyor birlikte erişim isteği), kopyalama etkinliği görürsünüz. durumu "Sürüyor" olarak ve yalnızca içine tıkladığınızda ["Details" bağlantı Eylemler altında](copy-activity-overview.md#monitoring) "RequestingConsent" durumu görürsünüz.  Veri erişim onaylayan grubunun bir üyesi, Privileged Access Management istekte veri ayıklama devam etmeden önce onaylamanız gerekir.

Başvuran [burada](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Approving-data-access-requests) onaylayan nasıl onaylayabilirsiniz veri erişim isteği ve başvuru [burada](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#integration-with-privileged-access-management) Privileged Access Management genel tümleştirme bir açıklama için verileri ayarlama dahil olmak üzere erişim onaylayan grubu.

## <a name="policy-validation"></a>İlke doğrulama

ADF yönetilen bir uygulamanın bir parçası olarak oluşturulur ve yönetim kaynak grubundaki kaynaklar üzerinde yapılan Azure ilkeleri atamaları, her kopyalama etkinliği çalıştırma, için ilke atamaları uygulandığından emin olmak için ADF denetler. Başvuru [burada](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#policies) desteklenen ilkeleri listesi.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Office 365 Bağlayıcısı kullanarak bir kılavuz için bkz. [Office 365'ten veri yükleme](load-office-365-data.md) makalesi.

SDK'ları ve aşağıdaki araçlardan birini kullanarak kopyalama etkinlikli bir işlem hattı oluşturabilirsiniz. Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler içeren bir öğreticiye gitmek için bir bağlantıyı seçin. 

- [Azure portal](quickstart-create-data-factory-portal.md)
- [.NET SDK](quickstart-create-data-factory-dot-net.md)
- [Python SDK'sı](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [REST API](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager şablonu](quickstart-create-data-factory-resource-manager-template.md). 

Aşağıdaki bölümler, Data Factory varlıklarını belirli Office 365 Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Office 365 bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Office365** | Evet |
| office365TenantId | Office 365 hesabına ait olduğu azure Kiracı kimliği. | Evet |
| servicePrincipalTenantId | Azure AD web uygulamanızı altında bulunduğu Kiracı bilgilerini belirtin. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak Integration Runtime.  Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır |

>[!NOTE]
> Arasındaki fark **office365TenantId** ve **servicePrincipalTenantId** ve buna karşılık gelen bir değer sağlamak için:
>- Ardından aynı kiracıyı sunmalıdır kendi kuruluşunuzun kullanımına yönelik Office 365 verilerine karşı uygulama geliştirme, kuruluşunuzun AAD Kiracı kimliği her iki özellik için bir kurumsal geliştiriciyseniz, kimliği
>- Müşterinizin office365TenantId sonra müşterileriniz için uygulama geliştirme bir ISV geliştiriciyseniz (uygulama Yükleyici), AAD Kiracı kimliği ve servicePrincipalTenantId şirketinizin AAD Kiracı kimliği olur

**Örnek:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Office 365 veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Office 365'ten veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **Office365Table** | Evet |
| tableName | Office 365'ten ayıklamak için veri kümesinin adı. Başvuru [burada](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#datasets) Office 365 veri kümeleri için ayıklama kullanılabilir listesi. | Evet |
| Karşılaştırma | Office 365'ten ayıklamak için belirli satırları filtrelemek için kullanılan bir koşul ifadesi.  Başvuru [burada](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#filters) hangi sütunların her bir tabloyu ve filtre ifade biçimi için koşul filtreleme için kullanılan çıkış bulunacak. | Hayır<br>(Hiçbir koşul sağlanırsa, son 30 güne ait verileri ayıklamak için varsayılandır) |

**Örnek**

```json
{
    "name": "Office365Table1",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "ReceivedDateTime",
                "type": "datetime"
            },
            {
                "name": "SentDateTime",
                "type": "datetime"
            },
            {
                "name": "HasAttachments",
                "type": "boolean"
            },
            {
                "name": "InternetMessageId",
                "type": "string"
            },
            {
                "name": "Subject",
                "type": "string"
            },
            {
                "name": "Importance",
                "type": "string"
            },
            {
                "name": "ParentFolderId",
                "type": "string"
            },
            {
                "name": "Sender",
                "type": "string"
            },
            {
                "name": "From",
                "type": "string"
            },
            {
                "name": "ToRecipients",
                "type": "string"
            },
            {
                "name": "CcRecipients",
                "type": "string"
            },
            {
                "name": "BccRecipients",
                "type": "string"
            },
            {
                "name": "ReplyTo",
                "type": "string"
            },
            {
                "name": "ConversationId",
                "type": "string"
            },
            {
                "name": "UniqueBody",
                "type": "string"
            },
            {
                "name": "IsDeliveryReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsReadReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsRead",
                "type": "boolean"
            },
            {
                "name": "IsDraft",
                "type": "boolean"
            },
            {
                "name": "WebLink",
                "type": "string"
            },
            {
                "name": "CreatedDateTime",
                "type": "datetime"
            },
            {
                "name": "LastModifiedDateTime",
                "type": "datetime"
            },
            {
                "name": "ChangeKey",
                "type": "string"
            },
            {
                "name": "Categories",
                "type": "string"
            },
            {
                "name": "Id",
                "type": "string"
            },
            {
                "name": "Attachments",
                "type": "string"
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Contact_v0",
            "predicate": "CreatedDateTime >= 2018-07-11T16:00:00.000Z AND CreatedDateTime <= 2018-07-18T16:00:00.000Z"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Office 365 kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="office-365-as-source"></a>Kaynak olarak Office 365

Office 365'ten veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **Office365Source**. Kopyalama etkinliği desteklenen herhangi bir ek özellik **kaynak** bölümü.

**Örnek:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
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
                "type": "Office365Source"
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
