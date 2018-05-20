---
title: Azure Service Fabric kümesi Bakiye | Microsoft Docs
description: Service Fabric kümesi Resource Manager ile kümenizi Dengeleme giriş bilgileri.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5d2f195c50750a5c7685f62c909f77b2960613e6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="balancing-your-service-fabric-cluster"></a>Service fabric kümesi Dengeleme
Service Fabric kümesi kaynak yöneticisi ekleme veya kaldırma işlemleri düğümleri veya hizmetlerin tepki dinamik yük değişiklikleri destekler. Bir kısıtlama ihlali de otomatik olarak düzeltir ve proaktif olarak küme yeniden dengeler. Ancak bu eylemler ne sıklıkta alınır ve bunları tetikleyen?

Küme Kaynak Yöneticisi'ni gerçekleştiren iş üç farklı kategorisi vardır. Bunlar:

1. Yerleştirme – bu aşamada herhangi bir durum bilgisi olan çoğaltmaları veya eksik olan durum bilgisiz örnekleri yerleştirme ile ilgilidir. Yerleştirme, hem yeni hizmetleri hem de işleme durum bilgisi olan çoğaltmaları veya başarısız olan durum bilgisiz örnekleri içerir. Silme ve çoğaltmalar veya örnekleri bırakma burada ele alınır.
2. Kısıtlama denetler – Bu aşama olup olmadığını denetler ve farklı kısıtlamalarından (kurallar) sistemi içinde ihlalleri düzeltir. Düğümler kapasite değildir ve bir hizmetin kısıtlamalarından karşılandığından emin olduktan gibi şeyler kuralları örnekleridir.
3. – Bu aşama denetler dengelenmesi farklı ölçümleri bakiyesini yapılandırılmış istenilen düzeyine göre gerekli olup olmadığını görmek için Dengeleme. Bu durumda daha dengeli kümede düzenleme bulmaya çalışır.

## <a name="configuring-cluster-resource-manager-timers"></a>Küme Kaynak Yöneticisi zamanlayıcıları yapılandırma
İlk Dengeleme geçici denetimleri kümesini zamanlayıcılar kümesidir. Bu zamanlayıcılar ne sıklıkta Küme Kaynak Yöneticisi'ni küme inceler ve düzeltici eylemleri gerçekleştirir yönetir.

Küme Kaynak Yöneticisi yapabilir düzeltmeleri farklı bu türlerinin her biri kendi sıklığı yöneten farklı bir Zamanlayıcı tarafından denetlenir. Her Zamanlayıcı başlatıldığında görevi zamanlandı. Resource Manager varsayılan olarak:

* durumu tarar ve güncelleştirmeleri (örneğin, bir düğümü çalışmıyor kayıt) uygular her 1/saniyede 10
* yerleştirme denetim bayrağını ayarlar 
* saniyede kısıtlaması onay bayrağını ayarlar
* beş saniyede karşı bayrağını ayarlar.

Aşağıda bu zamanlayıcılar yöneten yapılandırma örnekleri şunlardır:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Bugün Küme Kaynak Yöneticisi'ni yalnızca bu eylemlerden biri, bir kerede sırayla gerçekleştirir. Biz "minimum aralıkları" ve zamanlayıcılar "ayarı bayrakları" devre dışı olduğunda gerçekleştirilecek eylemleri olarak bu zamanlayıcılar başvurmak nedeni budur. Örneğin, Küme Kaynak Yöneticisi'ni Dengeleme kümesi önce hizmetleri oluşturmak üzere bekleyen istekleri ilgilenir. Belirtilen varsayılan zaman aralıklarına göre gördüğünüz gibi Küme Kaynak Yöneticisi'ni sık yapması gereken her şey için tarar. Normalde bu her adımı sırasında yapılan değişiklikler kümesi küçük olduğunu gösterir. Sık olarak küçük değişiklikler Küme Kaynağı Yöneticisi'nin şeyler kümede gerçekleştiğinde esnek olmasını sağlar. Varsayılan zamanlayıcılar olayları aynı türde çoğunu aynı anda ortaya eğilimindedir bu yana bazı yığınlama sağlar. 

Örneğin, başarısız olduğunda düğümleri aynı anda kadar tüm hata etki alanlarını yapabilirsiniz. Tüm bu hatalar sonraki durum sırasında yakalanan güncelleştirme sonra *PLBRefreshGap*. Düzeltmeleri, kısıtlama denetimi gibi aşağıdaki yerleştirme sırasında belirlenir ve Dengeleme çalıştırır. Varsayılan olarak Küme Kaynak Yöneticisi'ni değil kümesindeki değişiklikleri saatlik aracılığıyla tarama ve adresi aynı anda tüm değişiklikleri çalışılıyor. Bunun yapılması karmaşası WINS'e için yol açar.

