---
title: "Otomatik ölçeklendirme sanal makine ölçek ayarlar ile Azure CLI | Microsoft Docs"
description: "Azure CLI 2.0 ile nasıl sanal makine ölçek otomatik ölçeklendirme kurallar oluşturmak ayarlar"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 8552f6b2723fef2c61d49a34d2d60c2a6c209a32
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Bir sanal makineyi ölçeği ile Azure CLI 2.0 Ayarla otomatik olarak ölçeklendirin
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz VM örneği sayısını tanımlayın. Uygulama talep değiştikçe otomatik olarak artırın veya VM örneği sayısını azaltın. Otomatik ölçeklendirme özelliği ile isteğe bağlı müşteri takip edin veya uygulamanızın yaşam döngüsü boyunca uygulama performans değişikliklerine yanıt verme olanak sağlar.

Bu makalede, Ölçek kümesi VM örnekleri performansını izlemek Azure CLI 2.0 ile otomatik ölçeklendirme kurallarını oluşturulacağını gösterir. Otomatik ölçeklendirme kurallar artırın veya bu performans ölçümleri yanıta VM örnekleri sayısını azaltın. Bu adımları tamamlayabilmeniz için [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) veya [Azure portal](virtual-machine-scale-sets-autoscale-portal.md).


## <a name="prerequisites"></a>Önkoşullar
Otomatik ölçeklendirme kuralları oluşturmak için mevcut bir sanal makine gereksinim ölçek kümesi. Bir ölçek kümesi oluşturabileceğiniz [Azure portal](virtual-machine-scale-sets-create-portal.md), [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md), veya [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Otomatik ölçeklendirme kurallarını oluşturmayı kolaylaştırmak için ölçek kümesi için bazı değişkenler tanımlayın. Aşağıdaki örnek, ölçeği adlandırılmış Ayarla değişkenleri tanımlar *myScaleSet* kaynak grubunda adlı *myResourceGroup* ve *eastus* bölge. Aboneliğinizi kimliği elde edilir [az hesabı Göster](/cli/azure/account#az_account_show). Hesabınızla ilişkili birden çok aboneliğiniz varsa, yalnızca ilk abonelik döndürülür. Adları ve abonelik kimliği aşağıdaki gibi ayarlayın:

```azurecli
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

## <a name="define-an-autoscale-profile"></a>Otomatik ölçeklendirme profili tanımlayın
Otomatik ölçeklendirme kurallarını JSON (JavaScript nesne gösterimi) ile Azure CLI 2.0 olarak dağıtılır. Tanımlar ve otomatik ölçeklendirme kurallarını dağıtır tam JSON makalenin sonraki bölümlerinde bulunabilir. 

Otomatik ölçeklendirme profili başlangıcı varsayılan tanımlar, minimum ve maksimum ölçek kümesi kapasitesi. Aşağıdaki örnek, varsayılan ve en az kapasitesi ayarlar *2* VM örnekleri ve en fazla *10*:

```json
{
  "name": "autoscale rules",
  "capacity": {
    "minimum": "2",
    "maximum": "10",
    "default": "2"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak genişletmek için kural oluşturma
Uygulama talep artarsa, Ölçek VM örnekleri üzerindeki yük artar ayarlayın. Bu artan yükün tutarlı, ise yalnızca kısa isteğe bağlı yerine, Ölçek kümesi VM örnekleri sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu VM örnekleri oluşturulur ve uygulamalarınızı dağıtılan bunlara trafiğini yük dengeleyici üzerinden dağıtmak ölçek kümesini başlar. CPU veya disk, ne kadar süreyle uygulama yük belirli bir eşiği karşılamalı ve ölçek eklemek için kaç VM örnekleri kümesi gibi izlemek için hangi ölçümleri denetler.

Ortalama CPU yükünü 10 dakikalık bir süre içinde % 70'den büyük olduğunda ayarlamak ölçek VM örnekleri sayısını artırır bir kural oluşturalım. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında artar. Ölçek kümesi VM örnekleri az sayıda'de, ayarladığınız `type` için *ChangeCount* ve artırmak `value` tarafından *1* veya *2* örnekleri. Çok sayıda VM örneği, % %10 20 veya bir artış ölçek kümesi VM örnekleri daha uygun olabilir.

Bu kural için aşağıdaki parametreleri kullanılır:

| Parametre         | Açıklama                                                                                                         | Değer           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | İzleme ve ölçek uygulamak için performans ölçüm Eylemler ayarlayın.                                                   | CPU Yüzdesi  |
| *timeGrain*       | Ne sıklıkta ölçümleri analiz için toplanır.                                                                   | 1 dakika        |
| *timeAggregation* | Toplanan ölçümleri analiz için nasıl toplanması gerektiğini tanımlar.                                                | Ortalama         |
| *timeWindow*      | Ölçüm ve eşik değerlerini karşılaştırılır önce izlenen süre miktarı.                                   | 10 dakika      |
| *işleci*        | Ölçüm verilerinin eşikle karşılaştırmak için kullanılan işleci.                                                     | Büyüktür    |
| *Eşik*       | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden değeri.                                                      | 70%             |
| *yönü*       | Ölçek kümesini yukarı veya aşağı kuralın geçerli olduğunda ölçeklendirmeniz gerekir tanımlar.                                             | Artır        |
| *türü*            | VM örneği sayısı yüzdesi miktarda değiştirilmesi gerektiğini belirtir.                                 | Yüzde değişikliği  |
| *değer*           | Kuralın uygulandığı zaman kaç VM örnekleri yukarı veya aşağı ölçeklendirilmelidir.                                            | 20              |
| *cooldown*        | Otomatik ölçeklendirme eylemleri etkili olması için zamanı sağlayacak şekilde kural önce beklenecek süreyi yeniden uygulanır. | 5 dakika       |

Aşağıdaki örnek VM örnek sayısını ölçeklendirmek için kural tanımlar. *MetricResourceUri* abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için önceden tanımlanmış değişkenleri kullanır:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "GreaterThan",
    "threshold": 70
  },
  "scaleAction": {
    "direction": "Increase",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-in"></a>İçinde otomatik olarak ölçeklendirmek için kural oluşturma
Bir akşam veya hafta sonu, uygulamayı isteğe bağlı azaltabilir. Bu azalmasına yükü bir süre boyunca tutarlı ise, Ölçek kümesi VM örnekleri sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu ölçek eylemi geçerli talebi karşılamak için gerekli örnek sayısı yalnızca çalışacak şekilde ayarlayın, Ölçek çalıştırmak için maliyeti azaltır.

Ortalama CPU yükünü 10 dakikalık bir süre içinde % 30 daha sonra düştüğünde ayarlamak ölçek VM örneği sayısı azalır başka bir kural oluşturun. Aşağıdaki örnek VM örnek sayısını ölçeklendirmek için kural tanımlar. *MetricResourceUri* abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için önceden tanımlanmış değişkenleri kullanır:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "LessThan",
    "threshold": 30
  },
  "scaleAction": {
    "direction": "Decrease",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Otomatik ölçeklendirme kurallarını ölçek kümesine Uygula
Son adım otomatik ölçeklendirme profili ve kuralları ölçek kümenize uygulamaktır. Ölçek sonra uygulama talebe göre otomatik olarak içeri veya dışarı ölçeklendirebilirsiniz. Otomatik ölçeklendirme profili ile uygulama [az İzleyici otomatik ölçeklendirme ayarları oluşturmak](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) gibi. Tam JSON profili ve önceki kısımlarında not ettiğiniz kuralları kullanır.

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "autoscale by percentage based on CPU usage",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Ölçek kümesi örneği monitör sayısı
Sayısı ve VM örneği durumunu görmek için ölçek kümesi örneklerinin bir listesini görüntüleyin. [az vmss listesi-örneklerini](/cli/azure/vmss#az_vmss_list_instances). Ölçeği otomatik olarak ayarla çıkışı ölçeklendirir veya ölçek içinde otomatik olarak ölçeklendirir gibi sağlamayı VM örneği sağlama durumunda durumu gösterir. Aşağıdaki örnek ölçeği adlandırılmış Ayarla VM örneği durumunun görünümleri *myScaleSet* kaynak grubunda adlı *myResourceGroup*:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```


## <a name="autoscale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme
Önceki örneklerde otomatik olarak içeri veya dışarı temel ana ölçümleri CPU kullanımı gibi kümesiyle ölçeği genişletilmiş. Zamanlamada göre otomatik ölçeklendirme kuralları da oluşturabilirsiniz. Zamanlama tabanlı kurallar çekirdek çalışma saatleri gibi uygulama talep beklenen bir artış öncesinde VM örneklerinin sayısını otomatik olarak ölçeğini ve örnek sayısı daha az düşündüğünüz bir anda otomatik olarak ölçekleme izin ver hafta sonu gibi talep.

Zamanlama tabanlı otomatik ölçeklendirme kurallarını kullanmak için bir sabit başlangıç ve bitiş zaman penceresi için çalıştırmak üzere VM örneklerinin sayısını tanımlayan bir JSON profili oluşturun. Aşağıdaki örnek, bir kural ölçeği genişletme tanımlar *10* adresindeki örnekleri *9* saatlerinde her iş günü (Pazartesi-Cuma).

```json
{
  "name": "Scale out during each work day",
  "capacity": {
      "minimum": "10",
      "maximum": "10",
      "default": "10"
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
              9
          ],
          "minutes": [
              0
          ]
      }
  }
}
```

İçinde sırasında Akşam ölçeklendirmek için daha düşük bir VM örnekleri ve uygun başlangıç saati sayısını başka bir kural oluşturun.

Aşağıdaki tam bir örnek ölçek genişletme ve ölçek kuralları tanımlar ve ardından otomatik ölçeklendirme profili ile geçerlidir [az İzleyici otomatik ölçeklendirme ayarları oluşturmak](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create). Bu örnek önceki örneklerde oluşturulan otomatik ölçeklendirme ölçüm tabanlı kurallar üzerine yazar. *MetricResourceUri* abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için önceden tanımlanmış değişkenleri kullanır:

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "Scale out during each work day",
          "capacity": {
            "minimum": "10",
            "maximum": "10",
            "default": "10"
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
                9
              ],
              "minutes": [
                0
              ]
            }
          }
        },
        {
          "name": "Scale in during the evening",
          "capacity": {
            "minimum": "3",
            "maximum": "3",
            "default": "3"
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
                18
              ],
              "minutes": [
                0
              ]
            }
          }
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yatay olarak ölçekleme ve artırmak veya azaltmak için otomatik ölçeklendirme kurallarını kullanmayı öğrendiniz *numarası* , Ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırmak veya azaltmak VM örneği için ölçeklendirmek *boyutu*. Daha fazla bilgi için bkz: [dikey otomatik ölçeklendirme sanal makine ölçek kümeleri ile](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Üzerindeki VM örneklerinize yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek ayarlar Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kurallarını taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanmak](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Ayrıca [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için kullanım denetim günlüklerini](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
