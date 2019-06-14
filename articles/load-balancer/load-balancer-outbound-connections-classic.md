---
title: (Klasik) azure'da giden bağlantıları
titlesuffix: Azure Load Balancer
description: Bu makalede Azure nasıl sağladığını açıklar. bulut Hizmetleri, genel internet Hizmetleri ile iletişim kurmak için.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: kumud
ms.openlocfilehash: 3267d79387586f5ca8475d7ac0ed0f86d3f64f0d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60595038"
---
# <a name="outbound-connections-classic"></a>Giden bağlantılar (Klasik)

Azure, birçok farklı mekanizmalar aracılığıyla müşteri dağıtımları için giden bağlantı sağlar. Bu makalede, senaryolar nelerdir, bunlar uygulandığında, nasıl çalıştıklarını ve bunların nasıl yönetileceğini açıklar.

>[!NOTE]
>Bu makale yalnızca klasik dağıtımlar kapsar.  Gözden geçirme [giden bağlantılar](load-balancer-outbound-connections.md) azure'a tüm Resource Manager dağıtım senaryoları için.

Azure'da bir dağıtımı dışında Azure genel IP adres alanındaki uç noktalar ile iletişim kurabilir. Genel IP adresi alanı içindeki bir hedefe giden bir akış örneği başlatır, Azure genel bir IP adresi için özel IP adresini dinamik olarak eşler. Bu eşleme oluşturulduktan sonra dönüş trafiği giden bu kaynaklı akış için Ayrıca özel IP adresini akışı geldiği ulaşabilirsiniz.

