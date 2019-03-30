---
title: Azure Service Fabric kümenizi Dengeleme | Microsoft Docs
description: İle Service Fabric Küme Kaynak Yöneticisi kümenizi Dengeleme giriş.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 74fe4f7c4c231f80c7555f39f840a85baae310e9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662032"
---
# <a name="balancing-your-service-fabric-cluster"></a>Service fabric kümenizi Dengeleme
Service Fabric küme kaynak yöneticisi ekleme veya çıkarma düğümleri veya hizmetler için tepki dinamik yük değişiklikleri destekler. Sabiti ihlallerini de otomatik olarak düzeltir ve proaktif olarak kümeye yeniden dengeler. Ancak bu eylemler ne sıklıkta alınır ve bunları tetikler?

Küme Kaynak Yöneticisi gerçekleştiren iş üç farklı kategorisi vardır. Bunlar:

1. Yerleştirme: Bu aşamada herhangi bir durum bilgisi olan yinelemeler veya eksik olan durum bilgisi olmayan örnekleri yerleştirme ile ilgilidir. Yerleştirme, yeni hizmetler ve işleme durum bilgisi olan yinelemeler veya başarısız olan durum bilgisi olmayan örnekleri içerir. Silme ve çoğaltmalar veya örnekleri bırakarak burada ele alınır.
2. Kısıtlama kontrol – Bu aşama denetler ve sistemi içinde farklı yerleştirme kısıtlamaları (kuralları) ihlalleri düzeltir. Düğümleri kapasite aşımı olmayan ve bir hizmetin yerleştirme kısıtlamaları karşılanmasını sağlamak gibi şeyler kuralları örnekleridir.
3. -Bu aşama denetler yeniden Dengeleme Bakiye farklı ölçümler için yapılandırılmış istenen düzeyine göre gerekli olup olmadığını görmek için Dengeleme. Bu durumda daha dengeli kümede düzenleme bulmayı dener.

## <a name="configuring-cluster-resource-manager-timers"></a>Küme Kaynak Yöneticisi zamanlayıcılar yapılandırma
İlk Dengeleme geçici denetimleri kümesini zamanlayıcılar kümesidir. Bu zamanlayıcılar ne sıklıkta küme kaynak yöneticisi küme inceler ve düzeltici eylemleri gerçekleştirir yönetir.

Küme Kaynak Yöneticisi yapabilirsiniz düzeltmeleri farklı bu türlerinin her biri kendi sıklığı yöneten farklı bir Zamanlayıcı tarafından denetlenir. Her bir zamanlayıcı başlatıldığında, görevi zamanlandı. Varsayılan olarak Kaynak Yöneticisi:

* durumuna tarar ve güncelleştirmeleri (örneğin, bir düğüm kaydı) her 1/10 saniyenin
* yerleştirme denetim bayrağını ayarlar 
* saniyede kısıtlama denetimi bayrağını ayarlar
* her beş saniyede karşı bayrağını ayarlar.

Bu zamanlayıcılar yöneten yapılandırma örnekleri aşağıda verilmiştir:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

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

Bugün küme kaynak yöneticisi yalnızca bu eylemlerden biri, bir kerede sırayla gerçekleştirir. "En düşük aralık" ve "ayarı bayrak olarak" zamanlayıcılar alarmın olduğunda alınan eylemleri olarak bu zamanlayıcılar diyoruz nedeni budur. Örneğin, Küme Kaynak Yöneticisi, küme Dengeleme önce hizmetleri oluşturmak üzere bekleyen istekleri üstlenir. Belirtilen varsayılan zaman aralıkları görebileceğiniz gibi küme kaynak yöneticisi sık yapmanız gereken her şey için tarar. Normalde bu her adımı sırasında yapılan değişiklikler kümesini küçük olduğunu gösterir. Sık olarak küçük değişiklik kümesinde böyle bir durum olduğunda, hızlı yanıt verdiğini Küme Kaynak Yöneticisi sağlar. Varsayılan zamanlayıcılar birçok olayların aynı tür gerçekleştiklerini eğilimli olduğundan bazı yığınlama sağlar. 

Örneğin, başarısız olduğunda düğümleri kadar tüm hata etki alanları aynı anda yapabilirsiniz. Bu hatalar sonraki durum sırasında yakalanan tüm güncelleştirme sonrasında *PLBRefreshGap*. Düzeltmeleri, kısıtlama denetimi gibi aşağıdaki yerleştirme sırasında belirlenir ve Dengeleme çalıştırır. Varsayılan olarak küme kaynak yöneticisi değil kümesindeki değişiklikleri saatlik aracılığıyla tarama ve tek seferde tüm değişiklikleri adres çalışılıyor. Bunun yapılması, değişim artışları için yol açar.

