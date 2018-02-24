---
title: "Azure etkinlik günlüğü arşiv | Microsoft Docs"
description: "Bir depolama hesabı, uzun vadeli bekletme için Azure etkinlik günlüğü arşiv öğrenin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 0b041cc6a986c6f7a11d213f03294c9716c20d04
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="archive-the-azure-activity-log"></a>Arşiv Azure etkinlik günlüğü
Bu makalede, biz arşivlemek için Azure portal, PowerShell cmdlet'leri veya platformlar arası CLI nasıl kullanabileceğinizi gösterir, [ **Azure etkinlik günlüğü** ](monitoring-overview-activity-logs.md) depolama hesabındaki. Bu seçenek, Denetim, statik çözümleme veya yedekleme (ile bekletme ilkesi üzerinde tam denetim) 90 gün daha uzun süre, etkinlik günlüğü korumak istiyorsanız kullanışlıdır. Yalnızca, olayları 90 gün boyunca Koru gerek isterseniz daha az, etkinlik günlüğü olaylarını arşivleme etkinleştirmeden Azure platform 90 gün boyunca tutulur bu yana bir depolama hesabına arşivleme ayarlamanız gerekmez.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları gerçekleştirmeniz [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) etkinlik günlüğü arşiv. Böylece daha iyi İzleme verilerine erişimi denetleyebilirsiniz içine depolanmış, diğer izleme olmayan verilere sahip varolan bir depolama hesabı kullanmamanızı öneririz. Tanılama günlüklerini ve ölçümler bir depolama hesabına arşivlemeye çalışıyorsunuz, ancak, merkezi bir konumda tüm izleme verilerini korumak için etkinlik günlüğü de bu depolama hesabını kullanmak için anlamlı olabilir. Kullandığınız depolama hesabı, genel amaçlı depolama hesabı, blob storage hesabı olması gerekir. Depolama hesabı ayarı yapılandıran kullanıcı her iki aboneliğin uygun RBAC erişime sahip olduğu sürece günlükleri yayma abonelik ile aynı abonelikte olması gerekmez.

