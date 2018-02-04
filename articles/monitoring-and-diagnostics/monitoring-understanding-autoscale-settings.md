---
title: "Otomatik ölçeklendirme ayarlarını Azure anlama | Microsoft Docs"
description: "Otomatik ölçeklendirme ayarlarını ve nasıl çalıştığını ayrıntılı bir dökümü."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: 73c79ec4ee1beb5220e088421c78ffffd932eef1
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="understand-autoscale-settings"></a>Otomatik Ölçeklendirme ayarlarını anlama
Otomatik ölçeklendirme ayarları, uygulamanızın fluctuating yükü işlemek üzere çalışan kaynakları sağ miktarına sahip sağlamaya yardımcı olur. Zamanlanmış tarih ve saatte yük veya performans ölçümleri temelinde veya tetiklenen tetiklenmesi için otomatik ölçeklendirme ayarlarını yapılandırabilirsiniz. Bu makalede bir ayrıntılı bir otomatik ölçeklendirme ayarı anatomisi inceler. Makaleyi ayarı özelliklerini ve şema ile başlar ve farklı bir profil türleri aracılığıyla yapılandırılabilir anlatılmaktadır. Son olarak, belirli bir zamanda çalıştırmak için hangi profilin azure'daki otomatik ölçeklendirme özelliği nasıl değerlendirir anlatılmaktadır.

## <a name="autoscale-setting-schema"></a>Otomatik ölçeklendirme ayarı şeması
Otomatik ölçeklendirme ayarı şema göstermek için aşağıdaki otomatik ölçeklendirme ayarı kullanılır. Bu otomatik ölçeklendirme ayarı olduğunu dikkate almak önemlidir:
- Bir profili. 
- Bu profildeki iki ölçüm kuralları: genişleme için diğeri ölçeklemek için.
  - Sanal makine ölçek kümesinin ortalama Yüzde CPU ölçüm son 10 dakika için yüzde 85 ' büyük olduğunda genişleme kuralı tetiklenir.
  - Sanal makine ölçek kümesinin ortalama yüzde 60'den az olduğunda ölçek bileşenini kuralı tetiklenir geçen dakika.

