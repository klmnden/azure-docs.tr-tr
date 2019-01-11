---
title: Otomatik ölçeklendirme ve bölgesel olarak yedekli Application Gateway'i Azure (genel Önizleme)
description: Bu makalede, web uygulaması güvenlik duvarı istek boyutu sınırları ve Azure portal ile Application Gateway'de hariç tutma listeleri hakkında bilgiler sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 1/10/2019
ms.author: victorh
ms.openlocfilehash: f5885fd2ac76550990c9a56a1d200bbe11555918
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213765"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Otomatik ölçeklendirme ve bölgesel olarak yedekli bir uygulama ağ geçidi (genel Önizleme)

Uygulama ağ geçidi ve Web uygulaması Güvenlik Duvarı (WAF) artık performans geliştirmeleri sunar ve otomatik ölçeklendirme, bölge artıklığı ve statik VIP'ler için destek gibi önemli yeni özellikleri için destek ekleyen yeni bir v2 SKU altında genel önizlemede kullanıma sunuldu. Genel olarak kullanılabilen SKU altında var olan özellikler ile ilgili bilinen kısıtlamaların bölümünde listelenen bazı özel durumlar yeni v2 SKU desteklemeye devam eder. Yeni v2 SKU'ları aşağıdaki geliştirmeler şunları içerir:

- **Otomatik ölçeklendirme**: Uygulama ağ geçidi veya WAF dağıtımlar altında SKU otomatik ölçeklendirme ölçeğini artırabilir veya trafik yük düzenleri değişen aşağı dayalı. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. Bu nedenle, SKU true esneklik sunar. Yeni bir SKU'da Application Gateway hem de sabit kapasite (otomatik ölçeklendirmeyi devre dışı) otomatik ölçeklendirme etkin modda çalışabilir. Sabit kapasite modu, tutarlı ve öngörülebilir iş yüklerine sahip senaryolar için kullanışlıdır. Otomatik ölçeklendirme modu, uygulama trafiği'deki çok sayıda bkz uygulamalarda yararlıdır.
   
   > [!NOTE]
   > WAF SKU'su için otomatik ölçeklendirme şu anda kullanılamıyor. Otomatik ölçeklendirme modu yerine sabit kapasite moduyla WAF yapılandırın.
- **Bölge yedekliliği**: Bir uygulama ağ geçidi veya WAF dağıtım, birden çok kullanılabilirlik alanı, sağlama ve döndürme ayrı Application Gateway örneğinden gereğini ortadan kaldıran bir Traffic Manager ile her bölgedeki yayılabilir. Tek bir bölge veya uygulama ağ geçidi örneklerinin dağıtıldığı birden çok bölge böylece kalmasını sağlama bölge hata dayanıklılığı seçebilirsiniz. Uygulamalar için arka uç havuzu kullanılabilirlik alanları genelinde benzer şekilde dağıtılabilir.
- **Performans iyileştirmeleri**: En fazla 5 X daha iyi SSL SKU sunar otomatik ölçeklendirme, genel olarak kullanılabilen SKU karşılaştırıldığında performans yük boşaltma.
- **Daha hızlı dağıtım ve güncelleştirme zamanı** genel olarak kullanılabilen SKU karşılaştırıldığında daha hızlı dağıtım ve güncelleştirme zamanı SKU otomatik ölçeklendirme sağlar.
- **Statik VIP**: Uygulama ağ geçidi VIP artık statik VIP türü özel olarak destekler. Bu da Application Gateway ile ilişkilendirilmiş VIP değerinin yeniden başlatma sonrasında dahi aynı kalmasını sağlar.

> [!IMPORTANT]
> Otomatik ölçeklendirme yapan ve alanlar arası yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Desteklenen bölgeler
Otomatik ölçeklendirme SKU, Doğu ABD 2, Orta ABD, Batı ABD 2, Orta Kuzey ABD, Batı ABD, Güney Orta ABD, Fransa Orta, Batı Avrupa, Kuzey Avrupa, UK Batı, Güney Doğu Asya ve Japonya Doğu içinde kullanılabilir.

## <a name="pricing"></a>Fiyatlandırma
Önizleme süresince ücretsizdir. Key Vault, sanal makineler, vb. gibi uygulama ağ geçidi dışındaki kaynaklar için faturalandırılırsınız. 

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

|Sorun|Ayrıntılar|
|--|--|
|Kimlik doğrulama sertifikası|Desteklenmiyor.<br>Daha fazla bilgi için [ile Application Gateway uçtan uca SSL'ne genel bakış](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Standard_v2 ve standart Application Gateway, aynı alt ağda karıştırma|Desteklenmiyor.<br>Ayrıca, otomatik ölçeklendirme etkin olduğunda, bir alt ağ yalnızca bir uygulama ağ geçidi olabilir.|
|Kullanıcı tanımlı yol (UDR) uygulama ağ geçidi alt ağı üzerinde|Desteklenmiyor|
|Gelen bağlantı noktası aralığı için NSG| -65200 ila 65535 Standard_v2 için SKU<br>-65503 için 65534 standart SKU için.<br>Daha fazla bilgi için [SSS](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Azure Tanılama'da Performans Günlükleri|Desteklenmiyor.<br>Azure ölçümleri kullanılmalıdır.|
|Faturalandırma|Şu anda hiçbir ödeme yoktur.|
|FIPS modundayken, WebSocket|Bunlar şu anda desteklenmemektedir.|
|ILB yalnızca modu|Bu şu anda desteklenmiyor. Genel ve ILB modu birlikte desteklenir.|
|Web uygulaması Güvenlik Duvarı'nı otomatik ölçeklendirme|WAF, otomatik ölçeklendirme modu desteklemez. Sabit kapasite modu desteklenir.|

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak bir ayrılmış sanal IP adresiyle bir otomatik ölçeklendirme, bölge yedekli uygulama ağ geçidi oluşturma](tutorial-autoscale-ps.md)
- Daha fazla bilgi edinin [Application Gateway](overview.md).
- Daha fazla bilgi edinin [Azure Güvenlik Duvarı](../firewall/overview.md). 

