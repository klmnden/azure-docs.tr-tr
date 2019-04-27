---
title: Azure sanal Makineler'i kullanarak gelişmiş otomatik ölçeklendirme
description: Resource Manager ve VM ölçek kümeleri ile birden çok kural ve e-posta gönderin ve ölçek eylemleri ile Web kancası URL'leri çağrı profilleri kullanır.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2016
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 6da653bc94c8b549282ab9124dba23b08771c5f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787805"
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>VM ölçek kümeleri için Resource Manager şablonlarını kullanarak gelişmiş otomatik ölçeklendirme yapılandırması
Ölçek daraltma ve sanal makine ölçek kümelerinde yinelenen bir zamanlamaya göre veya belirli bir tarihe göre performans ölçüm eşiklere dayanarak genişleme kullanabilirsiniz. Ölçek eylemleri için e-posta ve Web kancası bildirimleri de yapılandırabilirsiniz. Bu kılavuzda, bir VM ölçek kümesi'nde bir Resource Manager şablonu kullanarak bu nesneleri yapılandırma örneği gösterilir.

> [!NOTE]
> Bu izlenecek yol, sanal makine ölçek kümeleri için adımlar açıklanmaktadır, ancak aynı bilgiyi otomatik ölçeklendirme için geçerlidir. [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts) Bir basit performans ölçümü CPU gibi basit bir ölçek daraltma veya genişletme ayarını bir VM ölçek kümesi bağlı için başvurmak [Linux](../../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-cli.md) ve [Windows](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) belgeleri
>
>

