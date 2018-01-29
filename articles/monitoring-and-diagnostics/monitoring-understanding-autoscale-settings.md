---
title: "Otomatik ölçeklendirme ayarlarını anlama | Microsoft Docs"
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
ms.openlocfilehash: 79602cf053d834bf3d6dc6b4d5568637b179d5c7
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="understand-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını anlama
Otomatik ölçeklendirme ayarları, uygulamanızın fluctuating yükü işlemek üzere çalışan kaynakları sağ miktarına sahip olmanızı sağlar. Yükleme veya performans belirtmek veya bir zamanlanmış tarih ve saatte tetikleyicisi ölçümleri temel tetiklenmesi için otomatik ölçeklendirme ayarlarını yapılandırabilirsiniz. Bu makalede bir ayrıntılı bir otomatik ölçeklendirme ayarı anatomisi inceler. Makaleyi şeması ve bir ayarın özelliklerini anlayarak başlar, ardından yapılandırılabilir ve son olarak nasıl otomatik ölçeklendirme herhangi bir zamanda çalıştırmak için hangi profilin değerlendirir anlatılmaktadır farklı profil türleri anlatılmaktadır.

## <a name="autoscale-setting-schema"></a>Otomatik ölçeklendirme ayarı şeması
Otomatik ölçeklendirme ayarı şema göstermek için aşağıdaki otomatik ölçeklendirme ayarı kullanılır. Bu otomatik ölçeklendirme ayarı olduğunu dikkate almak önemlidir:
- Bir profili 
- Bu iki ölçüm kuralları bu profilde vardır; bir genişleme ve ölçek için.
- Sanal makine ölçek kümesinin ortalama Yüzde CPU ölçüm son 10 min % 85'den büyük olduğunda genişleme kuralı tetiklenir.
- Sanal makine ölçek kümesinin ortalama % 60'den az olduğunda ölçek bileşenini kuralı tetiklenir geçen dakika.

