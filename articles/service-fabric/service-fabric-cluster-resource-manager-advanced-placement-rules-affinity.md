---
title: Service Fabric kümesi Kaynak Yöneticisi - benzeşim | Microsoft Docs
description: Benzeşim hizmet doku hizmetler için yapılandırma genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 57abea79a620aa83e16ad4cc2fd78a4294f2b278
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Yapılandırma ve Service Fabric hizmeti benzeşimini kullanma
Benzeşim Bulut ve mikro dünyaya daha büyük tek yapılı uygulamalar geçişini kolaylaştırır yardımcı olmak için temel olarak sağlanan bir denetimdir. Bunun yapılması rağmen Hizmetleri, performansı artırmak için bir en iyi duruma getirme yan etkileri olabilir olarak da kullanılır.

Daha büyük bir uygulama veya bir Service Fabric için aklınızda mikro (ya da dağıtılmış bir ortama) yalnızca tasarlanmış değildi getiren varsayalım. Bu geçiş türünü yaygındır. Ortamına tüm uygulama kaldırma tarafından bu paketleme başlattığınız ve emin olunmasını sorunsuz çalışıyor. Daha sonra tümünü birbirine konuşun farklı küçük Hizmetleri bölmeyi başlatın.

Sonuç olarak, uygulamanın bazı sorunlar yaşıyor bulabilirsiniz. Sorunları genellikle bu kategoriden ayrılır:

1. Bazı tek yapılı uygulama bileşeni X belgelenmemiş bir bağımlılık Y bileşende vardı ve yalnızca bu bileşenleri ayrı hizmetlerine açık. Bu hizmetleri artık kümedeki farklı düğümlerde çalışan olduğundan, bunlar bozuk.
2. Bu bileşenlerin üzerinden iletişim kurmasına (adlandırılmış kanallar yerel | bellek paylaşılan | diskteki dosyaları) ve gerçekten performansı artırmak için yerel bir paylaşılan kaynak şu anda yazma olması gerekir. Bu sabit bağımlılık daha sonra belki de kaldırılır.
3. Her şeyi sorun yoktur, ancak bu iki bileşenin gerçekten chatty/performans hassas olduğunu öyle. Bunlar ayrı hizmetlerine taşındıklarında genel tanked uygulama performansı veya gecikme artar. Sonuç olarak, genel uygulama beklentilerini karşılamıyor.

Bu durumda, biz bizim düzenleme iş kaybetmek istemediğiniz ve monolith geri dönmek istemiyorum. Son koşul bile düz bir iyileştirme istenebilir. Ancak, biz doğal Hizmetleri olarak çalışmak için bileşenler tasarlayabilirsiniz kadar (veya biz performans beklentilerini başka bir yolla çözebilir kadar) yerleşim yeri bazı duygusu gerek yapacağız.

Ne yapmanız gerekir? İyi üzerinde benzeşim kapatma çalışabilir.

## <a name="how-to-configure-affinity"></a>Benzeşim yapılandırma
Benzeşim ayarlamak için iki farklı hizmet arasında bir benzeşim ilişkisi tanımlayın. "Bir hizmet başka bir işaret" ve "Bu hizmet yalnızca burada hizmetinin çalıştığını çalıştırabilirsiniz." belirten olarak benzeşim düşünebilirsiniz Bazen biz (burada üst alt işaretleyin) bir üst/alt ilişkisi benzeşimi bakın. Benzeşim çoğaltmaları veya bir hizmet örnekleri olan başka bir hizmet olarak aynı düğümlere yerleştirilir sağlar.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Bir alt hizmeti yalnızca bir tek benzeşim ilişkisinde yer alabilir. İki üst hizmetler için aynı anda Benzetilen için alt istediyseniz, birkaç seçeneğiniz vardır:
> - İlişkileri ters (parentService1 ve parentService2 adresindeki geçerli alt hizmet noktası varsa), veya
> - Bir üst kurala göre bir hub olarak belirleyin ve tüm hizmetleri, bu hizmeti noktası. 
>
> Sonuçta elde edilen yerleştirme davranışını kümedeki aynı olmalıdır.
>

## <a name="different-affinity-options"></a>Farklı benzeşim seçenekleri
Benzeşim birkaç bağıntı düzenleri biri ile temsil edilir ve iki farklı mod vardır. En yaygın benzeşim NonAlignedAffinity veririz modudur. NonAlignedAffinity içinde çoğaltmaları ya da farklı hizmet örneklerinin aynı düğümlerinde yerleştirilir. Diğer AlignedAffinity moddur. Yalnızca durum bilgisi olan hizmetler ile hizalanmış benzeşim yararlıdır. Benzeşim hizalı iki durum bilgisi olan hizmet yapılandırma hizmetlerin ana olarak birbiriyle aynı düğümlere yerleştirilir sağlar. İkincil kopya aynı düğümlerine yerleştirilmesini bu hizmetlerin her çifti neden olur. Ayrıca NonAlignedAffinity durum bilgisi olan hizmetler için yapılandırmak üzere (daha az ortak rağmen) mümkündür. NonAlignedAffinity, iki durum bilgisi olan hizmet farklı kopyalarını aynı düğümler üzerinde çalışır, ancak kendi ana farklı düğümlerde şunun.