Küme Kaynak Yöneticisi'ni de belirlemek için bazı ek bilgiler gerekir imbalanced küme. İçin sahip olduğumuz iki parça yapılandırma: *BalancingThresholds* ve *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Dengeleme eşikleri
Dengeleme eşik dengelenmesi tetiklemek ana denetimdir. Ölçüm için Dengeleme eşik bir _oranı_. Bu ölçüm ait bir ölçüm yük miktarı az yüklenen düğümde bölü en yüklenen düğümde yük aşarsa, *BalancingThreshold*, küme imbalanced olur. Sonuç olarak Dengeleme Küme Kaynak Yöneticisi'ni denetlediğinde tetiklenir. *MinLoadBalancingInterval* Zamanlayıcı tanımlar dengelenmesi gerekliyse, Küme Kaynak Yöneticisi ' ne sıklıkta denetlemeniz gerekir. Denetimi, herhangi bir şey olur anlamına gelmez. 

Dengeleme eşikleri küme tanımının bir parçası olarak ölçüm başına temelinde tanımlanır. Ölçümler hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>
![Eşik örnek Dengeleme][Image1]
</center>

Bu örnekte, her hizmetin bazı ölçüsünün bir birimi kullanıyor. Üst örnekte, bir düğümde en fazla yük beştir ve iki en düşük gereksinimdir. Bu ölçüm karşı eşiği üç olduğunu düşünelim. Kümedeki oranı 5/2 olduğundan 2.5 ve diğer bir deyişle belirtilenden daha az = üç eşiğinin Dengeleme, küme dengelenir. Küme Kaynak Yöneticisi'ni denetlediğinde dengelemesiz tetiklenir.

En az iki oranı beş içinde kaynaklanan olsa da alt örnekte bir düğümde en fazla yük 10 ' dur. Beş üç bu ölçümü için belirlenen karşı eşiği değerinden daha büyük. Sonuç olarak, bir çalıştırma dengelenmesi karşı Zamanlayıcı harekete zamanlanan sonraki sefer olacaktır. Böyle bir durumda bazı yük Düğüm3 için genellikle dağıtılır. Service Fabric kümesi Kaynak Yöneticisi doyumsuz bir yaklaşım kullanmadığı için bazı yükleme Düğüm2 için de dağıtılmış. 

<center>
![Eşik örnek Eylemler Dengeleme][Image2]
</center>

> [!NOTE]
> "Dengeleme" Yük kümenizdeki yönetmek için iki farklı stratejileri işler. Küme Kaynak Yöneticisi'ni kullanan varsayılan strateji, kümedeki düğümler arasında yük dağıtmaktır. Diğer strateji [birleştirme](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Birleştirme aynı Dengeleme çalışması sırasında gerçekleştirilir. Yük Dengeleme ve birleştirme stratejilerini aynı kümedeki farklı ölçümleri için kullanılabilir. Bir hizmetin Dengeleme ve birleştirme ölçümleri olabilir. Birleştirme ölçümleri için kümedeki yükleri oranını tetikler olduğunda dengelenmesi _aşağıda_ karşı eşiği. 
>

Dengeleme eşiğin altına alma, açık bir hedef değil. Dengeleme eşikleri olan yalnızca bir *tetikleyici*. Çalıştırır Dengeleme olduğunda Küme Kaynak Yöneticisi'ni varsa bunu yapabilirsiniz, hangi iyileştirmeleri belirler. Karşı arama yalnızca koparılan devre dışı olduğundan, hiçbir şey taşır anlamına gelmez. Bazen küme imbalanced ancak düzeltmek için çok kısıtlanmış olabilir. Alternatif olarak, çok hareketleri geliştirmeleri gerektiren [pahalı](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Etkinlik eşikleri
Bazen, düğümleri görece imbalanced olmasına rağmen *toplam* yük devretme kümesinde miktarı düşük. Yük eksikliği geçici DIP olabilir ya da küme yeni yalnızca alma olduğundan ve önyükleme içinde. Her iki durumda da olduğundan biraz kazanılacak Dengeleme kümesi süre beklemesini istemeyebilirsiniz. Küme Dengeleme yapılan, ağ harcamanız ve işlem kaynaklarını daha büyük yapmadan şeyler hareket etmek için *mutlak* fark. Önlemek için gereksiz taşır, etkinlik eşikleri bilinen başka bir denetim yoktur. Etkinlik eşikleri bazı mutlak alt bağımlı etkinliğinin belirtmenize olanak sağlar. Hiçbir düğümü bu eşiğin üstünde ise, Dengeleme eşiğine olsa bile Dengeleme tetiklenen değil.

