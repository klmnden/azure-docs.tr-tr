---
title: Arşiv Azure tanılama günlüklerini | Microsoft Docs
description: Azure tanılama günlüklerinize bir depolama hesabı, uzun vadeli bekletme için arşiv öğrenin.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: johnkem
ms.openlocfilehash: bf776ba8aaeca361250f39fb2c62233ee1dfbd5b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="archive-azure-diagnostic-logs"></a>Arşiv Azure tanılama günlükleri

Bu makalede, biz arşivlemek için Azure portal, PowerShell cmdlet'leri, CLI veya REST API nasıl kullanabileceğinizi gösterir, [Azure tanılama günlüklerini](monitoring-overview-of-diagnostic-logs.md) depolama hesabındaki. Bu seçenek, tanılama günlüklerinize denetim, statik çözümleme veya yedekleme için bir isteğe bağlı bekletme ilkesi ile korumak istiyorsanız kullanışlıdır. Depolama hesabı ayarı yapılandıran kullanıcı her iki aboneliğin uygun RBAC erişime sahip olduğu sürece günlükleri yayma kaynak ile aynı abonelikte olması gerekmez.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları gerçekleştirmeniz [depolama hesabı oluşturma](../storage/storage-create-storage-account.md) tanılama günlüklerinize arşiv. Böylece daha iyi İzleme verilerine erişimi denetleyebilirsiniz içine depolanmış, diğer izleme olmayan verilere sahip varolan bir depolama hesabı kullanmamanızı öneririz. Ayrıca bir depolama hesabına tanılama ölçümleri ve etkinlik günlüğü arşivleme, ancak onu merkezi bir konumda tüm izleme verilerini korumak için tanılama günlükleri de bu depolama hesabını kullanmak için anlamlı olabilir. Kullandığınız depolama hesabı, genel amaçlı depolama hesabı, blob storage hesabı olması gerekir.

## <a name="diagnostic-settings"></a>Tanılama ayarları

