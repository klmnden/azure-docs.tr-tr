---
title: Azure Service Fabric kümesini ölçekleme | Microsoft Docs
description: Azure Service Fabric kümeleri veya çıkış ve yukarı veya aşağı ölçeklendirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/13/2018
ms.author: aljo
ms.openlocfilehash: cb9cb3998ed8208ff7b19aee8a984e4c057408ae
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302255"
---
# <a name="scaling-azure-service-fabric-clusters"></a>Ölçeklendirme Azure Service Fabric kümeleri
Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir düğüm denir. Kümeler, potansiyel olarak binlerce düğümde içerebilir. Service Fabric kümesi oluşturduktan sonra küme yatay yönde ölçeklendirebilirsiniz (düğüm sayısını değiştirme) ya da dikey yönde (düğümlerin kaynakları değiştirin).  Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.  Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

Neden bir kümenin ölçeğini? Uygulama talepleri zamanla değişir.  Daha yüksek uygulama iş yükü veya ağ trafiği karşılamak ya da küme kaynaklarını talep düştüğünde azaltmak için küme kaynaklarını artırmam gerekiyor.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>Giriş ve çıkış ölçeklendirme ve yatay ölçeklendirme
Kümedeki düğüm sayısını değiştirir.  Yeni düğüm, kümeye katılmak sonra [Küme Kaynak Yöneticisi](service-fabric-cluster-resource-manager-introduction.md) Hizmetleri için var olan düğümleri üzerindeki yükü azaltan taşır.  Küme kaynaklarını verimli bir şekilde kullanılmayan, düğüm sayısını da azaltabilirsiniz.  Küme düğümleri bırakın gibi hizmetleri devre dışı düğümleri taşıyın ve yük arttıkça Kalan düğümlerde.  Sanal makine sayısı için kullanmak ve iş yükü değil Bu vm'lerdeki ödeme olduğundan Azure'da çalışan bir kümedeki düğüm sayısını azaltma, para tasarrufu yapabileceğiniz.  

- Avantajları: Teoride, Ölçek sonsuz.  Uygulamanız için ölçeklenebilirlik tasarlanmışsa, daha fazla düğüm ekleyerek sınırsız büyüme etkinleştirebilirsiniz.  Bulut ortamlarında araçları ekleme veya düğümleri, kapasiteyi ayarlamak daha kolaydır ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız kaldırma daha kolay hale getirir.  
- Olumsuz: Uygulamaları olmalıdır [ölçeklendirilebilirlik için tasarlanmış](service-fabric-concepts-scalability.md).  Uygulama veritabanları ve kalıcı olarak iyi ölçeklendirme yapmasını ek mimari iş gerektirebilir.  [Güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md) Service Fabric durum bilgisi olan hizmetler, ancak çok uygulama verilerinizi ölçeklendirme kolaylaştırır.

Sanal makine ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Her düğüm türü, ölçeklendirilebilir veya out bağımsız olarak, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri yapılabilir. 

