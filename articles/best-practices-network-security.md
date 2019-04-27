---
title: Azure ağ güvenliği için en iyi uygulamalar | Microsoft Docs
description: Azure'da güvenli ağ ortamları oluşturmaya yardımcı olmak için kullanılabilir temel özellikleri öğrenin
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 74a46c7788b0a0dc729ab977489ed6d97a387a6e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60619568"
---
# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft bulut Hizmetleri ve ağ güvenliği
Microsoft bulut Hizmetleri, Hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve karma bağlantı için birçok seçenek sunar. Müşteriler, bu hizmetler Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile erişmek seçebilirsiniz. Microsoft Azure platformu, müşterilerin sorunsuz bir şekilde altyapılarını buluta genişletin ve çok katmanlı mimariler oluşturun olanak tanır. Ayrıca, üçüncü taraf güvenlik hizmetleri ve sanal gereçler sunarak Gelişmiş özellikleri etkinleştirebilirsiniz. Bu teknik incelemede, güvenlik ve müşteriler, Microsoft bulut hizmetlerine ExpressRoute aracılığıyla erişilen kullanırken dikkate almanız gereken mimari sorunları hakkında genel bir bakış sağlar. Ayrıca, Azure sanal ağları daha güvenli hizmetleri oluşturma kapsar.

## <a name="fast-start"></a>Hızlı Başlangıç
Aşağıdaki mantıksal grafiği, belirli bir örneği Azure platformu ile birçok güvenlik teknikleri yönlendirebilir. Hızlı başvuru için en iyi Örneğinize uyan örnek bulun. Genişletilmiş açıklamalar için özel olarak kağıt okumaya devam edin.
[![0]][0]

