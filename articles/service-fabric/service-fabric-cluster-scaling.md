---
title: Azure Service Fabric küme ölçeklendirme | Microsoft Docs
description: Service Fabric kümeleri veya çıkışı ve yukarı veya aşağı Ölçeklendirmesi hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: f9e3f190decdc907cf57a0235b9d7142081bd2f1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208038"
---
# <a name="scaling-service-fabric-clusters"></a>Ölçeklendirme Service Fabric kümeleri
Service Fabric kümesi bir ağa bağlı içine, mikro dağıtılır ve yönetilen sanal veya fiziksel makineler kümesidir. Bir makine ya da bir kümenin parçasıysa VM düğüm denir. Küme düğümleri binlerce potansiyel olarak içerebilir. Service Fabric kümesi oluşturduktan sonra küme yatay yönde ölçeklendirebilirsiniz (düğüm sayısını değiştirmenize) veya dikey (düğümlerin kaynakları değiştirin).  Küme herhangi bir zamanda kümede iş yükleri çalıştırırken bile ölçeklendirebilirsiniz.  Küme ölçeklendirir gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

Neden küme ölçeklendirme? Uygulama gereksinimleri zamanla değiştirin.  Artan uygulama iş yükü veya ağ trafiğini karşılamak ya da isteğe bağlı düştüğünde küme kaynaklarını azaltmak için küme kaynaklarını artırma gerekebilir.

### <a name="scaling-in-and-out-or-horizontal-scaling"></a>Giriş ve çıkış ölçeklendirme ya da yatay ölçek
Kümedeki düğüm sayısını değiştirir.  Kümeye yeni düğümler katılma sonra [küme Resource Manager](service-fabric-cluster-resource-manager-introduction.md) Hizmetleri için var olan düğümler üzerindeki yükü azaltır taşır.  Küme kaynaklarını verimli bir şekilde kullanılmayan, düğüm sayısını da azaltabilirsiniz.  Küme düğümleri bırakın gibi hizmetleri devre dışı düğümleri taşıyın ve yük arttıkça Kalan düğümlerde.  Azure'da çalışan bir küme içindeki düğümlerin sayısının azaltılması, VM'lerin sayısını, kullanım ve iş yükünün olmayan bu vm'lerde ücret ödersiniz beri para kaydedebilirsiniz.  

- Olumlu: Sonsuz ölçek, teorik.  Uygulamanızı ölçeklenebilirlik için tasarlanmıştır, daha fazla düğüm ekleyerek sınırsız büyüme etkinleştirebilirsiniz.  Bulut ortamlarında araç ekleme veya düğümleri, böylece kapasite ayarlanacak kolaydır ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız kaldırma daha kolay hale getirir.  
- Olumsuz: Uygulamaları olmalıdır [ölçeklenebilirlik için tasarlanmış](service-fabric-concepts-scalability.md).  Uygulama veritabanları ve kalıcılığı de ölçeklendirmek için ek bir mimari iş gerektirebilir.  [Güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md) Service Fabric durum bilgisi olan Hizmetleri'nde ancak çok uygulama verilerinizi ölçeklendirmek kolaylaştırır.

### <a name="scaling-up-and-down-or-vertical-scaling"></a>Yukarı ve aşağı doğru ölçeklendirme veya dikey ölçeklendirme 
Kümedeki düğümler kaynakları (CPU, bellek veya depolama) değiştirir.
- Olumlu: Yazılım ve uygulama mimarisi aynı kalır.
- Olumsuz: tek düğümler üzerindeki kaynaklara ne kadar artırmak için bir sınır olduğundan sınırlı ölçek. Kapalı kalma süresi, çünkü fiziksel veya sanal ekleyin ve kaynakları kaldırmak için makineleri çevrimdışına almak üzere gerekecektir.

## <a name="scaling-an-azure-cluster-in-or-out"></a>Bir Azure bir küme veya ölçeklendirme
Sanal makine ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynaktır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Her düğüm türü, genişletilebilir veya çıkışı bağımsız olarak, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini olabilir. 

