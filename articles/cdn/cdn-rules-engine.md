---
title: "Azure CDN kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma | Microsoft Docs"
description: "Kurallar altyapısı, belirli türde bir içerik teslim engelleme gibi HTTP isteklerini Azure CDN tarafından nasıl işleneceğini özelleştirme, önbellek ilkesi tanımlayın ve HTTP üstbilgileri değiştirmenize olanak sağlar."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Kurallar altyapısı HTTP isteklerini, belirli türde bir içerik teslim engelleme, önbellek ilkesi tanımlama ve HTTP üstbilgileri değiştirme gibi işlenme özelleştirmenizi sağlar.  Bu öğretici bir kural oluşturma CDN varlıklar önbelleğe alma davranışını değiştirir gösterilmektedir.  Ayrıca video içeriği bulunmamaktadır kullanılabilir "[Ayrıca bkz.](#see-also)" bölümü.

   > [!TIP] 
   > Ayrıntılı sözdizimi başvuru için bkz: [kuralları altyapısı başvurusu](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Öğretici
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Tıklayın **HTTP büyük** sekmesini ve ardından, **kurallar altyapısı**.
   
    Yeni bir kural için seçenekler görüntülenir.
   
    ![CDN yeni kural seçenekleri](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Birden çok kural listelenmiş görevlerin sırası nasıl işlendiğini etkiler. Bir sonraki kural önceki bir kural tarafından belirtilen eylemleri geçersiz kılabilir.
   > 
   > 
3. Bir ad girin **adı / açıklaması** metin kutusu.
4. Kuralın uygulanacağı istekleri türünü tanımlayın.  Varsayılan olarak, **her zaman** eşleşme koşul seçilidir.  Kullanacağınız **her zaman** Bu öğretici için bu nedenle, seçili bırakın.
   
   ![CDN eşleşme koşulu](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Açılır listede kullanılabilen koşullar türlerde eşleşme vardır.  Eşleşme koşul solundaki mavi bilgi simgesine tıklayarak şu anda seçili koşul ayrıntılı olarak anlatılmıştır.
   > 
   >  Ayrıntılı koşullu ifadeler tam listesi için bkz: [kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Eşleşme koşullar ayrıntılı tam listesi için bkz [kurallar altyapısı eşleşen koşullar](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Tıklatın  **+**  düğmesine **özellikleri** yeni bir özellik eklemek için.  Sol taraftaki açılır menüde seçin **zorla iç Max-Age**.  Görüntülenen metin kutusuna girin **300**.  Geri kalan varsayılan değerler bırakın.
   
   ![CDN özelliği](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Olarak eşleşme koşullarla yeni özellik solundaki mavi bilgi simgesine tıklayarak bu özellik hakkındaki ayrıntıları görüntüler.  Durumunda **zorla iç Max-Age**, biz varlığın geçersiz kılma **Cache-Control** ve **Expires** CDN kenar düğümüne kaynaktan varlık zaman yenilenecek denetlemek için üstbilgiler.  Bizim örneğimizde, 300 saniye CDN kenar düğümüne varlık kendi kaynaktan varlık yenileme önce 5 dakika için önbelleğe anlamına gelir.
   > 
   > Ayrıntılı özelliklerin tam listesi için bkz: [kurallar altyapısı özellik ayrıntıları](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Tıklatın **Ekle** yeni kuralını kaydetmek için düğmesi.  Yeni Kural artık onayını bekliyor. Bunu onaylandıktan sonra durumu değiştirilecek **bekleyen XML** için **etkin XML**.
   
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
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Azure Cuma: Azure CDN's güçlü yeni Premium özellikleri](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)