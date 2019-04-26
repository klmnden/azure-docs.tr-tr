---
title: Azure Etkinlik günlüğünü arşivleme
description: Azure etkinlik günlüğünüzü bir depolama hesabında uzun süreli saklama için arşivleme.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: b6009471048232b52020e4bef6272ed8cb1bd35b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345864"
---
# <a name="archive-the-azure-activity-log"></a>Azure Etkinlik günlüğünü arşivleme
Bu makalede, biz arşivlemek için Azure portalı, PowerShell cmdlet'leri veya platformlar arası CLI nasıl kullanabileceğinizi gösterir, [ **Azure etkinlik günlüğü** ](../../azure-monitor/platform/activity-logs-overview.md) bir depolama hesabında. Etkinlik günlüğünüzü 90 günden uzun (ile bekletme ilkesini üzerinde tam denetim) denetim, statik analiz veya yedekleme korumak istiyorsanız, bu seçenek kullanışlıdır. Yalnızca olaylarınızı 90 gün boyunca Beklet gerekir ya da daha az, etkinlik günlüğü olaylarını arşivleme etkinleştirmeden Azure platformunda 90 gün boyunca bekletilir olduğundan bir depolama hesabına arşivleme ayarlamak ihtiyacınız yoktur.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce yapmanız [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) etkinlik günlüğünüzü arşiv. İçinde depolanır, bu sayede daha iyi İzleme verilerine erişimi denetleyebilir, diğer izleme olmayan veriler içeren mevcut bir depolama hesabını kullanmamanızı öneririz. Ayrıca Tanılama günlükleri ve ölçümler bir depolama hesabına arşivleme, Bununla birlikte, tüm izleme verilerini merkezi bir konumda tutmak için etkinlik günlüğü de bu depolama hesabını kullanmak için mantıklı olabilir. Ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan ile aynı abonelikte olması depolama hesabı yok.

## <a name="log-profile"></a>Günlük profilini
Aşağıdaki yöntemlerden birini kullanarak Etkinlik günlüğünü arşivleme için ayarlamanız **günlük profilini** aboneliği. Günlük profilini depolanan veya akış olayları ve çıkışları türünü tanımlar — depolama hesabı ve/veya olay hub'ı. Ayrıca bir depolama hesabında depolanan olayları için bekletme ilkesi (saklanacağı gün sayısı) tanımlar. Bekletme İlkesi, sıfır olarak ayarlanırsa, olayları süresiz olarak depolanır. Aksi takdirde, bu 1 ile 365 arasında herhangi bir değere ayarlanabilir. Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silinecektir. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar. [Daha fazla günlük hakkında burada profilleri](../../azure-monitor/platform/activity-logs-overview.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Portalı kullanarak Etkinlik günlüğünü arşivleme
1. Portalında **etkinlik günlüğü** sol taraftaki gezinti bağlantısı. Etkinlik günlüğü için bir bağlantı görmüyorsanız, tıklayın **tüm hizmetleri** ilk bağlantı.
   
    ![Etkinlik günlüğü dikey penceresine gidin](media/archive-activity-log/activity-logs-portal-navigate-v2.png)
2. Dikey pencerenin en üstünde tıklayın **dışarı aktarma, olay Hub'ına**.
   
    ![Dışarı Aktar düğmesini tıklatın](media/archive-activity-log/activity-logs-portal-export-v2.png)
3. Açılan dikey pencerede kutusunu işaretlemeniz **dışarı aktarmak için bir depolama hesabı** ve bir depolama hesabı seçin.
   
    ![Bir depolama hesabı](media/archive-activity-log/act-log-portal-export-blade.png)
4. Kaydırıcı veya metin kutusunu kullanarak, depolama hesabınızdaki etkinlik günlüğü olaylarını tutulmalıdır gün (0 ila 365) sayısını tanımlayın. Verilerinizi süresiz olarak kalıcı'u depolama hesabına sahip olmasını isterseniz, bu sayı sıfır olarak ayarlayın. Daha fazla 365 gün sayısını girin gerekiyorsa, aşağıda açıklanan PowerShell veya CLI yöntemleri kullanın.
5. **Kaydet**’e tıklayın.