Aşağıdaki yöntemlerden birini kullanarak tanılama günlüklerinize arşivlemek için ayarladığınız bir **tanılama ayarını** belirli bir kaynak için. Kaynağın tanılama ayarını kategorilerini günlükleri ve hedef için (depolama hesabı, olay hub'ları ad alanı veya günlük analizi) gönderilen ölçüm verileri tanımlar. Ayrıca her bir günlük kategorisi ve ölçüm verileri bir depolama hesabında depolanan olayları için bekletme ilkesi (tutulacağı gün sayısı) tanımlar. Bir bekletme ilkesi sıfır olarak ayarlanırsa, bu günlüğü kategori olaylar (yani, sonsuza kadar söylemeniz) süresiz olarak depolanır. Bir bekletme ilkesi, aksi takdirde herhangi bir sayıda gün 1 ile 2147483647 arasında olabilir. [Daha fazla bilgiyi burada tanılama ayarları hakkında](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings). Bekletme ilkeleri uygulanan başına günlük, olduğundan, ilke (UTC) dışında tutma sunulmuştur gün günlüklerinden gün sonunda silinir. Bir günlük bir Bekletme İlkesi nesneniz varsa, örneğin, günün bugün başında dünden önceki gün günlüklerinden silinecek

> [!NOTE]
> Çok boyutlu ölçümleri tanılama ayarları aracılığıyla gönderme şu anda desteklenmiyor. Ölçümleri boyutlarla boyut değerleri toplanan düzleştirilmiş tek boyutlu ölçümleri olarak dışarı aktarılır.
>
> *Örneğin*: bir olay hub'ındaki 'Gelen iletileri' Ölçüm incelediniz ve üzerinde grafiğinin bir sıra gerçekleştiriliyordu. Ancak, ölçüm gelen tüm iletilerin tüm temsil edilir tanılama ayarları aracılığıyla dışarı aktardığınızda olay hub'ı sıralar.
>
>

## <a name="archive-diagnostic-logs-using-the-portal"></a>Portalı kullanarak arşiv tanılama günlükleri

1. Portal, Azure izleyicisine gidin ve tıklayın **tanılama ayarları**

    ![Azure İzleyicisi İzleme bölümü](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türü listeyi filtrelemek sonra tanılama ayarını ayarlamak istediğiniz kaynak'ı tıklatın.

3. Hiçbir ayar kaynağı varsa, sizden istenir bir ayar oluşturmak için seçtiğiniz. "Tanılamayı açın."'i tıklatın

   ![Tanılama ayarını - ayar Ekle](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Kaynak üzerinde var olan ayarları varsa, bu kaynak üzerinde zaten yapılandırılmış ayarları listesini görürsünüz. "Tanılama ayarını Ekle" yi tıklatın.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve için kutuyu **verme depolama hesabına**, bir depolama hesabı seçin. İsteğe bağlı olarak, bu günlükler kullanarak korumak için gün sayısını ayarlayın **bekletme (gün)** kaydırıcılar. Sıfır gün bekletme günlükleri süresiz olarak depolar.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni bir ayar bu kaynak için ayarları listesi görüntülenir ve yeni olay verilerini oluşturulan hemen bu depolama hesabına tanılama günlüklerini arşivlenir.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Azure PowerShell aracılığıyla arşiv tanılama günlükleri

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| ResourceId |Evet |Kaynağın tanılama ayarını ayarlamak istediğiniz kaynak kimliği. |
| StorageAccountId |Hayır |Tanılama günlüklerini kaydedilmesi gereken depolama hesabının kaynak kimliği. |
| Kategoriler |Hayır |Etkinleştirmek için günlük kategorileri virgülle ayrılmış listesi. |
| Etkin |Evet |Tanılama etkin veya bu kaynağa devre dışı olup olmadığını gösteren bir Boole değeri. |
| RetentionEnabled |Hayır |Bir bekletme ilkesi bu kaynakta etkin olmadığını gösteren bir Boole değeri. |
| retentionInDays |Hayır |Kendisi için olayları 1 ile 2147483647 arasında korunması gereken gün sayısı. Sıfır değeri günlükleri süresiz olarak depolar. |

## <a name="archive-diagnostic-logs-via-the-azure-cli-20"></a>Azure CLI 2.0 aracılığıyla arşiv tanılama günlükleri

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Ek kategoriler tanılama günlük olarak geçirilen JSON dizisine sözlükler ekleyerek ekleyebileceğiniz `--logs` parametresi.

`--resource-group` Bağımsız değişken, yalnızca gerekli olduğunda `--storage-account` bir nesne kimliği değil Depolama için tanılama günlükleri arşivlemek için tam belgeleri için bkz [CLI komut başvurusu](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>REST API aracılığıyla arşiv tanılama günlükleri

[Bu belgede bakın](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) Azure İzleyici REST API'sini kullanarak tanılama ayarlama nasıl ayarlanacağı hakkında bilgi için.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Tanılama günlüklerini depolama hesabındaki şeması

Arşivleme kurduktan sonra bir depolama kapsayıcısı etkinleştirdiğiniz günlük kategorilerden birini olay oluştuktan hemen sonra depolama hesabı oluşturulur. Kapsayıcı içinde BLOB'ları, tanılama günlüklerini ve etkinlik günlüğü arasında aynı biçimde izleyin. Bu BLOB'ları yapıdır:

> Öngörüler - günlükleri-{günlük kategori adı} / ResourceId = / ABONELİKLERİ / {abonelik kimliği} /RESOURCEGROUPS/ {kaynak grubu adı} /PROVIDERS/ {kaynak sağlayıcı adı} / {kaynak türü} / {kaynak adı} / y = {dört basamaklı sayısal year} / m = {iki basamaklı sayısal month} / d = {iki basamaklı sayısal günü} / h = {iki basamaklı 24 saatlik hour}/m=00/PT1H.json

Veya daha basit bir şekilde

> Öngörüler - günlükleri-{günlük kategori adı} / ResourceId = / {kaynak kimliği} / y = {dört basamaklı sayısal year} / m = {iki basamaklı sayısal month} / d = {iki basamaklı sayısal günü} / h = {iki basamaklı 24 saatlik hour}/m=00/PT1H.json

Örneğin, bir blob adı olabilir:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Her bir PT1H.json blob JSON blobu blob URL'SİNDE belirtilen saat içinde gerçekleşen olayların içerir (örneğin, h = 12). Bunlar ortaya çıktığında mevcut saatte olayları PT1H.json dosyasına eklenir. Dakika değeri (m 00 =) her zaman tanılama günlük olayları tek tek bloblar saat başına ayrılmış bu yana 00.

PT1H.json dosya içinde her olay bu biçim aşağıdaki "kayıtlar" dizisinde depolanır:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
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
| category |Olay günlüğü kategori. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (yani sözlük). |

> [!NOTE]
> Özellikleri ve bu özellikleri kullanımını kaynak bağlı olarak değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [BLOB'ları çözümleme için karşıdan yükle](../storage/storage-dotnet-how-to-use-blobs.md)
* [Bir olay hub'ları ad alanına akış tanılama günlükleri](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Tanılama günlükleri hakkında daha fazla bilgi](monitoring-overview-of-diagnostic-logs.md)
