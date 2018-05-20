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
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 080a4e670b06544d84e3d34a0b04bdb91a95aff1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="what-is-azure-load-balancer"></a>Azure yük dengeleyici nedir?

Azure yük dengeleyici ile uygulamalarınızı ölçekleme ve hizmetlerinizi için yüksek kullanılabilirlik oluşturun. Yük Dengeleyici gelen ve giden senaryolarını destekler, düşük gecikme süreli ve yüksek verimlilik sağlar ve akışlar tüm TCP ve UDP uygulamalar için en çok bir milyonlarca ölçeklendirir.  

Yük Dengeleyici arka uç havuzu örneklerine kurallarını ve sistem durumu araştırmalarının göre yük dengeleyicinin ön uçta gelmesini yeni gelen akışları dağıtır. 

Ayrıca, bir genel yük dengeleyiciye genel IP adresleri için özel IP adresleri çevirerek giden bağlantılar için sanal makineleri (VM'ler), sanal ağınızda sağlayabilir.

İçinde iki SKU'ları Azure yük dengeleyici kullanılabilir: temel ve standart. Ölçek, özellikler ve fiyatlandırma farklılıklar vardır. Yaklaşımlar biraz farklı olabilir ancak temel yük dengeleyici ile mümkün olan her senaryo standart yük dengeleyici ile de oluşturulabilir. Yük Dengeleyici hakkında bilgi edinin gibi temel öğeleri ve SKU'ya özgü farklar öğrenmeniz önemlidir.

## <a name="why-use-load-balancer"></a>Yük Dengeleyici neden kullanılır? 

Azure yük dengeleyici kullanabilirsiniz:

* Yük Dengeleme gelen Internet trafiği Vm'leriniz için. Bu yapılandırma olarak bilinen bir [genel yük dengeleyiciye](#publicloadbalancer).
* Bir sanal ağ içinde trafik yükünü dengele VM'ler arasında. Yük Dengeleyici ön uç karma bir senaryoda bir şirket içi ağ üzerinden de ulaşabilir. Her iki senaryoyu olarak bilinen bir yapılandırmasını kullanmak bir [iç yük dengeleyici](#internalloadbalancer).
* Belirli VM ile birlikte gelen ağ adresi çevirisi (NAT) kuralları belirli bir bağlantı noktası için bağlantı noktası iletme trafiği.
* Sağlamak [giden bağlantı](load-balancer-outbound-connections.md) bir genel yük dengeleyici kullanarak sanal ağınızdaki VM'ler için.


>[!NOTE]
> Azure yük dengeleyici çözümleri tam olarak yönetilen bir paketi senaryolarınız için sağlar. Aradığınız, Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL boşaltma") veya başına-HTTP/HTTPS isteği, uygulama katmanı için gözden [uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md). Aradığınız, Genel DNS için Yük Dengeleme, gözden [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md). Uçtan uca senaryolarınızı gerektiğinde bu çözümleri birleştirme yararlanabilir.

## <a name="what-are-load-balancer-resources"></a>Yük Dengeleyici kaynaklar nelerdir?

Bir yük dengeleyici kaynak bir genel yük dengeleyiciye veya bir iç yük dengeleyici olarak bulunabilir. Yük Dengeleyici kaynağın işlevleri, bir ön uç, bir kural, bir sistem durumu araştırması ve arka uç havuzu tanımını ifade edilir. VM arka uç havuzundan belirterek arka uç havuzuna VM'ler yerleştirin.

Yük Dengeleyici kaynaklar içerisinde, nasıl Azure oluşturmak istediğiniz senaryo elde etmek için çok kiracılı altyapı program ifade edebilirsiniz nesneleridir. Yük Dengeleyici kaynakları ve gerçek altyapısı arasında doğrudan ilişkisi yoktur. Bir yük dengeleyici oluşturma örneğini oluşturmaz ve kapasite her zaman kullanılabilir. 

## <a name="fundamental-load-balancer-features"></a>Temel yük dengeleyici özellikleri

Yük Dengeleyici TCP ve UDP uygulamalar için aşağıdaki temel yetenekleri sağlar:

* **Yük Dengeleme**

    Azure yük dengeleyici ile ön uç arka uç havuzu örneklerine ulaşan trafik dağıtmak için bir Yük Dengeleme kuralı oluşturabilirsiniz. Yük Dengeleyici gelen akışları dağıtım için bir karma tabanlı algoritması kullanır ve arka uç havuzu örnekleri akışlarına üstbilgileri uygun şekilde yeniden yazar. Bir sunucu, bir sistem durumu araştırması sağlıklı arka uç nokta gösterdiğinde yeni akışları almak kullanılabilir.
    
    Varsayılan olarak, yük dengeleyici kullanılabilir sunuculara akışları eşlemek için kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve IP protokol numarası oluşan bir 5 bölütlü karma kullanır. Belirli bir kural için 2 veya 3 bölütlü karma kullanmama tarafından belirli bir kaynak IP adresine benzeşimi oluşturmayı seçebilirsiniz. Yük dengeli bir ön uç arkasındaki aynı örneğinde aynı paket akışı tüm paketlerin ulaşır. Ne zaman istemci aynı kaynak IP, kaynak bağlantı noktası değişikliklerini yeni akışından başlatır. Sonuç olarak, 5-tanımlama grubu farklı arka uç noktasına gitmek trafiğine neden.

    Daha fazla bilgi için bkz: [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md). Aşağıdaki resimde karma tabanlı dağıtım görüntüler:

    ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

    *Şekil: Karma tabanlı dağıtım*

* **Bağlantı noktası iletme**

    Yük Dengeleyici ile belirli bir bağlantı noktasına bir sanal ağ içindeki belirli arka uç örneğinin belirli ön uç IP adresinin belirli bir bağlantı noktasından gelen NAT kuralı bağlantı noktası iletme trafiği için oluşturabilirsiniz. Bu Yük Dengeleme olarak aynı olan karma tabanlı dağıtım tarafından gerçekleştirilir. Bu özellik için ortak senaryolar Azure sanal ağ içindeki tekil VM örnekleriyle Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH) oturumlarını verilmiştir. Çeşitli bağlantı noktaları aynı ön uç IP adresinde birden çok iç uç nokta eşleyebilirsiniz. Vm'leriniz bir ek atlama kutusu gerek kalmadan internet üzerinden uzaktan yönetmek için kullanabilirsiniz.

