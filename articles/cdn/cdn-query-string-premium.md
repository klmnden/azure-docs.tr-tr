---
title: "Denetim Azure önbelleğe alma davranışını sorgu dizeleriyle - Premium CDN | Microsoft Docs"
description: "Azure CDN sorgu dizesini önbelleğe alma dosyaları nasıl sorgu dizeleri içerdiğinde önbelleğe alınacağını kontrol eder."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - Premium
> [!div class="op_single_selector"]
> * [Standart](cdn-query-string.md)
> * [Verizon'dan Azure CDN Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Sorgu dizesini dosyaları nasıl sorgu dizeleri içerdiğinde önbelleğe alınacağını denetimleri önbelleğe alma.

> [!IMPORTANT]
> Standart ve Premium CDN ürünleri için önbelleğe alma işlevinin aynı sorgu dizesi sağlasa da, kullanıcı arabirimi farklıdır.  Arabirim için bu belgede açıklanan **verizon'dan Azure CDN Premium**.  İle sorgu dizesi önbelleğe alma için **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**, bkz: [CDN önbelleğe alma davranışını denetleme, sorgu dizeleri içeren istekleri](cdn-query-string.md).
> 
> 

Üç modu kullanılabilir:

* **Standart önbellek**: Bu varsayılan moddur.  CDN kenar düğümüne sorgu dizesi istek sahibi kaynağa ilk istek ve önbellek varlık geçer.  Önbelleğe alınan varlık süresi doluncaya kadar kenar düğümden sunulan tüm istekler bu varlık için sorgu dizesi göz ardı eder.
* **Hayır-cache**: Bu modda, CDN kenar düğümüne sorgu dizeleri içeren istekleri önbelleğe alınmaz.  Kenar düğümüne doğrudan kaynaktan varlığı alır ve bunu her istek ile istek sahibi geçirir.
* **Önbellek benzersiz**: Bu mod, kendi önbelleği ile benzersiz bir varlık olarak bir sorgu dizesi her bir istekle değerlendirir.  Örneğin, bir istek için başlangıç yanıttan *foo.ashx?q=bar* o aynı sorgu dizesi ile sonraki önbellekler için döndürülen ve kenar düğümüne önbelleğe.  Bir istek için *foo.ashx?q=somethingelse* canlı bir ayrı varlık kendi süresiyle önbelleğe alınması.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Sorgu dizesini önbelleğe alma premium CDN profili ayarlarını değiştirme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **HTTP büyük** sekmesini ve ardından üzerine gelerek **önbellek ayarları** çıkma.  Tıklayın **sorgu dizesi önbelleğe alma**.
   
    Sorgu dizesini önbelleğe alma seçenekleri görüntülenir.
   
    ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string-premium/cdn-query-string.png)
3. Seçiminizi yaptıktan sonra tıklatın **güncelleştirme** düğmesi.

> [!IMPORTANT]
> Kaydın yayılması zaman yararlanırken ayar değişiklikleri hemen görünür olmayabilir.  <b>Verizon'dan Azure CDN</b> profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.
> 
> 

