---
title: Azure Service fabric'te ölçüm birleştirme | Microsoft Docs
description: Genel Bakış birleştirme kullanarak veya Service fabric'te ölçümler için bir strateji olarak paketleme
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 6e041e41372c72c6792c1fb4a1fbdc3bbe475b21
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60844408"
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Service Fabric yük ile ölçümleri ve birleştirme
Yükleme ölçümleri kümedeki yönetmek için Service Fabric Küme Kaynak Yöneticisi ait varsayılan yükünü dağıtmak için stratejisidir. Düğümleri eşit olarak kullanılan sağlama Çekişme ve israfı neden sıcak ve soğuk noktaları ortadan kaldırır. Küme iş yüklerini dağıtma de hata belirli bir iş yükünün büyük bir yüzdesini almaz sağlar beri hataları geri kalan bakımından en güvenli yoldur. 

Service Fabric Küme Kaynak Yöneticisi, birleştirme olan yük yönetmek için farklı bir strateji desteklemiyor. Birleştirme, küme üzerinde bir ölçüm kullanımı dağıtmak çalışmak yerine, bunu birleştirilir, anlamına gelir. Yalnızca bir ters çevirmeyi ölçüm yük ortalama standart sapmasını en aza indirmek yerine stratejisi – Dengeleme varsayılan birleştirme, Küme Kaynak Yöneticisi artırmak dener.

## <a name="when-to-use-defragmentation"></a>Ne zaman birleştirme kullanılır?
Yükünü kümede dağıtma bazı her bir düğümünde kaynaklarını tüketir. Bazı iş yükleri, olağanüstü büyük ve çoğu veya tüm bir düğümü kullanan hizmetler oluşturun. Bu gibi durumlarda, oluşturulan büyük iş yükleri olduğunda olmadığından yeterli alan bunları çalıştırmak için herhangi bir düğümde mümkündür. Büyük iş yükleri, Service fabric'te bir sorun değildir; Bu gibi durumlarda, bu büyük bir iş yükü yer kümeye yeniden düzenlemek için gerekli küme kaynak yöneticisi belirler. Ancak, kümede zamanlanması için beklenecek sırada iş yükü vardır.

Çok sayıda hizmet ve değiştirmemiz durumunda ise kümede yerleştirilecek büyük iş yükü için uzun sürebilir. Bu, kümedeki diğer iş yükleri de büyük ve bu nedenle yeniden düzenlemek için daha uzun sürer daha yüksektir. Service Fabric ekibi, bu senaryonun benzetimleri oluşturma süreleri ölçülür. Küme kullanımı yukarıda aldı % 30 ile % 50 arasında hemen sonra büyük hizmetleri oluşturmak çok daha uzun sürdüğü bulduk. Bu senaryonun işlenmesi için birleştirme karşı bir strateji kullanıma sunduk. Büyük iş yükleri için özellikle olanları oluşturulma zamanı birleştirme gerçekten bu yeni iş yüklerini Yardım önemli olduğu kümedeki zamanlanmış bulduk.

Hizmetlerin yükünü daha az düğümlere sıkıştırmak proaktif olarak denemek için Küme Kaynak Yöneticisi için birleştirme ölçümleri yapılandırabilirsiniz. Bu yardımcı, olan neredeyse her zaman küme yeniden düzenleme olmadan büyük Hizmetleri yer olduğundan emin olur. Küme yeniden düzenlemek zorunda değil, büyük iş yükleri hızlıca oluşturulmasını sağlar.

Çoğu kişi birleştirme gerekmez. Hizmetleri olan genellikle küçük, bu nedenle bunları yer kümede zor değildir. İçeride mümkün olduğunda, çoğu hizmetler küçük ve hızlı bir şekilde ve paralel taşınabilir olduğundan, hızlı bir şekilde yeniden gider. Ancak, büyük Hizmetleri varsa ve bunları kolayca birleştirme stratejisi sonra oluşturulan gerekiyorsa sizin için uygun. Birleştirme sonraki kullanmanın ödün açıklayacağız. 

## <a name="defragmentation-tradeoffs"></a>Birleştirme Seçenekleri
Daha fazla hizmet başarısız düğümleri üzerinde çalışan bu yana birleştirme hataların, impactfulness artırabilirsiniz. Kümedeki kaynaklar ayırma, büyük iş yüklerinin oluşturulması bekleniyor tutulması gereken olduğundan birleştirme maliyetleri de artırabilirsiniz.

Aşağıdaki diyagramda iki küme görsel bir temsilini, bir birleştirilmiştir ve olmayan bir sağlar. 

<center>

![Dengeli ve kümeleri birleştirilmiş karşılaştırma][Image1]
</center>

Dengeli durumda en büyük hizmet nesnelerinden birine yerleştirmek gereken hareketleri sayısını göz önünde bulundurun. Birleştirilmiş bir kümede bulunan büyük iş yükü tüm diğer hizmetler taşımak beklemek zorunda kalmadan düğümleri dört veya beş yerleştirilebilir.

## <a name="defragmentation-pros-and-cons"></a>Birleştirme avantajları ve dezavantajları
Bu nedenle bu diğer kavramsal ödünler nelerdir? Nesnelerin interneti hakkında düşünmeye hızlı bir tablo şu şekildedir:

| Birleştirme uzmanları | Birleştirme simgeler |
| --- | --- |
| Büyük Hizmetleri daha hızlı oluşturulmasını sağlar |Concentrates artan Çekişme daha az düğümü yükleme |
| Etkinleştirir, oluşturma sırasında veri taşıma daha düşük |Hataları daha fazla hizmet etkiler ve daha fazla dalgalanma neden |
| Alan geri kazanma ve gereksinimleri zengin açıklaması sağlar |Daha karmaşık genel kaynak yönetimi yapılandırma |

Birleştirilmiş ve normal ölçümleri aynı kümedeki karıştırabilirsiniz. Küme Kaynak Yöneticisi, mümkün olduğunca diğerleri yayılmasını çalışırken birleştirme ölçümleri birleştirmek çalışır. Birleştirme karıştırma stratejileri Dengeleme sonuçlarını da dahil olmak üzere çeşitli etkenlere bağlıdır:
  - Ölçüm birleştirme ölçümleri sayısı ve Dengeleme sayısı
  - Herhangi bir hizmeti iki tür ölçüm kullanıp kullanmadığını 
  - Ölçüm ağırlıkları
  - Geçerli ölçüm yükler
  
Deneme gerekli tam yapılandırma belirlemek için gereklidir. Birleştirme ölçüm üretimde etkinleştirmeden önce kapsamlı iş yüklerinizi ölçü öneririz. Bu birleştirme ve dengeli ölçümleri aynı hizmetinde kullanırken özellikle doğrudur. 

## <a name="configuring-defragmentation-metrics"></a>Birleştirme ölçümleri yapılandırma
Birleştirme ölçümlerin yapılandırılmasını bir genel kümedeki kararıdır ve tek tek ölçümleri birleştirme için seçilebilir. Aşağıdaki yapılandırma kod parçacıkları, birleştirme için ölçümleri yapılandırma gösterilmektedir. Bu durumda, "Metric1", "Metric2" normalde dengelenmeye devam ederken bir birleştirme ölçüm yapılandırılır. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi man seçenekleri kümesi tanımlamak için vardır. Bunlar hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
- Kullanım ve kapasite kümedeki Service Fabric Küme Kaynak Yöneticisi'ni nasıl yönettiğini ölçümleridir. Ölçümler ve nasıl yapılandırılacakları hakkında daha fazla bilgi edinmek için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
