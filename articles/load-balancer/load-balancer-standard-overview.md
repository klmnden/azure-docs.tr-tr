---
title: Azure standart yük dengeleyici genel bakış | Microsoft Docs
description: Azure standart yük dengeleyici özelliklerine genel bakış
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: d7ee74a19f806faed0bcfcfa5f1c5de3937d9f31
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-load-balancer-standard-overview"></a>Azure yük dengeleyici standart genel bakış

Azure yük dengeleyici uygulamalarınızı ölçekleme ve hizmetleriniz için yüksek kullanılabilirlik oluşturmanıza olanak sağlar. Yük Dengeleyici gelen hem de giden senaryoları için kullanılır ve düşük gecikme, yüksek verimlilik sağlar ve akışlar tüm TCP ve UDP uygulamalar için en çok bir milyonlarca ölçeklendirir. 

Bu makalede, standart yük dengeleyici üzerinde odaklanmıştır.  Azure yük dengeleyici için daha genel bir bakış için gözden [yük dengeleyici genel bakış](load-balancer-overview.md) de.

## <a name="what-is-standard-load-balancer"></a>Standart yük dengeleyici nedir?

Standart yük dengeleyici temel yük dengeleyicisi genişletilmiş ve daha ayrıntılı bir özelliği olan tüm TCP ve UDP uygulamalar için yeni bir yük dengeleyici üründür.  Benzer şekilde olsa da, farklarla bu makalede açıklanan şekilde öğrenmeniz önemlidir.

Genel veya iç yük dengeleyici olarak standart yük dengeleyici standart kullanabilirsiniz. Ve bir genel ve bir iç yük dengeleyici kaynak bir sanal makineye bağlanabilir.

Yük Dengeleyici kaynağın işlevleri her zaman bir ön uç, bir kural, bir sistem durumu araştırması ve arka uç havuzu tanımını ifade edilir.  Bir kaynak birden çok kural içerebilir. Sanal makineler sanal makinenin NIC kaynak arka uç havuzundan belirterek arka uç havuzuna yerleştirebilirsiniz.  Bir sanal makine ölçek kümesi söz konusu olduğunda, bu parametre ağ profili geçirilen ve genişletilmiş.

Sanal ağ kaynak için bir anahtar yönü kapsamıdır.  Temel yük dengeleyicisi bir kullanılabilirlik kümesi kapsam içinde var, ancak standart bir yük dengeleyici sanal ağ kapsamını ile tamamen tümleşiktir ve tüm sanal ağ kavramları uygulayın.

Yük Dengeleyici kaynaklar içerisinde, nasıl Azure oluşturmak istediğiniz senaryo elde etmek için çok kiracılı altyapı program ifade edebilirsiniz nesneleridir.  Yük Dengeleyici kaynakları ve gerçek altyapısı arasında doğrudan ilişkisi yoktur; bir yük dengeleyici oluşturma örneğini oluşturmaz, kapasite her zaman kullanılabilir olur ve hiçbir başlatma veya dikkate alınması gereken gecikmeler ölçeklendirme vardır. 

>[!NOTE]
> Azure tam olarak yönetilen Yük Dengeleme çözümlerinde senaryolarınız için dizisi sağlar.  TLS sonlandırma ("SSL boşaltma") veya HTTP/HTTPS isteği uygulama katmanı işleme başına arıyorsanız, gözden [uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md).  Aradığınız, Genel DNS için Yük Dengeleme, gözden [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md).  Uçtan uca senaryolarınızı gerektiğinde bu çözümleri birleştirme yararlı.

## <a name="why-use-standard-load-balancer"></a>Standart yük dengeleyici neden kullanılır?

Sanal veri merkezleri, büyük ve karmaşık çok bölge mimarileri küçük ölçekli dağıtımlarından tam aralığını standart yük dengeleyici kullanabilirsiniz.

Standart yük dengeleyici ve temel yük dengeleyici arasındaki farklar genel bir bakış için aşağıdaki tabloyu gözden geçirin:

>[!NOTE]
> Yeni Tasarım, standart yük dengeleyici kullanmayı düşünmelisiniz. 

