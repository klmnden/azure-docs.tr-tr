---
title: Service Fabric Küme Kaynak Yöneticisi - benzeşim | Microsoft Docs
description: Service Fabric Hizmetleri için benzeşim yapılandırmaya genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 29377492b90f366227ca7bedf85890b7734ea25f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62118424"
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Yapılandırma ve Service Fabric hizmet benzeşimi kullanma
Benzeşim Bulut ve mikro hizmetler dünyaya tek parçalı büyük uygulamaların geçişi kolaylaştırmak amacıyla, esas olarak sağlanan bir denetimdir. Bunun yapılması rağmen Hizmetleri performansını iyileştirme için bir iyileştirme yan etkileri olabilir olarak da kullanılır.

Daha büyük bir uygulamanın ya da bir mikro hizmetler, Service fabric'e unutmayın (veya herhangi bir dağıtılmış ortamda) ile yalnızca tasarlanmış değildi getirdiğiniz varsayalım. Bu tür geçişe yaygındır. Uygulamanın tamamında ortamına kaldırarak tarafından paketleyerek başlattığınız ve emin olunmasını sorunsuz çalışıyor. Daha sonra tüm birbirine konuşun farklı daha küçük hizmetlerine bozucu başlatın.

Sonunda, uygulama bazı sorunlar yaşıyor bulabilirsiniz. Sorunları genellikle bu kategoriden birine girer:

1. Bazı X tek parçalı uygulama belgelenmemiş bir bağımlılık Y bileşende bileşeninde ve yalnızca bu bileşenleri ayrı hizmet olarak açılır. Bu hizmetleri artık kümedeki farklı düğümlere çalışır durumda olduğundan, bozuk.
2. Aracılığıyla bu bileşenleri iletişim (yerel adlandırılmış kanallar | paylaşılan bellek | diskteki dosyaları) ve bunların gerçekten performansı artırmak için yerel bir paylaşılan kaynak için şu anda yazabilmesi olması gerekir. Bu sabit bağımlılık daha sonra belki de kaldırılır.
3. Her şeyi bir sakınca yoktur, ancak bu iki bileşenin gerçekten geveze/performans hassas olduğunu ettik. Bunlar ayrı hizmet olarak taşındıklarında genel tanked uygulama performansı veya gecikme süresi artar. Sonuç olarak, genel uygulama beklentilerini karşılamıyor.

Bu gibi durumlarda, size sunduğumuz yeniden düzenleme iş kaybetmek istemediğiniz ve tek parçalı mimariden geri dönmek istemiyorsanız. Son durum bile düz bir iyileştirme istenebilir. Ancak biz bileşenleri doğal olarak hizmet olarak çalışacak şekilde yeniden tasarlayıp tasarlayamayacağınızı kadar (veya başka bir şekilde performans beklentilerini çözme kadar) bazı yerleşim yeri duygusu gerek ekleyeceğiz.

Ne yapmanız gerekir? Benzeşimine kapatma de deneyebilirsiniz.

## <a name="how-to-configure-affinity"></a>Benzeşim yapılandırma
Benzeşim ayarlamak için iki farklı hizmetler arasında bir benzeşim ilişkisi tanımlayın. "Bir hizmetin başka bir işaretleme" ve "Bu hizmet yalnızca burada hizmetinin çalışıp çalışmadığını çalıştırabilirsiniz." diyerek olarak benzeşim düşünebilirsiniz Bazen benzeşimi (burada üst alt öğe noktası) bir üst/alt ilişkisi olarak diyoruz. Çoğaltmalar veya bir hizmetin örneklerine başka bir hizmet olarak aynı düğümlere yerleştirilir benzeşimi sağlar.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Bir alt hizmeti yalnızca bir tek benzeşimi ilişkisinde yer alabilir. İki üst hizmet için aynı anda Benzetilen için alt istediyseniz, birkaç seçeneğiniz vardır:
> - İlişkileri ters (parentService1 ve geçerli alt hizmeti noktası parentService2 varsa), veya
> - Üst öğeyi bir kurala göre bir merkez olarak belirleyin ve tüm hizmetleri bu hizmet in üzerine gelin. 
>
> Sonuçta elde edilen vranışı kümedeki aynı olmalıdır.
>

## <a name="different-affinity-options"></a>Farklı bir benzeşim seçenekleri
Benzeşim birkaç bağıntı düzenlerden birini ifade edilir ve iki farklı mod vardır. En yaygın benzeşim NonAlignedAffinity dediğimiz modudur. NonAlignedAffinity çoğaltmalar veya farklı hizmet örneklerinin aynı düğümlere yerleştirilir. Diğer AlignedAffinity modudur. Hizalanmış benzeşimi, yalnızca durum bilgisi olan hizmetlerle yararlıdır. İki durum bilgisi olan hizmet benzeşimi hizalı için yapılandırma seçimlerine hizmetlerin birbiriyle aynı düğümlere yerleştirilir sağlar. Her çift ikinciller için söz konusu hizmetler aynı düğümlere yerleştirilmesine neden olur. Ayrıca durum bilgisi olan hizmetler için NonAlignedAffinity yapılandırmak için (daha az ortak karşın) mümkündür. NonAlignedAffinity, iki durum bilgisi olan hizmetler farklı kopyaya aynı düğümler üzerinde çalışır, ancak bunların seçimlerine farklı düğümlere bulunabileceğini.

