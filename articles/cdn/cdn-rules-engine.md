---
title: "Azure CDN kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma | Microsoft Docs"
description: "Kurallar altyapısı, belirli türde bir içerik teslim engelleme gibi HTTP isteklerini Azure CDN tarafından nasıl işleneceğini özelleştirme, önbellek ilkesi tanımlayın ve HTTP üstbilgileri değiştirmenize olanak sağlar."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: mazha
ms.openlocfilehash: fe3df703f7eb244a52756c4d015e9ea598224ce1
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Azure CDN kurallar altyapısı HTTP isteklerinin işlenme özelleştirmenizi sağlar. Örneğin, bir önbellek ilkesi tanımlama veya bir HTTP üstbilgisi değiştirirken belirli içerik türlerini teslimini engelliyor. Bu öğretici CDN varlıklar önbelleğe alma davranışını değiştiren bir kuralın nasıl oluşturulacağı gösterilmektedir. Kurallar altyapısı sözdizimi hakkında daha fazla bilgi için bkz: [Azure CDN kurallar altyapısı başvuru](cdn-rules-engine-reference.md).

## <a name="access"></a>Access
Kurallar altyapısı erişmek için önce seçmelisiniz **Yönet** üstünden **CDN profili** Azure CDN Yönetim sayfasına erişmek için sayfa. Uç noktanız için dinamik site ivmesini (DSA) olup olmadığını getirilmiştir bağlı olarak kuralları, uç nokta türü için uygun kuralları altyapısıyla sonra erişim:

- Genel web teslim veya diğer DSA olmayan iyileştirme için en iyi duruma getirilmiş uç noktalar: 
    
    Seçin **HTTP büyük** sekmesini ve ardından **kurallar altyapısı**.

    ![HTTP için kurallar altyapısı](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- DSA için en iyi duruma getirilmiş uç noktalar: 
    
    Seçin **ADN** sekmesini ve ardından **kurallar altyapısı**. 
    
    ADN tarafından Verizon DSA içeriği belirtmek için kullanılan bir terimdir. Burada oluşturduğunuz herhangi bir kuralın, DSA için optimize edilmemiş tüm uç noktaları profilinizde göz ardı edilir. 

    ![DSA için kurallar altyapısı](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Öğretici
1. Gelen **CDN profili** sayfasında, **Yönet**.
   
    ![CDN profili Yönet düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Seçin **HTTP büyük** sekmesini ve ardından **kurallar altyapısı**.
   
    Yeni bir kural için seçenekler görüntülenir.
   
    ![CDN yeni kural seçenekleri](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Birden çok kural listelenmiş görevlerin sırası nasıl işlendiğini etkiler. Bir sonraki kural önceki bir kural tarafından belirtilen eylemleri geçersiz kılabilir.
   > 
3. Bir ad girin **adı / açıklaması** metin kutusu.
4. Kuralın uygulanacağı istekleri türünü tanımlayın. Varsayılan eşleşme koşulunu kullanmak **her zaman**. 
   
   ![CDN kural eşleşen koşulu](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Açılır listede birden çok eşleme koşulları kullanılabilir. Şu anda seçili eşleşme koşulu hakkında daha fazla bilgi için solunda mavi Bilgi simgesini seçin.
   > 
   >  Koşullu deyimler ayrıntılı bir listesi için bkz [kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Eşleşme koşullar ayrıntılı bir listesi için bkz [kurallar altyapısı eşleşme koşullar](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
1. Yeni bir özellik eklemek için seçin  **+**  düğmesine **özellikleri**.  Sol taraftaki açılır menüde seçin **zorla iç Max-Age**.  Görüntülenen metin kutusuna girin **300**. Geri kalan varsayılan değerler değiştirmeyin.
   
   ![CDN kural özelliği](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Açılır listede birden çok özellikleri kullanılabilir. Şu anda seçili özelliği hakkında daha fazla bilgi için solunda mavi Bilgi simgesini seçin. 
   >
   > İçin **zorla iç Max-Age**, varlığın `Cache-Control` ve `Expires` üstbilgileri zaman CDN kenar düğümüne kaynaktan varlık yeniler denetlemek için geçersiz kılınır. Bu örnekte, 300 saniye veya 5 kendi kaynaktan varlık yeniler önce dakika varlık CDN kenar düğümüne önbelleğe alır.
   > 
   > Özelliklerin ayrıntılı bir listesi için bkz: [kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md).
   > 
   > 
1. Tıklatın **Ekle** yeni kuralını kaydetmek için düğmesi.  Yeni Kural artık onayını bekliyor. Bunu onaylandıktan sonra durum değişiklikleri **bekleyen XML** için **etkin XML**.
   
   > [!IMPORTANT]
   > Kuralları değişiklikleri yayılması 90 dakika sürebilir.
   > 
   > 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Kuralları altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kurallar altyapısı eşleşme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Azure Cuma: Azure CDN'ın güçlü yeni premium özellikleri](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)