> [!NOTE]
> Bir ayar birden çok profil var. Daha fazla bilgi için bkz: [profilleri](#autoscale-profiles) bölümü. Bir profil, birden çok genişleme kuralları ve ölçek içinde tanımlanan kurallar da sahip olabilirsiniz. Nasıl değerlendirildiği görmek için bkz: [değerlendirme](#autoscale-evaluation) bölümü.

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
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
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
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
      }
    ]
  }
}
```

| Section | Öğe adı | Açıklama |
| --- | --- | --- |
| Ayar | Kimlik | Otomatik ölçeklendirme ayarının kaynak kimliği Otomatik ölçeklendirme, bir Azure Resource Manager kaynak ayarlardır. |
| Ayar | ad | Otomatik ölçeklendirme ayarı adı. |
| Ayar | location | Otomatik ölçeklendirme ayarının konumu. Bu konum ölçeklendirilmiş kaynak konumundan farklı olabilir. |
| properties | targetResourceUri | Genişletilmiş kaynak kaynak kimliği. Yalnızca kaynak başına bir otomatik ölçeklendirme ayarına sahip olabilir. |
| properties | Profilleri | Otomatik ölçeklendirme ayarı bir veya daha fazla profil oluşur. Otomatik ölçeklendirme altyapısı, her çalıştığında bir profil yürütür. |
| Profili | ad | Profil adı. Profil tanımanıza yardımcı olacak herhangi bir ad seçebilirsiniz. |
| Profili | Capacity.Maximum | İzin verilen en fazla kapasitesi. Bu profili yürütülürken otomatik ölçeklendirme, kaynağınızın bu sayının üstündeki ölçeklenmez sağlar. |
| Profili | Capacity.minimum | İzin verilen en az kapasitesi. Bu profili yürütülürken otomatik ölçeklendirme, kaynağınızın bu sayının altına ölçeklenmez sağlar. |
| Profili | Capacity.default | (Bu durumda, "vmss1" CPU) kaynak ölçümü okunurken bir sorun oluştu ve Geçerli kapasitenin altında varsayılan ise, otomatik ölçeklendirme çıkışı varsayılan olarak ölçeklendirir. Bu kaynak kullanılabilirliğini sağlamak için yapılır. Geçerli kapasitenin zaten varsayılan kapasitesinden yüksek ise, otomatik ölçeklendirme, ölçeklendirme sağlamaz. |
| Profili | rules | Otomatik ölçeklendirme profili kuralları kullanılarak maksimum ve minimum kapasiteleri arasında otomatik olarak ölçeklendirir. Bir profili birden çok kural bulunabilir. Genellikle iki kural vardır: zaman ölçeğini belirlemek için diğeri zaman içinde ölçeklendirmek belirlemek için. |
| kural | metricTrigger | Kural ölçüm koşulunu tanımlar. |
| metricTrigger | metricName | Ölçüm adı. |
| metricTrigger |  metricResourceUri | Ölçüm yayar kaynak kaynak kimliği. Çoğu durumda, genişletilmiş kaynak ile aynı olur. Bazı durumlarda, farklı olabilir. Örneğin, bir depolama sıradaki iletilerin sayısına dayalı olarak bir sanal makine ölçek kümesi ölçeklendirebilirsiniz. |
| metricTrigger | timeGrain | Ölçüm örnekleme süresi. Örneğin, **TimeGrain "PT1M" =** istatistiği öğesinde belirtilen toplama yöntemi kullanarak ölçümleri 1 dakikada toplanması anlamına gelir. |
| metricTrigger | İstatistiği | Toplama yöntemi timeGrain süre içinde. Örneğin, **istatistiği = "Ortalama"** ve **timeGrain "PT1M" =** ortalama gerçekleştirerek ölçümleri 1 dakikada toplanması anlamına gelir. Bu özellik, ölçüm nasıl örneklenen belirler. |
| metricTrigger | timeWindow | Geri ölçümlerini aramak için süre miktarı. Örneğin, **timeWindow "PT10M" =** otomatik ölçeklendirme her çalıştığında, son 10 dakika için ölçümleri sorgular anlamına gelir. Ölçümlerinizi normalleştirilmiş, zaman penceresi sağlar ve geçici ani tepki önler. |
| metricTrigger | timeAggregation | Örneklenen ölçümleri toplamak için kullanılan toplama yöntemi. Örneğin, **TimeAggregation = "Ortalama"** ortalama gerçekleştirerek örneklenen ölçümleri toplamak. Önceki örnekte, on 1 dakikalık örnekleri almak ve bunları ortalaması. |
| kural | scaleAction | Kural metricTrigger tetiklendiğinde yapılacak eylem. |
| scaleAction | yön | "Ölçeğini veya"Azaltmak"için ölçek artırma".|
| scaleAction | değer | Artırmak veya kaynak kapasitesinin azaltmak için ne kadar. |
| scaleAction | cooldown | Yeniden ölçeklendirme önce bir ölçeklendirme işlemi sonra beklenecek süre miktarı. Örneğin, varsa **cooldown "PT10M" =**, yeniden için başka bir 10 dakika ölçeklendirmek otomatik ölçeklendirme denemez. Cooldown ekleme veya kaldırma örneklerinin sonra kararlı ölçümleri izin vermektir. |

## <a name="autoscale-profiles"></a>Otomatik ölçeklendirme profili

Üç tür otomatik ölçeklendirme profili vardır:

- **Normal profil:** en yaygın profil. Haftanın gününü veya belirli bir günü göre kaynağınız ölçeklendirme gerekmiyorsa, normal bir profili kullanabilirsiniz. Bu profil, genişletmek ne zaman ve ne zaman içinde ölçeklendirmek dikte ölçüm kuralları ile sonra yapılandırılabilir. Yalnızca tanımlı bir normal profili olması gerekir.

    Bu makalede kullanılan örnek profili, normal bir profili örneğidir. Kaynağınız için bir statik örnek sayısı için ölçeklendirme profili ayarlamak mümkündür unutmayın.

- **Tarih profili sabit:** özel durumlar için bu profilidir. Örneğin, 26 aralık 2017'üzerinde (Pasifik Saati) önce yaklaşan önemli olaya sahip varsayalım. Bu günlük, ancak hala aynı ölçümleri ölçekte farklı olması için minimum ve maksimum kapasiteleri kaynağınız istiyor. Bu durumda, bir sabit tarih profili, ayarın profiller listesine eklemeniz gerekir. Profil, yalnızca olayın günü çalışacak şekilde yapılandırılır. Diğer herhangi bir gün için otomatik ölçeklendirme normal profili kullanır.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
- **Yineleme profil:** bu profili haftanın belirli bir günde her zaman kullanıldığından emin olmak bu tür profilinde sağlar. Yineleme profilleri yalnızca bir başlangıç saatine sahip. Sonraki yinelenme profili kadar çalıştırın veya sabit tarih profili başlatmak üzere ayarlanır. Aynı ayarda tanımlanan normal bir profil olsa bile bu profili bir otomatik ölçeklendirme ayarı yalnızca bir yineleme profili ile çalışır. Bu profili nasıl kullanıldığını aşağıdaki iki örnek gösterilmektedir:

    **Örnek 1: Hafta sonları karşılaştırması**
    
    Hafta sonu, 4, maksimum kapasite istediğinizi düşünelim. Daha fazla yük beklediğiniz çünkü günlerinde, 10 olması için en fazla kapasite istiyorsunuz. Bu durumda, ayarınız iki yineleme profilleri, bir hafta sonları ve diğer günlerinde çalıştırılacak içerecektir.
    Ayar şu şekildedir:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    Önceki ayarı, her bir yineleme profili bir zamanlama olduğunu gösterir. Bu zamanlamayı profili başladığında belirler çalışıyor. Başka bir profili çalıştırma süresi olduğunda profili durdurur.

    Örneğin, önceki ayarında, 12: 00'da Pazartesi günü başlatmak için "weekdayProfile" ayarlanır. Bu profili Pazartesi günü 12: 00'da çalışmaya başladıktan anlamına gelir. 12:00 "weekendProfile" çalıştıran başlaması için ne zaman zamanlanır'de, Cumartesi kadar devam eder.

    **Örnek 2: iş saatleri**
    
    İş saatleri (09:00:00 ila 5:00) bir ölçüm eşiği ve diğer tüm saatler için farklı bir sahip olmasını istediğiniz varsayalım. Ayarı şöyle olabilir:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
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
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Önceki ayarı "businessHoursProfile" Pazartesi günü 9: 00'da çalışan başlar ve 17: 00'dan için devam gösterir. Ne zaman olan "nonBusinessHoursProfile" çalıştıran başlatır. 09:00:00 kadar Salı "nonBusinessHoursProfile" çalışır ve ardından "businessHoursProfile" yeniden devreye girer. Bu, 17: 00'dan Cuma kadar tekrarlar. Bu noktada, "nonBusinessHoursProfile" tüm Pazartesi 9: 00'da çalışır.
    
> [!Note]
> Azure portal otomatik ölçeklendirme kullanıcı arabiriminde yinelenme profilleri için son kez zorlar ve otomatik ölçeklendirme ayarının varsayılan profili yinelenme profilleri arasında çalışan başlar.
    
## <a name="autoscale-evaluation"></a>Otomatik ölçeklendirme değerlendirme
Otomatik ölçeklendirme ayarlarını birden çok profil olabilir ve her bir profil ölçüm birden çok kural bulunabilir o, otomatik ölçeklendirme ayarının nasıl değerlendirilir anlamak önemlidir. Otomatik ölçeklendirme işi her çalıştığında, geçerli profil seçerek başlar. Ardından otomatik ölçeklendirme, en düşük ve en yüksek değerler ve tüm profilinde ölçüm kurallarını değerlendirir ve bir ölçek eylemi gerekli olup olmadığını belirler.

### <a name="which-profile-will-autoscale-pick"></a>Hangi profil otomatik ölçeklendirme seçer?

Otomatik ölçeklendirme profili seçmek için aşağıdaki sırayı kullanır:
1. Şimdi çalıştırmak için yapılandırılmış herhangi bir sabit tarih profil için ilk kez görünüyor. Varsa, otomatik ölçeklendirme dosya çalışır. Çalıştırmaması gerektiği birden çok sabit tarih profili varsa, otomatik ölçeklendirme ilk seçer.
2. Hiçbir sabit tarih profili varsa, otomatik ölçeklendirme yinelenme profilleri arar. Bir yineleme profili bulunursa, onu çalıştırır.
3. Sabit tarih veya yineleme profilleri yoksa, otomatik ölçeklendirme normal profili çalışır.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Otomatik ölçeklendirme birden çok kural nasıl değerlendirmek?

Otomatik ölçek çalıştırmak için hangi profilin belirledikten sonra profilindeki tüm genişleme kuralları değerlendirir (kurallarıyla bunlar **yönü "Artırmak" =**).

Bir veya daha fazla ölçeklendirme kurallarını tetiklenen, otomatik ölçeklendirme tarafından belirlenen yeni kapasite hesaplar **scaleAction** bu kuralların her biri. Sonra bunu çıkışı hizmet kullanılabilirliğini sağlamak için bu kapasiteleri maksimum kadar ölçeklendirir.

Örneğin, var. 10 geçerli kapasitesine bir sanal makine ölçek kümesi varsayalım. İki genişleme kurallar vardır: yüzde 10 kapasitesini artırır ve 3 sayıları tarafından kapasitesini artırır. İlk kural yeni 11 kapasitelerinde neden olur ve ikinci kural kapasite 13 neden olur. Hizmet kullanılabilirliğini sağlamak için otomatik ölçeklendirme maksimum kapasitede, böylece ikinci kuralı sonuçları eylem seçilen seçer.

Genişleme kural tetiklenen varsa, otomatik ölçeklendirme tüm ölçek bileşenini kuralları değerlendirir (ile kurallar **yönü "Azaltmak" =**). Tüm ölçek bileşenini kuralları tetiklenen otomatik ölçeklendirme yalnızca bir ölçek eylemi alır.

Otomatik ölçeklendirme tarafından belirlenen yeni kapasite hesaplar **scaleAction** bu kuralların her biri. Ardından hizmet kullanılabilirliğini sağlamak için bu kapasiteleri maksimum içinde sonuçları ölçek eylemi seçer.

Örneğin, var. 10 geçerli kapasitesine bir sanal makine ölçek kümesi varsayalım. İki ölçek kurallar vardır: yüzde 50 kapasite azalır ve kapasite tarafından 3 sayıları azaltır. İlk kural 5'in yeni bir kapasiteden neden olur ve ikinci kural 7'in bir kapasite neden olur. Hizmet kullanılabilirliğini sağlamak için otomatik ölçeklendirme maksimum kapasitede, böylece ikinci kuralı sonuçları eylem seçilen seçer.

## <a name="next-steps"></a>Sonraki adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi için aşağıdaki başvurarak öğrenin:

* [Otomatik Ölçeklendirmeye Genel Bakış](monitoring-overview-autoscale.md)
* [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](insights-autoscale-common-metrics.md)
* [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](insights-autoscale-best-practices.md)
* [E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](insights-autoscale-to-webhook-email.md)
* [Otomatik ölçeklendirme REST API'si](https://msdn.microsoft.com/library/dn931953.aspx)
