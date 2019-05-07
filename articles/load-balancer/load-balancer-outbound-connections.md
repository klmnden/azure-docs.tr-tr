---
title: Azure'da giden bağlantıları
titlesuffix: Azure Load Balancer
description: Bu makalede, Azure VM'ler, ortak Internet Hizmetleri ile iletişim kurmak nasıl sağladığını açıklar.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2019
ms.author: kumud
ms.openlocfilehash: d5f52829f5895b30afd160cc8ded755332aca5c5
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190163"
---
# <a name="outbound-connections-in-azure"></a>Azure'da giden bağlantıları

Azure, birçok farklı mekanizmalar aracılığıyla müşteri dağıtımları için giden bağlantı sağlar. Bu makalede, senaryolar nelerdir, bunlar uygulandığında, nasıl çalıştıklarını ve bunların nasıl yönetileceğini açıklar.

>[!NOTE] 
>Bu makalede, yalnızca Resource Manager dağıtımları ele alınmıştır. Gözden geçirme [giden bağlantılar (Klasik)](load-balancer-outbound-connections-classic.md) azure'a tüm Klasik dağıtım senaryoları için.

Azure'da bir dağıtımı dışında Azure genel IP adres alanındaki uç noktalar ile iletişim kurabilir. Genel IP adresi alanı içindeki bir hedefe giden bir akış örneği başlatır, Azure genel bir IP adresi için özel IP adresini dinamik olarak eşler. Bu eşleme oluşturulduktan sonra dönüş trafiği giden bu kaynaklı akış için Ayrıca özel IP adresini akışı geldiği ulaşabilirsiniz.

