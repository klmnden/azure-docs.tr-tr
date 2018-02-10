---
title: "Azure'da giden bağlantılar anlama | Microsoft Docs"
description: "Bu makalede, Azure ortak Internet Hizmetleri ile iletişim kurmak sanal makineleri nasıl sağladığını açıklar."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Azure’da giden bağlantıları anlama

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Azure müşteri dağıtımları için giden bağlantı birkaç farklı yollarla sağlar.  Bu makalede senaryolar nelerdir, bunlar uygulandığında, nasıl çalıştığını ve bunların nasıl yönetileceğini açıklar.

Bir dağıtımda Azure dışında Azure ortak IP adres alanındaki uç noktalar ile iletişim kurabilir. Bir örneği ortak IP adresi alanı içindeki bir hedefe giden bir akışı başlattığında, Azure özel IP adresi genel bir IP adresi dinamik olarak eşler.  Bu eşleme oluşturulduktan sonra bu giden kaynaklı akışı dönüş trafiği de özel IP adresi akış geldiği ulaşabilir.

Azure kaynak ağ adresi çevirisi (SNAT) bu işlemi gerçekleştirmek için kullanır.  Birden çok özel IP adresleri, tek bir ortak IP adresi maskelemeyi Azure kullanır [adresi çevirisi (PAT) bağlantı noktası](#pat) özel IP adresleri geçici için.  Kısa ömürlü bağlantı noktaları PAT için kullanılır ve [önceden ayrılmış](#preallocatedports) havuzu boyutuna göre.

Vardır birden çok [giden senaryoları](#scenarios). Bu senaryolar, gerektiğinde birleştirilebilir. Dağıtım modeli için uygulamak gibi özellikleri, kısıtlamalar ve desenler dikkatle anlamak için bunları gözden geçirin ve uygulama senaryosu.  Gözden geçirmek için yönergeler [bu senaryoları yönetme](#snatexhaust).

## <a name="scenarios"></a>Senaryoya genel bakış

Azure iki ana dağıtım modeline sahiptir: Azure Resource Manager ve klasik. Yük Dengeleyici ve ilgili kaynakları açıkça tanımlanmış kullanırken [Azure Resource Manager kaynaklarını](#arm).  Klasik dağıtımlar bir yük dengeleyici kavramı soyut ve uç noktaları tanımını aracılığıyla benzer bir işlev hızlı bir [bulut hizmeti](#classic). Geçerli [senaryoları](#scenarios) üzerinde hangi dağıtım modeli kullanılır dağıtımınız için bağımlı.

### <a name="arm"></a>Azure Resource Manager

Azure şu anda Azure Resource Manager kaynakları için giden bağlantı ulaşmak için üç farklı yöntem sağlar.  [Klasik](#classic) dağıtımları sahip bir alt kümesini bu senaryoları.

| Senaryo | Yöntem | Açıklama |
| --- | --- | --- |
| [1. VM örnek düzeyinde ortak IP adresiyle (ile veya yük dengeleyici olmadan)](#ilpip) | SNAT, bağlantı noktası maskelemeyi kullanılmıyor |Azure örneğin NIC IP yapılandırması için atanan ortak IP kullanır  Örneğinin tüm kısa ömürlü bağlantı noktaları kullanılabilir vardır. |
| [2. VM (örneği üzerinde örnek düzeyinde ortak IP adres yok) ile ilişkili ortak yük dengeleyici](#lb) | Yük Dengeleyici ön uçları bağlantı noktası (PAT) maskelemeyi ile SNAT kullanma |Azure ortak IP adresi ön uçları birden çok özel IP adresleri ve ön uçları PAT için kısa ömürlü bağlantı noktalarını kullanır ortak yük dengeleyicinin paylaşır. |
| [3. Tek başına VM (yük dengeleyici, örnek düzeyinde ortak IP adresi yok)](#defaultsnat) | Bağlantı noktası (PAT) maskelemeyi ile SNAT | Azure otomatik olarak snat Uygulamanız için bir ortak IP adresi atar, bu ortak IP adresi birden fazla özel IP adreslerini kullanılabilirlik kümesi ile paylaşır ve bu genel IP adresi kısa ömürlü bağlantı noktalarını kullanır.  Bu senaryo 1 ve 2 önceki senaryoları için bir geri dönüş senaryodur ve görünürlük ve denetim gerekip gerekmediğini önerilmez. |

Azure ortak IP adres alanındaki dışında uç noktalar ile iletişim kurmak için bir VM istemiyorsanız gerektiğinde erişimi engellemek için ağ güvenlik grupları (NSG) kullanabilirsiniz.  Nsg'leri kullanarak ele alınmıştır daha ayrıntılı olarak [giden bağlantıyı engelliyor](#preventoutbound).  Tasarım ve bu makalenin kapsamı dışında herhangi bir giden erişim olmayan bir sanal ağ yönetme hakkında yönergeler ve uygulama ayrıntı tasarlayın.

### <a name="classic"></a>Klasik (bulut Hizmetleri)

Klasik dağıtımlar için kullanılabilir bir alt kümesini senaryoları için kullanılabilir senaryolardır [Azure Resource Manager](#arm) dağıtımları ve yük dengeleyici temel.

Klasik sanal makine için Azure Resource Manager kaynakları açıklandığı gibi aynı üç temel senaryo vardır ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Klasik Web çalışanı rolü yalnızca iki senaryo vardır ([2](#lb), [3](#defaultsnat)).  [Hafifletme stratejileri](#snatexhaust) de aynı farklar vardır.

İçin kullanılan algoritma [kısa ömürlü bağlantı noktaları ön tahsis](#ephemeralprots) için PAT Klasik dağıtımlar için Azure Resource Manager kaynak dağıtımları ile aynıdır.  

### <a name="ilpip"></a>Senaryo 1: VM bir örnek düzeyinde ortak IP adresi ile

Bu senaryoda, VM bir örnek düzeyinde ortak IP (atanmış ILPIP) sahiptir. Giden bağlantılar kaygı kadar VM veya dengelendiği olup, önemli değildir.  Bu senaryo başkalarının önceliklidir. Bir ILPIP kullanıldığında, VM için tüm giden trafik akışları ILPIP kullanır.  

Bağlantı noktası maskelenmiş (PAT) kullanılmaz ve tüm kısa ömürlü bağlantı noktaları kullanılabilir VM sahiptir.

Uygulamanız çok sayıda giden trafik akışları başlatır ve SNAT bağlantı noktası Tükenme deneyimi, atamayı düşünebilirsiniz bir [SNAT kısıtlamaları azaltmak için ILPIP](#assignilpip). Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) onu okumalıdır.

### <a name="lb"></a>Senaryo 2: Yük dengeli VM örnek düzeyinde ortak IP adresi olmadan

Bu senaryoda, VM bir genel yük dengeleyici arka uç havuzu bir parçasıdır.  VM, kendisine atanmış bir ortak IP adresi yok. Yük Dengeleyici kaynak ortak IP ön uç arka uç havuzu ile arasında bir bağlantı oluşturmak için bir yük dengeleyici kuralı ile yapılandırılmalıdır. Bu kural yapılandırmasını tamamlamazsanız bu senaryo için açıklandığı gibi davranıştır [hiçbir örnek düzeyinde ortak IP ile tek başına VM](#defaultsnat).  Kural başarılı olması için çalışma dinleyiciyi arka uç havuzu ya da durum araştırması gerekli değildir.

Yük dengeli VM giden akış oluşturduğunda, Azure ortak yük dengeleyici ön uç genel IP adresine giden akış özel kaynak IP adresi çevirir. Azure bu işlemi gerçekleştirmek için kaynak ağ adresi çevirisi (SNAT) kullanan yanı [adresi çevirisi (PAT) bağlantı noktası](#pat) geçici bir ortak IP adresi arkasında birden çok özel IP adresleri için. Yük dengeleyicinin genel IP adresi ön uç kısa ömürlü bağlantı noktaları, tek tek akışları VM tarafından kaynaklanan ayırt etmek için kullanılır. SNAT dinamik olarak kullanan [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) giden trafik akışları oluşturduğunuzda. Bu bağlamda snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümünde ve tükenmiş olabilir sınırlı bir kaynaktır. Ne olduğunu anlamak önemlidir [tüketilen](#pat). Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) için tasarımı ve gerektiğinde etkisini anlamak için.

Zaman [(Genel) birden çok IP adresi bir yük dengeleyici temel ile ilişkili](load-balancer-multivip-overview.md), tüm bu ortak IP adresleridir bir [giden trafik akışları ve bir aday seçili](#multivipsnat).  

Kullanabileceğiniz [yük dengeleyici için günlük analizi](load-balancer-monitor-log.md) ve [snat Uygulamanız için izleme için uyarı olay günlüklerini bağlantı noktası Tükenme iletileri](load-balancer-monitor-log.md#alert-event-log) yük dengeleyici temel giden bağlantılarla sağlığını izlemek için.

### <a name="defaultsnat"></a>Senaryo 3: Tek başına VM örnek düzeyinde ortak IP adresi olmadan

VM, Azure yük dengeleyici havuzu parçası olmayan ve kendisine atanmış bir örnek düzeyinde ortak IP (ILPIP) adresi yok. VM giden akış oluşturduğunda, Azure ortak kaynak IP adresine giden akış özel kaynak IP adresi çevirir. Bu giden akış için kullanılan ortak IP adresi yapılandırılabilir değildir ve aboneliğin ortak IP kaynak sınırınızı sayılmaz. Azure kaynak ağ adresi çevirisi (SNAT) bağlantı noktası maskelemeyi ile kullanır ([PAT](#pat)) bu işlemi gerçekleştirmek için. Bu senaryo benzer [Senaryo 2](#lb), var. dışında kullanılan IP adresi üzerinde denetimi yoktur.  Bu, ne zaman senaryoları 1 ve 2. Senaryo mevcut için geri dönüş bir senaryodur.  Giden adres üzerinde denetim isterseniz bu senaryo önerilmez.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümünde ve tükenmiş olabilir sınırlı bir kaynaktır. Ne olduğunu anlamak önemlidir [tüketilen](#pat). Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) için tasarımı ve gerektiğinde etkisini anlamak için.

### <a name="combinations"></a>Birden çok, birleştirilmiş senaryoları

Belirli bir sonucu elde etmek için önceki bölümlerde açıklanan senaryoları birleştirilebilir.  Birden fazla senaryoyu mevcut olduğunda bir öncelik sırası geçerlidir: [Senaryo 1](#ilpip) önceliklidir [Senaryo 2](#lb) ve [3](#defaultsnat) (yalnızca Azure Kaynak Yöneticisi) ve [Senaryo 2](#lb) geçersiz kılmaları [Senaryo 3](#defaultsnat) (Azure Resource Manager ve klasik).

Burada uygulama yoğun hedefleri sınırlı sayıda giden bağlantılar kullanır, ancak Ayrıca, bir yük dengeleyici ön gelen akışları alır Azure Resource Manager dağıtım örneğidir. Bu durumda, 1 ve 2 Tahliye için senaryolar birleştirebilirsiniz.  Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) ek desenler için.

### <a name="multivipsnat"></a>Giden trafik akışları için birden çok ön Uçlar

Yük Dengeleyici temel seçin tek bir ön uç kullanılan giden trafik akışları olmasını zaman [birden çok (Genel) IP ön uçlar](load-balancer-multivip-overview.md) giden trafik akışları adaylar.  Bu seçim yapılandırılabilir değildir ve seçim algoritması rasgele olarak düşünülmelidir.  Belirli bir IP adresi için belirleyebileceğiniz açıklandığı gibi giden [senaryoları birleştirilmiş](#combinations).

## <a name="snat"></a>SNAT ve PAT anlama

### <a name="pat"></a>Bağlantı noktası maskelenmiş SNAT (PAT)

Ortak bir yük dengeleyici kaynak VM örnekleriyle ilişkili olduğunda, her giden bağlantı kaynağı yeniden yazılmıştır. Kaynak sanal ağ özel IP adres alanından yük dengeleyici ön uç genel IP adresi yeniden yazılmıştır. Ortak IP adres alanı, 5-tanımlama grubu (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) akışının benzersiz olması gerekir.  Kısa ömürlü bağlantı noktaları, tek bir ortak IP adresini birden çok akış kaynaklanan beri özel kaynak IP adresini yeniden yazma işlemi sonra bunun için kullanılır.  Bu kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir. 

Akış tek hedef IP adresi, bağlantı noktası ve protokol başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokol birden çok akışlar için her bir akışa tek bir SNAT bağlantı noktasını kullanır. Bu akış aynı hedef IP adresi, bağlantı noktası ve protokol aynı ortak IP adresinden kaynaklanan zaman benzersiz olmasını sağlar. 

Her farklı bir hedef IP adresi, bağlantı noktası ve protokolü, birden çok akış tek bir SNAT bağlantı noktası paylaşır. Hedef IP adresi, bağlantı noktası ve protokol yapar akışları ek kaynak gerek kalmadan benzersiz ortak IP adresi alanı akışlarında ayırt etmek için kullanılacak bağlantı noktaları.

SNAT bağlantı noktası kaynakları tükendi SNAT bağlantı noktaları tarafından varolan akışları serbest bırakılana kadar giden trafik akışları yük devredersiniz. Yük Dengeleyici akışı kapatır ve kullandığında SNAT bağlantı noktalarını geri kazanır bir [4 dakikalık boşta kalma zaman aşımı](#idletimeout) boşta akışları SNAT noktalarından geri kazanma için.

Gözden geçirme [yönetme SNAT](#snatexhaust) ve yaygın olarak SNAT bağlantı noktası tükenmesine yol koşulları azaltmak desenler için bölüm.

### <a name="preallocatedports"></a>Kısa ömürlü bağlantı noktası ön tahsisi SNAT (PAT) maskelemeyi bağlantı noktası

Sayısı, önceden ayrılmış SNAT kullanılabilir bağlantı noktası sayısını belirlemek için bir algoritma kullanırken arka uç havuzu boyutuna göre azure kullandığı bağlantı noktası maskelenmiş SNAT ([PAT](#pat)).  Belirli bir ortak IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktaları SNAT bağlantı noktalarıdır.

Azure bağlantı noktalarına her VM NIC IP yapılandırmasını SNAT preallocates. Bir IP yapılandırması havuza eklendiğinde, SNAT bağlantı noktaları arka uç havuzu boyutuna göre bu IP yapılandırması için önceden ayrılmış. Klasik Web çalışanı rolleri için ayırma rol örneğidir.  Giden trafik akışları oluşturduğunuzda, [PAT](#pat) dinamik olarak (önceden ayrılmış sınıra kadar) kullanır ve akış kapandığında Bu bağlantı noktaları serbest veya [boş durma zaman aşımlarını](#ideltimeout).

Aşağıdaki tabloda SNAT bağlantı noktası preallocations arka uç havuzu boyutlarda katmanları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına ön tahsis SNAT bağlantı noktaları|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Kullanılabilir SNAT bağlantı noktası sayısını doğrudan bağlantı sayısını tercüme etmez unutmamak önemlidir. Tek bir SNAT bağlantı noktası benzersiz birden çok varış yeri için yeniden kullanılabilir. Bağlantı noktaları, Akışlar benzersiz hale getirmek gerekliyse, yalnızca tüketilen. Başvurmak [exhaustible bu kaynak yönetme](#snatexhaust) tasarım ve azaltma kılavuzunu yanı sıra açıklayan bölüm için [PAT](#pat).

Arka uç havuzu boyutunu değiştirme bazı yerleşik akışlarının etkileyebilir. Arka uç havuzu boyutunu artırır ve sonraki katmanla geçişleri, ön tahsis SNAT bağlantı noktalarını yarısı sonraki daha büyük arka uç havuzu katmanına geçiş sırasında geri kazanılır. Reclaimed SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrayacağı ve kurulmaları gerekir. Ön tahsis bağlantı noktaları kullanılabilir olduğu sürece yeni bağlantı girişimleri hemen başarılı.

Arka uç havuzu boyutunu azaltır ve daha düşük bir katman geçişleri, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, varolan SNAT bağlantı noktalarını ayrılmış ve bunların ilgili akışları etkilenmez.

## <a name="snatexhaust"></a>SNAT (PAT) bağlantı noktası Tükenme yönetme

[Kısa ömürlü bağlantı noktaları](#preallocatedports) için kullanılan [PAT](#pat) açıklandığı gibi bir exhaustible kaynağı olan [örnek düzeyinde ortak IP adresi olmadan tek başına VM](#defaultsnat) ve [olmadan yük dengeli VM Örnek düzeyinde ortak IP adresi](#lb).

Aynı hedef IP adresine ve bağlantı noktası, birçok giden TCP veya UDP bağlantı başlatma ve giden bağlantılar başarısız inceleyin veya destek tarafından önerilir biliyorsanız tükettiğini SNAT bağlantı noktalarının (önceden ayrılmış [kısa ömürlü bağlantı noktaları](#preallocatedports)tarafından kullanılan [PAT](#pat)), birkaç genel azaltma seçeneğiniz vardır.  Bu seçenekleri gözden geçirin ve ne kullanılabilir ve senaryonuz için en iyisi olduğuna karar verin.  Olası bir olan veya daha fazla bu senaryo yönetmenize yardımcı olabilir.

Giden bağlantı davranışını anlama ile ilgili sorunlar yaşıyorsanız, IP yığını istatistikleri (netstat) kullanabilir veya paket yakalamaları kullanarak bağlantı davranışlarla için faydalı olabilir.  Bu paket yakalamaları örneğinizi konuk işletim sistemi içinde gerçekleştirmek ya da kullanmak [paket yakalama için Ağ İzleyicisi](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Bağlantılarını yeniden uygulamasını değiştirme 
Uygulamanızdaki bağlantıları yeniden kullanarak snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir.  Bu varsayılan HTTP/1.1 burada bağlantıyı yeniden olduğu gibi protokoller için özellikle doğrudur.  Ve taşıma (yani REST) için HTTP kullanan diğer protokolleri sırayla yararlı olabilir.  Yeniden her zaman tek tek, atomik TCP bağlantıları her istek için daha iyi ve daha fazla kullanıcı çok verimli TCP işlemleri sonuçlanır.

### <a name="connection pooling"></a>Bağlantı havuzu kullanmak için uygulamasını değiştirme
Bir bağlantı, uygulamanızda sabit kümesi (her mümkün olduğunca yeniden) bağlantıları istekleri dahili olarak dağıtıldığı düzeni havuzu tercih edebilirsiniz.  Bu kısıtlamalar kısa ömürlü sayısı kullanımda bağlantı noktaları ve daha tahmin edilebilir bir ortam oluşturur.  Tek bir bağlantı üzerinde bir işlemi cevap engelleme zaman birden çok eşzamanlı operasyonlar vererek bu istekleri üretimini de artırabilirsiniz.  Bağlantı havuzu uygulamanızı veya uygulamanız için yapılandırma ayarlarını geliştirmek için kullanmakta olduğunuz framework içinde zaten olabilir.  Bu bağlantı yeniden ile birleştirebilirsiniz ve aynı zamanda gecikme süresi ve kaynak azaltma TCP işlemleri çok verimli kullanımdan benefiting çalışırken bağlantı noktalarını aynı hedef IP adresine ve bağlantı noktası sabit, tahmin edilebilir sayısı, birden çok istek kullanma kullanımı.  UDP işlemler de avantajını numarası yönetilmesi UDP akışları sırayla Egzoz koşullar önlemek ve SNAT bağlantı noktası kullanımı yönetebilirsiniz.

### <a name="retry logic"></a>Uygulamanızı daha az agresif yeniden deneme mantığı kullanacak şekilde değiştirin
Zaman [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) için kullanılan [PAT](#pat) olan tükendi veya uygulama hataları, agresif ya da kaba kuvvet decay ve geri Çekilme mantığı neden olmadan Tükenme oluşur veya kalıcı hale getirmek yeniden deneme sayısı. Daha az agresif bir yeniden deneme mantığı kullanarak kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir.   Kısa ömürlü bağlantı noktaları 4 dakikalık boşta kalma zaman aşımı (ayarlanamıyor) varsa ve deneme çok agresif varsa, Tükenme, kendi temizlenir fırsatı.  Bu nedenle, uygulamanızı nasıl ve ne sıklıkta işlemleri yeniden deneme dikkate bir kritik tasarım konusudur.

### <a name="assignilpip"></a>Her VM için bir örnek düzeyinde ortak IP atayın
Bu senaryonuza değiştirir [örnek düzeyinde ortak IP bir VM](#ilpip).  Her VM için kullanılan genel IP tüm kısa ömürlü bağlantı noktaları (aksine, burada bir genel IP kısa ömürlü bağlantı noktaları tüm VM ile ilgili arka uç havuzu ile ilişkili paylaşılan senaryoları) VM kullanılabilir.  Ek bir maliyet ortak IP adresleri ve olası etkisini uygulamaları güvenilir listeye almayı çok sayıda tek tek IP adresleri gibi göz önünde bulundurmanız dengelemeler vardır.

>[!NOTE] 
>Bu seçenek, Web çalışanı rolleri için kullanılamaz.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımı sıfırlamak için ayarlandığında Canlı tutmalar kullanın

Giden bağlantılar 4 dakikalık boşta kalma zaman aşımı vardır. Bu ayarlanabilir değildir. Ancak, Aktarım (yani TCP etkin tutma) kullanabilir veya boş bir akış yenilemek ve bu boşta kalma zaman aşımı sıfırlamak için uygulama katmanı ayarlandığında Canlı tutmalar gerekli.

## <a name="discoveroutbound"></a>Belirli bir VM tarafından kullanılan genel IP bulma
Giden bir bağlantıyı ortak kaynak IP adresi belirlemek için birçok yolu vardır. OpenDNS VM ortak IP adresini gösteren bir hizmet sunar. Nslookup komutunu kullanarak, ad myip.opendns.com için bir DNS sorgusu OpenDNS çözümleyici gönderebilirsiniz. Hizmet sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Sanal makineden aşağıdaki sorguyu çalıştırın, yanıt o VM için kullanılan genel IP değildir.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Giden bağlantıyı engelliyor
Bazen bir çıkış akışı oluşturmak için izin verilmesi bir VM için istenmeyen veya hangi hedefleri giden trafik akışları ile ulaşılabilen yönetmek için bir zorunluluk olabilir veya hangi hedefleri gelen akışları başlayabilir. Bu durumda, kullandığınız [ağ güvenlik grupları (NSG)](../virtual-network/virtual-networks-nsg.md) VM de ulaşabilir hedefleri yönetmek için hangi ortak hedef olarak gelen akışları başlatabilirsiniz. Yük dengeli bir VM için bir NSG uyguladığınızda, dikkat edilmesi gereken [varsayılan etiketleri](../virtual-network/virtual-networks-nsg.md#default-tags) ve [varsayılan kuralları](../virtual-network/virtual-networks-nsg.md#default-rules).

VM, sistem durumu araştırma isteklerine Azure yük Dengeleyiciden aldığından emin olmak gerekir. Bir NSG'yi sistem durumu araştırma AZURE_LOADBALANCER varsayılan etiket isteklerinden engelliyorsa VM durumu araştırması başarısız olur ve VM düşürüleceği. Yük Dengeleyici, bu VM için yeni akışları gönderme durdurur.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yük dengeleyici temel](load-balancer-overview.md).
- Daha fazla bilgi edinmek [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md).
- Başka bir anahtar bazıları hakkında bilgi edinin [ağı yetenekleri](../networking/networking-overview.md) azure'da.