## <a name="log-profile"></a>Günlük profili
Aşağıdaki yöntemlerden birini kullanarak Etkinlik günlüğü arşivlemek için ayarladığınız **günlük profili** aboneliği. Günlük profili depolanan ya da akışı olayları ve çıkışları türünü tanımlar — depolama hesabı ve/veya olay hub'ı. Ayrıca bir depolama hesabında depolanan olayları için bekletme ilkesi (tutulacağı gün sayısı) tanımlar. Bekletme İlkesi sıfır olarak ayarlanırsa, olaylar süresiz olarak depolanır. Aksi takdirde, bu 1 ile 2147483647 arasında herhangi bir değere ayarlanabilir. Bekletme ilkeleri uygulanan başına günlük, olduğundan, ilke (UTC) dışında tutma sunulmuştur gün günlüklerinden gün sonunda silinir. Örneğin, bir günlük bir Bekletme İlkesi nesneniz varsa, günün bugün başında dünden önceki gün günlüklerinden silinecek. [Daha fazla bilgiyi hakkında günlük profilleri burada](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Portalı kullanarak Etkinlik günlüğü
1. Portalı'nda tıklatın **etkinlik günlüğü** sol taraftaki gezinti bağlantısı. Etkinlik günlüğü için bir bağlantı görmüyorsanız tıklatın **tüm hizmetleri** ilk bağlantı.
   
    ![Etkinlik günlüğü dikey penceresine gidin](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Dikey pencerenin üstündeki **verme**.
   
    ![Dışa Aktar düğmesini tıklatın](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Görüntülenen dikey penceresinde, onay kutusunu için **dışarı aktarma bir depolama hesabına** ve bir depolama hesabı seçin.
   
    ![Bir depolama hesabı ayarlama](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Kaydırıcı veya metin kutusunu kullanarak, depolama hesabınızı etkinlik günlüğü olaylarını tutulmalıdır gün sayısı tanımlayın. Verilerinizi süresiz olarak kalıcı'u depolama hesabına sahip tercih ederseniz, bu sayı sıfır olarak ayarlayın.
5. **Kaydet**’e tıklayın.

## <a name="archive-the-activity-log-via-powershell"></a>Arşiv PowerShell aracılığıyla etkinlik günlüğü
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| StorageAccountId |Hayır |Kaynak Kimliği depolama hesabının etkinlik günlükleri kaydedilmesi gerekir. |
| Konumlar |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgeler virgülle ayrılmış listesi. Tüm bölgelerin bir listesi görüntüleyebilirsiniz [bu sayfasını ziyaret tarafından](https://azure.microsoft.com/en-us/regions) veya kullanarak [Azure Yönetimi REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Evet |Hangi olayların tutulacağını için 1 ile 2147483647 arasında gün sayısı. Sıfır değeri günlükleri süresiz olarak depolar (sürekli). |
| Kategoriler |Evet |Toplanması gereken olay kategorileri virgülle ayrılmış listesi. Olası değerler şunlardır: yazma, silme ve eylem. |

## <a name="archive-the-activity-log-via-cli"></a>Arşiv CLI yoluyla etkinlik günlüğü
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| ad |Evet |Günlük profilinin adı. |
| storageId |Hayır |Kaynak Kimliği depolama hesabının etkinlik günlükleri kaydedilmesi gerekir. |
| Konumları |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgeler virgülle ayrılmış listesi. Tüm bölgelerin bir listesi görüntüleyebilirsiniz [bu sayfasını ziyaret tarafından](https://azure.microsoft.com/en-us/regions) veya kullanarak [Azure Yönetimi REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Evet |Hangi olayların tutulacağını için 1 ile 2147483647 arasında gün sayısı. Sıfır değeri günlükleri süresiz olarak depolar (sürekli). |
| kategoriler |Evet |Toplanması gereken olay kategorileri virgülle ayrılmış listesi. Olası değerler şunlardır: yazma, silme ve eylem. |

## <a name="storage-schema-of-the-activity-log"></a>Etkinlik günlüğü depolama şeması
Arşivleme kurduktan sonra bir etkinlik günlüğü olay oluştuktan hemen sonra bir depolama kapsayıcısı depolama hesabında oluşturulur. Kapsayıcı içinde BLOB'ları, tanılama günlüklerini ve etkinlik günlüğü arasında aynı biçimde izleyin. Bu BLOB'ları yapıdır:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
> 
> 

Örneğin, bir blob adı olabilir:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Her bir PT1H.json blob JSON blobu blob URL'SİNDE belirtilen saat içinde gerçekleşen olayların içerir (örneğin h = 12). Bunlar ortaya çıktığında mevcut saatte olayları PT1H.json dosyasına eklenir. Dakika değeri (m 00 =) her zaman etkinlik günlüğü olaylarını tek tek bloblar saat başına ayrılmış bu yana 00.

PT1H.json dosya içinde her olay bu biçim aşağıdaki "kayıtlar" dizisinde depolanır:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Öğe adı | Açıklama |
| --- | --- |
| time |Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası. |
| resourceId |Etkilenen kaynağının kaynak kimliği. |
| operationName |İşlemin adı. |
| category |Eylem kategorisi örn. Yazma, okuma, eylem. |
| resultType |Sonuç türü örn. Başarılı, başarısız, Başlat |
| resultSignature |Kaynak türüne bağlıdır. |
| durationMs |Milisaniye cinsinden işlem süresi |
| callerIpAddress |İşlem, UPN Talebi veya kullanılabilirliğine göre SPN talep yürüttü kullanıcının IP adresi. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir correlationıd değeri paylaşan olayları aynı uber eyleme ait. |
| identity |Yetkilendirme ve talep tanımlayan JSON blobu. |
| Yetkilendirme |BLOB olay RBAC özelliklerinin. Genellikle "eylem", "rol" ve "scope" özellikleri içerir. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı" |
| location |Bölge konumu gerçekleştiği (veya genel). |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (yani sözlük). |

> [!NOTE]
> Özellikleri ve bu özellikleri kullanımını kaynak bağlı olarak değişebilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [BLOB'ları çözümleme için karşıdan yükle](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Akış Event hubs'a etkinlik günlüğü](monitoring-stream-activity-logs-event-hubs.md)
* [Etkinlik günlüğü hakkında daha fazla bilgi](monitoring-overview-activity-logs.md)