> [!NOTE]
> Bir ayar birden çok profil yoksa, atlamak [profilleri](#autoscale-profiles) daha fazla bilgi için bölüm.
> Bir profil de birden çok genişleme sağlayabilirsiniz kurallarını ve tanımlanmış, Ölçek bileşenini kurallarını atlamak için [değerlendirme bölüm](#autoscale-evaluation) nasıl değerlendirildiği görmek için

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
| Profili | ad | Profilin adını, profil tanımanıza yardımcı olacak herhangi bir ad seçebilirsiniz. |
| Profili | Capacity.Maximum | İzin verilen en fazla kapasitesi. Bu otomatik ölçeklendirme sağlar bu profili yürütülürken kaynağınız bu sayının üstündeki ölçeklenmez. |
| Profili | Capacity.minimum | İzin verilen en az kapasitesi. Bu otomatik ölçeklendirme sağlar bu profili yürütülürken kaynağınız bu sayının altına ölçeklenmez. |
| Profili | Capacity.default | (Bu durumda, "vmss1" cpu) kaynak ölçümü okunurken bir sorun oluştu ve Geçerli kapasitenin varsayılan kapasitenin altında sonra kaynak kullanılabilirliğini sağlamak için ise, otomatik ölçeklendirme ölçekler varsayılan genişletme. Geçerli kapasitenin zaten varsayılan kapasitesinden yüksek ise, otomatik ölçeklendirme ölçek bileşenini değil. |
| Profili | rules | Otomatik ölçeklendirme profili kurallarını kullanarak maksimum ve minimum kapasiteleri arasında otomatik olarak ölçeklendirir. Bir profili birden çok kural bulunabilir. İki kural, bir zaman genişleme belirlemek için ve diğer zaman ölçek bileşenini belirlemek için temel senaryo olmalıdır. |
| kural | metricTrigger | Kural ölçüm koşulunu tanımlar. |
| metricTrigger | metricName | Ölçüm adı. |
| metricTrigger |  metricResourceUri | Ölçüm yayar kaynak kaynak kimliği. Çoğu durumda, genişletilmiş kaynak ile aynı olur. Bazı durumlarda, farklı olabilir, örneğin bir depolama sıradaki iletilerin sayısına dayalı olarak bir sanal makine ölçek kümesi ölçeklendirebilirsiniz. |
| metricTrigger | timeGrain | Ölçüm örnekleme süresi. Örneğin, TimeGrain "ölçümleri olmalıdır anlamına gelir toplanan kullanarak her 1 dakika içinde"istatistiği."belirtilen toplama yöntemi PT1M" = |
| metricTrigger | İstatistiği | Toplama yöntemi timeGrain süre içinde. Örneğin, istatistik = "Ortalama" ve timeGrain ölçümleri olmalıdır "PT1M" anlamına gelir = 1 dakikada ortalama gerçekleştirerek birleştirilir. Bu özellik, ölçüm nasıl örneklenen belirler. |
| metricTrigger | timeWindow | Geri ölçümlerini aramak için süre miktarı. Örneğin, timeWindow = "PT10M" anlamına gelir otomatik ölçeklendirme her çalıştığında, son 10 dakika için ölçümleri sorgular. Zaman penceresi ölçümlerinizi normalleştirilmiş sağlar ve geçici ani tepki önler. |
| metricTrigger | timeAggregation | Örneklenen ölçümleri toplamak için kullanılan toplama yöntemi. Örneğin, TimeAggregation = "Ortalama" toplama örneklenen ölçümleri ortalama gerçekleştirerek. Yukarıdaki örnekte on 1 dakikalık örnekleri almak ve bunları ortalaması. |
| kural | scaleAction | Kural metricTrigger tetiklendiğinde yapılacak eylem. |
| scaleAction | yön | "Genişleme için artırın", "ölçek için azaltmak"|
| scaleAction | değer | Artırmak veya kaynak kapasitesinin azaltmak için ne kadar |
| scaleAction | cooldown | Yeniden ölçeklendirme önce bir ölçeklendirme işlemi sonra beklenecek süre miktarı. Örneğin, varsa cooldown bir ölçeklendirme işlemi tamamlandıktan sonra otomatik ölçeklendirme yeniden için başka bir 10 dakika ölçeklendirme çalışmaz sonra "PT10M" =. Cooldown ekleme veya kaldırma örneklerinin sonra kararlı ölçümleri izin vermektir. |

## <a name="autoscale-profiles"></a>Otomatik ölçeklendirme profili

Üç tür otomatik ölçeklendirme profili vardır:

1. **Normal profil:** en yaygın profil. Haftanın gününü veya belirli bir günü farklı dayalı kaynağınız ölçeklendirme gerekmiyorsa, daha sonra yalnızca normal bir otomatik ölçeklendirme ayarının profilinde ayarlamanız gerekir. Bu profil, genişletmek ne zaman ve ne zaman ölçek bileşenini dikte ölçüm kuralları ile sonra yapılandırılabilir. Yalnızca tanımlı bir normal profili olması gerekir.

    Bu makalede kullanılan örnek profili, normal bir profili örneğidir. Kaynağınız için bir statik örnek sayısı için ölçeklendirme profili ayarlamak mümkündür unutmayın.

2. **Tarih profili sabit:** tanımlanan normal profiliyle 26 aralık 2017'üzerinde (Pasifik Saati) önce yaklaşan önemli bir olayı varsa ve o gün farklı olabilir, ancak hala aynı ölçülerine ölçeklendirmek için kaynak minimum/maksimum kapasiteleri istiyorsanız düşünelim . Bu durumda, bir sabit tarih profili, ayarın profiller listesine eklemeniz gerekir. Profil, yalnızca olayın günü çalışacak şekilde yapılandırılır. Diğer herhangi bir gün için normal profili yürütülür.

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
    
3. **Yineleme profil:** bu profili haftanın belirli bir günde her zaman kullanıldığından emin olmak bu tür profilinde sağlar. Yineleme profilleri yalnızca bir başlangıç saati, sonuç olarak bunlar kadar sonraki yinelenme profili çalıştıran veya sabit tarih profili başlatmak üzere ayarlanır. Bir otomatik ölçeklendirme yalnızca bir yineleme profiliyle ayarı aynı ayarda tanımlanan normal bir profil olsa bile bu profili yürütür. Bu profili kullanımını aşağıdaki iki örnek gösterilmektedir:

    **Örnek 1 - haftanın günü vs. Hafta sonları** hafta sonu 4 olması için en fazla kapasite istiyor ancak günlerinde, daha fazla yük beklediğiniz bu yana 10 olması için en fazla kapasite istediğiniz varsayalım. Bu durumda, ayarınız iki yineleme profilleri, bir hafta sonları ve diğer günlerinde çalıştırılacak içerecektir.
    Ayarı şöyle olabilir:

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

    Önceki ayarı bakarak, fark edeceksiniz her yineleme profiline sahip bir zamanlama, yürütme profili yeniden başlatıldığında bu zamanlamanın belirler. Başka bir profili yürütme süresi olduğunda çalıştırma profili durdurur.

    Örneğin, önceki ayarında "weekdayProfile" Pazartesi günü 12.am yürütme bu profili başlatır anlamına gelen 12 a.m. Pazartesi günü başlanacak ayarlanır. Bunu çalıştırmaya başlamak için "weekendProfile" zamanlandığı Cumartesi kadar 12a.m. yürütmeye devam eder.

    **Örnek 2 - iş saatleri** başka bir örnek atalım, belki de ölçüm eşiği olmasını istediğiniz = iş saatlerinde 9: 00 ' x' -17: 00 ve 17: 00'den 9: 00 sonraki gün ölçüm eşiği 'y' olmasını istiyorsunuz.
    Ayarı şöyle olabilir:
    
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
    
    Önceki ayarı bakarak "businessHoursProfile" Pazartesi günü 9'da yürütülürken başlar. ve 17: 00 kadar yürütme tutar ne zaman olduğu için "nonBusinessHoursProfile" yürütme başlatır. Kadar 9: 00 "nonBusinessHoursProfile" yürütür Salı ve "businessHoursProfile" devreye girer. Cuma 17: 00, bu noktada, bu süreç boyunca tüm Pazartesi 9: 00 "nonBusinessHoursProfile" yürütür kadar bu tekrarlar "businessHoursProfile" Pazartesi 9: 00 yürütme başlamıyor beri
    
> [!Note]
> Azure portalında otomatik ölçeklendirme UX yinelenme profilleri için son kez zorlar ve otomatik ölçeklendirme ayarının varsayılan profili yinelenme profilleri Between yürütme başlar.
    
## <a name="autoscale-evaluation"></a>Otomatik ölçeklendirme değerlendirme
Bu otomatik ölçeklendirme verilen ayarlar birden çok otomatik ölçeklendirme profili olabilir ve her bir profil otomatik ölçeklendirme ayarının nasıl değerlendirilir anlamak önemlidir birden fazla ölçüm kuralları olabilir. Değerleri ve ölçüm kurallarını profilinde profili otomatik ölçeklendirme seçtikten sonra geçerli profil seçerek başlar otomatik ölçeklendirme iş çalıştırmaları değerlendirir min, her zaman en büyük ve bir ölçek eylemi gerekli olup olmadığını belirler.

### <a name="which-profile-will-autoscale-pick"></a>Hangi profil otomatik ölçeklendirme seçer?
- Otomatik ölçeklendirme İlk bakar herhangi için varsa, şimdi çalışacak şekilde yapılandırılmış tarih profili sabit, otomatik ölçeklendirme yürütür. Çalıştırmaması gerektiği birden çok sabit tarih profili varsa, otomatik ölçeklendirme ilk seçin.
- Hiçbir sabit tarih profili varsa, otomatik ölçeklendirme yinelenme profilleri arar bulundu, ardından bunu yürütülmeden durumunda.
- Varsa sabit yok veya yineleme profilleri sonra otomatik ölçeklendirme normal profili yürütün.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Otomatik ölçeklendirme birden çok kural nasıl değerlendirmek?

Otomatik ölçek çalıştırmak için hangi profilin beklenen belirledikten sonra profildeki tüm genişleme kural değerlendirerek başlatır (yönü kurallarıyla "Artırmak" =).
- Bir veya daha fazla ölçeklendirme kurallarını tetiklenen, otomatik ölçeklendirme bu kuralların her scaleAction tarafından belirlenen yeni kapasite hesaplar. Ardından, ölçeklendirme hizmet kullanılabilirliğini sağlamak için bu kapasiteleri en büyük genişletme.
- Örneğin: 10 geçerli kapasitesine sahip bir sanal makine ölçek kümesi varsa ve iki genişleme kuralları; biri, % 10 kapasitesini artırır ve biri, 3 ile kapasitesini artırır. İlk kural yeni 11 kapasitelerinde neden olur ve ikinci kural kapasite 13 neden olur. Hizmet kullanılabilirliğini sağlamak için otomatik ölçeklendirme maksimum kapasite, böylece ikinci kuralı sonuçları eylem seçilen seçer.

Genişleme kural tetiklenen varsa, otomatik ölçeklendirme tüm ölçek bileşenini kuralları değerlendirir (yönü kurallarıyla = "Azaltmak"). Tüm ölçek bileşenini kuralları tetiklenen otomatik ölçeklendirme yalnızca bir ölçek eylemi alır.
- Otomatik ölçeklendirme bu kuralların her scaleAction tarafından belirlenen yeni kapasite hesaplar. Ardından hizmet kullanılabilirliğini sağlamak için bu kapasiteleri maksimum içinde sonuçları ölçek eylemi seçer.
- Örneğin: 10 geçerli kapasitesine sahip bir sanal makine ölçek kümesi varsa ve iki ölçek bileşenini kurallar vardır; biri, % 50 kapasite azalır ve biri, 3 ile kapasite azalır. İlk kural 5'in yeni bir kapasiteden neden olur ve ikinci kural 7'in bir kapasite neden olur. Hizmet kullanılabilirliğini sağlamak için otomatik ölçeklendirme maksimum kapasite, böylece ikinci kuralı sonuçları eylem seçilen seçer.

## <a name="next-steps"></a>Sonraki Adımlar
Bilgi edinmek için otomatik ölçeklendirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Otomatik Ölçeklendirmeye Genel Bakış](monitoring-overview-autoscale.md)
* [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](insights-autoscale-common-metrics.md)
* [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](insights-autoscale-best-practices.md)
* [E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](insights-autoscale-to-webhook-email.md)
* [Otomatik ölçeklendirme REST API'si](https://msdn.microsoft.com/library/dn931953.aspx)