* **Belirsiz ve şeffaf uygulama**

    Yük Dengeleyici doğrudan TCP veya UDP veya uygulama katmanı ve tüm TCP ile etkileşime girmez veya UDP uygulama senaryosu desteklenmiyor.  Yük Dengeleyici yok sonlandırmak veya akışları kaynaklanan, etkileşimde akış yükü hiçbir uygulama katmanı ağ geçidi işlevi sağlar ve protokolü el sıkışmaları her zaman meydana doğrudan istemci ve arka uç havuzu örnek arasında.  Gelen bir akış yanıt her zaman bir sanal makineden bir yanıt olan.  Sanal makinede akış geldiğinde, özgün kaynak IP adresini de korunur.  Daha fazla saydamlık göstermek için örnekler birkaç:
    - Tüm uç yalnızca bir VM tarafından yanıt verdi.  Örneğin, bir TCP anlaşması her zaman istemci ve seçilen arka uç VM arasında oluşur.  Bir ön uç isteğine yanıt VM arka ucu tarafından üretilen bir yanıt olan. Bir ön uç bağlantısı başarıyla doğruladığınızda en az bir arka uç sanal makine için uçtan uca bağlantı özelliklerini doğrulama.
    - Uygulama yükü yük dengeleyici ve tüm UDP saydam veya TCP uygulama desteklenebilir. HTTP istek işleme veya uygulama katmanı yüklerini işlenmesini gerektiren iş yükleri için (örneğin, HTTP URL'lerini ayrıştırma), bir katman 7 yük dengeleyici gibi kullanması gereken [uygulama ağ geçidi](https://azure.microsoft.com/services/application-gateway).
    - Yük Dengeleyici TCP yükü belirsiz olduğundan ve TLS Boşaltması ("SSL") sağlanmaz, yük dengeleyici kullanarak uçtan uca şifrelenmiş senaryolar yapı ve VM TLS bağlantıda sonlandırarak TLS uygulamalar için büyük ölçeklendirme elde.  Örneğin, TLS oturum anahtarlama kapasitenizi yalnızca arka uç havuzuna eklemek VM'lerin sayısını ve türünü sınırlıdır.  "SSL boşaltma", uygulama katmanı işleme veya Azure sertifika yönetimi temsilci istiyorsanız gerektiriyorsa, Azure'nın katman 7 yük dengeleyici kullanması gereken [uygulama ağ geçidi](https://azure.microsoft.com/services/application-gateway) yerine.
        

* **Otomatik yeniden yapılandırma**

    Yukarı veya aşağı örnekleri ölçeklendirdiğinizde yük dengeleyici anında kendisini yeniden yapılandırır. Yük Dengeleyici kaynak üzerinde başka işlemler olmadan yük dengeleyici ekleme veya VM'ler arka uç havuzundan kaldırılıyor yeniden yapılandırır.

* **Sistem durumu araştırmalarının**

     Arka uç havuzundaki örneklerinin sistem durumunu belirlemek için yük dengeleyici tanımladığınız sistem durumu araştırmalarının kullanır. Yanıt bir araştırma başarısız olduğunda, yük dengeleyici sağlıksız örneklerine yeni bağlantılar gönderme durdurur. Var olan bağlantıların etkilenmez ve VM'yi kapatın veya boşta zaman aşımı oluşur, akış uygulama sonlanana kadar devam eder.

    Üç tür araştırmalar desteklenir:

    - **HTTP özel araştırma**: bir arka uç havuzu örneğinin sistem durumunu belirlemek için kendi özel mantık oluşturmak için bu araştırma kullanabilirsiniz. Yük Dengeleyici uç noktanızı (15 dakikada, varsayılan olarak) düzenli olarak araştırmaları. Örnek, bir HTTP 200 ile (varsayılan 31 saniye cinsinden) zaman aşımı süresi içinde yanıt verirse sağlıklı olarak kabul edilir. HTTP 200 dışındaki herhangi bir durumla Bu araştırma başarısız olmasına neden olur. Bu araştırma, yük dengeleyicinin döndürme örneklerini kaldırmak için kendi mantığı uygulamak için de yararlıdır. Örneğin, yüzde 90 CPU büyük bir örneğiyse, 200 durumuna döndürmek için örnek yapılandırabilirsiniz.  Bu araştırma varsayılan Konuk aracı araştırması geçersiz kılar.

    - **TCP özel araştırma**: tanımlı araştırma noktasına başarılı bir TCP oturumu oluşturma Bu araştırma kullanır. Belirtilen dinleyici VM'de var olduğu sürece, bu araştırma başarılı olur. Bağlantıyı reddetti araştırma başarısız olur. Bu araştırma varsayılan Konuk aracı araştırması geçersiz kılar.

    - **Konuk Aracısı araştırma (platformunda yalnızca bir hizmet [PaaS] vm'lerle)**: yük dengeleyici ayrıca VM Konuk Aracısı'nı kullanabilir. Konuk Aracısı dinler ve yalnızca örnek hazır durumda olduğunda bir HTTP 200 Tamam yanıt ile yanıt verir. Aracı bir HTTP 200 Tamam ile yanıt vermiyorsa, yük dengeleyici örneği yanıt olarak işaretler ve trafiği için bu örneği göndermeye durdurur. Yük Dengeleyici örneği ulaşmaya çalışır devam eder. Yük Dengeleyici trafiği için bu örneği ile bir HTTP 200 Konuk aracısı yanıt verirse, yeniden gönderir. Konuk Aracısı araştırmalar son çare olan ve HTTP veya TCP özel araştırma yapılandırmaları mümkün olduğunda önerilmez. 
    
* **Giden bağlantılar (SNAT)**

    Sanal ağınızdaki özel IP adreslerinden tüm giden trafik akışları Internet'teki ortak IP adresleri için bir yük dengeleyici ön uç IP adresi çevrilebilir. Ortak bir ön uç arka uç VM bir Yük Dengeleme kuralı yapmamanız bağlıdır, Azure genel ön uç IP adresine otomatik olarak çevrilecek giden bağlantılar programlar.

    * Ön uç hizmetinin başka bir örneğine dinamik olarak eşlenebilir çünkü kolay yükseltme ve olağanüstü durum kurtarma hizmetlerinin etkinleştirin.
    * Daha kolay erişim denetimi listesi (ACL) yönetimi için. Ön uç bakımından ifade ACL IP'leri Hizmetleri ölçek yukarı veya aşağı değiştirmeyin veya imzalanmasını.  Makineler uygulamaları güvenilir listeye almayı yükünü azaltabilir daha küçük bir IP adresi sayısı giden bağlantılara çevirme.

    Daha fazla bilgi için bkz: [giden bağlantılar](load-balancer-outbound-connections.md).

Standart yük dengeleyici bu temelleri ötesinde ek SKU'ya özgü özellikleri vardır. Ayrıntılar için bu makalenin sonraki bölümlerinde gözden geçirin.

## <a name="skus"></a> Yük Dengeleyici SKU karşılaştırma

Yük Dengeleyici, temel ve standart her senaryo ölçek, özellikler, farklı ve fiyatlandırma SKU destekler. Temel yük dengeleyici ile mümkün olan her senaryo standart yük dengeleyici ile de oluşturulabilir. Aslında, her iki SKU'ları için API benzer ve bir SKU belirtimi aracılığıyla çağrılır. Yük Dengeleyici ve genel IP için SKU'ları desteklemek için API 2017-08-01 API ile başlayarak kullanılabilir. Her iki SKU'ları aynı genel API ve yapıya sahip.

Ancak, seçtiğiniz SKU bağlı olarak, tam senaryo yapılandırması biraz farklı olabilir. Bir makale yalnızca belirli SKU'ya uyguladığında yük Dengeleyicinizin belgelerine çağırır. Karşılaştırmak ve farkları anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz: [standart yük dengeleyici genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Daha yeni bir tasarım senaryo kullanıyorsanız, standart yük dengeleyici kullanmayı düşünün. 

Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca bir SKU'ya, hiçbir zaman hem de bağlanabilir. Ortak IP adresleri ile kullandığınız zaman, yük dengeleyici ve genel IP adresi SKU eşleşmesi gerekir. Yük Dengeleyici ve ortak IP SKU'ları değişebilir değildir.

_Henüz zorunlu olmasa da SKU'ları açıkça belirtmek için en iyi bir uygulamadır._  Şu anda gerekli değişiklikleri en az olarak tutulduğunu. Bir SKU belirtilmezse, temel SKU 2017-08-01 API sürümü kullanmak için bir amaç yorumlanır.

>[!IMPORTANT]
>Standart yük dengeleyici, yeni bir yük dengeleyici ürün ve bir üst büyük ölçüde temel yük dengeleyicinin ' dir. Bu iki ürün arasında önemli ve kasıtlı farklar vardır. Temel yük dengeleyici ile mümkün olan her uçtan uca senaryo da olan standart yük dengeleyici oluşturulabilir. Temel yük dengeleyiciye kullanmış olduğunuz, standart ve temel ve etkilerini arasında davranışı en son değişiklikleri anlamak için standart yük dengeleyici öğrenmeniz. Bu bölümde dikkatle gözden geçirin.

| | [Standart SKU](load-balancer-standard-overview.md) | Temel SKU |
| --- | --- | --- |
| Arka uç havuzu boyutu | En fazla 1000 örnekleri. | En fazla 100 örnekleri. |
| Arka uç havuzu uç noktaları | Tüm VM harmanlama VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri dahil olmak üzere tek bir sanal ağda. | Sanal makineleri tek bir kullanılabilirlik kümesi veya sanal makine ölçek kümesi. |
| Azure Kullanılabilirlik Alanları | Bölge olarak yedekli ve zonal ön uçları için gelen ve giden, giden akış eşlemeleri bölge hatası varlığını sürdürmesini, çapraz bölge Yük Dengeleme. | / |
| Tanılama | Azure İzleyici bayt ve paket sayaçları, sistem durumu da dahil olmak üzere çok boyutlu ölçümleri durumu, bağlantı denemeleri (TCP Eşitlemeye), giden bağlantı durumu (SNAT başarılı ve başarısız akışları), etkin veri düzlemi ölçümleri araştırma. | Azure Log Analytics public için yük dengeleyici yalnızca, SNAT tükenmesi Uyarısı, arka uç havuzu sistem durumu sayısı. |
| HA bağlantı noktaları | İç yük dengeleyici. | / |
| Varsayılan olarak güvenli | Varsayılan olarak, IP ve yük dengeleyici için ortak uç noktaları kapalı. Akış trafiği için ağ güvenlik grubu açıkça beyaz liste varlıklara kullanılması gerekir. | Varsayılan açın, ağ güvenlik grubu isteğe bağlıdır. |
| Giden bağlantılar | Birden çok ön Kural başına çevirin ile sona erer. Giden bir senaryo _gerekir_ açıkça oluşturulabilir VM giden bağlantı kullanabilmek için. [Sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) giden bağlantısı olmadan erişilebilir ve doğru işlenen veri sayılmaz. Sanal Ağ Hizmeti uç noktalar olarak kullanılamaz Azure PaaS Hizmetleri dahil olmak üzere tüm genel IP adresleri, giden bağlantı ve işlenen veri doğru sayısı üzerinden ulaşılmalıdır. Yalnızca bir iç yük dengeleyici VM hizmet veren, varsayılan SNAT aracılığıyla giden bağlantılar kullanılamaz. Giden SNAT programlama Aktarım Protokolü olduğu belirli, gelen Yük Dengeleme kuralı protokole ilişkin temel. | Birden çok ön uçlar mevcut olduğunda rastgele seçili tek ön uç. Yalnızca bir iç yük dengeleyici VM hizmet veren, varsayılan SNAT kullanılır. |
| Birden çok ön Uçlar | Gelen ve giden. | Yalnızca gelen. |
| Yönetim işlemleri | Çoğu işlemleri < 30 saniye sayısı. | 60-90 saniye tipik. |
| SLA | 99,99 iki sağlıklı VM ile bir veri yolu için yüzde. | VM SLA örtülü. | 
| Fiyatlandırma | Ücretleri kuralların sayısını temel alır ve işlenen veri gelen veya giden, kaynakla ilişkilendirilmiş.  | Ücret ödemeden. |

Daha fazla bilgi için bkz: [hizmet sınırları için yük dengeleyici](https://aka.ms/lblimits). Standart yük dengeleyici Ayrıntılar için bkz [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing), ve [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Kavramlar

### <a name = "publicloadbalancer"></a>Genel yük dengeleyiciye

Bir genel yük dengeleyiciye genel IP adresi ve bağlantı noktası numarasını gelen trafiğin özel IP adresi ve bağlantı noktası numarasını VM ve tersi yönde yanıt trafiği sanal makineden eşler. Yük Dengeleme kuralları uygulayarak, birden çok sanal makineleri veya hizmetleri belirli trafik türlerine dağıtabilirsiniz. Örneğin, birden çok web sunucusu arasında web isteği trafik yükünü yayılabilir.

Yük dengeli bir uç nokta üç VM'ler için genel ve özel TCP bağlantı noktası 80 arasında paylaşılan web trafiği için aşağıdaki şekilde gösterilmiştir. Bu üç VM'ler içinde yük dengeli bir kümesidir.

![Ortak yük dengeleyici örneği](./media/load-balancer-overview/IC727496.png)

*Şekil: bir genel yük dengeleyicisi kullanarak Dengeleme web trafiği yükleme*

İnternet istemcilerinin, TCP bağlantı noktası 80 üzerinde bir web uygulaması genel IP adresi için Web sayfası istekleri gönderdiğinizde, Azure yük dengeleyici istekleri için yük dengeli kümesi içinde üç VM'ler arasında dağıtır. Yük Dengeleyici algoritmalar hakkında daha fazla bilgi için bkz: [yük dengeleyici özelliği](load-balancer-overview.md##fundamental-load-balancer-features) bu makalenin.

Varsayılan olarak, Azure yük dengeleyici ağ trafiği birden çok VM örnekleri arasında eşit olarak dağıtır. Oturum benzeşimi da yapılandırabilirsiniz. Daha fazla bilgi için bkz: [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç yük dengeleyici

Bir iç yük dengeleyici sanal ağ içinde olmayan veya Azure altyapı erişmek için bir VPN kullanan kaynaklara trafiğini yönlendirir. Bu bakımdan, bir iç yük dengeleyici bir genel yük dengeleyiciden farklıdır. Azure altyapı, bir sanal ağ yük dengelemesi ön uç IP adreslerine erişimi sınırlandırır. Ön uç IP adresleri ve sanal ağlar hiçbir zaman doğrudan bir Internet uç noktasına sunulur. İç iş kolu satır uygulama Azure'da çalıştırın ve Azure içinde veya şirket içi kaynaklardan gelen sonuna erişilir.

Bir iç yük dengeleyici Yük Dengeleme aşağıdaki türleri sağlar:

* **Bir sanal ağ içinde**: yük VM'lerin sanal ağ aynı sanal ağda bulunan sanal makineleri bir dizi Dengeleme.
* **Bir şirket içi sanal ağ için**: yük aynı sanal ağda bulunan sanal makineleri bir dizi şirket içi bilgisayarlardan Dengeleme. 
* **Çok katmanlı uygulamalar için**: Yük Dengeleme burada arka uç katmanları olmayan internet'e yönelik internet'e yönelik çok katmanlı uygulamalar için. Arka uç katmanları trafik Yük Dengeleme gerektiren Internet'e Katmanı (sonraki şekilde bakın).
* **Satır iş kolu uygulamaları için**: Yük Dengeleme ek yük dengeleyici donanım veya yazılım Azure üzerinde barındırılan satır iş kolu uygulamaları için. Bu senaryo, yük dengelemesi, trafik olduğu bilgisayarları kümesinde yer alan şirket içi sunucuları içerir.

![İç yük dengeleyici örneği](./media/load-balancer-overview/IC744147.png)

*Şekil: çok katmanlı uygulamalar hem genel hem iç yük dengeleyicisi kullanarak Dengeleme yükleme*

## <a name="pricing"></a>Fiyatlandırma
Standart yük dengeleyici kullanımına yapılandırılmış Yük Dengeleme kuralları sayısı ve işlenen gelen ve giden veri miktarına göre ücret kesilir. Fiyatlandırma bilgileri, standart yük dengeleyici için Git [yük dengeleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/load-balancer/) sayfası.

Temel yük dengeleyici ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

Standart yük dengeleyici SLA hakkında daha fazla bilgi için Git [yük dengeleyici SLA](https://aka.ms/lbsla) sayfası. 

## <a name="limitations"></a>Sınırlamalar

- Yük Dengeleyici Yük Dengeleme ve bu belirli IP protokolleri için bağlantı noktası iletme için TCP veya UDP bir üründür.  Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP için desteklenen ve ICMP dahil olmak üzere diğer IP protokolleri için desteklenmiyor. Yük Dengeleyici sonlandırmak değil, yanıt veya aksi halde bir UDP veya TCP akışı yükü ile etkileşim. Bir proxy değil. Doğrulama başarılı bir ön uç bağlantısı olan bir Yük Dengeleme veya gelen NAT kuralı (TCP veya UDP) kullanılan aynı protokolü ile bant yer almalıdır _ve_ , sanal makineleriniz en az biri gerekir oluşturmak yanıt için bir istemci için bir ön uç bağlantı noktasından yanıt bakın.  Yük Dengeleyici ön uç bir bant dışı yanıt almadıktan hiçbir sanal makine yanıt verebilmesini gösterir.  Bir sanal makine yanıt verebilmesini olmadan bir yük dengeleyici ön etkileşimde mümkün değildir.  Bu durum giden bağlantılar için de geçerlidir nerede [bağlantı noktası maskeli SNAT](load-balancer-outbound-connections.md#snat) olduğu TCP ve UDP; için desteklenen yalnızca ICMP dahil olmak üzere diğer IP protokolleri de başarısız olacak.  Azaltmak için bir örnek düzeyinde ortak IP adresi atayın.
- Sağlayan ortak yük Dengeleyiciler aksine [giden bağlantılar](load-balancer-outbound-connections.md) sanal ağ içindeki özel IP adresleri için ortak IP adresleri geçiş, iç yük dengeleyici giden Çevir değil kaynaklanan ön uç için bir iç yük dengeleyici her ikisi de olarak özel IP adres alanı bağlantılardır.  Bu, burada çeviri gerekli değildir, benzersiz, iç IP adresi alanı içindeki SNAT Tükenme olası önler.  Bir çıkış akışı bir VM'den arka uç havuzundaki hangi havuzda bulunduğu iç yük dengeleyici ön uç akışına çalışırsa olan yan etkisi _ve_ eşlenmiş geri bu kendisini iki Bacak akışının eşleşmiyorsa ve akış başarısız olur.  Akış ön uç akışına oluşturulan arka uç havuzundaki aynı VM dön eşleyemiyorsanız akışı başarılı olur.   Akış geri kendisine eşler, giden akış için ön uç sanal makineden kaynaklanacak şekilde, karşılık gelen akış sanal makineden kendisine kaynaklanacak şekilde görüntülenir. Konuk işletim sistemlerine açısından bakıldığında, sanal makinenin içinde aynı akışın gelen ve giden bölümleri eşleşmiyor. TCP yığınına, kaynak ve hedef eşleşmeyen gibi aynı akışının parçası olacak şekilde bu yarıları aynı akışın tanımaz.  Akış için arka uç havuzundaki başka bir VM eşlendiği akış yarısının eşleşir ve VM akışına başarılı bir şekilde yanıt verebilir.  Bu senaryo için belirti aralıklı zaman aşımları olmasıdır. Bir üçüncü taraf proxy'nin arkasında iç yük ya da ekleme dahil (arka uç havuzları ilgili iç yük dengeleyici ön uç arka uç havuzundan akışlarına kaynaklanan) Bu senaryo güvenilir bir şekilde elde etmek için birkaç ortak geçici çözüm vardır Dengeleyici veya [DSR stil kurallarını kullanarak](load-balancer-multivip-overview.md).  Azaltmak için bir genel yük dengeleyiciye kullanabilirken, sonuçta elde edilen yatkın senaryodur [SNAT Tükenme](load-balancer-outbound-connections.md#snat) ve dikkatle yönetilen sürece kaçınılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure yük dengeleyici genel bir bakış vardır. Bir yük dengeleyici kullanmaya başlamak için bir tane oluşturun, sanal makineleri bir özel IIS uzantısı yüklendikten ve yük dengelemesi ile VM'ler arasındaki web uygulaması oluşturun. Bilgi edinmek için bkz [temel bir yük dengeleyici oluşturma](quickstart-create-basic-load-balancer-portal.md) hızlı başlangıç.