| | Standart SKU | Temel SKU |
| --- | --- | --- |
| Arka uç havuzu boyutu | en fazla 1000 örnekleri | en fazla 100 örnekleri |
| Arka uç havuzu uç noktaları | harmanlama sanal makinelerin kullanılabilirlik kümeleri dahil olmak üzere tek bir sanal ağda herhangi bir sanal makineye sanal makine ölçek ayarlar. | sanal makineler tek kullanılabilirlik kümesi veya sanal makine ölçek kümesi |
| Kullanılabilirlik Alanları | bölge olarak yedekli ve zonal ön uçlar için gelen ve giden, giden akış eşlemeleri bölge hatası varlığını sürdürmesini, çapraz bölge Yük Dengeleme | / |
| Tanılama | Azure İzleyici bayt ve paket sayaçları, sistem durumu da dahil olmak üzere çok boyutlu ölçümleri araştırma durumu, bağlantı denemeleri (TCP Eşitlemeye), giden bağlantı durumu (SNAT başarılı ve başarısız akışları), etkin veri düzlemi ölçümleri | Yalnızca ortak yük dengeleyici, SNAT tükenmesi Uyarısı, arka uç havuzu sistem durumu sayısı için Azure günlük analizi |
| HA bağlantı noktaları | İç yük dengeleyici | / |
| Varsayılan olarak güvenli | ortak IP ve yük dengeleyici uç noktaları ve ağ güvenlik grubu için kapalı varsayılan açıkça güvenilir listeye trafiği için akış için kullanılması gerekir | Varsayılan açın, ağ güvenlik grubu isteğe bağlı |
| Giden bağlantılar | Kural başına birden çok ön uçlar ile çevirin. Giden bir senaryo _gerekir_ açıkça oluşturulabilir giden bağlantı kullanabilmek sanal makine için.  [VNet Hizmeti uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) giden bağlantısı olmadan erişilebilir ve doğru işlenen veri sayılmaz.  VNet Hizmeti uç noktalar olarak kullanılamaz Azure PaaS Hizmetleri dahil olmak üzere tüm genel IP adresleri, giden bağlantı ve işlenen veri doğrultusunda sayısı üzerinden ulaşılmalıdır. Bir iç yük dengeleyici yalnızca bir sanal makine hizmet veren, varsayılan SNAT aracılığıyla giden bağlantılar kullanılamaz. Giden SNAT programlama gelen Yük Dengeleme kuralını protokolünü temel aktarım belirli protokolüdür. | Birden çok ön uçlar mevcut olduğunda rastgele seçili tek bir ön uç.  Bir sanal makine yalnızca iç yük dengeleyici hizmet veren, varsayılan SNAT kullanılır. |
| Birden çok ön Uçlar | Gelen ve giden | Yalnızca gelen |
| Yönetim işlemleri | Çoğu işlemleri < 30 saniye | 60-90 saniye tipik |
| SLA | iki sağlıklı sanal makinelerle veri yolu için % 99,99 | VM SLA örtülü | 
| Fiyatlandırma | İşlenen veri kuralları sayısına göre gelen veya giden kaynakla ilişkili ücret  | Ücret ödemeden |

