---
title: Hizmetinizi korumak için yük dengeleyici sistem durumu araştırmalarını kullanan | Microsoft Docs
description: Yük dengeleyicinin arkasında izlemek için sistem durumu araştırmaları kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2018
ms.author: kumud
ms.openlocfilehash: 58bc0c0669992b8b3884e24c39862f47412b9110
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584664"
---
# <a name="load-balancer-health-probes"></a>Load Balancer sistem durumu araştırmaları

Azure Load Balancer, hangi arka uç havuzu örneklerinin yeni akışlar alırsınız belirlemek için sistem durumu araştırmaları kullanır. Arka uç örnek bir uygulama hatasını algılamak için sistem durumu araştırmaları kullanabilirsiniz. Ayrıca özel yanıt durum araştırması oluşturmak ve da durum yoklaması için akış denetimini kullanın ve yük dengeleyiciye yeni akışlar göndereceğini ya da yeni akışlar bir arka uç örneğe gönderilmesini durdurmaya devam edilip edilmeyeceğini sinyal. Bu yük ya da planlanan kapalı kalma süresi yönetmek için kullanılabilir. Durum araştırması başarısız olduğunda, yük dengeleyici sağlıksız ilgili örneğine yeni akışlar gönderme durdurur.

Kullanılabilir sistem durumu araştırmaları türlerini ve hangi SKU, yük Dengeleyicide, sistem durumu araştırmaları davranır bağlıdır biçimini kullanmaktadır. Örneğin, yeni ve mevcut akışlar davranışını bir akış olarak TCP veya UDP de hangi yük dengeleyici SKU kullandığınız olduğuna bağlıdır.

