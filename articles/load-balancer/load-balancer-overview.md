---
title: Azure yük dengeleyici genel bakış | Microsoft Docs
description: Azure yük dengeleyici özellikleri, mimari ve uygulama genel bakış. Yük Dengeleyici nasıl çalıştığını öğrenin ve bulutta yararlanın.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: 3a5d1e897d8ffe063ecf9277bef346c8b7c5092b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-load-balancer-overview"></a>Azure Load Balancer’a genel bakış

Azure yük dengeleyici uygulamalarınızı ölçekleme ve hizmetleriniz için yüksek kullanılabilirlik oluşturmanıza olanak sağlar. Yük Dengeleyici gelen hem de giden senaryolarını destekler ve düşük gecikme, yüksek verimlilik sağlar ve akışlar tüm TCP ve UDP uygulamalar için en çok bir milyonlarca ölçeklendirir.   

Yük Dengeleyici arka uç havuzu örneklerine kurallarını ve sistem durumu araştırmalarının göre yük dengeleyicinin ön uçta ulaşan yeni gelen akışları dağıtır.  

Ayrıca, ortak bir yük dengeleyici genel IP adresleri için özel IP adresleri çevirerek giden bağlantılar için sanal makineler, sanal ağ içinde de da sağlayabilirsiniz.

İki farklı SKU'ları Azure yük dengeleyici kullanılabilir: temel ve standart.  Ölçek, özellikler ve fiyatlandırma farklılıklar vardır.  Yaklaşım biraz farklı olabilir ancak temel yük dengeleyici ile olası her senaryo standart yük dengeleyici ile de oluşturulabilir.  Yük Dengeleyici hakkında bilgi edinin gibi temel öğeleri ve SKU'ya özgü farklar öğrenmeniz önemlidir.

## <a name="why-use-load-balancer"></a>Yük Dengeleyici neden kullanılır? 

Azure yük dengeleyici için kullanılabilir:

* Gelen Internet trafiği dengelemek için sanal makineler yükleyin. Bu yapılandırma olarak bilinen bir [ortak yük dengeleyici](#publicloadbalancer).
* Sanal makineler bir sanal ağ içinde arasındaki yük dengelemek trafiği. Bir yük dengeleyici ön karma bir senaryoda bir şirket içi ağ üzerinden de ulaşabilir. Bu senaryoların her ikisini de olarak bilinen bir yapılandırma kullanmanız bir [iç yük dengeleyici](#internalloadbalancer).
* Gelen NAT kuralları ile belirli sanal makinelerde belirli bir bağlantı için bağlantı noktası iletme trafiği.
* Sağlamak [giden bağlantı](load-balancer-outbound-connections.md) ortak bir yük dengeleyici kullanarak sanal ağınızdaki sanal makineler için.


>[!NOTE]
> Azure tam olarak yönetilen Yük Dengeleme çözümlerinde senaryolarınız için dizisi sağlar.  TLS sonlandırma ("SSL boşaltma") veya HTTP/HTTPS isteği uygulama katmanı işleme başına arıyorsanız, gözden [uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md).  Aradığınız, Genel DNS için Yük Dengeleme, gözden [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md).  Uçtan uca senaryolarınızı gerektiğinde bu çözümleri birleştirme yararlı.

## <a name="what-is-load-balancer"></a>Yük Dengeleyici nedir?

Bir yük dengeleyici kaynak bir genel yük dengeleyiciye veya bir iç yük dengeleyici olarak bulunabilir. Yük Dengeleyici kaynağın işlevleri, bir ön uç, bir kural, bir sistem durumu araştırması ve arka uç havuzu tanımını ifade edilir.  Sanal makineler, sanal makine arka uç havuzundan belirterek arka uç havuzuna yerleştirilir.

Yük Dengeleyici kaynaklar içerisinde, nasıl Azure oluşturmak istediğiniz senaryo elde etmek için çok kiracılı altyapı program ifade edebilirsiniz nesneleridir.  Yük Dengeleyici kaynakları ve gerçek altyapısı arasında doğrudan ilişkisi yoktur; bir yük dengeleyici oluşturma örneğini oluşturmaz ve kapasite her zaman kullanılabilir. 

## <a name="fundamental-load-balancer-features"></a>Temel yük dengeleyici özellikleri

Yük Dengeleyici TCP ve UDP uygulamalar için aşağıdaki temel yetenekleri sağlar:

* **Yük Dengeleme**

    Azure yük dengeleyici, bir Yük Dengeleme kuralı ön uç arka uç havuzu örneklerine ulaşan trafik dağıtmak için oluşturmanıza olanak sağlar.  Gelen akış dağıtım için bir karma tabanlı algoritması kullanır ve arka uç havuzu örnekleri akışlarına üstbilgileri uygun şekilde yeniden yazar. Sunucu durumu araştırması sağlıklı arka uç nokta gösterdiğinde yeni akışları almak kullanılabilir.
    
    Varsayılan olarak, kullanılabilir sunuculara akışları eşlemek için kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve IP protokol numarası oluşan bir 5 bölütlü karma kullanır.  Belirli bir kural için 2 veya 3 bölütlü karma kullanmama tarafından belirli bir kaynak IP adresine benzeşimi oluşturmayı seçebilirsiniz.  Yük dengeli ön uç arkasındaki aynı örneğinde aynı paket akışı tüm paketlerin ulaşır.  Ne zaman istemci aynı kaynak IP, kaynak bağlantı noktası değişikliklerini yeni akışından başlatır. Sonuçta elde edilen 5-tanımlama grubu, sonuç olarak farklı arka uç noktasına gitmek trafiği neden olabilir.

    Daha fazla bilgi için bkz: [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md). Aşağıdaki grafikte karma tabanlı dağılımını gösterir:

    ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

    *Şekil - karma tabanlı dağıtım*

* **Bağlantı noktası iletme**

    Azure yük dengeleyici gelen NAT kuralı bağlantı noktası iletme trafiği belirli ön uç IP adresi sanal ağ içinde belirli arka uç örneğinin belirli bir bağlantı noktası için belirli bir bağlantı noktasından oluşturmanızı sağlar. Bu Yük Dengeleme olarak aynı olan karma tabanlı dağıtım tarafından gerçekleştirilir.  Bu özellik için genel senaryolar, tek tek sanal makine örnekleri sanal ağ içinde Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH) oturumlardır.  Farklı bağlantı noktalarını aynı ön uç IP adresinde birden çok iç uç nokta eşleyebilirsiniz. Bu, sanal makinelerinizi bir ek atlama kutusu gerek kalmadan Internet üzerinden uzaktan yönetmek için kullanabilirsiniz.

* **Belirsiz ve şeffaf uygulama**

    Yük Dengeleyici doğrudan TCP veya UDP veya uygulama katmanı ve tüm TCP ile etkileşime girmez veya UDP tabanlı uygulama senaryosu desteklenmiyor.  Örneğin, yük dengeleyici TLS kendisini sona olsa da, yapı ve yük dengeleyici kullanarak TLS uygulamalarının ölçeklendirmek ve sanal makineye TLS bağlantı sonlandırılacak. Yük Dengeleyici bir akış sonlandırmak değil ve protokolü el sıkışmaları her zaman doğrudan istemci ve karma seçili arka uç havuzu örnek arasında. Örneğin, bir TCP anlaşması her zaman istemci ve seçilen arka uç sanal makine arasında olur.  Ve bir isteğine yanıt olarak bir ön uç için arka uç sanal makineden oluşturulan bir yanıt.  Yük dengeleyicinin giden ağ performansı, boşta kalma zaman aşımı hiçbir zaman ulaşıldığında akışları ve sanal makine seçtiğiniz SKU tarafından yalnızca sınırlı uzun bir süre için etkin kalacak ' dir.

* **Otomatik yeniden yapılandırma**

    Yukarı veya aşağı örnekleri ölçeklendirdiğinizde azure yük dengeleyici anında kendisini yeniden yapılandırır. Yük Dengeleyici kaynak üzerinde başka işlemler olmadan yük dengeleyici ekleme ya da sanal makineleri arka uç havuzundan kaldırılıyor yeniden yapılandırır.

* **Sistem durumu araştırmalarının**

    Azure yük dengeleyici arka uç havuzundaki örneklerinin sistem durumunu belirlemek için tanımladığınız sistem durumu araştırmalarının kullanır. Yanıt bir araştırma başarısız olduğunda, yük dengeleyici sağlıksız örneklerine yeni bağlantılar gönderme durdurur. Var olan bağlantıların değil etkilendiğini ve boşta zaman aşımı oluşur, akış uygulama sonlanana veya sanal makine kapatılana kadar devam eder.

    Üç tür araştırmalar desteklenir:

    - **HTTP özel araştırma:** bir arka uç havuzu örneğinin sistem durumunu belirlemek için kendi özel mantık oluşturmak için kullanın. Yük Dengeleyici uç noktanızı (15 dakikada, varsayılan olarak) düzenli olarak araştırma. Örnek, bir HTTP 200 ile (varsayılan 31 saniye cinsinden) zaman aşımı süresi içinde yanıt verirse sağlıklı olarak kabul edilir. HTTP 200 dışındaki herhangi bir durumla Bu araştırma başarısız olmasına neden olur.  Ayrıca yük dengeleyicinin döndürme örneklerini kaldırmak için kendi mantığı uygulamak için kullanışlıdır. Örneğin, % 90 CPU örneğiyse 200 olmayan durumuna döndürmek için örnek yapılandırabilirsiniz.   Bu araştırma varsayılan Konuk aracı araştırması geçersiz kılar.

    - **TCP özel araştırma:** tanımlanan araştırma bağlantı noktasına başarılı TCP oturumu oluşturulması bu araştırma dayanır.  Belirtilen dinleyici sanal makinede mevcut olduğu sürece, bu araştırma başarılı olur. Bağlantıyı reddetti araştırma başarısız olur. Bu araştırma varsayılan Konuk aracı araştırması geçersiz kılar.

    - **Konuk Aracısı araştırma (olarak bir hizmet yalnızca sanal makineler platformunda):** yük dengeleyici sanal makine içinde Konuk Aracısı'nı da kullanabilir. Konuk Aracısı dinler ve yalnızca örnek hazır durumda olduğunda bir HTTP 200 Tamam yanıt ile yanıt verir. Aracı bir HTTP 200 Tamam ile yanıt vermiyorsa, yük dengeleyici örneği yanıt olarak işaretler ve trafiği için bu örneği göndermeye durdurur. Yük Dengeleyici örneği ulaşmaya çalışır devam eder. Bir HTTP 200 ile Konuk aracısı yanıt verirse, yük dengeleyici trafiği için bu örneği yeniden gönderir.  Konuk Aracısı araştırmalar son çare olan ve HTTP veya TCP özel araştırma yapılandırmaları mümkün olduğunda kullanılmamalıdır. 
    
* **Giden bağlantılar (kaynak NAT)**

    Sanal ağınızdaki özel IP adreslerinden tüm giden trafik akışları Internet'teki ortak IP adresleri için bir yük dengeleyici ön uç IP adresi çevrilebilir. Ortak bir ön uç arka uç sanal makine bir Yük Dengeleme kuralı yapmamanız bağlıdır, Azure genel ön uç bilgisayarın IP adresine otomatik olarak çevrilecek giden bağlantılar programlar. Bu kaynak NAT (SNAT) olarak da adlandırılır. SNAT önemli avantajları sağlar:

    + Ön uç hizmetinin başka bir örneğine dinamik olarak eşlenebilir olduğundan, hizmetlerin kolay yükseltme ve olağanüstü durum kurtarma etkinleştirir.
    + Erişim denetimi listesi (ACL) yönetimini kolaylaştırır. Ön uç Hizmetleri, ölçeklendirin gibi IP'leri değiştirmeyin bakımından ifade ACL aşağı veya imzalanmasını.

    Başvurmak [giden bağlantılar](load-balancer-outbound-connections.md) özelliği hakkında ayrıntılı bilgi için makalenin.

Standart yük dengeleyici bu temelleri ötesinde ek SKU'ya özgü yeteneklerine sahiptir.  Ayrıntılar için bu makalenin sonraki bölümlerinde gözden geçirin.

## <a name="skus"></a> Yük Dengeleyici SKU karşılaştırma

Azure yük dengeleyici iki farklı SKU'ları destekler: temel ve standart.  Senaryo ölçek, özellikler ve fiyatlandırma farklılıklar vardır.  Her senaryo temel yük dengeleyici ile mümkün olan standart yük dengeleyici de oluşturulabilir.  Aslında, her iki SKU'ları için API benzer ve bir SKU belirtimi aracılığıyla çağrılır.  Yük Dengeleyici ve genel IP için SKU'ları desteklemek için API 2017-08-01 API ile başlayarak kullanılabilir.  Her iki SKU'ları aynı genel API ve yapıya sahip.

Ancak, SKU, seçilen bağlı olarak, tam senaryo yapılandırma ayrıntısı biraz farklı olabilir. Bir makale için yalnızca belirli bir SKU uygulanabilir olduğunda yük dengeleyici belgelerine çağırır. Aşağıdaki tabloda karşılaştırır ve farkları anlamak için aşağıdaki gözden geçirin.  Gözden geçirme [standart yük dengeleyici genel bakış](load-balancer-standard-overview.md) daha ayrıntılı bilgi için.

>[!NOTE]
> Yeni Tasarım, standart yük dengeleyici kullanmayı düşünmelisiniz. 

Yalnızca tek başına sanal makineler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri bir SKU için hiçbir zaman hem de bağlanabilir. Genel IP adresi kullanıldığında, ortak IP adresi ve yük dengeleyici SKU eşleşmesi gerekir. Yük Dengeleyici ve ortak IP SKU'ları değişebilir değildir.

_Henüz zorunlu olmasa da SKU'ları açıkça belirtmek için en iyi bir uygulamadır._  Şu anda gerekli değişiklikleri en az olarak tutulduğunu. Bir SKU belirtilmezse, temel SKU 2017-08-01 API sürümü kullanmak için amacınız olarak yorumlanır.

>[!IMPORTANT]
>Standart yük dengeleyici, yeni bir yük dengeleyici ürün ve bir üst büyük ölçüde temel yük dengeleyicinin ' dir.  Her iki ürün arasında önemli ve kasıtlı farklar vardır.  Temel yük dengeleyici ile olası tüm uçtan uca senaryoyu olan standart yük dengeleyici oluşturulabilir.  Temel yük dengeleyiciye zaten kullanılıyorsa, standart ve temel ve etkilerini arasındaki davranış önemli değişiklikler anlamak için standart yük dengeleyici öğrenmeniz. Bu bölümde dikkatle gözden geçirin.

| | [Standart SKU](load-balancer-standard-overview.md) | Temel SKU |
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

Gözden geçirme [hizmet sınırları için yük dengeleyici](https://aka.ms/lblimits).  Standart yük dengeleyici için de daha ayrıntılı gözden [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing), ve [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Kavramlar

### <a name = "publicloadbalancer"></a>Genel yük dengeleyiciye

Ortak yük dengeleyici, genel IP adresi ve gelen trafiğin özel IP adresi için bağlantı noktası numarasını ve bağlantı noktası numarası sanal makinenin ve tersi yanıt trafiği sanal makineden eşler. Yük Dengeleme kurallarında belirli birden çok sanal makineler veya hizmetler arasındaki trafik türlerinin dağıtmak olanak sağlar. Örneğin, birden çok web sunucusu arasında web isteği trafik yükünü yayılabilir.

Yük dengeli bir uç nokta için genel ve özel TCP bağlantı noktası 80 üç sanal makineler arasında paylaşılan web trafiği için aşağıdaki şekilde gösterilmiştir. Bu üç sanal makine bir yük dengeli kümesi yok.

![Ortak yük dengeleyici örneği](./media/load-balancer-overview/IC727496.png)

**Şekil 1: web trafiği ortak bir yük dengeleyicisi kullanarak Dengeleme yükleme**

Internet istemcilerinin, TCP bağlantı noktası 80 üzerinde bir web uygulaması genel IP adresi için web sayfası istekleri gönderdiğinizde, Azure yük dengeleyici istekleri için yük dengeli kümesi içinde üç sanal makine arasında dağıtır. Yük Dengeleyici algoritmalar hakkında daha fazla bilgi için bkz: [yük dengeleyici genel bakış sayfasında](load-balancer-overview.md#load-balancer-features).

Varsayılan olarak, Azure yük dengeleyici ağ trafiği birden çok sanal makine örnekleri arasında eşit olarak dağıtır. Oturum benzeşimi da yapılandırabilirsiniz. Daha fazla bilgi için bkz: [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç yük dengeleyici

İç yük dengeleyici yalnızca bir sanal ağ içinde olmayan veya Azure altyapı erişmek için bir VPN kullanan kaynaklara trafiğini yönlendirir. Bu bakımdan iç yük dengeleyici bir genel yük Dengeleyiciden farklıdır. Azure altyapı, bir sanal ağ yük dengelemesi ön uç IP adreslerine erişimi sınırlandırır. Ön uç IP adresleri ve sanal ağlar hiçbir zaman doğrudan bir Internet uç noktasına sunulur. İç iş kolu satır uygulama Azure'da çalıştırın ve Azure içinde veya şirket içi kaynaklardan gelen sonuna erişilir.

İç yük dengeleyici Yük Dengeleme aşağıdaki türleri sağlar:

* Bir sanal ağ içinde: yük VM'lerin sanal ağ aynı sanal ağda bulunan sanal makineleri bir dizi Dengeleme.
* Bir şirket içi sanal ağ için: yük aynı sanal ağda bulunan sanal makineleri bir dizi şirket içi bilgisayarlardan Dengeleme. 
* Çok katmanlı uygulamalar için: Yük Dengeleme uç katmanları nerede değil Internet'e Internet'e çok katmanlı uygulamalar için. İnternet'e yönelik trafiği dengelemesini arka uç katmanları gerektirir (bkz. Şekil 2) katmanı.
* Satır iş kolu uygulamaları için: Yük Dengeleme ek yük dengeleyici donanım veya yazılım Azure üzerinde barındırılan satır iş kolu uygulamaları için. Bu senaryo, yük dengelemesi, trafik olduğu bilgisayarları kümesinde yer alan şirket içi sunucuları içerir.

![İç yük dengeleyici örneği](./media/load-balancer-overview/IC744147.png)

**Şekil 2 - Yük Dengeleme hem genel hem iç yük dengeleyici kullanan çok katmanlı uygulamalar**

## <a name="pricing"></a>Fiyatlandırma
Standart yük dengeleyici Yük Dengeleme kuralları yapılandırılmış ve işlenen tüm gelen ve giden veri sayısına göre kartınızdan bir üründür. Standart fiyatlandırma bilgileri için yük dengeleyici, ziyaret [yük dengeleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/load-balancer/) sayfası.

Temel yük dengeleyici ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

Standart yük dengeleyici SLA hakkında daha fazla bilgi için ziyaret [yük dengeleyici SLA](https://aka.ms/lbsla) sayfası. 

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [daha ayrıntılı standart yük dengeleyici](load-balancer-standard-overview.md)
- Kullanma hakkında bilgi edinin [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)
- Kullanma hakkında bilgi edinin [yük dengeleyici giden bağlantılar için](load-balancer-outbound-connections.md)
- Hakkında bilgi edinin [yük dengeleyici HA bağlantı noktaları](load-balancer-ha-ports-overview.md)
- Kullanma hakkında bilgi edinin [birden çok ön uçlar olan yük dengeleyici](load-balancer-multivip-overview.md)
- Hakkında bilgi edinin [VNet Hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
- Oluşturmayı öğrenin bir [temel genel yük dengeleyiciye](load-balancer-get-started-internet-portal.md)

