---
title: Service Fabric uygulamaları için kapasite planlaması | Microsoft Docs
description: Service Fabric uygulaması için gerektirdiği işlem düğüm sayısını belirleme açıklar
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
ms.openlocfilehash: d7ca566b86ed79aa773d7af2553223c79ed9944a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64701852"
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Kapasite planlaması için Service Fabric uygulamaları
Bu belge, Azure Service Fabric uygulamalarınızı çalıştırmak için gereken kaynakları (CPU, RAM, disk depolama) miktarını tahmin edin öğretir. Zaman içinde kaynak gereksinimlerinizi yaygındır. Bazı kaynaklar genellikle, hizmetinizi geliştirme/test ve sonra üretime gidebilir ve popülerliği uygulamanız büyüdükçe daha fazla kaynak gerektiren gerektirir. Uygulamanızı tasarlarken, uzun vadeli gereksinimlerini düşünün ve hizmetinizin yüksek müşteri talebini karşılayacak şekilde ölçeklendirilen izin seçimler.

 Service Fabric kümesi oluşturduğunuzda, ne tür sanal makineler (VM'ler) küme olun karar verin. Her VM, sınırlı miktarda CPU (çekirdekler ve hız), ağ bant genişliği, RAM ve disk depolama alanı biçiminde kaynaklar ile birlikte gelir. Hizmetinizi zamanla büyüdükçe, daha büyük kaynaklara sunar ve/veya kümenize daha fazla VM ekleme Vm'leri yükseltebilirsiniz. Kümeye dinamik olarak eklenin yeni VM'ler avantajlarından yararlanmanıza olanak vermek amacıyla ikincisini yapmak için hizmetinize başlangıçta mimari gerekir.

Bazı hizmetler, VM'ler üzerinde çok az için hiçbir veri yönetin. Bu nedenle, bu hizmetleri için kapasite planlama, diğer bir deyişle sanal makinelerin uygun CPU (çekirdekler ve hız) seçerek öncelikli olarak performansı odaklanmanız gerekir. Ayrıca, ağ bant genişliği, ağ aktarımı ne sıklıkla gerçekleştiğini ve ne kadar verinin aktarıldığı dahil olmak üzere düşünmelisiniz. Hizmetinizi iyi hizmet kullanım arttıkça gerçekleştirmek gerekiyorsa, tüm sanal makinelerde ağ istekleri küme ve Yük Dengelemesi yapmak için daha fazla sanal makine ekleyebilirsiniz.

VM'ler üzerinde büyük miktarda veriyi yönetmesine Hizmetleri için kapasite planlaması öncelikle boyutuna odaklanmanız gerekir. Bu nedenle, sanal makinenin RAM ve disk depolama kapasitesi dikkatli bir şekilde düşünmelisiniz. Windows sanal bellek yönetim sisteminde RAM gibi uygulama koduna bakın. disk alanı sağlar. Ayrıca, Service Fabric çalışma zamanı yalnızca sık erişimli veriler bellek içinde tutarak ve soğuk verileri diske taşıyarak akıllı disk belleği sunar. Uygulamalar, bu nedenle VM kullanılabilşir olandan daha fazla bellek kullanabilir. Sanal Makinenin RAM daha fazla disk depolama alanı tutabilirsiniz olduğundan daha fazla RAM'i olan performans, yalnızca artırır. Seçtiğiniz VM, sanal makinede, istediğiniz verileri depolamak için büyük bir disk olması gerekir. Benzer şekilde, VM, size istediğiniz performansı sağlamak için yeterli RAM olmalıdır. Hizmetinizin veriler zamanla artar, daha fazla sanal makine verileri ve küme için tüm sanal makinelerde ekleyebilirsiniz.

## <a name="determine-how-many-nodes-you-need"></a>Gereksinim duyduğunuz kaç düğümleri
Hizmetinizi bölümleme, hizmetinizin verileri ölçeklendirmenize olanak tanır. Bölümleme hakkında daha fazla bilgi için bkz: [Service Fabric bölümleme](service-fabric-concepts-partitioning.md). Her bölüm tek bir VM uygun olmalıdır, ancak tek bir VM'de birden çok (küçük) bölüme yerleştirilebilir. Bu nedenle, daha fazla küçük bölümlemeye sahip olmak, birkaç büyük bölümlemeye sahip olmak daha büyük esnekliği sağlar. Bölümlerin çok sahip Service Fabric yükü artırır ve bölümler arasında işlemler gerçekleştirilemiyor dengedir. Aynı zamanda hizmet kodunuzu sık farklı bölümlerde canlı veri parçalarını erişmesi gerekiyorsa de daha fazla olası ağ trafiği mevcuttur. Hizmetinizi tasarlarken dikkatli bir şekilde bu ve etkili bir bölümleme stratejisinde gelmesi dezavantajları düşünmelisiniz.

Uygulamanız DB_Size GB ile bir yıl büyümesine beklediğiniz bir depo boyutuna sahip tek bir durum bilgisi olan hizmet sahip varsayalım. Daha fazla uygulama (ve bölümler) eklemek için bu yıl ötesinde büyüme deneyimi gibi.  Hizmetiniz için yineleme sayısını belirleyen çoğaltma faktörü (RF) toplam DB_Size etkiler. Tüm yinelemeler genelinde toplam DB_Size DB_Size ile çarpılmış çoğaltma faktördür.  Node_Size hizmetiniz için kullanmak istediğiniz düğümü başına disk alanı/RAM temsil eder. En iyi performans için küme üzerinde DB_Size belleğe sığması gereken ve sanal makinenin RAM bir Node_Size seçilmelidir. RAM kapasiteden büyük bir Node_Size ayırarak, Service Fabric çalışma zamanı tarafından sağlanan disk belleği bağlı. Bu nedenle, performansınızı tüm verilerinizi sık erişimli olarak kabul edilir, uygun olmayabilir (o zamandan bu yana veriler havuzda daraltma veya genişletme). Ancak, yalnızca bir bölümü verilerin etkin olduğu birçok hizmetler için daha uygun maliyetli olur.

En yüksek performans için gereken düğüm sayısı şu şekilde hesaplanabilir:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Büyümenin hesaba katılması
Hizmetiniz için başlamış DB_Size yanı sıra büyümeye beklediğiniz DB_Size göre düğüm sayısını işlem isteyebilirsiniz. Ardından, böylece, düğüm sayısını fazladan sağlama değil, hizmetinize büyüdükçe düğüm sayısını büyütün. Ancak bölüm sayısı maksimum büyüme hizmetinizi çalıştırırken gereken düğüm sayısını dayanmalıdır.

Bazı ek makineler herhangi bir zamanda kullanılabilir (örneğin, birkaç sanal makine kesilirse) herhangi bir hata veya beklenmedik artışları işleyebilmesi uygundur.  Ek kapasite, beklenen ani artışlar kullanılarak belirlenmesi, ancak birkaç ek VM'ler ayırmak için bir başlangıç noktası olan (yüzde 5-10 ek).

Önceki tek bir durum bilgisi olan hizmet varsayar. Birden fazla durum bilgisi olan hizmet varsa, eşitliğe diğer hizmetlerle ilgili DB_Size eklemeniz gerekir. Alternatif olarak, her durum bilgisi olan hizmet için ayrı ayrı düğüm sayısını hesaplayabilir.  Hizmetinizi çoğaltmalar veya dengeli olmayan bölüm olabilir. Bölüm diğerlerinden daha fazla veri de olabilen aklınızda bulundurun. Bölümleme hakkında daha fazla bilgi için bkz: [yönelik en iyi uygulamalar makalesi bölümleme](service-fabric-concepts-partitioning.md). Ancak, önceki Denklem bölüm ve çoğaltma olan Service Fabric çoğaltmaları düğümleri arasında en iyi duruma getirilmiş bir şekilde yayıldığını ulaşılamamasını garantilediğinden belirsiz.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Maliyet hesaplamasında bir elektronik tablo kullanın
Şimdi bazı gerçek sayılar formülde şimdi yerleştirin. Bir [örnek elektronik tablo](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) üç tür veri nesneleri içeren bir uygulama için kapasite planlama işlemi gösterilmektedir. Her bir nesne için kaç olmasını bekliyoruz nesneleri ve boyutuna yakın. Ayrıca her nesne türü istiyoruz kaç çoğaltmaları seçiyoruz. Elektronik Tablo toplam kümede depolanacak bellek miktarını hesaplar.

Ardından bir VM boyutu ve aylık maliyet girin. VM boyutuna göre elektronik tablo bölümleri fiziksel düğümlerde uyum sağlamak için verilerinizi bölmek için kullanmanız gerekir en az sayıda söyler. Bölümler, uygulamanızın belirli hesaplama uyum sağlamak için daha fazla sayıda isteyebilecekleri ve ağ trafiğini gerekiyor. Kullanıcı profili nesnelerini yönetme bölüm sayısı bir altı arttı elektronik gösterir.

Şimdi, bu bilgilere dayanarak elektronik tablo üzerinde 26 düğümlü bir küme istenen bölümleri ve çoğaltmalarını ile tüm verileri fiziksel olarak alabilir gösterir. Düğüm hataları ve yükseltmeler için bazı ek düğümler isteyebilirsiniz ancak bu küme densely, paketlenmiş. Elektronik tablo, aynı zamanda boş düğümü olur çünkü 57'den fazla düğümleriniz ek değer olmadan sağladığını gösterir. 57 düğümlerin üzerinde düğüm hataları ve yükseltmeler için yine de gitmek isteyebilirsiniz. Uygulamanızın belirli ihtiyaçlarına uygun elektronik değiştirebilirsiniz.   

![Maliyet hesaplamasında elektronik tablo][Image1]

## <a name="next-steps"></a>Sonraki adımlar
Kullanıma [bölümleme Service Fabric Hizmetleri] [ 10] hizmetinizi bölümleme hakkında daha fazla bilgi için.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