<center>
![Benzeşim modları ve bunların etkileri][Image1]
</center>

### <a name="best-effort-desired-state"></a>En iyi çaba istenen durumu
En iyi çaba bir benzeşim ilişkidir. Birlikte bulundurma özelliğini ya da aynı şekilde çalışan yürütülebilir bir işlemi yapar güvenilirlik aynı garanti sağlamaz. Bir benzeşim ilişkisi Hizmetleri'nde başarısız olabilir ve bağımsız olarak taşınmasına temelde farklı varlıklardır. Ancak bu sonlarını geçici bir benzeşim ilişkisi de kesebilir. Örneğin, kapasite sınırlamaları benzeşim ilişkisi hizmet nesneleri yalnızca bazılarını belirli bir düğümde uygun olduğu anlamına gelebilir. Bu durumlarda, yerinde bir benzeşim ilişkisi olsa, diğer kısıtlamaları nedeniyle zorlanamaz. Bunu yapmak Mümkünse, ihlalin daha sonra otomatik olarak düzeltilir.

### <a name="chains-vs-stars"></a>Zincirleri yıldız karşılaştırması
Bugün Küme Kaynak Yöneticisi'ni benzeşimi ilişkileri modeli zincirleri için mümkün değildir. Bu, bir alt bir benzeşim ilişkisi olan bir hizmettir anlamı başka bir benzeşim ilişkisi içinde üst öğesi olamaz. Bu ilişki türünde model istiyorsanız, etkin bir zincir yerine bir yıldız olarak model sahip. Bir yıldız bir zincirinden taşımak için alttaki alt ilk çocuğun üst öğeye bunun yerine üst öğe. Hizmetlerinizin düzenleme bağlı olarak, birden çok kez yapmanız gerekebilir. Doğal üst hizmet ise, bir yer tutucu olarak hizmet veren bir oluşturmanız gerekebilir. Gereksinimlerinize bağlı olarak, aynı zamanda içine ara isteyebilirsiniz [uygulama grupları](service-fabric-cluster-resource-manager-application-groups.md).

<center>
![Zincirleri vs. Yıldız bağlamında benzeşimi ilişkileri][Image2]
</center>

Bugün benzeşimi ilişkileri hakkında dikkat edilecek başka bir tek yönlü şeydir. Bu alt ile üst yerleştirilen benzeşim kural yalnızca zorlar anlamına gelir. Alt ile üst bulunduğundan emin olunmasını sağlamamasıdır. Farklı Hizmetleri ile farklı yaşam döngüleri vardır ve başarısız ve bağımsız olarak hareket beri benzeşim ilişkisi mükemmel veya anında zorlanamaz olduğunu dikkate almak önemlidir. Örneğin, bunu kilitlendi olduğundan üst aniden başka bir düğüme yöneltilir varsayalım. Küme Kaynak Yöneticisi ve Yük Devretme Yöneticisi Hizmetleri, tutarlı tutmak bu yana yük devretme ilk olarak, işler ve öncelik kullanılabilir. Yük devretme işlemi tamamlandıktan sonra benzeşim ilişkisi bozuk, ancak Küme Kaynak Yöneticisi'ni alt ile üst bulunduğu değil bildirimler kadar her şeyi ince taşıdığını düşündüğü. Bu tür denetimler düzenli olarak gerçekleştirilir. Küme Kaynak Yöneticisi'ni kısıtlamaları nasıl hesaplar hakkında daha fazla bilgi kullanılabilir [bu makalede](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), ve [bu](service-fabric-cluster-resource-manager-balancing.md) bu kısıtlamaların değerlendirilir tempoyla yapılandırma hakkında daha fazla ettiği.   


### <a name="partitioning-support"></a>Bölümleme desteği
Burada üst bölümlenmiş ilişkiler desteklenmez, benzeşim son benzeşim hakkında dikkat edin şeydir. Bölümlenmiş üst Hizmetleri sonunda desteklenmiyor olabilir, ancak bugün verilmez.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Makineler, küçük bir kümesini Hizmetleri sınırlamak veya hizmetleri, yük toplayarak kullanmak için [uygulama grupları](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png