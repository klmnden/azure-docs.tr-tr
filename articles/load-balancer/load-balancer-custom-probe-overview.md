---
title: Azure Load Balancer'ın ölçeklendirin ve hizmetiniz için yüksek kullanılabilirlik sağlamak için sistem durumu araştırmalarının kullanın
titlesuffix: Azure Load Balancer
description: Yük dengeleyicinin arkasında izlemek için sistem durumu araştırmaları kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: kumud
ms.openlocfilehash: e488a4a6438279270f3d86dafa16c45eda184059
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415692"
---
# <a name="load-balancer-health-probes"></a>Load Balancer sistem durumu araştırmaları

Azure Load Balancer, Yük Dengeleme kuralları ile kullanmak için sistem durumu araştırmalarını sağlar.  Sistem durumu araştırması yapılandırması ve araştırma yanıtları hangi arka uç havuzu örneklerinin yeni akışlar alıp almayacağını belirler. Arka uç örnek bir uygulama hatasını algılamak için sistem durumu araştırmaları kullanabilirsiniz. Ayrıca, özel yanıt durum araştırması oluşturma ve yükleme veya planlanan kapalı kalma süresi yönetmek için akış denetimi için durum araştırması kullanabilirsiniz. Durum araştırması başarısız olduğunda, yük dengeleyici sağlıksız ilgili örneğine yeni akışlar gönderme durdurur.

Sistem durumu araştırmaları, birden çok protokolü destekler. Kullanılabilirlik sistem durumu araştırma belirli bir protokolü destekleyen belirli bir türdeki yük dengeleyici SKU göre değişir.  Ayrıca, hizmet davranışı, yük dengeleyici SKU tarafından değişir.

