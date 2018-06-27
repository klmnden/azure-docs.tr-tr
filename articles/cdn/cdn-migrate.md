---
title: Bir Azure CDN profili Verizon standart Verizon Premium geçirme | Microsoft Docs
description: Bir profili Verizon standart Verizon Premium geçiş işleminin ayrıntıları hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: 074dbb094e7ae2cd1f1719016928bd7348da3949
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958064"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Bir Azure CDN profili standart Verizon Premium Verizon geçirme

Azure CDN uç noktalarınızı yönetmek için bir Azure içerik teslim ağı (CDN) profili oluşturduğunuzda, dört farklı ürünler, içinden seçim yapabileceğiniz için sunar. Farklı ürün ve bunların kullanılabilir özellikleri hakkında daha fazla bilgi için bkz: [karşılaştırmak Azure CDN ürün özellikleri](cdn-features.md).

Olduğunuz oluşturursanız, bir **verizon'dan Azure CDN standart** profil ve CDN uç noktalarınızı yönetmek için kullandığınız kendisine yükseltme seçeneğine sahip bir **verizon'dan Azure CDN Premium** profili. Yükselttiğinizde, CDN uç noktalarınızı ve tüm verileriniz korunur. 

> [!IMPORTANT]
> Yükseltme yaparsam sonra bir **verizon'dan Azure CDN Premium** profili, daha sonra dönüştüremiyor onu geri bir **verizon'dan Azure CDN standart** profili.
> 

Yükseltmek için bir **verizon'dan Azure CDN standart** profili, kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Profil karşılaştırma
**Verizon'dan Azure CDN Premium** profilleri sahip aşağıdaki anahtar farkları **verizon'dan Azure CDN standart** profilleri:
- Azure CDN özellikleri gibi belirli [sıkıştırma](cdn-improve-performance.md), [kuralları önbelleğe alma](cdn-caching-rules.md), ve [coğrafi filtreleme](cdn-restrict-access-by-country.md)Azure CDN arabirimi kullanamazsınız, Verizon portalı yoluyla kullanmanız gerekir **Yönet** düğmesi.
- API: Aksine standart Verizon ile API Premium Verizon Portalı'ndan erişilen bu özellikleri denetlemek için kullanamazsınız. Ancak, bir uç nokta oluşturma/silme, önbelleğe alınan varlıklar temizleme/yükleme ve etkinleştirme/özel bir etki alanı devre dışı bırakma gibi diğer ortak özellikleri denetlemek için API'yi kullanabilirsiniz.
- Fiyatlandırma: Premium Verizon standart Verizon'den veri aktarımları için farklı bir fiyatlandırma yapısına vardır. Daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/pricing/details/cdn/).

**Verizon'dan Azure CDN Premium** profilleri aşağıdaki ek özellikleri vardır:
- [Belirteç kimlik doğrulama](cdn-token-auth.md): kullanıcıların elde edilir ve güvenli kaynaklar getirmek için bir belirteç olanak sağlar.
- [Kurallar altyapısı](cdn-rules-engine.md): HTTP isteklerinin işlenme özelleştirmenize olanak tanır.
- Gelişmiş analiz araçları:
   - [Ayrıntılı HTTP analizi](cdn-advanced-http-reports.md)
   - [Edge Performans Analizi](cdn-edge-performance.md)
   - [Gerçek zamanlı analiz](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Sonraki adımlar
Kurallar altyapısı hakkında daha fazla bilgi için bkz: [Azure CDN kurallar altyapısı başvuru](cdn-rules-engine-reference.md).

