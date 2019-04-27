---
title: Azure CDN profili Verizon standart Verizon Premium sürümüne geçirme | Microsoft Docs
description: Bir profili Verizon standart Verizon Premium'a geçiş ayrıntıları hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 7768dde424aedc295b53512db50c9dfc9db9ab8c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635644"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Azure CDN profili için Premium Verizon standart Verizon geçirme

Uç noktalarınızı yönetmek için bir Azure Content Delivery Network (CDN) profili oluşturduğunuzda, Azure CDN, içinden seçim yapabileceğiniz için dört farklı ürün sunar. Çeşitli ürünlerin ve bunların kullanılabilir özellikleri hakkında daha fazla bilgi için bkz. [karşılaştırma Azure CDN ürün özellikleri](cdn-features.md).

Olduğunuz oluşturursanız, bir **verizon'dan Azure CDN standart** profili ve CDN uç noktalarınızı yönetmek için kullandığınız için yükseltme seçeneğine sahip bir **verizon'dan Azure CDN Premium** profili. Yükselttiğinizde, CDN uç noktalarınızı ve tüm verileriniz korunur. 

> [!IMPORTANT]
> İçin yükselttikten sonra bir **verizon'dan Azure CDN Premium** profili, daha sonra dönüştürülemiyor, başa bir **verizon'dan Azure CDN standart** profili.
> 

Yükseltmek için bir **verizon'dan Azure CDN standart** profili, kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Profil karşılaştırma
**Verizon'dan Azure CDN Premium** profilleri, aşağıdaki anahtar fark sahip **verizon'dan Azure CDN standart** profilleri:
- Azure CDN özellikleri gibi belirli [sıkıştırma](cdn-improve-performance.md), [önbelleğe alma kuralları](cdn-caching-rules.md), ve [coğrafi filtreleme](cdn-restrict-access-by-country.md), Azure CDN arabirimini kullanamazsınız, Verizon portalı üzerinden kullanmanız gerekir **Yönet** düğmesi.
- API: Aksine standart Verizon ile API Premium Verizon portaldan erişilen bu özellikleri denetlemek için kullanamazsınız. Ancak, bir uç nokta oluşturma/silme, önbelleğe alınan varlıkları temizleme/yükleme ve etkinleştirme/özel bir etki alanını devre dışı bırakma gibi diğer ortak özellikleri denetlemek için API'yi kullanabilirsiniz.
- Fiyatlandırma: Premium Verizon standart Verizon daha veri aktarımları için farklı bir fiyatlandırma yapısına sahiptir. Daha fazla bilgi için [Content Delivery Network fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

**Verizon'dan Azure CDN Premium** profilleri şu ek özellikleri vardır:
- [Belirteç kimlik doğrulaması](cdn-token-auth.md): Güvenli kaynaklara getirmek için bir belirteç elde edilir ve kullanıcıların sağlar.
- [Kural altyapısı](cdn-rules-engine.md): HTTP isteklerinin nasıl işleneceğini özelleştirmenize olanak sağlar.
- Gelişmiş analiz araçları:
   - [Ayrıntılı HTTP analizi](cdn-advanced-http-reports.md)
   - [Uç Performans Analizi](cdn-edge-performance.md)
   - [Gerçek zamanlı analiz](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Sonraki adımlar
Kural altyapısı hakkında daha fazla bilgi için bkz: [Azure CDN kural altyapısı başvurusu](cdn-rules-engine-reference.md).

