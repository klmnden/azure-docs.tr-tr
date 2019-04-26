---
title: Service Fabric Küme Kaynak Yöneticisi'nde azaltma | Microsoft Docs
description: Service Fabric küme kaynak yöneticisi tarafından sağlanan kısıtlamalar yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4abc3e4a28b8b98070affe19b7b7ca38f904c45b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384978"
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Service Fabric Küme Kaynak Yöneticisi azaltma
Kümenin küme kaynak yöneticisi doğru şekilde yapılandırmış olduğunuz bile kesintiye. Örneğin, bir yükseltme sırasında oluştuysa ne olacağını eşzamanlı düğüm ve hata etki alanı hataları - olabilir? Küme Kaynak Yöneticisi her zaman her şeyi düzeltmek yeniden düzenleme ve küme düzeltme çalışılırken kümenin kaynak tüketmeye çalışır. Kısıtlamalar yardımcı küme kaynakları sabitlemek için - kullanabilmesi için bir backstop sağlamak düğümleri geri dönün, düzeltilmiş BITS dağıtılan ağ bölümlerinden onarımı.

Durumlarda bu tür yardımcı olmak için çeşitli kısıtlamalar Service Fabric Küme Kaynak Yöneticisi'ni içerir. Bu kısıtlamalar tüm oldukça büyük hammers ' dir. Genellikle, dikkatli planlama ve sınama yapılmadan değiştirilmesi olmamalıdır.

Küme Kaynak Yöneticisi'nin kısıtlamalar değiştirirseniz, beklenen gerçek yüke ayarlamak. Küme Sabitle bazı durumlarda daha uzun sürer geldiğini bile bazı kısıtlamalar yerinde ihtiyacınız belirleyebilir. Test kısıtlamalar için doğru değerleri belirlemek için gereklidir. Kısıtlamalar makul bir sürede değişikliklerine yanıt verme kümesine izin vermek için yeterince yüksek ve gerçekten çok fazla kaynak tüketimi önlemek için yeterince düşük olması gerekir. 

Çoğu zaman müşterilerin gördük zaten kısıtlanmış bir kaynak ortamında olduğundan bırakıldı kısıtlamalar kullanın. Bazı örnekler tek tek düğümlere veya paralel işleme sınırlamaları nedeniyle çok sayıda durum bilgisi olan çoğaltma oluşturmak mümkün olmayan diskler için sınırlı ağ bant genişliği olacaktır. Kısıtlamaları, başarısız veya yavaş işlem neden olan bu kaynaklar, işlemleri sık zora. Müşteriler kısıtlamalar kullanılan ve süreyi genişletme olduğunda bu durumlarda, kümenin kararlı bir duruma ulaşmak için alır. Müşteriler ayrıca, düşük genel güvenilirlik kısıtlanan sırada çalışan son anladım.


## <a name="configuring-the-throttles"></a>Kısıtlamalar yapılandırma

Service Fabric çoğaltma hareketleri sayısını azaltma için iki mekanizma vardır. Service Fabric 5.7 önce var olan varsayılan bir mekanizma azaltma izin taşımaların mutlak bir sayı temsil eder. Bu, her boyuttaki kümeleri için çalışmaz. Özellikle büyük kümeler için varsayılan değer, daha küçük kümelerinde etkisi yaparken gerekli olsa bile Dengeleme aşağı önemli ölçüde yavaşlatmasını çok küçük olabilir. Önceki Bu mekanizma yüzde tabanlı kısıtlanarak, hizmetleri ve düğüm sayısını düzenli olarak değiştiği dinamik kümeleriyle daha iyi ölçeklenir yerini almıştır.

Kısıtlamalar kümelerindeki çoğaltmaların sayısı yüzdesi temel alır. Yüzde tabanlı kısıtlamalar etkinleştirme kuralı belirtme: "hareket % çoğaltmalarının birden fazla 10 10 dakikalık bir aralığı", örneğin.

Yüzde tabanlı azaltma için yapılandırma ayarları şunlardır:

  - GlobalMovementThrottleThresholdPercentage - hareketleri kümedeki herhangi bir zamanda, izin verilen en fazla sayısı, yinelemeler kümedeki toplam sayısının yüzdesi olarak ifade edilen. 0 sınır olmadığını. Varsayılan değer 0’dır. Bu ayar ve GlobalMovementThrottleThreshold belirtilirse, ardından daha pasif sınırı kullanılır.
  - GlobalMovementThrottleThresholdPercentageForPlacement - hareketleri çoğaltmaları kümedeki toplam sayısının yüzdesi olarak ifade edilen yerleştirme aşamasında, izin verilen en fazla sayısı. 0 sınır olmadığını. Varsayılan değer 0’dır. Bu ayar ve GlobalMovementThrottleThresholdForPlacement belirtilirse, ardından daha pasif sınırı kullanılır.
  - GlobalMovementThrottleThresholdPercentageForBalancing - hareketleri çoğaltmaları kümedeki toplam sayısının yüzdesi olarak ifade edilen karşı aşamasında, izin verilen en fazla sayısı. 0 sınır olmadığını. Varsayılan değer 0’dır. Bu ayar ve GlobalMovementThrottleThresholdForBalancing belirtilirse, ardından daha pasif sınırı kullanılır.

Azaltma yüzdesi belirtirken, %5 0,05 belirtmeniz gerekir. Bu kısıtlamalar yönetilen saniye cinsinden belirtilen GlobalMovementThrottleCountingInterval aralığıdır.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Varsayılan sayısı temel kısıtlamalar
Daha eski kümeleri sahip veya bu yapılandırmalar beri yükseltilmiş kümelerinde hala korur durumunda bu bilgiler verilmiştir. Genel olarak, bunlar yüzde tabanlı kısıtlamalar ile yukarıdaki değiştirilir önerilir. Yüzde tabanlı azaltma varsayılan olarak devre dışı olduğundan bu kısıtlamalar bir küme için varsayılan kısıtlamalar devre dışı ve yüzde tabanlı kısıtlamalar ile değiştirilen kadar devam eder. 

  - GlobalMovementThrottleThreshold – Bu ayar, bazı zamanla hareketleri kümedeki toplam sayısını denetler. Süreyi saniye cinsinden GlobalMovementThrottleCountingInterval olarak belirtilir. GlobalMovementThrottleThreshold için varsayılan değer 1000'dir ve 600 GlobalMovementThrottleCountingInterval için varsayılan değerdir.
  - MovementPerPartitionThrottleThreshold – Bu ayar, bazı zaman içinde herhangi bir hizmet bölümü için hareketleri toplam sayısını denetler. Süreyi saniye cinsinden MovementPerPartitionThrottleCountingInterval olarak belirtilir. MovementPerPartitionThrottleThreshold için varsayılan değer 50'dir ve 600 MovementPerPartitionThrottleCountingInterval için varsayılan değerdir.

Bu kısıtlamalar yapılandırmasını yüzde tabanlı azaltma olarak aynı deseni izler.

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- Küme Kaynak Yöneticisi kümesi tanımlamak için birçok seçenek vardır. Bunlar hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
