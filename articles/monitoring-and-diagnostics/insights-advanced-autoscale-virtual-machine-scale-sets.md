---
title: Azure sanal makineleri kullanarak gelişmiş otomatik ölçeklendirme
description: Birden çok kural ve e-posta göndermek ve ölçek eylemleri ile Web kancası URL'leri çağrı profillerinin Resource Manager ve VM ölçek kümesi kullanır.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2016
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 9ff8c28a139d9a16d31a61b560ef7f5759d0a3f5
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267739"
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>VM ölçek kümesi için Resource Manager şablonları kullanarak gelişmiş otomatik ölçeklendirme yapılandırma
Ölçek ve sanal makine ölçek kümeleri yinelenen bir zamanlamaya göre veya belirli bir tarihe göre performans ölçüm eşiklere dayanarak genişleme kullanabilirsiniz. Ölçeklendirme eylemi için e-posta ve Web kancası bildirimleri de yapılandırabilirsiniz. Bu kılavuz, bir VM ölçek kümesi üzerinde bir Resource Manager şablonu kullanarak bu nesneleri yapılandırma örneği gösterilir.

> [!NOTE]
> Bu izlenecek adımları VM ölçek kümesi için açıklamaktadır, ancak aynı bilgiler için otomatik ölçeklendirmeyi geçerlidir [bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/), ve [uygulama hizmeti - Web Apps](https://azure.microsoft.com/services/app-service/web/).
> Bir basit performans ölçümü CPU gibi basit bir ölçek giriş/çıkış VM ölçek kümesi ayarda dayanarak, bakın [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) ve [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) belgeleri
>
>

## <a name="walkthrough"></a>Kılavuz
Bu kılavuzda, kullandığımız [Azure kaynak Gezgini](https://resources.azure.com/) yapılandırmak ve ölçek kümesi için otomatik ölçeklendirme ayarı güncelleştirmek için. Azure kaynak Gezgini, Resource Manager şablonları aracılığıyla Azure kaynaklarınızı yönetmek için kolay bir yoludur. Azure kaynak Gezgini aracı yeniyseniz, okuma [bu giriş](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Bir temel otomatik ölçeklendirme ayarı ile yeni bir ölçek dağıtın. Bu makalede Windows sahip Azure hızlı başlama Galerisi adresinden kullanan ölçek temel otomatik ölçeklendirme şablonla ayarlayın. Linux ölçek kümeleri aynı şekilde çalışır.
2. Ölçek kümesi oluşturulduktan sonra Azure kaynak Gezgini'nden ölçek kümesi kaynağa gidin. Aşağıdaki Microsoft.ınsights düğümü altında bakın.

    ![Azure Gezgini](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    Şablon yürütme varsayılan otomatik ölçeklendirme ayarı adı ile oluşturdu **'autoscalewad'**. Sağ tarafta, bu otomatik ölçeklendirme ayarı tam tanımını görüntüleyebilirsiniz. Bu durumda, varsayılan otomatik ölçeklendirme ayarında bir CPU tabanlı % genişleme ve ölçek bileşenini kuralı ile birlikte gelir.  

3. Artık daha fazla profilleri ve zamanlama veya özel gereksinimler dayalı kurallar da ekleyebilirsiniz. Üç profil ile bir otomatik ölçeklendirme ayarı oluşturuyoruz. Profilleri ve otomatik ölçeklendirme kurallarında anlamak için gözden [otomatik ölçeklendirme en iyi yöntemler](insights-autoscale-best-practices.md).  

    | Profilleri & kuralları | Açıklama |
    |--- | --- |
    | **Profil** |**Performans/ölçü dayalı** |
    | Kural |Hizmet veri yolu kuyruğu ileti sayısı > x |
    | Kural |Hizmet veri yolu kuyruğu ileti sayısı < y |
    | Kural |CPU % > n |
    | Kural |CPU % < p |
    | **Profil** |**Hafta içi gün sabah saat (kuralları yok)** |
    | **Profil** |**Ürün başlatma gün (kuralları yok)** |

4. Bu kılavuz için kullandığımız kuramsal bir ölçeklendirme senaryo aşağıda verilmiştir.

    * **Temel yük** - veya ölçeklendirmek my ölçek set.* üzerinde barındırılan Uygulamam üzerindeki yükü göre istiyorum
    * **Sıra boyutu ileti** -ı bir hizmet veri yolu kuyruğu gelen iletiler için Uygulamam için kullanın. Sıranın ileti sayısı ve CPU % kullanın ve ileti sayısı ya da CPU değerse threshold.* bir ölçek eylemi tetiklemek için bir varsayılan profili yapılandırma
    * **Haftanın günü ve saat** -'Hafta içi gün sabah saat' adlı bir haftalık yinelenen 'zaman günün' temel profil istiyor. Geçmiş verilerini temel alarak bu sağlar.* sırasında my uygulamanın yükü işlemek üzere VM örnekleri belirli sayıda daha iyidir bildirin
    * **Özel tarih** -'Ürün başlatma Day' profili eklendi. Uygulamam pazarlama duyuruları nedeniyle ve size yeni bir ürün uygulamasında geçirdiğinizde yükü işlemek üzere hazır olması için t belirli tarihler için İleriyi
    * *Son iki profilleri diğer performans ölçüm tabanlı içinde kurallar bunları da sahip olabilirsiniz. Bu durumda, bir tane değil karar ve bunun yerine varsayılan performans ölçüm yararlanmayı kuralları dayalı. Kurallar yinelenen ve tarih temelli profilleri için isteğe bağlıdır.*

    Otomatik ölçeklendirme altyapısının önceliği profilleri ve kuralları yakalanan de [otomatik ölçeklendirmeyi en iyi yöntemler](insights-autoscale-best-practices.md) makalesi.
    Otomatik ölçeklendirme için ortak ölçümleri listesi için bkz [otomatik ölçeklendirme için ortak ölçümleri](insights-autoscale-common-metrics.md)

5. Olduğunuzdan emin olun, üzerinde **okuma/yazma** kaynak Gezgininde modu

    ![Autoscalewad, varsayılan otomatik ölçeklendirme ayarı](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Düzenle'yi tıklatın. **Değiştir** aşağıdaki yapılandırma ile otomatik ölçeklendirme ayarında 'profilleri' öğesi:

    ![Profilleri](./media/insights-advanced-autoscale-vmss/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 85
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Desteklenen alanlar ve bunların değerleri için bkz: [otomatik ölçeklendirme REST API belgeleri](https://msdn.microsoft.com/library/azure/dn931928.aspx). Şimdi, otomatik ölçeklendirme ayarı daha önce açıklanan üç profil içerir.

7. Son olarak, otomatik ölçeklendirme Ara **bildirim** bölümü. Otomatik ölçeklendirme bildirimleri bir genişletme zaman üç şey yapmanıza olanak sağlar veya eylemi başarıyla tetiklendi.
   - Yönetici ve ortak yöneticileri aboneliğinizin bildir
   - Kullanıcı e-posta
   - Bir Web kancası çağrı tetikler. Harekete olduğunda, bu Web kancası otomatik ölçeklendirmeyi koşul hakkındaki meta verileri gönderir ve kaynak ölçek kümesi. Otomatik ölçeklendirme Web kancası yükü hakkında daha fazla bilgi için bkz: [yapılandırma Web kancası & otomatik ölçeklendirme için e-posta bildirimleri](insights-autoscale-to-webhook-email.md).

   Otomatik ölçeklendirme ayarı değiştirmek için aşağıdakileri ekleyin, **bildirim** öğesi değeri null

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]

   ```

   İsabet **Put** otomatik ölçeklendirme ayarı güncelleştirmek için kaynak Gezgininde düğmesi.

Otomatik ölçeklendirme ayarında birden fazla ölçek profillerini içeren ve bildirimleri ölçeklendirmek için ayarlanmış bir VM ölçekte güncelleştirdiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi için bu bağlantıları kullanın.

[Otomatik ölçeklendirme sanal makine ölçek kümeleri ile ilgili sorunları giderme](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Otomatik ölçeklendirme için ortak ölçümleri](insights-autoscale-common-metrics.md)

[Azure otomatik ölçeklendirme için en iyi yöntemler](insights-autoscale-best-practices.md)

[Otomatik ölçeklendirme PowerShell kullanarak yönetme](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Otomatik ölçeklendirme CLI kullanarak yönetme](insights-cli-samples.md#autoscale)

[Web kancası & otomatik ölçeklendirme için e-posta bildirimleri yapılandırma](insights-autoscale-to-webhook-email.md)
