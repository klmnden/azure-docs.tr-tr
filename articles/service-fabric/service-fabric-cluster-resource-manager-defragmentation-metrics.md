---
title: Azure Service Fabric ölçümlerini birleştirilmesine | Microsoft Docs
description: Birleştirme kullanarak veya Service Fabric ölçümlerini için bir strateji olarak sevk bir genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: a35ae5933729615d634359e64e31d43536d81431
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Ölçümleri ve Service Fabric yük birleştirme
Kümedeki yük ölçümleri yönetmek için Service Fabric kümesi kaynak yöneticisinin varsayılan stratejisi yük dağıtmaktır. Düğümleri eşit olarak kullanıldığını sağlama Çekişme hem harcanan kaynaklar için yol sıcak ve soğuk noktalar önler. Küme iş yüklerini dağıtma de hata belirli bir iş yükü büyük bir yüzdesini almaz sağlar beri hataları sürdüren bakımından güvenlisidir. 

Service Fabric kümesi Kaynak Yöneticisi, birleştirme olan yük yönetmek için farklı bir strateji desteklemiyor. Birleştirme, küme üzerinde bir ölçüm kullanımını dağıtmayı denemeden yerine onu birleştirilir olduğunu anlamına gelir. Birleştirme yalnızca bir ters çevirmeyi Dengeleme ölçüm yük ortalama standart sapmasını en aza yerine stratejisi – varsayılan olarak, bu artırmak Küme Kaynak Yöneticisi'ni çalışır.

## <a name="when-to-use-defragmentation"></a>Birleştirme kullanma zamanı
Yük devretme kümesinde dağıtmak, bazı kaynaklara her düğümde tüketir. Bazı iş yükleri olağanüstü büyük ve çoğu veya hepsi bir düğümün tüketen Hizmetleri oluşturun. Bu durumlarda, oluşturulan büyük iş yüklerini olduğunda olmadığından yeterli alan bunları çalıştırmak için herhangi bir düğümde mümkündür. Büyük iş yüklerini Service Fabric bir sorun değildir; Bu durumlarda, bu büyük iş yükü için yer açmak için kümeyi yeniden düzenlemek için gerekli Küme Kaynak Yöneticisi'ni belirler. Ancak, kümede zamanlanacak beklemek bu sırada iş yükü vardır.

Daha sonra birçok Hizmetleri ve hareket etmek için durum varsa, kümede yerleştirilecek büyük iş yükü uzun bir süredir ele geçirebilir. Bu, kümedeki diğer iş yükleri de büyük varsa ve bu nedenle yeniden düzenlemek için daha uzun sürer daha yüksektir. Service Fabric ekip oluşturma zamanları ile bu senaryonun benzetimleri ölçülür. %30 ve % 50 arasında yukarıdaki Küme kullanımı var hemen büyük hizmetleri oluşturma çok uzun sürdüğü bulduk. Bu senaryo işlemek için birleştirme karşı bir strateji gösterdiğimizi. Büyük iş yükleri için özellikle olanları oluşturulma zamanı birleştirme gerçekten bu yeni iş yüklerine Yardım önemli olduğu kümede zamanlanmış bulduk.

Daha az düğümü Hizmetleri yük sıkıştırmak proaktif olarak denemek için Küme Kaynak Yöneticisi'nin birleştirme ölçümleri yapılandırabilirsiniz. Bu, olduğunu neredeyse her zaman küme yeniden düzenleme olmadan büyük Hizmetleri için yeriniz sağlamaya yardımcı olur. Küme reorganıze zorunluluğunu büyük iş yüklerini hızlı oluşturma sağlar.

