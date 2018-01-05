---
title: "Service Fabric kümesi kapasite planlaması | Microsoft Docs"
description: "Service Fabric kümesi kapasite planlama konuları. Nodetypes, işlemleri, dayanıklılık ve güvenilirlik katmanları"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: chackdan
ms.openlocfilehash: 8e2fceaf7e8a0d6c177d3122bd07de5b8c11f295
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric kümesi kapasite planlama konuları
Her üretim dağıtımı için kapasite planlamasının önemli bir adımdır. Bu işlemin bir parçası olarak göz önünde bulundurmanız gereken öğelerin bazıları aşağıda verilmiştir.

* Düğüm sayısı ile başlatmak için küme gereksinimlerinizi türleri
* Her bir düğüm türü (boyutu, birincil, internet'e yönelik, sanal makineleri, vb. sayısı) özellikleri
* Küme güvenilirlik ve dayanıklılık özellikleri

Bize kısaca bu öğelerin her birini gözden geçirin.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Düğüm sayısı ile başlatmak için küme gereksinimlerinizi türleri
İlk olarak, oluşturmakta olduğunuz küme için kullanılacak neler olduğunu ve bu kümesine dağıtmayı planlayan ne tür uygulamaları kullanıma şekil gerekir. Küme amacı açık değilse, büyük olasılıkla olmayan henüz kapasite planlama işlemi girmek için hazır.

Kümenizi ile başlatmak için gereken düğüm türleri sayısı kurun.  Her düğüm türü, bir sanal makine ölçek kümesi için eşlenir. Her düğüm türü sonra ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini olabilir. Bu nedenle düğüm türleri sayısı karar temelde aşağıdaki konuları için gelir:

* Uygulamanız birden çok hizmetlere sahip olmadığı ve herhangi biri ortak veya İnternete olması gerekiyor mu? Tipik uygulamalar, bir istemciden giriş aldığı bir ön uç ağ geçidi hizmeti ve iletişim kuran bir veya daha fazla arka uç hizmetleriyle ön uç hizmetleri içerir. Bu nedenle bu durumda, en az iki düğüm türleri sahip sonlandırın.
* (Uygulamanızı kurma), hizmetler, daha fazla RAM veya daha yüksek CPU döngüsü gibi farklı altyapı gereksinimleri var mı? Örneğin, dağıtmak istediğiniz uygulamayı bir ön uç hizmeti ve arka uç hizmeti içeren bize varsayın. Ön uç hizmeti bağlantı noktalarının Internet'e açık olması daha küçük vm'lerde (örneğin, D2 VM boyutları) çalıştırabilirsiniz.  Arka uç hizmetine hesaplama yoğun ve Internet olmayan daha büyük sanal makineler (VM boyutları D4, D6, D15 gibi ile) çalışması gerekiyor ancak karşılıklı.
  
  Bir düğüm türünde, tüm hizmetler almaya karar verebilirsiniz rağmen bu örnekte, bunları bir kümede iki düğüm türleriyle yerleştirmenizi öneririz.  Bu, her bir düğüm türünde VM boyutu veya internet bağlantısı gibi farklı özelliklere sahip sağlar. VM sayısını Genişletilebilir bağımsız olarak, de.  
* Gelecek tahmin edilemez olduğundan, bildiğiniz bulguları ile gidin ve uygulamalarınızı başlaması gereken düğüm türleri sayısına karar verin. Her zaman ekleyebilir veya düğüm türleri daha sonra kaldırabilirsiniz. Service Fabric kümesi en az bir düğüm türü olmalıdır.

## <a name="the-properties-of-each-node-type"></a>Her düğüm türünün özelliklerini
**Düğüm türü** bulut Hizmetleri roller olarak görülebilir. Düğüm türleri, VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlar. Service Fabric kümesi içinde tanımlanan her düğüm türü ayrı sanal makine ölçek kümesi ayarlanır. Sanal makine ölçek kümesini dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynaktır. Farklı sanal makine ölçek kümesi tanımlanan, her düğüm türü sonra ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini olabilir.

Okuma [bu belgeyi](service-fabric-cluster-nodetypes.md) Nodetypes ilişki için sanal makine ölçek kümesi hakkında daha fazla ayrıntı için nasıl RDP tek bir örnek için yeni bağlantı noktalarını vb. açın.

Kümenizi birden fazla düğüm türüne sahip olabilir, ancak birincil düğüm türü (portalda tanımladığınız ilk bir) üretim iş yükleri için kullanılan küme için en az beş VM'ler (veya test kümeleri için en az üç sanal makineleri) olması gerekir. Resource Manager şablonu kullanarak küme oluşturuyorsanız, ardından Ara **birincil** düğüm türü tanımı altında özniteliği. Birincil düğüm türü düğümü Service Fabric Sistem Hizmetleri yerleştirildiği türüdür.  

### <a name="primary-node-type"></a>Birincil düğüm türü
Birden çok düğüm türleri ile bir küme için bunları birincil olarak birini seçmeniz gerekir. Birincil düğüm türü özellikleri şunlardır:

* **VM'ler en küçük boyut** için birincil düğüm türü tarafından belirlenen **dayanıklılık katmanı** seçtiğiniz. Bronz dayanıklılık katmanı için varsayılandır. Dayanıklılık katmanı nedir ve bunu sürebilir değerleri hakkında ayrıntılar için aşağı kaydırın.  
* **VM'ler en az sayıda** için birincil düğüm türü tarafından belirlenen **güvenilirlik katmanı** seçtiğiniz. Güvenilirlik katmanı için Gümüş varsayılandır. Güvenilirlik katmanı nedir ve bunu sürebilir değerleri hakkında ayrıntılar için aşağı kaydırın. 


* Service Fabric Sistem Hizmetleri (örneğin, Küme Yöneticisi hizmeti veya görüntü Deposu hizmetini) birincil düğüm türünde yerleştirilir ve güvenilirlik ve dayanıklılık kümesinin belirlenir şekilde güvenilirlik katmanı değeri ve dayanıklılık katmanı tarafından değeri birincil düğüm türü seçin.

![İki düğüm türleri olan kümesinin ekran görüntüsü ][SystemServices]

### <a name="non-primary-node-type"></a>Olmayan birincil düğüm türü
Bir küme ile birden çok düğüm türü için bir birincil düğüm türü ve bunları geri kalanı birincil olmayan. Birincil olmayan düğüm türü özellikleri şunlardır:

* Sanal makineleri minimum boyut bu düğüm türü için seçtiğiniz dayanıklılık katmanı tarafından belirlenir. Bronz dayanıklılık katmanı için varsayılandır. Dayanıklılık katmanı nedir ve bunu sürebilir değerleri hakkında ayrıntılar için aşağı kaydırın.  
* Bu düğüm türü için en az sayıda sanal makineleri olabilir. Ancak bu düğüm türü çalıştırmak istediğiniz uygulama/hizmetleri çoğaltmalarının sayısına dayalı bu numarayı seçmeniz gerekir. Küme dağıttıktan sonra bir düğüm türünde VM sayısını artırılabilir.

## <a name="the-durability-characteristics-of-the-cluster"></a>Küme dayanıklılık özellikleri
Dayanıklılık katmanı ile Azure altyapının Vm'leriniz sahip ayrıcalıkları sisteme göstermek için kullanılır. Birincil düğüm türü bu ayrıcalık sistem hizmetlerini ve durum bilgisi olan hizmetleriniz için çekirdek gereksinimleri etkileyen herhangi VM düzeyinde altyapı istek (örneğin, bir VM yeniden başlatma, VM yeniden görüntü oluşturma veya VM geçiş) duraklatmak Service Fabric verir. Birincil olmayan düğüm türleri'nde, bu ayrıcalık içinde çalışan, durum bilgisi olan hizmetler için çekirdek gereksinimleri etkileyen herhangi VM düzeyinde altyapı istekleri VM yeniden başlatma, VM yeniden görüntü oluşturma, VM geçiş vb. gibi duraklatmak Service Fabric verir.

Bu ayrıcalık, aşağıdaki değerleri ifade edilir:

* Altın - altyapı işleri UD başına iki saatlik bir süre duraklatılabilir. Altın dayanıklılık yalnızca tam düğüm L32s, GS5, G5, DS15_v2, D15_v2 (genel olarak tüm VM boyutları 'Örneği notta tek bir müşteriye ayrılmış donanım için ayrılmış olarak' işaretlenen http://aka.ms/vmspecs listelenmiş vb. gibi VM SKU'ları üzerinde etkin Tam düğümü VM'ler)
* Gümüş - altyapı işleri UD başına 10 dakikalık bir süre duraklatıldı ve tüm standart vm'lerde tek çekirdek ve yukarıdaki kullanılabilir.
* Bronz - ayrıcalıkların. Varsayılan değer budur. Yalnızca bu dayanıklılık düzeyi düğüm türleri için Çalıştır kullanın _yalnızca_ durum bilgisiz iş yükleri. 

> [!WARNING]
> Bronz dayanıklılık çalıştıran NodeTypes elde _ayrıcalıkların_. Bu, durum bilgisiz iş yükleri etkisi altyapı işleri durduruldu gecikmeli ya olduğunu anlamına gelir. Bu tür işleri kapalı kalma süresi ve diğer sorunlar neden yüklerinizi hala etkileyebilir mümkündür. Her tür üretim iş yükü için en az çalışan Gümüş önerilir. 5 düğümleri dayanıklılık altın veya gümüş sahip tüm düğüm-türü için minimum sayısını bulundurmanız gerekir. 
> 

Her düğüm türleri için dayanıklılık düzeyini seçin alın. Gümüş ve diğer Bronz aynı kümedeki sahip veya altın bir dayanıklılık düzeyine sahip olmak için bir düğüm-türü seçebilirsiniz. **5 düğümleri dayanıklılık altın veya gümüş sahip tüm düğüm-türü için minimum sayısını korumalıdır**. 

**Gümüş veya altın dayanıklılık düzeyleri kullanmanın yararları**
 
1. Bir ölçek işleminde gerekli adımları sayısını azaltan (diğer bir deyişle, düğümü devre dışı bırakma ve Kaldır-ServiceFabricNodeState denir otomatik olarak)
2. Bir müşteri tarafından başlatılan yerinde VM SKU değiştirme işlemi ya da Azure altyapı işlemleri nedeniyle veri kaybı riskini azaltır.
     
**Dezavantajlarını Gümüş veya altın dayanıklılık düzeyleri**
 
1. Sanal makine ölçek kümesi ve diğer ilgili Azure kaynaklarını dağıtımlar) Gecikmeli, zaman aşımına ya da tamamen sorunları kümenizdeki veya altyapı düzeyinde tarafından engellendi. 
2. Sayısı artar [çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (örneğin, birincil takasları) nedeniyle Azure altyapı işlemleri sırasında düğüm deactivations otomatik.

### <a name="recommendations-on-when-to-use-silver-or-gold-durability-levels"></a>Zaman Gümüş veya altın dayanıklılık düzeyleri kullanılacağı hakkında öneriler

Durum bilgisi olan hizmetleri beklediğiniz ölçek bileşenini barındıran tüm düğüm türleri için Gümüş veya altın dayanıklılık kullanın (VM örneği sayısını azaltmak) sık sık ve dağıtım işlemlerini bu ölçek işlemleri basitleştirme lehinde Gecikmeli tercih. (VM örnekleri ekleme) genişleme senaryoları dayanıklılık katmanı tercih ettiğiniz yürütülmez, yalnızca ölçek içinde değil.

### <a name="changing-durability-levels"></a>Dayanıklılık düzeylerini değiştirme
- Gümüş veya altın dayanıklılık düzeylerine sahip düğüm türleri için Bronz indirgenemez.
- Bronz Gümüş veya altın yükseltmek birkaç saat sürebilir.
- Dayanıklılık düzeyi değiştirirken, hem Service Fabric uzantı yapılandırmasındaki VMSS kaynağınız ve Service Fabric küme kaynağı düğümü tür tanımında güncelleştirdiğinizden emin olun. Bu değerlerin eşleşmesi gerekir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Düğümü için işletimsel öneriler, Gümüş veya altın dayanıklılık düzeyi ayarlamış olduğunuz yazın.

1. Küme ve uygulamalar her zaman sağlıklı tutmak ve uygulamaların tümüne yanıt emin olun [hizmet çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (derleme yinelemede kalmış gibi) zamanında.
2. (Ölçek yukarı/aşağı) VM SKU değişiklik yapmak için daha güvenli şekilde benimsemeyi: bir sanal makine ölçek kümesi VM SKU'su değiştirme doğası gereği güvenli olmayan bir işlemdir ve bu nedenle, mümkünse kaçınılmalıdır. Sık karşılaşılan sorunları önlemek için izleyebileceğiniz işlem şöyledir.
    - **Birincil olmayan nodetypes için:** yeni sanal makine ölçek kümesi oluşturmanız önerilir, yeni sanal makine ölçek kümesi/düğüm türü eklemek ve eski sanal makine ölçek kümesi örnek azaltmak için hizmet yerleşimi kısıtlamasını değiştirme 0 olarak (düğümleri kaldırılmasını küme güvenilirliğini etkilemeyen emin olmak için budur) bir seferde bir düğüm sayısı.
    - **Birincil nodetype için:** birincil düğüm türünde VM SKU değiştirmeyin bizim önerilir. SKU desteklenmiyor birincil düğüm türü değiştirme. Yeni SKU kapasitesi nedeni, daha fazla örnekleri ekleme öneririz. Bu, mümkün değildir, yeni bir küme oluşturun ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa), eski kümeden. Herhangi bir sistem hizmet durumu geri yükleme gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda yeniden oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların, küme üzerinde çalışan uygulamalarınızı yeni kümeye dağıtabilir, geri yüklenecek bir şey vardır. Desteklenmeyen rota gidin ve VM SKU değiştirmek istediğiniz karar verirseniz, sanal makine ölçek kümesi modeli tanımını yeni SKU yansıtacak şekilde değişiklikler yapma. Yalnızca bir nodetype kümeniz varsa, daha sonra durum bilgisi olan uygulamaların tümüne yanıt emin olun [hizmet çoğaltma yaşam döngüsü olayları](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (derleme yinelemede kalmış gibi) zamanında ve hizmet çoğaltma yeniden oluşturma süresi beş dakikadan daha kısa bir süre (için Gümüş dayanıklılık düzeyi) olur. 


> [!WARNING]
> VM ölçek kümesi VM SKU boyutunu değiştirme en az Gümüş dayanıklılık çalıştırmayan önerilmez. VM SKU boyutunu değiştirme veri bozucu yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için en az bazı özelliği olmadan işlemi durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile öngörülemeyen diğer işlemsel sorunlara neden olabilir. 
> 
    
3. Tüm sanal makine ölçek altın dayanıklılık düzeyine sahip kümesi için en az beş düğüm sayısını korumak veya gümüş etkin
4. Rastgele VM örneklerini silmek değil, her zaman sanal makine ölçek kümesi ölçek özelliği tuşunu kullanın. Rastgele VM örnekleri silinmesini UD ve FD üzerinden yayılan VM örneğinde dengede değil oluşturma bir olasılığı vardır. Bu dengesizliği sistemleri düzgün bir şekilde yük dengelemesi hizmet örnekleri/hizmet çoğaltmalar arasında olanağı olumsuz yönde etkileyebilir.
6. (VM örneklerini kaldırma), Ölçek gerçekleştirilir gibi yalnızca bir düğümün aynı anda otomatik ölçeklendirme kullanıyorsanız, ardından kuralları ayarlayın. Aynı anda birden fazla örneği Ölçeklendirmesi güvenli değil.
7. Birincil düğüm türü Ölçeklendirmesi, hiçbir zaman onu birden fazla güvenilirlik katmanını sağlar ölçeğini.


## <a name="the-reliability-characteristics-of-the-cluster"></a>Kümenin güvenilirliği
Güvenilirlik katmanı çoğaltmaları birincil düğüm türünde bu kümede çalıştırmak istediğiniz sistem hizmetlerinin sayısını ayarlamak için kullanılır. Daha fazla sayıda yineleme, daha güvenilir sistem kümenizdeki hizmetleridir.  

Güvenilirlik katmanı aşağıdaki değerleri alabilir:

* Sistem Hizmetleri Çalıştır platinum - sahip bir hedef çoğaltma 9 sayısını ayarlayın.
* Sistem Hizmetleri Çalıştır altın - sahip bir hedef çoğaltma 7 sayısını ayarlayın.
* Sistem Hizmetleri Çalıştır Gümüş - sahip bir hedef çoğaltma 5 sayısını ayarlayın. 
* Sistem Hizmetleri Çalıştır Bronz - sahip bir hedef çoğaltma 3 sayısını ayarlayın.

> [!NOTE]
> Seçtiğiniz güvenilirlik katmanı birincil düğüm türünüz olmalıdır düğüm sayısı alt sınırı belirler. 
> 
> 


### <a name="recommendations-for-the-reliability-tier"></a>Güvenilirlik katmanı ilgili öneriler.

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

Birincil düğüm türü kapasite planlama Kılavuzu İşte

1. **Azure üzerinde herhangi bir üretim iş yükünü çalıştırmak için VM örneği sayısı:** 5 en az bir birincil düğüm türü boyutunu belirtmeniz gerekir. 
2. **Azure'da test iş yüklerini çalıştırmak için VM örneklerinin sayısını** minimum birincil düğüm türü boyutu 1 veya 3 belirtebilirsiniz. Tek düğümlü bir küme, özel bir yapılandırma ve bunu ile çalışır, bu küme dışında ölçek desteklenmiyor. Tek düğümlü bir küme, güvenilirlik ve bu nedenle, Resource Manager şablonu, sahip olduğunuz Kaldır/değil Bu yapılandırmayı belirtmek (yapılandırma değeri ayarı olmayan değil yeterli). Portal ayarlanan bir düğümün küme ayarladıysanız, sonra yapılandırma otomatik olarak dikkate. 1 ve 3 düğümlü kümeler, üretim iş yükleri çalıştırmak için desteklenmez. 
3. **VM SKU:** birincil düğüm türü olduğundan Sistem Hizmetleri çalıştırdığı, onun için gereken genel yoğun dikkate al yük, seçtiğiniz VM SKU planlama kümesine yerleştirilecek. I burada anlamı göstermeye-birincil düğüm türü, "Lungs", beyin oxygen sağladıkları olduğunu düşündüğünüz benzetme işte ve bu nedenle, gövde beyin yeterli oxygen almazsa yükselmesine. 

Bir küme kapasite gereksinimlerini belirlenir olduğundan kümede çalıştırmayı planladığınız iş yüküne göre biz, ancak İşte yardımcı olmak için geniş Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sağlayamıyor

Üretim iş yükleri için 


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

> [!NOTE]
> Kümenizi bir service fabric sürümü 5.6 değerinden üzerinde çalışıyorsa, (5.6 Bu sorun düzeltilmiştir) çalışma zamanı, bir sorun nedeniyle 5'ten az, birincil olmayan düğüm türüne aşağı Ölçeklendirmesi sonuçları çağırmanız kadar sağlıksız, kapatma küme sistem durumu [ Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) uygun düğümü ada sahip. Okuma [Service Fabric kümesi içeri veya dışarı gerçekleştirmek](service-fabric-cluster-scale-up-down.md) daha fazla ayrıntı için
> 
>

**VM SKU:** VM SKU onun için seçtiğiniz planladığınız her düğüm yerleştirilecek yoğun yük dikkate gerekir böylece bu düğüm uygulama hizmetlerinizi burada çalıştırıyorsanız, türüdür. Nodetype kapasite ihtiyaçlarını, biz, ancak İşte yardımcı olmak için geniş Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sağlayamaz böylece kümedeki çalıştırmayı planladığınız iş yükü tarafından belirlenir

Üretim iş yükleri için 


- Standart D3 veya standart D3_V2 veya eşdeğeri önerilen VM SKU değerdir. 
- Minimum desteklenen VM SKU standart D1 veya standart D1_V2 veya eşdeğer kullanılır. 
- Kısmi çekirdek standart A0 gibi VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU performans nedenleriyle üretim iş yükleri için desteklenmez.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Kapasite planlama tamamlamak ve küme ayarlama sonra aşağıdaki okuyun:

* [Service Fabric kümesi güvenliği](service-fabric-cluster-security.md)
* [Olağanüstü Durum Kurtarma planlaması](service-fabric-disaster-recovery.md)
* [Sanal makine ölçek Nodetypes ilişkisini ayarlayın](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
