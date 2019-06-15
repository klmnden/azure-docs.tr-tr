---
title: Azure ön kapısı hizmeti - Yönlendirme mimarisi | Microsoft Docs
description: Bu makale ön kapısı'nın mimari genel görünüm yönünü anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 6af5e7c7d8788dffa8f144b2ee77c291ceda86c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736300"
---
# <a name="routing-architecture-overview"></a>Yönlendirme mimarisine genel bakış

Azure ön kapısı istemcinizi aldığında hizmet ardından, ya da yanıtlar (önbelleğe alma etkinse) uygulamaları veya ileten bunları uygun uygulama arka uca (ters Ara sunucu) ister.

</br>Trafik yönlendirme Azure ön kapısı yanı sıra arka uçları için yönlendirme en iyi duruma getirme fırsatları vardır.

## <a name = "anycast"></a>(Anycast) yönlendirme trafiği için ön kapı ortam seçme

Azure ön kapısı yönlendirme ortamları yararlanır [Anycast](https://en.wikipedia.org/wiki/Anycast) DNS (etki alanı adı sistemi) ve HTTP (Köprü Metni Aktarım Protokolü) trafiği için bu nedenle kullanıcı trafiğinin gider ağ topolojisi (en az açısından en yakın ortamına atlama). Bu mimari, daha iyi gidiş dönüş süreleri (bölme TCP avantajlarını en üst düzeye), son kullanıcılar için genellikle sunar. Ön kapı "kademesini" birincil ve geri dönüş, ortamlarına düzenler.  Dış halkası, düşük gecikme sunan kullanıcılara yakın olan ortamlar sahiptir.  Bir sorun ortaya çıkar, dış halkası ortam için bir yük devretme işleyebilir ortamları iç halkanın sahiptir. Dış halkası tüm trafiği tercih edilen hedef olan, ancak iç halkanın dış halkası taşmasından trafiği işlemek gerekli değildir. VIP (sanal Internet Protokolü adresleri) açısından her ön uç ana bilgisayar veya etki alanı ön kapısı tarafından sunulan ortamlarının duyurulur yalnızca bir geri dönüş VIP yanı sıra, iç ve dış halkası ortamlarda tarafından bildirilen bir birincil VIP atanır İç halka. 

</br>Bu genel bir strateji, son kullanıcılarınızın gelen istekleri her zaman en yakın ön kapısı ortam ulaşmak ve tercih edilen ön kapısı ortamı sağlıksız olsa bile, ardından trafiği otomatik olarak sonraki en yakın ortama hareket etmesini sağlar.

## <a name = "splittcp"></a>(Bölme TCP) ön kapısı ortamla bağlantı kuruluyor

[Bölünmüş TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) yüksek bir gidiş dönüş süresi daha küçük parçalara ödemesi gerekir bir bağlantı bölerek gecikme süreleri ve TCP sorunları azaltmak için bir tekniktir.  Son kullanıcılara yaklaştırarak ön kapısı ortamları yerleştirme ve ön kapısı ortam içindeki TCP bağlantılarını sonlandıran bir TCP bağlantı uygulama arka ucuna büyük gidiş dönüş süresini (RTT) ile iki TCP bağlantıları ayrılır. Son kullanıcı ve ön kapısı ortamlar arasında kısa bağlantı üzerinde üç kısa gidiş dönüş yerine üç uzun gecikme süresi kaydetme başvurular, bağlantı kuran anlamına gelir.  Ön kapısı ortamı ve arka uç arasındaki uzun bağlantı, önceden oluşturulmuş ve TCP bağlantı süresi tekrar kaydetmeyi birden çok son kullanıcı çağrıları arasında yeniden kullanılabilir.  Geçerli bağlantının güvenliğini sağlamak için daha fazla gidiş dönüş olarak SSL/TLS (Aktarım Katmanı Güvenliği) bağlantı kurarken çarpılır.

## <a name="processing-request-to-match-a-routing-rule"></a>Yönlendirme kural ile eşleştirilecek istek işleme
Bir SSL yapılması ve bağlantı kurduktan sonra bir istek yönlendirme kuralı eşleşen bir ön kapısı ortamda gölünüzdeki olduğunda el sıkışması, ilk adımdır. Bu eşleşme, hangi belirli yönlendirme kuralı isteği eşleştirilecek kapı, tüm yapılandırmalardan önde temelde belirliyor. Ön kapısı nasıl yaptığını okuma [yol ile eşleşen](front-door-route-matching.md) daha fazla bilgi için.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Mevcut arka uçları yönlendirme kuralı için arka uç havuzundaki tanımlama
Ön kapısı gelen isteğe göre yönlendirme kuralı için bir eşleşme olduğunda ve önbelleğe alma varsa, ardından sonraki adıma eşleşen bir rota ile ilişkili arka uç havuzu araştırma durumu almaktır. Arka uç sistem durumu kullanarak ön kapısı nasıl izlediğini hakkında okuyun [sistem durumu Araştırmalarının](front-door-health-probes.md) daha fazla bilgi için.

## <a name="forwarding-the-request-to-your-application-backend"></a>Uygulama arka ucunuzu iletilmeden
Son olarak, önbellek yok yapılandırılmış olduğu varsayılırsa, kullanıcı isteği göre "iyi" arka uç iletilir, [ön kapısı yönlendirme yöntemini](front-door-routing-methods.md) yapılandırma.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
