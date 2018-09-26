---
title: Otomatik ölçeklendirme ve bölgesel olarak yedekli Application Gateway'i Azure (genel Önizleme) | Microsoft Docs
description: Bu makalede, web uygulaması güvenlik duvarı istek boyutu sınırları ve Azure portal ile Application Gateway'de hariç tutma listeleri hakkında bilgiler sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: victorh
ms.openlocfilehash: ab1c9405042de02183b8742fa940a3a5a482923a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165239"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Otomatik ölçeklendirme ve bölgesel olarak yedekli bir uygulama ağ geçidi (genel Önizleme)

Uygulama ağ geçidi ve Web uygulaması Güvenlik Duvarı (WAF) artık performans geliştirmeleri sunar ve otomatik ölçeklendirme, bölge artıklığı ve statik VIP'ler için destek gibi önemli yeni özellikleri için destek ekleyen yeni bir SKU'ya altında genel önizlemede kullanıma sunuldu. Mevcut özellikleri genel kullanıma sunulan SKU altında yeni bir SKU'da ilgili bilinen kısıtlamaların bölümünde listelenen birkaç özel durum desteklenmeye devam edilir. Yeni SKU'lara aşağıdaki geliştirmeler şunları içerir:

- **Otomatik ölçeklendirme**: uygulama ağ geçidi veya WAF dağıtımlar altında SKU ölçeklendirme yapabilen yukarı veya aşağı trafiği değişen tabanlı otomatik ölçeklendirme desenleri yükleme. Otomatik ölçeklendirme, ayrıca bir dağıtım boyutunu veya örnek sayısının sağlama sırasında seçin gereksinimini ortadan kaldırır. Bu nedenle, SKU true esneklik sunar. Yeni bir SKU'da Application Gateway, hem sabit kapasite (otomatik ölçeklendirmeyi devre dışı) yanı sıra otomatik ölçeklendirme etkin modda çalışabilir. Sabit kapasite modu, tutarlı ve öngörülebilir iş yüklerine sahip senaryolar için kullanışlıdır. Otomatik ölçeklendirme modu, çok sayıda uygulama trafiği varyans bkz uygulamalarda yararlıdır.
   
   > [!NOTE]
   > WAF SKU'su için otomatik ölçeklendirme şu anda kullanılamıyor. Otomatik ölçeklendirme modu yerine sabit kapasite moduyla WAF yapılandırın.
- **Bölge yedekliliği**: bir uygulama ağ geçidi veya WAF dağıtım, birden çok kullanılabilirlik alanları, sağlamak ve bir Traffic Manager ile her bölgedeki ayrı bir Application Gateway örneğinden döndürme gereğini ortadan kaldıran yayılabilir. Tek bir bölge veya uygulama ağ geçidi örneklerinin dağıtıldığı birden çok bölge böylece kalmasını sağlama bölge hata dayanıklılığı seçebilirsiniz. Uygulamalar için arka uç havuzu kullanılabilirlik alanları genelinde benzer şekilde dağıtılabilir.
- **Performans iyileştirmeleri**: 5 X daha iyi, SSL yük boşaltma performans SKU genel kullanıma göre otomatik ölçeklendirme SKU kadar sunar.
- **Daha hızlı dağıtım ve güncelleştirme zamanı** genel olarak kullanılabilen SKU karşılaştırıldığında daha hızlı dağıtım ve güncelleştirme zamanı SKU otomatik ölçeklendirme sağlar.
- **Statik VIP**: VIP application gateway artık statik VIP türü özel olarak desteklemektedir. Bu, uygulama ağ geçidiyle ilişkili VIP yeniden başlatmadan sonra bile değişmez sağlar.

> [!IMPORTANT]
> Otomatik ölçeklendirme ve bölgesel olarak yedekli bir uygulama ağ geçidi SKU'su, şu anda genel Önizleme aşamasındadır. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Belirli özellikler desteklenmiyor olabilir ya da kısıtlı yeteneklere sahip olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Desteklenen bölgeler
Otomatik ölçeklendirme SKU, Doğu ABD 2, Orta ABD, Batı abd2, Fransa Orta, Batı Avrupa ve Güneydoğu Asya.

## <a name="pricing"></a>Fiyatlandırma
Önizleme süresince ücretsizdir. Key Vault, sanal makineler, vb. gibi uygulama ağ geçidi dışındaki kaynaklar için faturalandırılırsınız. 

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

|Sorun|Ayrıntılar|
|--|--|
|Faturalandırma|Şu anda hiçbir ödeme yoktur.|
|FIPS modundayken, WebSocket|Bunlar şu anda desteklenmemektedir.|
|ILB yalnızca modu|Bu şu anda desteklenmiyor. Genel ve ILB modu birlikte desteklenir.|
|Web uygulaması Güvenlik Duvarı'nı otomatik ölçeklendirme|WAF, otomatik ölçeklendirme modu desteklemez. Sabit kapasite modu desteklenir.|

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak bir ayrılmış sanal IP adresiyle bir otomatik ölçeklendirme, bölge yedekli uygulama ağ geçidi oluşturma](tutorial-autoscale-ps.md)
- Daha fazla bilgi edinin [Application Gateway](overview.md).
- Daha fazla bilgi edinin [Azure Güvenlik Duvarı](../firewall/overview.md). 