Küme Kaynak Yöneticisi belirlemek için bazı ek bilgiler de gerekli küme imbalanced. Bunun için iki yapılandırma parçalarını sunuyoruz: *BalancingThresholds* ve *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Dengeleme eşikleri
Bir dengeleme eşiği yeniden Dengeleme tetiklemek ana denetim. Bir ölçüm için Dengeleme eşiğin bir _oranı_. Yükü az yüklenen düğümü yük miktarı bölü en yüklenen düğümde bir ölçüm için ölçüm'ın aşarsa *BalancingThreshold*, küme imbalanced kaldırılır. Sonuç olarak Dengeleme küme kaynak yöneticisi denetlediğinde tetiklenir. *MinLoadBalancingInterval* Zamanlayıcıyı yeniden Dengeleme gerekiyorsa, Küme Kaynak Yöneticisi ne sıklıkta denetleyeceğini tanımlar. Denetleme, hiçbir şey olmuyor gelmez. 

Dengeleme eşikleri kümesi tanımının bir parçası olarak ölçüm başına temelinde tanımlanır. Ölçümler hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

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

Bu örnekte, her hizmetin bir bazı ölçüm birimi tüketiyor. İlk örnekte, bir düğümde en fazla yükü beş ve en az iki. Bu ölçüm karşı eşiği üç olduğunu varsayalım. Kümedeki oranı 5/2 olduğundan, 2.5 ve diğer bir deyişle belirtilen değerden = üç eşiğinin Dengeleme, küme dengelenir. Küme Kaynak Yöneticisi denetlerken hiçbir Dengeleme tetiklenir.

En az iki beş bir oranı kaynaklanan olmakla birlikte alt örnek bir düğümde en fazla yükü 10 ' dur. Beş üç Bu ölçüm için belirlenen karşı eşik değerinden büyüktür. Sonuç olarak, bir çalıştırma yeniden Dengeleme karşı Zamanlayıcı harekete zamanlanmış bir sonraki açışınızda olacaktır. Böyle bir durumda Düğüm3 için genellikle bazı yük dağıtılır. Bazı yük, aynı zamanda Service Fabric Küme Kaynak Yöneticisi doyumsuz bir yaklaşım kullanmadığı Düğüm2 için Dağıtılmış. 

<center>

![Eşik örnek eylemleri Dengeleme][Image2]
</center>

