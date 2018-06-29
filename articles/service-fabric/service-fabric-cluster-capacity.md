---
title: Service Fabric kümesi kapasite planlaması | Microsoft Docs
description: Service Fabric kümesi kapasite planlama konuları. Nodetypes, işlemleri, dayanıklılık ve güvenilirlik katmanları
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: chackdan
ms.openlocfilehash: aca03452ff5655d3a7180009f42df14c9459a9ff
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061567"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric kümesi kapasite planlama konuları
Her üretim dağıtımı için kapasite planlamasının önemli bir adımdır. Bu işlemin bir parçası olarak göz önünde bulundurmanız gereken öğelerin bazıları aşağıda verilmiştir.

* Düğüm sayısı ile başlatmak için küme gereksinimlerinizi türleri
* Her bir düğüm türü (boyutu, birincil, internet'e yönelik, sanal makineleri, vb. sayısı) özellikleri
* Kümenin güvenilirlik ve dayanıklılık özellikleri

> [!NOTE]
> En düşük düzeyde tüm gözden geçirmeniz gereken **izin** planlama sırasında ilke değerleri yükseltin. Değerleri uygun şekilde ayarlanmış sağlamak ve kümenizi daha sonra değiştirilemez sistemi yapılandırma ayarları nedeniyle yazma azaltmak için budur. 
> 

Bize kısaca bu öğelerin her birini gözden geçirin.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Düğüm sayısı ile başlatmak için küme gereksinimlerinizi türleri
İlk olarak, oluşturmakta olduğunuz küme için kullanılacak neler olduğunu şekil gerekir.  Hangi tür uygulamaların bu kümesine dağıtmayı planlıyor musunuz? Küme amacı açık değilse, büyük olasılıkla olmayan henüz kapasite planlama işlemi girmek için hazır.

Kümenizi ile başlatmak için gereken düğüm türleri sayısı kurun.  Her düğüm türü için bir sanal makine ölçek kümesi eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Bu nedenle düğüm türleri sayısı karar temelde aşağıdaki konuları için gelir:

* Uygulamanız birden çok hizmetlere sahip olmadığı ve herhangi biri ortak veya İnternete olması gerekiyor mu? Tipik uygulamalar, bir istemciden giriş aldığı bir ön uç ağ geçidi hizmeti ve iletişim kuran bir veya daha fazla arka uç hizmetleriyle ön uç hizmetleri içerir. Bu nedenle bu durumda, en az iki düğüm türleri sahip sonlandırın.
* (Uygulamanızı kurma), hizmetler, daha fazla RAM veya daha yüksek CPU döngüsü gibi farklı altyapı gereksinimleri var mı? Örneğin, dağıtmak istediğiniz uygulamayı bir ön uç hizmeti ve arka uç hizmeti içeren bize varsayın. Ön uç hizmeti bağlantı noktalarının Internet'e açık olması daha küçük vm'lerde (örneğin, D2 VM boyutları) çalıştırabilirsiniz.  Arka uç hizmetine hesaplama yoğun ve Internet olmayan daha büyük sanal makineler (VM boyutları D4, D6, D15 gibi ile) çalışması gerekiyor ancak karşılıklı.
  
  Bir düğüm türünde, tüm hizmetler almaya karar verebilirsiniz rağmen bu örnekte, bunları bir kümede iki düğüm türleriyle yerleştirmenizi öneririz.  Bu, her bir düğüm türünde VM boyutu veya internet bağlantısı gibi farklı özelliklere sahip sağlar. VM sayısını Genişletilebilir bağımsız olarak, de.  
* Gelecek tahmin etmek için bildiğiniz bulguları ile gidin ve uygulamalarınızı başlaması gereken düğüm türleri sayısını seçin. Her zaman ekleyebilir veya düğüm türleri daha sonra kaldırabilirsiniz. Service Fabric kümesi en az bir düğüm türü olmalıdır.

## <a name="the-properties-of-each-node-type"></a>Her düğüm türünün özelliklerini
**Düğüm türü** bulut Hizmetleri roller olarak görülebilir. Düğüm türleri, VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlar. Service Fabric kümesi içinde tanımlanan her düğüm türü eşlendiğini bir [sanal makine ölçek kümesi](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Her düğüm ayrı ölçeği ayarlamak ve ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktalarının açık yoksa ve farklı kapasite ölçümlerini türüdür. Düğüm türleri ve sanal makine ölçek kümeleri arasındaki ilişkileri hakkında daha fazla bilgi için örneklerden birini rdp'ye yeni açmak nasıl bağlantı noktaları ve nasıl vb. bkz [Service Fabric kümesi düğüm türleri](service-fabric-cluster-nodetypes.md).

Service Fabric kümesi birden fazla düğüm türü oluşabilir. Bu olay tek bir birincil düğüm türü küme oluşur ve bir veya daha fazla birincil olmayan düğüm türleri.

Tek düğüm türü yalnızca 100 sanal makine ölçek kümesi başına en fazla düğüm aşamaz. Sanal makine ölçek ayarlar hedeflenen ölçeği elde etmek için ve otomatik ölçeklendirme olamaz automagically eklemeniz gerekebilir sanal makine ölçek kümeleri ekleyin. Sanal makine ölçek yerinde ayarlar ekleme dinamik bir kümeye bir görevdir ve yaygın olarak bu yeni küme oluşturma sırasında sağlanan uygun düğümü türleriyle sağlama kullanıcılar sonuçlanır. 

### <a name="primary-node-type"></a>Birincil düğüm türü

Service Fabric Sistem Hizmetleri (örneğin, Küme Yöneticisi hizmeti veya görüntü Deposu hizmetini) birincil düğüm türünde yerleştirilir. 

![İki düğüm türleri olan kümesinin ekran görüntüsü][SystemServices]

* **VM'ler en küçük boyut** için birincil düğüm türü tarafından belirlenen **dayanıklılık katmanı** seçtiğiniz. Varsayılan dayanıklılık katmanı Bronz ' dir. Bkz: [küme dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) daha fazla ayrıntı için.  
* **VM'ler en az sayıda** için birincil düğüm türü tarafından belirlenen **güvenilirlik katmanı** seçtiğiniz. Varsayılan güvenilirlik katmanı Gümüş ' dir. Bkz: [güvenilirliği kümenin](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) daha fazla ayrıntı için.  

Azure Resource Manager şablonu, birincil düğüm türü ile yapılandırılmış `isPrimary` altında öznitelik [düğüm türü tanımı](https://docs.microsoft.com/en-us/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Olmayan birincil düğüm türü

Birden çok düğüm türleri ile bir kümede, rest birincil olmayan ve bir birincil düğüm türü yoktur.

* **VM'ler en küçük boyut** için birincil olmayan düğüm türleri tarafından belirlenir **dayanıklılık katmanı** seçtiğiniz. Varsayılan dayanıklılık katmanı Bronz ' dir. Bkz: [küme dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) daha fazla ayrıntı için.  
* **VM'ler en az sayıda** birincil olmayan düğüm türleri için biridir. Ancak, bu düğüm türü çalıştırmak istediğiniz uygulama/hizmetleri çoğaltmalarının sayısına dayalı bu numarayı seçmeniz gerekir. Küme dağıttıktan sonra bir düğüm türünde VM sayısını artırılabilir.

## <a name="the-durability-characteristics-of-the-cluster"></a>Küme dayanıklılık özellikleri
Dayanıklılık katmanı ile Azure altyapının Vm'leriniz sahip ayrıcalıkları sisteme göstermek için kullanılır. Birincil düğüm türü bu ayrıcalık sistem hizmetlerini ve durum bilgisi olan hizmetleriniz için çekirdek gereksinimleri etkileyen herhangi VM düzeyinde altyapı istek (örneğin, bir VM yeniden başlatma, VM yeniden görüntü oluşturma veya VM geçiş) duraklatmak Service Fabric verir. Birincil olmayan düğüm türleri'nde, bu ayrıcalık, durum bilgisi olan hizmetleri için çekirdek gereksinimleri etkileyen herhangi VM düzeyinde altyapı istekleri (örneğin, VM yeniden başlatma, VM yeniden görüntü oluşturma ve VM geçiş) duraklatmak Service Fabric verir.

| Dayanıklılık katmanı  | Gerekli en az sayıda sanal makineleri | Desteklenen VM SKU'ları                                                                  | VMSS yaptığınız güncelleştirmeler                               | Güncelleştirmeleri ve Azure tarafından başlatılan Bakımı                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Altın             | 5                              | Tek bir müşteri (örneğin, L32s GS5, G5, DS15_v2, D15_v2) ayrılmış tam düğümlü SKU'ları | Service Fabric kümesi tarafından onaylanan kaldıktan sonra | Önceki arızalardan kurtarmak çoğaltmalar için ek süre izin vermek için UD başına 2 saat için duraklatıldı |
| Gümüş           | 5                              | Sanal makineleri tek çekirdek veya üzeri                                                        | Service Fabric kümesi tarafından onaylanan kaldıktan sonra | Herhangi önemli bir süre ertelendi                                                    |
| Bronz           | 1                              | Tümü                                                                                | Service Fabric kümesi tarafından Gecikmeli değil           | Herhangi önemli bir süre ertelendi                                                    |

> [!WARNING]
> Düğüm türleri ile Bronz dayanıklılık çalıştıran elde _ayrıcalıkların_. Bu, durum bilgisiz iş yükleri etkisi altyapı işleri değil durduruldu veya kaldırılacak Gecikmeli, hangi iş yüklerinizi etkileyebilecek anlamına gelir. Yalnızca Bronz yalnızca durum bilgisiz iş yükleri çalıştıran düğüm türleri için kullanın. Üretim iş yükleri için Gümüş çalıştıran veya yukarıdaki önerilir. 
>

**Gümüş veya altın dayanıklılık düzeyleri kullanmanın yararları**
 
- Bir ölçek işleminde gerekli adımları sayısını azaltan (diğer bir deyişle, düğümü devre dışı bırakma ve Kaldır-ServiceFabricNodeState denir otomatik olarak).
- Bir müşteri tarafından başlatılan yerinde VM SKU değiştirme işlemi ya da Azure altyapı işlemleri nedeniyle veri kaybı riskini azaltır.

**Dezavantajlarını Gümüş veya altın dayanıklılık düzeyleri**
 
- Dağıtımları için sanal makine ölçek ayarlayın ve diğer ilgili Azure kaynaklarını Gecikmeli, zaman aşımına uğrayabilir veya tamamen sorunları kümenizdeki veya altyapı düzeyinde tarafından engellendi. 
- Sayısı artar [çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-lifecycle.md) (örneğin, birincil takasları) nedeniyle Azure altyapı işlemleri sırasında düğüm deactivations otomatik.
- Azure platformu yazılım güncelleştirmeleri veya donanım bakım etkinlikleri yaşanan süreler için hizmet dışına düğümleri alır. Bu etkinlikler sırasında düğüm durumu devre dışı bırakılması/devre dışı olan görebilirsiniz. Bu, kümenizi kapasitesini geçici olarak azaltır, ancak küme veya uygulamaların kullanılabilirliğini etkileyen değil.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Gümüş veya altın dayanıklılık düzeyleri kullanıldığı durumlar için öneriler

Durum bilgisi olan hizmetleri beklediğiniz ölçek bileşenini barındıran tüm düğüm türleri için Gümüş veya altın dayanıklılık kullanın (VM örneği sayısını azaltmak) sık sık ve dağıtım işlemlerini Gecikmeli tercih ediyorsanız ve bu ölçek bileşenini basitleştirme lehinde azaltılması için kapasite işlemler. (VM örnekleri ekleme) genişleme senaryoları dayanıklılık katmanı tercih ettiğiniz yürütülmez, yalnızca ölçek içinde değil.

### <a name="changing-durability-levels"></a>Dayanıklılık düzeylerini değiştirme
- Gümüş veya altın dayanıklılık düzeylerine sahip düğüm türleri için Bronz indirgenemez.
- Bronz Gümüş veya altın yükseltmek birkaç saat sürebilir.
- Dayanıklılık düzeyi değiştirirken, hem Service Fabric uzantısı yapılandırmasında, sanal makine ölçek kümesi kaynağı ve Service Fabric küme kaynağı düğümü tür tanımında güncelleştirdiğinizden emin olun. Bu değerlerin eşleşmesi gerekir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Düğümü için işletimsel öneriler, Gümüş veya altın dayanıklılık düzeyi ayarlamış olduğunuz yazın.

- Küme ve uygulamalar her zaman sağlıklı tutmak ve uygulamaların tümüne yanıt emin olun [hizmet çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-lifecycle.md) (derleme yinelemede kalmış gibi) zamanında.
- (Ölçek yukarı/aşağı) VM SKU değişiklik yapmak için daha güvenli şekilde benimsemeyi: bir sanal makine ölçek kümesi VM SKU'su değiştirme doğası gereği güvenli olmayan bir işlemdir ve bu nedenle, mümkünse kaçınılmalıdır. Sık karşılaşılan sorunları önlemek için izleyebileceğiniz işlem şöyledir.
    - **Birincil olmayan düğüm türleri için:** yeni sanal makine ölçek kümesi oluşturmanız önerilir, yeni sanal makine ölçek kümesi/düğüm türü eklemek ve eski sanal makine ölçek kümesi örnek azaltmak için hizmet yerleşimi kısıtlamasını değiştirme 0 olarak (düğümleri kaldırılmasını küme güvenilirliğini etkilemeyen emin olmak için budur) bir seferde bir düğüm sayısı.
    - **Birincil düğüm türü için:** birincil düğüm türünde VM SKU değiştirmeyin bizim önerilir. SKU desteklenmiyor birincil düğüm türü değiştirme. Yeni SKU kapasitesi nedeni, daha fazla örnekleri ekleme öneririz. Bu, mümkün değildir, yeni bir küme oluşturun ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa), eski kümeden. Herhangi bir sistem hizmet durumu geri yükleme gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda yeniden oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların, küme üzerinde çalışan uygulamalarınızı yeni kümeye dağıtabilir, geri yüklenecek bir şey vardır. Desteklenmeyen rota gidin ve VM SKU değiştirmek istediğiniz karar verirseniz, sanal makine ölçek sonra belgelenir modeli tanımını yeni SKU yansıtacak şekilde ayarlayın. Yalnızca bir düğüm türü kümeniz varsa, daha sonra durum bilgisi olan uygulamaların tümüne yanıt emin olun [hizmet çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-lifecycle.md) (derleme yinelemede kalmış gibi) zamanında ve hizmet çoğaltma yeniden oluşturma süresi beş dakikadan daha kısa bir süre (için Gümüş dayanıklılık düzeyi) olur. 

    > [!WARNING]
    > En az Gümüş dayanıklılık çalıştırmayan önerilen sanal makine ölçek kümesi VM SKU boyutunu değiştirme. VM SKU boyutunu değiştirme veri bozucu yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için en az bazı özelliği olmadan işlemi durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile öngörülemeyen diğer işlemsel sorunlara neden olabilir. 
    > 
    
- Beş düğümleri altın dayanıklılık düzeyine sahip herhangi bir sanal makine ölçek kümesi için en düşük sayısı korumak veya gümüş etkinleştirilmiş.
- Her VM ölçeği Gümüş veya altın dayanıklılık düzeyinde Ayarla Service Fabric kümesi içinde kendi düğüm türüne eşlemeniz gerekir. Birden çok VM eşleme tek düğüm türü için ölçek kümeleri düzgün çalışmasını Service Fabric kümesi Azure altyapısı arasında eşgüdüme engeller.
- Rastgele VM örneklerini silmek değil, her zaman sanal makine ölçek kümesi ölçek özelliği tuşunu kullanın. Rastgele VM örnekleri silinmesini UD ve FD üzerinden yayılan VM örneğinde dengede değil oluşturma bir olasılığı vardır. Bu dengesizliği sistemleri düzgün bir şekilde yük dengelemesi hizmet örnekleri/hizmet çoğaltmalar arasında olanağı olumsuz yönde etkileyebilir.
- (VM örneklerini kaldırma), Ölçek gerçekleştirilir gibi yalnızca bir düğümün aynı anda otomatik ölçeklendirme kullanıyorsanız, ardından kuralları ayarlayın. Aynı anda birden fazla örneği Ölçeklendirmesi güvenli değil.
- Silme veya birincil düğüm türünde VM serbest bırakma, hiçbir zaman hangi güvenilirlik katmanını gerektirdiğini aşağıda ayrılmış VM'ler sayısını azaltmanız gerekir. Bu işlemler, Gümüş veya altın dayanıklılık düzeyi ile ayarlanmış bir ölçek süresiz olarak engellenir.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Kümenin güvenilirliği
Güvenilirlik katmanı çoğaltmaları birincil düğüm türünde bu kümede çalıştırmak istediğiniz sistem hizmetlerinin sayısını ayarlamak için kullanılır. Daha fazla sayıda yineleme, daha güvenilir sistem kümenizdeki hizmetleridir.  

Güvenilirlik katmanı aşağıdaki değerleri alabilir:

* Sistem Hizmetleri Çalıştır platinum - sahip bir hedef çoğaltma dokuz sayısını ayarlayın.
* Sistem Hizmetleri Çalıştır altın - sahip bir hedef çoğaltma yedi sayısını ayarlayın.
* Sistem Hizmetleri Çalıştır Gümüş - sahip bir hedef çoğaltma beş sayısını ayarlayın. 
* Bronz - sistem hizmetlerini çalıştıran bir hedef ile çoğaltma üç sayısını ayarlayın.

> [!NOTE]
> Seçtiğiniz güvenilirlik katmanı birincil düğüm türünüz olmalıdır düğüm sayısı alt sınırı belirler. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Güvenilirlik katmanı için öneriler

Artırma ya da (tüm düğüm türleri VM örnekleri toplamı) kümenizin boyutunu azaltma, kümenizi bir katmanından diğerine güvenilirliğini güncelleştirmeniz gerekir. Bunun yapılması, Küme yükseltme sayısını ayarlamak sistem hizmetleri çoğaltma değiştirmek için tetikler. Yükseltme ilerleme kümesine düğüm ekleme gibi diğer herhangi bir değişiklik yapmadan önce tamamlanması için bekleyin.  Service Fabric Explorer veya çalıştırarak yükseltmesinin ilerleme durumunu izleyebilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Güvenilirlik katmanı seçme özelliği üzerinde öneri aşağıda verilmiştir.

| **Küme boyutu** | **Güvenilirlik katmanı** |
| --- | --- |
| 1 |Güvenilirlik katmanı parametresi belirtmeyin sistem bu hesaplar |
| 3 |Bronz |
| 5 veya 6|Gümüş |
| 7 veya 8 |Altın |
| 9 ve sonraki sürümü |Platinum |

## <a name="primary-node-type---capacity-guidance"></a>Birincil düğüm türü - Kapasite Kılavuzu

Birincil düğüm türü kapasite planlama Kılavuzu şöyledir:

- **Azure üzerinde herhangi bir üretim iş yükünü çalıştırmak için VM örneği sayısı:** 5 en az bir birincil düğüm türü boyutunu belirtmeniz gerekir. 
- **Azure'da test iş yüklerini çalıştırmak için VM örneklerinin sayısını** minimum birincil düğüm türü boyutu 1 veya 3 belirtebilirsiniz. Tek düğümlü bir küme, özel bir yapılandırma ve bunu ile çalışır, bu küme dışında ölçek desteklenmiyor. Tek düğümlü bir küme, güvenilirlik ve bu nedenle, Resource Manager şablonu, sahip olduğunuz Kaldır/değil Bu yapılandırmayı belirtmek (yapılandırma değeri ayarı olmayan değil yeterli). Portal ayarlanan bir düğümün küme ayarladıysanız, sonra yapılandırma otomatik olarak dikkate. Bir ve üç düğümlü kümeler, üretim iş yükleri çalıştırmak için desteklenmez. 
- **VM SKU:** birincil düğüm türü olduğundan Sistem Hizmetleri çalıştırdığı, onun için gereken genel yoğun dikkate al yük, seçtiğiniz VM SKU planlama kümesine yerleştirilecek. I burada anlamı göstermeye-birincil düğüm türü, "Lungs", beyin oxygen sağladıkları olduğunu düşündüğünüz benzetme işte ve bu nedenle, gövde beyin yeterli oxygen almazsa yükselmesine. 

Bir küme kapasite gereksinimlerini belirlenir olduğundan kümede çalıştırmayı planladığınız iş yüküne göre biz, ancak İşte yardımcı olmak için geniş Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sağlayamıyor

Üretim iş yükleri için: 

- Standart D3 veya standart D3_V2 veya eşdeğer bir en az yerel SSD 14 GB ile önerilen VM SKU değerdir.
- Minimum desteklenen VM SKU standart D1 veya standart D1_V2 veya eşdeğer bir en az yerel SSD 14 GB ile kullanılır. 
- Kısmi çekirdek standart A0 gibi VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU performans nedenleriyle üretim iş yükleri için desteklenmez.

> [!WARNING]
> Şu anda çalışan bir kümede VM SKU boyutunu birincil düğüm değiştirilmesi desteklenmiyor. Bu nedenle birincil düğüm türü VM SKU dikkatlice kapasite gelecekteki gereksinimlerinizi dikkate alarak seçin. Şu anda birincil düğüm türünüz için yeni bir VM SKU (küçük veya büyük) sağ kapasiteye sahip yeni bir küme oluşturmak için geçileceğini desteklenen tek yol dağıtmak, uygulamalarınız ve (varsa) uygulama durumunu geri yüklemek için gelen [ yedeklemeleri'en son hizmet](service-fabric-reliable-services-backup-restore.md) eski kümeden gerçekleştirmişsiniz. Herhangi bir sistem hizmet durumu geri yükleme gerekmez, yeni küme uygulama dağıtırken yeniden oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların, küme üzerinde çalışan uygulamalarınızı yeni kümeye dağıtabilir, geri yüklenecek bir şey vardır.
> 

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Olmayan birincil düğüm türü - durum bilgisi olan iş yükleri için kapasite Kılavuzu

Service fabric kullanarak durum bilgisi olan iş yükleri için bu kılavuzu olduğundan [güvenilir koleksiyonları veya reliable Actors](service-fabric-choose-framework.md) birincil olmayan düğüm türünde çalışan.

**VM örneği sayısı:** durum bilgisi olan üretim iş yükleri için en az ve hedef çoğaltma sayısı 5 ile çalıştırmanızı önerilir. Bu, kararlı durumda, her bir hata etki alanı ve yükseltme etki alanı'nda (çoğaltma kümesinden) çoğaltma şunun olduğunu anlamına gelir. Birincil düğüm türü için tüm güvenilirlik katmanı kavram, sistem hizmetleri için bu ayarı belirtmek için bir yoldur. Bu nedenle aynı göz önünde bulundurarak, durum bilgisi olan hizmetler için geçerlidir.

Durum bilgisi olan iş yükleri içinde çalıştırıyorsanız, bu nedenle üretim iş yükleri için minimum önerilen olmayan birincil düğüm türü 5, boyutudur.

**VM SKU:** VM SKU onun için seçtiğiniz planladığınız her düğüm yerleştirilecek yoğun yük dikkate gerekir böylece bu düğüm uygulama hizmetlerinizi burada çalıştırıyorsanız, türüdür. Nodetype kapasite ihtiyaçlarını, biz, ancak İşte yardımcı olmak için geniş Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sağlayamaz böylece kümedeki çalıştırmayı planladığınız iş yükü tarafından belirlenir

Üretim iş yükleri için 

- Standart D3 veya standart D3_V2 veya eşdeğer bir en az yerel SSD 14 GB ile önerilen VM SKU değerdir.
- Minimum desteklenen VM SKU standart D1 veya standart D1_V2 veya eşdeğer bir en az yerel SSD 14 GB ile kullanılır. 
- Kısmi çekirdek standart A0 gibi VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU üretim iş yükleri için performansı artırmak için özellikle olmayan desteklenmiyor.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Olmayan birincil düğüm türü - durum bilgisiz iş yükleri için kapasite Kılavuzu

Bu kılavuz, birincil olmayan nodetype üzerinde çalışan durum bilgisiz iş yükleri.

**VM örneği sayısı:** durum bilgisiz üretim iş yükleri için en düşük desteklenen olmayan birincil düğüm türü boyutu 2'dir. Bu, uygulamanızı ve hizmetinizi vererek VM örneği kaybı varlığını sürdürmesi için iki durum bilgisiz örneklerini çalıştırmanıza olanak sağlar. 

**VM SKU:** VM SKU onun için seçtiğiniz planladığınız her düğüm yerleştirilecek yoğun yük dikkate gerekir böylece bu düğüm uygulama hizmetlerinizi burada çalıştırıyorsanız, türüdür. Düğüm türü Kapasite ihtiyaçlarını, biz, ancak İşte yardımcı olmak için geniş Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sağlayamaz böylece kümedeki çalıştırmayı planladığınız iş yükü tarafından belirlenir

Üretim iş yükleri için 

- Standart D3 veya standart D3_V2 veya eşdeğeri önerilen VM SKU değerdir. 
- Minimum desteklenen VM SKU standart D1 veya standart D1_V2 veya eşdeğer kullanılır. 
- Kısmi çekirdek standart A0 gibi VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU performans nedenleriyle üretim iş yükleri için desteklenmez.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Kapasite planlama tamamlamak ve küme ayarlama sonra aşağıdaki okuyun:

* [Service Fabric kümesi güvenliği](service-fabric-cluster-security.md)
* [Service Fabric küme ölçeklendirme](service-fabric-cluster-scaling.md)
* [Olağanüstü Durum Kurtarma planlaması](service-fabric-disaster-recovery.md)
* [Sanal makine ölçek Nodetypes ilişkisini ayarlayın](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
