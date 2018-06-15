---
title: En iyi güvenlik uygulamaları, Azure ağı | Microsoft Docs
description: Güvenli ağ ortamları oluşturmaya yardımcı olmak için mevcut anahtar özelliklerinden bazıları öğrenin
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
ms.openlocfilehash: cf015f4857a22b755813d0be1af5a55a8b7b6535
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360481"
---
# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft bulut Hizmetleri ve ağ güvenliği
Microsoft bulut hizmetlerine hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve karma bağlantı için birçok seçenek sunar. Müşteriler, Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile bu hizmetlere erişmek seçebilirsiniz. Microsoft Azure platformu sorunsuz bir şekilde altyapılarını bulutunu oluşturmak ve genişletmek için çok katmanlı mimarileri olanak tanır. Ayrıca, üçüncü tarafların güvenlik hizmetleri ve sanal gereçler sunarak Gelişmiş özellikleri etkinleştirebilirsiniz. Bu teknik incelemede güvenlik ve müşteriler, ExpressRoute aracılığıyla erişilebilir Microsoft bulut hizmetlerini kullanırken dikkate almanız gereken mimari sorunları genel bakış sağlar. Ayrıca, Azure sanal ağları daha güvenli Hizmetleri oluşturulması ele alınmaktadır.

## <a name="fast-start"></a>Hızlı Başlat
Aşağıdaki mantığı grafik Azure platformu ile çok sayıda güvenlik teknikleri belirli bir örneği için yönlendirebilirsiniz. Hızlı başvuru için en iyi durumunuz uyan örnek bulun. Genişletilmiş açıklamalar için kağıt okuma devam edin.
[![0]][0]

