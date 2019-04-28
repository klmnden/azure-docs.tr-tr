---
title: Kaynak değişikliklerini alma
description: Bir kaynak değiştirildiği bulma işlemini anlama ve değiştirilen özellikler listesini alın.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/20/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 0ae85b45dfcd80056316ed5f2099aab4057d24c8
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63760818"
---
# <a name="get-resource-changes"></a>Kaynak değişikliklerini alma

Kaynakları günlük kullanımı, yeniden yapılandırma ve hatta yeniden dağıtım kurs değiştirilir.
Değişiklik, bir kişi veya otomatik bir işlem tarafından gelebilir. Çoğu değişiklik bilinçli olarak böyle tasarlanmıştır ancak bazen değil. Değişiklik geçmişi son 14 gün ile Azure kaynak Graph yapmanızı sağlar:

- Bir Azure Resource Manager özelliğinde ne zaman değişiklik algılandığını öğrenme.
- Bu değişiklik olayı kapsamında hangi özelliklerin değiştirildiğini görme.

Değişiklik algılama ve ayrıntıları, aşağıdaki örnek senaryolar için değerlidir:

- Anlamak için Olay yönetimi sırasında _potansiyel olarak_ ilgili değişiklikler. Belirli bir zaman penceresi sırasında değişiklik olaylarını sorgulamak ve değişiklik ayrıntıları değerlendirin.
- Bir yapılandırma yönetim veritabanıdır tutarak bir CMDB, güncel bilinir. Tüm kaynaklar ve bunların tam özellik kümeleri üzerinde zamanlanmış sıklığı yenilemek yerine yalnızca nelerin değiştiğini alın.
- Hangi özelliklerde değiştirilmiş bir kaynağın uyumluluk durumu değiştiğinde anlama. Bu ek özellikler değerlendirmesi bir Azure İlkesi tanım yönetilmesi gereken diğer özellikleri hakkında Öngörüler sağlar.

Bu makalede, Kaynak grafiğin SDK'sı aracılığıyla bu bilgileri toplamak gösterilmektedir. Bu bilgiler Azure portalında görmek için Azure İlkesi'nin bkz [değişiklik geçmişini](../../policy/how-to/determine-non-compliance.md#change-history-preview).

> [!NOTE]
> Değişiklik ayrıntıları kaynak Graph'te için Resource Manager özelliklerdir. Azure Automation'ın bir sanal makine içinde değişiklikleri izlemek için bkz [değişiklik izleme](../../../automation/automation-change-tracking.md) veya Azure İlkesi'nin [sanal makineler için konuk yapılandırma](../../policy/concepts/guest-configuration.md).

> [!IMPORTANT]
> Azure kaynak Graph'te değişiklik geçmişini genel Önizleme aşamasındadır.

## <a name="find-when-changes-were-detected"></a>Ne zaman değişiklik algılanmadı Bul

Bir kaynakta yapılan değişiklikler görmeye ilk adımı, bir zaman penceresi içinde bu kaynakla ilgili değişiklik olayları bulmaktır. Bu adım aracılığıyla gerçekleştirilir **resourceChanges** REST uç noktası.

**ResourceChanges** uç nokta, iki parametre istek gövdesindeki gerektirir:

- **ResourceId**: Azure kaynak değişiklikler için aranacak.
- **Aralığı**: Bir özellik _Başlat_ ve _son_ ne zaman denetleneceğini değişiklik kullanarak olay için tarihlerini **Zulu saat dilimi (Z)**.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-03-28T00:00:00.000Z",
        "end": "2019-03-31T00:00:00.000Z"
    }
}
```

Yukarıdaki istek gövdesi, REST API URI'sini ile **resourceChanges** olan:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

Yanıt şu örneğe benzer:

```json
{
    "changes": [{
            "changeId": "2db0ad2d-f6f0-4f46-b529-5c4e8c494648",
            "beforeSnapshot": {
                "timestamp": "2019-03-29T01:32:05.993Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-29T01:54:24.42Z"
            }
        },
        {
            "changeId": "9dc352cb-b7c1-4198-9eda-e5e3ed66aec8",
            "beforeSnapshot": {
                "timestamp": "2019-03-28T10:30:19.68Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-28T21:12:31.337Z"
            }
        }
    ]
}
```

Her değişiklik olayı için algılanan **ResourceId** sahip bir **Changeıd** o kaynağa benzersiz. Sırada **Changeıd** dize bazen diğer özellikleri içerebilir, benzersiz olması garanti. Değişiklik kaydı süreleri içerir, önce ve sonra anlık görüntüleri alınmıştır.
Bazı zaman noktasında Bu pencerede değişiklik olayı oluştu.

## <a name="see-what-properties-changed"></a>Özellikleri nelerin değiştiğini görmek

İle **Changeıd** gelen **resourceChanges** uç noktasını **resourceChangeDetails** REST uç noktasını sonra değişiklik olayı ayrıntılarını almak için kullanılır.

**ResourceChangeDetails** uç nokta, iki parametre istek gövdesindeki gerektirir:

- **ResourceId**: Azure kaynak değişiklikler için aranacak.
- **Changeıd**: Benzersiz değişiklik olayı için **ResourceId** kaynağından toplanan **resourceChanges**.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "53dc0515-b86b-4bc2-979b-e4694ab4a556"
}
```

Yukarıdaki istek gövdesi, REST API URI'sini ile **resourceChangeDetails** olan:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

Yanıt şu örneğe benzer:

```json
{
    "changeId": "53dc0515-b86b-4bc2-979b-e4694ab4a556",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

**beforeSnapshot** ve **afterSnapshot** her olaya anlık görüntünün alındığı zamanı ve özelliklerini o anda. Bu anlık görüntüler arasında belirli bir noktada değişiklik oldu. Yukarıdaki örneğe bakarak, değişen özellik olduğunu görebiliriz **supportsHttpsTrafficOnly**.

Program aracılığıyla sonuçları karşılaştırmak için karşılaştırma **içeriği** farkı belirlemek için her anlık görüntü bölümü. Tüm anlık görüntü karşılaştırırsanız **zaman damgası** beklenen rağmen bir fark olarak her zaman gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md).
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md).
- Öğrenme [kaynakları keşfedin](../concepts/explore-resources.md).
