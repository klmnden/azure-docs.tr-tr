---
title: Azure uygulama ağ geçidi çalışır
description: Bu makalede nasıl bilgi, uygulama ağ geçidi çalışıyor sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 06206ececcb1a51da402c4232f19801793c1cd4a
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807346"
---
# <a name="how-application-gateway-works"></a>Uygulama ağ geçidi nasıl çalışır?

Uygulama ağ geçidi olan bir bulut uygulama teslim denetleyicisi ile Katman 7 (HTTP) Yük Dengeleme, web trafiğini sunucularınız genelinde yönetmenizi sağlar. Ayrıca, ayrıca yaygın web açıklarına ve güvenlik açıkları web hizmetlerinizi merkezi koruma sağlayan bir Web uygulaması Güvenlik Duvarı (WAF) özellikleri sağlar.

Bir istemci uygulama ağ geçidi için bir HTTP isteği yaptığında, ters bir proxy gibi davranır ve trafiği yapılandırmanıza göre arka uç sunucularına iletir. Ayrıca, uygulama ağ geçidi Ayrıca arka uç sunucularını izler ve bu trafiğin yalnızca sağlıklı arka uçları yönlendirir sağlar.

![nasıl uygulama ağ geçidi çalışır](.\media\how-application-gateway-works\how-application-gateway-works.png)

## <a name="how-request-is-accepted"></a>İstek nasıl kabul edilir

Bir istemci uygulama ağ geçidine bir istek göndermeden önce bir etki alanı adı sistemi (DNS) sunucusu kullanarak uygulama ağ geçidinin etki alanı adı çözümler. Uygulama ağ Geçitlerinizi azure.com etki alanında olduğundan DNS girişi, Azure tarafından denetlenir. Azure DNS IP adresi olan istemciye döndürür *ön uç IP adresi* uygulama ağ geçidinin. Application Gateway, bir veya daha fazla gelen trafiği kabul eder *dinleyicileri*. Dinleyici bağlantı isteklerini denetleyen bir işlemdir. Fronted IP adresi, protokol ve istemcilerinden gelen bağlantıları uygulama ağ geçidi için bağlantı noktası numarası ile yapılandırılır. WAF etkinleştirilirse, Application Gateway istek üstbilgilerini gövdesi (varsa) karşı denetler ve *WAF kurallarını* isteğin geçerli bir isteği - bu durumda olup olmadığını belirlemek için arka uç - ya da bir güvenlik tehdidi içinde yönlendirilir İstek durumda engellenir.  

## <a name="how-request-is-routed"></a>İstek nasıl yönlendirileceğini

İstek geçerli olması için bulunursa (veya WAF etkin değilse), *yönlendirme kuralı iste* ilişkili *dinleyici* belirlemek için değerlendirilir *arka uç havuzu* için hangi yönlendirilmesini isteğidir. Kurallar, portalda listelendikleri sırayla işlenir. Temel *yönlendirme kuralı iste* yapılandırma, uygulama ağ geçidi, tüm istekleri dinleyici belirli bir arka uç havuzuna yönlendirmek veya bunları URL yolu veya çok bağlı olarak farklı arka uç havuzlarına yönlendirebilirsiniz karar verir *yeniden yönlendirme istekleri* başka bir bağlantı noktasına veya dış site

Bir kez bir *arka uç* *havuzu* seçilen, Application Gateway bir arka uç sunucularının sağlıklı olup havuzunda yapılandırılmış (y.y.y.y) istek gönderir. Sunucusunun sistem durumunu tarafından belirlenen bir *durum araştırması*. Arka uç havuzunda birden fazla sunucu içeriyorsa, Application Gateway hepsini bir kez deneme algoritması kullanır isteklerini sağlıklı sunucular arasında yönlendirir, böylece istekleri sunuculardaki yük.

Bir arka uç sunucusuna belirlendikten sonra Application Gateway yapılandırmasında temel arka uç sunucusuyla yeni bir TCP oturumu açılır *HTTP ayarları*. *HTTP ayarları* protokol, bağlantı noktası ve diğer yönlendirme ile ilgili belirten bir bileşeni arka uç sunucusuyla yeni bir oturum oluşturmak için gerekli olduğu ayarlıyor. HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafiğin, böylece uçtan uca SSL yerine getirmeye veya şifrelenmemiş şifreli olup olmadığını belirler. Application Gateway, arka uç sunucusuna özgün istek gönderirken ilgili geçersiz kılma için ana bilgisayar adı, yol, Protokolü HTTP ayarlarında yapılan herhangi bir özel yapılandırma geliştirir; Bakımı tanımlama bilgilerine dayalı oturum benzeşimi, bağlantı boşaltma, arka uç, vb. konak adından çekme.

Application gateway Ayrıca bu üç varsayılan HTTP * üstbilgileri ekler: x-iletilen-için x iletilen proto ve x-iletilen-bağlantı noktasına iletilen arka ucuna isteği.

Arka uç sunucu isteği işler ve sayfa içeriği ile bir HTTP yanıtı uygulama ağ geçidine gönderir sonra uygulama ağ geçidi sayfasını tarayıcıda görüntülendiği istemciye yanıt iletir.

## <a name="application-load-balancing-type"></a>Uygulama Yük Dengeleme türü

Bir iç uygulama yük dengeleyici ya da Internet'e yönelik uygulama load balancer, uygulama ağ geçidi kullanabilirsiniz. Internet'e yönelik bir uygulama ağ geçidi genel IP adresleri bulunur. DNS Internet'e yönelik bir uygulama ağ geçidi genel IP adresini genel olarak çözümlenebilen adıdır. Bu nedenle, Internet'e yönelik uygulama ağ geçitleri, Internet üzerinden istemcilerden gelen istekleri yönlendirebilirsiniz.

İç uygulama ağ geçidi, yalnızca özel IP adresine sahiptir. İç uygulama ağ geçidi DNS adını, özel IP adresini dahili olarak çözülebilir. Bu nedenle, iç yük Dengeleyiciler uygulama ağ geçidi için yalnızca sanal ağdan sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz.

Hem Internet'e yönelik hem de iç uygulama ağ geçitleri, arka uç sunucularına özel IP adresleri kullanarak istekleri yönlendirmeyi unutmayın. Arka uç havuzu kaynağınızı özel bir IP adresi, VM'nin NIC yapılandırması veya dahili olarak çözümlenebilir bir adres içeriyorsa ve arka uç havuzu genel bir uç nokta ise, uygulama ağ geçidi ön uç genel IP tarafından sunucuya ulaşmak için kullanır. Ön uç genel IP adresi hazırlamadıysanız biri için giden dış bağlantı atanır.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway nasıl çalıştığı hakkında daha fazla edindikten sonra Git [uygulama ağ geçidi bileşenleri](application-gateway-components.md) bileşenlerinin daha ayrıntılı olarak anlamak için.
