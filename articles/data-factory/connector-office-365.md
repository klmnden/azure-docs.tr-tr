---
title: Office 365 Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
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
ms.date: 05/07/2019
ms.author: jingwang
ms.openlocfilehash: 9ca3cbb1ef46c7fe53b6b16bda40ebef245613f3
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415649"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure'a Office 365'ten veri kopyalama

Azure Data Factory, Office 365'te kurumsal veri ölçeklenebilir bir şekilde Azure'a Kiracı ve analiz uygulamaları oluşturun ve bu değerli veri varlıklarını üzerinde göre içgörü zengin getirmenize olanak sağlar. Privileged Access Management ile tümleştirme için değerli seçkin verileri Office 365'te güvenli erişim denetimi sağlar.  Microsoft Graph veriler üzerinde daha fazla bilgi için Bağlan, lütfen [bu bağlantıyı](https://docs.microsoft.com/graph/data-connect-concept-overview).

Bu makalede, kopyalama etkinliği Azure Data Factory'de Office 365'ten veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Şimdilik, tek bir kopyalama etkinliği içinde yalnızca yapabilecekleriniz **Office 365'ten veri kopyalama [Azure Blob Depolama](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), ve [Azure Data Lake depolama 2. nesil ](connector-azure-data-lake-storage.md) JSON biçiminde** (setOfObjects yazın). Office 365 veri depolarının veya başka biçimlerde diğer türleri yüklemek istiyorsanız, daha fazla veri hiçbirine yüklemek için sonraki kopyalama etkinliği ile ilk kopyalama etkinliği zincirleyebilirsiniz [desteklenen ADF hedef depoları](copy-activity-overview.md#supported-data-stores-and-formats) (bakın" bir havuz olarak desteklenen"tablosundaki"desteklenen veri depoları ve biçimler").

