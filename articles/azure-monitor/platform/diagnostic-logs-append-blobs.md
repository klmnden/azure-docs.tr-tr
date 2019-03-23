---
title: Azure İzleyici tanılama günlüklerine biçimi değişikliği hazırlama
description: Azure tanılama günlükleri, kullanılacak taşınacak 1 Kasım 2018'de ekleme blobları.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: ab5fba6bbbf6ade83c7699edec937ba02b222939
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370066"
---
# <a name="prepare-for-format-change-to-azure-monitor-diagnostic-logs-archived-to-a-storage-account"></a>Azure İzleyici tanılama günlükleri bir depolama hesabına arşivlenmiş biçimi değişiklik için hazırlama

> [!WARNING]
> Gönderiyorsanız [Azure kaynak tanılama günlükleri veya kaynak tanılama ayarlarını kullanarak depolama hesabı ölçümleri](./../../azure-monitor/platform/archive-diagnostic-logs.md) veya [kullanarak bir depolama hesabı için etkinlik günlüklerini günlük profilleri](./../../azure-monitor/platform/archive-activity-log.md), veri biçimi Depolama hesabı JSON satırlarına 1 Kasım 2018 tarihinde değişecektir. Etki ve nasıl yeni biçime işlemek için araçlarınızı güncelleştirmek aşağıdaki yönergeleri açıklanmaktadır. 
>
> 

## <a name="what-is-changing"></a>Ne değişiyor

Azure İzleyici kaynak tanılama verilerini ve etkinlik günlüğü verileri Event Hubs ad alanı, bir Azure depolama hesabına veya Azure İzleyici'de bir Log Analytics çalışma alanına göndermenize olanak sağlayan bir özellik sunar. Üzerinde bir sistem performans sorunu çözmek için **1 Kasım 2018'den 12:00 gece UTC** veri gönderme BLOB depolamaya günlük biçimini değiştirir. Diğer bir deyişle okuma verileri blob depolama dışına tooling varsa, yeni veri biçimi anlamak için araçlarınızı güncelleştirmeniz gerekiyor.

* Perşembe 1 Kasım 2018'de UTC gece 12:00, blob biçimi olacak şekilde değiştirmek [JSON satırları](http://jsonlines.org/). Bu, her kayıt tarafından bir yeni satır dış kayıtları diziden ve JSON kayıtlar arasında hiçbir virgül ile ayrılmış anlamına gelir.
* Tek seferde tüm Aboneliklerdeki tüm tanılama ayarları için blob biçimi değişiklikler. 1 Kasım için yayılan ilk PT1H.json dosyasına bu yeni biçim kullanır. Blob ve kapsayıcı adları aynı kalır.
* 1 Kasım tarihine kadar geçerli biçimde veri yaymak tanılama ayarını arasındaki 1 Kasım devam eder.
* Bu değişiklik, tek seferde tüm genel bulut bölgeler arasında oluşur. Değişiklik henüz Azure Çin'de, Azure Almanya ve Azure kamu bulutlarında gerçekleşmez.
* Bu değişiklik aşağıdaki veri türlerini etkiler:
  * [Azure kaynak tanılama günlükleri](./../../azure-monitor/platform/archive-diagnostic-logs.md) ([burada kaynakların listesini görmek](./../../azure-monitor/platform/diagnostic-logs-schema.md))
  * [Tanılama ayarları tarafından dışarı aktarılan bir azure kaynak ölçümleri](./../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings)
  * [Günlük profilleri tarafından dışarı aktarılan azure etkinlik günlüğü verileri](./../../azure-monitor/platform/archive-activity-log.md)
* Bu değişiklik etkilemez:
  * Ağ akışı günlükleri
  * Azure Hizmet Günlükleri'ni (örneğin, Azure App Service tanılama günlükleri, depolama analizi günlüklerinde) henüz Azure İzleyici kullanılabilir değil
  * Azure tanılama günlükleri ve etkinlik günlüklerini diğer hedeflere (Event Hubs, Log Analytics) yönlendirme

### <a name="how-to-see-if-you-are-impacted"></a>Etkilenip etkilenmediğinizi görme

Yalnızca, bu değişiklikten etkilenen:
1. Kaynak tanılama ayarı kullanarak bir Azure depolama hesabı için günlük verileri gönderen ve
2. Bu günlükleri depolama JSON yapısını bağımlı araçları vardır.
 
Azure depolama hesabınız için veri gönderen kaynak tanılama ayarlarını olup olmadığını belirlemek için gidebilirsiniz **İzleyici** bölümü portalın tıklayarak **tanılama ayarları**ve tanımlayın sahip herhangi bir kaynağa **tanı durumu** kümesine **etkin**:

![Azure İzleyici tanılama ayarları dikey penceresi](./media/diagnostic-logs-append-blobs/portal-diag-settings.png)

Tanı durumu etkin ayarlanırsa, etkin bir tanılama ayarı, bu kaynağa sahip. Kaynak tanılama ayarları depolama hesabı için veri gönderen, görmek için tıklayın:

![Depolama hesabı etkin](./media/diagnostic-logs-append-blobs/portal-storage-enabled.png)

Bu kaynak tanılama ayarlarını kullanarak depolama hesabı için veri gönderen kaynaklarınız varsa, bu depolama hesabındaki verilerin biçimi bu değişiklikten etkilenecek. Bu depolama hesaplarından dışına işleyen özel bir araç yoksa, biçimini değiştirme, etkilemez.

### <a name="details-of-the-format-change"></a>Biçim değişiklik ayrıntıları

Azure blob depolama alanındaki PT1H.json dosyasına biçimi geçerli bir JSON dizisi kayıtlarını kullanır. Artık bir anahtar kasası günlük dosyası örneği aşağıdadır:

```json
{
    "records": [
        {
            "time": "2016-01-05T01:32:01.2691226Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "78",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

Yeni biçimi kullanan [JSON satırları](http://jsonlines.org/), burada her olay bir satır, yeni bir olay yeni satır karakterini gösterir. Yukarıdaki örnek gibi PT1H.json dosyasına değişiklikten sonra görüneceğini aşağıda verilmiştir:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Bu yeni biçimi kullanarak anında iletme günlük dosyaları için Azure İzleyici sağlar [bloblarını](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs), sürekli olarak yeni olay verilerini ekleme için daha etkili olan.

## <a name="how-to-update"></a>Güncelleştirme

Yalnızca daha fazla işleme için bu günlük dosyaları alan özel bir araç varsa güncelleştirmeleri yapmanız gerekir. Yapıyorsanız, bir dış log analytics'e veya SIEM aracınızla öneririz kullanımı [bunun yerine bu veri alımı için event hubs'ı kullanarak](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/). Olay hub'ları tümleştirme birçok hizmet günlüklerinden işleme ve belirli bir günlük konumunda yer işareti ekleme açısından daha kolay olur.

Özel Araçlar hem geçerli hem de yukarıda açıklanan JSON satır biçiminde işlemek için güncelleştirilmesi gerekir. Bu, verileri yeni biçiminde görünür başladığında, araçlarınızı değil sonu garanti eder.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [kaynak tanılama günlükleri bir depolama hesabına arşivleme](./../../azure-monitor/platform/archive-diagnostic-logs.md)
* Hakkında bilgi edinin [etkinlik günlüğü verileri bir depolama hesabına arşivleme](./../../azure-monitor/platform/archive-activity-log.md)

