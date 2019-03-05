---
title: Azure uygulama ağ geçidi çalışır
description: Bu makalede nasıl bilgi, uygulama ağ geçidi çalışıyor sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: d37114fda7f442a5fa077c8dde9fd8aec3ac4378
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317110"
---
# <a name="how-application-gateway-works"></a>Uygulama ağ geçidi nasıl çalışır?

Bu makalede, gelen istekleri ve bunları arka ucuna yönlendiren uygulama ağ geçidinin nasıl kabul açıklanmaktadır.

![nasıl uygulama ağ geçidi çalışır](.\media\how-application-gateway-works\how-application-gateway-works.png)

## <a name="how-request-is-accepted"></a>İstek nasıl kabul edilir

Bir istemci uygulama ağ geçidine bir istek göndermeden önce bir etki alanı adı sistemi (DNS) sunucusu kullanarak uygulama ağ geçidinin etki alanı adı çözümler. Uygulama ağ geçitlerinizi azure.com etki alanında olduğundan DNS girişi, Azure tarafından denetlenir. Azure DNS IP adresi olan istemciye döndürür *ön uç IP adresi* uygulama ağ geçidinin. Uygulama ağ geçidi, bir veya daha fazla gelen trafiği kabul eder *dinleyicileri*. Dinleyici bağlantı isteklerini denetleyen bir mantıksal bir varlıktır. Fronted IP adresi, protokol ve istemcilerinden gelen bağlantıları uygulama ağ geçidi için bağlantı noktası numarası ile yapılandırılır. Web uygulaması Güvenlik Duvarı (WAF) etkinleştirilirse, Application Gateway istek üstbilgilerini gövdesi (varsa) karşı denetler ve *WAF kurallarını* isteğin geçerli bir isteği - bu durumda olup olmadığını belirlemek için bu için yönlendirilir arka uç - veya güvenlik tehdidi, istek çalışması engellenir.  

Uygulama ağ geçidi, bir iç uygulama yük dengeleyici veya bir Internet'e yönelik uygulama yük dengeleyici olarak kullanılabilir. Internet'e yönelik bir uygulama ağ geçidi genel IP adresleri bulunur. DNS Internet'e yönelik bir uygulama ağ geçidi genel IP adresini genel olarak çözümlenebilen adıdır. Bu nedenle, Internet'e yönelik uygulama ağ geçitleri, Internet üzerinden istemcilerden gelen istekleri yönlendirebilirsiniz. İç uygulama ağ geçitleri, yalnızca özel IP adresine sahip. Özel IP adresini genel olarak çözümlenebilen iç uygulama ağ geçidi DNS adı. Bu nedenle, iç yük Dengeleyiciler yalnızca uygulama ağ geçidi için sanal ağdan sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz. Uygulama ağ geçitleri, hem Internet'e yönelik hem de iç isteklerini, özel IP adresleri kullanarak, arka uç sunucularına yönlendirir. Bu nedenle, arka uç sunucularınızın bir iç veya Internet'e yönelik bir uygulama ağ geçidi istekleri almak için genel IP adresleri olması gerekmez.

## <a name="how-request-is-routed"></a>İstek nasıl yönlendirileceğini

İstek geçerli olması için bulunursa (veya WAF etkin değilse), *yönlendirme kuralı iste* ilişkili *dinleyici* belirlemek için değerlendirilir *arka uç havuzu* için hangi yönlendirilmesini isteğidir. Kurallar, portalda listelendikleri sırayla işlenir. Temel *yönlendirme kuralı iste* yapılandırma, uygulama ağ geçidi, tüm istekleri dinleyici belirli bir arka uç havuzuna yönlendirmek veya bunları URL yolu veya çok bağlı olarak farklı arka uç havuzlarına yönlendirebilirsiniz karar verir *yeniden yönlendirme istekleri* başka bir bağlantı noktasına veya dış site

Bir kez bir *arka uç* *havuzu* seçilen, uygulama ağ geçidi bir arka uç sunucularının sağlıklı olup havuzunda yapılandırılmış (y.y.y.y) istek gönderir. Sunucusunun sistem durumunu tarafından belirlenen bir *durum araştırması*. Arka uç havuzunda birden fazla sunucu içeriyorsa, uygulama ağ geçidi hepsini bir kez deneme algoritması kullanır isteklerini sağlıklı sunucular arasında yönlendirir, böylece istekleri sunuculardaki yük.

Bir arka uç sunucusuna belirlendikten sonra uygulama ağ geçidi yapılandırmada göre arka uç sunucusuyla yeni bir TCP oturumu açılır *HTTP ayarları*. *HTTP ayarları* protokol, bağlantı noktası ve diğer yönlendirme ile ilgili belirten bir bileşeni arka uç sunucusuyla yeni bir oturum oluşturmak için gerekli olduğu ayarlıyor. HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafik, böylece uçtan uca SSL yerine getirmeye veya şifrelenmemiş şifreli olup olmadığını belirler. Uygulama ağ geçidi özgün istek için arka uç sunucusuna gönderilirken, ilgili geçersiz kılma için ana bilgisayar adı, yol, Protokolü HTTP ayarlarında yapılan herhangi bir özel yapılandırma geliştirir; Bakımı tanımlama bilgilerine dayalı oturum benzeşimi, bağlantı boşaltma, arka uç, vb. konak adından çekme.

İç uygulama ağ geçidi, yalnızca özel IP adresine sahiptir. İç uygulama ağ geçidi DNS adını, özel IP adresini dahili olarak çözülebilir. Bu nedenle, iç yük Dengeleyiciler uygulama ağ geçidi için yalnızca sanal ağdan sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz.

Hem Internet'e yönelik hem de iç uygulama ağ geçitleri, arka uç sunucularına özel IP adresleri kullanarak istekleri yönlendirmeyi unutmayın. Arka uç havuzu kaynağınızı özel bir IP adresi, VM'nin NIC yapılandırması veya dahili olarak çözümlenebilir bir adres içeriyorsa ve arka uç havuzu genel bir uç nokta ise, uygulama ağ geçidi ön uç genel IP tarafından sunucuya ulaşmak için kullanır. Ön uç genel IP adresi hazırlamadıysanız biri için giden dış bağlantı atanır.


## <a name="next-steps"></a>Sonraki adımlar

Uygulama ağ geçidi nasıl çalıştığı hakkında daha fazla edindikten sonra bkz: [uygulama ağ geçidi bileşenleri](application-gateway-components.md) bileşenlerinin daha ayrıntılı olarak anlamak için.