| | Standart SKU | Temel SKU |
| --- | --- | --- |
| [Araştırma türleri](#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Davranışı araştırma](#probedown) | Tüm araştırmaları, tüm TCP akışları devam edin. | Tüm araştırmaları aşağı tüm TCP akışlar sona erer. | 

> [!IMPORTANT]
> Yük Dengeleyici sistem durumu araştırmalarını 168.63.129.16 IP adresinden kaynaklanan ve örneğinizin işaretlemek araştırmaları için engellenen değil gerekir.  Gözden geçirme [araştırma kaynak IP adresi](#probesource) Ayrıntılar için.

## <a name="types"></a>Araştırma türleri

Durum yoklaması, aşağıdaki üç protokolden kullanarak dinleyicileri için yapılandırılabilir:

- [TCP dinleyicisi](#tcpprobe)
- [HTTP uç noktaları](#httpprobe)
- [HTTPS uç noktaları](#httpsprobe)

Sistem durumu araştırmaları türlerini yük dengeleyici SKU bağlı olarak farklılık gösterir:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standart SKU |    &#9989; |   &#9989; |   &#9989; |
| Temel SKU |   &#9989; |   &#9989; | &#10060; |

Seçtiğiniz araştırma türü ne olursa olsun, herhangi bir bağlantı noktası üzerinde gerçek hizmetin sağlandığı bağlantı noktası dahil olmak üzere, bir arka uç örneğinde sistem durumu araştırmaları gözlemleyebilirsiniz.

### <a name="tcpprobe"></a> TCP araştırma

TCP araştırmaları, bir üç yönlü açık TCP el sıkışması tanımlı bir bağlantı ile gerçekleştirerek bir bağlantıyı başlatırsınız.  TCP araştırmaları bir bağlantıyla bir dört yönlü Kapat TCP el sıkışması sonlandırın.

En düşük araştırma aralığı 5 saniyedir ve iyi durumda olmayan yanıtlar en az sayıda 2'dir.  Tüm aralıkları toplam süresi, 120 saniye aşamaz.

Bir TCP araştırması başarısız olur:
* Örnek noktasındaki TCP dinleyiciyi sırasında zaman aşımı süresi hiç yanıt vermiyor.  Bir araştırma araştırma işaretleme önce yanıtlanmamış gitmek için yapılandırılan başarısız istek sayısı temel alınarak işaretlenir.
* Örneğinden sıfırlama bir TCP araştırması alır.

#### <a name="resource-manager-template"></a>Resource Manager şablonu

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> HTTP / HTTPS araştırma

> [!NOTE]
> HTTPS araştırması için kullanılabilir, yalnızca [Standard Load Balancer](load-balancer-standard-overview.md).

HTTP ve HTTPS araştırmaları, üzerinde bir TCP araştırması oluşturun ve bir HTTP GET ile belirtilen yol sorun. Bu araştırmaların her ikisi de göreli yollar için HTTP GET destekler. HTTPS araştırmaları, aynı Aktarım Katmanı Güvenliği (TLS, SSL adıyla) birlikte HTTP araştırmaları gibi sarmalayıcı. Örnek bir HTTP 200 durum zaman aşımı süresi içinde yanıt verdiğinde durum yoklaması işaretlenir.  Durum yoklaması, yapılandırılmış bir sistem durumu araştırma bağlantı noktası varsayılan olarak her 15 saniyede denetlemek çalışır. En düşük araştırma aralığı 5 saniyedir. Tüm aralıkları toplam süresi, 120 saniye aşamaz.

HTTP / HTTPS araştırmaları ayrıca sistem durumu araştırması ifade etmek istiyorsanız yararlı olabilir.  Araştırma bağlantı noktasını da hizmet için dinleyici ise örneklerini yük dengeleyici rotasyonuna kaldırmak için kendi mantığını uygular. Örneğin, % 90 CPU ise örneğini kaldırmaya karar ve 200 HTTP durum döndürür. 

Artık Cloud Services'ı kullanın ve w3wp.exe kullanan web rolleri, ayrıca otomatik izleme, Web sitesini ulaşın. Web sitesi kodunuzdaki hataları, yük dengeleyici araştırması için 200 durumu döndürür.

Bir HTTP / HTTPS araştırma başarısız:
* Araştırma uç noktasına bir HTTP yanıt kodu 200 (örneğin, 403, 404 veya 500) dışında döndürür. Bu durum araştırması hemen işaretler. 
* Araştırma uç noktası 31 saniyelik zaman aşımı süresi boyunca hiç yanıt vermiyor. Araştırma ve tüm zaman aşımı aralıkları toplamını ulaşılana kadar çalışmıyor olarak işaretleneceğini önce birden fazla araştırma isteklerine yanıtlanmamış geçebilir.
* Araştırma uç noktası, TCP sıfırlama yoluyla bağlantıyı kapatır.

#### <a name="resource-manager-templates"></a>Resource Manager şablonları

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Konuk aracı araştırması (yalnızca klasik)

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri), varsayılan olarak araştırma izlemesi için konuk Aracısı kullanın.  Konuk aracı araştırması son çare yapılandırmadır.  Her zaman açık olarak bir TCP durum araştırması veya HTTP araştırması kullanın. Konuk aracı araştırması, çoğu uygulama senaryoları için açıkça tanımlanmış araştırmaları olabildiğince verimli değildir.

Konuk aracı araştırması, Konuk aracısının VM içindeki bir denetimdir. Dinler ve hazır durumda yalnızca örnektir ile bir HTTP 200 OK yanıtı yanıt verdiği. (Geri dönüştürme veya durduruluyor meşgul diğer durumlar vardır.)

Daha fazla bilgi için [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmaları](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için genel yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Konuk Aracısı HTTP 200 OK ile yanıt vermezse, yük dengeleyici örnek yanıt vermiyor olarak işaretler. Ardından, bu örneğe akışlar gönderme durdurur. Yük Dengeleyici örneği denetlemek devam eder. 

Yük Dengeleyici yeni akışlar bu örneğe Konuk Aracısı bir HTTP 200 yanıt verirse, yeniden gönderir.

Bir web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan yapı veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıt) hataları Konuk Aracısı ile bildirilen değildir. Sonuç olarak, yük dengeleyici rotasyon dışında bu örneğe almaz.

<a name="health"></a>
## <a name="probehealth"></a>Sistem durumu araştırması

TCP, HTTP ve HTTPS sistem durumu araştırmaları sağlıklı olarak kabul edilir ve rol örneği iyi durumda olduğunda olarak işaretle:

* VM başlatıldıktan sonra sistem durumu araştırması kez başarılı olur.
* Belirtilen sayıda rol örneği sağlıklı olarak işaretlemek için gerekli bir araştırmalarla kazanmıştır.

> [!NOTE]
> İyi durumda rol örneği getirmeden önce yük dengeleyici durum yoklaması dalgalanma, artık bekler. Bu ek bekleme süresi, kullanıcı ve altyapı korur ve kasıtlı bir ilkedir.

## <a name="probe-count-and-timeout"></a>Yoklama sayısı ve zaman aşımı

Araştırma davranışı bağlıdır:

* İşaretlenecek örneği izin başarılı bir araştırmalarla sayısı kadar yukarı.
* İşaretlenecek örneği neden başarısız yoklama sayısını kapalı olarak.

Belirtilen zaman aşımı ve aralık değerleri bir örneği olarak işaretlenmiş olup olmadığını belirlemek yukarı veya aşağı doğru.

## <a name="probedown"></a>Davranışı araştırma

### <a name="tcp-connections"></a>TCP bağlantıları

Yeni TCP bağlantıları kalan sağlam bir arka uç örneklerine başarılı olur.

Bir arka uç örneği durum araştırması başarısız olursa, yerleşik TCP bağlantıları bu arka uç örneğe devam edin.

Yeni akış, bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, arka uç havuzuna gönderilir. Standart Load Balancer, devam etmek için yerleşik TCP akışları izin verir.  Temel yük dengeleyici arka uç havuzu için tüm mevcut TCP akışları sona erer.
 
Yük Dengeleyici (TCP bağlantıları sonlanmamasına) hizmeti aracılığıyla bir pass ve akışı her zaman istemci ve sanal makinenin konuk işletim sistemi ve uygulama arasında. Bir havuzla tüm araştırmaları bir ön uç akışı alır ve SYN bildirim ile yanıt vermek için sağlam bir arka uç örnek olarak TCP bağlantı açma denemelerinin (SYN) yanıt vermemesine neden olur

### <a name="udp-datagrams"></a>UDP veri birimi

UDP veri birimi için sağlıklı arka uç örnekleriyle verilecektir.

UDP bağlantısız ve izlenen UDP için hiçbir akış durumu yoktur. Herhangi bir arka uç örneğinin sistem durumu araştırması başarısız olursa, mevcut UDP akışları arka uç havuzunda iyi durumda başka bir örneğine taşıyabilir.

Bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, temel ve standart Load balancer'ları için mevcut UDP akışları sonlanacaktır.

<a name="source"></a>
## <a name="probesource"></a>Kaynak IP adresi araştırma

Yük Dengeleyici, dağıtılmış bir yoklama hizmeti için kendi iç sistem durumu modeli kullanır. Araştırma her konakta bulunduğunu bilmesine olduğu VM'ler ve programlanmış müşterinin yapılandırma başına sistem durumu araştırmaları oluşturmak için isteğe bağlı olabilir. Sistem durumu araştırması trafiği doğrudan durum yoklaması oluşturur yoklama hizmeti ile VM müşteri arasındadır. Tüm yük dengeleyici sistem durumu araştırmaları, kaynak 168.63.129.16 IP adresinden kaynaklanan.  IP adres alanı RFC1918 boşluk olmayan bir sanal ağ içinde kullanabilirsiniz.  Genel olarak ayrılmış kullanarak, Microsoft IP adresi, sanal ağ içinde kullandığınız IP adres alanı ile bir IP adresi çakışması olasılığını azaltır ait.  Bu IP adresi değişmez tüm bölgelerde aynıdır ve yalnızca iç Azure platformu bileşenini bu IP adresinden bir paket kaynağı bir güvenlik riski değil. 

Bu kaynak IP adresi AzureLoadBalancer hizmet etiketi tanımlar, [ağ güvenlik grupları](../virtual-network/security-overview.md) ve varsayılan olarak sistem durumu araştırması trafiğe izin verir.

Yük Dengeleyici sistem durumu araştırmaları yanı sıra [aşağıdaki işlemleri bu IP adresi kullanın](../virtual-network/what-is-ip-address-168-63-129-16.md):

- VM platformu ile iletişim kurmak için bir "Hazır" durumunda olduğu sinyal aracısı sağlar
- Özel DNS sunucuları tanımlamaz müşterilere filtrelenmiş ad çözümlemesi sağlamak için DNS sanal sunucu ile iletişimi sağlar.  Bu filtreleme müşteriler bunların dağıtım ana bilgisayar adları yalnızca çözümleyebilmesini sağlar.
- DHCP hizmeti azure'da dinamik bir IP adresi almak VM sağlar.

## <a name="design"></a> Tasarım Kılavuzu

Sistem durumu araştırmaları, hizmeti dayanıklı hale ve ölçeklendirme olanak tanımak için kullanılır. Yanlış yapılandırma veya hatalı bir tasarım modeli, kullanılabilirlik ve ölçeklenebilirlik hizmetinizin etkileyebilir. Tüm bu belgeyi gözden geçirin ve ne senaryonuz etkisini Bu araştırma yanıt düşürüleceği olduğunda veya olarak işaretlenmiş ve uygulama senaryonuzu kullanılabilirliğini ne etkiler göz önünde bulundurun.

Uygulamanız için sistem durumu modeli tasarladığınızda, bu örneğe durumunu yansıtan bir arka uç örneğindeki bir bağlantı noktası araştırma __ve__ sağlayan uygulama hizmeti.  Uygulama bağlantı noktasını ve araştırma bağlantı noktası aynı olması gerekmez.  Bazı senaryolarda, servis uygulamanızı sağlar bağlantı noktası değerinden farklı olacak şekilde araştırma bağlantı noktası için istenen olabilir.  

Bazen yalnızca, uygulama durumunu algılayan, ancak ayrıca örneğinizi almak veya yeni akışlar almamayı doğrudan yük dengeleyiciye sinyal sistem durumu araştırma yanıtı oluşturmak, uygulamanız için yararlı olabilir.  Durum araştırması başarısız olarak yeni akışlar backpressure ve azaltma teslim örneğine oluşturmak veya uygulamanızın bakım için hazırlanmanıza ve senaryonuz boşaltma başlatmak için uygulamanıza izin vermek için araştırma yanıt işleyebilirsiniz.  Standart Load Balancer'ı kullanırken bir [aşağı araştırma](#probedown) sinyal TCP akışları, boşta kalma zaman aşımı veya bağlantı kapatma kadar devam etmek her zaman sağlayacaktır. 

UDP Yük Dengeleme için arka uç örnek bir özel durum araştırması sinyal oluşturmak ve UDP uygulama durumunu yansıtacak şekilde karşılık gelen dinleyici hedefleyen bir TCP, HTTP veya HTTPS durum araştırması kullanabilirsiniz.

Kullanırken [HA bağlantı noktaları Yük Dengeleme kuralları](load-balancer-ha-ports-overview.md) ile [standart Load Balancer](load-balancer-standard-overview.md), yükü dengelenmiş tüm bağlantı noktalarının ve tek bir sistem durumu araştırma yanıt tüm örneği durumu yansıtmalıdır.

Proxy bir sistem durumu araştırma bu yapılandırmayı sizin senaryonuzda zincirleme hatalara neden olabileceğinden, sanal ağda başka bir örneği için sistem durumu araştırması aldığı örnek üzerinden ya da değil çevir.  Aşağıdaki senaryoları düşünün: üçüncü taraf ağ Gereçleri kümesi bir yük dengeleyici kaynağı cihazları için ölçek ve yedeklilik sağlamak için arka uç havuzundaki dağıtılır ve durum yoklaması araştırma bir bağlantı noktası yapılandırıldığında, üçüncü taraf Gereci proxy'leri veya Gereç arkasındaki diğer sanal makineler için çevirir.  Gereç arkasında tek bir sanal makineden herhangi bir araştırma yanıt çevirmek için kullandığınız aynı bağlantı noktasını ya da proxy istekleri gereç arkasındaki diğer sanal makineler için araştırma, gereç ölü işaretler. Bu yapılandırma tüm uygulama senaryonun sonucunda bir gereç arkasında tek bir arka uç örnek geçişli hata neden olabilir.  Tetikleyici, Load Balancer'ı (Gereci örnek) özgün hedef işaretlenecek neden olur ve tüm uygulama senaryonuzu sırayla devre dışı bırakabilirsiniz bir aralıklı araştırma hatası olabilir. Bunun yerine gerecin durumu araştırma. Sistem durumu sinyal belirlemek için araştırma seçimini ağ sanal Gereçleri (NVA) senaryoları için önemli bir noktadır ve uygun bir sistem durumu sinyali gibi senaryolar için nedir için uygulama satıcınıza başvurun gerekir.

İzin verme durumunda [kaynak IP](#probesource) örneğinizin ulaşamadık olduğu gibi güvenlik duvarı ilkelerini araştırması sistem durumu araştırması başarısız olur.  Buna karşılık, yük dengeleyici örneğinizin sistem durumu araştırma hatası nedeniyle aşağı işaretler.  Bu yanlış yapılandırma, yük dengeli uygulama senaryonuzu başarısız olmasına neden olabilir.

Örneğiniz işaretlemek Load Balancer'ın sistem durumu araştırması için **gerekir** bu IP adresi herhangi bir Azure izin [ağ güvenlik grupları](../virtual-network/security-overview.md) ve yerel güvenlik duvarı ilkeleri.  Varsayılan olarak, her bir ağ güvenlik grubu içerir [hizmet etiketi](../virtual-network/security-overview.md#service-tags) AzureLoadBalancer sistem durumu araştırması trafiğe izin vermek için.

Bir sistem durumu araştırma hatası test veya tek bir örneğini işaretlemek istiyorsanız, kullanabileceğiniz bir [ağ güvenlik grupları](../virtual-network/security-overview.md) durum yoklaması açıkça engelleyecek (hedef bağlantı noktası veya [kaynak IP](#probesource)) ve benzetimi Araştırma hatası.

Sanal ağınıza ait 168.63.129.16 içeren IP adresi aralığı Microsoft ile yapılandırmayın.  Bu tür yapılandırmaları durum yoklaması IP adresiyle birbiriyle çakışır ve senaryonuz başarısız olmasına neden olabilir.

Sanal makinenizde birden fazla arabirimi varsa, temel alınan arabirimde araştırma yanıt Sigortası gerekir.  Kaynak ağ gerekebilir adresi bu adresi bir arabirim başına temelinde VM'deki çevir.

Etkinleştirmeyin [TCP zaman damgaları](https://tools.ietf.org/html/rfc1323).  TCP zaman damgaları etkinleştirme, sistem durumu araştırmaları sanal makinenin konuk işletim sistemi TCP yığını, yük Dengeleyicide işaretlenmesi ilgili uç noktayı aşağı sonuçları tarafından bırakılan TCP paketleri nedeniyle başarısız olmasına neden olur.  Varsayılan olarak güvenlik düzenli olarak TCP zaman damgaları etkin sıkı VM görüntüleri ve devre dışı bırakılmalıdır.

## <a name="monitoring"></a>İzleme

Hem genel hem de iç [Standard Load Balancer](load-balancer-standard-overview.md) uç ve arka uç örnek araştırma durumu Azure İzleyicisi üzerinden çok boyutlu ölçümler olarak kullanıma sunar. Bu ölçümler, diğer Azure Hizmetleri veya iş ortağı uygulamalar tarafından tüketilebilir. 

Genel Load Balancer temel sistem durumu araştırma durumu Azure İzleyici günlüklerine aracılığıyla arka uç havuzu başına özetlenen kullanıma sunar.  Azure İzleyici günlüklerine iç temel yük Dengeleyiciler için kullanılabilir değil.  Kullanabileceğiniz [Azure İzleyici günlükleri](load-balancer-monitor-log.md) herkese açık yük dengeleyici araştırması durumunu denetleyin ve yoklama sayısı. Günlüğe kaydetme, Power BI veya Azure operasyonel İçgörüler ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

- HTTPS araştırmaları, bir istemci sertifikası ile karşılıklı kimlik doğrulamasını desteklemez.
- TCP zaman damgaları etkinleştirildiğinde sistem durumu araştırmaları başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- [PowerShell kullanarak Resource Manager'da herkese açık yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md)
- [Sistem durumu araştırmaları için REST API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Yeni sistem durumu araştırması becerileriyle ile istek [Load Balancer'ın Uservoice](https://aka.ms/lbuservoice)