>[!IMPORTANT]
>- Veri fabrikasını ve havuz veri deposu içeren Azure aboneliğinin, Office 365 kiracısı olarak aynı Azure Active Directory (Azure AD) kiracısı altında olması gerekir.
>- Kopyalama etkinliği için kullanılan Azure Integration Runtime bölge sağlamak hem de Office 365 Kiracı kullanıcının posta kutusuna bulunduğu bölgede hedefi olan. Başvuru [burada](concepts-integration-runtime.md#integration-runtime-location) Azure IR konumu nasıl belirlendiğini öğrenmek. Başvurmak [burada tablo](https://docs.microsoft.com/graph/data-connect-datasets#regions) desteklenen Office bölgeleri ve karşılık gelen Azure bölgelerinin listesi için.
>- Hizmet sorumlusu kimlik doğrulaması, hedef deposu olarak Azure Blob Depolama, Azure Data Lake depolama Gen1 ve Azure Data Lake depolama Gen2'ye desteklenen yalnızca kimlik doğrulama mekanizmasıdır.

## <a name="prerequisites"></a>Önkoşullar

Azure'da Office 365'ten veri kopyalamak için aşağıdaki önkoşul adımlarını tamamlamanız gerekir:

- Office 365 Kiracı yöneticiniz kolaylaşmasına eylemleri açıklandığı gibi tamamlayın [burada](https://docs.microsoft.com/graph/data-connect-get-started).
- Oluşturun ve Azure Active Directory'de bir Azure AD web uygulaması yapılandırın.  Yönergeler için [bir Azure AD uygulaması oluştur](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Office 365 için bağlı hizmetini tanımlamak için kullanacağınız aşağıdaki değerleri not edin:
    - Kiracı kimliği Yönergeler için [Kiracı kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    - Uygulama kimliği ve uygulama anahtarı.  Yönergeler için [uygulama kimliği ve kimlik doğrulama anahtarını Al](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key).
- Azure AD web uygulaması sahibi olarak veri erişim isteği yapacak kullanıcı kimliğini ekleyin (Azure AD web uygulaması > Ayarlar > sahipleri > Ekle sahibi). 
    - Kullanıcı kimliği, içinden verileri alma ve Konuk kullanıcı olmamalıdır Office 365 kuruluşta olması gerekir.

## <a name="approving-new-data-access-requests"></a>Onaylama yeni veri erişim istekleri

Bu ilk kez kullanıyorsanız, bu bağlam için verileri isteyen (veri erişimi tablo olmasından, hangi hedef hesap, içine yüklenen verilerdir ve hangi kullanıcı kimliğini veri yapıyor birlikte erişim isteği), kopyalama etkinliği görürsünüz. durumu "Sürüyor" olarak ve yalnızca içine tıkladığınızda ["Details" bağlantı Eylemler altında](copy-activity-overview.md#monitoring) "RequestingConsent" durumu görürsünüz.  Veri erişim onaylayan grubunun bir üyesi, Privileged Access Management istekte veri ayıklama devam etmeden önce onaylamanız gerekir.

Başvuran [burada](https://docs.microsoft.com/graph/data-connect-tips#approve-pam-requests-via-office-365-admin-portal) onaylayan nasıl onaylayabilirsiniz veri erişim isteği ve başvuru [burada](https://docs.microsoft.com/graph/data-connect-pam) Privileged Access Management genel tümleştirme bir açıklama için verileri ayarlama dahil olmak üzere erişim onaylayan grubu.

## <a name="policy-validation"></a>İlke doğrulama

ADF yönetilen bir uygulamanın bir parçası olarak oluşturulur ve yönetim kaynak grubundaki kaynaklar üzerinde yapılan Azure ilkeleri atamaları, her kopyalama etkinliği çalıştırma, için ilke atamaları uygulandığından emin olmak için ADF denetler. Başvuru [burada](https://docs.microsoft.com/graph/data-connect-policies#policies) desteklenen ilkeleri listesi.

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
| tableName | Office 365'ten ayıklamak için veri kümesinin adı. Başvuru [burada](https://docs.microsoft.com/graph/data-connect-datasets#datasets) Office 365 veri kümeleri için ayıklama kullanılabilir listesi. | Evet |
| allowedGroups | Grup Seçimi koşul.  Kendisi için veriler alınır en fazla 10 kullanıcı gruplarını seçmek için bu özelliği kullanın.  Grup yok belirtilirse, tüm kuruluş için veri döndürülür. | Hayır |
| userScopeFilterUri | Zaman `allowedGroups` özelliği belirtilmedi, Office 365'ten ayıklamak için belirli satırları filtrelemek için tüm Kiracı üzerinde uygulanan bir koşul ifadesi kullanabilirsiniz. Koşul biçimi Microsoft Graph API'leri, sorgu biçiminin örn eşleşmelidir `https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'`. | Hayır |
| dateFilterColumn | Tarih/saat filtre sütunu adı. Hangi Office 365 için veri ayıklanan zaman aralığı sınırlamak için bu özelliği kullanın. | Veri kümesi bir veya daha fazla DateTime sütunları varsa, Evet. Başvuru [burada](https://docs.microsoft.com/graph/data-connect-filtering#filtering) bu tarih/saat filtre gerektiren veri kümeleri listesi. |
| startTime | Filtrelenecek tarih saat değeri başlatın. | Yanıt Evet ise `dateFilterColumn` belirtilir |
| endTime | Filtrelenecek tarih saat değeri son. | Yanıt Evet ise `dateFilterColumn` belirtilir |

**Örnek**

```json
{
    "name": "DS_May2019_O365_Message",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "Id",
                "type": "String",
                "description": "The unique identifier of the event."
            },
            {
                "name": "CreatedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was created."
            },
            {
                "name": "LastModifiedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was last modified."
            },
            {
                "name": "ChangeKey",
                "type": "String",
                "description": "Identifies the version of the event object. Every time the event is changed, ChangeKey changes as well. This allows Exchange to apply changes to the correct version of the object."
            },
            {
                "name": "Categories",
                "type": "String",
                "description": "The categories associated with the event. Format: ARRAY<STRING>"
            },
            {
                "name": "OriginalStartTimeZone",
                "type": "String",
                "description": "The start time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "OriginalEndTimeZone",
                "type": "String",
                "description": "The end time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "ResponseStatus",
                "type": "String",
                "description": "Indicates the type of response sent in response to an event message. Format: STRUCT<Response: STRING, Time: STRING>"
            },
            {
                "name": "iCalUId",
                "type": "String",
                "description": "A unique identifier that is shared by all instances of an event across different calendars."
            },
            {
                "name": "ReminderMinutesBeforeStart",
                "type": "Int32",
                "description": "The number of minutes before the event start time that the reminder alert occurs."
            },
            {
                "name": "IsReminderOn",
                "type": "Boolean",
                "description": "Set to true if an alert is set to remind the user of the event."
            },
            {
                "name": "HasAttachments",
                "type": "Boolean",
                "description": "Set to true if the event has attachments."
            },
            {
                "name": "Subject",
                "type": "String",
                "description": "The text of the event's subject line."
            },
            {
                "name": "Body",
                "type": "String",
                "description": "The body of the message associated with the event.Format: STRUCT<ContentType: STRING, Content: STRING>"
            },
            {
                "name": "Importance",
                "type": "String",
                "description": "The importance of the event: Low, Normal, High."
            },
            {
                "name": "Sensitivity",
                "type": "String",
                "description": "Indicates the level of privacy for the event: Normal, Personal, Private, Confidential."
            },
            {
                "name": "Start",
                "type": "String",
                "description": "The start time of the event. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "End",
                "type": "String",
                "description": "The date and time that the event ends. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "Location",
                "type": "String",
                "description": "Location information of the event. Format: STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>>"
            },
            {
                "name": "IsAllDay",
                "type": "Boolean",
                "description": "Set to true if the event lasts all day. Adjusting this property requires adjusting the Start and End properties of the event as well."
            },
            {
                "name": "IsCancelled",
                "type": "Boolean",
                "description": "Set to true if the event has been canceled."
            },
            {
                "name": "IsOrganizer",
                "type": "Boolean",
                "description": "Set to true if the message sender is also the organizer."
            },
            {
                "name": "Recurrence",
                "type": "String",
                "description": "The recurrence pattern for the event. Format: STRUCT<Pattern: STRUCT<Type: STRING, `Interval`: INT, Month: INT, DayOfMonth: INT, DaysOfWeek: ARRAY<STRING>, FirstDayOfWeek: STRING, Index: STRING>, `Range`: STRUCT<Type: STRING, StartDate: STRING, EndDate: STRING, RecurrenceTimeZone: STRING, NumberOfOccurrences: INT>>"
            },
            {
                "name": "ResponseRequested",
                "type": "Boolean",
                "description": "Set to true if the sender would like a response when the event is accepted or declined."
            },
            {
                "name": "ShowAs",
                "type": "String",
                "description": "The status to show: Free, Tentative, Busy, Oof, WorkingElsewhere, Unknown."
            },
            {
                "name": "Type",
                "type": "String",
                "description": "The event type: SingleInstance, Occurrence, Exception, SeriesMaster."
            },
            {
                "name": "Attendees",
                "type": "String",
                "description": "The collection of attendees for the event. Format: ARRAY<STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>, Status: STRUCT<Response: STRING, Time: STRING>, Type: STRING>>"
            },
            {
                "name": "Organizer",
                "type": "String",
                "description": "The organizer of the event. Format: STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>>"
            },
            {
                "name": "WebLink",
                "type": "String",
                "description": "The URL to open the event in Outlook Web App."
            },
            {
                "name": "Attachments",
                "type": "String",
                "description": "The FileAttachment and ItemAttachment attachments for the message. Navigation property. Format: ARRAY<STRUCT<LastModifiedDateTime: STRING, Name: STRING, ContentType: STRING, Size: INT, IsInline: BOOLEAN, Id: STRING>>"
            },
            {
                "name": "BodyPreview",
                "type": "String",
                "description": "The preview of the message associated with the event. It is in text format."
            },
            {
                "name": "Locations",
                "type": "String",
                "description": "The locations where the event is held or attended from. The location and locations properties always correspond with each other. Format:  ARRAY<STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>, LocationEmailAddress: STRING, LocationUri: STRING, LocationType: STRING, UniqueId: STRING, UniqueIdType: STRING>>"
            },
            {
                "name": "OnlineMeetingUrl",
                "type": "String",
                "description": "A URL for an online meeting. The property is set only when an organizer specifies an event as an online meeting such as a Skype meeting"
            },
            {
                "name": "OriginalStart",
                "type": "DateTime",
                "description": "The start time that was set when the event was created in UTC time."
            },
            {
                "name": "SeriesMasterId",
                "type": "String",
                "description": "The ID for the recurring series master item, if this event is part of a recurring series."
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Event_v1",
            "dateFilterColumn": "CreatedDateTime",
            "startTime": "2019-04-28T16:00:00.000Z",
            "endTime": "2019-05-05T16:00:00.000Z",
            "userScopeFilterUri": "https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'"
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