Bir Azure küme ölçeklendirme, aşağıdaki yönergeleri göz önünde bulundurun:
- Üretim iş yükleri çalıştıran birincil düğüm türleri, her zaman beş veya daha fazla düğüme sahip olmalıdır.
- durum bilgisi olan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman beş veya daha fazla düğüme sahip olmalıdır.
- Durum bilgisiz üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman iki veya daha fazla düğüme sahip olmalıdır.
- Herhangi bir düğüm türü [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) altın veya gümüş her zaman beş veya daha fazla düğüme sahip olmalıdır.
- Değil rastgele VM örnekleri/düğümleri bir düğüm türünden kaldırmak için her zaman sanal makine ölçek kümesi ölçek özelliğini kullanın. Rastgele VM örnekleri silinmesini sistemleri düzgün bir şekilde yük dengeleme olanağı olumsuz etkileyebilir.
- Otomatik ölçeklendirme kurallarını kullanarak ayarlarsanız kuralları içinde ölçeklendirme böylece (VM örnekleri kaldırma) aynı anda tek bir düğüm yapılır. Aynı anda birden fazla örneği Ölçeklendirmesi güvenli değil.

Kümenizdeki Service Fabric düğüm türleri, oluşur beri sanal makine ölçek ayarlar arka uç, yapabilecekleriniz [otomatik ölçek kuralları ayarlamak veya elle ölçeklendirme](service-fabric-cluster-scale-up-down.md) her düğüm türü/sanal makine ölçek kümesi.

### <a name="programmatic-scaling"></a>Programsal ölçeklendirme
Çoğu senaryoda [el ile veya otomatik ölçeklendirme kurallarını içeren bir küme ölçeklendirme](service-fabric-cluster-scale-up-down.md) iyi çözümdür. Daha Gelişmiş senaryolar için olsa, bunlar sağ uygun olmayabilir. Bu yaklaşım olası dezavantajları şunlardır:

- El ile ölçeklendirme oturum açın ve açıkça ölçeklendirme işlemleri istek gerektirir. Ölçeklendirme işlemlerinin sık veya beklenmeyen zamanlarda gerekirse, bu yaklaşım iyi bir çözüm olmayabilir.
- Otomatik ölçek kuralı sanal makine ölçek kümesindeki bir örneği kaldırdığınızda, düğüm türü Gümüş veya altın dayanıklılık düzeyini olmadıkça bunlar otomatik olarak bilgi o düğümün ilişkili Service Fabric kümesinden kaldırmayın. Otomatik ölçek kuralı düzeyi Ayarla ölçekte (yerine Service Fabric düzeyinde) çalışması nedeniyle otomatik ölçek kuralı düzgün biçimde kapatılıyor olmadan Service Fabric düğümleri kaldırabilirsiniz. Bu utangaç düğüm kaldırma 'hayalet' Service Fabric düğüm durumu ölçek bileşenini işlemlerinden sonra ardınızda. Tek bir (veya hizmet), Service Fabric kümesindeki kaldırılmış düğüm durumu düzenli aralıklarla temizlemeniz gerekir.
- Hiçbir ek temizleme gerektiği şekilde altın veya gümüş dayanıklılık düzeyine sahip bir düğüm türü kaldırılan düğümlerini, otomatik olarak temizlenir.
- Vardır, ancak [birçok ölçümleri](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) otomatik ölçek kuralları tarafından desteklenen, hala olduğu sınırlı bir dizi. Senaryonuz bu kümesinde kapsanmayan bazı ölçüm göre ölçekleme için çağırırsa, otomatik ölçeklendirme kurallarını iyi bir seçenek olmayabilir.

