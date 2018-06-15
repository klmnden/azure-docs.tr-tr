---
title: Kapasite planlama için Service Fabric uygulamaları | Microsoft Docs
description: Service Fabric uygulaması için gereken işlem düğümleri sayısını tanımlamak açıklar
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: ''
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: ac8abbdbbe9125ea036d837c08e1089aa6d1e55d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212866"
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Kapasite planlama için Service Fabric uygulamaları
Bu belge, Azure Service Fabric uygulamaları çalıştırmak için gereken kaynakları (CPU, RAM, disk depolama) miktarı tahmin öğretir. Zaman içinde değiştirmek kaynak gereksinimlerinizi yaygındır. Siz hizmetinizi geliştirmek ve test, ve ardından üretim gidin ve popülerliği içinde uygulamanız büyüdükçe gibi daha fazla kaynak gerektirir genellikle birkaç kaynakları gerektirir. Uygulamanızı tasarlarken, uzun vadeli gereksinimleri düşünün ve hizmetinizin yüksek müşteri taleplerini karşılamak üzere ölçeklendirmek için izin seçimler.

 Service Fabric kümesi oluşturduğunuzda, ne sanal makineleri (VM'ler) tür kümeyi yapmak karar verin. Her VM sınırlı miktarda kaynak formunda CPU (çekirdekler ve hız), ağ bant genişliği, RAM ve disk depolaması ile birlikte gelir. Hizmetinizi zamanla büyüdükçe, daha büyük kaynakları sunar ve/veya daha fazla sanal makineleri kümenize eklemek VM'ler yükseltebilirsiniz. Kümeye dinamik olarak eklenir yeni VM'ler avantajlarından olabilmesi için ikinci yapmak için hizmetiniz başlangıçta mimari gerekir.

Bazı hizmetler pek çok veri VM'ler kendilerini yönetin. Bu nedenle, bu hizmetleri için kapasite planlama VM'lerin uygun CPU (çekirdekler ve hız) seçerek anlamı öncelikle performansı, odaklanmanız gerekir. Ayrıca, ağ aktarımları ne sıklıkta ortaya çıkan ve ne kadar verinin aktarıldığı dahil olmak üzere ağ bant genişliğini göz önünde bulundurmalısınız. Hizmetinizi iyi hizmet kullanım arttıkça gerçekleştirmek gerekirse, ağ tüm sanal makineleri istekleri küme ve yük dengelemek için daha fazla sanal makineleri ekleyebilirsiniz.

Büyük miktarlarda veri VM'ler yönetme hizmetleri için kapasite planlaması öncelikle boyutuna odaklanmanız gerekir. Bu nedenle, VM RAM ve disk depolama kapasitesi dikkatlice düşünün. Windows sanal bellek yönetim sisteminde disk alanı gibi RAM uygulama koduna bakın sağlar. Ayrıca, Service Fabric çalışma zamanı yalnızca etkin verileri bellekte tutulması ve soğuk veriler diske taşıyarak akıllı disk belleği sağlar. Uygulamalar, bu nedenle VM kullanılabilşir olandan daha fazla bellek kullanabilir. VM daha fazla disk depolama alanı RAM tutabilirsiniz olduğundan daha fazla RAM sahip performansı, yalnızca artırır. Seçtiğiniz VM VM istediğiniz verileri depolamak için büyük bir disk olmalıdır. Benzer şekilde, VM ile istediğiniz performansı sağlamak için yeterli miktarda RAM olmalıdır. Hizmetinizin veri zaman içinde büyürse, daha fazla sanal makineleri küme ve bölüm veri tüm sanal makineler ekleyebilirsiniz.

## <a name="determine-how-many-nodes-you-need"></a>Gereksinim duyduğunuz kaç düğümleri belirler
Hizmetinizi bölümleme hizmetinizin verileri ölçeklendirmenizi sağlar. Bölümleme hakkında daha fazla bilgi için bkz: [bölümleme Service Fabric](service-fabric-concepts-partitioning.md). Her bölüm tek bir VM'ye sığması gerekir, ancak tek bir VM üzerinde birden çok (küçük) bölüm yerleştirilebilir. Bu nedenle, daha fazla küçük bölümlemeye sahip olmak birkaç büyük bölümlemeye sahip olmak daha büyük esneklik sağlar. Dengelemeyi bölümleri çok sayıda sahip Service Fabric yükü artırır ve bölümler uygulaması yapılan işlem gerçekleştirilemiyor ' dir. Aynı zamanda hizmet kodunuzun sık farklı bölümlerde canlı veri parçalarını erişmesi gerekirse ayrıca daha fazla olası ağ trafiği vardır. Hizmetinizi tasarlarken, bu Artıları ve eksileri etkili bir bölümleme stratejisine gelmesi dikkatle düşünmelisiniz.

Uygulamanız bir yıl içinde DB_Size GB'lık büyümeye beklediğiniz bir depolama boyutuna sahip tek bir durum bilgisi olan hizmet sahip varsayalım. Daha fazla uygulama (ve bölümler) eklemek hazır olduğunuz bu yıl ötesinde büyüme deneyimi gibi.  Hizmetiniz için yineleme sayısı belirleyen çoğaltma faktörü (RF) toplam DB_Size etkiler. Tüm çoğaltmalar arasında toplam DB_Size DB_Size tarafından çarpılan çoğaltma faktördür.  Node_Size hizmetiniz için kullanmak istediğiniz düğüm başına disk alanı/RAM temsil eder. En iyi performans için küme üzerinde DB_Size belleğe sığması ve VM RAM bir Node_Size seçilmelidir. RAM kapasiteden büyük bir Node_Size ayırarak Service Fabric çalışma zamanı tarafından sağlanan disk belleği öğesine bağlı. Bu nedenle, performansınızı tüm verilerinizi etkin olarak değerlendirilir, uygun olmayabilir (o zamandan bu yana veri disk belleği giriş/çıkış). Ancak, verileri yalnızca bir kısmı etkin olduğu birçok hizmetler için daha düşük maliyetli değildir.

Maksimum performans için gereken düğüm sayısının aşağıdaki gibi hesaplanabilir:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Büyüme için hesabı
Hizmetiniz için ile başlayan DB_Size yanı sıra büyümeye beklediğiniz DB_Size göre düğüm sayısını işlem isteyebilirsiniz. Ardından, düğüm sayısını aşırı sağlama değil böylece hizmetinizi büyüdükçe düğüm sayısı artar. Ancak bölüm sayısı en fazla büyüme hizmetinizi çalıştırırken, gerekli olan düğüm sayısını dayanmalıdır.

(Örneğin, birkaç VM'yi kesilirse) herhangi bir beklenmeyen ani veya hata işleyebilir bazı ek makineler herhangi bir zamanda kullanılabilir olması uygundur.  Beklenen ani kullanarak ek kapasite belirlenemediğinden olsa da, bazı ek VM'ler ayırmak için bir başlangıç noktası olduğu (yüzde 5-10 ek).

Yukarıdaki tek bir durum bilgisi olan hizmet varsayar. Birden fazla durum bilgisi olan hizmet varsa, eşitlik diğer hizmetlerle ilişkili DB_Size eklemeniz gerekir. Alternatif olarak, her durum bilgisi olan hizmet için ayrı ayrı düğüm sayısını hesaplayabilirsiniz.  Hizmetinizi çoğaltmaları veya dengeli olmayan bölüm olabilir. Bölümler diğerlerinden daha fazla veri de olabilen aklınızda bulundurun. Bölümleme hakkında daha fazla bilgi için bkz: [makale en iyi uygulamalar üzerinde bölümleme](service-fabric-concepts-partitioning.md). Ancak, önceki eşitlik bölüm ve çoğaltma olan Service Fabric çoğaltmaları düğümler arasında iyileştirilmiş bir şekilde genişlediğini olduğunu sağladığından belirsiz.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Maliyeti hesaplama için bir elektronik kullanın
Şimdi şimdi bazı gerçek sayılar formülde yerleştirin. Bir [örnek elektronik tablo](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) üç tür veri nesneleri içeren bir uygulama için kapasite planlama gösterilmektedir. Her bir nesne için boyutuna ve kaç tane olmasını bekliyoruz nesneleri yaklaşık. Biz de her nesne türünde istiyoruz kaç çoğaltmaları seçin. Elektronik Tablo toplam kümede depolanması için bellek miktarını hesaplar.

Ardından bir VM boyutu ve aylık maliyeti girin. VM boyutuna göre elektronik tablo bölümleri fiziksel düğümlerde uyacak şekilde, verilerinizi bölmek için kullanmanız gerekir en az sayıda söyler. Çok sayıda, uygulamanızın belirli hesaplama uyum sağlayacak şekilde bölümleri isteyebilecekleri ve ağ trafiğini gerekiyor. Kullanıcı profili nesnelerini yönetme bölüm sayısı altı birinden artırmıştır elektronik tablo gösterir.

Şimdi, bu bilgilere dayanarak elektronik 26 düğümlü bir küme üzerinde istenen bölümler ve çoğaltmalar tüm verileri fiziksel olarak alabilir gösterir. Düğüm hatalarını ve yükseltmeleri uyum sağlamak için bazı ek düğümler isteyebilirsiniz ancak, bu küme densely, paketlenmiş. Elektronik tablo, aynı zamanda boş düğümleri gerekir çünkü birden fazla 57 düğümleri sahip herhangi bir ek değer sağladığını gösterir. 57 düğümleri yine de düğüm hatalarını ve yükseltmeleri uyum Git isteyebilirsiniz. Uygulamanızın belirli ihtiyaçlarını karşılamak üzere elektronik tabloya değiştirebilirsiniz.   

![Maliyet hesaplamasında elektronik tablo][Image1]

## <a name="next-steps"></a>Sonraki adımlar
Kullanıma [bölümleme Service Fabric Hizmetleri] [ 10] hizmetinizi bölümleme hakkında daha fazla bilgi edinmek için.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