[Örnek 1: ağ güvenlik grupları (Nsg'ler) uygulamalarla korunmasına yardımcı olmak için bir çevre ağında (DMZ, sivil bölge veya denetimli alt ağ olarak da bilinir) oluşturun.](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Örnek 2: bir güvenlik duvarı ve Nsg'ler uygulamalarla korunmasına yardımcı olmak için bir çevre ağı oluşturun.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Örnek 3: bir güvenlik duvarı, kullanıcı tanımlı yönlendirme (UDR) ve NSG ağlarla korunmasına yardımcı olmak için bir çevre ağı oluşturun.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Örnek 4: bir siteden siteye, sanal Gereci sanal özel ağ (VPN) ile karma bağlantı ekleyin.](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Örnek 5: bir siteden siteye Azure VPN ağ geçidi ile karma bağlantı ekleyin.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Örnek 6: ExpressRoute ile karma bağlantı ekleyin.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Sanal ağlar, yüksek kullanılabilirlik ve hizmet zincirleme arasındaki bağlantıları ekleme örnekleri sonraki birkaç ay içinde bu belgeye eklenir.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft uyumluluk ve altyapısını koruma
Kuruluşlar koleksiyon ve bireylerin verilerin kullanımını yöneten Ulusal, Bölgesel ve sektöre özgü gereksinimlerine uymak yardımcı olmak için Microsoft 40'dan sertifikaları ve attestations sunar. Herhangi bir bulut hizmeti sağlayıcısına en kapsamlı kümesi.

Daha fazla bilgi için uyumluluk bilgileri bakın [Microsoft Trust Center][TrustCenter].

Microsoft bulut altyapısı hiper ölçekli küresel hizmetler çalıştırmak için gereken korumak için kapsamlı bir yaklaşım vardır. Microsoft bulut altyapısı içeren donanım, yazılım, ağlar ve yönetim ve fiziksel veri merkezleri yanı sıra belirli bir personelinin.

![2]

Bu yaklaşım, müşterilerin Microsoft bulut hizmetlerini dağıtmak daha güvenli bir temel sağlar. Tasarım ve bu hizmetleri korumak için bir güvenlik mimarisi oluşturmak, müşteriler için sonraki adımdır.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Geleneksel güvenlik mimarisi ve Çevre ağları
Bulut altyapısını koruma Microsoft yoğun invests rağmen müşterilerin da bulut hizmetlerinin ve kaynak grupları korumanız gerekir. Çok katmanlı bir yaklaşım güvenlik en iyi savunma sağlar. Bir çevre ağ güvenlik bölgesi iç ağ kaynaklarına güvenilmeyen bir ağdan korur. Bir çevre ağına kenarları ya da Internet ile korunan kurumsal BT altyapısı arasında sit ağ bölümlerini anlamına gelir.

Tipik kurumsal ağlarda, çekirdek altyapıyı yoğun olarak birden çok katmanlı güvenlik aygıtların çevreyi adresindeki fortified. Her katman sınır, aygıtları ve ilke zorlama noktaları oluşur. Her katman aşağıdaki ağ güvenlik aygıtları bir birleşimini içerebilir: güvenlik duvarları, hizmet reddi (DoS) önleme, izinsiz giriş algılama veya koruma sistemleri (Kimlikleri/IP) ve VPN aygıtları. İlke zorlama güvenlik duvarı ilkeleri, erişim denetim listelerini (ACL'ler) ya da belirli yönlendirme alabilir. İlk satırı doğrudan Internet'ten gelen trafiği kabul etmesini ağındaki savunma ağınıza daha fazla meşru istekler verirken bu mekanizmaların blok saldırılarına ve zararlı trafiği bir birleşiminden oluşur. Çevre ağındaki kaynaklarına doğrudan bu trafiğini yönlendirir. Bu kaynak sonra "derin doğrulama için sonraki sınır transiting ağındaki kaynaklara ilk konuşun". Bu ağın parçası genellikle her iki tarafında koruma çeşit ile Internet'e açık olduğu için dış katmanı çevre ağı adı verilir. Aşağıdaki şekilde, iki güvenlik sınırları ile bir şirket ağında tek alt ağ çevre ağına ilişkin bir örnek gösterilmektedir.

![3]

Bir çevre ağına uygulamak için kullanılan birçok mimarisi vardır. Bu mimari trafiği engellemek için her bir sınır, çeşitli mekanizmalar ile birden çok alt ağlı çevre ağına bir basit yük dengeleyiciden aralığı ve şirket ağının daha derin katmanları koruyun. Çevre ağı nasıl yapılandırıldığını ihtiyacınıza kuruluş ve genel kendi risk toleransınıza bağlıdır.

Müşteriler için genel Bulutlar, iş yüklerini ilerlerken, uyumluluk ve güvenlik gereksinimlerini karşılayacak şekilde Azure çevre ağ mimarisi için benzer özellikleri desteklemek için önemlidir. Bu belge, müşterilerin Azure güvenli ağ ortamında nasıl oluşturabileceğiniz yönergeleri sağlar. Çevre ağı üzerinde odaklanır, ancak aynı zamanda ağ güvenliği pek çok görünüşünün kapsamlı bir tartışma içerir. Bu tartışma aşağıdaki soruları bildirin:

* Nasıl bir çevre ağında Azure yerleştirilmiş olabilir?
* Çevre ağı oluşturmak kullanılabilen Azure özellikleri bazıları nelerdir?
* Arka uç iş yükleri nasıl korunabilir?
* Nasıl Internet iletişimi azure'da iş yüklerini kontrol edilir?
* Nasıl şirket içi ağlar Azure dağıtımlardan korunabilir?
* Ne zaman yerel Azure güvenlik özellikleri üçüncü taraf uygulamaları veya hizmetleri karşı kullanılsın mı?

Aşağıdaki diyagramda çeşitli müşteriler için Azure sağlayan güvenlik katmanları gösterilmektedir. Bu katmanlar, hem Azure platformu yerel hem de müşteri tanımlı özellikleri şunlardır:

![4]

Azure DDoS Azure karşı büyük ölçekli saldırılarına karşı korumaya yardımcı olur Internet'ten gelen. Hangi trafiğin bulut hizmeti aracılığıyla sanal ağa geçebilmesi belirlemek için kullanılan müşteri tanımlı genel IP adresleri (Bitiş) sonraki katmanıdır. Yerel Azure sanal ağ yalıtımı diğer ağlarla tam yalıtımı sağlar ve trafik yalnızca yapılandırılan kullanıcı yolları ve yöntemleri akar. Bu yolları ve yöntemleri sonraki katmanı nerede Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak için güvenlik sınırları oluşturmak için kullanılabilir durumdadır.

Sonraki bölümde, Azure sanal ağlar genel bir bakış sağlar. Bu sanal ağlar müşteriler tarafından oluşturulan ve dağıtılan yüklerini ne bağlanan olan. Sanal ağlar Azure müşteri dağıtımlarında korumak için bir çevre ağ kurmak için gerekli tüm ağ güvenlik özelliklerinin temelidir.

## <a name="overview-of-azure-virtual-networks"></a>Azure sanal ağlar genel bakış
Internet trafiği için Azure sanal ağlar sağlayabilmek için önce iki katmanı vardır güvenlik Azure platformu devralınmış:

1.    **DDoS koruması**: DDoS koruması olan bir Azure platformu büyük ölçekli Internet tabanlı saldırılara karşı korur Azure fiziksel ağ katmanı. Bu saldırıların Internet hizmeti doldurmaya girişiminde içinde birden çok "bot" düğümleri kullanır. Azure sağlam bir DDoS koruması kafes tüm gelen, giden ve çapraz Azure bölgesi bağlantı vardır. Bu DDoS koruma katmanı kullanıcı yapılandırılabilir özniteliklere sahip ve müşteriye erişilebilir durumda değil. Büyük ölçekli saldırılara karşı bir platform olarak Azure DDoS koruma katmanı korur, dışarı bağlı ve çapraz Azure bölgesini trafiği de izler. VNet üzerinde ağ sanal Gereçleri kullanarak ek Katmanlar esnek platform düzeyinde koruma trip olmayan küçük bir ölçek saldırılara karşı müşteri tarafından yapılandırılabilir. DDoS örneği eylem; internet'e yönelik IP adresi büyük ölçekli bir DDoS saldırılarına gerçekleşirse Azure kaynakları saldırıları algılamak ve hedeflenen hedefine ulaşıldı önce sorunlu trafiği kaydırın. Neredeyse her durumda, Saldırıya uğrayan bitiş noktası tarafından saldırı etkilenmez. Bir uç nokta etkilenir nadir durumlarda trafik diğer uç noktalar yalnızca Saldırıya uğrayan endpoint etkilenir. Bu nedenle diğer müşteriler ve Hizmetleri bu saldırılara karşı etkisi görür. Azure DDoS için büyük ölçekli saldırıları yalnızca arıyor dikkate almak önemlidir. Platform düzeyinde koruma eşikleri aşıldı önce belirli hizmetinizi çok mümkündür. Örneğin, tek bir A0 IIS sunucusunda bir web sitesi alınması çevrimdışı bir DDoS saldırılarına Azure platformu düzeyi DDoS koruma bir tehdit kayıtlı önce.

2.  **Genel IP adresleri**: genel IP adreslerinin (hizmet uç noktaları, genel IP adresleri, uygulama ağ geçidi ve bir ortak IP adresi kaynağınıza yönlendirilen internet sunmak diğer Azure özellikleri aracılığıyla etkin) bulut Hizmetleri veya kaynak izin ver ortak Internet IP adresleri ve bağlantı noktalarının açık olmasını Gruplar'ı tıklatın. Uç nokta iç adresi ve bağlantı noktası Azure sanal ağı üzerinde trafiği yönlendirmek için ağ adresi çevirisi (NAT) kullanır. Bu yol, sanal ağa geçirmek dış trafiği için birincil yoludur. Genel IP adresleri hangi trafik geçirilen ve nasıl ve nerede açın sanal ağ çevrilir belirlemek için yapılandırılabilir.

Sanal ağ trafiğini ulaştıktan sonra oyuna gelen birçok özellik vardır. Azure sanal ağlar, iş yüklerini ve temel ağ düzeyinde güvenlik geçerli olduğu eklemek müşteriler temelidir. Özel ağ (sanal ağ kaplama), aşağıdaki özellikleri ve özelliklere sahip müşteriler için Azure içinde değil:

* **Trafik yalıtımı**: bir sanal ağ trafiği yalıtımını Azure platformunda sınırıdır. Her iki sanal ağlar aynı müşteri tarafından oluşturulmamış olsa bile bir sanal ağdaki sanal makinelerden (VM'ler) farklı bir sanal ağ içindeki VM'ler için doğrudan iletişim kuramıyor. Yalıtım müşteri sanal makineleri sağlar önemli bir özelliktir ve iletişim bir sanal ağ içindeki özel kalır.

>[!NOTE]
>Trafik yalıtımı başvuruyor yalnızca trafiği *gelen* sanal ağ. Varsayılan olarak giden trafiği sanal ağ internet izin verilir, ancak Nsg'ler tarafından istenirse engellenebilir.
>
>

* **Çok katmanlı topoloji**: sanal ağlar izin müşteriler alt ağların ayırma ve farklı öğeler veya kendi iş yüklerinin "katmanları" için ayrı adres alanlarını belirleme tarafından çok katmanlı topolojisi tanımlayın. Bu mantıksal gruplar ve Topolojileri iş yükü türlerine göre farklı erişim ilkesini tanımlamak müşteriler etkinleştirmek ve de katmanı arasındaki trafik akışı denetler.
* **Şirket içi bağlantılar**: müşteriler Azure'da bir sanal ağ ve birden çok şirket içi siteler veya diğer sanal ağlar arasında şirketler arası bağlantı kurmak. Bir bağlantı oluşturmak için müşteriler, VNet eşlemesi, Azure VPN ağ geçitleri, üçüncü taraf ağ sanal Gereçleri veya ExpressRoute kullanabilirsiniz. Azure standart IPSec/IKE protokolleri ve ExpressRoute özel bağlantıyı kullanarak siteden siteye (S2S) VPN destekler.
* **NSG** ayrıntı düzeyi istenen düzeyde müşterilerin (ACL) kurallar oluşturmanıza olanak tanır: ağ arabirimleri, tek tek sanal makineleri veya sanal alt ağlar. Müşteriler erişimine izin verme veya reddetme iş yükleri bir sanal ağdan şirket içi bağlantılar aracılığıyla müşterinin ağlarındaki sistemleri arasındaki iletişim tarafından erişimi denetlemek veya Internet iletişimi doğrudan.
* **UDR** ve **IP iletimi** bir sanal ağ içindeki farklı katmanlar arasında iletişim yolları tanımlamak müşteriler izin. Müşteriler bir güvenlik duvarı, Kimlikleri/IP'leri ve diğer sanal gereçler dağıtmak ve bu güvenlik Gereçleri güvenlik sınırı İlkesi zorlaması, Denetim ve denetleme yoluyla ağ trafiğini yönlendirmek.
* **Ağ sanal Gereçleri** Azure markette: Azure Marketi ve VM görüntü Galerisi güvenlik Gereçleri güvenlik duvarları, yük Dengeleyiciler ve Kimlikler/IP'leri gibi kullanılabilir. Müşteriler çok katmanlı güvenli bir ağ ortamı tamamlamak için bu cihazları kendi sanal ağlara ve özellikle, (çevre ağ alt ağları dahil) kendi güvenlik sınırları dağıtabilirsiniz.

Bu özellik ve yetenekler Aşağıdaki diyagramda bir çevre ağ mimarisi ile Azure nasıl oluşturulabilir, bir örnek verilmiştir:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Çevre ağ özellikleri ve gereksinimleri
Çevre ağı doğrudan Internet üzerinden iletişim arabirim ağının ön uçtur. Gelen paketleri güvenlik duvarı, Kimlikleri ve IP'leri, gibi güvenlik Gereçleri arka uç sunucularına ulaşması önce akışına. Internet'e bağlı paketler iş yükleri de çevre ağındaki güvenlik Gereçleri ilke zorlaması, denetleme ve denetim amacıyla aracılığıyla ağ ayrılmadan önce akabilir. Ayrıca, çevre ağındaki müşteri sanal ağlar ve şirket içi ağlar arasında şirket içi VPN ağ geçitleri barındırabilir.

### <a name="perimeter-network-characteristics"></a>Çevre ağ özellikleri
Önceki şekil başvuran, iyi çevre ağ özelliklerini bazıları şunlardır:

* Internet'e yönelik:
  * Çevre ağ alt kendisini İnternete dönük, doğrudan Internet ile iletişim kurmasını değil.
  * Genel IP adresleri, VIP'ler ve/veya hizmet uç noktaları Internet trafiği için ön uç ağ ve aygıt geçirin.
  * Internet'ten gelen trafiği ön uç ağ üzerinde diğer kaynakları önce güvenlik cihazlar üzerinden geçirir.
  * Giden güvenlik etkinleştirilirse, trafiği güvenlik aygıtları internet geçirilmeden önce son adımı olarak geçirir.
* Korumalı ağ:
  * Çekirdek altyapıyı Internet'ten doğrudan hiçbir yolu yoktur.
  * Kanallar çekirdek altyapının Nsg'ler, güvenlik duvarları veya VPN cihazları gibi güvenlik cihazlarını üzerinden çapraz geçiş gerekir.
  * Diğer cihazları Internet ve çekirdek altyapıyı birleştirmesi gerekir değil.
  * İnternete dönük hem çevre ağına (Yukarıdaki şekilde gösterildiği gibi iki güvenlik duvarı simgeleri) sınırlarının karşılıklı korumalı ağ güvenlik cihazlarda gerçekte her sınır için tek sanal gereç farklı kuralları veya arabirimlerle olabilir. Çevre ağında iki sınırlarının yük mantıksal olarak ayrılmış, örneğin, bir fiziksel cihaz işleme.
* Diğer ortak yöntemler ve sınırlamalar:
  * İş yükleri, iş kritik bilgileri depolamamayı gerekir.
  * Erişim ve çevre ağ yapılandırmaları ve dağıtımları için güncelleştirmeleri yalnızca yetkili yöneticilerin sınırlıdır.

### <a name="perimeter-network-requirements"></a>Çevre ağ gereksinimleri
Bu özellikleri etkinleştirmek için sanal ağ gereksinimlerine başarılı çevre ağı uygulamak için aşağıdaki yönergeleri izleyin:

* **Alt ağ mimarisi:** sanal ağ alt ağın tamamını çevre ağı ayrılmış şekilde ayrılmış aynı sanal ağdaki diğer alt ağlara gelen belirtin. Bu ayrım çevre ağı ve bir güvenlik duvarı ya da Kimlikleri/IP'leri sanal gereç yoluyla diğer iç veya özel alt katmanları akışları arasındaki trafiğin sağlar.  Kullanıcı tanımlı yollar sınır ağlardaki Bu trafik sanal Gereci iletmek için gereklidir.
* **NSG:** çevre ağ alt kendisini Internet ile iletişimi izin vermek için açık olmalı, ancak bu gelmez müşteriler Nsg'ler atlayarak. Internet'e açık ağ yüzey en aza indirmek için genel güvenlik yöntemlerini takip edin. Dağıtımları veya özel uygulama protokolleri açık bağlantı noktalarını ve erişmesine izin verilen uzak adres aralıklarını kilitleyin. Bir tam kilitleme mümkün olmayan durumlar olabilir. Örneğin, müşterilerin Azure üzerinde bir dış Web sitesi varsa, çevre ağındaki tüm ortak IP adreslerinden gelen web isteklerini izin vermesi gerekir, ancak yalnızca web uygulaması bağlantı noktalarını açmanız gerekir: TCP bağlantı noktası 80 üzerinde ve/veya TCP bağlantı noktası 443'tür.
* **Yönlendirme tablosu:** çevre ağ alt kendisini Internet'e doğrudan iletişim kurabilmesi ancak bir güvenlik duvarı veya güvenlik üzerinden geçmeden doğrudan iletişim için ve arka uç veya şirket içi ağlardan izin vermemelidir Gereci.
* **Güvenlik Gereci yapılandırması:** yönlendirmek ve çevre ağı ve korumalı geri kalanı arasında paketlerin incelemek için güvenlik Gereçleri gibi Güvenlik Duvarı ' nı Kimlikleri, ağları ve IP cihazları çok konaklı olabilir. Bunlar çevre ağı ve arka uç alt ağlar için ayrı NIC'ler olabilir. Çevre ağındaki NIC'ler doğrudan ve karşılık gelen Nsg'ler ve çevre ağ yönlendirme tablosu ile Internet üzerinden iletişim kurar. Arka uç alt ağlara bağlanma NIC'ler daha Nsg'ler ve karşılık gelen arka uç alt ağ yönlendirme tablolarını sınırlı.
* **Güvenlik uygulama işlevinin:** genellikle çevre ağında dağıtılan güvenlik Gereçleri aşağıdaki işlevleri gerçekleştirin:
  * Güvenlik Duvarı: güvenlik duvarı kurallarının veya erişim denetimi ilkeleri gelen istekler için zorlamayı.
  * Tehdit algılama ve önleme: algılama ve kötü amaçlı saldırılara karşı Internet Azaltıcı.
  * Denetim ve günlük: denetim ve çözümleme için ayrıntılı günlük koruma.
  * Ters proxy: gelen istekleri karşılık gelen arka uç sunucularına yönlendirme. Bu yeniden yönlendirme uç cihazlarda hedef adresleri çevirme genellikle, arka uç sunucu adreslerini güvenlik duvarları ve eşleme içerir.
  * İletme proxy: NAT sağlayarak ve sanal ağ içindeki Internet'e başlatıldığına iletişimi için denetimi gerçekleştiriliyor.
  * Yönlendirici: gelen ve çapraz alt ağ trafiği sanal ağ içinde iletme.
  * VPN cihazı: müşterinin şirket içi ağlar ve Azure sanal ağlar arasında şirketler arası VPN bağlantısı için şirket içi VPN ağ geçitleri gibi davranır.
  * VPN sunucusu: VPN istemcileri için Azure sanal ağları birbirine bağlayan kabul etme.

> [!TIP]
> Aşağıdaki iki grup ayrı tutmak: kişiler çevre ağ güvenlik dişli ve uygulama geliştirme, dağıtım veya işlemleri yönetici olarak yetkilendirilmiş kişiler erişme yetkisi. Bu gruplar ayrı tutmak görevlerini ayrımı için sağlar ve uygulamaların güvenlik ve ağ güvenlik denetimleri atlayarak tek bir kişinin önler.
>
>

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Ağ sınırları oluştururken sorulan sorular
Bu bölümde, özellikle belirtilmediği sürece, "Ağ" terimi bir abonelik Yöneticisi tarafından oluşturulan, özel, Azure Sanal Ağları gösterir. Terim Azure içinde temel alınan fiziksel ağlara anlamına gelmez.

Ayrıca, Azure sanal ağlar, genellikle geleneksel şirket içi ağlar genişletmek için kullanılır. Siteden siteye veya ExpressRoute karma ağ çözümleri çevre ağ mimarisi dahil mümkündür. Bu karma bağlantı, ağ güvenlik sınırları oluşturmanın önemli bir konudur.

Aşağıdaki üç soruları bir çevre ağ ve birden çok güvenlik sınırı olan bir ağ oluştururken yanıtlamak için kritik öneme sahiptir.

#### <a name="1-how-many-boundaries-are-needed"></a>1) kaç tane sınırları gerekli mi?
İlk karar noktası kaç güvenlik sınırları içinde belirli bir senaryo gerekli karar vermektir:

* Tek bir sınır: bir ön uç çevre ağındaki sanal ağ ve Internet arasında.
* İki sınır: bir çevre ağı ve başka bir Internet tarafındaki çevre ağ alt ağı ve Azure sanal ağlar arka uç alt ağlar arasında.
* Üç sınırları: bir arka uç alt ağlar ve çevre ağı arasında bir çevre ağında Internet tarafındaki ve bir arka uç alt ağlar arasında ve şirket içi ağ.
* N sınırları: değişken numarası. Güvenlik gereksinimlerine bağlı olarak, belirli bir ağda uygulanabilir güvenlik sınırları sayısına bir sınır yoktur.

Sayısı ve türü farklılık gerekli sınırların bir şirketin riski dayanıklılık ve uygulanan belirli bir senaryoyu göre. Bu genellikle genellikle bir risk ve uyumluluk takım, bir ağ ve platform ekip ve bir uygulama geliştirme ekipleri dahil olmak üzere, bir kuruluştaki birden çok grubu ile birlikte kararı verilir. Güvenlik, söz konusu veri ve kullanılan teknolojiler bilgisine sahip kişiler, her uygulama için uygun güvenlik tutum sergilemek emin olmak için bu karara bir say olması gerekir.

> [!TIP]
> Belirli bir durumu için güvenlik gereksinimlerini karşılayan sınırları en az sayıda kullanın. Daha fazla sınırlarıyla işlemlerini ve sorun giderme daha zor olabilir zaman içinde birden fazla sınır ilkelerini yönetme ile ilgili yönetim yükünü yanı sıra. Ancak, yetersiz sınırları riski artırır. Bakiye bulma önemlidir.
>
>

![6]

Yukarıdaki şekil üç güvenlik sınır ağ üst düzey bir görünümünü gösterir. Çevre ağı ve Internet, Azure ön uç ve arka uç özel alt ağlar ve Azure arka uç alt ağ ve şirket içi kurumsal ağ arasında sınırları değildir.

#### <a name="2-where-are-the-boundaries-located"></a>2) sınırları bulunduğu?
Sınırları sayısı karar sonra bunları uygulamak İleri karar noktası yerdir. Genellikle üç seçenek vardır:

* Bir Internet tabanlı aracı hizmetini (Bu belgede ele alınan değil, örneğin, bir bulut tabanlı Web uygulaması güvenlik duvarı,) kullanarak
* Azure'da yerel özellikleri ve/veya ağ sanal Gereçleri kullanma
* Şirket içi ağda fiziksel aygıtların kullanma

Tamamen Azure ağlarda seçenekleri yerel Azure özellikleri (örneğin, Azure yük dengeleyicileri) veya ağ sanal Gereçleri zengin iş ortağı ekosistemi gelen Azure (örneğin, Check Point güvenlik duvarları) olur.

Bir sınır Azure ile şirket içi ağınız arasında gerekiyorsa, güvenlik aygıtları bağlantısı (veya her iki tarafında da) iki tarafında bulunabilir. Bu nedenle bir karar konumuna güvenlik dişli yerleştirmek için yapılması gerekir.

Yukarıdaki şekilde Internet çevre ağı ve ön için arka uç sınırları tamamen Azure içinde yer alır ve yerel Azure özellikleri veya ağ sanal Gereçleri olmalıdır. Azure (arka uç alt ağ) ve şirket ağı arasında sınır güvenlik cihazlarda Azure yan veya şirket içi yan veya iki tarafı da cihazlarda bile bir birleşimi olabilir. Önemli avantajları ve dezavantajları, ciddi dikkate alınması gereken iki seçenekten birini olabilir.

Örneğin, şirket içi ağ tarafında mevcut fiziksel güvenlik araçları kullanarak yeni bir dişli gereklidir avantajına sahiptir. Yalnızca, yeniden yapılandırılması gerekir. Dezavantajı, ancak tüm trafiği geri Azure'dan şirket içi ağ güvenlik dişli tarafından görülebilir gelmelidir emin olur. Bu nedenle Azure Azure trafik önemli gecikme tabi ve güvenlik ilkesi zorlaması için şirket içi ağınıza geri zorlandı, uygulama performansını etkiler ve kullanıcı, deneyimi.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) sınırları nasıl uygulanır?
Her güvenlik sınırı, büyük olasılıkla farklı yetenek gereksinimleri (örneğin, Kimlikleri ve güvenlik duvarı kuralları çevre ağına, ancak yalnızca arka uç alt ağı ve çevre ağı arasında ACL'leri Internet tarafındaki) vardır. Karar üzerinde kullanmak için aygıt (veya cihaz) senaryosu ve güvenlik gereksinimlerine bağlıdır. Aşağıdaki bölümde örnek 1, 2 ve 3 kullanılabilecek bazı seçenekler açıklanmaktadır. Azure yerel ağ özellikleri ve iş ortağı ekosistemi Azure'da kullanılabilir aygıtları gözden geçirerek, neredeyse her senaryo çözmek kullanılabilir çok seçeneklerini gösterir.

Başka bir anahtar uygulama karar Azure ile şirket içi ağ bağlanma noktasıdır. Azure sanal ağ geçidi ya da bir ağ sanal gereç kullanmalısınız? Bu seçenekler aşağıdaki bölümde (örnekler 4, 5 ve 6) daha ayrıntılı ele alınmıştır.

Ayrıca, azure'daki sanal ağlar arasında trafiği gerekli olabilir. Bu senaryolar gelecekte eklenir.

Önceki soruların yanıtlarını öğrendikten sonra [Hızlı Başlat](#fast-start) bölümü hangi örnekleri belirli bir senaryo için en uygun tanımlamaya yardımcı olabilir.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Örnekler: Azure sanal ağlar ile güvenlik sınırları oluşturma
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Örnek 1 yapı Nsg'ler uygulamalarla korunmasına yardımcı olmak için bir çevre ağı
[Hızlı Başlat dön](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example1]

[![7]][7]

#### <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, aşağıdaki kaynakları içeren bir abonelik vardır:

- Tek bir kaynak grubu
- İki alt ağa sahip bir sanal ağ: "Ön uç" ve "Arka uç"
- Her iki alt ağa uygulanan ağ güvenlik grubu
- Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
- Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
- Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu
- Uygulama web sunucusu ile ilişkili bir ortak IP

Komut dosyaları ve Azure Resource Manager şablonu için bkz: [ayrıntılı yapılandırma yönergeleri][Example1].

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu yerleşik ve altı kurallarıyla yüklendi.

> [!TIP]
> Genel olarak bakıldığında, daha genel "Deny" kuralları tarafından izlenen "İzin ver" özel kurallarınızı ilk olarak, oluşturmalısınız. Verilen öncelik hangi kuralları ilk olarak değerlendirilir belirler. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları her iki yönde gelen veya giden (alt ağ perspektifinden) uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kural gelen trafik için oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir.
2. Herhangi bir sanal makine için RDP trafiğinin (3389 numaralı bağlantı noktası) Internet'ten izin verilir.
3. Web sunucusu (IIS01) için HTTP trafiğine (bağlantı noktası 80) Internet'ten izin verilir.
4. Tüm trafiği (tüm bağlantı noktaları) IIS01 AppVM1 izin verilir.
5. Tüm trafiği (tüm bağlantı noktaları) Internet'ten tüm sanal ağa (her iki alt ağ) reddedildi.
6. Tüm trafiği (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağa reddedildi.

Bu kurallar ile bağlı her alt ağ için bir HTTP isteği hem kuralları 3, web sunucusu Internet'ten gelen ise (izin verin) ve 5 (Reddet) geçerli olur. Ancak 3 kuralı daha yüksek öncelikli olduğundan, yalnızca geçerli olur ve kural 5, oyuna gelen değil. Bu nedenle web sunucusuna HTTP isteği izin verilir. Bu aynı trafiği DNS01 sunucunun erişmeye, 5 kural (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmeniz izin verilmiyor. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafiği kurallarında 1 ve 4) Konuşmayı gelen ön uç alt ağ blokları. Bu kural kümesine durumda ön uçtaki web uygulaması bir saldırganın etkilediğinde arka uç ağ korur. Saldırgan (kaynaklarına AppVM01 sunucu üzerinde gösterilen yalnızca) arka uç "korumalı" Ağ erişimi sınırlı.

İnternet giden trafiğe izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir (örnek 3 bakın).

#### <a name="conclusion"></a>Sonuç
Bu örnek, arka uç alt ağından gelen trafiği yalıtma, nispeten basit ve kolay bir yoludur. Daha fazla bilgi için bkz: [ayrıntılı yapılandırma yönergeleri][Example1]. Bu yönergeleri içerir:

* Bu çevre ağı ile klasik PowerShell komut dosyaları oluşturma.
* Bu çevre ağı ile bir Azure Resource Manager şablonu oluşturma.
* Her NSG komutu ayrıntılı açıklamaları.
* Nasıl trafiğine izin verilen veya her katmanda reddedildi gösteren, ayrıntılı trafik akışı senaryoları.


### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Örnek 2 yapı bir güvenlik duvarı ve Nsg'ler uygulamalarla korunmasına yardımcı olmak için bir çevre ağı
[Hızlı Başlat dön](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example2]

[![8]][8]

#### <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, aşağıdaki kaynakları içeren bir abonelik vardır:

* Tek bir kaynak grubu
* İki alt ağa sahip bir sanal ağ: "Ön uç" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Ağ sanal gereç, güvenlik duvarı bir bu durumda ön uç alt ağına bağlı
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu

Komut dosyaları ve Azure Resource Manager şablonu için bkz: [ayrıntılı yapılandırma yönergeleri][Example2].

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu yerleşik ve altı kurallarıyla yüklendi.

> [!TIP]
> Genel olarak bakıldığında, daha genel "Deny" kuralları tarafından izlenen "İzin ver" özel kurallarınızı ilk olarak, oluşturmalısınız. Verilen öncelik hangi kuralları ilk olarak değerlendirilir belirler. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları her iki yönde gelen veya giden (alt ağ perspektifinden) uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kural gelen trafik için oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir.
2. Herhangi bir sanal makine için RDP trafiğinin (3389 numaralı bağlantı noktası) Internet'ten izin verilir.
3. Tüm (tüm bağlantı noktaları) Internet trafiği ağ sanal gereç (Güvenlik Duvarı) de izin verilir.
4. Tüm trafiği (tüm bağlantı noktaları) IIS01 AppVM1 izin verilir.
5. Tüm trafiği (tüm bağlantı noktaları) Internet'ten tüm sanal ağa (her iki alt ağ) reddedildi.
6. Tüm trafiği (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağa reddedildi.

Bu kurallar ile bağlı her alt ağ için bir HTTP isteği Internet'ten gelen güvenlik duvarı hem kuralları 3 ise (izin verin) ve 5 (Reddet) geçerli olur. Ancak 3 kuralı daha yüksek öncelikli olduğundan, yalnızca geçerli olur ve kural 5, oyuna gelen değil. Bu nedenle HTTP isteği Güvenlik Duvarı'na izin. Ön uç alt ağda olsa bile, o aynı trafik IIS01 sunucusuna ulaşmaya çalışıyordu, 5 kural (uygulamak reddetme), ve trafiği sunucuya geçirmeniz izin verilmiyor. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafiği kurallarında 1 ve 4) Konuşmayı gelen ön uç alt ağ blokları. Bu kural kümesine durumda ön uçtaki web uygulaması bir saldırganın etkilediğinde arka uç ağ korur. Saldırgan (kaynaklarına AppVM01 sunucu üzerinde gösterilen yalnızca) arka uç "korumalı" Ağ erişimi sınırlı.

İnternet giden trafiğe izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir (örnek 3 bakın).

#### <a name="firewall-rule-description"></a>Güvenlik duvarı kuralı açıklaması
Güvenlik Duvarı'nı kuralları iletme oluşturulmalıdır. Bu örnek yalnızca sınır Güvenlik Duvarı'na Internet trafiğini yönlendiren ve ardından web sunucusu için yalnızca bir iletme ağ adresi çevirisi (NAT) bu yana kuralı gereklidir.

İletme kuralı HTTP (80 veya 443 HTTPS için bağlantı noktası) erişmeye Güvenlik Duvarı'nı isabetler herhangi bir gelen kaynak adresi kabul eder. Güvenlik duvarının yerel arabirim dışında gönderilen ve 10.0.1.5 IP adresi ile web sunucusunu yeniden yönlendirildi.

#### <a name="conclusion"></a>Sonuç
Bu örnek, bir güvenlik duvarı ile uygulamanızı koruma ve arka uç alt ağından gelen trafiği yalıtma görece basit bir yoludur. Daha fazla bilgi için bkz: [ayrıntılı yapılandırma yönergeleri][Example2]. Bu yönergeleri içerir:

* Bu çevre ağı ile klasik PowerShell komut dosyaları oluşturma.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturma.
* Her NSG komutu ve güvenlik duvarı kuralı ayrıntılı açıklamaları.
* Nasıl trafiğine izin verilen veya her katmanda reddedildi gösteren, ayrıntılı trafik akışı senaryoları.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Örnek 3 yapı bir güvenlik duvarı ve UDR ve NSG ile ağ korumaya yardımcı olmak için bir çevre ağı
[Hızlı Başlat dön](#fast-start) | [ayrıntılı yönergeler için bu örnek oluşturma][Example3]

[![9]][9]

#### <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, aşağıdaki kaynakları içeren bir abonelik vardır:

* Tek bir kaynak grubu
* Bir sanal ağ üç alt ağ ile: "SecNet", "Ön uç" ve "Arka uç"
* Ağ sanal gereç, güvenlik duvarı bir bu durumda SecNet alt ağına bağlı
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki Windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu

Komut dosyaları ve Azure Resource Manager şablonu için bkz: [ayrıntılı yapılandırma yönergeleri][Example3].

#### <a name="udr-description"></a>UDR açıklaması
Varsayılan olarak, aşağıdaki sistem yolları gibi tanımlanır:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal her zaman, belirli bir ağ için sanal ağ oluşturan bir veya daha fazla tanımlanan adres öneklerini olur (diğer bir deyişle, bu sanal ağla olan her belirli bir sanal ağı nasıl tanımlandığına bağlı olarak sanal ağa değişir). Kalan sistem yolları statik ve tabloda belirtildiği gibi varsayılan.

Bu örnekte, iki yönlendirme tablolarını oluşturulur ve her bir ön uç ve arka uç alt ağlar için. Her tablo için belirli alt uygun statik yollar ile yüklenir. Bu örnekte, her tablo tüm trafiği (0.0.0.0/0) doğrudan üç yollar güvenlik duvarı aracılığıyla sahip (sonraki atlama = sanal Gereci IP adresi):

1. Güvenlik Duvarı atlamak yerel alt ağ trafiğine izin vermek için yerel alt ağ trafiği hiçbir sonraki atlama ile tanımlanır.
2. Bir sonraki güvenlik duvarı olarak tanımlanan atlama ile sanal ağ trafiği. Bu sonraki atlama doğrudan yönlendirmek, yerel sanal ağ trafiğine izin veren varsayılan kural geçersiz kılar.
3. Tüm kalan trafiği (0/0) bir sonraki Güvenlik Duvarı'nı tanımlanan atlama.

> [!TIP]
> Yerel alt ağ giriş UDR sonları yerel alt ağ iletişimleri olmaması.
>
> * Bizim örneğimizde, VNETLocal için işaret eden 10.0.1.0/24 önemlidir! NVA gönderilecek şekilde bu olmadan, başka bir yerel sunucuya (örneğin) 10.0.1.25 hedefleyen Web sunucusu (10.0.1.4) bırakarak paket başarısız olur. Nva'nın alt ağa gönderir ve alt ağ NVA sonsuz bir döngü için gönderecektir.
> * Bir yönlendirme döngüsü olasılığını genellikle Geleneksel, şirket içi cihazları olduğu alt ağlar, ayırmak için bağlı birden çok NIC ile cihazları genellikle daha yüksektir.
>
>

Yönlendirme tabloları oluşturulduktan sonra alt ağlarını bağlanmalıdır. Bir kez oluşturulur ve alt ağına bağlı ön uç alt ağ yönlendirme tablosu, bu çıkış gibi görünür:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR şimdi expressroute bağlantı hattı bağlı olduğu ağ geçidi alt ağına uygulanabilir.
>
> Örnek 3 ve 4 çevre ağınız ExpressRoute veya siteden siteye ağ ile etkinleştirmek nasıl örnekleri gösterilmektedir.
>
>

#### <a name="ip-forwarding-description"></a>IP iletimi açıklaması
IP iletimi UDR için yardımcı özelliğidir. IP iletimi Gereci özellikle trafiği alabilmesine ve bu trafiğin ultimate hedefine iletmek imkan tanıyan bir sanal gereç üzerinde bir ayardır.

AppVM01 DNS01 sunucuya istekte, örneğin, UDR bu trafiğin Güvenlik Duvarı'na rota. Etkin IP iletimi ile DNS01 hedef (10.0.2.4) trafiği (10.0.0.4) Gereci tarafından kabul edilir ve ultimate hedefine (10.0.2.4) iletilir. Yol tablosu sonraki atlama olarak bir güvenlik duvarına sahip olsa bile Güvenlik Duvarı'nı etkin IP iletimi, trafiği Gereci tarafından kabul değil. Bir sanal gereç kullanmak için IP iletme UDR birlikte etkinleştirmek için önemlidir.

#### <a name="nsg-description"></a>NSG açıklaması
Bu örnekte, bir NSG grubu yerleşik ve tek bir kural ile yüklenir. Bu grup, ardından yalnızca ön uç ve arka uç alt ağlara (SecNet değil) bağlıdır. Aşağıdaki kural oluşturulmakta olan bildirimli olarak:

* Tüm trafiği (tüm bağlantı noktaları) Internet'ten tüm sanal ağa (tüm alt ağlar) reddedildi.

Nsg'ler Bu örnekte kullanılan ana amacı el ile yetersizliğini karşı savunma ikincil bir katmanı olarak olsa da. Internet'ten ön uç veya arka uç alt ağlar için tüm gelen trafiği engellemek için belirtilir. Trafik yalnızca SecNet alt ağ güvenlik duvarı aracılığıyla akış (ve daha sonra uygunsa, ön uç veya arka uç alt açın). Ayrıca, bir yerde, UDR kurallarla ön uç veya arka uç alt yaptı tüm trafik çıkışı Güvenlik Duvarı'nda (UDR) sayesinde yönlendirilmesi. Güvenlik Duvarı'nı bu trafiği asimetrik bir akış halinde görür ve giden trafik bırakma. Bu nedenle güvenlik alt ağ korumaya üç katmanı vardır:

* Herhangi bir ön uç veya arka uç NIC'ler üzerinde hiçbir ortak IP adresleri.
* Internet'ten trafiği engelleme Nsg'ler.
* Güvenlik Duvarı bırakma asimetrik trafiği.

Bu örnekte NSG ile ilgili bir ilginç noktası güvenlik alt ağ dahil olmak üzere tüm sanal ağ, Internet trafiği reddetmek için yalnızca bir kural içermesidir. NSG yalnızca ön uç ve arka uç alt ağa bağlı olduğundan, kuralın trafiğinde ancak işlenen değil gelen güvenlik alt ağ için. Sonuç olarak, güvenlik alt ağ akışlarına trafiği.

#### <a name="firewall-rules"></a>Güvenlik duvarı kuralları
Güvenlik Duvarı'nı kuralları iletme oluşturulmalıdır. Güvenlik duvarı engelleme veya iletme tüm gelen, giden ve içi sanal ağ trafiğini olduğundan, birçok güvenlik duvarı kuralları gerekir. Ayrıca, tüm gelen trafiği güvenlik duvarı tarafından işlenmek üzere güvenlik hizmeti genel IP adresi (farklı bağlantı noktaları üzerinde) kadardır. Alt ağlar ayarlamadan önce mantıksal akış diyagramı için en iyi uygulamadır ve önlemek için güvenlik duvarı kuralları, daha sonra rework. Aşağıdaki şekilde, bu örnek için güvenlik duvarı kurallarının mantıksal bir görünümdür:

![10]

> [!NOTE]
> Ağ sanal kullanılan Gereci bağlı olarak, yönetim bağlantı noktalarını farklılık gösterir. Bu örnekte, bağlantı noktası 22, 801 ve 807 kullanan bir Barracuda NextGen Firewall başvuruluyor. Kullanılan aygıt yönetimi için kullanılan tam bağlantı noktalarını bulmak için Gereci satıcı belgelerine bakın.
>
>

#### <a name="firewall-rules-description"></a>Güvenlik duvarı kuralları açıklaması
Yalnızca kaynak o alt ağdaki güvenlik duvarının olmadığı için güvenlik alt ağ mantıksal önceki şemada gösterilmez. Aşağıdaki diyagramda, güvenlik duvarı kuralları ve nasıl mantıksal olarak izin ver veya Reddet trafiği akışı, gerçek yönlendirilmiş yol değil gösteriliyor. Ayrıca, RDP trafiğine için daha yüksek seçilen dış bağlantı noktalarını bağlantı noktaları (8014 – 8026) aralıklı ve geniş daha kolay okunabilir olması için yerel IP adresi son iki sekizlisinin hizalamak için seçildi (örneğin, yerel sunucu 10.0.1.4 8014 dış bağlantı noktasıyla ilişkili adresidir). Ancak, tüm yüksek çakışmayan bağlantı noktaları, kullanılabilir.

Bu örnekte, yedi tür kuralların gerekir:

* Dış kuralları (için gelen trafiği):
  1. Güvenlik Duvarı Yönetimi kuralı: Bu uygulama yeniden yönlendirme kuralı trafiğin ağ sanal gereç yönetim bağlantı noktalarına geçmesine izin verir.
  2. RDP kuralları (her Windows server için): Bu dört kuralları (her sunucu için bir tane) RDP aracılığıyla tek sunucuların Yönetimi izin verir. Dört RDP kuralları kullanılan ağ sanal gereç özelliklerine bağlı olarak bir kural içine de daraltılmış.
  3. Uygulama trafiği kuralları: kurallar, ön uç web trafiği için ilk ve ikinci arka uç trafiği (örneğin, web sunucusuna veri katmanı) için iki vardır. Bu kurallar yapılandırmasını (burada, sunucularınızın yerleştirilir) ağ mimarisine bağlıdır ve trafik akışları (hangi yönde trafik akışına ve hangi bağlantı noktaları kullanılır).
     * İlk kural, uygulama sunucusu ulaşmak gerçek uygulama trafiğine izin verir. Güvenlik ve Yönetim için diğer kurallardan izin verirken, uygulama trafiği ne harici kullanıcılar ya da hizmetleri uygulamalara erişmesine izin kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde tek bir web sunucusu yok. Böylece bir tek uygulama güvenlik duvarını gelen trafik dış IP, web sunucuları iç IP adresine yönlendirir. Yeniden yönlendirilen trafiği oturumu NAT iç sunucu çevrilmesi.
     * İkinci kuralı AppVM01 sunucu (ancak değil AppVM02) için anlaşmak web sunucusu izin vermek için arka uç herhangi bir bağlantı kuralıdır.
* İç kurallardan (içi sanal ağ trafiği)
  1. Internet kuralı giden: Bu kural seçilen ağlara geçirmek için herhangi bir ağ gelen trafiğe izin verir. Bu genellikle varsayılan bir kural zaten güvenlik duvarında ancak devre dışı durumdayken kuralıdır. Bu kural, bu örnek için etkinleştirilmelidir.
  2. DNS kuralı: Bu kural DNS sunucusuna iletmek yalnızca DNS (bağlantı noktası 53) trafiğine izin verir. Bu ortam için arka uç ön ucu çoğu trafiği engellendi. Bu kural, tüm yerel alt ağ üzerinden DNS özellikle sağlar.
  3. Alt ağ alt ağı kuralın: Bu kural, ön uç alt ağ (ancak tersi) herhangi bir sunucuya bağlanmak için arka uç alt ağda herhangi bir sunucu izin vermektir.
* Yedek operatördür kuralı (önceki birini karşılamıyor trafiği):
  1. Reddetme tüm trafik kuralı: herhangi biriyle eşleşen bir trafik akışı başarısız olursa bu şekilde önceki bu kural tarafından bırakıldı kuralları ve bu reddetme kuralı her zaman son bir kural (bakımından öncelik) olması gerekir. Bu kural varsayılan kuralıdır ve genellikle yerinde ve etkin. Hiçbir değişikliğe genellikle bu kural için gereklidir.

> [!TIP]
> İkinci uygulama trafik kuralı üzerinde varsayılan olarak, herhangi bir bağlantı noktası bu örnekte, basitleştirmek izin verilmez. Gerçek bir senaryoda, bu kural, saldırı yüzeyini azaltmak için en özel bağlantı noktası ve adres aralıkları kullanılmalıdır.
>
>

Önceki kural oluşturulduktan sonra trafiğine izin verilen veya istendiği gibi reddedilen emin olmak için her bir kural önceliğini gözden geçirilmesi önemlidir. Bu örnekte, öncelik sırasına göre kurallardır.

#### <a name="conclusion"></a>Sonuç
Daha karmaşık ancak bu örnekte, koruma ve daha önceki örneklerde ağ yalıtma şekilde tamamlayın. (Yalnızca uygulamayı örnek 2 korur ve örnek 1 yalnızca alt ağlar yalıtır). Bu tasarım, her iki yönde trafik izlenmesini sağlar ve yalnızca gelen uygulama sunucusu korur ancak bu ağdaki tüm sunucular için ağ güvenlik ilkeleri uygular. Ayrıca, kullanılan Gereci bağlı olarak, tam trafiği denetim ve tanıma yararlanılabilir. Daha fazla bilgi için bkz: [ayrıntılı yapılandırma yönergeleri][Example3]. Bu yönergeleri içerir:

* Bu örnek çevre ağında Klasik PowerShell komut dosyalarıyla oluşturma.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturma.
* Ayrıntılı her UDR NSG açıklamalarını komut ve güvenlik duvarı kuralı.
* Nasıl trafiğine izin verilen veya her katmanda reddedildi gösteren, ayrıntılı trafik akışı senaryoları.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Siteden siteye, sanal bir Gereci VPN ile karma bağlantı örnek 4 ekleme
[Hızlı Başlat dön](#fast-start) | Ayrıntılı yapılandırma yönergeleri kullanılabilir yakında

[![11]][11]

#### <a name="environment-description"></a>Ortam açıklaması
Bir ağ sanal gereç (NVA) kullanarak karma ağ 1, 2 veya 3 örneklerde açıklanan çevre ağ türlerinden herhangi birini eklenebilir.

Yukarıdaki şekilde gösterildiği gibi bir VPN bağlantısı (siteden siteye) Internet üzerinden bir NVA aracılığıyla Azure sanal ağı şirket içi ağ bağlanmak için kullanılır.

> [!NOTE]
> ExpressRoute, Azure ortak eşleme seçeneği etkin kullanırsanız, statik yol oluşturulmalıdır. Bu statik yol, Kurumsal Internet ve ExpressRoute bağlantısı aracılığıyla değil NVA VPN IP adresine yönlendirmek. ExpressRoute, Azure ortak eşleme seçeneğini gereken NAT VPN oturumu bozulabilir.
>
>

VPN yerinde olduktan sonra nva'nın tüm ağları ve alt ağlar için merkezi hub haline gelir. Güvenlik Duvarı iletme kurallarını hangi trafik akışına izin verildiğini belirlemek NAT çevrilir yönlendirilir veya (hatta trafik akışları için şirket içi ağınız ve Azure arasında) bırakıldı.

İyileştirilebilir veya bu tasarım deseni tarafından düşürülmüş bağlı olarak belirli kullanım örneğini gibi trafik akışına dikkatlice değerlendirilmelidir.

Örnek 3 yerleşik ortamı kullanarak ve siteden siteye VPN karma ağ bağlantısı ekleme, aşağıdaki tasarım üretir:

[![12]][12]

Şirket içi yönlendirici veya VPN için NVA ile uyumlu herhangi bir ağ aygıtı VPN istemcisi olacaktır. Bu fiziksel cihazı başlatma, NVA VPN bağlantısıyla koruma sorumlu olacaktır.

Mantıksal olarak NVA için ağ dört ayrı "güvenlik bölgeleri" gibi kuralları bu bölgeler arasındaki trafiği birincil müdürü olan NVA üzerinde görünür:

![13]

#### <a name="conclusion"></a>Sonuç
Bir Azure sanal ağı bir siteden siteye VPN karma ağ bağlantısı eklenmesi şirket içi ağ Azure'da güvenli bir şekilde genişletebilirsiniz. Bir VPN bağlantısı kullanarak, trafiğinizin şifrelenir ve yönlendiren Internet üzerinden. Bu örnekte nva'nın zorlamak ve Güvenlik İlkesi'ni yönetmek için merkezi bir konum sağlar. Daha fazla bilgi için ayrıntılı yapılandırma yönergeleri (yeni çıkacak) bakın. Bu yönergeleri içerir:

* Bu örnek çevre ağında PowerShell komut dosyalarıyla oluşturma.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturma.
* Bu tasarım trafiğinin nasıl akacağını gösteren, ayrıntılı trafik akışı senaryoları.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Örnek 5 bir siteden siteye Azure VPN ağ geçidi ile karma bağlantı ekleyin
[Hızlı Başlat dön](#fast-start) | Ayrıntılı yapılandırma yönergeleri kullanılabilir yakında

[![14]][14]

#### <a name="environment-description"></a>Ortam açıklaması
Bir Azure VPN ağ geçidi kullanarak karma ağ 1 veya 2 örneklerde açıklanan ya da çevre ağ türü eklenebilir.

Yukarıdaki şekilde gösterildiği gibi bir VPN bağlantısı (siteden siteye) Internet üzerinden şirket içi ağ bir Azure sanal ağı Azure VPN ağ geçidi üzerinden bağlanmak için kullanılır.

> [!NOTE]
> ExpressRoute, Azure ortak eşleme seçeneği etkin kullanırsanız, statik yol oluşturulmalıdır. Bu statik yol, Kurumsal Internet ve aracılığıyla değil ExpressRoute WAN NVA VPN IP adresine yönlendirmek. ExpressRoute, Azure ortak eşleme seçeneğini gereken NAT VPN oturumu bozulabilir.
>
>

Aşağıdaki şekilde, bu örnekte, iki ağ kenarları gösterilmektedir. İlk kenarına NVA ve Nsg'ler içi Azure ağları ve Azure ile Internet arasında trafiği akışı denetler. İkinci bir kenar ayrı ve yalıtılmış ağ ucunun şirket içi ve Azure arasında Azure VPN ağ geçidi olur.

İyileştirilebilir veya bu tasarım deseni tarafından düşürülmüş bağlı olarak belirli kullanım örneğini gibi trafik akışına dikkatlice değerlendirilmelidir.

Örnek 1 yerleşik ortamı kullanarak ve siteden siteye VPN karma ağ bağlantısı ekleme, aşağıdaki tasarım üretir:

[![15]][15]

#### <a name="conclusion"></a>Sonuç
Bir Azure sanal ağı bir siteden siteye VPN karma ağ bağlantısı eklenmesi şirket içi ağ Azure'da güvenli bir şekilde genişletebilirsiniz. Yerel Azure VPN ağ geçidi kullanarak trafiğinizi şifrelenmiş IPSec ve Internet üzerinden yönlendirir. Ayrıca, Azure VPN ağ geçidi kullanarak (hiçbir ek gibi üçüncü taraf NVAs ile maliyet Lisansı) daha düşük maliyetli seçeneği sağlayabilirsiniz. Bu seçenek, hiçbir NVA kullanıldığı örnek 1, en ekonomik olur. Daha fazla bilgi için ayrıntılı yapılandırma yönergeleri (yeni çıkacak) bakın. Bu yönergeleri içerir:

* Bu örnek çevre ağında PowerShell komut dosyalarıyla oluşturma.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturma.
* Bu tasarım trafiğinin nasıl akacağını gösteren, ayrıntılı trafik akışı senaryoları.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>ExpressRoute ile karma bağlantı örnek 6 ekleme
[Hızlı Başlat dön](#fast-start) | Ayrıntılı yapılandırma yönergeleri kullanılabilir yakında

[![16]][16]

#### <a name="environment-description"></a>Ortam açıklaması
ExpressRoute özel eşleme bağlantısı kullanarak karma ağ 1 veya 2 örneklerde açıklanan ya da çevre ağ türü eklenebilir.

Yukarıdaki şekilde gösterildiği gibi ExpressRoute özel eşleme, şirket içi ağınız ve Azure sanal ağı arasında doğrudan bir bağlantı sağlar. Yalnızca hizmet sağlayıcısı ağ ve hiçbir zaman Internet temas Microsoft Azure ağ trafiği transits.

> [!TIP]
> ExpressRoute kullanarak kurumsal ağ trafiğini Internet'ten tutar. Ayrıca hizmet düzeyi sözleşmeleri için ExpressRoute sağlayıcınızdan sağlar. Siteden siteye VPN ile 200 MB/sn Azure ağ geçidi en yüksek verimlilik iken Azure ağ geçidini ExpressRoute ile 10 GB/sn kadar geçirebilirsiniz.
>
>

Aşağıdaki diyagramda görüldüğü gibi bu seçenek ile ortamı iki ağ kenarları artık sahiptir. Ağ geçidi şirket içi ve Azure arasında ayrı ve yalıtılmış ağ kenar olsa da NVA ve NSG içi Azure ağları ve Azure ile Internet arasında trafiği akışı denetler.

İyileştirilebilir veya bu tasarım deseni tarafından düşürülmüş bağlı olarak belirli kullanım örneğini gibi trafik akışına dikkatlice değerlendirilmelidir.

Örnek 1 yerleşik ortamı kullanarak ve ardından bir ExpressRoute karma ağ bağlantısı ekleyerek, aşağıdaki tasarım üretir:

[![17]][17]

#### <a name="conclusion"></a>Sonuç
Bir ExpressRoute özel eşleme ağ bağlantısı eklenmesi, daha yüksek bir şekilde gerçekleştirme bir güvenli, daha düşük gecikme, Azure'da şirket içi ağ genişletebilirsiniz. Ayrıca, bu örnekte olduğu gibi yerel Azure ağ geçidini kullanma (üçüncü taraf NVAs gibi ile lisans Hayır ek) bir daha düşük maliyetli seçeneği sağlar. Daha fazla bilgi için ayrıntılı yapılandırma yönergeleri (yeni çıkacak) bakın. Bu yönergeleri içerir:

* Bu örnek çevre ağında PowerShell komut dosyalarıyla oluşturma.
* Bu örnek bir Azure Resource Manager şablonu ile oluşturma.
* Bu tasarım trafiğinin nasıl akacağını gösteren, ayrıntılı trafik akışı senaryoları.

## <a name="references"></a>Başvurular
### <a name="helpful-websites-and-documentation"></a>Yararlı Web siteleri ve belgeler
* Erişim Azure Azure Resource Manager ile:
* Azure PowerShell ile erişme: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Sanal ağ belgeleri: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Ağ güvenlik grubu belgeleri: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/security-overview.md)
* Kullanıcı tanımlı yönlendirme belgeleri: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Azure sanal ağ geçitleri: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Siteden siteye VPN: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* ExpressRoute belgeleri ("Başlarken" ve "Nasıl yapılır" bölümleri kontrol ettiğinizden emin olun): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "güvenlik seçenekleri akış çizelgesi"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "azure güvenlik özellikleri"
[3]: ./media/best-practices-network-security/dmzcorporate.png "bir şirket ağında bir DMZ"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "azure güvenlik mimarisi"
[5]: ./media/best-practices-network-security/dmzazure.png "bir Azure sanal ağında DMZ"
[6]: ./media/best-practices-network-security/dmzhybrid.png "üç güvenlik sınırlarla karma ağ"
[7]: ./media/best-practices-network-security/example1design.png "giriş DMZ NSG ile"
[8]: ./media/best-practices-network-security/example2design.png "giriş DMZ NVA ve NSG ile"
[9]: ./media/best-practices-network-security/example3design.png "NVA, NSG ve UDR ile çift yönlü DMZ"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "güvenlik duvarı kurallarını mantıksal görünümü"
[11]: ./media/best-practices-network-security/example3designoptions.png "DMZ NVA ile karma ağına bağlı"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ ile bir siteden siteye VPN kullanarak bağlanan NVA"
[13]: ./media/best-practices-network-security/example4networklogical.png "NVA açısından mantıksal ağ"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure ağ geçidi ile siteden siteye karma ağına bağlı"
[15]: ./media/best-practices-network-security/example5designs2s.png "siteden siteye VPN kullanarak Azure ağ geçidi ile DMZ"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure ağ geçidi ile ExpressRoute karma ağına bağlı"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ ile bir ExpressRoute bağlantı kullanarak Azure Gateway"

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