Azure, bu işlevi gerçekleştirmek için kaynak ağ adresi çevirisi (SNAT) kullanır. Birden çok özel IP adresleri, tek bir genel IP adresi gösteren, Azure kullanan [adres çevirisini (PAT)'bağlantı noktası](#pat) özel IP adresleri gizlemeye. Kısa ömürlü bağlantı noktaları için PAT kullanıldığında ve [önceden ayrılmış](#preallocatedports) havuz boyutunu temel alan.

Vardır birden çok [giden senaryoları](#scenarios). Bu senaryolar, gerektiği şekilde birleştirebilirsiniz. Bunlar, dağıtım modeli için geçerli olan özellikler, kısıtlamalar ve desenleri dikkatli bir şekilde anlamak için bunları gözden geçirin ve uygulama senaryosu. Gözden geçirme Kılavuzu [bu senaryoları yönetme](#snatexhaust).

## <a name="scenarios"></a>Senaryoya genel bakış

Azure, giden bağlantı Klasik dağıtımlar elde etmek için üç farklı yöntem sağlar.  Tüm Klasik dağıtımlar için kullanılabilir tüm üç senaryo vardır:

| Senaryo | Yöntem | IP protokolleri | Açıklama | Web çalışanı rolü | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. Örnek düzeyinde ortak IP adresine sahip VM](#ilpip) | SNAT, bağlantı noktası maskelemeyi kullanılmıyor | TCP, UDP VE ICMP, ESP | Azure, sanal makinenin atanmış genel IP kullanır. Örneğinin tüm kısa ömürlü bağlantı noktaları kullanılabilir vardır. | Hayır | Evet |
| [2. ortak yük dengeli uç nokta](#publiclbendpoint) | Bağlantı noktası (PAT) genel uç noktaya maskelemeyi ile SNAT | TCP, UDP | Azure genel IP adresi genel uç noktası, birden çok özel uç noktaları ile paylaşır. Azure, genel bir uç nokta, kısa ömürlü bağlantı noktaları için PAT kullanır. | Evet | Evet |
| [3. Tek başına VM](#defaultsnat) | Bağlantı noktası (PAT) maskelemeyi ile SNAT | TCP, UDP | Azure otomatik olarak genel bir IP adresi için SNAT belirler, bu genel IP adresi olan tüm dağıtım paylaşımları ve kısa ömürlü bağlantı noktaları genel uç noktası IP adresinin PAT için kullanır. Bu, önceki senaryolar için geri dönüş bir senaryodur. Görünürlük ve denetim gerekiyorsa bunu önermiyoruz. | Evet | Evet |

Bu, Azure Resource Manager dağıtımları için kullanılabilen giden bağlantı işlevlerinin bir alt kümesidir.  

Klasik dağıtımlarda farklı farklı işlevselliğe sahiptir:

| Klasik dağıtım | Kullanılabilen İşlevler | 
| --- | --- |
| Sanal makine | Senaryo [1](#ilpip), [2](#publiclbendpoint), veya [3](#defaultsnat) |
| Web çalışanı rolü | yalnızca senaryo [2](#publiclbendpoint), [3](#defaultsnat) | 

[Risk azaltma stratejisi](#snatexhaust) aynı farklılıkları da vardır.

Kısa ömürlü bağlantı noktaları için PAT Klasik dağıtımlar için ön tahsis için kullanılan algoritma, Azure Resource Manager kaynak dağıtımları ile aynıdır.

### <a name="ilpip"></a>Senaryo 1: Örnek düzeyinde ortak IP adresine sahip VM

Bu senaryoda, sanal makine bir örnek düzeyi genel IP (atanmış ILPIP) içeriyor. Giden bağlantılar endişe kadar VM yük dengeli uç noktası olup olmadığını önemi yoktur. Bu senaryo diğer önceliklidir. Bir ILPIP kullanıldığında, VM ILPIP tüm giden akışlar için kullanır.  

Genel bir VM'ye atanan IP bir 1:1 ilişki (yerine 1:many) ve bir durum bilgisi olmayan 1:1 NAT uygulanan  Kendini gizleyen bağlantı noktası (PAT) kullanılmaz ve tüm kısa ömürlü bağlantı noktaları kullanılabilir sanal makine içeriyor.

Uygulamanız çok sayıda giden akışlar başlatır ve SNAT bağlantı noktası tükenmesi deneyimi, atamayı göz önünde bulundurun bir [SNAT kısıtlamalarını azaltmak için ILPIP](#assignilpip). Gözden geçirme [yönetme SNAT tükenmesi](#snatexhaust) oluşmaz.

### <a name="publiclbendpoint"></a>Senaryo 2: Genel yük dengeli uç nokta

Bu senaryoda, VM veya Web çalışanı rolü yük dengeli uç nokta aracılığıyla genel bir IP adresi ile ilişkilidir. VM, kendisine atanmış bir genel IP adresi yok. 

Yük dengeli sanal Makineyi bir giden akış oluşturduğunda, Azure genel yük dengeli uç nokta genel IP adresine giden akış özel kaynak IP adresini çevirir. Azure, bu işlevi gerçekleştirmek için SNAT kullanır. Ayrıca, Azure kullanan [PAT](#pat) gizlemeye genel bir IP adresi arkasında birden çok özel IP adresi. 

Kısa ömürlü bağlantı noktaları yük dengeleyicinin genel IP adresi ön uç, tek tek akış sanal makine tarafından oluşturulan ayırt etmek için kullanılır. Dinamik olarak SNAT kullanan [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) giden akışlar oluşturduğunuzda. Bu bağlamda SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir.

Bölümünde anlatıldığı gibi SNAT bağlantı noktaları önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü. Bunlar tükenmiş olabilir sınırlı bir kaynak hedeflenmiştir. Nasıl olduğunu anlama açısından önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiği şekilde etkisini anlamak için gözden [yönetme SNAT tükenmesi](#snatexhaust).

Zaman [birden çok genel yük dengeli uç nokta](load-balancer-multivip.md) var, bu genel IP adreslerine giden akışlar için bir aday olan ve bir rastgele seçili.  

### <a name="defaultsnat"></a>Senaryo 3: İlişkili genel IP adresi yok

Bu senaryoda, VM veya Web çalışanı rolü genel bir yük dengeli uç noktasının bir parçası değil.  Ve VM söz konusu olduğunda, kendisine atanmış bir ILPIP adresi yok. Azure, VM'ye giden bir akış oluşturduğunda, özel kaynak IP adresini bir genel kaynak IP adresine giden akış çevirir. Giden Bu akış için kullanılan genel IP adresini yapılandırılabilir değildir ve bu aboneliğe ait genel IP kaynağı limite karşı sayılmaz.  Azure, otomatik olarak bu adresi ayırır.

Azure, bağlantı noktası maskelemeyi ile SNAT kullanır ([PAT](#pat)) bu işlevi gerçekleştirmek için. Kullanılan IP adresi üzerinde denetimi yoktur haricinde bu senaryo 2, benzer bir senaryodur. Bu senaryo 1 ve 2 mevcut olduğunda için geri dönüş bir senaryodur. Giden adresi üzerinde denetim istiyorsanız bu senaryo önerilmemektedir. Giden bağlantılar, uygulamanız önemli bir parçası ise, seçtiğiniz başka bir senaryo.

Bölümünde anlatıldığı gibi SNAT bağlantı noktaları önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü.  Vm'leri veya Web çalışanı rolü genel IP adresini paylaşım sayısı ön tahsis kısa ömürlü bağlantı noktası sayısını belirler.   Nasıl olduğunu anlama açısından önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiği şekilde etkisini anlamak için gözden [yönetme SNAT tükenmesi](#snatexhaust).

## <a name="snat"></a>SNAT ve PAT anlama

### <a name="pat"></a>Bağlantı noktası kendini gizleyen SNAT (PAT)

Bir dağıtım bir giden bağlantı yaptığında, her bir giden bağlantı kaynağı yeniden. Kaynak (yukarıda açıklanan senaryoları göre) dağıtımla ilişkili genel IP için özel IP adres alanından yeniden. Genel IP adres alanı, 5 demet (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) akış benzersiz olması gerekir.  

Kısa ömürlü bağlantı noktaları (SNAT), tek bir genel IP adresi birden çok akış kaynaklı olduğundan bu özel kaynak IP adresini yeniden yazma sonra elde etmek için kullanılır. 

Akışa tek bir hedef IP adresi, bağlantı noktası ve protokol başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokol birden çok akışlar için her bir akışın tek bir SNAT bağlantı noktasını kullanır. Bu, aynı genel IP adresinden kaynaklanan ve aynı hedef IP adresi, bağlantı noktası ve protokol gidin, Akışlar benzersiz olmasını sağlar. 

Birden çok akış her farklı bir hedef IP adresi, bağlantı noktası ve protokol, tek bir SNAT bağlantı noktası paylaşın. Hedef IP adresi, bağlantı noktası ve protokol akışları genel IP adresi alanını akışları ayırt etmek için bağlantı noktaları ek kaynak gerek kalmadan benzersiz olun.

SNAT bağlantı noktası kaynaklarını tüketmiş, mevcut akışları SNAT bağlantı noktalarını serbest bırakılana kadar giden akışlar başarısız. Yük Dengeleyici geri kazanır SNAT bağlantı noktaları akışı kapatır ve kullandığında bir [4 dakikalık boşta kalma zaman aşımı](#idletimeout) boşta akışlar SNAT noktalarından tekrar kullanılabilir hale getirme için.

Yaygın olarak SNAT bağlantı noktası tükenmesi için yol koşulları düzenlerini için gözden [yönetme SNAT](#snatexhaust) bölümü.

### <a name="preallocatedports"></a>Kısa ömürlü bağlantı noktası ön tahsis maskelemeyi SNAT (PAT) bağlantı noktası

Kendini gizleyen SNAT bağlantı noktasını sayısı önceden ayrılmış SNAT kullanılabilir bağlantı noktası sayısını belirlemek için bir algoritma kullanırken arka uç havuzu boyutuna göre azure kullanır ([PAT](#pat)). Kısa ömürlü bağlantı noktaları için belirli genel IP kaynak adresi kullanılabilir SNAT bağlantı noktalarıdır.

Belirli bir genel IP adresinin kaç VM veya Web çalışanı rolü örneği paylaşımında temel örneği dağıtıldığında azure bağlantı noktalarını SNAT preallocates.  Giden akışlar oluşturulduğunda [PAT](#pat) dinamik olarak (önceden ayrılmış sınıra kadar) kullanır ve bu bağlantı noktaları akışı kapatır veya boşta kalma zaman aşımı meydana serbest bırakır.

Aşağıdaki tabloda, arka uç havuz boyutları katmanları için SNAT bağlantı noktası preallocations gösterilmektedir:

| Örnekler | Örnek başına ön tahsis SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Kullanılabilir SNAT bağlantı noktasına doğrudan akışlar sayıya çevirmez unutmayın. Tek bir SNAT bağlantı noktası için birden fazla benzersiz hedefler yeniden kullanılabilir. Yalnızca akış benzersiz hale getirmek gerekli değilse, bağlantı noktaları tüketilir. Tasarım ve risk azaltma kılavuzu için ilgili bölümüne bakın. [exhaustible bu kaynağı yönetmek nasıl](#snatexhaust) ve açıklayan bölümüne [PAT](#pat).

Dağıtımınızın boyutunu değiştirmek, bazı oluşturulmuş akışlarınızı etkileyebilir. Arka uç havuz boyutunu artırır ve sonraki katmana geçiş, ön tahsis SNAT bağlantı noktaları yarısını sonraki daha büyük arka uç havuzu katmana geçiş sırasında geri kazanılır. Geri kazanılan SNAT bağlantı noktası ile ilişkili akışlar zaman aşımına uğrar ve yeniden oluşturulmaları gerekir. Yeni bir akış girişiminde bulunulursa, ön tahsis bağlantı noktaları kullanılabilir olduğu sürece, akış hemen başarılı olur.

Dağıtım boyutunu azaltır ve daha düşük bir katmana geçiş, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, var olan bağlantı noktaları SNAT ayrılan ve ilgili kendi akışlarını etkilenmez.

Bir bulut hizmeti imzalanmasını veya değiştirilen, altyapı, arka uç havuzunun kadar büyük olarak iki kez gerçek olarak olması için geçici olarak bildirebilir ve Azure sırayla daha az SNAT örnek başına bağlantı noktaları beklenenden erişinceye.  Bu, geçici olarak SNAT bağlantı noktası tükenmesi olasılığını artırabilir. Sonuçta gerçek boyuta havuz boyutunu geçer ve Azure, yukarıdaki tabloda göre beklenen sayı ön tahsis SNAT bağlantı noktaları otomatik olarak artırır.  Bu davranış tasarım gereğidir ve yapılandırılabilir değildir.

SNAT bağlantı noktaları ayırmaları olan belirli IP Aktarım Protokolü (TCP ve UDP korunur ayrı olarak) ve aşağıdaki koşullarda barındırılır:

### <a name="tcp-snat-port-release"></a>SNAT TCP bağlantı noktası sürüm

- Her iki sunucu/istemci FIN/ACK gönderirse SNAT bağlantı noktası 240 saniye sonra kullanıma sunulacaktır.
- Bir lk görülür, SNAT bağlantı noktası 15 saniye sonra kullanıma sunulacaktır.
- boşta kalma zaman aşımı ulaşıldı

### <a name="udp-snat-port-release"></a>SNAT UDP bağlantı noktası sürüm

- boşta kalma zaman aşımı ulaşıldı

## <a name="problemsolving"></a> Sorun giderme 

Bu bölümde SNAT tükenmesi ve azure'da giden bağlantıları ortaya çıkabilir diğer senaryolarının azaltmaya yardımcı olmak için tasarlanmıştır.

### <a name="snatexhaust"></a> SNAT (PAT) bağlantı noktası tükenmesi yönetme
[Kısa ömürlü bağlantı noktaları](#preallocatedports) için kullanılan [PAT](#pat) açıklandığı bir exhaustible kaynağı olan [ilişkili hiçbir genel IP](#defaultsnat) ve [genel yük dengeli uç nokta](#publiclbendpoint).

Aynı hedef IP adresine ve bağlantı noktası birçok giden TCP veya UDP bağlantılarının başlatma, ve giden bağlantılar başarısız inceleyin veya destek birimi tarafından tükettiğini SNAT bağlantı noktaları işiniz kopyaladınız biliyorsanız (önceden ayrılmış [kısa ömürlü bağlantı noktaları](#preallocatedports) tarafından kullanılan [PAT](#pat)), genel risk azaltma birkaç seçeneğiniz vardır. Bu seçenekleri gözden geçirin ve hangi kullanılabilir ve senaryonuz için en iyi olduğuna karar verin. Bu senaryo yönetme bir veya daha fazla yardımcı olabilir.

Giden bağlantı davranışını anlama ile ilgili sorunlar yaşıyorsanız, IP yığın istatistikleri (netstat) kullanabilirsiniz. Veya paket yakalamaları kullanarak bağlantı davranışlarla yardımcı olabilir.

#### <a name="connectionreuse"></a>Uygulama bağlantılarını yeniden değiştirme 
Uygulamanızdaki bağlantıları yeniden kullanarak SNAT için kullanılan kısa ömürlü bağlantı noktaları için isteğe bağlı azaltabilir. Bu, özellikle de HTTP/1.1, bağlantı yeniden varsayılan olduğu gibi protokolleri için geçerlidir. Ve HTTP Aktarım (örneğin, REST) olarak kullanan diğer protokolleri sırayla yararlı olabilir. 

Yeniden her zaman tek tek, atomik TCP bağlantıları her istek için daha iyidir. Daha fazla yüksek performanslı, çok da verimli TCP işlem sonuçları yeniden kullanın.

#### <a name="connection pooling"></a>Bağlantı havuzu kullanmak için uygulamayı değiştirme
Bir bağlantı, uygulamanızda bir sabit bağlantılar (her mümkün olduğunda yeniden) kümesi arasında istekleri dahili olarak dağıtıldığı düzeni havuzu kullanabilirsiniz. Bu düzen, kısa ömürlü bağlantı noktalarına sayısını kısıtlar ve daha öngörülebilir bir ortam oluşturur. Bu düzen, aynı anda birden çok işlem tek bir bağlantı üzerinde bir işlemin cevap engellediğinde sağlayarak isteklerin aktarım hızı da artırabilirsiniz.  

Bağlantı havuzu uygulamanızı veya uygulamanız için yapılandırma ayarlarını geliştirmek için kullanmakta olduğunuz framework içinde zaten olabilir. Bağlantı havuzu ile bağlantı yeniden birleştirebilirsiniz. Birden çok isteklerinizi sonra sabit, öngörülebilir birkaç bağlantı noktalarının aynı hedef IP adresine ve bağlantı noktası kullanır. İstek gecikme süresi ve kaynak kullanımı azaltma TCP işlemlerin verimli kullanımdan avantaj sağlıyor. UDP akışlarının sayısı yönetilmesi sırayla Egzozu koşullar önlemek ve SNAT bağlantı noktası kullanımı yönetmek için UDP işlemleri de yararlanabilir.

#### <a name="retry logic"></a>Daha az agresiftir yeniden deneme mantığı kullanmak için uygulamayı değiştirme
Zaman [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) için kullanılan [PAT](#pat) olan tükendi veya uygulama hataları, agresif veya deneme yanılma decay ve geri alma mantığını neden olmadan tükenmesi oluşur ya da kalıcı hale getirmek yeniden deneme sayısı. Kısa ömürlü bağlantı noktaları için isteğe bağlı daha agresif bir yeniden deneme mantığı kullanarak azaltabilir. 

Kısa ömürlü bağlantı noktaları, bir 4 dakikalık boşta kalma zaman aşımı (ayarlanamıyor) sahip. Çok agresif yeniden deneme işlemleri, kendi kendine temizlenir fırsatı tükenmesi vardır. Bu nedenle, dikkate nasıl--ve ne sıklıkta--uygulamanız yeniden deneme işlemleri bir kritik tasarım parçasıdır.

#### <a name="assignilpip"></a>Her VM için bir örnek düzeyinde genel IP atayın
Senaryonuz için değişiklikleri bir ILPIP atama [örnek düzeyinde ortak IP, bir VM'ye](#ilpip). VM'ye her VM için kullanılan tüm kısa ömürlü bağlantı noktaları genel IP'nin kullanılabilir. (Burada ilgili dağıtımla ilişkili tüm sanal makinelerin genel IP, kısa ömürlü bağlantı noktaları paylaşılan senaryolarında aksine.) Çok sayıda özel IP adreslerini beyaz listeye ekleme gibi olası etkisini göz önünde bulundurmanız eksileri vardır.

>[!NOTE] 
>Bu seçenek, web çalışanı rolü için kullanılamaz.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımı sıfırlamak için canlı tutma kullanın

Giden bağlantılar 4 dakikalık boşta kalma zaman aşımı vardır. Bu zaman aşımı ayarlanabilir değildir. Ancak, boş bir akış yenileyin ve gerekirse bu boşta kalma zaman aşımı sıfırlamak için Aktarım (örneğin, canlı tutma TCP) veya uygulama katmanı canlı tutma kullanabilirsiniz.  Tüm paketlenmiş yazılımlarda olup bu destekleniyor mu veya nasıl etkinleştirileceğini sağlayıcısına başvurun.  Genellikle yalnızca bir tarafına boşta kalma zaman aşımı sıfırlamak için canlı tutma oluşturması gerekiyorsa. 

## <a name="discoveroutbound"></a>Bir VM kullanan genel IP bulma
Bir giden bağlantı genel kaynak IP adresini belirlemek için birçok yolu vardır. OpenDNS, sanal makinenizin genel IP adresini göstermek bir hizmet sunar. 

Nslookup komutunu kullanarak bir DNS sorgusu adı myip.opendns.com için OpenDNS çözümleyiciye gönderebilirsiniz. Hizmet, sorgu göndermek için kullanılan kaynak IP adresini döndürür. Aşağıdaki sorgu, VM'den çalıştırdığınızda, o sanal makine için kullanılan genel IP yanıt verilmiştir:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [yük dengeleyici](load-balancer-overview.md) Resource Manager dağıtımında kullanılır.
- Modu hakkında bilgi edinin [giden bağlantı](load-balancer-outbound-connections.md) senaryoları Resource Manager dağıtımlarında kullanılabilir.
