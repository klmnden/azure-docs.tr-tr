---
title: Service Fabric kümesi Kaynak Yöneticisi'nde azaltma | Microsoft Docs
description: Service Fabric kümesi kaynak yöneticisi tarafından sağlanan kısıtlamaları yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e9db1070066a2a02b72b5cc051e59d8b04dc9928
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205136"
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Service Fabric kümesi Kaynak Yöneticisi azaltma
Küme Kaynak Yöneticisi'ni düzgün yapılandırdıktan olsa bile, küme kesintiye. Örneğin, yükseltme sırasında oluştu neler olacağını eşzamanlı düğüm ve hata etki alanı hataları - olabilir? Küme Kaynak Yöneticisi'ni daima her şeyi düzeltmek yeniden düzenleyin ve küme düzeltmek çalışarak küme kaynaklarını tüketen çalışır. Kısıtlamaları Yardım küme kaynakları Sabitle için - kullanabilmesi için bir backstop sağlamak düğümleri geri dönün, ağ bölümleri onarma, düzeltilmiş BITS dağıtılmış.

Durumlarda bu tür yardımcı olmak için Service Fabric kümesi Kaynak Yöneticisi birkaç kısıtlamaları içerir. Bu kısıtlamaları tüm oldukça büyük hammers ' dir. Genellikle, dikkatli planlama ve sınama yapılmadan değiştirilmesi gerekir.

Küme kaynak yöneticisinin kısıtlamaları değiştirirseniz, beklenen gerçek testiniz ayarlamak. Kümenin bazı durumlarda Sabitle daha uzun sürer anlamına gelir olsa bile, bazı kısıtlamaları yerinde gerek belirleyebilir. Sınama kısıtlamaları için doğru değerleri belirlemek için gereklidir. Kısıtlamaları makul bir sürede değişikliklerine yanıt vermelerini izin vermek için yeterince yüksek ve gerçekte çok fazla kaynak tüketimini önlemek için yeterince düşük olması gerekir. 

Çoğu müşteri gördük zaman zaten bir kaynak kısıtlanmış ortamında olduğundan bırakıldı kısıtlamaları kullanın. Bazı örnekler tek düğümler veya paralel işleme sınırlamaları nedeniyle birçok durum bilgisi olan çoğaltmaları oluşturmak mümkün olmayan diskleri için sınırlı ağ bant genişliği olacaktır. Kısıtlamaları, işlemleri başarısız veya yavaş işlemleri neden bu kaynakları doldurmaya. Müşteriler kısıtlamaları kullanılan ve süreyi genişletme biliyorduk bu durumlarda kararlı bir duruma kümeye kalırsınız. Müşterileri de bunlar kısıtlanan sırada düşük genel güvenilirlik çalışan sonlandırmak anladım.


## <a name="configuring-the-throttles"></a>Kısıtlamaları yapılandırma

Service Fabric çoğaltma hareketlerinin sayısı kısıtlama için iki mekanizma vardır. Service Fabric 5.7 önce var olan varsayılan bir mekanizma, bir izin verilen taşır mutlak sayı olarak azaltma temsil eder. Bu, tüm boyutları kümeleri için çalışmaz. Özellikle büyük kümeler için varsayılan değer, daha küçük kümelerde hiçbir etkisi yaparken gerekli olduğunda bile Dengeleme aşağı önemli ölçüde yavaşlamasının çok küçük olabilir. Bu önceki mekanizması yüzde tabanlı denetimi tarafından Hizmetleri ve düğüm sayısı düzenli olarak değiştiği dinamik kümeleriyle iyi ölçeklenir yerini yenisi aldı.

Kısıtlamaları kümelerindeki çoğaltmaları sayısının yüzdesi dayanır. Temel Percetage kısıtlamaları etkinleştirmek kural ifade: "hareket çoğaltmaları % 10'dan fazla 10 dakikalık bir zaman aralığı içinde", örneğin.

Yüzde tabanlı azaltma için yapılandırma ayarları şunlardır:

  - GlobalMovementThrottleThresholdPercentage - kümedeki herhangi bir zamanda izin hareketlerinin sayısı üst sınırı çoğaltmaları kümedeki toplam sayısı yüzdesi olarak ifade edilen. 0 sınır gösterir. Varsayılan değer 0'dır. Bu ayar ve GlobalMovementThrottleThreshold belirtilirse, daha pasif sınırı kullanılır.
  - GlobalMovementThrottleThresholdPercentageForPlacement - çoğaltmaları kümedeki toplam sayısı yüzdesi olarak ifade edilen yerleştirme aşamasında izin hareketlerinin sayısı üst sınırı. 0 sınır gösterir. Varsayılan değer 0'dır. Bu ayar ve GlobalMovementThrottleThresholdForPlacement belirtilirse, daha pasif sınırı kullanılır.
  - GlobalMovementThrottleThresholdPercentageForBalancing - çoğaltmaları kümedeki toplam sayısı yüzdesi olarak ifade edilen karşı aşamasında izin hareketlerinin sayısı üst sınırı. 0 sınır gösterir. Varsayılan değer 0'dır. Bu ayar ve GlobalMovementThrottleThresholdForBalancing belirtilirse, daha pasif sınırı kullanılır.

Azaltma yüzdesi belirtirken, %5 0,05 belirtmeniz gerekir. Bu kısıtlamaları yönetilen aralığı saniye cinsinden belirtilen GlobalMovementThrottleCountingInterval belirtir.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

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

### <a name="default-count-based-throttles"></a>Varsayılan dayalı sayısı kısıtlamaları
Eski kümede vardır veya bu yapılandırmalar beri yükseltildi kümelerinde hala korur durumunda bu bilgiler sağlanır. Genel olarak, bunlar yüzde tabanlı kısıtlamaları ile yukarıdaki değiştirilir önerilir. Yüzde tabanlı azaltma varsayılan olarak devre dışı bırakıldığından bu kısıtlamaları devre dışı ve yüzde tabanlı kısıtlamaları ile değiştirilen kadar bir küme için varsayılan kısıtlamaları kalır. 

  - GlobalMovementThrottleThreshold – Bu ayar, bazı zamanla hareketleri kümedeki toplam sayısını denetler. Süreyi saniye cinsinden GlobalMovementThrottleCountingInterval olarak belirtilir. GlobalMovementThrottleThreshold için varsayılan değer 1000'dir ve varsayılan değer GlobalMovementThrottleCountingInterval için 600'dür.
  - MovementPerPartitionThrottleThreshold – Bu ayar, bazı zamanla hareketleri herhangi bir hizmet bölümü için toplam sayısını denetler. Süreyi saniye cinsinden MovementPerPartitionThrottleCountingInterval olarak belirtilir. MovementPerPartitionThrottleThreshold için varsayılan değer 50'dir ve varsayılan değer MovementPerPartitionThrottleCountingInterval için 600'dür.

Bu kısıtlamaları yapılandırması azaltma yüzde tabanlı olarak aynı düzeni izler.

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- Küme Kaynak Yöneticisi'ni küme açıklamak için birçok seçeneğiniz vardır. Bunları hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