| | Standart SKU | Temel SKU |
| --- | --- | --- |
| [Araştırma türleri](#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Davranışı araştırma](#probedown) | Tüm araştırmaları, tüm TCP akışları devam edin. | Tüm araştırmaları aşağı tüm TCP akışları sonlandırın. | 

> [!IMPORTANT]
> Yük Dengeleyici sistem durumu araştırmalarını 168.63.129.16 IP adresinden kaynaklanan ve örneğinizin işaretlemek için araştırmalar için engellenen değil gerekir.  Gözden geçirme [araştırma kaynak IP adresi](#probesource) Ayrıntılar için.

## <a name="types"></a>Araştırma türleri

Sistem durumu araştırmaları, bir arka uç örneğindeki gerçek hizmetinin sağlanan bağlantı noktası dahil olmak üzere herhangi bir bağlantı noktası gözlemleyebilirsiniz. Sistem durumu araştırması protokolü, üç farklı türde sistem durumu araştırmaları için yapılandırılabilir:

- [TCP dinleyicisi](#tcpprobe)
- [HTTP uç noktaları](#httpprobe)
- [HTTPS uç noktaları](#httpsprobe)

Sistem durumu araştırmaları türlerini yük dengeleyici SKU bağlı olarak farklılık gösterir:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standart SKU |    &#9989; |   &#9989; |   &#9989; |
| Temel SKU |   &#9989; |   &#9989; | &#10060; |

UDP Yük Dengeleme, özel sistem durumu araştırması sinyal ya da bir TCP ve HTTP kullanarak arka uç örnek oluşturma veya HTTPS durum yoklaması.

Kullanırken [HA bağlantı noktaları Yük Dengeleme kuralları](load-balancer-ha-ports-overview.md) ile [standart Load Balancer](load-balancer-standard-overview.md), yükü dengelenmiş tüm bağlantı noktalarının ve tek bir sistem durumu araştırma yanıt tüm örneği durumu yansıtmalıdır.  

NAT veya proxy bir sistem durumu araştırma bu sizin senaryonuzda zincirleme hatalara neden olabileceğinden, başka bir örneği için sistem durumu araştırması ağınızda alır örnek üzerinden gerekir.

Bir sistem durumu araştırma hatası test veya tek bir örneğini işaretlemek istiyorsanız, bir güvenlik grubuna açık blok durum araştırması kullanabilirsiniz (hedef veya [kaynak](#probesource)).

### <a name="tcpprobe"></a> TCP araştırma

TCP araştırmaları, bir üç yönlü açık TCP el sıkışması tanımlı bir bağlantı ile gerçekleştirerek bir bağlantıyı başlatırsınız.  Bu, ardından bir dört yönlü Kapat TCP el sıkışması tarafından izlenir.

En düşük araştırma aralığı 5 saniyedir ve iyi durumda olmayan yanıtlar en az sayıda 2'dir.  Toplam süresi, 120 saniye aşamaz.

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

HTTP ve HTTPS araştırmaları, TCP bağlantısı kurmak ve bir HTTP GET ile belirtilen yol sorun. Bu araştırmaların her ikisi de göreli yollar için HTTP GET destekler. HTTPS araştırmaları, aynı Aktarım Katmanı Güvenliği (TLS, SSL adıyla) birlikte HTTP araştırmaları gibi sarmalayıcı. Örnek bir HTTP 200 durum zaman aşımı süresi içinde yanıt verdiğinde durum yoklaması işaretlenir.  Bu sistem durumu araştırmaları yapılandırılmış sistem durumu araştırma bağlantı noktası varsayılan olarak 15 saniyelik aralıklarla denetleyin dener. En düşük araştırma aralığı 5 saniyedir. Toplam süresi, 120 saniye aşamaz. 

HTTP / HTTPS araştırmaları de yük dengeleyici rotasyonuna örnekleri kaldırmak için kendi mantığını uygulamak istediğinizde yararlı olabilir. Örneğin, % 90 CPU ise örneğini kaldırmaya karar ve 200 HTTP durum döndürür. 

Artık Cloud Services'ı kullanın ve w3wp.exe kullanan web rolleri, ayrıca otomatik izleme, Web sitesini ulaşın. Web sitesi kodunuzdaki hataları, yük dengeleyici araştırması için 200 durumu döndürür.  HTTP araştırması varsayılan Konuk aracı araştırması geçersiz kılar. 

Bir HTTP / HTTPS araştırma başarısız:
* Araştırma uç noktasına bir HTTP yanıt kodu 200 (örneğin, 403, 404 veya 500) dışında döndürür. Bu durum yoklaması hemen işaretler. 
* Araştırma uç noktası 31 ikinci zaman aşımı süresi boyunca hiç yanıt vermiyor. Araştırma çalışmıyor olarak işaretleneceğini önce ayarlanan zaman aşımı değeri bağlı olarak birden fazla araştırma isteklerine yanıtlanmamış geçebilir (diğer bir deyişle, önce SuccessFailCount araştırmaları gönderilir).
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

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri), varsayılan olarak araştırma izlemesi için konuk Aracısı kullanın.   Bu bir seçenek son çare düşünmelisiniz.  Her zaman açık olarak bir TCP durum araştırması veya HTTP araştırması tanımlamanız gerekir. Konuk aracı araştırması, çoğu uygulama senaryoları için açıkça tanımlanmış araştırmaları olabildiğince verimli değildir.  

Konuk aracı araştırması, Konuk aracısının VM içindeki bir denetimdir. Dinler ve hazır durumda yalnızca örnektir ile bir HTTP 200 OK yanıtı yanıt verdiği. (Geri dönüştürme veya durduruluyor meşgul diğer durumlar vardır.)

Daha fazla bilgi için [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmaları](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için genel yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Konuk Aracısı HTTP 200 OK ile yanıt vermezse, yük dengeleyici örnek yanıt vermiyor olarak işaretler. Ardından, bu örneğe akışlar gönderme durdurur. Yük Dengeleyici örneği denetlemek devam eder. 

Yük Dengeleyici yeni akışlar bu örneğe Konuk Aracısı bir HTTP 200 yanıt verirse, yeniden gönderir.

Bir web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan yapı veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıt) hataları Konuk Aracısı ile bildirilen değildir. Sonuç olarak, yük dengeleyici rotasyon dışında bu örneğe almaz.

## <a name="probehealth"></a>Sistem durumu araştırması

TCP, HTTP ve HTTPS sistem durumu araştırmaları sağlıklı olarak kabul edilir ve rol örneği iyi durumda olduğunda olarak işaretle:

* Durum yoklaması, VM önyükleme ilk kez başarılı olur.
* (Daha önce açıklanan) SuccessFailCount için rol örneği sağlıklı olarak işaretlemek için gerekli başarılı bir araştırmalarla değerini tanımlar. Bir rol örneği kaldırıldıysa, başarılı, art arda gelen yoklama sayısını eşit veya rol örneği çalışıyor olarak işaretlemek için SuccessFailCount değerini aşıyor.

> [!NOTE]
> Bir rol örneğinin durumunu dalgalanma, iyi durumda rol örneği getirmeden önce yük dengeleyici artık bekler. Bu ek bekleme süresi, kullanıcı ve altyapı korur ve kasıtlı bir ilkedir.

## <a name="probe-count-and-timeout"></a>Yoklama sayısı ve zaman aşımı

Araştırma davranışı bağlıdır:

* İşaretlenecek örneği izin başarılı bir araştırmalarla sayısı kadar yukarı.
* İşaretlenecek örneği neden başarısız yoklama sayısını kapalı olarak.

SuccessFailCount içinde ayarlanan zaman aşımı ve sıklık değerleri örneğini çalıştıran ya da çalışmıyor onaylanıp onaylanmadığını belirler. Azure portalında, zaman aşımı sıklığı değerini iki kez ayarlanır.

Yük Dengeleme kuralı tek bir durum araştırması ilgili arka uç havuzuna tanımlanır.

## <a name="probedown"></a>Davranışı araştırma

### <a name="tcp-connections"></a>TCP bağlantıları

Yeni bir TCP bağlantısı iyi durumda olduğundan ve bir konuk işletim sistemi ve uygulama yeni bir akış kabul etmeyi arka uç örneğe başarılı olur.

Bir arka uç örneği durum araştırması başarısız olursa, yerleşik TCP bağlantıları bu arka uç örneğe devam edin.

Yeni akış, bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, arka uç havuzuna gönderilir. Standart Load Balancer, devam etmek için yerleşik TCP akışları izin verir.  Temel yük dengeleyici arka uç havuzu için tüm mevcut TCP akışları sona erer.
 
Akışı, her zaman sanal makinenin konuk işletim sistemi ve istemci arasında olduğundan, tüm araştırmaları bir havuzla bir ön uç akışını almak için sağlam bir arka uç örnek olarak TCP bağlantı açma denemelerinin yanıt vermemesine neden olur.

### <a name="udp-datagrams"></a>UDP veri birimi

UDP veri birimi için sağlıklı arka uç örnekleriyle verilecektir.

UDP bağlantısız ve izlenen UDP için hiçbir akış durumu yoktur. Herhangi bir arka uç örneğinin sistem durumu araştırması başarısız olursa, mevcut UDP akışları arka uç havuzunda iyi durumda başka bir örneğine taşıyabilir.

Bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, temel ve standart Load balancer'ları için mevcut UDP akışları sonlanacaktır.

## <a name="probesource"></a>Kaynak IP adresi araştırma

Yük Dengeleyici, dağıtılmış bir yoklama hizmeti için kendi iç sistem durumu modeli kullanır. VM'lerin bulunduğu her bir ana bilgisayar başına müşterinin yapılandırma sistem durumu araştırmaları oluşturmak için programlanabilir. Sistem durumu araştırması doğrudan durum yoklaması oluşturur altyapı bileşeni ve müşterinin sanal makine arasında trafiğidir. Tüm yük dengeleyici sistem durumu araştırmaları, kaynak 168.63.129.16 IP adresinden kaynaklanan.  Azure sanal ağ için kendi IP adreslerinizi getirebilir, bu sistem durumu araştırması kaynak IP adresini Microsoft ayrılmış genel olarak benzersiz olması garanti edilir.  Bu adres, tüm bölgelerde aynıdır ve değişmez. Yalnızca iç Azure platformu bu IP adresinden bir paket kaynağı bir güvenlik riski düşünülmemelidir. 

Yük Dengeleyici sistem durumu araştırmaları ek olarak, aşağıdaki işlemleri bu IP adresi kullanın:

- VM platformu ile iletişim kurmak için bir "Hazır" durumunda olduğu sinyal aracısı sağlar
- Özel DNS sunucuları tanımlamaz müşterilere filtrelenmiş ad çözümlemesi sağlamak için DNS sanal sunucu ile iletişimi sağlar.  Bu filtreleme müşteriler bunların dağıtım ana bilgisayar adları yalnızca çözümleyebilmesini sağlar.

Örneğiniz, işaretlemek Load Balancer'ın sistem durumu araştırması için **gerekir** bu IP adresi herhangi bir Azure izin [güvenlik grupları](../virtual-network/security-overview.md) ve yerel güvenlik duvarı ilkeleri.

Bu IP adresini güvenlik duvarı ilkelerinizi izin verme, örneğinizin ulaşamadık olduğundan durum yoklaması başarısız olur.  Buna karşılık, yük dengeleyici örneğinizin sistem durumu araştırma hatası nedeniyle aşağı işaretler.  Bu, yük dengeli Küme hizmetinin başarısız olmasına neden olabilir. 

Ayrıca 168.63.129.16 içeren IP adresi aralığı ait Microsoft ile Vnet'inizi yapılandırmamalısınız.  Bu durum yoklaması IP adresiyle birbiriyle çakışır.

Sanal makinenizde birden fazla arabirimi varsa, temel alınan arabirimde araştırma yanıt Sigortası gerekir.  Bu, benzersiz olarak kaynak NAT'ing bu adresi bir arabirim başına temelinde VM'deki gerektirebilir.

## <a name="monitoring"></a>İzleme

Hem genel hem de iç [Standard Load Balancer](load-balancer-standard-overview.md) uç ve arka uç örnek araştırma durumu Azure İzleyicisi üzerinden çok boyutlu ölçümler olarak kullanıma sunar. Bu, diğer Azure hizmetlerine veya 3. taraf uygulamalar tarafından ardından tüketilebilir. 

Genel Load Balancer temel sistem durumu araştırma durumu Log Analytics aracılığıyla arka uç havuzu başına özetlenen kullanıma sunar.  Bu, iç temel yük Dengeleyiciler için kullanılabilir değildir.  Kullanabileceğiniz [günlük analizi](load-balancer-monitor-log.md) herkese açık yük dengeleyici araştırması durumunu denetleyin ve yoklama sayısı. Günlüğe kaydetme, Power BI veya Azure operasyonel İçgörüler ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

-  HTTPS araştırmaları, bir istemci sertifikası ile karşılıklı kimlik doğrulamasını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- [PowerShell kullanarak Resource Manager'da herkese açık yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md)
- [Sistem durumu araştırmaları için REST API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Yeni sistem durumu araştırması becerileriyle ile istek [Load Balancer'ın Uservoice](https://aka.ms/lbuservoice)
