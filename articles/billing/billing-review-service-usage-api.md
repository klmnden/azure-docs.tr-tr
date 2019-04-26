---
title: Azure hizmeti REST API'si ile kaynak kullanımını gözden geçirin | Microsoft Docs
description: Azure hizmet kaynak kullanımı gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: erikre
ms.openlocfilehash: d3db4166810da981ff0117536d8550a6b2203924
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370994"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>REST API kullanarak Azure kaynak kullanımını gözden geçirin

Gözden geçirin ve tüketimini, Azure kaynaklarınızın yönetmenize, azure maliyet Yönetimi API'leri Yardım.

Bu makalede, saatlik kullanım bilgilerinizi ve filtreleri kullanarak raporu özelleştirebilir, veritabanları, sanal makinelerinin kullanım sorgulayabilmesi için nasıl kullanılacağını virgülle ayrılmış değer belge oluşturur ve etiketli günlük bir raporun nasıl oluşturulacağını öğrenin bir Azure kaynak grubundaki kaynaklar.

>[!NOTE]
> Maliyet Yönetimi API'si şu anda özel Önizleme aşamasındadır.

## <a name="create-a-basic-cost-management-report"></a>Temel Maliyet Yönetimi raporu oluşturma

Kullanım `reports` maliyet raporlama nasıl oluşturulacağını ve raporları nereye yayımlanacak tanımlamak için maliyet Yönetimi API işlemi.

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

`{subscriptionGuid}` Parametresi gereklidir ve API belirteci sağlanan kimlik bilgileri kullanılarak okunabilir bir abonelik kimliği içermelidir. , `{reportName}`

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik türü:*| Gereklidir. Kümesine `application/json`. |  
|*Yetkilendirme:*| Gereklidir. Geçerli bir kümesi `Bearer` belirteci. |

HTTP istek gövdesinde raporun parametrelerini yapılandırın. Aşağıdaki örnekte, her gün ne zaman etkin, bir Azure depolama blob kapsayıcısına yazılmış bir CSV dosyasıdır ve saatlik kaynak grubundaki tüm kaynaklar için maliyet bilgilerini içeren oluşturmak için rapor ayarlanır `westus`.

```json
{
    "properties": {
        "schedule": {
            "status": "Inactive",
            "recurrence": "Daily",
            "recurrencePeriod": {
                "from": "2018-08-21",
                "to": "2019-10-31"
            }
        },
        "deliveryInfo": {
            "destination": {
                "resourceId": "/subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
                "container": "MyReportContainer",
                "rootFolderPath": "MyScheduleTest"
            }
        },
        "format": "Csv",
        "definition": {
            "type": "Usage",
            "timeframe": "MonthToDate",
            "dataSet": {
                "granularity": "Hourly",
                "filter": {
                    "dimensions": {
                        "name": "ResourceLocation",
                        "operator": "In",
                        "values": [
                            "westus"
                        ]
                    }
                }
            }
        }
    }
}
```

,

## <a name="filtering-reports"></a>Raporları filtreleme

`filter` Ve `dimensions` maliyetlerinden belirli kaynak türlerine yönelik bir rapor odaklanmanıza olanak tanır, oluştururken, istek gövdesi bölümü. Önceki istek gövdesi bir bölgedeki tüm kaynaklara göre nasıl filtreleme yapılacağını gösterir. 

### <a name="get-all-compute-usage"></a>Tüm işlem kullanımını Al

Kullanım `ResourceType` tüm bölgeler arasında Azure sanal makine maliyetlerini aboneliğinizdeki bildirmek için boyut.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.ClassicCompute/virtualMachines", 
                "Microsoft.Compute/virtualMachines"
        ] 
    }
}
```

### <a name="get-all-database-usage"></a>Tüm veritabanı kullanımını Al

Kullanım `ResourceType` rapor Azure SQL veritabanı maliyetleri, aboneliğinizdeki tüm bölgeler arasında boyut.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.Sql/servers"
        ] 
    }
}
```

### <a name="report-on-specific-instances"></a>Belirli örnekleri raporu

`Resource` Boyut sayesinde rapor belirli kaynakların maliyetlerini.

```json
"filter": {
    "dimensions": {
        "name": "Resource",
        "operator": "In",
        "values": [
            "subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroup}/providers/Microsoft.ClassicCompute/virtualMachines/{ResourceName}"
        ]
    }
}
```

### <a name="changing-timeframes"></a>Zaman çerçevelerini değiştirme

Ayarlama `timeframe` tanımına `Custom` hafta dışında bir zaman çerçevesinde tarih ve tarih yerleşik seçenekleri aylık ayarlamak için.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