## <a name="walkthrough"></a>Kılavuz
Bu kılavuzda kullandığımız [Azure kaynak Gezgini](https://resources.azure.com/) yapılandırmak ve bir ölçek kümesini otomatik ölçeklendirme ayarını güncelleştirmek için. Azure kaynak Gezgini, Resource Manager şablonları aracılığıyla Azure kaynaklarını yönetmek için kolay bir yoludur. Azure kaynak Gezgini aracı yeniyseniz okuma [bu giriş](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Yeni bir ölçek kümesi bir temel otomatik ölçeklendirme ayarı ile dağıtın. Bu makalede bir sahip bir Windows Azure hızlı başlangıç Galerisi kullanan bir temel otomatik ölçeklendirme şablonla ölçek kümesi. Linux ölçek kümeleri aynı şekilde çalışır.
2. Ölçek kümesi oluşturulduktan sonra Azure kaynak Gezgini'nde ölçek kümesi kaynağına gidin. Aşağıdaki Microsoft.Insights düğümü altında görürsünüz.

    ![Azure Gezgini](media/autoscale-virtual-machine-scale-sets/azure_explorer_navigate.png)

    Şablon yürütme varsayılan otomatik ölçeklendirme ayarı adı ile oluşturduğu **'autoscalewad'**. Sağ tarafta, bu otomatik ölçeklendirme ayarı tam tanımı görüntüleyebilirsiniz. Bu durumda, varsayılan otomatik ölçeklendirme ayarında bir CPU tabanlı % genişletmek ve ölçeğini kuralı ile birlikte gelir.  

3. Artık daha fazla profilleri ve zamanlama veya özel gereksinimlerinize dayalı kurallar da ekleyebilirsiniz. Üç profil ile bir otomatik ölçeklendirme ayarı oluştururuz. Profiller ve otomatik ölçeklendirme kuralları anlamak için gözden [otomatik ölçeklendirme en iyi](autoscale-best-practices.md).  

    | Profiller ve kurallar | Açıklama |
    |--- | --- |
    | **Profil** |**Performans/ölçüm tabanlı** |
    | Kural |Service Bus kuyruk ileti sayısı > x |
    | Kural |Service Bus kuyruk ileti sayısı < y |
    | Kural |CPU % > n |
    | Kural |CPU % < p |
    | **Profil** |**Haftanın günü sabah saat (kuralları yok)** |
    | **Profil** |**Ürün Lansman günü (kuralları yok)** |

4. Bu kılavuz için kullandığımız kuramsal bir ölçeklendirme senaryo aşağıda verilmiştir.

   * **Temel yük** - veya ölçeklendirme my ölçek set.* üzerinde barındırılan Uygulamam yükü göre istiyorum
   * **İleti sırası boyutu** -Service Bus kuyruğuna gelen iletiler için uygulamama kullanabilirim. Ben sıranın ileti sayısı ve CPU % kullanın ve ileti sayısı ya da CPU değerse eşiği bir ölçeklendirme eylemi tetiklemek için bir varsayılan profili yapılandırın.\*
   * **Haftanın günü ve günün saati** -'Haftanın günü sabah saat' adlı bir haftalık yineleme 'zaman günün' temel profil istiyorum. Geçmiş verilerini temel alarak belirli sayıda sanal makine örneğini bu süre boyunca my uygulamanın yükü işlemek daha iyi biliyorum.\*
   * **Özel tarih** -'Başlat ürün Day' profili ekledim. Uygulamamın son pazarlama duyuruları ve sizi yeni ürün uygulamada yerleştirdiğinizde yükü işlemek hazır olması için miyim belirli tarihleri için önceden planlayın.\*
   * *Son iki profilleri diğer performans ölçümü tabanlı kuralların bunları da olabilir. Bu durumda, bir yok karar ve bunun yerine varsayılan performans ölçümü üzerinde yararlanmayı kurallarına göre. Kurallar yinelenen ve tarih temelli profilleri için isteğe bağlıdır.*

     Otomatik ölçeklendirme altyapısı önceliklendirilmesi profilleri ve kuralları yakalanan ayrıca [otomatik ölçeklendirme en iyi](autoscale-best-practices.md) makalesi.
     Otomatik ölçeklendirme için genel ölçümler bir listesi için bakın [otomatik ölçeklendirme için genel ölçümler](autoscale-common-metrics.md)

5. Emin olun, bulunduğunuz **okuma/yazma** modunda kaynak Gezgini

    ![Autoscalewad, varsayılan otomatik ölçeklendirme ayarı](media/autoscale-virtual-machine-scale-sets/autoscalewad.png)

6. Düzenle'ye tıklayın. **Değiştirin** aşağıdaki yapılandırma ile otomatik ölçeklendirme ayarını 'profillerini' öğesinde:

    ![Profilleri](media/autoscale-virtual-machine-scale-sets/profiles.png)

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
    Desteklenen alanlar ve bunların değerleri için bkz. [otomatik ölçeklendirmeyi REST API belgelerini](https://msdn.microsoft.com/library/azure/dn931928.aspx). Artık, otomatik ölçeklendirme ayarı, daha önce açıklanan üç profil içerir.

7. Son olarak, otomatik ölçeklendirme sırasında Ara **bildirim** bölümü. Otomatik ölçeklendirme bildirimleri ölçek genişletme sırasında üç şey yapmanıza olanak sağlar veya eylem başarıyla tetiklendi.
   - Bildirim Yöneticisi ve ortak Yöneticiler aboneliğinizin
   - Kullanıcı e-posta
   - Bir Web kancası çağrısı tetikleyin. Tetiklendiğinde, bu Web kancasını otomatik ölçeklendirme durumu hakkındaki meta verileri gönderir ve ölçek kümesi kaynak. Otomatik ölçeklendirme Web kancası yükü hakkında daha fazla bilgi için bkz: [Web kancası yapılandırma & e-posta bildirimleri için otomatik ölçeklendirme](autoscale-webhook-email.md).

   Otomatik ölçeklendirme ayarı değiştirmek için aşağıdakileri ekleyin, **bildirim** değeri null olan bir öğe

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

   İsabet **Put** otomatik ölçeklendirme ayarını güncelleştirmek için kaynak Gezgini'nde düğmesi.

Bir VM ölçek birden çok ölçek profilini içerir ve bildirimleri ölçeklendirme kümesi bir otomatik ölçeklendirme ayarında güncelleştirdik.

## <a name="next-steps"></a>Sonraki Adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi için şu bağlantıları kullanın.

[Sanal makine ölçek kümeleri ile otomatik ölçeklendirme sorunlarını giderme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Otomatik ölçeklendirme için genel ölçümler](autoscale-common-metrics.md)

[Azure otomatik ölçeklendirme için en iyi uygulamalar](autoscale-best-practices.md)

[Otomatik ölçeklendirmeyi PowerShell kullanarak yönetme](../../azure-monitor/platform/powershell-quickstart-samples.md#create-and-manage-autoscale-settings)

[Otomatik ölçeklendirme CLI kullanarak yönetme](cli-samples.md#autoscale)

[Web kancası ve otomatik ölçeklendirme için e-posta bildirimlerini yapılandırma](autoscale-webhook-email.md)

[Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings) şablon başvurusu