Gözden geçirme [hizmet sınırları için yük dengeleyici](https://aka.ms/lblimits), yanı [fiyatlandırma](https://aka.ms/lbpricing), ve [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Arka uç havuzu

Bir sanal ağ içinde herhangi bir sanal makine kaynağı için standart yük dengeleyici arka uç havuzları genişletir.  En fazla 1000 arka uç örnekleri içerebilir.  Bir NIC kaynak özelliği bir IP yapılandırması arka uç örneğidir.

Arka uç havuzu, tek başına sanal makineler, kullanılabilirlik kümeleri veya sanal makine ölçek kümesi içerebilir.  Arka uç havuzundaki kaynakları karıştırabilirsiniz ve bu kaynakları 150 toplam kadar herhangi bir birleşimini içerebilir.

Arka uç havuzu tasarlamak nasıl değerlendirirken için en az tasarlayabilirsiniz daha fazla yönetim işlemlerini süresini iyileştirmek için tek tek arka uç havuzu kaynaklarına sayısı.  Veri düzlemi performansı veya ölçeği fark yoktur.

## <a name="az"></a>Kullanılabilirlik bölgeleri

>[!NOTE]
> Kullanılacak [kullanılabilirlik bölgeleri Önizleme](https://aka.ms/availabilityzones) olan standart yük dengeleyici gerektirir [kullanılabilirlik bölgeler için kaydolma](https://aka.ms/availabilityzones).

Standart yük dengeleyici ek yetenekler kullanılabilirlik bölgeleri kullanılabildiği bölgelerde destekler.  Bu özellikleri tüm standart yük dengeleyiciye artımlı sağlar.  Kullanılabilirlik bölgeleri yapılandırmaları için genel ve iç standart yük dengeleyici kullanılabilir.

Olmayan zonal ön uçlar kullanılabilirlik bölgeleri içeren bir bölgede dağıtıldığında varsayılan bölge olarak yedekli olur.   Bölge olarak yedekli bir ön uç bölge hatası devam eder ve tüm bölgelere ayrılmış altyapısı tarafından eşzamanlı olarak sunulur. 

Ayrıca, belirli bir bölge için bir ön garanti edebilir. Zonal bir ön uç kader ilgili bölgeyle paylaşır ve yalnızca tek bir bölge içinde özel altyapı tarafından sunulur.

Çapraz bölge Yük Dengeleme için arka uç havuzu kullanılabilir olduğundan ve bir sanal ağda herhangi bir sanal makine kaynak bir arka uç havuzunun parçası olabilir.

Gözden geçirme [kullanılabilirlik bölgeler hakkında ayrıntılı bilgi ilgili yeteneklerini](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Tanılama

Standart yük dengeleyici Azure İzleyicisi üzerinden çok boyutlu ölçümleri sağlar.  Bu ölçümler olabilir filtre, gruplandırılmış ve performans ve sistem durumu hizmeti geçerli ve geçmiş Öngörüler sağlayın.  Kaynak durumu da desteklenir.  Aşağıda desteklenen tanılama kısa bir genel bakış verilmiştir:

| Ölçüm | Açıklama |
| --- | --- |
| VIP kullanılabilirliği | Yük Dengeleyici standart sürekli veri yolundan bir bölgedeki tüm VM destekleyen SDN yığını için ön uç yük dengeleyiciye uygular. Sağlıklı örnekleri kaldığı sürece ölçüm uygulamanızın yükü dengelenmiş trafiğinin aynı yol izler. Müşteriler tarafından kullanılan veri yolu ayrıca doğrulanır. Ölçüm uygulamanıza görünmez olur ve diğer işlemlerle engellemez.|
| DIP kullanılabilirliği | Yük Dengeleyici standart uygulama uç noktanın yapılandırma ayarlarınıza göre izler hizmeti yoklama dağıtılmış bir sistem durumu kullanır. Bu ölçüm bitiş noktası filtre görünümünde yük dengeleyici her bağımsız örnek uç başına havuzu veya bir toplama sağlar.  Yük Dengeleyici sistem durumu araştırma yapılandırmanızı tarafından belirtildiği gibi uygulamanızın nasıl görünümleri görebilirsiniz.
| Eşitlemeye paketleri | Yük Dengeleyici standart olmayan TCP bağlantılarını sonlandırma veya TCP veya UDP paket akışları ile etkileşim. Akışlar ve bunların el sıkışmaları her zaman kaynak ve VM örneği arasında olur. Daha iyi TCP protokolü senaryolarınızı gidermek için Eşitlemeye kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paketler sayaçları çalışır hale getirilir. Ölçüm alınan TCP Eşitlemeye paketlerin sayısını raporlar.|
| SNAT bağlantıları | Yük Dengeleyici standart ön uç genel IP adresine verdiğinizi giden akış sayısı bildirir. SNAT bağlantı noktalarını exhaustible bir kaynaktır. Bu ölçüm nasıl yoğun bir şekilde uygulamanızın üzerinde SNAT giden kaynaklı akışlar için bağlı bir gösterge verebilirsiniz.  Başarılı ve başarısız giden SNAT akışlar için sayaçları bildirilir ve sorun giderme ve giden trafik akışları durumunu anlamak için kullanılabilir.|
| Bayt sayaçları | Yük Dengeleyici standart ön uç başına işlenen veri bildirir.|
| Paket sayaçları | Yük Dengeleyici standart başına ön uç işlenen paketleri bildirir.|

Gözden geçirme [ayrıntılı standart yük dengeleyici tanılama tartışma](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>HA bağlantı noktaları

Standart yük dengeleyici kuralı yeni bir türünü destekler.  

Yük Dengeleme kuralları, uygulama ölçek yapmak ve yüksek oranda güvenilir için yapılandırabilirsiniz. Bir HA bağlantı noktalarını Yük Dengeleme kuralı, standart yük dengeleyici kullanırsanız, her bir iç standart yük dengeleyicinin ön uç IP adresi kısa ömürlü bağlantı noktasında akış dengelemesini başına sağlayacaktır.  Özellik pratik ya da tek tek bağlantı noktalarını belirlemek için istenmeyen olduğu diğer senaryolar için kullanışlıdır.

Bir HA bağlantı noktalarını Yük Dengeleme kuralı ağ sanal Gereçleri ve gelen bağlantı noktalarının geniş aralıklarını gerektiren herhangi bir uygulama için Etkin-pasif veya aktif-aktif n + 1 senaryoları oluşturmanızı sağlar.  Bir sistem durumu araştırması hangi arka uçlarını yeni akışları alma belirlemek için kullanılabilir.  Bir bağlantı noktası aralığı senaryo benzetmek için bir ağ güvenlik grubu kullanın.

>[!IMPORTANT]
> Bir ağ sanal gereç kullanmayı planlıyorsanız, kendi ürün HA bağlantı noktalarıyla olup olmadığını test edilmiştir hakkında rehberlik için satıcınıza başvurun ve uygulama için kendi özel yönergeleri izleyin. 

Gözden geçirme [ayrıntılı bağlantı noktalarının HA tartışma](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Varsayılan olarak güvenli

Tam olarak standart yük dengeleyici sanal ağa edildi ' dir.  Sanal ağ özel, kapalı bir ağdır.  Standart yük dengeleyicileri ve standart genel IP adresleri sanal ağ dışında erişilebilmesi bu sanal ağ izin verecek şekilde tasarlandığından, bu kaynakları artık açmadan sürece kapalı varsayılan. Bu ağ güvenlik grupları (Nsg'ler) artık açıkça izin vermek için kullanılır ve beyaz liste trafiğe izin anlamına gelir.  Tüm sanal veri merkezinizde oluşturmak ve NSG ne ve ne zaman kullanılabilir olmalıdır karar verebilirsiniz.  Bir NSG'yi bir alt ağdaki veya NIC, sanal makine kaynağının yoksa, biz bu kaynağa erişmeye trafiğine izin yok.

Nsg'ler ve senaryonuz için uygulama hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md).

### <a name="outbound"></a> Giden bağlantılar

Yük Dengeleyici, gelen ve giden senaryolarını destekler.  Standart yük dengeleyici göre giden bağlantılar için temel yük dengeleyici önemli ölçüde farklıdır.

Kaynak ağ adresi çevirisi (SNAT), yük dengeleyici ön uçlar ortak IP adreslerini iç, özel IP adresleri sanal ağınızdaki eşlemek için kullanılır.

Standart yük dengeleyici için yeni bir algoritma tanıtır bir [daha güçlü, ölçeklenebilir ve tahmin edilebilir SNAT algoritması](load-balancer-outbound-connections.md#snat) ve yeni yeteneklerini etkinleştirir, kaldırır belirsizlik ve zorlar açık yapılandırmaları yerine etkileri tarafı. Bu değişiklikler için yeni özellikler çıkmaya izin vermek gereklidir. 

Bu, standart yük dengeleyici ile çalışırken anımsaması anahtar tenets şunlardır:

- tamamlandığında bir kuralı, yük dengeleyici kaynak denetler.  Azure tüm programlama yapılandırmasıyla türer.
- birden çok ön uçlar kullanılabilir olduğunda, tüm ön uçlar kullanılır ve kullanılabilir SNAT bağlantı noktalarının sayısı her ön uç çarpar
- seçin ve giden bağlantılar için kullanılacak belirli bir ön uç için istemiyorsanız denetleyebilirsiniz.
- Giden senaryoları açık ve bu belirtilmiş kadar giden bağlantı yok.
- Yük Dengeleme kuralları nasıl SNAT programlanmış Infer. Yük Dengeleme Protokolü belirli kurallardır. SNAT belirli bir protokoldür ve yapılandırma bu yansıtacak yerine bir yan etkisi oluşturun.

#### <a name="multiple-frontends"></a>Birden çok ön Uçlar
Bekliyorsanız veya zaten giden bağlantılar için çok fazla istek yaşayan olan olduğundan daha fazla SNAT bağlantı noktalarını istiyorsanız da artımlı SNAT bağlantı noktası stok ek ön uçlar, kuralları ve arka uç havuzu aynı sanal makineye yapılandırarak ekleyebilirsiniz kaynaklar.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Hangi ön uç için kullanıldığından denetim giden
Yalnızca belirli ön uç IP adresinden kaynaklanacak şekilde giden bağlantılar sınırlamak istiyorsanız, isteğe bağlı olarak, giden eşleme ifade eder kuralda giden SNAT devre dışı bırakabilirsiniz.

#### <a name="control-outbound-connectivity"></a>Denetim giden bağlantı
Standart yük dengeleyici sanal ağ bağlamı içinde yok.  Bir sanal ağ yalıtılmış, özel bir ağdır.  Genel IP adresine sahip bir ilişki mevcut değilse, ortak bağlantısını izin verilmiyor.  Sizinle iletişim kurabileceği [VNet hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) içini ve sanal ağınıza yerel olduğundan.  Sanal ağınızın dışında bir hedefe giden bağlantı kurmak istiyorsanız, iki seçeneğiniz vardır:
- Örnek düzeyinde ortak IP adresi için bir sanal makine kaynağı olarak bir standart SKU ortak IP adresi atayın veya
- sanal makine kaynağı ortak bir standart yük dengeleyici arka uç havuzunda yerleştirin.

Her ikisi de sanal ağa sanal ağ dışında giden bağlantısını izin verir. 

Varsa, _yalnızca_ bir iç standart yük, sanal makine kaynağı bulunduğu arka uç havuzu ile ilişkilendirilen dengeleyici varsa, sanal makineniz yalnızca sanal ağ kaynaklarına erişebilir ve [VNet hizmeti Uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).  Giden bağlantı oluşturmak için önceki paragrafta açıklanan adımları izleyebilirsiniz.

Giden bağlantı önce standart SKU'ları kalır ile ilişkili olmayan bir sanal makine kaynağı.

Gözden geçirme [ayrıntılı giden bağlantılar tartışması](load-balancer-outbound-connections.md).

### <a name="multife"></a>Birden çok ön Uçlar
Yük Dengeleyici birden çok ön uçlar ile birden çok kural destekler.  Standart yük dengeleyici, bu giden senaryoları için genişletir.  Esas olarak gelen bir Yük Dengeleme kuralı tersini giden senaryolar verilmiştir.  Gelen Yük Dengeleme kuralını da giden bağlantılar için bir ilişkilendirme oluşturur. Standart yük dengeleyici Yük Dengeleme kuralı aracılığıyla bir sanal makine kaynakla ilişkili tüm ön uçlar kullanır.  Ayrıca, Yük Dengeleme üzerinde bir parametre kural ve bir Yük Dengeleme kuralı hiçbiri de dahil olmak üzere belirli ön uçlar seçimine izin verir giden bağlantı amaçları doğrultusunda gizlemek olanak sağlar.

Karşılaştırma için temel yük dengeleyici tek bir ön uç rastgele seçer ve hangisinin seçilmedi denetleme yeteneği yoktur.

Gözden geçirme [ayrıntılı giden bağlantılar tartışması](load-balancer-outbound-connections.md).

### <a name="operations"></a> Yönetim işlemleri

Standart yük dengeleyici kaynakları tamamen yeni bir altyapı platformda mevcut.  Bu önemli ölçüde daha hızlı yönetimi işlemleri için standart SKU'ları sağlar ve tamamlanma sürelerini genellikle 30 saniyeden daha kısa bir süre standart SKU kaynak başına bulunurlar.  Arka uç havuzları boyutu arttıkça, arka ucu için gerekli süre havuzu da artış değiştiğine dikkat edin.

Standart yük dengeleyici kaynakları değiştirebilir ve standart bir ortak IP adresi bir sanal makineden çok daha hızlı bir şekilde taşıyın.

## <a name="migration-between-skus"></a>SKU'ları arasında geçiş

SKU'ları değişebilir değildir. SKU bir kaynak grubundan diğerine taşımak için bu bölümdeki adımları izleyin.

### <a name="migrate-from-basic-to-standard-sku"></a>Standart SKU Basic'ten geçirme

1. Yeni bir standart kaynağı (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kurallarınızı oluşturun ve tanımları araştırma.

2. Yeni oluşturun veya varolan NSG NIC veya alt ağ üzerinde izin vermek istediğiniz diğer tüm trafik yanı sıra beyaz liste yükü dengelenmiş trafiği, araştırma güncelleştirin.

3. Temel SKU kaynakları (yük dengeleyici ve geçerli genel IP'ler) tüm VM örnekleri kaldırın. Ayrıca bir kullanılabilirlik kümesi tüm VM örnekleri kaldırdığınızdan emin olun.

4. Tüm VM örnekleri için yeni standart SKU kaynaklar bağlayın.

### <a name="migrate-from-standard-to-basic-sku"></a>Temel SKU standart geçirme

1. Yeni bir temel kaynak (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kurallarınızı oluşturun ve tanımları araştırma. 

2. Standart SKU kaynakları (yük dengeleyici ve geçerli genel IP'ler) tüm VM örnekleri kaldırın. Ayrıca bir kullanılabilirlik kümesi tüm VM örnekleri kaldırdığınızdan emin olun.

3. Tüm VM örnekleri için yeni temel SKU kaynaklar bağlayın.

>[!IMPORTANT]
>
>Temel ve standart SKU'ları kullanımıyla ilgili sınırlamalar vardır.
>
>HA bağlantı noktalarını ve standart SKU tanılama yalnızca standart SKU kullanılabilir. Standart SKU temel SKU geçirmek ve ayrıca bu özellikleri korur.
>
>Temel ve standart SKU farklar sayısı bu makalede açıklanan gibi sahiptir.  Anlama ve bunlar için hazırlama emin olun.
>
>SKU'ları eşleşen yük dengeleyici ve genel IP kaynakları için kullanılması gerekir. Temel SKU ve standart SKU kaynaklarının karışımına sahip olamaz. Tek başına sanal makineler, sanal makinelerin bir kullanılabilirlik kümesi kaynağı eklenemiyor veya aynı anda hem de SKU'ları için bir sanal makine ölçek kaynakları ayarlayın.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Yük Dengeleyici standart tüm genel bulut bölgelerde şu anda kullanılabilir değil.

## <a name="sla"></a>SLA

Standart yük dengeleyici % 99,99 SLA ile kullanılabilir.  Gözden geçirme [standart yük dengeleyici SLA](https://aka.ms/lbsla) Ayrıntılar için.

## <a name="pricing"></a>Fiyatlandırma

Standart yük dengeleyici Yük Dengeleme kuralları yapılandırılmış ve işlenen tüm gelen ve giden veri sayısına göre kartınızdan bir üründür. Standart fiyatlandırma bilgileri için yük dengeleyici, ziyaret [yük dengeleyici fiyatlandırma](https://aka.ms/lbpricing) sayfası.

## <a name="limitations"></a>Sınırlamalar

- Yük Dengeleyici arka uç örnekleri şu anda eşlenen sanal ağlarda yer alamaz. Arka uç hepsinin aynı bölgede olması gerekir.
- SKU'ları değişebilir değildir. Mevcut bir kaynağı SKU'su değiştiremeyebilir.
- Bir tek başına sanal makine kaynağı kaynak kullanılabilirlik kümesi veya sanal makine ölçek kümesi kaynağı hiçbir zaman hem de bir SKU başvuruda bulunabilir.
- [Azure uyarıları izleme](../monitoring-and-diagnostics/monitoring-overview-alerts.md) şu anda desteklenmiyor.
- [Taşıma abonelik işlemlerini](../azure-resource-manager/resource-group-move-resources.md) standart SKU LB ve PIP kaynakları için desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında bilgi edinin [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)
- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md).
- Hakkında bilgi edinin [standart yük dengeleyici tanılama](load-balancer-standard-diagnostics.md).
- Hakkında bilgi edinin [çok boyutlu ölçümleri desteklenen](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) tanılamada için [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md).
- Kullanma hakkında bilgi edinin [yük dengeleyici giden bağlantılar için](load-balancer-outbound-connections.md)
- Hakkında bilgi edinin [HA bağlantı noktalarını Yük Dengeleme kuralları ile standart yük dengeleyici](load-balancer-ha-ports-overview.md)
- Kullanma hakkında bilgi edinin [birden çok ön uçlar olan yük dengeleyici](load-balancer-multivip-overview.md)
- Hakkında bilgi edinin [sanal ağlar](../virtual-network/virtual-networks-overview.md).
- Daha fazla bilgi edinmek [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md).
- Hakkında bilgi edinin [VNet Hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
- Başka bir anahtar bazıları hakkında bilgi edinin [ağı yetenekleri](../networking/networking-overview.md) azure'da.
- Daha fazla bilgi edinmek [yük dengeleyici](load-balancer-overview.md).