[Örnek 1: Ağ güvenlik grupları (Nsg'ler) ile uygulamalarınızı korumanıza yardımcı olmak için bir çevre ağındaki (DMZ, sivil bölge veya denetimli alt ağ olarak da bilinir) oluşturun.](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Örnek 2: Bir güvenlik duvarı ve Nsg'ler ile uygulamalarınızı korumanıza yardımcı olmak için bir çevre ağı oluşturun.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Örnek 3: Ağ güvenlik duvarı, kullanıcı tanımlı yol (UDR) ve NSG ile korumaya yardımcı olmak için bir çevre ağı oluşturun.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Örnek 4: Bir siteden siteye, sanal gereç sanal özel ağ (VPN) ile karma bağlantı ekleyin.](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Örnek 5: Bir site siteye Azure VPN ağ geçidi ile karma bağlantı ekleyin.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Örnek 6: ExpressRoute ile karma bağlantı ekleyin.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Örnekler arasında sanal ağlar, yüksek kullanılabilirlik ve hizmet zinciri bağlantılar eklemek için önümüzdeki birkaç ay içinde bu belgeye eklenir.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft uyumluluk ve altyapıyı koruma
Koleksiyon ve bireylerin verilerin kullanımını yöneten Ulusal, Bölgesel ve sektöre özgü gereksinimleriyle uyumlu hızlandırmasına yardımcı olmak için 40'tan fazla sertifikaları ve karşıladığımızı Microsoft sunar. En kapsamlı bulut hizmeti sağlayıcıları.

Daha fazla bilgi için uyumluluk bilgilerini bakın [Microsoft Trust Center][TrustCenter].

Microsoft bulut altyapısı, küresel ölçekli hizmetler çalıştırmak için gereken korumak için kapsamlı bir yaklaşım vardır. Microsoft bulut altyapısı, donanım, yazılım, ağlar, içerir ve yönetim ve operasyon personeli, fiziksel veri merkezlerinin yanı sıra.

![2]

Bu yaklaşım, müşterilerin Microsoft bulut hizmetlerini dağıtmak daha güvenli bir temel sunar. Sonraki adım, tasarım ve bu hizmetleri korumak için bir güvenlik mimarisi oluşturmak müşterilere yöneliktir.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Geleneksel güvenlik mimarileri ve Çevre ağları
Microsoft bulut altyapısını koruma konusunda yoğun yatırım alışkanlıklarını olsa da, müşterileri de kendi bulut Hizmetleri ve kaynak gruplarını korumanız gerekir. Çok katmanlı bir güvenlik yaklaşımı, en iyi savunma sağlar. Bir çevre ağ güvenlik bölgesi iç ağ kaynaklarına, güvenilmeyen bir ağdan korur. Bir çevre ağına kenarları veya Internet ve korumalı kurumsal BT altyapısı arasında sit ağ kısımlarını gösterir.

Tipik kurumsal ağlarda, çekirdek altyapının yoğun olarak birden çok katmanlı güvenlik cihazların duvarlar en fortified. Her katmanın sınır, cihazlar ve ilke zorlama noktaları oluşur. Her katman aşağıdaki ağ güvenlik cihazları bir birleşimini içerebilir: güvenlik duvarları, hizmet reddi (DoS) önleme, izinsiz giriş algılama veya koruma sistemlerini (IDs/IPS) ve VPN cihazları. İlke zorlaması güvenlik duvarı ilkeleri, erişim denetim listeleri (ACL'ler) veya belirli yönlendirme biçiminde. İlk satır doğrudan Internet'ten gelen trafiği kabul ağ savunma meşru istekler daha fazla ağa sağlarken blok saldırıların ve zararlı trafiği bu mekanizmalar bir birleşiminden oluşur. Çevre ağındaki kaynaklara doğrudan bu trafiği yönlendirir. Bu kaynak ardından "derin doğrulama sonraki sınırlarını i yapılandırmamız ağdaki kaynakları ilk konuşabilir". Bu ağın parçası, her iki tarafında koruma çeşit ile genellikle internet açığa çıkarıldığı en dıştaki katman çevre ağı adı verilir. Aşağıdaki şekilde, iki güvenlik sınırları ile bir kurumsal ağda tek alt ağ çevre ağı örneği gösterilmektedir.

![3]

Bir çevre ağına uygulamak için kullanılan birçok yapıdan vardır. Bu mimarileri, trafiği engellemek için her sınırında çeşitli mekanizmalar ile birden çok alt çevre ağına bir basit yük dengeleyiciden aralığı ve derin katmanları şirket ağının koruyun. Çevre ağı nasıl oluşturulduğunu, kuruluş ve genel kendi risk toleransınıza belirli gereksinimlerine bağlıdır.

Müşteriler, iş yüklerini genel bulutlara taşırken, uyumluluk ve güvenlik gereksinimlerini karşılamak için azure'da çevre ağı mimarisi için benzer özellikleri desteklemek için önemlidir. Bu belge, müşterilerin Azure'nın bir güvenli ağ ortamında nasıl oluşturabileceğinizi yönergeleri sağlar. Çevre ağı üzerinde odaklanmaktadır, ancak Ayrıca ağ güvenliği birçok yönden kapsamlı hakkında ayrıntılı bilgi içerir. Bu tartışma aşağıdaki soruları bildirmek:

* Nasıl Azure bir çevre ağında oluşturulabilir mi?
* Çevre ağı oluşturmak kullanılabilen Azure özelliklerinden bazıları nelerdir?
* Arka uç iş yüklerini nasıl korunabilir?
* İş yüklerini azure'da Internet iletişimi kontrol nasıl?
* Nasıl dağıtımlarından azure'da şirket içi ağlar korunabilir?
* Ne zaman yerel Azure güvenlik özellikleri, üçüncü taraf ağ Gereçleri veya hizmetler ile karşılaştırması kullanılmalıdır?

Aşağıdaki diyagramda, çeşitli Azure müşterilerine yönelik güvenlik katmanları gösterilmektedir. Bu katmanlar, hem yerel, Azure platformunda hem de müşteri tarafından tanımlanan özellikler şunlardır:

![4]

Azure DDoS büyük ölçekli Azure saldırılara karşı korumaya yardımcı olur Internet'ten gelen. Hangi trafik bulut hizmeti aracılığıyla sanal ağa geçirebilirsiniz belirlemek için kullanılan müşteri tarafından tanımlanan genel IP adresleri (Bitiş) sonraki katmanıdır. Yerel Azure sanal tam bir yalıtım diğer tüm ağlardan ağ yalıtımı sağlar ve trafik yalnızca kullanıcı tarafından yapılandırılmış yolları ve yöntemleri akar. Bu yolları ve yöntemleri sonraki katmanı burada Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak için güvenlik sınırları oluşturmak için kullanılabilir olan.

Sonraki bölümde, Azure sanal ağlarına genel bir bakış sağlar. Bu sanal ağları müşteriler tarafından oluşturulan ve dağıtılan iş yüklerini ne bağlıyken olan. Sanal ağlar, bir çevre ağındaki müşteri dağıtımları azure'da korumak için'kurmak için gereken tüm ağ güvenlik özelliklerinin temelidir.

## <a name="overview-of-azure-virtual-networks"></a>Azure sanal ağlarına genel bakış
Azure sanal ağlarına Internet trafiğini sağlayabilmek için önce vardır iki güvenlik katmanları Azure platformuna devralınan:

1.    **DDoS koruması**: DDoS koruması, Azure platformu büyük ölçekli Internet tabanlı saldırılara karşı koruyan Azure ağının fiziksel katmanıdır. Bu tür saldırıları birden çok "bot" düğüm Internet hizmet doldurmak için olarak kullanın. Azure, tüm gelen, giden ve Azure çapraz bölge bağlantısı üzerinde sağlam bir DDoS koruma kafes sahiptir. Bu DDoS koruma katmanı kullanıcı yapılandırılabilir özniteliklere sahip ve müşteri için erişilebilir değil. Büyük ölçekli saldırılara karşı bir platform olarak Azure DDoS koruması katmanı korur, dışarı ilişkili ve çapraz Azure bölgesi trafiği de izler. VNet üzerinde ağ sanal Gereçleri kullanarak ek dayanıklılık katmanı platform düzeyi korumayı geçirmek değil daha küçük bir ölçek saldırılara karşı müşteri tarafından yapılandırılabilir. Bir DDoS örnek uygulamada; internet'e yönelik IP adresi tarafından büyük ölçekli bir DDoS saldırısının gerçekleşirse, Azure kaynaklarını saldırıları algılamak ve amaçlanan hedeflerine ulaştı önce sorunlu trafiği kaydırın. Hemen hemen tüm durumlarda, saldırının Saldırıya uğrayan uç nokta etkilenmez. Bir uç nokta etkilenir nadir durumlarda trafik diğer uç noktalar yalnızca Saldırıya uğrayan uç nokta etkilenir. Bu nedenle diğer müşteriler ve hizmetlerin herhangi bir etkisi, saldırılara karşı görür. Azure DDoS büyük ölçekli saldırıları için yalnızca arıyor dikkat edin önemlidir. Platform düzeyinde koruma eşikler aşıldığında önce belirli hizmetinizi aşırı yüklenebilir, mümkündür. Örneğin, tek bir A0 IIS sunucusunda bir web sitesi alınması çevrimdışı bir DDoS saldırısının tarafından Azure platform düzeyi DDoS koruması, tehdit kayıtlı önce.

2.  **Genel IP adresleri**: Genel IP adresleri (hizmet uç noktaları, genel IP adresleri, uygulama ağ geçidi ve genel bir IP adresi, kaynağa yönlendirilen İnternet'e sunmak diğer Azure özellikleri aracılığıyla etkin) bulut Hizmetleri veya kaynak grupları, genel Internet IP izin ver adresler ve bağlantı noktasının. Uç nokta, iç adresi ve bağlantı noktası Azure sanal ağındaki trafiği yönlendirmek için ağ adresi çevirisi (NAT) kullanır. Bu yol, sanal ağa geçirmeniz dış trafiği için birincil yoludur. Genel IP adresleri hangi trafiğin geçirilir ve nasıl ve nerede sanal ağı açın çevrilir belirlemek için yapılandırılabilir.

Sanal ağ trafiği ulaştığında, oyuna gelen birçok özellik vardır. Azure sanal ağları, müşterilerin iş yüklerini ve burada temel ağ düzeyinde güvenlikle geçerlidir eklemek temelidir. Bir özel ağ (sanal ağ kaplama), aşağıdaki özellikleri ve özelliklere sahip müşteriler için Azure üzerinde kullanılabilir:

* **Trafik yalıtımı**: Bir sanal ağ, Azure platformunda trafik yalıtımı sınırıdır. Her iki sanal ağ aynı müşteri tarafından oluşturulmamış olsa bile sanal makineleri (VM'ler) bir sanal ağdaki farklı bir sanal ağ içindeki VM'ler için doğrudan iletişim kuramaz. Yalıtım müşteri Vm'leri sağlar önemli bir özelliktir ve bir sanal ağ içindeki özel iletişimin kalır.

>[!NOTE]
>Trafik yalıtımı başvuruyor trafiği yalnızca *gelen* sanal ağa. Varsayılan olarak sanal ağdan giden trafiği İnternet'e izin verilir, ancak Nsg'ler tarafından istenirse engellenebilir.
>
>

* **Çok katmanlı topoloji**: Sanal ağ alt ağları ayırmak ve farklı öğeleri veya iş yüklerini "katmanları" için ayrı bir adres alanları belirleme tarafından çok katmanlı topoloji tanımlamalarına olanak. Bu mantıksal gruplandırmaları topolojileri müşterilerin, iş yükü türlerine göre farklı erişim ilkesini tanımlamak ve ayrıca Katmanlar arasındaki trafik akışı denetler.
* **Şirketler arası bağlantı**: Müşteriler, Azure'da bir sanal ağ ve birden çok şirket içi sitelere veya diğer sanal ağlar arasında şirketler arası bağlantı kurabilirsiniz. Bir bağlantı oluşturmak için müşterilerin, VNet eşlemesi, Azure VPN ağ geçitleri, üçüncü taraf ağ sanal Gereçleri veya ExpressRoute kullanabilirsiniz. Azure standart IPSec/IKE protokolü ve özel ExpressRoute bağlantısı kullanarak siteden siteye (S2S) VPN destekler.
* **NSG** kuralları (ACL'ler) oluşturmak istediğiniz ayrıntı düzeyinde müşterileriniz: ağ arabirimleri, tek tek sanal makineleri veya sanal alt ağlar. Müşteriler, erişimine izin verme veya reddetme iş yükleri bir sanal ağdan şirket içi bağlantılar aracılığıyla müşterinin ağlarındaki sistemleri arasındaki iletişim tarafından erişimi denetlemek veya Internet iletişimi doğrudan.
* **UDR** ve **IP iletimi** müşterilerin sanal ağ içindeki farklı Katmanlar arasındaki iletişim yollarını tanımlamak. Müşteriler, bir güvenlik duvarı, IDs/IPS ve diğer sanal gereçlerini dağıtma ve güvenlik sınırı ilke zorlaması, denetimi ve inceleme için bu güvenlik Gereçleri üzerinden ağ trafiğini yönlendirir.
* **Ağ sanal Gereçleri** Azure Marketi'nde: Güvenlik duvarlarını ve yük Dengeleyiciler IDs/IPS gibi güvenlik Gereçleri, Azure Marketi'nde ve VM görüntü Galerisi kullanılabilir. Müşteriler, çok katmanlı bir güvenli ağ ortamı tamamlamak için bu cihazları sanal ağlarına ve özellikle (çevre ağ alt ağları dahil) kendi güvenlik sınırları dağıtabilir.

Bu özellikleri ve yetenekleri, bir çevre ağı mimarisi Azure'da nasıl oluşturulabilir, bir örnek, aşağıdaki diyagramda verilmiştir:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Çevre ağ özellikleri ve gereksinimleri
Çevre ağına ağ iletişimi Internet'ten doğrudan arabirim ön uçtur. Gelen paket güvenlik duvarı, Kimlikleri ve IPS gibi güvenlik Gereçleri üzerinden arka uç sunucularına ulaşması önce akış. İnternet'e bağlı iş yüklerini paketleri ayrıca ilke zorlaması, inceleme ve denetim amacıyla, çevre ağındaki güvenlik Gereçleri üzerinden ağ ayrılmadan önce akabilir. Ayrıca, çevre ağındaki müşteri sanal ağlar ve şirket içi ağlar arasında şirketler arası VPN gateway'leri barındırabilirsiniz.

### <a name="perimeter-network-characteristics"></a>Çevre ağ özellikleri
Yukarıdaki şekilde başvuran, bazı iyi çevre ağ özelliklerini aşağıda belirtilmiştir:

* Internet'e yönelik:
  * Çevre ağ alt doğrudan Internet ile iletişim kurulurken İnternete, ' dir.
  * Genel IP adresleri, VIP ve/veya hizmet uç noktaları ön uç ağı ve cihazlar Internet trafiği geçirin.
  * Internet'ten gelen trafiği ön uç ağ üzerindeki diğer kaynaklara önce güvenlik cihazlar üzerinden geçirir.
  * Giden güvenlik etkinse, trafiği güvenlik cihazları aracılığıyla İnternet'e geçirilmeden önce son adım olarak geçirir.
* Korumalı ağ:
  * Çekirdek altyapısı için Internet'ten doğrudan yolu yoktur.
  * Çekirdek altyapısı kanallara Nsg'ler, güvenlik duvarları veya VPN cihazları gibi güvenlik cihazları aracılığıyla geçirmeleri gerekir.
  * Diğer cihazlar Internet ve çekirdek altyapısı köprü oluşturması gerekir değil.
  * Internet'e yönelik hem çevre ağına (önceki şekilde gösterildiği gibi iki güvenlik duvarı simgeleri) sınırlarını'e yönelik korumalı ağ güvenlik cihazlarda gerçekten fark yaratan kuralları veya arabirimleri ile tek bir sanal gereç olabilir Her sınır için. Her iki sınırlar çevre ağ için bir yükü işlemek mantıksal olarak ayrılmış, örneğin, bir fiziksel cihaz.
* Diğer genel yöntemler ve sınırlamalar:
  * İş yükleri, iş kritik bilgileri depolamamayı gerekir.
  * Erişim ve güncelleştirmeleri çevre ağ yapılandırmalara ve dağıtımlara, yalnızca yetkili yöneticiler sınırlıdır.

### <a name="perimeter-network-requirements"></a>Çevre ağ gereksinimleri
Bu özellikleri etkinleştirmek için sanal ağ gereksinimlerine başarılı çevre ağına uygulamak için aşağıdaki yönergeleri izleyin:

* **Alt ağ mimarisi:** Sanal ağ, alt ağın tamamını, aynı sanal ağdaki diğer alt ağlara ayrılmıştır, bir çevre ağı olarak ayrılmış şekilde belirtin. Bu ayrım çevre ağı ve güvenlik duvarı veya IDs/IPS sanal gereç aracılığıyla diğer iç veya özel alt katmanları akışları arasındaki trafik sağlar.  Kullanıcı tanımlı yollar sınır alt ağlardaki bu trafiği sanal gerece iletmektir gerekir.
* **NSG:** Çevre ağ alt Internet ile iletişim sağlamak için açık olmalıdır, ancak, müşterilerin Nsg'ler atlama gelmez. Internet'e açık ağ yüzey en aza indirmek için genel güvenlik uygulamalarını takip edin. Dağıtımları veya özel uygulama protokolleri açık olan bağlantı noktalarını ve erişim izni uzak adres aralıkları kilitleyin. Bir tam kilitleme mümkün değildir durumlar olabilir. Örneğin, müşterilerin Azure üzerinde bir dış Web sitesi varsa, çevre ağındaki tüm genel IP adreslerinden gelen web isteklerini izin vermesi gerekir ancak yalnızca web uygulaması bağlantı noktalarını açmanız gerekir: Bağlantı noktası 80 üzerinde TCP'ye ve/veya TCP bağlantı noktası 443 üzerinden.
* **Yönlendirme tablosu:** Çevre ağ alt kendisi, Internet'e doğrudan iletişim kurabilmesi, ancak bir güvenlik duvarı veya güvenlik Gereci geçmeden doğrudan arka uç veya şirket içi ağlara gelen ve giden iletişimin izin vermemelidir.
* **Güvenlik Gereci yapılandırması:** Yönlendirme ve çevre ağı ve korumalı ağ, güvenlik duvarı, Kimlikleri ve IPS gibi güvenlik Gereçleri geri kalanı arasındaki paketlerde incelemek için birden çok girişli cihazlar olabilir. Çevre ağı ve arka uç alt ağları için ayrı NIC'ler olabilir. Doğrudan ve karşılık gelen Nsg'leri ve çevre ağ yönlendirme tablosu ile Internet'ten çevre ağdaki bir NIC iletişim. Arka uç alt ağlarına bağlanan NIC'ler daha Nsg'leri ve karşılık gelen arka uç alt yönlendirme tablolarını kısıtlı sahip.
* **Güvenlik Gereci işlevi:** Genellikle çevre ağında dağıtılan güvenlik Gereçleri aşağıdaki işlevleri gerçekleştirir:
  * Güvenlik Duvarı: Güvenlik duvarı kuralları veya erişim denetimi ilkeleri gelen istekler için zorlama.
  * Tehdit algılama ve önleme: Algılama ve Internet'ten kötü amaçlı saldırıları azaltma.
  * Denetim ve günlüğe kaydetme: Denetim ve çözümleme için ayrıntılı günlük tutma.
  * Ters proxy: Gelen istekleri karşılık gelen arka uç sunucuları için yönlendirme. Bu yeniden yönlendirmeyi hedef adres ön uç cihazlarda çevirmek genellikle, arka uç sunucu adresleri için güvenlik duvarları ve eşleme ile ilgilidir.
  * İletim proxy'si: NAT sağlama ve sanal ağ içinde başlatıldığı için Internet iletişimi için denetimi gerçekleştirme.
  * Yönlendirici: Sanal ağ içinde gelen ve alt ağlar arasında trafiği yönlendirme.
  * VPN cihazı: Müşterinin şirket içi ağlar ve Azure sanal ağları arasında şirketler arası VPN bağlantısı için şirketler arası VPN ağ geçitleri olarak hareket etme.
  * VPN sunucusu: Azure sanal ağlarına bağlanan VPN istemcileri kabul etme.

> [!TIP]
> Aşağıdaki iki gruba birbirinden ayırmaya: çevre ağ güvenlik dişli ve uygulama geliştirme, dağıtım veya işlemleri yönetici olarak yetkilendirilmiş kişiler erişim yetkisi kişiler. Bu grupları ayrı tutmak bir için görev ayrımı sağlar ve tek bir kişi hem uygulamalar güvenlik ve ağ güvenlik denetimlerini atlamasını engeller.
>
>

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Ağ sınırları oluştururken sorulan sorular
Bu bölümde, özel olarak belirtilmediği sürece, bir abonelik Yöneticisi tarafından oluşturulan özel Azure sanal ağlarına vadeli "ağları" gösterir. Terim, azure'da temel alınan fiziksel ağlara anlamına gelmez.

Ayrıca, Azure sanal ağları, genellikle geleneksel şirket içi ağların kapsamını genişletmek için kullanılır. Siteden siteye veya ExpressRoute hibrit ağ çözümleri çevre ağı mimarileri birleştirmek mümkündür. Bu karma bağlantı, ağ güvenlik sınırları oluşturmanın önemli bir noktadır.

Aşağıdaki üç soruya bir çevre ağına ve birden çok güvenlik sınırları olan bir ağ oluştururken yanıtlamak için kritik öneme sahiptir.

#### <a name="1-how-many-boundaries-are-needed"></a>(1) kaç tane sınırları gerekli mi?
İlk karar noktası kaç güvenlik sınırları belirtilen bir senaryoda gereklidir karar verilmiştir:

* Tek bir sınır: Bir sanal ağ ve Internet arasında ön uç çevre ağındaki.
* İki sınırları: Bir çevre ağı ve başka bir Internet tarafındaki çevre ağ alt ağı ve arka uç alt ağda Azure sanal ağları arasında.
* Üç sınırları: Bir çevre ağına Internet tarafındaki bir çevre ağına hem de arka uç alt ağları arasında ve bir arka uç alt ağları ile şirket içi ağ arasında.
* N sınırları: Değişken bir sayı. Güvenlik gereksinimlerine bağlı olarak, belirli bir ağa uygulanan güvenlik sınırları sayısı sınırı yoktur.

Sayısı ve türü farklılık gerekli sınırları şirketin risk toleransı ve uygulanmakta olan belirli bir senaryoya bağlıdır. Bu karar genellikle genellikle bir risk ve uyumluluk takım, bir ağ ve platformu ekibinden ve bir uygulama geliştirme ekipleri dahil olmak üzere, bir kuruluştaki birden fazla grubu ile birlikte kurulur. Bilgi Bankası güvenlik, ilgili verileri ve kullanılmakta olan teknolojileri kişilerle bir say her uygulama için uygun güvenlik tutum sergilemek emin olmak için bu kararı olması gerekir.

> [!TIP]
> En küçük sayısı belirli bir durumda güvenlik gereksinimlerini karşılayan sınırlarını kullanın. Daha fazla sınırlarıyla ve sorun giderme işlemlerinin daha zor olabilir zaman içinde birden fazla sınır ilkeleri yönetmede söz konusu yönetim yükünü yanı sıra. Ancak, yetersiz sınırları riskini artırır. Denge sağlama kritik öneme sahiptir.
>
>

![6]

Önceki şekilde üç güvenlik sınırı Ağ üst düzey bir görünümünü gösterir. Sınırları, çevre ağı ve Internet, Azure ön uç ve arka uç özel alt ağlar ve Azure arka uç alt ağı ve şirket içi kurumsal ağ arasında değildir.

#### <a name="2-where-are-the-boundaries-located"></a>(2) sınırları nerede bulunuyor?
Sınırları sayısını karar sonra bunları uygulama sonraki karar noktası yerdir. Genellikle üç seçeneğiniz vardır:

* Bir Internet tabanlı Aracı hizmeti (Bu belgede ele alınmayan gibi bir bulut tabanlı Web uygulaması güvenlik duvarı) kullanma
* Azure'da yerel özellikleri ve/veya ağ sanal Gereçleri kullanma
* Şirket içi ağ üzerinde fiziksel cihazları kullanma

Tamamen Azure ağda, yerel Azure özellikleri (örneğin, Azure yük dengeleyicileri) veya ağ sanal gereçlerini zengin iş ortağı ekosistemi Azure (örneğin, denetim noktası güvenlik duvarları) seçenekleri bulunmaktadır.

Bir sınır, Azure ve şirket içi ağ arasında gerekli olursa, güvenlik cihazları ya da bağlantı (veya her iki tarafında da) tarafında bulunabilir. Bu nedenle güvenlik dişli yerleştirmek için konuma göre bir karar yapılmalıdır.

Önceki şekilde, Internet çevre ağı ve ön için arka uç sınırları tamamen Azure içinde yer alır ve yerel Azure özellikleri ya da ağ sanal Gereçleri olması gerekir. Azure (arka uç alt ağ) ve şirket ağı arasında bir sınır güvenlik cihazlarda Azure tarafında veya şirket içi yan veya her iki tarafında da cihazlarda bile bir birleşimi olabilir. Önemli avantajları ve dezavantajları ciddi dikkate alınması gereken iki seçenekten birini olabilir.

Örneğin, var olan fiziksel güvenlik dişli kullanarak şirket içi ağ tarafta hiçbir yeni dişli gereklidir avantajına sahiptir. Yalnızca, yeniden yapılandırılması gerekir. Olumsuz yönüyse, ancak tüm trafiği geri Azure'dan şirket içi ağa güvenlik dişli tarafından görülebilmesi için gerekir, geliyor. Azure'dan Azure'a trafiği önemli bir gecikme dolayısıyla tabi olabileceğiniz ve güvenlik ilkesi zorlaması için şirket içi ağınıza geri zorlandı, uygulama performansını etkiler ve kullanıcı, deneyimi.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) sınırları nasıl uygulanır?
Her bir güvenlik sınırı, büyük olasılıkla farklı özelliği gereksinimlerini (örneğin, Kimlikleri ve güvenlik duvarı kuralları çevre ağına, ancak yalnızca ACL'leri arka uç alt ağı ve çevre ağı arasında Internet tarafındaki) sahip olur. Karar üzerinde kullanılacak cihaz (veya cihaz sayısını) senaryo ve güvenlik gereksinimlerine bağlıdır. Aşağıdaki bölümde, örnek 1, 2 ve 3 kullanılabilecek bazı seçenekler açıklanmaktadır. Azure yerel ağ özellikleri ve iş ortağı ekosisteminden Azure'da kullanıma sunulan cihazları gözden geçirme, neredeyse tüm senaryoyu çözmek için kullanılabilir çok seçenekleri gösterir.

Azure ile şirket içi ağınıza bağlama başka bir önemli uygulama karar noktası var. Azure sanal ağ geçidi veya bir ağ sanal Gereci kullanmalısınız? Bu seçenekler (örnekler 4, 5 ve 6) aşağıdaki bölümünde daha ayrıntılı ele alınmıştır.

Ayrıca, Azure içindeki sanal ağlar arasındaki trafiği gerekli olabilir. Bu senaryolar gelecekte eklenecektir.

Önceki soruların yanıtlarını öğrendikten sonra [Hızlı Başlat](#fast-start) bölümü hangi örnekleri belirli bir senaryo için en uygun olduğunu anlamasına yardımcı olabilir.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Örnekler: Azure sanal ağları ile güvenlik sınırları oluşturma
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Örnek 1 Nsg'ler ile uygulamalarınızı korumanıza yardımcı olmak için bir çevre ağı oluşturma
[Hızlı Başlangıç için geri](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example1]

[![7]][7]

#### <a name="environment-description"></a>Ortam tanımı
Bu örnekte, aşağıdaki kaynakları içeren bir aboneliği vardır:

- Tek bir kaynak grubu
- İki alt ağlı sanal ağ: "FrontEnd" ve "Arka uç"
- Her iki alt ağa uygulanan ağ güvenlik grubu
- Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows server
- Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
- Bir DNS sunucusu ("DNS01") temsil eden bir Windows server
- Uygulama web sunucusu ile ilişkili bir genel IP

Betikleri ve bir Azure Resource Manager şablonu için bkz: [ayrıntılı derleme yönergeleri][Example1].

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi.

> [!TIP]
> Genel olarak bakıldığında, "İzin ver" özel kurallarınızı ilk olarak, daha genel "Reddet" kuralları tarafından izlenen oluşturmanız gerekir. Verilen öncelik hangi kuralları önce değerlendirilir belirler. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. Herhangi bir yönde gelen veya giden (alt ağ perspektifinden) NSG kuralları uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kuralları için gelen trafiği oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir.
2. Herhangi bir sanal makine için RDP trafiğinin (3389 bağlantı noktası) Internet'ten izin verilir.
3. Web sunucusu (IIS01) İnternet'ten gelen HTTP trafiğine (bağlantı noktası 80) izin verilir.
4. Tüm trafiğe (tüm bağlantı noktaları) IIS01 AppVM1 için izin verilir.
5. Tüm trafik (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağ (her iki alt ağ) için reddedildi.
6. Tüm trafik (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağına reddedildi.

Bu kurallar ile hem kuralları 3 web sunucusuna Internet'ten gelen HTTP isteği ise, her alt ağa bağlı (izin ver) ve 5 (Reddet) uygular. Ancak, kural 3 daha yüksek bir önceliğe sahip olduğundan, yalnızca uygulanacak ve kural 5, oyuna duruma gelmemesine. Bu nedenle web sunucusuna HTTP isteği izin verilecek. Bu aynı trafiği DNS01 sunucunun erişmeye, 5 kural (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmek için izin verilecek değil. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafik kuralları 1 ve 4) Konuşmayı gelen ön uç alt ağına engeller. Ön uç web uygulamasında bir saldırganın etkilediğinde durumunda bu kural kümesi arka uç ağ korur. Saldırgan (yalnızca AppVM01 sunucu üzerinde kullanıma sunulan kaynakları için) arka uç "korumalı" Ağ erişimi sınırlı.

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir (örnek 3 bakın).

#### <a name="conclusion"></a>Sonuç
Bu örnekte, arka uç alt ağından gelen trafiği yalıtma, görece basit ve kolay bir yoludur. Daha fazla bilgi için [ayrıntılı derleme yönergeleri][Example1]. Bu yönergeler şunlardır:

* Bu çevre ağı Klasik PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu çevre ağı ile bir Azure Resource Manager şablonu oluşturmak nasıl.
* Her NSG komut ayrıntılı açıklamaları.
* Trafiğe izin verilir veya trafik reddedilir her katmanda nasıl gösteren, ayrıntılı trafik akış senaryoları.


### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Örnek 2, bir çevre ağına bir güvenlik duvarı ve Nsg'ler ile uygulamalarınızı korumanıza yardımcı olmak için derleme
[Hızlı Başlangıç için geri](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example2]

[![8]][8]

#### <a name="environment-description"></a>Ortam tanımı
Bu örnekte, aşağıdaki kaynakları içeren bir aboneliği vardır:

* Tek bir kaynak grubu
* İki alt ağlı sanal ağ: "FrontEnd" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Bu durumda bir güvenlik duvarı bir ağ sanal Gereci ön uç alt ağına bağlı
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows server
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server

Betikleri ve bir Azure Resource Manager şablonu için bkz: [ayrıntılı derleme yönergeleri][Example2].

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi.

> [!TIP]
> Genel olarak bakıldığında, "İzin ver" özel kurallarınızı ilk olarak, daha genel "Reddet" kuralları tarafından izlenen oluşturmanız gerekir. Verilen öncelik hangi kuralları önce değerlendirilir belirler. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. Herhangi bir yönde gelen veya giden (alt ağ perspektifinden) NSG kuralları uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kuralları için gelen trafiği oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir.
2. Herhangi bir sanal makine için RDP trafiğinin (3389 bağlantı noktası) Internet'ten izin verilir.
3. Ağ sanal gerecinin (Güvenlik Duvarı) tüm Internet trafiğini (tüm bağlantı noktaları) izin verilir.
4. Tüm trafiğe (tüm bağlantı noktaları) IIS01 AppVM1 için izin verilir.
5. Tüm trafik (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağ (her iki alt ağ) için reddedildi.
6. Tüm trafik (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağına reddedildi.

Bu kurallar ile bir HTTP isteği Internet'ten gelen güvenlik duvarı kuralları hem 3 ise, her alt ağa bağlı (izin ver) ve 5 (Reddet) uygular. Ancak, kural 3 daha yüksek bir önceliğe sahip olduğundan, yalnızca uygulanacak ve kural 5, oyuna duruma gelmemesine. Bu nedenle HTTP isteği için Güvenlik Duvarı izin verilir. Ön uç alt ağda olsa bile, aynı trafik IIS01 sunucunun ulaşmaya edebiliyordum, 5 kural (Reddet. geçerli), ve trafiği sunucuya geçirmek için izin verilecek değil. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafik kuralları 1 ve 4) Konuşmayı gelen ön uç alt ağına engeller. Ön uç web uygulamasında bir saldırganın etkilediğinde durumunda bu kural kümesi arka uç ağ korur. Saldırgan (yalnızca AppVM01 sunucu üzerinde kullanıma sunulan kaynakları için) arka uç "korumalı" Ağ erişimi sınırlı.

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir (örnek 3 bakın).

#### <a name="firewall-rule-description"></a>Güvenlik duvarı kural açıklaması
Güvenlik Duvarı'nı, kuralları iletme oluşturulmalıdır. Bu örnek yalnızca sınır Güvenlik Duvarı Internet trafiği yönlendirir ve ardından web sunucusu için yalnızca bir iletme ağ adresi çevirisi (NAT) kuralı gereklidir.

İletme kuralı İsabetleri HTTP (bağlantı noktası 80 veya 443 HTTPS için) erişmeye Güvenlik Duvarı tüm gelen kaynak adresi kabul eder. Bu güvenlik duvarının dışında yerel arabirimi gönderilen ve 10.0.1.5'i IP adresi ile web sunucusuna yönlendirildi.

#### <a name="conclusion"></a>Sonuç
Bu örnek, bir güvenlik duvarı ile uygulamanızı korumak ve arka uç alt ağından gelen trafiği yalıtma oldukça basit bir yoludur. Daha fazla bilgi için [ayrıntılı derleme yönergeleri][Example2]. Bu yönergeler şunlardır:

* Bu çevre ağı Klasik PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturmak nasıl.
* Her NSG komut ve güvenlik duvarı kuralı ayrıntılı açıklamaları.
* Trafiğe izin verilir veya trafik reddedilir her katmanda nasıl gösteren, ayrıntılı trafik akış senaryoları.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Örnek 3 ağları bir güvenlik duvarı, UDR ve NSG ile korumaya yardımcı olmak için bir çevre ağı oluşturma
[Hızlı Başlangıç için geri](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example3]

[![9]][9]

#### <a name="environment-description"></a>Ortam tanımı
Bu örnekte, aşağıdaki kaynakları içeren bir aboneliği vardır:

* Tek bir kaynak grubu
* Üç alt ağ ile sanal ağ: "SecNet", "Ön uç" ve "Arka uç"
* Bu durumda bir güvenlik duvarı bir ağ sanal Gereci SecNet alt ağa bağlı
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows server
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server

Betikleri ve bir Azure Resource Manager şablonu için bkz: [ayrıntılı derleme yönergeleri][Example3].

#### <a name="udr-description"></a>UDR açıklaması
Varsayılan olarak, aşağıdaki sistem yolları olarak tanımlanır:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal her zaman belirli bir ağ için sanal ağ oluşturan bir veya daha fazla tanımlanan adres ön ekleri olur (diğer bir deyişle, bu sanal ağdan sanal ağa bağlı olarak belirli bir sanal ağın nasıl tanımlandığını değiştirir). Kalan sistem yolları statiktir ve tabloda belirtildiği gibi varsayılan.

Bu örnekte, iki yönlendirme tablolarını oluşturulur, her biri ön uç ve arka uç alt ağları için. Her tabloda belirtilen alt ağ için uygun statik yollar ile yüklenir. Bu örnekte, her tablonun tüm trafiği (0.0.0.0/0) doğrudan üç yoldan güvenlik duvarı üzerinden vardır (bir sonraki atlama sanal gerecin IP adresi =):

1. Yerel alt ağ trafiğinin güvenlik duvarını geçmesine izin vermek için yerel alt ağ trafiği hiçbir sonraki atlama ile tanımlanır.
2. Bir sonraki güvenlik duvarı tanımlanan atlama ile sanal ağ trafiği. Bu sonraki atlama doğrudan yönlendirmek yerel sanal ağ trafiğine izin veren varsayılan kuralı geçersiz kılar.
3. Tüm kalan trafiği (0/0) bir sonraki güvenlik duvarı olarak tanımlanan atlama.

> [!TIP]
> Yerel alt giriş UDR sonları yerel alt iletişimde olmaması.
>
> * Bizim örneğimizde VNETLocal için işaret eden 10.0.1.0/24 büyük/küçük harf önemlidir! NVA gönderilecek şekilde bu olmadan, başka bir yerel sunucuya (örneğin) 10.0.1.25 hedefleyen Web sunucusu (10.0.1.4) bırakarak paket başarısız olur. NVA alt ağına gönderir ve alt ağ sonsuz bir döngü içinde nva'ya gönderecektir.
> * Yönlendirme döngüsü olasılığını genellikle Geleneksel, şirket içi cihazları olan ayrı alt ağlara bağlı birden çok NIC ile cihazları genellikle daha yüksektir.
>
>

Yönlendirme tabloları oluşturulduktan sonra alt ağa bağlanmalıdır. Bir kez oluşturulur ve alt ağ ile bağlı ön uç alt ağına yönlendirme tablosuna bu Çıktıyı gibi görünür:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR, ExpressRoute bağlantı hattına bağlı olduğu ağ geçidi alt ağına artık uygulanabilir.
>
> ExpressRoute veya siteden siteye ağ çevre ağınızda etkinleştirme örnekleri 3 ve 4 örnekler gösterilmektedir.
>
>

#### <a name="ip-forwarding-description"></a>IP iletimi açıklaması
IP iletimi bir UDR için yardımcı özelliğidir. IP iletimi özellikle gerecine gönderilmiş olan trafiği almasını ve ardından bu trafiğin ultimate hedefine iletecek izin veren bir sanal gereç üzerinde bir ayardır.

AppVM01 DNS01 sunucuya istekte, örneğin, UDR bu trafiğe güvenlik duvarı rota. Etkin IP iletimi ile DNS01 hedef (10.0.2.4) trafiği (10.0.0.4) Gereci tarafından kabul edilir ve ardından kendi (10.0.2.4) ultimate hedef iletilen. Güvenlik Duvarı'sonraki atlama olarak yol tablosuna sahip olsa da güvenlik duvarını etkin IP iletimi, trafiği gereç tarafından kabul edilebiliyordu değil. Bir sanal gereç kullanmak için UDR yanı sıra IP iletimini etkinleştirmeniz unutmamak önemlidir.

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu oluşturulan ve ardından tek bir kural ile yüklendi. Bu grup, ardından yalnızca ön uç ve arka uç alt ağlara (SecNet değil) bağlıdır. Aşağıdaki kural bildirimli olarak üretiliyor:

* Tüm trafiği (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağ (tüm alt ağlar) reddedildi.

Nsg'leri Bu örnekte kullanılan olsa da, ikincil bir katman el ile gerçekleştirilen bir yanlış yapılandırma karşı savunma ana amacı gibidir. Internet'ten ön uç veya arka uç alt ağları için tüm gelen trafiği engelle olmaktır. Trafik yalnızca SecNet alt ağ güvenlik duvarı aracılığıyla akış (ve uygunsa, ön uç veya arka uç alt ağları açın). Ayrıca, UDR kurallarıyla bir yerde ön uç veya arka uç alt ağına yaptı tüm trafik kullanıma Güvenlik Duvarı (UDR) sayesinde yönlendirilmesi. Güvenlik Duvarı bu trafiği asimetrik bir akış görür ve giden trafiği bırakmaya. Bu nedenle güvenlik alt ağlar koruma üç katmanı vardır:

* Herhangi bir ön uç veya arka uç NIC üzerinde genel IP adresi yok.
* Nsg'ler trafik Internet'ten engelleme.
* Güvenlik Duvarı bırakılıyor asimetrik trafiği.

Bu örnekte NSG ile ilgili bir ilgi çekici nokta güvenlik alt ağ da dahil olmak üzere tüm sanal ağ, Internet trafiği reddetmek için yalnızca bir kural içermesidir. NSG yalnızca ön uç ve arka uç alt ağa bağlı olduğundan, kuralın trafiğinde ancak işlenen değil güvenlik alt ağına gelen. Sonuç olarak, güvenlik alt ağa trafiği akar.

#### <a name="firewall-rules"></a>Güvenlik duvarı kuralları
Güvenlik Duvarı'nı, kuralları iletme oluşturulmalıdır. Güvenlik duvarı engelleme veya ileten tüm gelen, giden ve içi sanal ağ trafiğini olduğundan, çok sayıda güvenlik duvarı kuralları gerekir. Ayrıca, tüm gelen trafiği güvenlik duvarı tarafından işlenecek güvenlik hizmeti genel IP adresini (farklı bağlantı noktaları üzerinde) kadardır. Alt ağlar ' ayarlamadan önce mantıksal akışlarda diyagram için en iyi uygulamadır ve önlemek için güvenlik duvarı kuralları, daha sonra yeniden. Aşağıdaki şekil, bu örnek için güvenlik duvarı kurallarını mantıksal bir görünümüdür:

![10]

> [!NOTE]
> Ağ sanal kullanılan Gerecinde bağlı olarak, yönetim bağlantı noktalarına farklılık gösterir. Bu örnekte, bağlantı noktası 22 ve 801 807 kullanan bir Barracuda NextGen güvenlik duvarı başvuruluyor. Kullanılan cihaz yönetimi için kullanılan bağlantı noktaları bulmak için gereç satıcı belgelerine bakın.
>
>

#### <a name="firewall-rules-description"></a>Güvenlik duvarı kuralları açıklaması
Güvenlik Duvarı bu alt ağda yalnızca kaynak olduğundan, yukarıdaki mantıksal diyagramda, güvenlik alt gösterilmez. Diyagram güvenlik duvarı kuralları ve nasıl mantıksal olarak izin ver veya Reddet trafik akışını, ancak gerçek yönlendirilmiş yolu göstermez. Ayrıca, RDP trafiği için daha yüksek seçilen dış bağlantı noktaları aralıklı bağlantı noktaları (8014 – 8026) ve son iki sekizlik tabanda daha kolay okunabilmesi için yerel IP adresi ile gevşek hizalamak için seçilmiş olan (örneğin, yerel sunucu adresi 10.0.1.4 ile ilişkili dış bağlantı noktası 8014). Tüm yüksek çakışma olmayan bağlantı noktaları, ancak kullanılabilir.

Bu örnekte, kural yedi türleri ihtiyacımız:

* Dış kuralları (gelen trafik için):
  1. Güvenlik Duvarı Yönetimi kuralı: Bu uygulama yeniden yönlendirme kuralı trafiği ağ sanal Gereci yönetim bağlantı noktalarına geçirilecek sağlar.
  2. RDP kuralları (her Windows server için): Bu dört kural (her sunucu için bir tane) RDP aracılığıyla tek tek sunucuların yönetimini sağlar. Kullanılan ağ sanal gerecinin özelliklerine bağlı olarak bir kural içinde dört RDP kuralları da daraltılmış.
  3. Uygulama trafik kuralları: İki ön uç web trafiği için ilk ve ikinci (örneğin, veri katmanı için web sunucusu) arka uç trafiği için bu kuralların vardır. Bu kurallar yapılandırmasını (burada sunucularınızın yerleştirilir) ağ mimarisine bağlıdır ve trafik akışları (hangi yönde trafik akışı ve bağlantı noktalarını kullanılır).
     * İlk kural, uygulama tarafından sunucuya ulaşmak gerçek uygulama trafiğine izin verir. Güvenlik ve Yönetim için diğer kurallardan olanak tanırken, uygulama trafiği ne dış kullanıcıları veya hizmetler uygulamalara izin ver kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde bir tek bir web sunucusu yok. Bu nedenle bir tek uygulama güvenlik duvarını gelen trafiğe web sunucuları iç IP adresi için dış IP yönlendirir. Yeniden yönlendirilen trafik oturumu NAT iç sunucuya çevrilmesi.
     * İkinci web sunucusunun AppVM01 sunucu (ancak değil AppVM02) iletişim kurmasına izin verecek şekilde arka uç kuralı herhangi bir bağlantı kuralıdır.
* (İçi sanal ağ trafiği) iç kurallar
  1. Giden Internet kuralı için: Bu kural, seçili ağlar için geçirilecek herhangi bir ağa gelen trafiğe izin verir. Bu kural, genellikle varsayılan bir kural zaten güvenlik duvarı, ancak devre dışı durumuna içindir. Bu kural, bu örnek için etkinleştirilmesi gerekir.
  2. DNS kuralı: Bu kural yalnızca DNS sunucusuna geçirilecek DNS (bağlantı noktası 53) trafiğine izin verir. Bu ortam için ön uçtan arka uç çoğu trafik engellenir. Bu kural, tüm yerel alt ağından DNS özellikle sağlar.
  3. Alt ağ kuralı için alt ağı: Bu kural, ön uç alt ağına (ancak tersine) herhangi bir sunucuya bağlanmak için arka uç alt ağdaki herhangi bir sunucu sağlamaktır.
* (Önceki birini karşılamıyor trafiği için) emniyet kuralı:
  1. Tüm trafik kuralı Reddet: Bu reddetme kuralı her zaman son kuralı (açısından öncelik) olması gerekir ve bu nedenle önceki kurallardan herhangi birinin eşleştirilecek trafik akışı başarısız olursa, bu kural tarafından bırakılır. Bu kural varsayılan kural içindir ve genellikle yerinde ve etkin. Herhangi bir değişiklik, genellikle bu kural için gereklidir.

> [!TIP]
> İkinci uygulama trafiği kuralı üzerinde herhangi bir bağlantı noktası bu örneği basitleştirmek amacıyla kullanılabilir. Gerçek bir senaryoda, bu kural saldırı yüzeyini azaltmak için en belirgin bir bağlantı noktası ve adres aralıklarının kullanılmalıdır.
>
>

Önceki kural oluşturulduktan sonra trafiğe izin verilir veya trafik reddedilir istediğiniz gibi emin olmak için her bir kural önceliklerini gözden geçirilmesi önemlidir. Bu örnekte, öncelik sırasına göre kurallardır.

#### <a name="conclusion"></a>Sonuç
Daha karmaşık ancak bu örnekte, koruma ve daha önceki örneklerde ağ yalıtma şekilde tamamlayın. (Örnek 2 yalnızca uygulamayı korur ve örnek 1 yalnızca alt ağlar yalıtır). Bu tasarım için trafik her iki yönde izlenmesini sağlar ve yalnızca gelen uygulama sunucusu korur ancak bu ağda tüm sunucular için ağ güvenlik ilkesi uygular. Ayrıca, kullanılan Gereci bağlı olarak, tam trafiği denetleme ve farkındalık gerçekleştirilebilir. Daha fazla bilgi için [ayrıntılı derleme yönergeleri][Example3]. Bu yönergeler şunlardır:

* Bu örnek çevre ağı Klasik PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturmak nasıl.
* Ayrıntılı açıklamaları her UDR NSG komut ve güvenlik duvarı kuralı.
* Trafiğe izin verilir veya trafik reddedilir her katmanda nasıl gösteren, ayrıntılı trafik akış senaryoları.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Örnek 4 Ekle bir siteden siteye, sanal Gereci VPN ile karma bağlantı
[Hızlı Başlangıç için geri](#fast-start) | Derleme yönergeleri kullanılabilir olan en kısa sürede ayrıntılı

[![11]][11]

#### <a name="environment-description"></a>Ortam tanımı
Bir ağ sanal Gereci (NVA) kullanarak karma ağ 1, 2 veya 3 örneklerde açıklandığı çevre ağ türlerinden birini eklenebilir.

Önceki resimde gösterildiği gibi bir VPN bağlantısı (siteden siteye) Internet üzerinden bir Azure sanal ağı bir NVA aracılığıyla şirket içi ağa bağlanmak için kullanılır.

> [!NOTE]
> ExpressRoute, Azure genel eşdüzey hizmet sağlama seçeneği etkin kullanırsanız, bir statik rota oluşturulmalıdır. Bu statik rota, Kurumsal Internet ve ExpressRoute bağlantısı aracılığıyla değil NVA VPN IP adresine yönlendirmek. ExpressRoute, Azure ortak eşleme seçeneği gerekli NAT VPN oturumu bozabilir.
>
>

VPN yerleştirildikten sonra NVA tüm ağlar ve alt ağlar için merkezi hub haline gelir. Güvenlik Duvarı iletme kurallarını hangi trafik akışına izin belirlemek NAT çevrilir yönlendirilirsiniz veya (hatta için şirket içi ağınız ve Azure arasında trafik akışları) bırakılır.

İyileştirilebilir veya bağlı olarak söz konusu kullanım örneği bu tasarım deseni tarafından düşürülmüş olarak trafik akışları dikkatlice değerlendirilmelidir.

Örnek 3 yerleşik ortamı kullanarak ve ardından bir siteden siteye VPN karma ağ bağlantısı ekleyerek, aşağıdaki tasarım üretir:

[![12]][12]

Şirket içi yönlendiricinizi veya VPN için NVA ile uyumlu herhangi bir ağ cihazı VPN istemcisi olacaktır. Bu fiziksel cihazı başlatmak ve VPN bağlantısı, NVA ile koruma için sorumlu olacaktır.

Mantıksal olarak NVA'ya yönelik ağ dört ayrı "güvenlik bölgeleri" gibi kuralları bu bölgeler arasında trafiği birincil Direktörü olan NVA üzerindeki görünür:

![13]

#### <a name="conclusion"></a>Sonuç
Bir Azure sanal ağına siteden siteye VPN karma ağ bağlantı eklenmesi şirket içi ağınıza güvenli bir şekilde Azure'a genişletebilirsiniz. Trafiğiniz şifrelenir ve yönlendiren bir VPN bağlantısı kullanarak, Internet üzerinden. Bu örnekte NVA zorlamak ve güvenlik ilkesini yönetmek için merkezi bir konum sağlar. Daha fazla bilgi için (gelecek) yapı ayrıntılı yönergelere bakın. Bu yönergeler şunlardır:

* Bu örnek çevre ağı PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturmak nasıl.
* Bu tasarım trafiğin nasıl akacağını gösteren, ayrıntılı trafik akış senaryoları.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Örnek 5 site siteye Azure VPN ağ geçidi ile karma Bağlantı Ekle
[Hızlı Başlangıç için geri](#fast-start) | Derleme yönergeleri kullanılabilir olan en kısa sürede ayrıntılı

[![14]][14]

#### <a name="environment-description"></a>Ortam tanımı
1 veya 2. örnekte açıklanan ya da bir çevre ağ türü için bir Azure VPN ağ geçidi kullanarak hibrit ağ eklenebilir.

Önceki şekilde gösterildiği gibi bir VPN bağlantısı üzerinden İnternet'e (siteden siteye) bir Azure sanal ağı bir Azure VPN gateway aracılığıyla şirket içi ağa bağlanmak için kullanılır.

> [!NOTE]
> ExpressRoute, Azure genel eşdüzey hizmet sağlama seçeneği etkin kullanırsanız, bir statik rota oluşturulmalıdır. Bu statik rota NVA VPN IP adresi, Kurumsal Internet ve aracılığıyla değil WAN ExpressRoute için yönlendirme. ExpressRoute, Azure ortak eşleme seçeneği gerekli NAT VPN oturumu bozabilir.
>
>

Aşağıdaki şekilde, bu örnekte iki ağ kenarlar gösterilir. İlk edge üzerinde Azure içi ağlar ve Azure ile Internet arasında trafiği akışı NVA ve Nsg'ler denetler. İkinci edge, Azure VPN ağ geçidi, şirket içi ile Azure arasında bir ayrı ve yalıtılmış ağ Edge ' dir.

İyileştirilebilir veya bağlı olarak söz konusu kullanım örneği bu tasarım deseni tarafından düşürülmüş olarak trafik akışları dikkatlice değerlendirilmelidir.

Örnek 1 yerleşik ortamı kullanarak ve ardından bir siteden siteye VPN karma ağ bağlantısı ekleyerek, aşağıdaki tasarım üretir:

[![15]][15]

#### <a name="conclusion"></a>Sonuç
Bir Azure sanal ağına siteden siteye VPN karma ağ bağlantı eklenmesi şirket içi ağınıza güvenli bir şekilde Azure'a genişletebilirsiniz. Yerel Azure VPN ağ geçidi kullanarak trafiğinizi, IPSec şifrelenmiş ve üzerinden İnternet'e yönlendirir. Ayrıca, Azure VPN ağ geçidi kullanarak (hiçbir ek maliyet üçüncü taraf nva'ları gibi ile lisans) daha düşük maliyetli bir seçenek sağlayabilir. Bu seçenek hiçbir NVA kullanıldığı örnek 1, en ekonomik bir seçenektir. Daha fazla bilgi için (gelecek) yapı ayrıntılı yönergelere bakın. Bu yönergeler şunlardır:

* Bu örnek çevre ağı PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturmak nasıl.
* Bu tasarım trafiğin nasıl akacağını gösteren, ayrıntılı trafik akış senaryoları.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>ExpressRoute ile karma bağlantı örnek 6 ekleme
[Hızlı Başlangıç için geri](#fast-start) | Derleme yönergeleri kullanılabilir olan en kısa sürede ayrıntılı

[![16]][16]

#### <a name="environment-description"></a>Ortam tanımı
ExpressRoute özel eşlemesi bağlantısı kullanarak hibrit ağ çevre ağ türlerinden 1 veya 2. örnekte açıklanan eklenebilir.

Önceki şekilde gösterildiği gibi ExpressRoute özel eşlemesi, şirket içi ağınız ve Azure sanal ağı arasında doğrudan bir bağlantı sağlar. Yalnızca hizmet sağlayıcısı ağ ve hiçbir zaman İnternet'e dokunmak Microsoft Azure ağ trafiği transits.

> [!TIP]
> ExpressRoute kullanarak kurumsal ağ trafiğini Internet dışında tutar. ExpressRoute Sağlayıcısı'ndan için hizmet düzeyi sözleşmeleri de sağlar. 200 MB/sn ile siteden siteye VPN, Azure ağ geçidi en fazla aktarım hızı bilgileriyse Azure ağ geçidi, ExpressRoute ile 10 GB/sn kadar geçirebilirsiniz.
>
>

Aşağıdaki diyagramda görüldüğü gibi bu seçenekle ortamı iki ağ kenarlar artık sahiptir. Ağ geçidini bir şirket içi ile Azure arasında ayrı ve yalıtılmış ağ edge olsa da NVA ve NSG trafik akışları içi Azure ağları için ve Azure ile Internet arasında denetler.

İyileştirilebilir veya bağlı olarak söz konusu kullanım örneği bu tasarım deseni tarafından düşürülmüş olarak trafik akışları dikkatlice değerlendirilmelidir.

Örnek 1 yerleşik ortamı kullanarak ve ardından bir ExpressRoute hibrit ağ bağlantısı ekleyerek, aşağıdaki tasarım üretir:

[![17]][17]

#### <a name="conclusion"></a>Sonuç
Bir ExpressRoute özel eşlemesi ağ bağlantısı eklenmesi şirket içi ağ şekilde daha yüksek performanslı bir güvenli, daha düşük gecikme süresi, Azure'a genişletebilirsiniz. Ayrıca, bu örnekte olduğu gibi yerel Azure ağ geçidi kullanarak (üçüncü taraf nva'ları gibi ile lisanslama Hayır ek) daha düşük maliyetli bir seçenek sağlar. Daha fazla bilgi için (gelecek) yapı ayrıntılı yönergelere bakın. Bu yönergeler şunlardır:

* Bu örnek çevre ağı PowerShell betikleri ile nasıl geliştireceğinizi.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturmak nasıl.
* Bu tasarım trafiğin nasıl akacağını gösteren, ayrıntılı trafik akış senaryoları.

## <a name="references"></a>Başvurular
### <a name="helpful-websites-and-documentation"></a>Faydalı Web siteleri ve belgeler
* Azure Resource Manager ile Azure erişim:
* Azure PowerShell ile erişme: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Sanal ağ belgeleri: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Ağ güvenlik grubu belgelerinde: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/security-overview.md)
* Kullanıcı tanımlı yönlendirme belgeler: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Azure sanal ağ geçitleri: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Siteden siteye VPN: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* ExpressRoute belgeleri ("Başlarken" ve "Nasıl yapılır" bölümleri denetlediğinizden emin olun): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "güvenlik seçenekleri akış çizelgesi"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "azure güvenlik özellikleri"
[3]: ./media/best-practices-network-security/dmzcorporate.png "kurumsal ağ içinde bir DMZ"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "azure güvenlik mimarisi"
[5]: ./media/best-practices-network-security/dmzazure.png "Azure sanal ağında bir DMZ"
[6]: ./media/best-practices-network-security/dmzhybrid.png "üç güvenlik sınırları ile hibrit ağ"
[7]: ./media/best-practices-network-security/example1design.png "gelen NSG ile DMZ"
[8]: ./media/best-practices-network-security/example2design.png "gelen NVA ve NSG ile DMZ"
[9]: ./media/best-practices-network-security/example3design.png "UDR NVA ve NSG ile iki yönlü DMZ"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "mantıksal görünümünü güvenlik duvarı kuralları"
[11]: ./media/best-practices-network-security/example3designoptions.png "DMZ NVA ile bağlı hibrit ağ"
[12]: ./media/best-practices-network-security/example4designs2s.png "bir siteden siteye VPN kullanarak bağlanan NVA ile DMZ"
[13]: ./media/best-practices-network-security/example4networklogical.png "NVA açısından mantıksal ağ"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure ağ geçidi ile siteden siteye hibrit ağ bağlı"
[15]: ./media/best-practices-network-security/example5designs2s.png "Azure ağ geçidi kullanılarak siteden siteye VPN ile DMZ"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure ağ geçidi ile bağlı ExpressRoute hibrit ağ"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "ExpressRoute bağlantısı kullanarak Azure ağ geçidi ile DMZ"

<!--Link References-->
[TrustCenter]: https://azure.microsoft.com/support/trust-center/compliance/
[Example1]: ./virtual-network/virtual-networks-dmz-nsg.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
