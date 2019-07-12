---
title: Azure Application Gateway'de - Azure portal Web uygulaması güvenlik duvarı kurallarını özelleştirme
description: Bu makalede, Azure portal ile Application Gateway içindeki web uygulaması güvenlik duvarı kurallarını özelleştirme hakkında bilgi sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: f4af52907ab2e950636dea0874b49500f3a6b587
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613450"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Azure portalı üzerinden Web uygulaması güvenlik duvarı kurallarını özelleştirme

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar, açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümesi (CRS tarafından) sağlanır. Bazı kurallar, hatalı pozitif sonuçları neden ve gerçek trafiği engelleyin. Bu nedenle, uygulama ağ geçidi kural gruplarının ve kuralların özelleştirme yeteneği sağlar. Belirli bir kural gruplarının ve kuralların hakkında daha fazla bilgi için bkz. [web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Uygulama ağ geçidi WAF katmanının kullanmıyorsa, application gateway WAF katmanına yükseltme seçeneğini sağ bölmede görünür. 

![WAF etkinleştir][fig1]

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplarının ve kuralların

**Kural gruplarının ve kuralların görüntülemek için**
   1. Uygulama ağ geçidine gidin ve ardından **Web uygulaması güvenlik duvarı**.  
   2. Seçin **Gelişmiş kural Yapılandırması**.  
   Bu görünüm sayfasında seçtiğiniz kural kümesi ile sağlanan tüm kural gruplarının bir tablo gösterir. Kuralın onay kutularının tümünü seçilir.

![Devre dışı kurallarını yapılandırma][1]

## <a name="search-for-rules-to-disable"></a>Arama kuralları devre dışı bırakmak için

**Web uygulaması güvenlik duvarı ayarları** sayfası aracılığıyla metin arama kurallarını filtreleme özelliği sağlar. Sonucu, yalnızca kural gruplarının ve aradığınız metni içeren kuralları gösterir.

![Arama kuralları][2]

## <a name="disable-rule-groups-and-rules"></a>Kural gruplarının ve kuralların devre dışı bırak

> [!IMPORTANT]
> Herhangi bir kural gruplarını veya kuralları devre dışı bırakılırken dikkatli olun. Bunun için daha yüksek güvenlik riskleri ortaya çıkarabilir.

Kuralları devre dışı bırakmak, tüm kural grubu ya da bir veya daha fazla kural grubu altında belirli kuralları devre dışı bırakabilirsiniz. 

**Kural gruplarının veya belirli kuralları devre dışı bırakmak için**

   1. Kuralları veya devre dışı bırakmak istediğiniz kuralı grupları arayın.
   2. Devre dışı bırakmak istediğiniz kuralları için onay kutularını temizleyin. 
   2. **Kaydet**’i seçin. 

![Değişiklikleri Kaydet][3]

## <a name="mandatory-rules"></a>Zorunlu kuralları

Aşağıdaki liste, istek önleme modunda engellemek WAF neden koşulları içerir. Algılama modunda, özel durumlar olarak oturum açmadıysanız.

Bu yapılandırılmış veya devreden çıkarılamaz:

* Gövde İnceleme açık sürece devre dışı (XML, JSON, form verileri) istek gövdesi ayrıştırılamadı. hata engellenme, istekte sonuçlanır.
* İstek gövdesi (dosya ile) veri uzunluğu yapılandırılan sınırdan daha büyük:
* İstek gövdesi (dosyaları dahil) sınırdan büyük
* WAF altyapısında bir iç hata oluştu

CRS 3.x özel:

* Gelen anomali puanı aşıldı eşiği

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