> [!NOTE]
> "Dengeleme" Yük kümenizdeki yönetmek için iki farklı stratejiler işler. Küme Kaynak Yöneticisi kullanan varsayılan küme içindeki düğümler arasında yük dağıtmak için stratejisidir. Diğer strateji [birleştirme](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Birleştirme aynı Dengeleme çalıştırma sırasında gerçekleştirilir. Dengeleme ve birleştirme stratejilerini aynı kümedeki farklı ölçümler için kullanılabilir. Bir hizmetin Dengeleme ve birleştirme ölçümleri olabilir. Birleştirme ölçümler için kümedeki yükleri oranını tetikler olduğunda yeniden Dengeleme _aşağıda_ karşı eşiği. 
>

Dengeleme eşiğin altına alma, açık bir hedef değil. Dengeleme eşikleri olan yalnızca bir *tetikleyici*. Çalıştırmaları Dengeleme, Küme Kaynak Yöneticisi bunu yapabilir, geliştirmeler varsa belirler. Bir dengeleme arama yalnızca başlatılır herhangi bir şey taşır anlamına gelmez. Bazen imbalanced ancak düzeltmek için çok kısıtlı bir kümedir. Alternatif olarak, çok hareketleri geliştirmeleri gerektiren [maliyetli](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Etkinlik eşikleri
Bazen, düğümlerinin görece imbalanced olmasına rağmen *toplam* kümesindeki yük miktarı düşük olur. Yük eksikliği geçici DIP olabilir ya da yeni veya deneyimsiz küme olduğu için önyüklemesi yapılır. Her iki durumda da olmadığı için biraz kazanılacak küme Dengeleme vakit geçirmeyi istemeyebilir. Küme Dengeleme yapılan, ağ harcama ve işlem kaynaklarını daha büyük yapmadan şeylerin yerini değiştirmemiz *mutlak* farkı. Önlemek için gereksiz taşır, etkinlik eşikleri bilinen başka bir denetim yoktur. Etkinlik eşikleri, etkinlik için mutlak bazı alt sınır belirtmenize olanak sağlar. Bu eşiğin üstünde düğümü yok ise Dengeleme eşiği karşılanıyorsa bile Dengeleme tetiklenmez.

Biz bu ölçümün üç bizim Dengeleme eşiği korumak varsayalım. Ayrıca bir etkinlik eşiğini 1536 sahibiz varsayalım. Küme Dengeleme yoktur eşiği imbalanced olsa bu durumda, bu etkinlik eşiği hiçbir düğüm hiçbir şey olmuyor şekilde karşılar. Alt örnekte Node1 etkinliği eşiğin üstünde ' dir. Dengeleme eşiği hem ölçüm etkinlik eşiği aştığından, Dengeleme zamanlanır. Örneğin, aşağıdaki diyagramda bakalım: 

<center>

![Etkinlik eşiği örneği][Image3]
</center>

Yalnızca Dengeleme eşikleri gibi etkinlik eşikleri başına-ölçüm Küme tanımı aracılığıyla şunlardır:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

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

Yük Dengeleme ve etkinlik eşikleri bağlı Dengeleme belirli bir ölçüm için - her ikisi de tetiklenir yalnızca Dengeleme eşiği ve etkinlik eşiği aşıldı, aynı ölçümü için ' dir.

> [!NOTE]
> Belirtilmediğinde, bir ölçüm Dengeleme eşiği 1, etkinlik Eşiği 0'dır. Bu, Küme Kaynak Yöneticisi, belirtilen her türlü yük için mükemmel bir şekilde dengeli bu ölçümü tutmak deneyecek anlamına gelir. Özel ölçümler kullanıyorsanız kendi Dengeleme ve etkinlik eşikleri açıkça ölçümlerinizi için tanımlamanız önerilir. 
>

## <a name="balancing-services-together"></a>Dengeleme hizmetlerini birlikte
Küme imbalanced olup olmamasına küme çapında bir karardır. Ancak, düzeltme hakkında Git şekilde bireysel hizmet çoğaltmalar ve örnekler etrafında taşınıyor. Bu, doğru mantıklı? Bir düğümde yığın bellek, birden çok çoğaltma veya örnek kılavuza katkıda bulunma. Dengesizliği düzeltme herhangi bir durum bilgisi olan yinelemeler veya imbalanced ölçümünü kullanın, durum bilgisi olmayan örnekleri taşıma gerektirebilir.

Bazen kendisi imbalanced olmadığı hizmet taşınmış ancak (yerel tartışma unutmayın ve daha önce genel ağırlık verme). Neden tüm bu hizmetin ölçümleri olanağı sunan, hizmet taşındı? Bir örneğe bakalım:

- Dört Hizmetleri, Service1, Service2, Service3 ve Service4 varsayalım. 
- Service1 ölçümleri Metric1 ve Metric2 bildirir. 
- Service2 ölçümleri Metric2 ve Metric3 bildirir. 
- Service3 ölçümleri Metric3 ve Metric4 bildirir.
- Ölçüm Metric99 Service4 bildirir. 

Elbette, burada buraya ekleyeceğiz görebilirsiniz: Zinciri yoktur! Biz dört bağımsız Hizmetleri gerçekten yoksa, üç hizmetlerinin ilgili ve kendi kendine kapalı bir sahibiz.

<center>

![Dengeleme hizmetlerini birlikte][Image4]
</center>

Bu zincir nedeniyle dengesizlik ölçümler 1-4 içinde çoğaltmalar veya hareket etmek için 1-3 hizmetlerine ait örnekler neden olabileceğini mümkündür. Dengesizlik ölçümler 1, 2 veya 3 olarak hareketleri içinde Service4 neden olamaz biliyoruz. Çoğaltmaları taşıyarak bu yana hiçbir noktası olacak veya örnekleri Service4 ait ölçümler 1-3 bakiyesini etkilemek için kesinlikle hiçbir şey yapabilirsiniz.

Küme Kaynak Yöneticisi, hangi hizmetlerin ilgili otomatik olarak belirler. Ekleme, kaldırma veya hizmetleri için ölçümler değiştirme ilişkilerini etkileyebilir. Örneğin, Service2 Dengeleme iki çalıştırma arasında Metric2 kaldırmak için güncelleştirilmiş olabilir. Bu zincir Service1 Service2 arasındaki keser. Şimdi iki ilgili hizmetler grupları yerine vardır üç:

<center>

![Dengeleme hizmetlerini birlikte][Image5]
</center>

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım ve kapasite kümedeki Service Fabric Küme Kaynak Yöneticisi'ni nasıl yönettiğini ölçümleridir. Ölçümler ve nasıl yapılandırılacakları hakkında daha fazla bilgi edinmek için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
* Taşıma maliyeti belirli hizmetleri taşıma diğerlerine göre daha pahalı küme kaynak yöneticisi için sinyal bir yoludur. Taşıma maliyeti hakkında daha fazla bilgi için bkz [bu makalede](service-fabric-cluster-resource-manager-movement-cost.md)
* Küme Kaynak Yöneticisi, değişim sıklığı kümedeki yavaşlatmaz yapılandırabileceğiniz çeşitli kısıtlamalar vardır. Bunlar genellikle gerekli değildir, ancak Eğer gerekiyorsa bunları hakkında bilgi edinebilirsiniz [burada](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
