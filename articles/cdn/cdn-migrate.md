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
ms.openlocfilehash: 615f35c602f9a086bdc5c32b04a97efa368411a3
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308134"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Bir Azure CDN profili standart Verizon Premium Verizon geçirme

Her biri farklı özellikler sunar azure içerik teslim ağı (CDN) teklifleri dört farklı ürünleri: 
- **Microsoft Azure CDN standart**
- **Akamai'den Azure CDN standart**
- **Verizon'dan Azure CDN standart**
- **Verizon'dan Azure CDN Premium**.

Kullanıyorsanız, bir **verizon'dan Azure CDN standart** profili CDN uç noktalarınızı yönetmek için kendisine yükseltme seçeneğine sahip bir **verizon'dan Azure CDN Premium** profil ve tüm verilerinizi koruyun.

Yükseltmek için bir **verizon'dan Azure CDN standart** profili, kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

> [!IMPORTANT]
> Yükseltme yaparsam sonra bir **verizon'dan Azure CDN Premium** profili, daha sonra dönüştüremiyor onu geri bir **verizon'dan Azure CDN standart** profili.
> 

## <a name="profile-comparison"></a>Profil karşılaştırma
**Verizon'dan Azure CDN Premium** profilleri sahip aşağıdaki anahtar farkları **verizon'dan Azure CDN standart** profilleri:
- Azure CDN özellikleri gibi belirli [sıkıştırma](cdn-improve-performance.md), kurallar, önbelleğe alma ve [coğrafi filtreleme](cdn-restrict-access-by-country.md), Azure CDN arabirimi kullanamazsınız, Verizon portalı yoluyla kullanmalısınız **Yönet**düğmesi.
- API: Standart Verizon Premium Verizon portalı üzerinden erişilen bu özellikleri denetlemek için API kullanamazsınız. Ancak, bir uç nokta oluşturma/silme, önbelleğe alınan varlıklar temizleme/yükleme ve etkinleştirme/özel bir etki alanı devre dışı bırakma gibi diğer ortak özellikleri denetlemek için API'yi kullanabilirsiniz.
- Fiyatlandırma: Premium Verizon standart Verizon'den veri aktarımları için farklı bir fiyatlandırma yapısına vardır. Daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/pricing/details/cdn/).

**Verizon'dan Azure CDN Premium** profilleri aşağıdaki ek özellikleri vardır:
- [Belirteç kimlik doğrulama](cdn-token-auth.md): kullanıcıların elde edilir ve güvenli kaynaklar getirmek için bir belirteç olanak sağlar.
- [Kurallar altyapısı](cdn-rules-engine.md): HTTP isteklerinin işlenme özelleştirmenize olanak tanır.
- Gelişmiş analiz araçları:
   - [Ayrıntılı HTTP analizi](cdn-advanced-http-reports.md)
   - [Edge Performans Analizi](cdn-edge-performance.md)
   - [Gerçek zamanlı analiz](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Sonraki adımlar
Azure CDN ürünü özelliklerin ayrıntılı karşılaştırması için bkz: [Azure CDN ürün özellikleri](Compare Azure CDN product features).

Kurallar altyapısı hakkında daha fazla bilgi için bkz: [Azure CDN kurallar altyapısı başvuru](cdn-rules-engine-reference.md).