Azure, bu işlevi gerçekleştirmek için kaynak ağ adresi çevirisi (SNAT) kullanır. Birden çok özel IP adresleri, tek bir genel IP adresi gösteren, Azure kullanan [adres çevirisini (PAT)'bağlantı noktası](#pat) özel IP adresleri gizlemeye. Kısa ömürlü bağlantı noktaları için PAT kullanıldığında ve [önceden ayrılmış](#preallocatedports) havuz boyutunu temel alan.

Vardır birden çok [giden senaryoları](#scenarios). Bu senaryolar, gerektiği şekilde birleştirebilirsiniz. Bunlar, dağıtım modeli için geçerli olan özellikler, kısıtlamalar ve desenleri dikkatli bir şekilde anlamak için bunları gözden geçirin ve uygulama senaryosu. Gözden geçirme Kılavuzu [bu senaryoları yönetme](#snatexhaust).

>[!IMPORTANT] 
>Standart Load Balancer ve standart genel IP yeni yetenekler ve farklı davranışları giden bağlantı sunar.  Bunlar temel SKU'ları ile aynı değildir.  Standart SKU'lar ile çalışırken giden bağlantı istiyorsanız, bunu standart genel IP adresleri veya genel bir Standard Load Balancer ile açıkça tanımlamalısınız.  Bu, kullanırken giden bağlantı ve standart iç Load Balancer oluşturma içerir.  Standart bir genel yük Dengeleyicideki her zaman giden kuralları kullanmanızı öneririz.  [Senaryo 3](#defaultsnat) standart SKU ile kullanılamaz.  Standart bir iç yük dengeleyici kullanıldığında anlamına giden bağlantı istiyorsanız arka uç havuzundaki sanal makineleri için giden bağlantı oluşturmak için adımları uygulamanız gerekir.  Giden bağlantı, bir tek başına VM, tüm sanal makinenin bir kullanılabilirlik kümesi'ndeki bağlamında bir VMSS ağdaki tüm örnekleri bir grup olarak davranır. Bir kullanılabilirlik kümesindeki tek bir sanal makine standart bir SKU ile ilişkili ise standart SKU ile ilişkili oldukları gibi tek bir örneği ile doğrudan ilgili olmasa bile anlamına gelir, bu kullanılabilirlik kümesi içindeki tüm VM örnekleri artık aynı kurallara göre davranır.  Genel kavramları anlamanıza, gözden geçirmek için tüm bu belgeyi dikkatli bir şekilde gözden [Standard Load Balancer](load-balancer-standard-overview.md) SKU'ları gözden geçirme arasındaki farklar için [giden kuralları](load-balancer-outbound-rules-overview.md).  Giden kuralları kullanarak, giden bağlantı tüm yönleri üzerinde ayrıntılı denetim sağlar.

## <a name="scenarios"></a>Senaryoya genel bakış

Azure Load Balancer ve ilgili kaynakları açıkça tanımlanmış kullanırken [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).  Azure, şu anda Azure Resource Manager kaynaklarını için giden bağlantı sağlamak için üç farklı yöntem sağlar. 

| SKU'lar | Senaryo | Yöntem | IP protokolleri | Açıklama |
| --- | --- | --- | --- | --- |
| Standart, temel | [1. (İle veya olmadan bir yük dengeleyici) bir örnek düzeyinde ortak IP adresine sahip VM](#ilpip) | SNAT, bağlantı noktası maskelemeyi kullanılmıyor | TCP, UDP VE ICMP, ESP | Azure, örneğin NIC IP yapılandırması için atanan genel IP kullanır. Örneğinin tüm kısa ömürlü bağlantı noktaları kullanılabilir vardır. Standard Load Balancer kullanırken kullanmalısınız [giden kuralları](load-balancer-outbound-rules-overview.md) giden bağlantı açıkça tanımlamak için |
| Standart, temel | [2. Genel Load Balancer (örneğinde örnek düzeyinde ortak IP adresi yok) bir VM ile ilişkili](#lb) | Yük Dengeleyici ön uç bağlantı noktası (PAT) maskelemeyi ile SNAT kullanma | TCP, UDP |Azure genel IP adresi genel yük dengeleyici ön uçlar, birden çok özel IP adresi ile paylaşır. Azure, ön uçlar için PAT kısa ömürlü bağlantı noktaları kullanır. |
| yok ya da temel | [3. Tek başına VM (yük dengeleyici, örnek düzeyinde ortak IP adresi yok)](#defaultsnat) | Bağlantı noktası (PAT) maskelemeyi ile SNAT | TCP, UDP | Azure otomatik olarak SNAT için genel bir IP adresi atar, bu genel IP adresi birden çok özel IP adresi kullanılabilirlik kümesinin ile paylaşır ve bu genel IP adresi kısa ömürlü bağlantı noktalarını kullanır. Bir geri dönüş için yukarıdaki senaryoların senaryodur. Görünürlük ve denetim gerekiyorsa bunu önermiyoruz. |

Azure'da genel IP adresi alanı dışında uç noktaları ile iletişim kurmak için bir VM istemiyorsanız, gerektiğinde erişimi engellemek için ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz. Bölüm [giden bağlantıyı engelliyor](#preventoutbound) Nsg'ler daha ayrıntılı olarak ele alınmaktadır. Tasarım Kılavuzu, uygulama ve tüm giden erişimi olmayan bir sanal ağ yönetme, bu makalenin kapsamı dışında olan.

### <a name="ilpip"></a>Senaryo 1: Örnek düzeyinde ortak IP adresine sahip VM

Bu senaryoda, sanal makine bir örnek düzeyi genel IP (atanmış ILPIP) içeriyor. Giden bağlantılar endişe kadar VM veya yük dengeli olup önemi yoktur. Bu senaryo diğer önceliklidir. Bir ILPIP kullanıldığında, VM ILPIP tüm giden akışlar için kullanır.  

Bir VM'ye atanmış bir genel IP, 1:1 ilişki olduğunu (1 yerine: birçok) ve bir durum bilgisi olmayan 1:1 NAT uygulanan  Kendini gizleyen bağlantı noktası (PAT) kullanılmaz ve tüm kısa ömürlü bağlantı noktaları kullanılabilir sanal makine içeriyor.

Uygulamanız çok sayıda giden akışlar başlatır ve SNAT bağlantı noktası tükenmesi deneyimi, atamayı göz önünde bulundurun bir [SNAT kısıtlamalarını azaltmak için ILPIP](#assignilpip). Gözden geçirme [yönetme SNAT tükenmesi](#snatexhaust) oluşmaz.

### <a name="lb"></a>Senaryo 2: Yük dengeli bir VM örnek düzeyinde ortak IP adresi olmadan

Bu senaryoda, VM'ye bir genel yük dengeleyici arka uç havuzu bir parçasıdır. VM, kendisine atanmış bir genel IP adresi yok. Yük Dengeleyici kaynağı ortak IP ön uç arka uç havuzu arasında bir bağlantı oluşturmak için bir yük dengeleyici kuralı ile yapılandırılması gerekir.

Bu kural yapılandırmasını tamamlamazsanız senaryo için açıklandığı gibi davranıştır [hiçbir örnek düzeyi genel IP ile tek başına VM](#defaultsnat). Kuralın çalışma dinleyici başarılı olması durum araştırması için arka uç havuzu sağlamak gerekli değildir.

Yük dengeli sanal Makineyi bir giden akış oluşturduğunda, Azure genel yük dengeleyici ön uç genel IP adresine giden akış özel kaynak IP adresini çevirir. Azure, bu işlevi gerçekleştirmek için SNAT kullanır. Ayrıca, Azure kullanan [PAT](#pat) gizlemeye genel bir IP adresi arkasında birden çok özel IP adresi. 

Kısa ömürlü bağlantı noktaları yük dengeleyicinin genel IP adresi ön uç, tek tek akış sanal makine tarafından oluşturulan ayırt etmek için kullanılır. Dinamik olarak SNAT kullanan [kısa ömürlü bağlantı noktaları önceden ayrılmış](#preallocatedports) giden akışlar oluşturduğunuzda. Bu bağlamda SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları denir.

SNAT bağlantı noktaları önceden açıklandığı ayrılan [anlama SNAT ve PAT](#snat) bölümü. Bunlar tükenmiş olabilir sınırlı bir kaynak hedeflenmiştir. Nasıl olduğunu anlama açısından önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiği şekilde etkisini anlamak için gözden [yönetme SNAT tükenmesi](#snatexhaust).

Zaman [birden çok genel IP adresi yük dengeleyici temel ile ilişkili](load-balancer-multivip-overview.md), bu genel IP adreslerine giden akışlar için bir aday olan ve bir rastgele seçili.  

Temel yük dengeleyici giden bağlantı durumunu izlemek için kullanabilirsiniz [Load Balancer için Azure İzleyici günlükleri](load-balancer-monitor-log.md) ve [uyarı olay günlüklerini](load-balancer-monitor-log.md#alert-event-log) SNAT bağlantı noktası tükenmesi iletileri izlemek için.

### <a name="defaultsnat"></a>Senaryo 3: Örnek düzeyinde ortak IP adresi olmadan tek başına VM

Bu senaryoda, VM'ye bir genel yük dengeleyici havuzu (ve bir iç Load Balancer standart havuz parçası olmayan) bir parçası değil ve kendisine atanmış bir ILPIP adresi yok. Azure, VM'ye giden bir akış oluşturduğunda, özel kaynak IP adresini bir genel kaynak IP adresine giden akış çevirir. Giden Bu akış için kullanılan genel IP adresini yapılandırılabilir değildir ve bu aboneliğe ait genel IP kaynağı limite karşı sayılmaz. Bu genel IP adresi, size ait değilse ve ayrılmış olamaz. VM veya kullanılabilirlik kümesi veya sanal makine ölçek kümesi dağıtmanız durumunda, bu genel IP adresi serbest bırakılacak ve yeni bir genel IP adresi isteniyor. Bu senaryo için IP adreslerini beyaz listeye ekleme özelliğini kullanmayın. Bunun yerine, diğer iki senaryolardan biri, burada açıkça giden senaryo ve giden bağlantı için kullanılacak genel IP adresi bildirdiğiniz kullanın.

>[!IMPORTANT] 
>Bu senaryo ayrıca olduğunda geçerlidir __yalnızca__ bir iç temel yük dengeleyici olarak eklenir. Senaryo 3 __kullanılamıyor__ iç bir Standard Load Balancer bir VM'ye bağlı olduğunda.  Açıkça oluşturmalısınız [Senaryo 1](#ilpip) veya [Senaryo 2](#lb) iç bir Standard Load Balancer'ı kullanmanın yanı sıra.

Azure, bağlantı noktası maskelemeyi ile SNAT kullanır ([PAT](#pat)) bu işlevi gerçekleştirmek için. Bu senaryo benzer [Senaryo 2](#lb)yoktur dışında kullanılan IP adresi üzerinde denetimi yoktur. Bu senaryo 1 ve 2 mevcut olduğunda için geri dönüş bir senaryodur. Giden adresi üzerinde denetim istiyorsanız bu senaryo önerilmemektedir. Giden bağlantılar, uygulamanız önemli bir parçası ise başka bir senaryo seçmeniz gerekir.

Bölümünde anlatıldığı gibi SNAT bağlantı noktaları önceden ayrılmış [anlama SNAT ve PAT](#snat) bölümü.  Vm'leri bir kullanılabilirlik kümesi paylaşımı serilerindeki hangi katmanda uygulanır belirler.  Tek başına VM kullanılabilirlik kümesi olmadan etkili bir şekilde bir 1 ön tahsis (1024 SNAT bağlantı noktaları) belirleme amacıyla havuzudur. SNAT bağlantı noktaları tükendi sınırlı bir kaynaktır. Nasıl olduğunu anlama açısından önemlidir [tüketilen](#pat). Bu tüketimi için tasarımı ve gerektiği şekilde etkisini anlamak için gözden [yönetme SNAT tükenmesi](#snatexhaust).

### <a name="combinations"></a>Birden çok, birleşik senaryoları

Belirli bir sonucu elde etmek için önceki bölümde açıklanan senaryolardan birleştirebilirsiniz. Birden fazla senaryo mevcut olduğunda, bir öncelik sırası uygular: [Senaryo 1](#ilpip) önceliklidir [Senaryo 2](#lb) ve [3](#defaultsnat). [Senaryo 2](#lb) geçersiz kılmalar [Senaryo 3](#defaultsnat).

Burada, uygulama yoğun hedefleri sınırlı sayıda giden bağlantılar kullanır ancak ayrıca bir yük dengeleyici ön ucuna gelen akışlar alır bir Azure Resource Manager dağıtım buna bir örnektir. Bu durumda, 1 ve 2 Tahliye için senaryoları birleştirebilirsiniz. Ek desenleri için gözden [yönetme SNAT tükenmesi](#snatexhaust).

### <a name="multife"></a> Birden çok ön uç için giden akışlar

#### <a name="standard-load-balancer"></a>Standart Load Balancer

Standart yük dengeleyici kullanan tüm aday aynı giden akışlar için saati [birden çok (Genel) IP ön uç](load-balancer-multivip-overview.md) mevcuttur. Her ön uç, giden bağlantılar için Yük Dengeleme kuralı etkinse, kullanılabilir ön tahsis SNAT bağlantı noktalarının sayısı çarpar.

Bir ön uç IP adresi ile yeni bir Yük Dengeleme kuralı seçenek giden bağlantılar için kullanılmasını engellemek seçebilirsiniz:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normalde, `disableOutboundSnat` seçeneği varsayılan olarak _false_ ve bu kural için arka uç havuzundaki Yük Dengeleme kuralını, ilişkili sanal makinelerin giden SNAT programları gösterir. `disableOutboundSnat` Değiştirilebilir _true_ Load Balancer, bu Yük Dengeleme kuralı, arka uç havuzundaki sanal makinelerin giden bağlantılar için ilişkili ön uç IP adresini kullanmasını önlemek için.  Ve yine de açıklandığı gibi giden akışlar için belirli bir IP adresi atayabilirsiniz [birden fazla birleştirilmiş senaryoları](#combinations) de.

#### <a name="load-balancer-basic"></a>Yük Dengeleyici temel

Temel yük dengeleyici giden akışlar için kullanılmak üzere tek bir ön uç seçtiği zaman [birden çok (Genel) IP ön uç](load-balancer-multivip-overview.md) giden akışlar için aday niteliği. Bu seçim için yapılandırılabilir değildir ve rastgele olarak seçimi algoritması göz önünde bulundurmanız gerekir. Belirli bir IP adresi giden akışlar için açıklandığı gibi atayabilirsiniz [birden fazla birleştirilmiş senaryoları](#combinations).

### <a name="az"></a> Kullanılabilirlik alanları

Kullanırken [standart Load Balancer ile kullanılabilirlik alanları](load-balancer-standard-availability-zones.md), bölgesel olarak yedekli ön uçlar, bölgesel olarak yedekli giden SNAT bağlantıları sağlayabilir ve SNAT programlama bölge hatası devam eder.  Giden SNAT bağlantılar kader bölgesel ön uçlar kullanıldığında, ait oldukları bölge ile paylaşır.

## <a name="snat"></a>SNAT ve PAT anlama

### <a name="pat"></a>Bağlantı noktası kendini gizleyen SNAT (PAT)

Bir genel yük dengeleyici kaynağı VM örnekleri ile ilişkili olduğunda, her bir giden bağlantı kaynağı yeniden. Kaynak sanal ağ özel IP adres alanından yük dengeleyicinin genel IP adresi ön uç için yeniden. Genel IP adres alanı, 5 demet (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) akış benzersiz olması gerekir.  Kendini gizleyen SNAT bağlantı noktası, TCP veya UDP IP protokollerle kullanılabilir.

Kısa ömürlü bağlantı noktaları (SNAT), tek bir genel IP adresi birden çok akış kaynaklı olduğundan bu özel kaynak IP adresini yeniden yazma sonra elde etmek için kullanılır. TCP ve UDP bağlantı noktası kendini gizleyen SNAT algoritması SNAT bağlantı noktaları farklı ayırır.

#### <a name="tcp"></a>TCP SNAT bağlantı noktaları

Akışa tek bir hedef IP adresi, bağlantı noktası başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokol birden çok TCP akışları için her TCP akışı tek bir SNAT bağlantı noktasını kullanır. Bu, aynı genel IP adresinden kaynaklanan ve aynı hedef IP adresi, bağlantı noktası ve protokol gidin, Akışlar benzersiz olmasını sağlar. 

Birden çok akış her farklı bir hedef IP adresi, bağlantı noktası ve protokol, tek bir SNAT bağlantı noktası paylaşın. Hedef IP adresi, bağlantı noktası ve protokol akışları genel IP adresi alanını akışları ayırt etmek için bağlantı noktaları ek kaynak gerek kalmadan benzersiz olun.

#### <a name="udp"></a> UDP SNAT bağlantı noktaları

SNAT UDP bağlantı noktası TCP SNAT bağlantı noktaları'dan farklı bir algoritma tarafından yönetilir.  Yük Dengeleyici için UDP "bağlantı noktası kısıtlı koni NAT" bilinen bir algoritma kullanır.  Hedef IP adresi, bağlantı noktası bağımsız olarak her bir akışa ilişkin bir SNAT bağlantı noktası kullanılır.

#### <a name="exhaustion"></a>Tükendi

SNAT bağlantı noktası kaynaklarını tüketmiş, mevcut akışları SNAT bağlantı noktalarını serbest bırakılana kadar giden akışlar başarısız. Yük Dengeleyici geri kazanır SNAT bağlantı noktaları akışı kapatır ve kullandığında bir [4 dakikalık boşta kalma zaman aşımı](#idletimeout) boşta akışlar SNAT noktalarından tekrar kullanılabilir hale getirme için.

SNAT UDP bağlantı noktaları genel SNAT TCP bağlantı noktaları kullanılan algoritması fark nedeniyle çok hızlı tüketebilir. Tasarım ve ölçek ile bu farkı test etmeniz gerekir.

Yaygın olarak SNAT bağlantı noktası tükenmesi için yol koşulları düzenlerini için gözden [yönetme SNAT](#snatexhaust) bölümü.

### <a name="preallocatedports"></a>Kısa ömürlü bağlantı noktası ön tahsis maskelemeyi SNAT (PAT) bağlantı noktası

Kendini gizleyen SNAT bağlantı noktasını sayısı önceden ayrılmış SNAT kullanılabilir bağlantı noktası sayısını belirlemek için bir algoritma kullanırken arka uç havuzu boyutuna göre azure kullanır ([PAT](#pat)). Kısa ömürlü bağlantı noktaları için belirli genel IP kaynak adresi kullanılabilir SNAT bağlantı noktalarıdır.

SNAT bağlantı noktalarını aynı sayıda UDP ve TCP için sırasıyla önceden ayrılmış ve bağımsız olarak IP Aktarım Protokolü tüketilen.  Ancak, SNAT bağlantı noktası kullanımı akışı UDP veya TCP olduğuna bağlı olarak farklılık gösterir.

>[!IMPORTANT]
>Standart SKU programlama SNAT IP Aktarım Protokolü ve Yük Dengeleme türünden türetilmiş.  SNAT yalnızca TCP Yük Dengeleme kuralı varsa, yalnızca TCP için kullanılabilir. UDP Yük Dengeleme kuralı aynı ön uç gelen aynı arka uç havuzuna sahip yalnızca bir Yük Dengeleme kuralı TCP ve UDP Giden SNAT ihtiyacınız varsa oluşturun.  Bu, UDP için programlama SNAT tetikler.  Bir çalışan kural veya sistem durumu araştırması gerekli değildir.  Temel SKU SNAT SNAT Yük Dengeleme kuralı belirtilen Aktarım Protokolü'ne olursa olsun, her iki IP aktarım protokolü için her zaman programlar.

Azure, her VM'nin NIC IP yapılandırması için bağlantı noktalarını SNAT preallocates. Bir IP yapılandırması havuza eklendiğinde SNAT bağlantı noktalarını arka uç havuz boyutunu temel alan bu IP yapılandırması için önceden ayrılmış. Giden akışlar oluşturulduğunda [PAT](#pat) dinamik olarak (önceden ayrılmış sınıra kadar) kullanır ve akış kapandığında Bu bağlantı noktalarını serbest veya [boşta kalma zaman aşımı](#idletimeout) gerçekleşir.

Aşağıdaki tabloda, arka uç havuz boyutları katmanları için SNAT bağlantı noktası preallocations gösterilmektedir:

| Havuz boyutu (sanal makine örnekleri) | IP yapılandırması başına ön tahsis SNAT bağlantı noktaları|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

>[!NOTE]
> Standart Load Balancer ile kullanırken [birden çok ön uç](load-balancer-multivip-overview.md), önceki tabloda kullanılabilir SNAT bağlantı noktalarının sayısı her ön uç IP adresi çarpar. Örneğin, 50 sanal makinenin 2 Yük Dengeleme kuralları, her bir ayrı ön uç IP adresi ile arka uç havuzu başına IP yapılandırması (2 x 1024) 2048 SNAT çıkış kullanır. Ayrıntılar için bkz. [birden çok ön uç](#multife).

Kullanılabilir SNAT bağlantı noktasına doğrudan akışlar sayıya çevirmez unutmayın. Tek bir SNAT bağlantı noktası için birden fazla benzersiz hedefler yeniden kullanılabilir. Yalnızca akış benzersiz hale getirmek gerekli değilse, bağlantı noktaları tüketilir. Tasarım ve risk azaltma kılavuzu için ilgili bölümüne bakın. [exhaustible bu kaynağı yönetmek nasıl](#snatexhaust) ve açıklayan bölümüne [PAT](#pat).

Arka uç havuzunun boyutunu değiştirmek, bazı oluşturulmuş akışlarınızı etkileyebilir. Arka uç havuz boyutunu artırır ve sonraki katmana geçiş, ön tahsis SNAT bağlantı noktaları yarısını sonraki daha büyük arka uç havuzu katmana geçiş sırasında geri kazanılır. Geri kazanılan SNAT bağlantı noktası ile ilişkili akışlar zaman aşımına uğrar ve yeniden oluşturulmaları gerekir. Yeni bir akış girişiminde bulunulursa, ön tahsis bağlantı noktaları kullanılabilir olduğu sürece, akış hemen başarılı olur.

Arka uç havuz boyutunu azaltır ve daha düşük bir katmana geçiş, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, var olan bağlantı noktaları SNAT ayrılan ve ilgili kendi akışlarını etkilenmez.

SNAT bağlantı noktaları ayırmaları olan belirli IP Aktarım Protokolü (TCP ve UDP korunur ayrı olarak) ve aşağıdaki koşullarda barındırılır:

### <a name="tcp-snat-port-release"></a>SNAT TCP bağlantı noktası sürüm

- Her iki sunucu/istemci FINACK gönderirse SNAT bağlantı noktası 240 saniye sonra kullanıma sunulacaktır.
- Bir lk görülür, SNAT bağlantı noktası 15 saniye sonra kullanıma sunulacaktır.
- Boşta kalma zaman aşımı ulaşıldı, bağlantı noktasını serbest bırakılır.

### <a name="udp-snat-port-release"></a>SNAT UDP bağlantı noktası sürüm

- Boşta kalma zaman aşımı ulaşıldı, bağlantı noktasını serbest bırakılır.

## <a name="problemsolving"></a> Sorun giderme 

Bu bölümde SNAT tükenmesi azaltmaya yardımcı olmak için tasarlanmıştır ve azure'da giden bağlantıları ile ortaya çıkabilir.

### <a name="snatexhaust"></a> SNAT (PAT) bağlantı noktası tükenmesi yönetme
[Kısa ömürlü bağlantı noktaları](#preallocatedports) için kullanılan [PAT](#pat) açıklandığı bir exhaustible kaynağı olan [örnek düzeyinde ortak IP adresi olmadan tek başına VM](#defaultsnat) ve [VM yük dengeli olmayan bir Örnek düzeyi genel IP adresi](#lb).

Aynı hedef IP adresine ve bağlantı noktası birçok giden TCP veya UDP bağlantılarının başlatma, ve giden bağlantılar başarısız inceleyin veya destek birimi tarafından tükettiğini SNAT bağlantı noktaları işiniz kopyaladınız biliyorsanız (önceden ayrılmış [kısa ömürlü bağlantı noktaları](#preallocatedports) tarafından kullanılan [PAT](#pat)), genel risk azaltma birkaç seçeneğiniz vardır. Bu seçenekleri gözden geçirin ve hangi kullanılabilir ve senaryonuz için en iyi olduğuna karar verin. Bu senaryo yönetme bir veya daha fazla yardımcı olabilir.

Giden bağlantı davranışını anlama ile ilgili sorunlar yaşıyorsanız, IP yığın istatistikleri (netstat) kullanabilirsiniz. Veya paket yakalamaları kullanarak bağlantı davranışlarla yardımcı olabilir. Bu paket yakalamaları örneğinizin konuk işletim sistemi içinde gerçekleştirmek veya kullanın [Ağ İzleyicisi paket yakalama için](../network-watcher/network-watcher-packet-capture-manage-portal.md).

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
Senaryonuz için değişiklikleri bir ILPIP atama [örnek düzeyinde ortak IP, bir VM'ye](#ilpip). VM'ye her VM için kullanılan tüm kısa ömürlü bağlantı noktaları genel IP'nin kullanılabilir. (Burada ilgili arka uç havuzu ile ilişkili tüm sanal makinelerin genel IP, kısa ömürlü bağlantı noktaları paylaşılan senaryolarında aksine.) Gibi genel IP adresleri ek maliyeti ve çok sayıda özel IP adreslerini beyaz listeye ekleme olası etkisini göz önünde bulundurmanız eksileri vardır.

>[!NOTE] 
>Bu seçenek, web çalışanı rolü için kullanılamaz.

#### <a name="multifesnat"></a>Birden çok ön uç kullanın

Genel Standard Load Balancer kullanırken, atadığınız [giden bağlantılar için birden çok ön uç IP adresi](#multife) ve [SNAT bağlantı noktalarının kullanılabilir Çarp](#preallocatedports).  Ön uç IP yapılandırması, kural ve ön uç genel IP için SNAT programlamasını tetiklemek için arka uç havuzu oluşturun.  Kural çalışması gerekmez ve bir durum araştırması başarılı olması gerekmez.  Birden çok ön uç için kullanıyorsanız de gelen (yalnızca giden), kullanmanız gereken özel sistem durumu araştırmaları iyi güvenilirlik sağlamak için.

>[!NOTE]
>Çoğu durumda, SNAT bağlantı noktası tükenmesi hatalı tasarımının bir yer işaretidir.  Neden SNAT bağlantı noktaları eklemek için daha fazla ön uçlar kullanmadan önce tükettiğini bağlantı noktaları olduğunu anladığınızdan emin olun.  Daha sonra başarısız olmasına neden olabilecek bir sorunu maskeleme.

#### <a name="scaleout"></a>Ölçeği genişletme

[Bağlantı noktaları önceden ayrılmış](#preallocatedports) arka uç havuzu boyutuna bağlı olarak ve bağlantı noktalarından bazılarını sonraki daha büyük arka uç havuzu boyut katmanını karşılamak için ayrılabilecek gerektiğinde uğramasını azaltmak için katmanları halinde gruplandırılmış atanır.  Belirli bir katman boyutu üst sınırı için arka uç havuzu ölçeklendirme tarafından verilen bir ön uç bağlantı noktası kullanımı SNAT yoğunluğunu artırmak için bir seçeneğiniz olabilir.  Bu uygulama için verimli bir şekilde ölçeklendirmek gerektirir.

Örneğin, iki arka uç havuzundaki sanal makinelerin 1024 SNAT bağlantı noktası başına dağıtım için bağlantı noktalarını 2048 SNAT toplam izin vererek, IP yapılandırması gerekir.  Dağıtım 50 sanal artırılacak olsaydı makinelerin sayısı SNAT bağlantı noktalarını bağlantı noktalarının kalır sabiti 51,200 (50 x 1024) toplam sanal makine başına önceden ayrılmış olsa bile dağıtım tarafından kullanılabilir.  Dağıtımınızı genişletmek istiyorsanız, sayısını denetleyin [bağlantı noktaları önceden ayrılmış](#preallocatedports) emin olmak için katman, ölçeği genişletme ilgili katman için en fazla şekil.  Ölçeği, 50 örneğe yerine 51 için seçtiyseniz önceki örnekte, uç ve bir sonraki katmana yukarı toplam olduğu gibi de VM başına daha az SNAT bağlantı durumu.

Sonraki daha büyük arka uç havuzu boyutu katmana ölçeği genişletme, varsa bazı zaman aşımına giden bağlantılar için olası ayrılabilecek ayrılmış bağlantı noktaları gerekir.  Yalnızca, SNAT bağlantı noktalarından bazılarını kullanıyorsanız, arasında sonraki daha büyük arka uç havuz boyutunu genişletme önemsizdir.  Yarı var olan bağlantı noktaları her zaman bir sonraki arka uç havuzu katmana taşıdığınız ayrılacaktır.  Bunun gerçekleşmesi için istemiyorsanız, katman boyutu dağıtımınıza şekil gerekir.  Veya gerektiğinde uygulamanızı algılamak ve yeniden deneyin emin olun.  TCP canlı tutma nasıl yardımcı olabileceğine içinde SNAT önceliksiz nedeniyle işlevi artık bağlantı noktalarının ne zaman gerçekleştiğini algılayın.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımı sıfırlamak için canlı tutma kullanın

Giden bağlantılar 4 dakikalık boşta kalma zaman aşımı vardır. Bu zaman aşımı ayarlanabilir değildir. Ancak, boş bir akış yenileyin ve gerekirse bu boşta kalma zaman aşımı sıfırlamak için Aktarım (örneğin, canlı tutma TCP) veya uygulama katmanı canlı tutma kullanabilirsiniz.  

TCP canlı tutma kullanırken, bunları bir bağlantı tarafında etkinleştirmek yeterlidir. Örneğin, bunları yalnızca boşta Zamanlayıcı akışın sıfırlamak için sunucu tarafında etkinleştirmek yeterli olduğundan ve başlatılan TCP canlı tutma için her iki tarafın için gerekli değildir.  Veritabanı istemci-sunucu yapılandırmalarını dahil olmak üzere uygulama katmanı için benzer kavramları vardır.  Uygulama belirli canlı tutma için hangi seçenekler mevcut için sunucu tarafı denetleyin.

## <a name="discoveroutbound"></a>Bir VM kullanan genel IP bulma
Bir giden bağlantı genel kaynak IP adresini belirlemek için birçok yolu vardır. OpenDNS, sanal makinenizin genel IP adresini göstermek bir hizmet sunar. 

Nslookup komutunu kullanarak bir DNS sorgusu adı myip.opendns.com için OpenDNS çözümleyiciye gönderebilirsiniz. Hizmet, sorgu göndermek için kullanılan kaynak IP adresini döndürür. Aşağıdaki sorgu, VM'den çalıştırdığınızda, o sanal makine için kullanılan genel IP yanıt verilmiştir:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Giden bağlantısını engelliyor
Bazen bir giden akış oluşturmak için izin verilmesi bir VM için istenmeyen olabilir. Hangi hedefleri ile giden akışlar ulaşılabilir yönetmek için bir gereksinim olabilir veya hangi hedefleri gelen akışlar başlayabilirsiniz. Bu durumda, kullanabileceğiniz [ağ güvenlik grupları](../virtual-network/security-overview.md) VM ulaşabileceği hedeflerini yönetmek için. Nsg'ler, hangi ortak hedef gelen akışlar başlatabilirsiniz yönetmek için de kullanabilirsiniz.

Yük dengeli bir VM için bir NSG uyguladığınızda, dikkat [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kuralları](../virtual-network/security-overview.md#default-security-rules). VM sistem durumu araştırma isteklerine Azure yük Dengeleyiciden alabileceğinizden emin olmanız gerekir. 

Bir NSG AZURE_LOADBALANCER varsayılan etiket gelen sistem durumu araştırması istekleri engelliyorsa, VM sistem durumu araştırması başarısız olur ve VM düşürüleceği. Yük Dengeleyici bu VM'ye yeni akışlar göndermeyi durdurur.

## <a name="limitations"></a>Sınırlamalar
- DisableOutboundSnat Yük Dengeleme kuralı portalında yapılandırırken bir seçenek olarak kullanılamaz.  REST, şablonu veya istemci araçları kullanın.
- Web çalışanı rolü bir sanal ağ ve diğer Microsoft platform Hizmetleri olmadan pre-sanal ağ hizmetleri ve platform hizmetleri nasıl işlevi gelen bir yan etkisi nedeniyle yalnızca bir iç standart yük dengeleyici kullanıldığında erişilebilir. Bu yan etkisi ilgili hizmet güvenmeyin veya temel platform değiştirilebilir. Her zaman, bir iç standart yük dengeleyici yalnızca kullanırken isterseniz açıkça giden bağlantı oluşturmak için ihtiyacınız varsaymalısınız. [SNAT varsayılan](#defaultsnat) senaryo bu makalede açıklanan 3 kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- Daha fazla bilgi edinin [giden kuralları](load-balancer-outbound-rules-overview.md) genel bir Standard Load Balancer için.
- Daha fazla bilgi edinin [yük dengeleyici](load-balancer-overview.md).
- Daha fazla bilgi edinin [ağ güvenlik grupları](../virtual-network/security-overview.md).
- Başka bir tuşa bazıları hakkında bilgi edinin [ağ özelliklerinden](../networking/networking-overview.md) azure'da.