Service Fabric ölçeklendirme nasıl ulaşmalıdır, senaryoya bağlıdır. Ölçeklendirme seyrek ise, ekleme veya düğümleri el ile kaldırma yeteneği büyük olasılıkla yeterli olur. Daha karmaşık senaryolar için otomatik ölçeklendirme kurallarını ve SDK'ları ölçeklendirmenizi program aracılığıyla gösterme güçlü alternatifleri sunar.

Azure API hangi uygulamaların program aracılığıyla sanal makine ile çalışması için ayarlar ölçeklendirme ve Service Fabric kümeleri izin yok. Varolan otomatik ölçeklendirme seçeneği senaryonuz için işe yaramazsa, bu API'leri, özel ölçeklendirme mantığını uygulamak mümkün kılar. 

Yeni bir durum bilgisiz hizmeti ölçeklendirme işlemlerini yönetmek için Service Fabric uygulaması eklemek için otomatik ölçeklendirme 'giriş yapılan' işlevselliği'Bu uygulama için bir yaklaşım ise. Kendi hizmet ölçeklendirme oluşturma en yüksek derecede denetim ve uygulamanızı üzerinden özelleştirilebilirliğini davranışı ölçeklendirme kullanıcının sağlar. Bu, ne zaman veya nasıl bir uygulama veya ölçeklendirir üzerinde kesin denetim gerektiren senaryolar için faydalı olabilir. Ancak, bu denetim bir kod karmaşıklığı kolaylığını ile birlikte gelir. Bu yaklaşımı kullanarak, Önemsiz olmayan olan kendi ölçeklendirme kod, gerektiği anlamına gelir. Hizmetin içinde `RunAsync` yöntemi, bir dizi Tetikleyicileri belirleyebilir ölçeklendirme (küme boyutu üst sınırı gibi parametreleri denetleniyor ve cooldowns ölçeklendirme dahil) gerekli olup olmadığını.   

