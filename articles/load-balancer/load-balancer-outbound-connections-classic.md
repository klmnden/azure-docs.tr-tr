---
title: Giden bağlantılar Azure (Klasik) | Microsoft Docs
description: Bu makale, Azure nasıl sağladığını açıklar bulut hizmetlerinin ortak Internet hizmetleriyle iletişim kurmasını.
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
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: ec13109173f89b53e32f903febcec13c7f38c574
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="outbound-connections-classic"></a>Giden bağlantılar (Klasik)

Azure müşteri dağıtımları için giden bağlantı birkaç farklı yollarla sağlar. Bu makalede senaryolar nelerdir, bunlar uygulandığında, nasıl çalıştığını ve bunların nasıl yönetileceğini açıklar.

>[!NOTE]
>Bu makalede yalnızca klasik dağıtımlarda yer almaktadır.  Gözden geçirme [giden bağlantılar](load-balancer-outbound-connections.md) azure'da tüm Resource Manager dağıtım senaryoları için.

Bir dağıtımda Azure dışında Azure ortak IP adres alanındaki uç noktalar ile iletişim kurabilir. Bir örneği ortak IP adresi alanı içindeki bir hedefe giden bir akışı başlattığında, Azure özel IP adresi genel bir IP adresi dinamik olarak eşler. Bu eşleme oluşturulduktan sonra bu giden kaynaklı akışı dönüş trafiği de özel IP adresi akış geldiği ulaşabilir.

