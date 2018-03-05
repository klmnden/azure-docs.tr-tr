---
title: "Azure'da giden bağlantılar | Microsoft Docs"
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
ms.openlocfilehash: 1c776d94d217622186d880352c518ad5a34b0949
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="outbound-connections-in-azure"></a>Azure'da giden bağlantılar

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Azure müşteri dağıtımları için giden bağlantı birkaç farklı yollarla sağlar. Bu makalede senaryolar nelerdir, bunlar uygulandığında, nasıl çalıştığını ve bunların nasıl yönetileceğini açıklar.

Bir dağıtımda Azure dışında Azure ortak IP adres alanındaki uç noktalar ile iletişim kurabilir. Bir örneği ortak IP adresi alanı içindeki bir hedefe giden bir akışı başlattığında, Azure özel IP adresi genel bir IP adresi dinamik olarak eşler. Bu eşleme oluşturulduktan sonra bu giden kaynaklı akışı dönüş trafiği de özel IP adresi akış geldiği ulaşabilir.

Azure kaynak ağ adresi çevirisi (SNAT) bu işlemi gerçekleştirmek için kullanır. Birden çok özel IP adresleri, tek bir ortak IP adresi maskelemeyi Azure kullanır [adresi çevirisi (PAT) bağlantı noktası](#pat) özel IP adresleri geçici için. Kısa ömürlü bağlantı noktaları PAT için kullanılır ve [önceden ayrılmış](#preallocatedports) havuzu boyutuna göre.

Vardır birden çok [giden senaryoları](#scenarios). Bu senaryolar, gerektiğinde birleştirebilirsiniz. Dağıtım modeli için uygulamak gibi özellikleri, kısıtlamalar ve desenler dikkatle anlamak için bunları gözden geçirin ve uygulama senaryosu. Gözden geçirmek için yönergeler [bu senaryoları yönetme](#snatexhaust).

## <a name="scenarios"></a>Senaryoya genel bakış

Azure iki ana dağıtım modeline sahiptir: Azure Resource Manager ve klasik. Azure yük dengeleyici ve ilgili kaynakları açıkça tanımlanmış kullanırken [Azure Resource Manager](#arm). Klasik dağıtımlar bir yük dengeleyici kavramı soyut ve uç noktaları tanımını aracılığıyla benzer bir işlev hızlı bir [bulut hizmeti](#classic). Geçerli [senaryoları](#scenarios) bağımlı dağıtımınız için hangi dağıtım modeli kullanırsınız.

### <a name="arm"></a>Azure Resource Manager

Azure şu anda Azure Resource Manager kaynakları için giden bağlantı ulaşmak için üç farklı yöntem sağlar. [Klasik](#classic) dağıtımları sahip bir alt kümesini bu senaryoları.

| Senaryo | Yöntem | Açıklama |
| --- | --- | --- |
| [1. Örnek düzeyinde ortak IP adresi (ile veya yük dengeleyici olmadan) olan VM](#ilpip) | SNAT, bağlantı noktası maskelemeyi kullanılmıyor |Azure örneğin NIC IP yapılandırması için atanan ortak IP kullanır Örneğinin tüm kısa ömürlü bağlantı noktaları kullanılabilir vardır. |
| [2. Bir VM'yi (örneği üzerinde örnek düzeyinde ortak IP adres yok) ile ilişkili ortak yük dengeleyici](#lb) | Yük Dengeleyici ön uçlar bağlantı noktası (PAT) maskelemeyi ile SNAT kullanma |Azure ortak yük dengeleyici ön uçlar genel IP adresi ile birden çok özel IP adresleri paylaşır. Azure PAT için ön uçlar kısa ömürlü bağlantı noktalarını kullanır. |
| [3. Tek başına VM (yük dengeleyici, örnek düzeyinde ortak IP adresi yok)](#defaultsnat) | Bağlantı noktası (PAT) maskelemeyi ile SNAT | Azure otomatik olarak snat Uygulamanız için bir ortak IP adresi atar, bu ortak IP adresi birden fazla özel IP adreslerini kullanılabilirlik kümesi ile paylaşır ve bu genel IP adresi kısa ömürlü bağlantı noktalarını kullanır. Bu, önceki senaryoları için geri dönüş bir senaryodur. Görünürlük ve denetim gerekiyorsa bunu yapmanız önerilmez. |

Azure ortak IP adres alanındaki dışında uç noktalar ile iletişim kurmak için bir VM istemiyorsanız, gerektiğinde erişimi engellemek için ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz. Bölüm [giden bağlantıyı engelliyor](#preventoutbound) Nsg'ler daha ayrıntılı olarak anlatılmaktadır. Tasarım Kılavuzu, uygulama, herhangi bir giden erişim olmadan bir sanal ağ yönetme bu makalenin kapsamı dışındadır ise.

### <a name="classic"></a>Klasik (bulut Hizmetleri)

Klasik dağıtımlar için kullanılabilir bir alt kümesi için kullanılabilir senaryoları senaryolardır [Azure Resource Manager](#arm) dağıtımları ve yük dengeleyici temel.

Klasik sanal makine için Azure Resource Manager kaynakları açıklandığı gibi aynı üç temel senaryo vardır ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Klasik web çalışanı rolü yalnızca iki senaryo vardır ([2](#lb), [3](#defaultsnat)). [Hafifletme stratejileri](#snatexhaust) de aynı farklar vardır.

İçin kullanılan algoritma [kısa ömürlü bağlantı noktaları ön tahsis](#ephemeralprots) için PAT Klasik dağıtımlar için Azure Resource Manager kaynak dağıtımları ile aynıdır.  

### <a name="ilpip"></a>Senaryo 1: VM bir örnek düzeyinde ortak IP adresi ile

Bu senaryoda, VM bir örnek düzeyinde ortak IP (atanmış ILPIP) sahiptir. Giden bağlantılar kaygı kadar VM veya dengelendiği olup önemli değildir. Bu senaryo başkalarının önceliklidir. Bir ILPIP kullanıldığında, VM için tüm giden trafik akışları ILPIP kullanır.  

Bağlantı noktası maskelenmiş (PAT) kullanılmaz ve tüm kısa ömürlü bağlantı noktaları kullanılabilir VM sahiptir.

Uygulamanız çok sayıda giden trafik akışları başlatır ve SNAT bağlantı noktası Tükenme deneyimi, atama göz önünde bulundurun bir [SNAT kısıtlamaları azaltmak için ILPIP](#assignilpip). Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) okumalıdır.

### <a name="lb"></a>Senaryo 2: Yük dengeli VM örnek düzeyinde ortak IP adresi olmadan

Bu senaryoda, VM bir genel yük dengeleyici arka uç havuzu bir parçasıdır. VM, kendisine atanmış bir ortak IP adresi yok. Yük Dengeleyici kaynak ortak IP ön uç arka uç havuzu ile arasında bir bağlantı oluşturmak için bir yük dengeleyici kuralı ile yapılandırılmalıdır. 

Bu kural yapılandırmasını tamamlamazsanız bu senaryo için açıklandığı gibi davranıştır [hiçbir örnek düzeyinde ortak IP ile tek başına VM](#defaultsnat). Kuralın başarılı olması sistem durumu araştırması için arka uç havuzundaki çalışma dinleyicisi olması gerekli değildir.

Yük dengeli VM giden akış oluşturduğunda, Azure ortak yük dengeleyici ön uç genel IP adresine giden akış özel kaynak IP adresi çevirir. Azure SNAT bu işlemi gerçekleştirmek için kullanır. Ayrıca Azure kullanır [PAT](#pat) geçici bir ortak IP adresi arkasında birden çok özel IP adresleri için. 

Yük dengeleyicinin genel IP adresi ön uç kısa ömürlü bağlantı noktaları, tek tek akışları VM tarafından kaynaklanan ayırt etmek için kullanılır. SNAT dinamik olarak kullanan [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) giden trafik akışları oluşturduğunuzda. Bu bağlamda snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü. Bitti sınırlı bir kaynak olup olmadıklarını. Ne olduğunu anlamak önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiğinde etkisini anlamak için gözden [yönetme SNAT Tükenme](#snatexhaust).

Zaman [(Genel) birden çok IP adresini yük dengeleyici temel ile ilişkili](load-balancer-multivip-overview.md), her türlü bu ortak IP adresleridir bir [giden trafik akışları için aday](#multivipsnat), ve biri seçilidir.  

Yük Dengeleyici temel giden bağlantılarla sağlığını izlemek için kullanabileceğiniz [yük dengeleyici için günlük analizi](load-balancer-monitor-log.md) ve [uyarı olayı günlükleri](load-balancer-monitor-log.md#alert-event-log) SNAT bağlantı noktası Tükenme iletileri izlemek için.

### <a name="defaultsnat"></a>Senaryo 3: Tek başına VM örnek düzeyinde ortak IP adresi olmadan

Bu senaryoda, VM Azure yük dengeleyici havuzu parçası olmayan ve kendisine atanmış bir ILPIP adresi yok. VM giden akış oluşturduğunda, Azure ortak kaynak IP adresine giden akış özel kaynak IP adresi çevirir. Bu giden akış için kullanılan ortak IP adresi yapılandırılabilir değildir ve aboneliğin ortak IP kaynak sınırınızı sayılmaz. 

Azure bağlantı noktası maskelemeyi ile SNAT kullanır ([PAT](#pat)) bu işlemi gerçekleştirmek için. Bu senaryo benzer [Senaryo 2](#lb), var. dışında kullanılan IP adresi üzerinde denetimi yoktur. Bu, ne zaman 1 ve 2 senaryoları mevcut için geri dönüş bir senaryodur. Giden adres üzerinde denetim isterseniz, bu senaryo öneririz yok.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü. Bitti sınırlı bir kaynak olup olmadıklarını. Ne olduğunu anlamak önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiğinde etkisini anlamak için gözden [yönetme SNAT Tükenme](#snatexhaust).

### <a name="combinations"></a>Birden çok, birleştirilmiş senaryoları

Belirli bir sonucu elde etmek için önceki bölümlerde açıklanan senaryoları birleştirebilirsiniz. Birden fazla senaryoyu mevcut olduğunda bir öncelik sırası geçerlidir: [Senaryo 1](#ilpip) önceliklidir [Senaryo 2](#lb) ve [3](#defaultsnat) (yalnızca Azure Resource Manager). [Senaryo 2](#lb) geçersiz kılmaları [Senaryo 3](#defaultsnat) (Azure Resource Manager ve klasik).

Burada uygulama yoğun hedefleri sınırlı sayıda giden bağlantılar kullanır, ancak Ayrıca, bir yük dengeleyici ön gelen akışları alır Azure Resource Manager dağıtım örneğidir. Bu durumda, senaryoları Tahliye için 1 ve 2 birleştirebilirsiniz. Ek desenler için gözden [yönetme SNAT Tükenme](#snatexhaust).

### <a name="multivipsnat"></a> Giden trafik akışları için birden çok ön Uçlar

Yük Dengeleyici temel seçer giden akışlar için kullanılacak tek bir ön uç zaman [birden çok (Genel) IP ön uçlar](load-balancer-multivip-overview.md) giden trafik akışları için aday değildir. Bu seçim yapılandırılabilir değildir ve rastgele seçimi algoritmasının göz önünde bulundurmanız gerekir. Giden trafik akışları için belirli bir IP adresi açıklandığı gibi atayabilirsiniz [birden çok, birleştirilmiş senaryoları](#combinations).

## <a name="snat"></a>SNAT ve PAT anlama

### <a name="pat"></a>Bağlantı noktası maskelenmiş SNAT (PAT)

Ortak bir yük dengeleyici kaynak VM örnekleriyle ilişkili olduğunda, her giden bağlantı kaynağı yeniden yazılmıştır. Kaynak sanal ağ özel IP adres alanından ön yük dengeleyicinin genel IP adresi yeniden yazılmıştır. Ortak IP adres alanı, 5-tanımlama grubu (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) akışının benzersiz olması gerekir.  

Kısa ömürlü bağlantı noktaları (SNAT) tek bir ortak IP adresini birden çok akış kaynaklı olduğundan bu özel kaynak IP adresini yeniden yazma işlemi sonra elde etmek için kullanılır. 

Akış tek hedef IP adresi, bağlantı noktası ve protokol başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokol birden çok akışlar için her bir akışa tek bir SNAT bağlantı noktasını kullanır. Bu, aynı ortak IP adresinden kaynaklanan ve aynı hedef IP adresi, bağlantı noktası ve protokol Git akışları benzersiz olmasını sağlar. 

Her farklı bir hedef IP adresi, bağlantı noktası ve protokolü, birden çok akış tek bir SNAT bağlantı noktası paylaşır. Hedef IP adresi, bağlantı noktası ve protokol akışları ortak IP adresi alanı akışlarında ayırt etmek için bağlantı noktalarını ek kaynak gerek kalmadan benzersiz olun.

SNAT bağlantı noktası kaynakları tükendi, varolan akışları SNAT bağlantı noktalarını bırakana kadar giden trafik akışları başarısız. Yük Dengeleyici akışı kapatır ve kullandığında SNAT bağlantı noktalarını geri kazanır bir [4 dakikalık boşta kalma zaman aşımı](#idletimeout) boşta akışları SNAT noktalarından geri kazanma için.

Yaygın olarak SNAT bağlantı noktası tükenmesine yol koşulları azaltmak desenler için gözden [yönetme SNAT](#snatexhaust) bölümü.

### <a name="preallocatedports"></a>Kısa ömürlü bağlantı noktası ön tahsisi SNAT (PAT) maskelemeyi bağlantı noktası

Sayısı, önceden ayrılmış SNAT kullanılabilir bağlantı noktası sayısını belirlemek için bir algoritma kullanırken arka uç havuzu boyutuna göre azure kullandığı bağlantı noktası maskelenmiş SNAT ([PAT](#pat)). Belirli bir ortak IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktaları SNAT bağlantı noktalarıdır.

Azure bağlantı noktalarına her VM NIC IP yapılandırmasını SNAT preallocates. Bir IP yapılandırması havuza eklendiğinde, SNAT bağlantı noktaları arka uç havuzu boyutuna göre bu IP yapılandırması için önceden ayrılmış. Klasik web çalışanı rolleri için ayırma rol örneğidir. Giden trafik akışları oluşturduğunuzda, [PAT](#pat) dinamik olarak (önceden ayrılmış sınıra kadar) kullanır ve akış kapandığında Bu bağlantı noktaları serbest veya [boş durma zaman aşımlarını](#ideltimeout) gerçekleşir.

Aşağıdaki tabloda SNAT bağlantı noktası preallocations arka uç havuzu boyutlarda katmanları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına ön tahsis SNAT bağlantı noktaları|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

Kullanılabilir SNAT bağlantı noktası sayısını doğrudan akışları sayıya tercüme etmez unutmayın. Tek bir SNAT bağlantı noktası benzersiz birden çok varış yeri için yeniden kullanılabilir. Yalnızca akış benzersiz hale getirmek için gerekli olduğunda bağlantı noktaları kullanılır. Tasarım ve azaltma kılavuzu için ilgili bölümüne bakın. [exhaustible bu kaynak yönetme](#snatexhaust) ve açıklayan bölümü [PAT](#pat).

Arka uç havuzu boyutunu değiştirme bazı yerleşik akışlarının etkileyebilir. Arka uç havuzu boyutunu artırır ve sonraki katmanla geçişleri, ön tahsis SNAT bağlantı noktalarını yarısı sonraki daha büyük arka uç havuzu katmanına geçiş sırasında geri kazanılır. Reclaimed SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrar ve kurulmaları gerekir. Yeni bir akış bulamazsa, ön tahsis bağlantı noktaları kullanılabilir olduğu sürece, akış hemen başarılı olur.

Arka uç havuzu boyutunu azaltır ve daha düşük bir katman geçişleri, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, varolan SNAT bağlantı noktalarını ayrılmış ve bunların ilgili akışları etkilenmez.

## <a name="snatexhaust"></a>SNAT (PAT) bağlantı noktası Tükenme yönetme

[Kısa ömürlü bağlantı noktaları](#preallocatedports) için kullanılan [PAT](#pat) açıklandığı gibi bir exhaustible kaynağı olan [örnek düzeyinde ortak IP adresi olmadan tek başına VM](#defaultsnat) ve [yük dengeli VM olmadan bir Örnek düzeyinde ortak IP adresi](#lb).

Aynı hedef IP adresi ve bağlantı noktası fazla giden TCP veya UDP bağlantı başlatma, ve giden bağlantılar başarısız izleyebilmenizi veya destek tarafından tükettiğini SNAT bağlantı noktalarını olduğunuz önerilir biliyorsanız (önceden ayrılmış [kısa ömürlü bağlantı noktaları](#preallocatedports) tarafından kullanılan [PAT](#pat)), birkaç genel azaltma seçeneğiniz vardır. Bu seçenekleri gözden geçirin ve ne kullanılabilir ve senaryonuz için en iyisi olduğuna karar verin. Bu senaryo yönetmek bir veya daha fazla yardımcı olabilir.

Giden bağlantı davranışını anlama ile ilgili sorunlar yaşıyorsanız, IP yığını istatistikleri (netstat) kullanabilirsiniz. Veya paket yakalamaları kullanarak bağlantı davranışlarla için faydalı olabilir. Bu paket yakalamaları örneğinizi konuk işletim sistemi içinde gerçekleştirmek ya da kullanmak [paket yakalama için Ağ İzleyicisi](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Bağlantılarını yeniden uygulamaya değiştirme 
Uygulamanızdaki bağlantıları yeniden kullanarak snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir. Bu, özellikle HTTP/1.1, bağlantı yeniden varsayılan olduğu gibi protokoller için geçerlidir. Ve taşıma (örneğin, REST) HTTP kullanan diğer protokolleri sırayla yararlı olabilir. 

Yeniden her zaman tek tek, atomik TCP bağlantıları her istek için daha iyi olur. Daha fazla kullanıcı, çok verimli TCP işlemleri sonuçlarında yeniden kullanabilirsiniz.

### <a name="connection pooling"></a>Bağlantı havuzu kullanmak için uygulamayı değiştirme
Bir bağlantı, uygulamanızda sabit kümesi (her mümkün olduğunca yeniden) bağlantıları istekleri dahili olarak dağıtıldığı düzeni havuzu tercih edebilirsiniz. Bu düzen kullanımda kısa ömürlü bağlantı noktası sayısını kısıtlar ve daha tahmin edilebilir bir ortam oluşturur. Bu düzen, tek bir bağlantı üzerinde bir işlemi cevap engelleme zaman birden çok eşzamanlı operasyonlar sağlayarak isteklerin verimini de artırabilirsiniz.  

Bağlantı havuzu zaten uygulamanızı veya uygulamanız için yapılandırma ayarlarını geliştirmek için kullanmakta olduğunuz framework içinde mevcut olabilir. Bağlantı yeniden ile bağlantı havuzu birleştirebilirsiniz. Birden çok istek daha sonra aynı hedef IP adresi ve bağlantı noktası için bağlantı noktaları, sabit, tahmin edilebilir birtakım tüketir. İstek gecikme süresi ve kaynak kullanımını azaltarak TCP işlemleri verimli kullanımdan da yararlanır. UDP akışlarının sayısı yönetilmesi sırayla Egzoz koşullar önlemek ve SNAT bağlantı noktası kullanımı yönetmek için UDP işlemleri de yararlanabilir.

### <a name="retry logic"></a>Uygulamayı daha az agresif yeniden deneme mantığı kullanacak şekilde değiştirin
Zaman [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) için kullanılan [PAT](#pat) olan tükendi veya uygulama hataları, agresif ya da kaba kuvvet decay ve geri Çekilme mantığı neden olmadan Tükenme oluşur veya kalıcı hale getirmek yeniden deneme sayısı. Daha az agresif bir yeniden deneme mantığı kullanarak kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir. 

Kısa ömürlü bağlantı noktaları 4 dakikalık boşta kalma zaman aşımı (ayarlanamıyor) vardır. Yeniden deneme çok agresif Tükenme, kendi temizlenir fırsatı yoktur. Bu nedenle, uygulamanızı nasıl--ve hangi sıklıkta--işlemleri yeniden deneme dikkate bir kritik tasarım parçasıdır.

### <a name="assignilpip"></a>Her VM için bir örnek düzeyinde ortak IP atayın
Bir ILPIP atama değişiklikleri senaryonuz için [örnek düzeyinde ortak IP bir VM](#ilpip). Her VM için kullanılan tüm kısa ömürlü bağlantı noktaları genel IP, VM'ye kullanılabilir. (Burada ilgili arka uç havuzuyla ilişkili tüm sanal makineler ile bir genel IP kısa ömürlü bağlantı noktaları paylaşılan senaryolarında aksine.) Ek bir maliyet ortak IP adresleri ve olası etkisini uygulamaları güvenilir listeye almayı çok sayıda tek tek IP adresleri gibi göz önünde bulundurmanız dengelemeler vardır.

>[!NOTE] 
>Bu seçenek, web çalışanı rolleri için kullanılamaz.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımı sıfırlamak için ayarlandığında Canlı tutmalar kullanın

Giden bağlantılar 4 dakikalık boşta kalma zaman aşımı vardır. Bu zaman aşımı ayarlanabilir değil. Ancak, boş bir akış yenileyin ve gerekirse bu boşta kalma zaman aşımı sıfırlamak için Aktarım (örneğin, TCP ayarlandığında Canlı tutmalar) veya uygulama katmanı ayarlandığında Canlı tutmalar kullanabilirsiniz.

## <a name="discoveroutbound"></a>Bir VM kullandığı genel IP bulma
Giden bir bağlantıyı ortak kaynak IP adresi belirlemek için birçok yolu vardır. OpenDNS VM ortak IP adresini gösteren bir hizmet sunar. 

Nslookup komutunu kullanarak, ad myip.opendns.com için bir DNS sorgusu OpenDNS çözümleyici gönderebilirsiniz. Hizmet sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Sanal makineden aşağıdaki sorguyu çalıştırın yanıt o VM için kullanılan genel IP olur:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Giden bağlantıyı engelliyor
Bazen bir çıkış akışı oluşturmak için izin verilmesi bir VM için istenmeyen olabilir. Hangi hedefleri giden trafik akışları ile ulaşılabilen yönetmek için bir zorunluluk olabilir veya hangi hedefleri gelen akışları başlayabilirsiniz. Bu durumda, kullanabileceğiniz [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) VM ulaşabilir hedefleri yönetmek için. Nsg'ler, hangi ortak hedef gelen akışları başlatabilirsiniz yönetmek için de kullanabilirsiniz. 

Yük dengeli bir VM için bir NSG uyguladığınızda, dikkat [varsayılan etiketleri](../virtual-network/virtual-networks-nsg.md#default-tags) ve [varsayılan kuralları](../virtual-network/virtual-networks-nsg.md#default-rules). VM, sistem durumu araştırma isteklerine Azure yük Dengeleyiciden aldığından emin olmak gerekir. 

Bir NSG'yi sistem durumu araştırma AZURE_LOADBALANCER varsayılan etiket isteklerinden engelliyorsa VM durumu araştırması başarısız olur ve VM düşürüleceği. Yük Dengeleyici, bu VM için yeni akışları gönderme durdurur.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yük dengeleyici temel](load-balancer-overview.md).
- Daha fazla bilgi edinmek [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md).
- Başka bir anahtar bazıları hakkında bilgi edinin [ağı yetenekleri](../networking/networking-overview.md) azure'da.
