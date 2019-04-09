---
title: Azure web uygulaması güvenlik duvarı - sık sorulan sorular
description: Bu sayfa, Azure ön kapısı hizmeti hakkında sık sorulan soruların yanıtlarını sağlar
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: kumud;tyao
ms.openlocfilehash: 05d01851d0a3dc9df6c396e862ce93defd957c70
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59255929"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall"></a>Azure web uygulaması güvenlik duvarı için sık sorulan sorular

Bu makalede, Azure web uygulaması Güvenlik Duvarı (WAF) özellikleri ve işlevleri hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="what-is-azure-waf"></a>Azure WAF nedir?

Azure WAF, web uygulamalarınızı SQL ekleme, siteler arası betik ve diğer web açıklarına gibi ortak tehditlerden korunmasına yardımcı, bir web uygulaması Güvenlik Duvarı ' dir. Web uygulamalarınızı erişimi denetlemek için özel ve yönetilen kurallarından oluşan bir birleşiminden oluşan bir WAF ilkesi tanımlayabilirsiniz.

Bir WAF Azure İlkesi, uygulama ağ geçidi veya Azure ön kapısı services üzerinde barındırılan web uygulamalarına uygulanabilir.

## <a name="what-is-waf-for-azure-front-door-service"></a>WAF Azure ön kapısı hizmeti nedir? 

Azure ön kapısı yüksek oranda ölçeklenebilir, Global olarak dağıtılmış bir uygulama ve içerik teslim ağı ' dir. Ön kapısı ile tümleştirildiğinde azure WAF, hizmet reddi durdurur ve hedef Azure ağı uç cihazlarında uygulama saldırılarına, sanal ağınıza girmeden önce yakın saldırı kaynakları koruma performanstan ödün vermeden sunar.

## <a name="how-will-i-be-charged-for-azure-waf-for-front-door"></a>Nasıl Azure waf ön kapı için ücretlendirilirim?
Genel Önizleme süresince, ön Kapıda WAF kullanım ücretsiz. Ön kapısı ücret ek olduğunu unutmayın. Ön kapısı hizmet fiyatlandırmaya [burada](https://azure.microsoft.com/pricing/details/frontdoor/).

## <a name="does-azure-waf-support-https"></a>Azure WAF HTTPS destekliyor mu?

SSL boşaltma ön kapısı hizmeti sunar. WAF, yerel olarak ön kapısı ile tümleştirilir ve şifresi çözülür sonra bir istek inceleyebilirsiniz.

## <a name="does-azure-waf-support-ipv6"></a>Azure WAF IPv6 destekliyor mu?

Evet. IPv4 ve IPv6 IP kısıtlaması yapılandırabilirsiniz.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>Yönetilen kural kümelerini nasıl güncel misiniz?

Tehdit ortamını değiştirmeye kalmasını sağlamak için elimizden geleni. Yeni bir kural güncelleştirildikten sonra varsayılan kural kümesi ile yeni bir sürüm numarası eklenir.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>Bir değişiklik my WAF ilkeye yaparsanız yayılması zaman nedir?

Bir WAF İlkesi dağıtımı genel genellikle yaklaşık 5 dakika sürer ve genellikle daha çabuk tamamlanır.

## <a name="can-waf-policies-be-different-for-different-regions"></a>WAF ilkeleri için farklı bölgelere farklı olabilir mi?

WAF ön kapısı hizmeti ile tümleştirildiğinde, genel bir kaynaktır. Aynı yapılandırma, tüm ön kapısı konumlar arasında geçerlidir.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>My ön kapısı yalnızca olmasını uç için nasıl erişim sınırlama?

Yalnızca ön kapısı giden IP adresi aralıkları için izin verme veya tüm Internet'ten doğrudan erişimini, arka uçtaki IP erişim denetim listesi yapılandırabilirsiniz. Hizmet etiketleri, sanal ağınızda kullanabilmeniz için desteklenir. Ayrıca, X iletilen konak HTTP üstbilgisi alanını web uygulamanız için geçerli olduğunu doğrulayabilirsiniz.




## <a name="which-azure-waf-options-should-i-choose"></a>Hangi Azure WAF seçenekleri seçmem gerekir?

Azure'da WAF ilkeleri uygularken iki seçenek vardır. WAF ile Azure ön kapısı bir Global olarak dağıtılmış bir kenar güvenlik çözümüdür. WAF uygulama ağ geçidi ile bölgesel, adanmış bir çözümdür. Genel performans ve güvenlik gereksinimlerinize göre bir çözüm seçin öneririz. Daha fazla bilgi için [yük dengeleyici ile Azure'nın uygulama teslim paketi](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite).


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>WAF ile aynı özellikleri tümleşik tüm platformlarda destekleniyor mu?

Şu anda ModSec CRS 2.2.9 ve CRS 3.0 kuralları yalnızca Application Gateway WAF ile desteklenmektedir. Hız sınırlama, coğrafi filtreleme ve Azure yönetilen varsayılan kural kümesi kuralları, yalnızca Azure ön kapısı, WAF ile desteklenir.

## <a name="is-ddos-protection-integrated-with-front-door"></a>DDoS koruması, ön kapısı ile tümleşiktir? 

Azure ağ kenarlar Global olarak dağıtılmış, Azure ön kapısı, devralarak ve coğrafi olarak büyük hacimli saldırıları yalıtır. İmzaları bilinen sınır http (s) saldırılarını otomatik olarak engelleme ve hızını özel WAF ilke oluşturabilirsiniz. Daha fazla daha, DDoS koruması standart, arka uçları dağıtıldığı sanal ağ üzerinde etkinleştirebilirsiniz. Azure DDoS koruması standart müşteriler maliyet koruma, SLA garantisi ve uzmanlara anında yardım için erişim DDoS hızlı yanıt ekibinden bir saldırı sırasında da dahil olmak üzere ek avantajlardan yararlanabilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure web uygulaması güvenlik duvarı](waf-overview.md).
- Daha fazla bilgi edinin [Azure ön kapısı](front-door-overview.md).