<center>

![Benzeşim modlarını ve bunların etkileri][Image1]
</center>

### <a name="best-effort-desired-state"></a>En iyi çaba istenen durum
En iyi çaba bir benzeşim ilişkidir. Düzenleme ya da aynı şekilde çalışan yürütülebilir işlem yapar güvenilirlik aynı garanti sağlamaz. Bir benzeşim ilişkisinde başarısız olabilir ve bağımsız olarak taşınabilir tamamen farklı varlıklar hizmetleridir. Ancak bu sonu geçici bir benzeşim ilişkisi de kesebilir. Örneğin, kapasite sınırlamaları benzeşim ilişkisi hizmet nesneleri yalnızca bazıları belirli bir düğümde en uygun olan anlamına gelebilir. Bu gibi durumlarda, yerinde bir benzeşim ilişkisi olsa da diğer kısıtlamaları nedeniyle zorlanamaz. Bunu yapmak Mümkünse, ihlalin daha sonra otomatik olarak düzeltilir.

### <a name="chains-vs-stars"></a>Zincirleri yıldız karşılaştırması
Bugün küme kaynak yöneticisi için benzeşim ilişkilerin modelini zincirleri mümkün değildir. Bu alt öğe bir benzeşim ilişkisi olan bir hizmeti olan anlamı, başka bir benzeşim ilişkisi içinde üst olamaz. Bu ilişki türünde, model istiyorsanız, verimli bir zincir yerine bir yıldız olarak model gerekir. İçin bir yıldız bir zincirinden taşımak için alttaki alt ilk alt öğenin üst için bunun yerine üst öğe. Hizmetlerinizi düzenleme bağlı olarak, birden çok kez yapmanız gerekebilir. Doğal üst hizmet yok ise, bir yer tutucu olarak hizmet veren bir tane oluşturmak zorunda kalabilirsiniz. Gereksinimlerinize bağlı olarak, ayrıca içine öğrenmek isteyebilirsiniz [uygulama grupları](service-fabric-cluster-resource-manager-application-groups.md).

<center>

![Zincirleri vs. Yıldız bağlamında benzeşimi ilişkileri][Image2]
</center>

Bugün benzeşimi ilişkileri hakkında dikkat edilecek başka bir şey, varsayılan olarak yönlüdür ' dir. Başka bir deyişle, alt ile üst yerleştirilen benzeşim kuralı yalnızca zorlar. Alt ile üst bulunduğu garantilemez. Bu nedenle, benzeşim ihlali yok ve herhangi bir nedenden dolayı ve ihlali gidermek için--üst alt öğenin düğüme taşıdığınızdan ihlali düzelttikten bile--alt üst öğenin düğüme ardından taşımak için uygun değildir üst th için taşınır değil e alt düğümü. Config ayarlanıyor [MoveParentToFixAffinityViolation](service-fabric-cluster-fabric-settings.md) için true, yönlülüğü kaldırmanız. Farklı Hizmetleri ile farklı yaşam döngülerine sahip başarısız ve bağımsız olarak hareket beri benzeşim ilişkisi mükemmel veya anında zorlanamaz olduğunu unutmayın. Örneğin, kaldığı için üst aniden başka bir düğüme devreder varsayalım. Küme Kaynak Yöneticisi ve yük devretme Hizmetleri, tutarlılığını, koruma bu yana yük devretme ilk olarak, işlemek ve öncelik kullanılabilir. Yük devretme işlemi tamamlandıktan sonra benzeşim ilişkisi bozuk, ancak alt ile üst bulunduğu değil bildirimler kadar her şeyi iyi Küme Kaynak Yöneticisi düşünüyor. Bu tür denetimler düzenli gerçekleştirilir. Küme Kaynak Yöneticisi kısıtlamalar nasıl hesaplar hakkında daha fazla bilgi kullanılabilir [bu makalede](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), ve [bunu](service-fabric-cluster-resource-manager-balancing.md) bu kısıtlamaları olan temposu yapılandırma hakkında daha fazla hakkında konuşuyor değerlendirilir.   


### <a name="partitioning-support"></a>Bölümleme desteği
Üst burada bölümlenen ilişkileri desteklenmez, benzeşim son benzeşimi hakkında dikkat edin şeydir. Bölümlenmiş üst Hizmetleri sonunda desteklenmiyor olabilir, ancak bugün verilmez.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Küçük bir makine Hizmetleri sınırlamak veya hizmetlerin yükü toplayarak kullanmak için [uygulama grupları](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png