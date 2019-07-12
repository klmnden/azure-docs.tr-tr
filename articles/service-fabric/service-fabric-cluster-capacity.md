---
title: Service Fabric kümesi kapasite planlaması | Microsoft Docs
description: Service Fabric kümesi kapasite planlaması konuları. NodeType, işlemler, dayanıklılık ve güvenilirlik katmanları
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/09/2019
ms.author: chackdan
ms.openlocfilehash: 6b11a3ba4fbffe1d35b590f2e5c47f19b6fb028c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718125"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric kümesi kapasite planlaması konuları
Herhangi bir üretim dağıtımı için kapasite planlaması önemli bir adımdır. Bu işlemin bir parçası olarak dikkate almanız gereken öğelerden bazıları aşağıda verilmiştir.

* Düğüm türleri kümenizi ile başlatmak için gereken sayısı
* Her düğüm türünün (boyut, birincil, internet'e yönelik, sayısı VM'ler gibi) özellikleri
* Kümenin güvenilirlik ve dayanıklılık özellikleri

> [!NOTE]
> En düşük düzeyde tüm gözden geçirmeniz gereken **izin** planlaması sırasında ilke değerleri yükseltin. Değerleri uygun şekilde ayarlandığından emin olun ve kümeniz daha sonra değiştirilemez sistem yapılandırma ayarları nedeniyle yazma azaltmak için budur. 
> 

Bize kısaca bu öğelerin her birini gözden geçirin.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Düğüm türleri kümenizi ile başlatmak için gereken sayısı
Öncelikle, oluşturmakta olduğunuz küme için kullanılacak neler olduğunu şekil gerekir.  Ne tür uygulamaların bu kümesi dağıtmayı planlıyor musunuz? Küme amacına açık değilse, en olası olmayan henüz kapasite planlama işlemi girmek için hazır.

Düğüm türleri kümenizi ile başlatmak için gereken sayıda kurun.  Her düğüm türü, bir sanal makine ölçek kümesine eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Bu nedenle düğüm türü sayısı kararı temelde aşağıdaki önemli noktalar için gelir:

* Uygulamanızı birden çok hizmetlere sahip olmadığı ve herhangi biri genel veya internet'e yönelik olmam gerekir mi? Normal uygulama, istemciden gelen giriş alan bir ön uç ağ geçidi hizmeti ve iletişim kuran bir veya daha fazla arka uç Hizmetleri ile ön uç hizmetleri içerir. Bu nedenle bu durumda, en az iki düğüm türleri sonunda.
* Farklı altyapı gereksinimleri gibi daha büyük RAM veya daha yüksek CPU çevrimleri (uygulamanızı ayarlama yapan) hizmetlerinizi var mı? Örneğin, dağıtmak istediğiniz uygulamanın ön uç hizmeti ve arka uç hizmeti içerdiğini bize varsayın. Ön uç hizmeti (örneğin, D2 VM boyutları) bağlantı noktalarının açık internet'e sahip daha küçük sanal makinelerinde çalıştırabilirsiniz.  Arka uç hizmeti, hesaplama yoğun ve internet olmayan daha büyük Vm'leri (D4, D6, D15 gibi VM boyutlarıyla) çalıştırma gerekiyor ancak yaşıyor.
  
  Bir düğüm türünde, tüm hizmetleri koymak karar verebilirsiniz ancak bu örnekte, bunları bir kümede iki düğüm türleri ile yerleştirmenizi öneririz.  Bu, her düğüm türü, internet bağlantısı veya VM boyutu gibi farklı özelliklere sahip olmasını sağlar. VM sayısı ölçeklendirilebilir bağımsız olarak da.  
* Geleceği tahmin etmeyi çünkü bildiğiniz bilgileri gidin ve uygulamalarınız ile başlaması gereken düğüm türleri sayısını seçin. Her zaman ekleyebilir veya daha sonra düğüm türlerini kaldırın. Service Fabric kümesi en az bir düğüm türü olmalıdır.