Azure kaynak ağ adresi çevirisi (SNAT) bu işlemi gerçekleştirmek için kullanır. Birden çok özel IP adresleri, tek bir ortak IP adresi maskelemeyi Azure kullanır [adresi çevirisi (PAT) bağlantı noktası](#pat) özel IP adresleri geçici için. Kısa ömürlü bağlantı noktaları PAT için kullanılır ve [önceden ayrılmış](#preallocatedports) havuzu boyutuna göre.

Vardır birden çok [giden senaryoları](#scenarios). Bu senaryolar, gerektiğinde birleştirebilirsiniz. Dağıtım modeli için uygulamak gibi özellikleri, kısıtlamalar ve desenler dikkatle anlamak için bunları gözden geçirin ve uygulama senaryosu. Gözden geçirmek için yönergeler [bu senaryoları yönetme](#snatexhaust).

## <a name="scenarios"></a>Senaryoya genel bakış

Azure giden bağlantı Klasik dağıtımlar ulaşmak için üç farklı yöntem sağlar.  Tüm Klasik dağıtımlar için kullanılabilir tüm üç senaryo vardır:

| Senaryo | Yöntem | Açıklama | Web çalışanı rolü | IaaS | 
| --- | --- | --- | --- | --- |
| [1. Örnek düzeyinde ortak IP adresi olan VM](#ilpip) | SNAT, bağlantı noktası maskelemeyi kullanılmıyor |Azure sanal makine atanan genel IP kullanır. Örneğinin tüm kısa ömürlü bağlantı noktaları kullanılabilir vardır. | Hayır | Evet |
| [2. ortak yük dengeli uç nokta](#publiclbendpoint) | Bağlantı noktası (PAT) için genel bir uç nokta maskelemeyi ile SNAT |Azure genel IP adresi ortak uç birden çok özel uç ile paylaşır. Azure PAT için genel bir uç nokta kısa ömürlü bağlantı noktalarını kullanır. | Evet | Evet |
| [3. Tek başına VM ](#defaultsnat) | Bağlantı noktası (PAT) maskelemeyi ile SNAT | Azure otomatik olarak snat Uygulamanız için bir ortak IP adresi belirler, bu ortak IP adresi ile tüm dağıtım paylaşımları ve PAT için ortak uç noktası IP adresi kısa ömürlü bağlantı noktalarını kullanır. Bu, önceki senaryoları için geri dönüş bir senaryodur. Görünürlük ve denetim gerekiyorsa bunu yapmanız önerilmez. | Evet | Evet|

Bu Azure Resource Manager dağıtımları için kullanılabilen giden bağlantı işlevlerinin bir alt kümesidir.  

Klasik farklı dağıtımlarında farklı işlevselliğe sahiptir:

| Klasik dağıtım | İşlevi kullanılabilir | 
| --- | --- |
| Sanal Makine | Senaryo [1](#ilpip), [2](#publiclbendpoint), veya [3](#defaultsnat) |
| Web çalışanı rolü | yalnızca senaryo [2](#publiclbendpoint), [3](#defaultsnat) | 

[Hafifletme stratejileri](#snatexhaust) de aynı farklar vardır.

[Kısa ömürlü bağlantı noktaları ön tahsis için kullanılan algoritma](#ephemeralports) için PAT Klasik dağıtımlar için Azure Resource Manager kaynak dağıtımları ile aynıdır.

### <a name="ilpip"></a>Senaryo 1: VM bir örnek düzeyinde ortak IP adresi ile

Bu senaryoda, VM bir örnek düzeyinde ortak IP (atanmış ILPIP) sahiptir. Giden bağlantılar kaygı kadar VM yük dengeli uç noktası olup olmadığını önemli değildir. Bu senaryo başkalarının önceliklidir. Bir ILPIP kullanıldığında, VM için tüm giden trafik akışları ILPIP kullanır.  

Bir ortak bir VM'ye atanan IP bir 1:1 ilişki (yerine 1:many) olduğu ve durum bilgisiz 1:1 NAT uygulanan  Bağlantı noktası maskelenmiş (PAT) kullanılmaz ve tüm kısa ömürlü bağlantı noktaları kullanılabilir VM sahiptir.

Uygulamanız çok sayıda giden trafik akışları başlatır ve SNAT bağlantı noktası Tükenme deneyimi, atama göz önünde bulundurun bir [SNAT kısıtlamaları azaltmak için ILPIP](#assignilpip). Gözden geçirme [yönetme SNAT Tükenme](#snatexhaust) okumalıdır.

### <a name="publiclbendpoint"></a>Senaryo 2: Ortak yük dengeli uç nokta

Bu senaryoda, VM veya Web çalışanı rolü yük dengeli uç nokta aracılığıyla ortak bir IP adresi ile ilişkilidir. VM, kendisine atanmış bir ortak IP adresi yok. 

Yük dengeli VM giden akış oluşturduğunda, Azure ortak yük dengeli uç nokta ortak IP adresine giden akış özel kaynak IP adresi çevirir. Azure SNAT bu işlemi gerçekleştirmek için kullanır. Ayrıca Azure kullanır [PAT](#pat) geçici bir ortak IP adresi arkasında birden çok özel IP adresleri için. 

Yük dengeleyicinin genel IP adresi ön uç kısa ömürlü bağlantı noktaları, tek tek akışları VM tarafından kaynaklanan ayırt etmek için kullanılır. SNAT dinamik olarak kullanan [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) giden trafik akışları oluşturduğunuzda. Bu bağlamda snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü. Bitti sınırlı bir kaynak olup olmadıklarını. Ne olduğunu anlamak önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiğinde etkisini anlamak için gözden [yönetme SNAT Tükenme](#snatexhaust).

Zaman [birden çok ortak yük dengeli uç nokta](load-balancer-multivip.md) var, bu ortak IP adresleri olan bir [giden trafik akışları için aday](#multivipsnat), ve bir rastgele seçili.  

### <a name="defaultsnat"></a>Senaryo 3: ilişkili ortak IP adresi yok

Bu senaryoda, VM veya Web çalışanı rolü ortak bir yük dengeli uç nokta bir parçası değil.  Ve VM söz konusu olduğunda, kendisine atanmış bir ILPIP adresi yok. VM giden akış oluşturduğunda, Azure ortak kaynak IP adresine giden akış özel kaynak IP adresi çevirir. Bu giden akış için kullanılan ortak IP adresi yapılandırılabilir değildir ve aboneliğin ortak IP kaynak sınırınızı sayılmaz.  Azure otomatik olarak bu adresi ayırır.

Azure bağlantı noktası maskelemeyi ile SNAT kullanır ([PAT](#pat)) bu işlemi gerçekleştirmek için. Bu senaryo benzer [Senaryo 2](#lb), var. dışında kullanılan IP adresi üzerinde denetimi yoktur. Bu, ne zaman 1 ve 2 senaryoları mevcut için geri dönüş bir senaryodur. Giden adres üzerinde denetim isterseniz, bu senaryo öneririz yok. Giden bağlantılar, uygulamanızın önemli bir parçası ise, seçtiğiniz başka bir senaryo.

Bölümünde açıklandığı gibi SNAT bağlantı noktalarını önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü.  Sanal makineleri veya genel IP adresi paylaşımı Web çalışanı rollerinin sayısını ön tahsis kısa ömürlü bağlantı noktası sayısını belirler.   Ne olduğunu anlamak önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiğinde etkisini anlamak için gözden [yönetme SNAT Tükenme](#snatexhaust).

## <a name="snat"></a>SNAT ve PAT anlama

### <a name="pat"></a>Bağlantı noktası maskelenmiş SNAT (PAT)

Bir dağıtım giden bir bağlantıyı yaptığında, her giden bağlantı kaynağı yeniden yazılmıştır. Kaynak (yukarıda açıklanan senaryoları bağlı olarak) dağıtım ile ilgili genel IP için özel IP adres alanından yeniden. Ortak IP adres alanı, 5-tanımlama grubu (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) akışının benzersiz olması gerekir.  

Kısa ömürlü bağlantı noktaları (SNAT) tek bir ortak IP adresini birden çok akış kaynaklı olduğundan bu özel kaynak IP adresini yeniden yazma işlemi sonra elde etmek için kullanılır. 

Akış tek hedef IP adresi, bağlantı noktası ve protokol başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokol birden çok akışlar için her bir akışa tek bir SNAT bağlantı noktasını kullanır. Bu, aynı ortak IP adresinden kaynaklanan ve aynı hedef IP adresi, bağlantı noktası ve protokol Git akışları benzersiz olmasını sağlar. 

Her farklı bir hedef IP adresi, bağlantı noktası ve protokolü, birden çok akış tek bir SNAT bağlantı noktası paylaşır. Hedef IP adresi, bağlantı noktası ve protokol akışları ortak IP adresi alanı akışlarında ayırt etmek için bağlantı noktalarını ek kaynak gerek kalmadan benzersiz olun.

SNAT bağlantı noktası kaynakları tükendi, varolan akışları SNAT bağlantı noktalarını bırakana kadar giden trafik akışları başarısız. Yük Dengeleyici akışı kapatır ve kullandığında SNAT bağlantı noktalarını geri kazanır bir [4 dakikalık boşta kalma zaman aşımı](#idletimeout) boşta akışları SNAT noktalarından geri kazanma için.

Yaygın olarak SNAT bağlantı noktası tükenmesine yol koşulları azaltmak desenler için gözden [yönetme SNAT](#snatexhaust) bölümü.

### <a name="preallocatedports"></a>Kısa ömürlü bağlantı noktası ön tahsisi SNAT (PAT) maskelemeyi bağlantı noktası

Sayısı, önceden ayrılmış SNAT kullanılabilir bağlantı noktası sayısını belirlemek için bir algoritma kullanırken arka uç havuzu boyutuna göre azure kullandığı bağlantı noktası maskelenmiş SNAT ([PAT](#pat)). Belirli bir ortak IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktaları SNAT bağlantı noktalarıdır.

Belirli bir ortak IP adresi kaç VM veya Web çalışanı rolü örnekleri paylaşımında dayalı bir örneği dağıtıldığında azure SNAT bağlantı noktalarını preallocates.  Giden trafik akışları oluşturduğunuzda, [PAT](#pat) dinamik olarak (önceden ayrılmış sınıra kadar) kullanır ve akış kapandığında Bu bağlantı noktaları serbest veya [boş durma zaman aşımlarını](#ideltimeout) gerçekleşir.

Aşağıdaki tabloda SNAT bağlantı noktası preallocations arka uç havuzu boyutlarda katmanları gösterilmektedir:

| Örnekler | Örneği başına ön tahsis SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

Kullanılabilir SNAT bağlantı noktası sayısını doğrudan akışları sayıya tercüme etmez unutmayın. Tek bir SNAT bağlantı noktası benzersiz birden çok varış yeri için yeniden kullanılabilir. Yalnızca akış benzersiz hale getirmek için gerekli olduğunda bağlantı noktaları kullanılır. Tasarım ve azaltma kılavuzu için ilgili bölümüne bakın. [exhaustible bu kaynak yönetme](#snatexhaust) ve açıklayan bölümü [PAT](#pat).

Dağıtımınızın boyutunu değiştirme bazı yerleşik akışlarının etkileyebilir. Arka uç havuzu boyutunu artırır ve sonraki katmanla geçişleri, ön tahsis SNAT bağlantı noktalarını yarısı sonraki daha büyük arka uç havuzu katmanına geçiş sırasında geri kazanılır. Reclaimed SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrar ve kurulmaları gerekir. Yeni bir akış bulamazsa, ön tahsis bağlantı noktaları kullanılabilir olduğu sürece, akış hemen başarılı olur.

Varsa dağıtım boyutunu azaltır ve daha düşük bir katman geçişleri, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, varolan SNAT bağlantı noktalarını ayrılmış ve bunların ilgili akışları etkilenmez.

SNAT bağlantı noktalarını ayırmaları olan belirli IP Aktarım Protokolü (TCP ve UDP korunur ayrı ayrı) ve aşağıdaki koşullar altında serbest bırakılır:

### <a name="tcp-snat-port-release"></a>TCP SNAT bağlantı noktası sürüm

- Her iki sunucu/istemci FIN/ACK gönderirse SNAT bağlantı noktası 240 saniye sonra kullanıma sunulacaktır.
- Bir RST görülen, SNAT bağlantı noktası 15 saniye sonra kullanıma sunulacaktır.
- boşta kalma zaman aşımı ulaşıldı

### <a name="udp-snat-port-release"></a>UDP SNAT bağlantı noktası sürüm

- boşta kalma zaman aşımı ulaşıldı

## <a name="problemsolving"></a> Sorun giderme 

Bu bölümde, SNAT Tükenme ve azure'da giden bağlantılar ile oluşabilecek diğer senaryolar azaltmaya yardımcı olmak için tasarlanmıştır.

### <a name="snatexhaust"></a> SNAT (PAT) bağlantı noktası Tükenme yönetme
[Kısa ömürlü bağlantı noktaları](#preallocatedports) için kullanılan [PAT](#pat) açıklandığı gibi bir exhaustible kaynağı olan [ilişkili hiçbir genel IP](#defaultsnat) ve [ortak yük dengeli uç nokta](#publiclbendpoint).

Aynı hedef IP adresi ve bağlantı noktası fazla giden TCP veya UDP bağlantı başlatma, ve giden bağlantılar başarısız izleyebilmenizi veya destek tarafından tükettiğini SNAT bağlantı noktalarını olduğunuz önerilir biliyorsanız (önceden ayrılmış [kısa ömürlü bağlantı noktaları](#preallocatedports) tarafından kullanılan [PAT](#pat)), birkaç genel azaltma seçeneğiniz vardır. Bu seçenekleri gözden geçirin ve ne kullanılabilir ve senaryonuz için en iyisi olduğuna karar verin. Bu senaryo yönetmek bir veya daha fazla yardımcı olabilir.

Giden bağlantı davranışını anlama ile ilgili sorunlar yaşıyorsanız, IP yığını istatistikleri (netstat) kullanabilirsiniz. Veya paket yakalamaları kullanarak bağlantı davranışlarla için faydalı olabilir.

#### <a name="connectionreuse"></a>Bağlantılarını yeniden uygulamaya değiştirme 
Uygulamanızdaki bağlantıları yeniden kullanarak snat Uygulamanız için kullanılan kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir. Bu, özellikle HTTP/1.1, bağlantı yeniden varsayılan olduğu gibi protokoller için geçerlidir. Ve taşıma (örneğin, REST) HTTP kullanan diğer protokolleri sırayla yararlı olabilir. 

Yeniden her zaman tek tek, atomik TCP bağlantıları her istek için daha iyi olur. Daha fazla kullanıcı, çok verimli TCP işlemleri sonuçlarında yeniden kullanabilirsiniz.

#### <a name="connection pooling"></a>Bağlantı havuzu kullanmak için uygulamayı değiştirme
Bir bağlantı, uygulamanızda sabit kümesi (her mümkün olduğunca yeniden) bağlantıları istekleri dahili olarak dağıtıldığı düzeni havuzu tercih edebilirsiniz. Bu düzen kullanımda kısa ömürlü bağlantı noktası sayısını kısıtlar ve daha tahmin edilebilir bir ortam oluşturur. Bu düzen, tek bir bağlantı üzerinde bir işlemi cevap engelleme zaman birden çok eşzamanlı operasyonlar sağlayarak isteklerin verimini de artırabilirsiniz.  

Bağlantı havuzu zaten uygulamanızı veya uygulamanız için yapılandırma ayarlarını geliştirmek için kullanmakta olduğunuz framework içinde mevcut olabilir. Bağlantı yeniden ile bağlantı havuzu birleştirebilirsiniz. Birden çok istek daha sonra aynı hedef IP adresi ve bağlantı noktası için bağlantı noktaları, sabit, tahmin edilebilir birtakım tüketir. İstek gecikme süresi ve kaynak kullanımını azaltarak TCP işlemleri verimli kullanımdan da yararlanır. UDP akışlarının sayısı yönetilmesi sırayla Egzoz koşullar önlemek ve SNAT bağlantı noktası kullanımı yönetmek için UDP işlemleri de yararlanabilir.

#### <a name="retry logic"></a>Uygulamayı daha az agresif yeniden deneme mantığı kullanacak şekilde değiştirin
Zaman [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) için kullanılan [PAT](#pat) olan tükendi veya uygulama hataları, agresif ya da kaba kuvvet decay ve geri Çekilme mantığı neden olmadan Tükenme oluşur veya kalıcı hale getirmek yeniden deneme sayısı. Daha az agresif bir yeniden deneme mantığı kullanarak kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir. 

Kısa ömürlü bağlantı noktaları 4 dakikalık boşta kalma zaman aşımı (ayarlanamıyor) vardır. Yeniden deneme çok agresif Tükenme, kendi temizlenir fırsatı yoktur. Bu nedenle, uygulamanızı nasıl--ve hangi sıklıkta--işlemleri yeniden deneme dikkate bir kritik tasarım parçasıdır.

#### <a name="assignilpip"></a>Her VM için bir örnek düzeyinde ortak IP atayın
Bir ILPIP atama değişiklikleri senaryonuz için [örnek düzeyinde ortak IP bir VM](#ilpip). Her VM için kullanılan tüm kısa ömürlü bağlantı noktaları genel IP, VM'ye kullanılabilir. (Burada ilgili dağıtımla ilişkili tüm sanal makineler ile bir genel IP kısa ömürlü bağlantı noktaları paylaşılan senaryolarında aksine.) Olası etkisini uygulamaları güvenilir listeye almayı çok sayıda tek tek IP adresleri gibi düşünmek dengelemeler vardır.

>[!NOTE] 
>Bu seçenek, web çalışanı rolleri için kullanılamaz.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımı sıfırlamak için ayarlandığında Canlı tutmalar kullanın

Giden bağlantılar 4 dakikalık boşta kalma zaman aşımı vardır. Bu zaman aşımı ayarlanabilir değil. Ancak, boş bir akış yenileyin ve gerekirse bu boşta kalma zaman aşımı sıfırlamak için Aktarım (örneğin, TCP ayarlandığında Canlı tutmalar) veya uygulama katmanı ayarlandığında Canlı tutmalar kullanabilirsiniz.  Paketlenmiş yazılımları olup destekleniyorsa veya nasıl etkinleştirileceği konusunda sağlayıcısına başvurun.  Genellikle yalnızca bir yan boşta kalma zaman aşımı sıfırlamak için ayarlandığında Canlı tutmalar oluşturması gerekiyorsa. 

## <a name="discoveroutbound"></a>Bir VM kullandığı genel IP bulma
Giden bir bağlantıyı ortak kaynak IP adresi belirlemek için birçok yolu vardır. OpenDNS VM ortak IP adresini gösteren bir hizmet sunar. 

Nslookup komutunu kullanarak, ad myip.opendns.com için bir DNS sorgusu OpenDNS çözümleyici gönderebilirsiniz. Hizmet sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Sanal makineden aşağıdaki sorguyu çalıştırın yanıt o VM için kullanılan genel IP olur:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yük dengeleyici](load-balancer-overview.md) Resource Manager dağıtımları kullanılır.
- Modu hakkında bilgi edinin [giden bağlantı](load-balancer-outbound-connections.md) senaryoları Resource Manager dağıtımları içinde kullanılabilir.
