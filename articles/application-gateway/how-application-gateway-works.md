---
title: Azure uygulama ağ geçidi çalışır
description: Bu makalede nasıl bilgi, uygulama ağ geçidi çalışıyor sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: bbaf651233d4cebad3f45e5cf3823bcaf6ce38b6
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905792"
---
# <a name="how-application-gateway-works"></a>Uygulama ağ geçidi nasıl çalışır?

Bu makalede, gelen istekleri ve bunları arka ucuna yönlendiren uygulama ağ geçidinin nasıl kabul açıklanmaktadır.

![nasıl uygulama ağ geçidi çalışır](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-a-request-is-accepted"></a>Nasıl bir istek kabul edilir

Bir istemci uygulama ağ geçidine bir istek göndermeden önce bir etki alanı adı sistemi (DNS) sunucusu kullanarak uygulama ağ geçidinin etki alanı adı çözümler. Uygulama ağ geçitlerinizi azure.com etki alanında olduğundan DNS girişi, Azure tarafından denetlenir. Azure DNS IP adresi olan istemciye döndürür *ön uç IP adresi* uygulama ağ geçidinin. Uygulama ağ geçidi, bir veya daha fazla gelen trafiği kabul eder *dinleyicileri*. Dinleyici bağlantı isteklerini denetleyen bir mantıksal bir varlıktır. Fronted IP adresi, protokol ve istemcilerinden gelen bağlantıları uygulama ağ geçidi için bağlantı noktası numarası ile yapılandırılır. Web uygulaması Güvenlik Duvarı (WAF) etkinleştirilirse, Application Gateway istek üstbilgilerini gövdesi (varsa) karşı denetler ve *WAF kurallarını* isteğin geçerli bir isteği - bu durumda olup olmadığını belirlemek için bu için yönlendirilir arka uç - veya güvenlik tehdidi, istek çalışması engellenir.  

Uygulama ağ geçidi, bir iç uygulama yük dengeleyici veya bir Internet'e yönelik uygulama yük dengeleyici olarak kullanılabilir. Internet'e yönelik bir uygulama ağ geçidi genel IP adresleri bulunur. DNS Internet'e yönelik bir uygulama ağ geçidi genel IP adresini genel olarak çözümlenebilen adıdır. Bu nedenle, Internet'e yönelik uygulama ağ geçitleri, Internet üzerinden istemcilerden gelen istekleri yönlendirebilirsiniz. İç uygulama ağ geçitleri, yalnızca özel IP adresine sahip. Özel IP adresini genel olarak çözümlenebilen iç uygulama ağ geçidi DNS adı. Bu nedenle, iç yük Dengeleyiciler yalnızca uygulama ağ geçidi için sanal ağdan sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz. Uygulama ağ geçitleri, hem Internet'e yönelik hem de iç isteklerini, özel IP adresleri kullanarak, arka uç sunucularına yönlendirir. Bu nedenle, arka uç sunucularınızın bir iç veya Internet'e yönelik bir uygulama ağ geçidi istekleri almak için genel IP adresleri olması gerekmez.

## <a name="how-a-request-is-routed"></a>Bir isteği nasıl yönlendirileceğini

İstek geçerli olması için bulunursa (veya WAF etkin değilse), *yönlendirme kuralı iste* ilişkili *dinleyici* belirlemek için değerlendirilir *arka uç havuzu* için hangi yönlendirilmesini isteğidir. Kurallar, portalda listelendikleri sırayla işlenir. Temel *yönlendirme kuralı iste* yapılandırma, uygulama ağ geçidi, tüm istekleri dinleyici belirli bir arka uç havuzuna yönlendirmek veya bunları URL yolu veya çok bağlı olarak farklı arka uç havuzlarına yönlendirebilirsiniz karar verir *yeniden yönlendirme istekleri* başka bir bağlantı noktasına veya dış site

Bir kez bir *arka uç* *havuzu* seçilen, uygulama ağ geçidi bir arka uç sunucularının sağlıklı olup havuzunda yapılandırılmış (y.y.y.y) istek gönderir. Sunucusunun sistem durumunu tarafından belirlenen bir *durum araştırması*. Arka uç havuzunda birden fazla sunucu içeriyorsa, uygulama ağ geçidi hepsini bir kez deneme algoritması kullanır isteklerini sağlıklı sunucular arasında yönlendirir, böylece istekleri sunuculardaki yük.

Bir arka uç sunucusuna belirlendikten sonra uygulama ağ geçidi yapılandırmada göre arka uç sunucusuyla yeni bir TCP oturumu açılır *HTTP ayarları*. *HTTP ayarları* protokol, bağlantı noktası ve diğer yönlendirme ile ilgili belirten bir bileşeni arka uç sunucusuyla yeni bir oturum oluşturmak için gerekli olduğu ayarlıyor. HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafik, böylece uçtan uca SSL yerine getirmeye veya şifrelenmemiş şifreli olup olmadığını belirler. Uygulama ağ geçidi özgün istek için arka uç sunucusuna gönderilirken, ilgili geçersiz kılma için ana bilgisayar adı, yol, Protokolü HTTP ayarlarında yapılan herhangi bir özel yapılandırma geliştirir; Bakımı tanımlama bilgilerine dayalı oturum benzeşimi, bağlantı boşaltma, arka uç, vb. konak adından çekme.

İç uygulama ağ geçidi, yalnızca özel IP adresine sahiptir. İç uygulama ağ geçidi DNS adını, özel IP adresini dahili olarak çözülebilir. Bu nedenle, iç yük Dengeleyiciler uygulama ağ geçidi için yalnızca sanal ağdan sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz.

Arka uç havuzu dahili olarak çözülebilir FQDN veya özel bir IP adresi varsa, uygulama ağ geçidi isteği örneği özel IP adreslerini kullanarak arka uç sunucusuna yönlendirir. Application Gateway, arka uç havuzu, dış uç noktası veya harici olarak çözülebilir FQDN içeriyorsa, istek ön uç genel IP adresini kullanarak arka uç sunucusuna yönlendirir. DNS çözümlemesi bir özel DNS bölgesi veya özel bir DNS sunucusu yapılandırdıysanız temel veya Azure DNS tarafından sağlanan varsayılan alır. Ön uç genel IP adresi hazırlamadıysanız biri için giden dış bağlantı atanır.

### <a name="modifications-to-the-request"></a>İstek yapılan değişiklikler

Uygulama ağ geçidi istekleri arka uca iletir önce tüm istekler için 4 ek üst bilgilere ekler. Bu üst X-iletilen-için X iletilen proto, X iletilen bağlantı ve X özgün ana bilgilerdir. X-iletilen-için üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Geçerli değerler x iletilen proto için HTTP veya HTTPS ' dir. X iletilen bağlantı, uygulama ağ geçidinde istek sınırına bağlantı noktasını belirtir. X-özgün ana bilgisayar üst bilgisi ile istek gelen özgün ana bilgisayar üst bilgisini içerir. Bu başlığı trafiğin arka uca yönlendirilmesini önce gelen barındırma üst bilgisi nerede değişiklik Azure Web sitesi tümleştirmesi, bu gibi senaryolarda yararlı olur. Oturum benzeşimi etkinleştirilirse, isteğe bağlı olarak, ardından bir ağ geçidi yönetilen benzeşim tanımlama bilgisi eklenir. 

Ayrıca üst bilgileri kullanarak değiştirmek için uygulama ağ geçidini yapılandırabilirsiniz [yeniden HTTP üstbilgileri](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) veya URI yolu yol geçersiz kılma ayarı kullanarak değiştirebilirsiniz. Ancak yapılandırılması sürece tüm gelen istekler için arka uç olarak taşınır.


## <a name="next-steps"></a>Sonraki adımlar

Uygulama ağ geçidi nasıl çalıştığı hakkında daha fazla edindikten sonra bkz: [uygulama ağ geçidi bileşenleri](application-gateway-components.md) bileşenlerinin daha ayrıntılı olarak anlamak için.