Azure kümesine ölçeklendirme, aşağıdaki yönergeleri göz önünde bulundurun:
- Üretim iş yükleri çalıştıran birincil düğüm türü, her zaman beş veya daha fazla düğüme sahip olmalıdır.
- durum bilgisi olan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman beş veya daha fazla düğüme sahip olmalıdır.
- durum bilgisi olmayan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman iki veya daha fazla düğüme sahip olmalıdır.
- Herhangi bir düğüm türü [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Silver veya Gold beş veya daha fazla düğüm her zaman olmalıdır.
- Rastgele VM örnekleri/düğümleri bir düğüm türünden kaldırın değil, her zaman sanal makine ölçek kümesi ölçeği özelliği aşağı kullanın. Rastgele VM örnekleri silinmesini sistemleri düzgün bir şekilde yük dengeleme olanağı olumsuz etkileyebilir.
- Otomatik ölçeklendirme kurallarını kullanarak ayarlarsanız kuralları ve böylece ölçek artırma (sanal makine örnekleri kaldırma) aynı anda tek bir düğüm gerçekleştirilir. Aynı anda birden fazla örneğini ölçeklendirme güvenli değildir.

Kümenizde Service Fabric düğüm türleri, oluşur beri arka uç sanal makine ölçek kümeleri, yapabilecekleriniz [otomatik ölçeklendirme kurallarını ayarlamaya veya el ile ölçeklendirme](service-fabric-cluster-scale-up-down.md) her düğüm türü/sanal makine ölçek kümesi.

### <a name="programmatic-scaling"></a>Programlı olarak ölçeklendirme
Birçok senaryoda [el ile veya otomatik ölçeklendirme kuralları ile bir küme ölçeklendirme](service-fabric-cluster-scale-up-down.md) iyi bir çözümdür. Daha Gelişmiş senaryolar için bunların doğru uygun olmayabilir. Bu yaklaşımların olası dezavantajları şunlardır:

- El ile ölçeklendirme oturum açın ve istek açıkça ölçeklendirme işlemleri yapmanızı gerektirir. Ölçeklendirme işlemleri sık veya beklenmeyen zamanlarda gerekiyorsa, bu yaklaşım, iyi bir çözüm olmayabilir.
- Otomatik ölçeklendirme kurallarını örneğini bir sanal makine ölçek kümesinden kaldırırken bir dayanıklılık düzeyi Silver veya Gold düğüm türüne sahip olmadığı sürece bunlar otomatik olarak bilgi o düğümün ilişkili bir Service Fabric kümesinden kaldırmayın. Otomatik ölçeklendirme kurallarını ölçekte düzeyini ayarla (yerine Service Fabric düzeyinde) çalıştığından, otomatik ölçeklendirme kurallarını düzgün biçimde kapatılması olmadan Service Fabric düğümleri kaldırabilirsiniz. Bu rude düğümü kaldırma 'ghost' Service Fabric düğüm durumu ölçeğini işlemlerinden sonra bırakır. Service Fabric kümesindeki kaldırılan düğüm durumunu düzenli aralıklarla temizlemek bir kişi (veya hizmet) gerekir.
- Hiçbir ek temizleme gerektiği şekilde bir düğüm türü ile bir dayanıklılık düzeyi Silver veya Gold kaldırılan düğümü, otomatik olarak temizlenir.
- Bulunmasına rağmen [çok sayıda ölçüm](../azure-monitor/platform/autoscale-common-metrics.md) otomatik ölçeklendirme kuralları tarafından desteklenen, yine de öyledir sınırlı. Senaryonuz için küme içinde kapsanmayan bazı ölçüme göre ölçeklendirme çağırırsa, otomatik ölçeklendirme kurallarını iyi bir seçenek olmayabilir.

Service Fabric ölçeklendirme yaklaşımınız, senaryoya bağlıdır. Ekleme veya düğümleri el ile kaldırma yeteneği, ölçeklendirme seyrek, büyük olasılıkla yeterli olur. Daha karmaşık senaryolarda, otomatik ölçeklendirme kurallarını ve SDK'ları programlı bir şekilde ölçeklendirme imkanı gösterme güçlü alternatifleri sunar.

Azure API'leri, Service Fabric kümeleri ve ölçek kümelerine uygulama program aracılığıyla sanal makine ile çalışmak için izin yok. Mevcut otomatik ölçeklendirme seçenekleri senaryonuz için işe yaramazsa, bu API'leri, özel ölçeklendirme mantığını mümkün kılar. 

Otomatik ölçeklendirme bu 'giriş yapılmış' işlevi bir yaklaşım, yeni bir durum bilgisi olmayan hizmet ölçeklendirme işlemlerini yönetmek için Service Fabric uygulaması eklemektir. Kendi hizmet ölçeklendirme oluşturma, Denetim ve sağlamadığından, uygulamanızın üzerinde yüksek derecede ölçekleme davranışı kullanıcının sağlar. Bu, ne zaman veya bir uygulama veya ölçeklendirir üzerinde kesin denetim gerektiren senaryolar için faydalı olabilir. Ancak, bu denetim, kod karmaşıklığı bir tradeoff ile birlikte gelir. Bu yaklaşımı kullanarak ihtiyacınız Önemsiz olan kendi ölçekleme kod, anlamına gelir. Hizmetin içinde `RunAsync` yöntemi, bir dizi Tetikleyicileri belirleyebilir ölçeklendirme (küme boyutu üst sınırı gibi parametreleri denetleniyor ve cooldowns ölçeklendirme dahil) gerekli olup olmadığını.   

Kullanılan sanal makine ölçek kümesi için etkileşimlerini (hem de geçerli sanal makine örneği sayısını denetlemek ve değiştirmek için) API [fluent Azure bilgi işlem yönetimi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Fluent işlem kitaplığı, sanal makine ölçek kümeleri ile etkileşim kurmaya yönelik kullanımı kolay API sağlar.  Service Fabric kümesi etkileşime girmek için [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Ölçeklendirme kod, ancak ölçeği kümedeki bir hizmet olarak çalıştırmak zorunda değildir. Her ikisi de `IAzure` ve `FabricClient` ölçeklendirme hizmeti bir konsol uygulaması veya Windows hizmeti Service Fabric uygulaması dışında çalıştırma kolayca olabilir, böylece bunların ilişkili Azure kaynakları için uzaktan bağlanabilirsiniz.

Bağlı olarak bu sınırlamalar, isteyebilirsiniz [uygulama, otomatik ölçekleme modelleri daha fazla özelleştirilmiş](service-fabric-cluster-programmatic-scaling.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Ölçeği artırmayı veya dikey ölçeklendirme 
Kümedeki düğümler kaynakları (CPU, bellek veya depolama) değiştirir.
- Avantajları: Yazılım ve uygulama mimarisi aynı kalır.
- Olumsuz: Kaynakları tek tek düğümlere ne kadar artırmak için bir sınır olduğundan sınırlı ölçek. Kapalı kalma süresi, fiziksel veya sanal kaynak ekleme veya kaldırma için makineleri çevrimdışına almak ihtiyacınız olacağı için.

Sanal makine ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Ardından her düğüm türü ayrı olarak yönetilebilir.  Ölçeği artırılabilen veya azaltılabilen bir düğüm türü, Ölçek kümesindeki sanal makine örnekleri SKU'su değiştirilmesini kapsar. 

> [!WARNING]
> Adresindeki çalışmadığı sürece bir ölçek kümesi/düğüm türündeki sanal makine SKU'su değiştirmeyin öneririz [dayanıklılık Gümüş veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM SKU boyutunu değiştirerek verileri yıkıcı yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için bazı kabiliyeti olmadan işlem durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile diğer öngörülemeyen operasyonel sorunlara neden olabilir. 
>

Aşağıdaki kılavuz, Azure kümesine ölçeklerken göz önünde bulundurun:
- Bir birincil düğüm türü ölçeklendirme ise, hiçbir zaman, daha fazlasını ölçeğini [güvenilirlik katmanını](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) sağlar.

Düğüm türü ölçeği artırılabilen veya azaltılabilen işlemi, birincil olmayan veya birincil düğüm türü olmasına bağlı olarak farklıdır.

### <a name="scaling-non-primary-node-types"></a>Birincil olmayan düğüm türleri ölçeklendirme
Yeni bir düğüm türü, ihtiyacınız olan kaynakları ile oluşturun.  Yeni düğüm türü içerecek şekilde hizmetleri çalıştırma yerleştirme kısıtlamaları güncelleştirin.  Aşamalı olarak (bir kerede), böylece küme güvenilirliğini etkilenmez eski düğüm türü örneği sayısı örnek sayısı sıfıra indirin.  Eski düğüm türü kullanımdan alındı olarak Hizmetleri aşamalı olarak yeni düğüm türü geçirin.

### <a name="scaling-the-primary-node-type"></a>Birincil düğüm türü ölçeklendirme
Birincil düğüm türündeki sanal makine SKU'su değiştirmemenizi öneririz. Daha fazla küme kapasitesine ihtiyacınız varsa, daha fazla örnek eklenmesi önerilir. 

Varsa mümkün değil, yeni bir küme oluşturabileceğiniz ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa) eski kümenizden. Herhangi bir sistem hizmet durumunu geri yüklemek gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların kümenizde çalışan uygulamalarınızı yeni kümeye dağıtın, geri yüklemek için hiçbir şey vardır. Desteklenmeyen bir rotayı ve sanal makine SKU'su değiştirmek istediğiniz karar verirseniz, ardından belgelenir sanal makine ölçek kümesi yeni SKU yansıtacak şekilde Model tanımı. Kümenizi yalnızca bir düğüm türü varsa, daha sonra durum bilgisi olan tüm uygulamalarınızı tüm yanıt emin olun [hizmet çoğaltması yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) vakitli ve hizmet çoğaltma yeniden (yapı içinde çoğaltma takılmış gibi) beş dakikadan kısa bir süre (Gümüş dayanıklılık düzeyi için) süresidir. 

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Azure kümesine veya dışa ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Azure bir kümeyi programlama yoluyla ölçeklendirme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Tek başına küme içe veya dışa ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