Sanal makine ölçek kümesi için etkileşimler (her ikisi de geçerli sanal makine örneği sayısını denetlemek ve değiştirmek için) kullanılan API [fluent yönetim Azure işlem kitaplığının](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Fluent işlem kitaplığı, sanal makine ölçek kümeleri ile etkileşmek için kullanımı kolay API sağlar.  Service Fabric kümesi etkileşim kurmak için kullanmak [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Ölçeklendirme kod, ancak ölçeklendirilmesi için kümedeki bir hizmet olarak çalıştırmak gerekmez. Her ikisi de `IAzure` ve `FabricClient` ölçeklendirme hizmeti bir konsol uygulaması veya Windows hizmeti Service Fabric uygulaması dışında çalıştırma kolayca olabilir şekilde ilişkili Azure kaynaklarını uzaktan bağlanabilirsiniz.

Bu sınırlamalar bağlı olarak, size isteyebilirsiniz [uygulama daha özelleştirilmiş otomatik ölçekleme modelleri](service-fabric-cluster-programmatic-scaling.md).


## <a name="scaling-a-standalone-cluster-in-or-out"></a>Bir tek başına küme içeri veya dışarı ölçeklendirme
Tek başına kümeleri Service Fabric kümesi şirket içi dağıtmanıza izin verir veya tercih ettiğiniz bulut sağlayıcısı.  Dağıtımınıza bağlı olarak sanal makineleri veya fiziksel makineler düğüm türleri oluşur. Azure üzerinde çalışan kümelerle karşılaştırıldığında, tek başına küme ölçeklendirme işlemi biraz daha karmaşıktır.  El ile kümedeki düğüm sayısını değiştirmenize ve daha sonra küme yapılandırmasını yükseltme çalıştırın.

Düğümleri kaldırma birden fazla yükseltme başlatabilir. Bazı düğümler işaretlenir `IsSeedNode=”true”` etiketi ve küme sorgulayarak tanımlanabilir kullanarak bildirim [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). Çekirdek düğümleri gibi senaryolarda taşınacak gerekeceğinden bu düğümleri kaldırılmasını diğerlerinden daha uzun sürebilir. Küme en az üç birincil düğüm türü düğümlerinin bulundurmanız gerekir.

Tek başına küme ölçeklendirme, aşağıdaki yönergeleri göz önünde bulundurun:
- Birincil düğüm değiştirme ardına toplu olarak ekleme ve kaldırma yerine gerçekleştirilen bir düğüm olmalıdır.
- Düğüm türü kaldırmadan önce düğümü türüne başvuran tüm düğümleri olup olmadığını denetleyin. Bu düğümler, karşılık gelen düğüm türü kaldırmadan önce kaldırın. Tüm ilgili düğümleri kaldırıldıktan sonra NodeType kümesi yapılandırmasından kaldırmak ve bir yapılandırmaya başlamadan kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Daha fazla bilgi için bkz: [bir tek başına küme ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-an-azure-cluster-up-or-down"></a>Bir Azure küme yukarı veya aşağı Ölçeklendirmesi
Sanal makine ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynaktır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Her düğüm türü sonra ayrı olarak yönetilebilir.  Düğüm türü yukarı veya aşağı Ölçeklendirmesi ölçek kümesindeki sanal makine örnekleri SKU'su değiştirilmesidir. 

> [!WARNING]
> Adresindeki çalışmıyorsa ölçek kümesi/düğüm türünde VM SKU değiştirmemenizi öneririz [Silver dayanıklılık veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM SKU boyutunu değiştirme veri bozucu yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için bazı özelliği olmadan işlemi durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile öngörülemeyen diğer işlemsel sorunlara neden olabilir. 
>

Bir Azure küme ölçeklendirme, aşağıdaki kılavuz göz önünde bulundurun:
- Birincil düğüm türü Ölçeklendirmesi, hiçbir zaman onu birden çok ne ölçeğini [güvenilirlik katmanı](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) sağlar.

Düğüm türü yukarı veya aşağı Ölçeklendirmesi birincil olmayan veya birincil düğüm türü olup bağlı olarak farklı işlemidir.

### <a name="scaling-non-primary-node-types"></a>Birincil olmayan düğüm türleri ölçeklendirme
Yeni düğüm türü ihtiyacınız olan kaynakları ile oluşturun.  Yeni düğüm türü eklenecek Hizmetleri çalışan kısıtlamalarından güncelleştirin.  Kademeli olarak (bir kerede), eski düğüm türü örnek sayısı örnek sayısını sıfır olarak azaltıp küme güvenilirliğini etkilenmez.  Eski düğüm türü decommisioned olduğu gibi hizmetleri aşamalı olarak yeni düğüm türü geçirin.

### <a name="scaling-the-primary-node-type"></a>Birincil düğüm türü ölçeklendirme
Birincil düğüm türünde VM SKU değiştirmemenizi öneririz. Daha fazla küme kapasitesine ihtiyacınız varsa, daha fazla örnekleri ekleme öneririz. 

Varsa mümkün değildir, yeni bir kümesi oluşturabileceğiniz ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa), eski kümeden. Herhangi bir sistem hizmet durumu geri yükleme gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda yeniden oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların, küme üzerinde çalışan uygulamalarınızı yeni kümeye dağıtabilir, geri yüklenecek bir şey vardır. Desteklenmeyen rota gidin ve VM SKU değiştirmek istediğiniz karar verirseniz, sanal makine ölçek sonra belgelenir modeli tanımını yeni SKU yansıtacak şekilde ayarlayın. Yalnızca bir düğüm türü kümeniz varsa, daha sonra durum bilgisi olan uygulamaların tümüne yanıt emin olun [hizmet çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-lifecycle.md) (derleme yinelemede kalmış gibi) zamanında ve hizmet çoğaltma yeniden oluşturma süresi beş dakikadan daha kısa bir süre (için Gümüş dayanıklılık düzeyi) olur. 

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Bir Azure bir küme veya ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Bir Azure küme programlı olarak ölçekleme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Giriş veya çıkış standaone küme ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