Çoğu kişi birleştirme gerek yoktur. Hizmetleri olan genellikle küçük, bunları yer kümede zor değildir. Düzenlenmesi mümkün olduğunda, çünkü çoğu Hizmetleri küçük ve hızlı bir şekilde ve paralel taşınabilir öğeyi hızlı bir şekilde, yeniden başlatılır. Ancak, büyük Hizmetleri varsa ve bunları hızlı bir şekilde birleştirme stratejisi sonra oluşturulan gereksinim sizin için gerekir. Biz birleştirme sonraki kullanmanın dengelemeden ele alacağız. 

## <a name="defragmentation-tradeoffs"></a>Birleştirme bileşim
Daha fazla hizmet başarısız düğümleri üzerinde çalışan bu yana birleştirme hataları, impactfulness artırabilir. Kümedeki kaynaklar için büyük iş yüklerini oluşturulmasını bekleyen ayırma tutulması gereken beri birleştirme maliyetleri de artırabilirsiniz.

Aşağıdaki diyagramda iki küme görsel gösterimi, birleştirilmiş diğeri olmayan sağlar. 

<center>
![Karşılaştırma ve dengeli kümeleri birleştirilmiş][Image1]
</center>

Dengeli durumda en büyük hizmet nesnelerden biri yerleştirileceği gerekli olacak hareketleri sayısını göz önünde bulundurun. Birleştirilmiş kümede büyük iş yükü herhangi diğer hizmetler taşımak beklemek zorunda kalmadan dört veya beş düğümlerinde yerleştirilemedi.

## <a name="defragmentation-pros-and-cons"></a>Birleştirme Artıları ve eksileri
Bu nedenle bu diğer kavramsal bileşim nelerdir? Hızlı bir tablo dikkat etmeniz gereken noktalar aşağıdadır:

| Birleştirme uzmanları | Birleştirme simgeler |
| --- | --- |
| Büyük Hizmetleri daha hızlı oluşturulmasını sağlar |Concentrates Çekişme artırma daha az düğümü yükleme |
| Etkinleştirir veri taşıma oluşturma sırasında düşürün. |Hataları daha fazla hizmet etkileyebilir ve daha fazla karmaşası neden |
| Alan geri kazanma ve gereksinimleri zengin açıklaması sağlar |Daha karmaşık genel kaynak yönetimi yapılandırma |

Aynı kümedeki birleştirilmiş ve normal ölçümleri karıştırabilirsiniz. Mümkün olduğunca başkalarının yayılmak çalışırken birleştirme ölçümleri birleştirmek Küme Kaynak Yöneticisi'ni çalışır. Birleştirme karıştırma ve stratejileri Dengeleme sonuçlarını dahil olmak üzere çeşitli etkenlere bağlıdır:
  - Birleştirme ölçümleri sayısı ve ölçümleri Dengeleme sayısı
  - Herhangi bir hizmet ölçümleri her iki tür kullanıp kullanmadığını 
  - Ölçüm ağırlığı
  - Geçerli ölçüm yükler
  
Deneme gerekli tam yapılandırmasını belirlemek için gereklidir. Birleştirme ölçümleri üretimde etkinleştirmeden önce iş yüklerinin kapsamlı ölçüm öneririz. Bu birleştirme ve aynı hizmet içinde dengeli ölçümleri karıştırma durumlarda özellikle geçerlidir. 

## <a name="configuring-defragmentation-metrics"></a>Birleştirme ölçümleri yapılandırma
Birleştirme ölçümleri yapılandırma bir genel kümedeki bir karardır ve tek tek ölçümleri birleştirme için seçilebilir. Aşağıdaki yapılandırma parçacıkları nasıl birleştirme için ölçümleri yapılandırılacağını gösterir. Bu durumda, "Metric2" normalde dengelenmeye devam ederken "Metric1" bir birleştirme ölçü olarak yapılandırılır. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

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
- Küme Kaynak Yöneticisi'ni küme açıklamak için adam seçenekleri vardır. Bunları hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
- Service Fabric kümesi Kaynak Yöneticisi, kullanım ve kapasite kümedeki nasıl yönettiğini ölçümleridir. Ölçümleri ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