## <a name="archive-the-activity-log-via-powershell"></a>PowerShell aracılığıyla Etkinlik günlüğünü arşivleme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your storage account belongs to>"
   $storageAccountName = "<your storage account name>"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -StorageAccountId $storageAccountId
   ```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| StorageAccountId |Evet |Etkinlik günlükleri kaydedileceği depolama hesabı kaynak kimliği. |
| Konumlar |Evet |Etkinlik günlüğü olayları toplamak istiyorsanız bölgelerin virgülle ayrılmış listesi. Tüm bölgelerin listesi için aboneliği kullanarak görüntüleyebileceğiniz `(Get-AzLocation).Location`. |
| Retentionındays |Hayır |Hangi olayların tutulacağını, 1 ile 365 arasında bir gün sayısı. Sıfır değeri günlükler süresiz olarak depolar (sonsuz). |
| Categories |Hayır |Virgülle ayrılmış liste toplanması gereken olay kategorileri. Olası değerler şunlardır: yazma, silme ve eylem.  Sağlanmazsa, ardından tüm olası değerler kabul edilir |

## <a name="archive-the-activity-log-via-cli"></a>CLI aracılığıyla Etkinlik günlüğünü arşivleme

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --storage-account-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>"
   ```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| ad |Evet |Günlük profilinin adı. |
| Depolama hesabı kimliği |Evet |Etkinlik günlükleri kaydedileceği depolama hesabı kaynak kimliği. |
| konumlar |Evet |Boşlukla ayrılmış etkinlik günlüğü olayları toplamak istiyorsanız bölgelerin listesi. Tüm bölgelerin listesi için aboneliği kullanarak görüntüleyebileceğiniz `az account list-locations --query [].name`. |
| gün |Evet |Hangi olayların tutulacağını, 1 ile 365 arasında bir gün sayısı. Sıfır değeri günlükler süresiz olarak depolar (sonsuz).  Sıfır ise, ardından etkin parametresi ayarlanmalıdır true. |
|enabled | Evet |TRUE veya False.  Bekletme İlkesi devre dışı bırakmak veya etkinleştirmek için kullanılır.  TRUE ise gün parametresi 0'dan büyük bir değer olması gerekir.
| kategoriler |Evet |Boşlukla ayrılmış toplanması gereken olay kategorilerinin listesi. Olası değerler şunlardır: yazma, silme ve eylem. |

## <a name="storage-schema-of-the-activity-log"></a>Etkinlik günlüğü depolama şeması
Arşiv ayarlamış olduğunuz sonra bir etkinlik günlüğü olayı oluşur oluşmaz depolama hesabında bir depolama kapsayıcısı oluşturulur. Aşağıda gösterildiği gibi etkinlik ve tanılama günlükleri arasında adlandırma kuralını kapsayıcı içindeki blobları izleyin:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir blob adı aşağıdaki gibi olabilir:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Her PT1H.json blobu blob URL'SİNDE belirtilen saat içinde gerçekleşen olayların JSON blobu içerir (örneğin h = 12). Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Dakika değeri (m = 00) her zaman 00, etkinlik günlüğü olayları saat başına bloblara ayrılmış sonra.

PT1H.json dosyasına içinde her olay şu biçimi takip "kayıt" dizisinde depolanır:

``` JSON
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
| time |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| operationName |İşlemin adı. |
| category |Eylem kategorisi örn. Yazma, okuma, eylem. |
| resultType |Sonuç türü örn. Başarılı, başarısız, başlangıç |
| resultSignature |Kaynak türüne bağlıdır. |
| durationMs |Milisaniye cinsinden işlem süresi |
| callerIpAddress |İşlem, UPN Talebi veya SPN talep kullanılabilirliğine göre gerçekleştiren kullanıcının IP adresi. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir Correlationıd paylaşan olayları aynı uber eyleme ait. |
| identity |Yetkilendirme ve talep açıklayan JSON blob. |
| Yetkilendirme |BLOB RBAC özelliklerinin olay. Genellikle, "action", "rolü" ve "scope" özelliklerini içerir. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı", "Bilgilendirici" ve "Ayrıntılı" |
| location |Bölge oluştuğu konumu (veya genel). |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (yani, sözlük). |

> [!NOTE]
> Özellikleri ve bu özelliklerini kullanımını kaynağa bağlı olarak değişebilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Blobları analiz için indirin](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Etkinlik günlüğünün Event Hubs'a Stream](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
* [Etkinlik günlüğü hakkında daha fazla bilgi](../../azure-monitor/platform/activity-logs-overview.md)