## <a name="the-properties-of-each-node-type"></a>Her düğüm türünün özellikleri
**Düğüm türü** bulut hizmetlerindeki roller gibi çalışır görülebilir. Düğüm türleri VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlayın. Bir Service Fabric kümesinde tanımlanan her düğüm türü eşlenen bir [sanal makine ölçek kümesi](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Her düğüm türü ayrı bir ölçek kümesi ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri yapılabilir ' dir. Düğüm türleri ve sanal makine ölçek kümeleri arasındaki ilişkiler hakkında daha fazla bilgi için nasıl örneklerinden birinde RDP için yeni Aç bağlantı noktaları ve benzeri bkz [Service Fabric küme düğümü türlerini](service-fabric-cluster-nodetypes.md).

Service Fabric kümesi birden fazla düğüm türü oluşabilir. Bu olay tek bir birincil düğüm türü küme oluşur ve bir veya daha fazla birincil olmayan düğüm türleri.

Tek bir düğüm türü, sanal makine ölçek kümesi SF uygulamalar için başına 100 düğüm ötesinde güvenilir bir şekilde ölçeklendirilemez; güvenilir bir şekilde 100 düğüm büyük gerçekleştirmekten, ek sanal makine ölçek kümeleri eklemenizi gerektirir.

### <a name="primary-node-type"></a>Birincil düğüm türü

Service Fabric sistem hizmetlerinin (örneğin, Küme Yöneticisi hizmeti veya görüntü Store hizmet) birincil düğüm türünde yerleştirilir. 

![İki düğüm türleri olan bir kümenin ekran görüntüsü][SystemServices]

* **VM boyutu için alt** için birincil düğüm türü tarafından belirlenir **dayanıklılık katmanı** belirleyin. Varsayılan dayanıklılık katmanı Bronz gösterir. Bkz: [kümenin dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) daha fazla ayrıntı için.  
* **En düşük VM sayısı** için birincil düğüm türü tarafından belirlenir **güvenilirlik katmanını** belirleyin. Varsayılan güvenilirlik katmanını Silver ' dir. Bkz: [güvenilirliği kümenin](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) daha fazla ayrıntı için.  

Azure Resource Manager şablonundan birincil düğüm türü ile yapılandırılmış `isPrimary` altında özniteliği [düğüm türü tanımı](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Olmayan birincil düğüm türü

Birden çok düğüm türü ile bir kümede, rest, birincil olmayan ve bir birincil düğüm türü yoktur.

* **VM boyutu için alt** için birincil olmayan düğüm türleri tarafından belirlenir **dayanıklılık katmanı** belirleyin. Varsayılan dayanıklılık katmanı Bronz gösterir. Daha fazla bilgi için [kümenin dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).  
* **En düşük VM sayısı** birincil olmayan düğüm türlerinde biridir. Ancak, bu düğüm türü çalıştırmak istediğiniz uygulama/hizmetleri çoğaltmasına göre bu sayı seçmeniz gerekir. Küme dağıttıktan sonra bir düğüm türünde VM sayısı artırılabilir.

## <a name="the-durability-characteristics-of-the-cluster"></a>Kümenin dayanıklılık özellikleri
Dayanıklılık katmanı, sanal makinelerinizin temel Azure altyapısıyla sahip ayrıcalıkları sisteme belirtmek için kullanılır. Birincil düğüm türü, sistem hizmetleri ve durum bilgisi olan hizmetlerinizin çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istek (örneğin, bir VM yeniden başlatma, VM yeniden görüntü oluşturma ya da VM geçişi) duraklatmak Service Fabric bu ayrıcalığı sağlar. Birincil olmayan düğüm türleri bu ayrıcalık, durum bilgisi olan hizmetler için çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istekler (örneğin, VM yeniden başlatma, VM yeniden görüntü oluşturma ve VM geçişi) duraklatmak Service Fabric verir.

| Dayanıklılık katmanı  | Gerekli en düşük VM sayısı | Desteklenen VM SKU'ları                                                                  | Sanal makine ölçek kümenize yaptığınız güncelleştirmeler                               | Güncelleştirmeleri ve Azure tarafından başlatılan bakım                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Gold             | 5                              | Tek bir müşteriye (örneğin, L32s GS5, G5, DS15_v2, D15_v2) ayrılmış tam düğümlü SKU'ları | Service Fabric kümesi tarafından onaylanmış kadar ertelendi | Önceki hatalardan kurtarmak çoğaltmaları için ek süre vermek amacıyla UD başına 2 saat için duraklatıldı |
| Silver           | 5                              | Sanal makineleri tek çekirdekli veya üzeri en az 50 GB yerel SSD                      | Service Fabric kümesi tarafından onaylanmış kadar ertelendi | Önemli bir süre boyunca Gecikmeli                                                    |
| Bronz           | 1\.                              | VM en az 50 GB yerel SSD                                              | Service Fabric kümesi tarafından Gecikmeli değil           | Önemli bir süre boyunca Gecikmeli                                                    |

> [!WARNING]
> Çalışan Bronz dayanıklılığa sahip düğüm türleri elde _ayrıcalıkların olmadığı_. Bu durum bilgisiz iş yüklerinizi etkileyen altyapı işler değil durduruldu veya kaldırılacak geciktirilmiş, hangi iş yüklerinizi etkileyebilecek anlamına gelir. Yalnızca Bronz yalnızca durum bilgisiz iş yükleri çalıştıran düğümü türleri için kullanın. Üretim iş yükleri çalıştıran Silver veya yukarıda önerilir. 
> 
> Herhangi bir dayanıklılık düzeyi ne olursa olsun [ayırmayı kaldırma](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/deallocate) VM ölçek kümesi üzerinde işlem kümesi yok eder

**Silver veya Gold dayanıklılık düzeyleri kullanmanın avantajları**
 
- Bir ölçeklendirme işlemi gereken adım sayısını azaltır (diğer bir deyişle, düğümü devre dışı bırakma ve kaldırma ServiceFabricNodeState çağırıldı otomatik olarak).
- Bir müşteri tarafından başlatılan yerinde VM SKU değiştirme işlemi ya da Azure altyapı işlemler nedeniyle veri kaybı riskini azaltır.

**Silver veya Gold dayanıklılık düzeyleri kullanmanın dezavantajları**
 
- Dağıtımlar, sanal makine ölçek kümesi ve diğer ilgili Azure kaynaklarını Gecikmeli, zaman aşımına uğrayabilir veya tamamen sorunları, kümenizdeki veya altyapı düzeyinde tarafından engellendi. 
- Sayısını artıran [çoğaltma yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) (örneğin, birincil takasları) nedeniyle Azure altyapı işlemleri sırasında düğüm deactivations otomatik.
- Azure platformu yazılım güncelleştirme veya donanım bakım etkinlikleri gerçekleşen sırasında süreler için hizmet düğümlerinde alır. Bu etkinlikleri sırasında devre dışı bırakılması/devre dışı durumuna sahip düğümleri görebilirsiniz. Bu, kümenizin kapasitesi geçici olarak azaltır, ancak kullanılabilirlik kümesi veya uygulamaları etkilememesi gerekir.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Silver veya Gold dayanıklılık düzeyleri kullanıldığı durumlar için öneriler

Silver veya Gold dayanıklılık beklediğiniz ölçek için durum bilgisi olan hizmetler barındıran tüm düğüm türleri için kullanın (VM örneği sayısını azaltmak) sık sık ve dağıtım işlemlerini geciktirileceği tercih ediyorsanız ve bu ölçek açma basitleştirme yerine getirilmesini kapasite işlemler. (VM içeren örneklere ekleme) ölçek genişletme senaryoları dayanıklılık katmanı ettiğiniz yürütülmez, yalnızca ölçek de yapar.

### <a name="changing-durability-levels"></a>Dayanıklılık düzeylerini değiştirme
- Silver veya Gold dayanıklılık düzeyine sahip düğüm türleri için Bronz düşürülemez.
- Silver veya Gold Bronz ' yükseltme birkaç saat sürebilir.
- Dayanıklılık düzeyini değiştirirken, hem Service Fabric uzantısı yapılandırmasında sanal makine ölçek kümesi kaynağınıza ve Service Fabric kümesi kaynağınızın düğüm türü tanımında güncelleştirdiğinizden emin olun. Bu değerlerin eşleşmesi gerekir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Düğümü için işletimsel önerileri için silver veya gold dayanıklılık düzeyi ayarlamak yazın.

- Küme ve uygulamalar her zaman durumunun iyi kalmasını sağlamak ve uygulamalar için tüm yanıt emin [hizmet çoğaltması yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) (derleme çoğaltma takılmış gibi) zamanında.
- Değiştirme (Ölçek artırma/azaltma) bir sanal makine SKU'su yapmak için daha güvenli şekilde benimseme: Bir sanal makine ölçek kümesi sanal makine SKU'su değiştirme adımları ve konuları gerektirir. Sık karşılaşılan sorunları önlemek için izlemeniz gereken süreç şöyledir.
    - **Birincil olmayan düğüm türleri için:** Önerilen yeni sanal makine ölçek kümesi oluşturma, yeni sanal makine ölçek kümesi/düğüm türü eklemek ve ardından sıfır olarak teker teker (Bunu yapmak için olan bir düğümü eski sanal makine ölçek kümesi örnek sayısını azaltmak için hizmet yerleştirme kısıtlamasını değiştirme emin düğümlerin kaldırılması kümenin güvenilirlik etkilemez).
    - **Birincil düğüm türü:** Seçtiğiniz sanal makine SKU'su kapasitede ise ve daha büyük bir VM SKU için değiştirmek istediğiniz kılavuzumuzu izleyin [birincil düğüm türü için dikey ölçeklendirme](https://docs.microsoft.com/azure/service-fabric/service-fabric-scale-up-node-type). 

- En az bir etkin Silver veya Gold dayanıklılık düzeyine sahip tüm sanal makine ölçek kümesi için beş düğüm sayısı korur.
- Silver veya Gold dayanıklılık düzeyi ile her sanal makine ölçek, Service Fabric kümesi içinde kendi düğüm türüne eşlemeniz gerekir. Birden çok sanal makine ölçek kümeleri, tek bir düğüm türü için eşleme, Service Fabric kümesi ve Azure altyapı arasında koordinasyon gereksinimini olabildiğince düzgün çalışmasını engeller.
- Rastgele VM örneklerini silmek değil, her zaman sanal makine ölçek kümesi ölçek özelliği aşağı kullanın. Rastgele VM örneklerinin silme işlemi, UD ve FD üzerinden yayılan VM örneğinde dengede değil oluşturma bir olasılığına sahiptir. Bu dengesizliği sistemleri düzgün bir şekilde Yük Dengeleme Hizmeti hizmeti örnekleri çoğaltmaları arasından olanağı olumsuz yönde etkileyebilir.
- (VM örneklerini kaldırarak) içinde ölçeklendirme yapılır yalnızca bir düğümün aynı anda otomatik ölçeklendirme kullanırken, ardından kuralları ayarlayın. Aynı anda birden fazla örneğini ölçeklendirme güvenli değildir.
- Silme veya birincil düğüm türünde VM serbest bırakılıyor, hiçbir zaman ne güvenilirlik katmanını gerektirir aşağıda ayrılmış VM sayısı azaltmanız gerekir. Bu işlemler, bir ölçek kümesindeki bir dayanıklılık düzeyi Silver veya Gold ile süresiz olarak engellenir.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Kümenin güvenilirlik özellikleri
Güvenilirlik katmanı çoğaltmaları birincil düğüm türünde bu kümede çalıştırmak istediğiniz sistem hizmetlerinin sayısını ayarlamak için kullanılır. Daha fazla sayıda yineleme, daha güvenilir, kümenizin sistem hizmetleri şunlardır.  

Güvenilirlik katmanı, şu değerleri alabilir:

* Platinum - sistem hizmetlerini çalıştıran bir hedef çoğaltma yedi sayısını ayarlayın.
* Altın - sistem hizmetlerini çalıştıran bir hedef çoğaltma yedi sayısını ayarlayın.
* Silver - sistem hizmetlerini çalıştıran bir hedef çoğaltma beş sayısını ayarlayın. 
* Bronz - sistem hizmetlerini çalıştıran bir hedef çoğaltma üç ayarlayın.

> [!NOTE]
> Seçtiğiniz güvenilirlik katmanını birincil düğüm türünüz olmalıdır düğüm sayısı alt sınırı belirler. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Güvenilirlik katmanı için öneriler

Artırır veya azaltırsanız, küme (tüm düğüm türleri VM örnekleri toplamı), kümenizin bir katmandan diğerine güvenilirlik güncelleştirmeniz gerekir. Bunun yapılması, Sistem Hizmetleri çoğaltma değiştirmek için sayınızın Küme yükseltme tetikler. Kümeye düğüm ekleme gibi diğer herhangi bir değişiklik yapmadan önce tamamlamak için devam eden yükseltme için bekleyin.  Service Fabric Explorer veya çalıştırarak yükseltmesinin ilerleme durumunu izleyebilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Güvenilirlik katmanı seçme öneri aşağıdadır.  Çekirdek düğüm sayısı da düğümleri güvenilirlik katmanı için en düşük sayısına ayarlayın.  Örneğin, altın güvenilirliğe sahip bir küme için 7 çekirdek düğümleri yoktur.

| **Küme düğümleri sayısı** | **Güvenilirlik katmanı** |
| --- | --- |
| 1\. |Güvenilirlik katmanı parametreyi belirtmezseniz, sistem hesaplar |
| 3 |Bronz |
| 5 veya 6|Silver |
| 7 veya 8 |Gold |
| 9 ve üstü |Platinum |

## <a name="primary-node-type---capacity-guidance"></a>Birincil düğüm türü - Kapasite Kılavuzu

Birincil düğüm türü kapasite planlama Kılavuzu şu şekildedir:

- **Tüm üretim iş yüklerini Azure'da çalıştırmak için sanal makine örneği sayısı:** Birincil düğüm türü boyutu için alt 5 ve bir güvenilirlik katmanını, Gümüş belirtmeniz gerekir.  
- **Test iş yüklerini Azure'da çalıştırmak için sanal makine örneği sayısını** 1 veya 3 en düşük birincil düğüm türü boyutu belirtebilirsiniz. Bir düğüm kümesi çalıştıran özel bir yapılandırma ve bu nedenle, bu küme olarak ölçeklendirilmesini desteklenmiyor. Tek düğümlü bir küme, güvenilirlik ve bu nedenle Resource Manager şablonunuzu, sahip olduğunuz Kaldır/değil söz konusu yapılandırmayı belirtmek (yapılandırma değeri ayarı yok değil yeterli). Portal ayarlanan bir düğüm kümesi ayarlama, daha sonra yapılandırma otomatik olarak dikkate. Bir ile üç düğümlü kümeler, üretim iş yüklerini çalıştırmak için desteklenmez. 
- **SANAL MAKİNE SKU'SU:** Bunun için genel yoğun dikkate al, yüklemelisiniz seçtiğiniz sanal makine SKU'su planı kümesine yerleştirmek birincil düğüm türü Sistem Hizmetleri çalıştırdığı olduğundan. Ne burada bilmem göstermek-birincil düğüm türü, "Lungs", oxygen, beyin için sağlanan korumanın olduğunu düşündüğünüz bir benzerliği işte ve beyin yeterli oxygen almazsa, bu nedenle, gövde alternatife. 

Küme kapasitesi gereksinimlerini belirlenir olduğundan, kümedeki çalıştırmayı planladığınız iş yüküne göre ancak İşte yardımcı olacak geniş kapsamlı Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sunamıyoruz

Üretim iş yükleri için: 

- Kümelerinize ayrılması önerilir ikincil NodeType uygulamanızı dağıtmak için birincil NodeType sistem hizmetleri ve yerleştirme kısıtlamaları kullanın.
- Önerilen sanal makine SKU'su, standart D2_V2 veya en az 50 GB'lık yerel SSD eşdeğerini değil.
- En düşük desteklenen sanal makine SKU'su standart_d2_v3 veya standart D1_V2 ya da en az 50 GB'lık yerel SSD eşdeğerini kullanılır. 
- Bizim en az 50 GB önerilir. Özellikle Windows kapsayıcıları ne zaman çalışan, iş yükleriniz için daha büyük disklerin gereklidir. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Bir dizi VM SKU'ları, performans nedenleriyle üretim iş yükleri için desteklenmez.
- Düşük öncelikli VM'ler desteklenmez.

> [!WARNING]
> Birincil düğüm üzerinde çalışan bir küme VM SKU boyutu değiştirme bir ölçeklendirme işlemi ve belirtilmiştir [sanal makine ölçek kümesi ölçeği genişletme](virtual-machine-scale-set-scale-node-type-scale-out.md) belgeleri.

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Olmayan birincil düğüm türü - durum bilgisi olan iş yükleri için kapasite Kılavuzu

Bu kılavuzu kullanarak Service fabric durum bilgisi olan iş yükleri için olan [güvenilir koleksiyonlar veya reliable Actors](service-fabric-choose-framework.md) birincil olmayan düğüm türü çalıştırmakta olduğunuz.

**Sanal makine örneği sayısını:** Durum bilgisi olan üretim iş yükleri için en az ve hedef çoğaltma sayısının 5 ile çalıştırmanızı önerilir. Bu, kararlı durumda, her hata etki alanı ve yükseltme etki alanında (çoğaltma kümesinden) çoğaltmasıyla düştüğünden emin anlamına gelir. Birincil düğüm türü için tüm güvenilirlik katmanını kavramı, sistem hizmetleri için bu ayarı belirtmek için bir yoldur. Bu nedenle aynı göz önünde bulundurarak, durum bilgisi olan hizmetler için geçerlidir.

Bu nedenle, durum bilgisi olan iş yükleri içinde çalıştırıyorsanız, üretim iş yükleri için en az önerilen olmayan birincil düğüm türü boyutu 5 ise.

**SANAL MAKİNE SKU'SU:** Sanal makine SKU'su, seçtiğiniz her bir düğümüne yerleştirmek için planlama yükü dikkate gerekir, böylece bu düğüm, uygulama hizmetleri çalıştığı, türüdür. Düğüm türü, kapasite ihtiyaçlarını, ancak İşte yardımcı olacak geniş kapsamlı Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sunamıyoruz için kümedeki çalıştırmayı planladığınız iş yüküne göre belirlenir

Üretim iş yükleri için 

- Önerilen sanal makine SKU'su, standart D2_V2 veya en az 50 GB'lık yerel SSD eşdeğerini değil.
- En düşük desteklenen sanal makine SKU'su standart_d2_v3 veya standart D1_V2 ya da en az 50 GB'lık yerel SSD eşdeğerini kullanılır. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Bir dizi VM SKU'ları, performans nedenleriyle üretim iş yükleri için desteklenmez.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Olmayan birincil düğüm türü - durum bilgisiz iş yükleri için kapasite Kılavuzu

Bu kılavuz, birincil olmayan düğüm türünde çalışan durum bilgisiz iş yükleri.

**Sanal makine örneği sayısını:** Durum bilgisiz olduğundan üretim iş yükleri için en düşük desteklenen olmayan birincil düğüm türü boyutu 2'dir. Bu, uygulamanızı ve hizmetinizi vererek bir sanal makine örneği kaybı varlığını sürdürmesi için iki durum bilgisiz örneklerini çalıştırmanıza olanak sağlar. 

**SANAL MAKİNE SKU'SU:** Sanal makine SKU'su, seçtiğiniz her bir düğümüne yerleştirmek için planlama yükü dikkate gerekir, böylece bu düğüm, uygulama hizmetleri çalıştığı, türüdür. Düğüm türü Kapasite ihtiyaçlarını, kümede çalıştırmayı planladığınız iş yüküne göre belirlenir. Belirli iş yükünüz için nitel rehberlik sunamıyoruz.  Ancak, başlamanıza yardımcı olmak için geniş kılavuzunu aşağıdadır.

Üretim iş yükleri için 

- Önerilen sanal makine SKU'su, standart D2_V2 veya eşdeğer değil. 
- En düşük desteklenen sanal makine SKU'su standart D1 veya standart D1_V2 veya eşdeğer kullanılır. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Bir dizi VM SKU'ları, performans nedenleriyle üretim iş yükleri için desteklenmez.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Son kapasite planlaması ve bir küme oluşturma sonra aşağıdaki okuyun:

* [Service Fabric küme güvenliği](service-fabric-cluster-security.md)
* [Service Fabric kümesini ölçekleme](service-fabric-cluster-scaling.md)
* [Olağanüstü Durum Kurtarma planlaması](service-fabric-disaster-recovery.md)
* [NodeType sanal makine ölçek ilişkisi](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
