---
title: Sistem durumunu izlemek için yük dengeleyici özel araştırmalar kullanın | Microsoft Docs
description: Yük dengeleyicinin arkasında izlemek için Azure Load Balancer için özel araştırmalar'ı kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2018
ms.author: kumud
ms.openlocfilehash: afe46cf9fc710decba4524bd5a0fe1e73804f636
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344173"
---
# <a name="load-balancer-health-probes"></a>Load Balancer sistem durumu araştırmaları

Azure Load Balancer, hangi arka uç havuzu örneklerinin yeni akışlar alırsınız belirlemek için sistem durumu araştırmaları kullanır. Arka uç örnek bir uygulama hatasını algılamak için sistem durumu araştırmaları kullanabilirsiniz. Ayrıca özel yanıt durum araştırması oluşturmak ve da durum yoklaması için akış denetimini kullanın ve yük dengeleyiciye yeni akışlar göndereceğini ya da yeni akışlar bir arka uç örneğe gönderilmesini durdurmaya devam edilip edilmeyeceğini sinyal. Bu yük ya da planlanan kapalı kalma süresi yönetmek için kullanılabilir.

Durum araştırması başarısız olduğunda, yük dengeleyici sağlıksız ilgili örneğine yeni akışlar gönderme durdurur. Yeni ve mevcut akışlar davranışını bir akış olarak TCP veya UDP de hangi yük dengeleyici SKU kullandığınız olmasına göre değişir.  Gözden geçirme [Ayrıntılar için araştırma davranışı aşağı](#probedown).

## <a name="health-probe-types"></a>Sistem durumu araştırması türleri

Sistem durumu araştırmaları, bir arka uç örneğindeki gerçek hizmetinin sağlanan bağlantı noktası dahil olmak üzere herhangi bir bağlantı noktası gözlemleyebilirsiniz. Durum araştırması, TCP dinleyicisi ya da HTTP uç noktalarını destekler. 

UDP Yük Dengeleme için TCP veya HTTP sistem durumu araştırması kullanarak arka uç örnek bir özel durum araştırması sinyal oluşturmanız gerekir.

Kullanırken [HA bağlantı noktaları Yük Dengeleme kuralları](load-balancer-ha-ports-overview.md) ile [standart Load Balancer](load-balancer-standard-overview.md), yükü dengelenmiş tüm bağlantı noktalarının ve tek bir sistem durumu araştırma yanıt tüm örneği durumu yansıtmalıdır.  

NAT veya proxy bir sistem durumu araştırma bu sizin senaryonuzda zincirleme hatalara neden olabileceğinden, başka bir örneği için sistem durumu araştırması ağınızda alır örnek üzerinden gerekir.

### <a name="tcp-probe"></a>TCP araştırma

TCP araştırmaları, bir üç yönlü açık TCP el sıkışması tanımlı bir bağlantı ile gerçekleştirerek bir bağlantıyı başlatırsınız.  Bu, ardından bir dört yönlü Kapat TCP el sıkışması tarafından izlenir.

En düşük araştırma aralığı 5 saniyedir ve iyi durumda olmayan yanıtlar en az sayıda 2'dir.  Toplam süresi, 120 saniye aşamaz.

Bir TCP araştırması başarısız olur:
* Örnek noktasındaki TCP dinleyiciyi sırasında zaman aşımı süresi hiç yanıt vermiyor.  Bir araştırma araştırma işaretleme önce yanıtlanmamış gitmek için yapılandırılan başarısız istek sayısı temel alınarak işaretlenir.
* Örneğinden sıfırlama bir TCP araştırması alır.

### <a name="http-probe"></a>HTTP araştırma

HTTP araştırmaları, TCP bağlantısı kurmak ve bir HTTP GET ile belirtilen yol sorun. HTTP araştırmaları, HTTP GET için göreli yolların destekler. Örnek bir HTTP 200 durum zaman aşımı süresi içinde yanıt verdiğinde durum yoklaması işaretlenir.  Yapılandırılmış bir sistem durumu araştırma bağlantı noktası varsayılan olarak her 15 saniyede denetlemek için HTTP sistem durumu araştırmaları girişimi. En düşük araştırma aralığı 5 saniyedir. Toplam süresi, 120 saniye aşamaz. 


HTTP araştırmaları örneklerini yük dengeleyici rotasyonuna kaldırmak için kendi mantığını uygulamak istediğinizde yararlı olabilir. Örneğin, % 90 CPU ise örneğini kaldırmaya karar ve 200 HTTP durum döndürür. 

Artık Cloud Services'ı kullanın ve w3wp.exe kullanan web rolleri, ayrıca otomatik izleme, Web sitesini ulaşın. Web sitesi kodunuzdaki hataları, yük dengeleyici araştırması için 200 durumu döndürür.  HTTP araştırması varsayılan Konuk aracı araştırması geçersiz kılar. 

Bir HTTP araştırması başarısız olur:
* HTTP araştırma uç noktasına bir HTTP yanıt kodu 200 (örneğin, 403, 404 veya 500) dışında döndürür. Bu durum yoklaması hemen işaretler. 
* HTTP araştırma uç noktası çalışmıyor hiç sırasında bir 31 ikinci zaman aşımı süresi. Araştırma çalışmıyor olarak işaretleneceğini önce ayarlanan zaman aşımı değeri bağlı olarak birden fazla araştırma isteklerine yanıtlanmamış geçebilir (diğer bir deyişle, önce SuccessFailCount araştırmaları gönderilir).
* HTTP araştırma uç noktası, TCP sıfırlama yoluyla bağlantıyı kapatır.

### <a name="guest-agent-probe-classic-only"></a>Konuk aracı araştırması (yalnızca klasik)

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri), varsayılan olarak araştırma izlemesi için konuk Aracısı kullanın.   Bu bir seçenek son çare düşünmelisiniz.  Her zaman bir durum araştırması açıkça ile bir TCP veya HTTP araştırması tanımlamanız gerekir. Konuk aracı araştırması, çoğu uygulama senaryoları için açıkça tanımlanmış araştırmaları olabildiğince verimli değildir.  

