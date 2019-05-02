---
title: Bir uygulama ağ geçidi nasıl çalışır?
description: Bu makalede bir uygulama ağ geçidi nasıl çalıştığı hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: a16421182f533f5aa2ad4bcc2e58e910cc7e8ca6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702422"
---
# <a name="how-an-application-gateway-works"></a>Bir uygulama ağ geçidi nasıl çalışır?

Bu makalede, nasıl bir uygulama ağ geçidi gelen istekleri kabul eder ve bunları arka ucuna yolları açıklanmaktadır.

![Nasıl bir uygulama ağ geçidi isteği kabul eder](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-an-application-gateway-accepts-a-request"></a>Nasıl bir uygulama ağ geçidi isteği kabul eder

1. Bir istemci, bir uygulama ağ geçidine bir istek göndermeden önce bir etki alanı adı sistemi (DNS) sunucusunu kullanarak uygulama ağ geçidi etki alanı adını çözümler. Azure, tüm application gateway'ler azure.com etki alanında olduğundan DNS girişini denetler.

2. Azure DNS, uygulama ağ geçidi ön uç IP adresi olduğu istemci IP adresini döndürür.

3. Uygulama ağ geçidi, bir veya daha fazla dinleyicileri gelen trafiği kabul eder. Dinleyici bağlantı isteklerini denetleyen bir mantıksal bir varlıktır. Ön uç IP adresi, protokol ve istemcilerinden gelen bağlantıları uygulama ağ geçidi için bağlantı noktası numarası ile yapılandırılır.

4. Bir web uygulaması Güvenlik Duvarı (WAF) kullanılıyorsa, uygulama ağ geçidi istek üst bilgileri ve gövdesini varsa, WAF kurallarına karşı denetler. Bu eylem geçerli istek veya bir güvenlik tehdidi isteği olup olmadığını belirler. İstek geçerliyse, arka uca yönlendirilir. İstek geçerli değilse bir güvenlik tehdidi olarak engellenir.

Azure Application Gateway, bir iç uygulama yük dengeleyici veya bir internet'e yönelik uygulama yük dengeleyici olarak kullanılabilir. İnternet'e yönelik bir uygulama ağ geçidi genel IP adreslerini kullanır. DNS internet'e yönelik bir uygulama ağ geçidi genel IP adresini genel olarak çözümlenebilen adıdır. Sonuç olarak, internet'e yönelik uygulama ağ geçitleri, istemci istekleri internet'e yönlendirebilirsiniz.

İç uygulama ağ geçitleri yalnızca özel IP adresleri kullanın. Özel IP adresini genel olarak çözümlenebilen iç uygulama ağ geçidi DNS adı. Bu nedenle, iç yük Dengeleyiciler yalnızca uygulama ağ geçidi için sanal ağ erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz.

Hem internet'e yönelik hem de iç uygulama ağ geçidi istekleri arka uç sunucularına özel IP adresleri kullanarak yönlendirebilir. Arka uç sunucuları iç gelen istekleri veya internet'e yönelik bir uygulama ağ geçidi almak için genel IP adresleri gerekmez.

## <a name="how-an-application-gateway-routes-a-request"></a>Bir isteğin bir uygulama ağ geçidi tarafından nasıl yönlendirdiği

Bir WAF kullanımda olmayan ya da bir isteğin geçerli olup, uygulama ağ geçidi dinleyicisi ile ilişkili istek yönlendirme kuralı değerlendirir. Bu eylem, isteği yönlendirmek için hangi arka uç havuzu belirler.

Kurallar, portalda listelendikleri sırayla işlenir. İstek yönlendirme kuralı bağlı olarak, uygulama ağ geçidi rota istekleri farklı arka uç havuzları URL yolu veya yeniden yönlendirme istekleri başka bir bağlantı noktasına göre ya da dış dinleyici belirli bir arka uç havuzuna üzerindeki tüm istekleri yönlendirmek belirler site.

Uygulama ağ geçidi arka uç havuzuna seçtiğinde (y.y.y.y) havuzundaki sağlam bir arka uç sunuculardan birine isteği gönderir. Sunucusunun sistem durumunu bir durum araştırması tarafından belirlenir. Arka uç havuzu birden çok sunucu varsa, uygulama ağ geçidi istekleri sağlıklı sunucular arasında dolaştırmak için hepsini bir kez deneme algoritması kullanır. Bu yük sunucularda istekleri.

Uygulama ağ geçidi arka uç sunucusuna belirledikten sonra HTTP ayarları temel alarak arka uç sunucusuyla yeni bir TCP oturumu açılır. HTTP ayarları, protokol, bağlantı noktası ve arka uç sunucusuyla yeni bir oturum oluşturmak için gerekli diğer yönlendirme ile ilgili ayarları belirtin.

HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafiğin (Bu nedenle yerine getirmeye uçtan uca SSL) şifrelenmiş veya şifrelenmemiş belirler.

Bir uygulama ağ geçidi arka uç sunucunun özgün istek gönderdiğinde, ana bilgisayar adı, yol ve protokolü geçersiz kılma için ilgili HTTP ayarlarında yapılan herhangi bir özel yapılandırma geliştirir. Bu eylem, tanımlama bilgilerine dayalı oturum benzeşimi, arka uç ve benzeri bağlantı boşaltma, ana bilgisayar adı seçimi tutar.

Bir iç uygulama ağ geçidi, yalnızca özel IP adreslerini kullanır. Bir iç uygulama ağ geçidi DNS adını, özel IP adresi olarak çözümlenebilir. Sonuç olarak, iç yük Dengeleyiciler yalnızca uygulama ağ geçidi için sanal ağ erişimi olan istemcilerden gelen istekleri yönlendirebilirsiniz.

 >[!NOTE]
 >Hem internet'e yönelik ve iç uygulama ağ geçitleri isteklerini, özel IP adresleri kullanarak arka uç sunucularına yönlendirir. Bu eylem, özel bir IP adresi, VM'nin NIC yapılandırması veya dahili olarak çözümlenebilir adresi arka uç havuzu kaynağınızı içeren gerçekleşir. Arka uç havuzu:
> - **Genel bir uç nokta**, uygulama ağ geçidi tarafından sunucuya ulaşmak için ön uç genel IP kullanır. Ön uç genel IP adresi yoksa, bir giden dış bağlantı için atanır.
> - **Dahili olarak çözülebilir FQDN veya özel bir IP adresi içeren**, kendi örneği özel IP adresleri kullanarak uygulama ağ geçidi isteği arka uç sunucusuna yönlendirir.
> - **Dış uç noktası veya harici olarak çözülebilir FQDN içeren**, ön uç genel IP adresini kullanarak uygulama ağ geçidi isteği arka uç sunucusuna yönlendirir. DNS çözümlemesi bir özel DNS bölgesi veya özel bir DNS sunucusu, yapılandırılması durumunda temel veya Azure tarafından sağlanan DNS varsayılan kullanır. Ön uç genel IP adresi yoksa, bir giden dış bağlantı için atanır.

### <a name="modifications-to-the-request"></a>İstek yapılan değişiklikler

Bir uygulama ağ geçidi istekleri arka uca iletir önce tüm istekler için dört ek üst bilgilere ekler. Bu üst x-iletilen-için x iletilen proto, x iletilen bağlantı ve x özgün ana bilgilerdir. X-iletilen-için üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir.

Geçerli değerler x iletilen proto için HTTP veya HTTPS ' dir. X iletilen bağlantı olduğu uygulama ağ geçidi istek sınırına bağlantı noktasını belirtir. X-özgün ana bilgisayar üst bilgisi ile istek gelen özgün ana bilgisayar üst bilgisini içerir. Bu başlığı trafiğin arka uca yönlendirilmesini önce gelen barındırma üst bilgisi nerede değiştiren Azure Web sitesi tümleştirmesi yararlıdır. Ardından bir seçenek olarak oturum benzeşimi etkinleştirilirse, bir ağ geçidi ile yönetilen benzeşim tanımlama bilgisi ekler.

Üst bilgileri kullanarak değiştirmek için uygulama ağ geçidini yapılandırabilirsiniz [yeniden HTTP üstbilgileri](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) veya yol geçersiz kılma ayarı kullanarak bir URI yolu değiştirmek için. Ancak, bunu yapmak için yapılandırılmış sürece tüm gelen istekleri arka ucuna taşınır.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ağ geçidi bileşenleri hakkında bilgi edinin](application-gateway-components.md)
