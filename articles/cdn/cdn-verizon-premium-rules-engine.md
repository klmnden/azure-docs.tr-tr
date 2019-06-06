---
title: Azure CDN from Verizon Premium kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma | Microsoft Docs
description: Kural altyapısı nasıl HTTP isteklerini Azure CDN from Verizon Premium tarafından belirli türlerdeki içerik teslimini engelleme gibi işleneme şeklini özelleştirmenizi, önbelleğe alma ilkesi tanımlama ve HTTP üst bilgilerini değiştirme sağlar.
services: cdn
author: mdgattuso
ms.service: cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 81af3073d64e4379972568a57907a7fb2f82356d
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481737"
---
# <a name="override-http-behavior-using-the-azure-cdn-from-verizon-premium-rules-engine"></a>Azure CDN from Verizon Premium kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Azure CDN kurallar altyapısı nasıl HTTP isteklerinin işleneme şeklini özelleştirmenizi sağlar. Örneğin, bir önbelleğe alma ilkesi tanımlama veya HTTP üstbilgisi değiştirme belirli içerik türlerini teslimini engelleme. Bu öğreticide, varlıklar CDN önbelleğe alma davranışını değiştiren bir kuralın nasıl oluşturulacağı gösterilmektedir. Kural altyapısı sözdizimi hakkında daha fazla bilgi için bkz. [Azure CDN kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md).

## <a name="access"></a>Access

Kural altyapısı erişmek için önce seçmelisiniz **Yönet** üstünden **CDN profili** Azure CDN Yönetim sayfasına erişmek için sayfa. Uç noktanız için dinamik site Hızlandırma (DSA) olup olmadığını iyileştirilmiştir bağlı olarak, uç nokta türü için uygun olan kurallar kümesini içeren kural altyapısı ardından erişin:

- Genel web teslimatı veya diğer DSA olmayan iyileştirme için en iyi duruma getirilmiş uç noktalar:
    
    Seçin **HTTP büyük** sekmesine ve ardından seçin **kurallar altyapısı**.

    ![HTTP için kurallar altyapısı](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- DSA için en iyi duruma getirilmiş uç noktalar:
    
    Seçin **ADN** sekmesine ve ardından seçin **kurallar altyapısı**.
    
    ADN Verizon tarafından DSA içeriği belirtmek için kullanılan bir terimdir. Burada oluşturduğunuz herhangi bir kural, DSA için optimize edilmemiş tüm uç noktalar profilinizde göz ardı edilir.

    ![Kural altyapısı DSA](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Öğretici

1. Gelen **CDN profili** sayfasında **Yönet**.
   
    ![CDN profili Yönet düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    CDN yönetim portalına açılır.

2. Seçin **HTTP büyük** sekmesine ve ardından seçin **kurallar altyapısı**.
   
    Yeni bir kural için seçenekler görüntülenir.
   
    ![CDN yeni kural seçenekleri](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Birden çok kural listelendiği sırayı nasıl işleneceğini etkiler. Bir sonraki kural, bir önceki kuralı tarafından belirtilen eylemleri geçersiz kılabilir.
   >

3. Bir ad girin **adı / açıklaması** metin.

4. Kuralın uygulanacağı istek türlerini tanımlayın. Varsayılan eşleşme koşulu kullanmak **her zaman**.
   
   ![CDN kural eşleşme koşulu](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Birden çok eşleşme koşulu, açılan listede kullanılabilir. Şu anda seçili eşleşme koşulu hakkında daha fazla bilgi için etiketin solunda mavi Bilgi simgesini seçin.
   >
   >  Koşullu ifadeler ayrıntılı bir listesi için bkz. [kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-match-conditions.md).
   >  
   > Eşleştirme koşulları ayrıntılı bir listesi için bkz. [kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md).
   >
   >

5. Yeni bir özellik eklemek için seçin **+** düğmesinin yanındaki **özellikleri**.  Sol taraftaki açılan listede seçin **zorla iç Max-Age**.  Görünen metin kutusuna girin **300**. Kalan varsayılan değerleri değiştirmeyin.
   
   ![CDN kural özelliği](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Birden çok özellik açılan listede kullanılabilir. Şu anda seçilen özelliği hakkında daha fazla bilgi için etiketin solunda mavi Bilgi simgesini seçin.
   >
   > İçin **zorla iç Max-Age**, varlığın `Cache-Control` ve `Expires` üstbilgileri, CDN kenar düğümüne kaynaktan bir varlık yeniler denetlemek için geçersiz kılınır. Bu örnekte, 300 saniye veya 5, kaynaktan bir varlık yenilenmeden önce dakika varlık CDN kenar düğümüne önbelleğe alır.
   >
   > Özelliklerin ayrıntılı listesi için bkz. [kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md).
   >
   >

6. Seçin **Ekle** yeni kuralını kaydetmek için.  Yeni Kural artık onayını bekliyor. Bunu onaylandıktan sonra durumu değişir **bekleyen XML** için **etkin XML**.
   
   > [!IMPORTANT]
   > Kuralları değişiklikleri Azure CDN'de yayılması 10 dakika sürebilir.
   >
   >

## <a name="see-also"></a>Ayrıca bkz.

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Azure Fridays: Azure CDN'ın güçlü yeni premium özellikleri](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)