Biz bizim Dengeleme eşiğinin Bu ölçüm için üç korumak varsayalım. Ayrıca 1536 etkinlik eşiğinin sahibiz varsayalım. Küme Dengeleme var. eşik imbalanced olsa ilk durumda, bu etkinlik eşik hiçbir düğümü hiçbir şey olmaz şekilde karşılar. Alt örnekte Düğüm1 etkinliği eşiğin üstünde ' dir. Dengeleme eşik ve ölçüm etkinlik eşiği aştığından, Dengeleme zamanlanır. Örnek olarak, aşağıdaki diyagramda bakalım: 

<center>
![Etkinlik eşik örneği][Image3]
</center>

Yalnızca Dengeleme eşikleri gibi etkinlik eşikleri tanımlı başına Metrik Küme tanımı aracılığıyla şunlardır:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Yük Dengeleme ve etkinlik hem Dengeleme belirli bir ölçüm için - bağlı tetiklenir yalnızca Dengeleme eşik ve etkinlik eşiği aşıldı varsa aynı ölçümü eşiklere.

> [!NOTE]
> Belirtilmediğinde, bir ölçüm Dengeleme eşiği 1. ve etkinlik Eşik 0'dır. Bu, Küme Kaynak Yöneticisi'ni mükemmel için belirli bir yük dengeli Bu ölçüm tutmak deneyecek anlamına gelir. Özel ölçümleri kullanıyorsanız, kendi Yük Dengeleme ve etkinlik eşikleri açıkça ölçümlerinizi için tanımlamak önerilir. 
>

## <a name="balancing-services-together"></a>Hizmetleri birlikte Dengeleme
Küme imbalanced olup olmamasına küme çapında bir karardır. Ancak, düzeltme hakkında Git şekilde bireysel hizmet çoğaltmaları ve geçici örnekleri taşımaktır. Bu doğru mantıklıdır? Bellek bir düğümde yığılmış, birden çok çoğaltmalar veya örnekleri için katkıda bulunan. Dengesizliği düzelttikten herhangi bir durum bilgisi olan çoğaltmaları veya imbalanced ölçümün kullanmak durum bilgisiz örneklerinin hareketli gerektirebilir.

Bazen imbalanced kendisini değildi bir hizmet taşınmış ancak (yerel tartışma unutmayın ve daha önce genel ağırlık verir). Tüm bu hizmetin ölçümleri dengeli neden bir hizmet taşındığında? Bir örnek görelim:

- Dört Hizmetleri, Service1, Service2, Service3 ve Service4 varsayalım. 
- Service1 ölçümleri Metric1 ve Metric2 bildirir. 
- Service2 ölçümleri Metric2 ve Metric3 bildirir. 
- Service3 ölçümleri Metric3 ve Metric4 bildirir.
- Service4 ölçüm Metric99 bildirir. 

Burada burada yapacağız kötülerinden görebilirsiniz: bir zinciri! Biz gerçekten dört bağımsız Hizmetleri yoksa, ilişkili üç hizmeti ve kendi başına kapalıdır bir sunuyoruz.

<center>
![Hizmetleri birlikte Dengeleme][Image4]
</center>

Bu zincir nedeniyle bir dengesizliği ölçümleri 1-4, çoğaltmaları veya hizmetleri hareket etmek için 1-3 ait örnekleri neden olabilir. mümkündür. Ölçümleri 1, 2 veya 3 içinde bir dengesizliği içinde Service4 hareketleri sebep olamaz biliyoruz. Çoğaltmaları taşıyarak bu yana hiçbir noktası olacaktır veya Service4 ait örnekleri ölçümleri 1-3 bakiyesini etkilemeye kesinlikle hiçbir şey yapabilirsiniz.

Küme Kaynak Yöneticisi'ni otomatik olarak hangi hizmetlerin ilgili rakamlar. Ekleme, kaldırma veya hizmetleri için ölçümler değiştirme ilişkilerini etkileyebilir. Örneğin, Service2 Dengeleme iki çalışmaları arasında Metric2 kaldırmak için güncelleştirilmiş olabilir. Service1 Service2 arasındaki zinciri keser. Şimdi yerine iki grup ilgili hizmetlerin vardır üç:

<center>
![Hizmetleri birlikte Dengeleme][Image5]
</center>

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric kümesi Kaynak Yöneticisi, kullanım ve kapasite kümedeki nasıl yönettiğini ölçümleridir. Ölçümleri ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
* Taşıma maliyeti için Küme Kaynak Yöneticisi'ni, belirli hizmetleri diğerlerinden taşımak daha pahalı sinyal bir yoludur. Taşıma maliyeti hakkında daha fazla bilgi için başvurmak [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md)
* Küme Kaynak Yöneticisi'ni kümedeki karmaşası aşağı yavaş yapılandırdığınız birkaç kısıtlamaları vardır. Bunlar normalde gerekli değildir, ancak bunlara ihtiyacınız varsa bunları hakkında bilgi edinebilirsiniz [burada](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