Konuk aracı araştırması, Konuk aracısının VM içindeki bir denetimdir. Dinler ve hazır durumda yalnızca örnektir ile bir HTTP 200 OK yanıtı yanıt verdiği. (Geri dönüştürme veya durduruluyor meşgul diğer durumlar vardır.)

Daha fazla bilgi için [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmaları](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için genel yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Konuk Aracısı HTTP 200 OK ile yanıt vermezse, yük dengeleyici örnek yanıt vermiyor olarak işaretler. Ardından, bu örneğe akışlar gönderme durdurur. Yük Dengeleyici örneği denetlemek devam eder. 

Yük Dengeleyici yeni akışlar bu örneğe Konuk Aracısı bir HTTP 200 yanıt verirse, yeniden gönderir.

Bir web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan yapı veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıt) hataları Konuk Aracısı ile bildirilen değildir. Sonuç olarak, yük dengeleyici rotasyon dışında bu örneğe almaz.

## <a name="probe-health"></a>Sistem durumu araştırması

TCP ve HTTP sistem durumu araştırmaları sağlıklı olarak kabul edilir ve rol örneği iyi durumda olduğunda olarak işaretle:

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

> [!IMPORTANT]
> Bir yük dengeleyici durum araştırması 168.63.129.16 IP adresini kullanır. Bu genel IP adresi, iç platform kaynaklara Getir your-kendi-IP Azure sanal ağ senaryosu için iletişimi kolaylaştırır. Tüm bölgelerde genel sanal IP adresi 168.63.129.16 kullanılır ve bunu değiştirmez. Herhangi bir Azure bu IP adreslerine izin verecek öneririz [güvenlik grupları](../virtual-network/security-overview.md) ve yerel güvenlik duvarı ilkeleri. Yalnızca iç Azure platformu bu adresten bir paket kaynağı bir güvenlik riski düşünülmemelidir. Bu IP adresi beklenmeyen davranış senaryoları, çeşitli güvenlik duvarı ilkelerinizi izin verme durumunda yük hata da dahil olmak üzere hizmet dengeli. Ayrıca 168.63.129.16 içeren bir IP adresi aralığı ile Vnet'inizi yapılandırmamalısınız.  Sanal makinenizde birden fazla arabirimi varsa, temel alınan arabirimde araştırma yanıt Sigortası gerekir.  Bu, benzersiz olarak kaynak NAT'ing bu adresi bir arabirim başına temelinde VM'deki gerektirebilir.

## <a name="probedown"></a>Davranışı araştırma

### <a name="tcp-connections"></a>TCP Bağlantıları

Yeni bir TCP bağlantısı iyi durumda olan ve bir konuk işletim sistemi ve uygulama yeni bir akış kabul etmeyi arka uç örneğe başarılı olur.

Bir arka uç örneği durum araştırması başarısız olursa, yerleşik TCP bağlantıları bu arka uç örneğe devam edin.

Yeni akış, bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, arka uç havuzuna gönderilir. Standart Load Balancer, devam etmek için yerleşik TCP akışları izin verir.  Temel yük dengeleyici arka uç havuzu için tüm mevcut TCP akışlar sona erer.
 
Akışı, her zaman sanal makinenin konuk işletim sistemi ve istemci arasında olduğundan, tüm araştırmaları bir havuzla bir ön uç akışını almak için sağlam bir arka uç örnek olarak TCP bağlantı açma denemelerinin yanıt vermemesine neden olur.

### <a name="udp-datagrams"></a>UDP veri birimi

UDP veri birimi için sağlıklı arka uç örnekleriyle verilecektir.

UDP bağlantısız ve izlenen UDP için hiçbir akış durumu yoktur. Herhangi bir arka uç örneğinin sistem durumu araştırması başarısız olursa, mevcut UDP akışları arka uç havuzunda iyi durumda başka bir örneğine taşıyabilir.

Bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, temel ve standart Load balancer'ları için mevcut UDP akışları sonlanacaktır.

## <a name="monitoring"></a>İzleme

Tüm [Standard Load Balancer](load-balancer-standard-overview.md) araştırma durumu Azure İzleyici aracılığıyla örnek başına çok boyutlu ölçümler olarak kullanıma sunar.

Temel yük dengeleyici araştırması durumu Log Analytics aracılığıyla arka uç havuzu başına kullanıma sunar.  Bu, yalnızca genel temel yük Dengeleyiciler için kullanılabilir ve iç temel yük Dengeleyiciler için kullanılabilir.  Kullanabileceğiniz [günlük analizi](load-balancer-monitor-log.md) herkese açık yük dengeleyici araştırması durumunu denetleyin ve yoklama sayısı. Günlüğe kaydetme, Power BI veya Azure operasyonel İçgörüler ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

-  HTTP sistem durumu araştırması TLS (HTTPS) desteklemez.  Bir TCP araştırması 443 numaralı bağlantı noktasını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell kullanarak Resource Manager'da herkese açık yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md)
- [Sistem durumu araştırmaları için REST API](https://docs.microsoft.com/en-us/rest/api/load-balancer/loadbalancerprobes/get)

