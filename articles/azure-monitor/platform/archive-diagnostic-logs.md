---
title: Azure tanılama günlüklerini arşivleme
description: Azure tanılama günlükleriniz bir depolama hesabında uzun süreli saklama için Arşivlemeyi öğreneceksiniz.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: bc1804e547bb1a29fc0dc680b948f1bb31af8307
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244926"
---
# <a name="archive-azure-diagnostic-logs"></a>Azure tanılama günlüklerini arşivleme

Bu makalede, biz arşivlemek için Azure portalı, PowerShell cmdlet'leri, CLI veya REST API'sini nasıl kullanabileceğinizi gösterir, [Azure tanılama günlükleri](diagnostic-logs-overview.md) bir depolama hesabında. Bu seçenek, Denetim, statik analiz veya yedekleme için bir isteğe bağlı bekletme ilkesi ile tanılama günlüklerini tutmak istiyorsanız yararlıdır. Ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan kaynak ile aynı abonelikte olması depolama hesabı yok.

> [!WARNING]
> Depolama hesabındaki günlük verilerinin biçimi, 1 Kasım 2018 tarihinde JSON Satırları olarak değişecektir. [Etkinin açıklaması ve yeni biçimi işlemek üzere araçlarınızı güncelleştirme için bu makaleye bakın.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce yapmanız [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) tanılama günlüklerinizi arşiv. İçinde depolanır, bu sayede daha iyi İzleme verilerine erişimi denetleyebilir, diğer izleme olmayan veriler içeren mevcut bir depolama hesabını kullanmamanızı öneririz. Ayrıca bir depolama hesabına tanılama ölçümleri ve Etkinlik günlüğünü arşivleme, ancak, tüm izleme verilerini merkezi bir konumda tutmak için de tanılama günlükleriniz için bu depolama hesabı kullanmak mantıklı olabilir.

> [!NOTE]
>  Şu anda arşivlenemiyor verileri bir depolama hesabı, güvenli bir sanal ağda.

## <a name="diagnostic-settings"></a>Tanılama ayarları

Aşağıdaki yöntemlerden birini kullanarak, tanılama günlüklerini arşivleme için ayarlamanız bir **tanılama ayarını** belirli bir kaynak için. Bir kaynağın tanılama ayarını günlükleri ve ölçüm verileri bir hedefe (depolama hesabı, Event Hubs ad alanı veya Log Analytics çalışma alanı) gönderilen kategorileri tanımlar. Ayrıca her bir kategori günlük ve ölçüm verileri bir depolama hesabında depolanan olayları için bekletme ilkesi (saklanacağı gün sayısı) tanımlar. Bir bekletme ilkesi, sıfır olarak ayarlanırsa, olay günlüğü kategori için (yani, sonsuza kadar söylemek) süresiz olarak depolanır. Bir bekletme ilkesi, aksi takdirde herhangi bir sayıda gün 1 ile 2147483647 arasında olabilir. [Daha fazla tanılama ayarları hakkında](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings). Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silinecektir. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar. 

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="archive-diagnostic-logs-using-the-portal"></a>Portalı kullanarak tanılama günlüklerini arşivleme

1. Portal, Azure İzleyicisi'ne gidin ve tıklayarak **tanılama ayarları**

    ![Azure İzleyicisi İzleme](media/archive-diagnostic-logs/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türe göre listeyi filtreleyin ve kaynağın tanılama ayarını ayarlamak istediğiniz'i tıklatın.

3. Hiçbir ayar kaynağı varsa, istenen bir ayar oluşturmak için seçtiğiniz. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/archive-diagnostic-logs/diagnostic-settings-none.png)

   Kaynakta mevcut ayarları varsa, bu kaynakta zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve kutusunu işaretlemeniz **depolama hesabına dışarı aktarma**, ardından depolama hesabını seçin. İsteğe bağlı olarak, bu günlükleri kullanarak saklanacağı gün sayısını ayarlayın **bekletme (gün)** kaydırıcıları. Bekletme günü sayısının sıfır günlükler süresiz olarak depolar.

   ![Tanılama ayarını - var olan ayarları Ekle](media/archive-diagnostic-logs/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve oluşturulan yeni olay verilerini hemen sonra bu depolama hesabına tanılama günlükleri arşivlenir.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Azure PowerShell aracılığıyla tanılama günlüklerini arşivleme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| ResourceId |Evet |Kaynağın tanılama ayarını ayarlamak istediğiniz kaynak kimliği. |
| StorageAccountId |Hayır |Tanılama günlükleri kaydedileceği depolama hesabı kaynak kimliği. |
| Categories |Hayır |Günlük kategorileri etkinleştirmek için virgülle ayrılmış listesidir. |
| Enabled |Evet |Tanılama etkin veya bu kaynak üzerinde devre dışı olup olmadığını gösteren bir Boole değeri. |
| RetentionEnabled |Hayır |Bir bekletme ilkesi bu kaynak üzerinde etkin olmadığını belirten bir Boole değeri. |
| Retentionındays |Hayır |Kendisi için olayları 1 ile 2147483647 arasında korunması gereken gün sayısı. Sıfır değeri, günlükler süresiz olarak depolar. |

## <a name="archive-diagnostic-logs-via-the-azure-cli"></a>Azure CLI aracılığıyla tanılama günlüklerini arşivleme

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

Tanılama günlüğüne olarak geçirilen JSON dizisi sözlükleri ekleyerek ek kategori ekleyebilirsiniz `--logs` parametresi.

`--resource-group` Bağımsız değişken, yalnızca gerekli if `--storage-account` bir nesne kimliği değil. Depolama için tanılama günlüklerini arşivleme tüm belgeler için bkz. [CLI komut başvurusu](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>REST API aracılığıyla tanılama günlüklerini arşivleme

[Bu belgeye bakın](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) bir tanılama ayarı Azure İzleyici REST API'sini kullanarak nasıl ayarlanacağı hakkında bilgi.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Tanılama günlüklerinin depolama hesabındaki şema

Arşiv kurduktan sonra bir olay etkinleştirdiğiniz günlük kategorileri birinde oluşur oluşmaz depolama hesabında bir depolama kapsayıcısı oluşturulur. Aşağıda gösterildiği gibi etkinlik ve tanılama günlükleri arasında adlandırma kuralını kapsayıcı içindeki blobları izleyin:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir blob adı aşağıdaki gibi olabilir:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Her PT1H.json blobu, blob URL’sinde belirtilen saat (örneğin, h=12) içinde gerçekleşen bir JSON olay blobu içerir. Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Dakika değeri (m = 00) her zaman 00, tanılama günlük olayları saat başına bloblara ayrılmış sonra.

PT1H.json dosyasına içinde her olay şu biçimi takip "kayıt" dizisinde depolanır:

``` JSON
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
| time |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| operationName |İşlemin adı. |
| category |Olay günlüğü kategorisi. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (yani, sözlük). |

> [!NOTE]
> Özellikleri ve bu özelliklerini kullanımını kaynağa bağlı olarak değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Blobları analiz için indirin](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Bir Event Hubs ad alanı için Stream tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Azure İzleyici ile Azure Active Directory günlüklerini arşivleme](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Tanılama günlükleri hakkında daha fazla bilgi](../../azure-monitor/platform/diagnostic-logs-overview.md)

