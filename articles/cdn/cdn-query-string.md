---
title: "Azure CDN önbelleğe alma davranışını sorgu dizeleriyle denetleme | Microsoft Docs"
description: "Azure CDN sorgu dizesini önbelleğe alma dosyaları nasıl sorgu dizeleri içerdiğinde önbelleğe alınacağını kontrol eder."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle etkinleştirmek
> [!div class="op_single_selector"]
> * [Standart](cdn-query-string.md)
> * [Verizon'dan Azure CDN Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Sorgu dizesini dosyaları nasıl sorgu dizeleri içerdiğinde önbelleğe alınacağını denetimleri önbelleğe alma.

> [!IMPORTANT]
> Standart ve Premium CDN ürünleri için önbelleğe alma işlevinin aynı sorgu dizesi sağlasa da, kullanıcı arabirimi farklıdır.  Arabirim için bu belgede açıklanan **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**.  İle sorgu dizesi önbelleğe alma için **verizon'dan Azure CDN Premium**, bkz: [CDN önbelleğe alma davranışını denetleme istekleri sorgu dizeleriyle - Premium](cdn-query-string-premium.md).
> 
> 

Üç modu kullanılabilir:

* **Sorgu dizelerini yoksayabilir**: Bu varsayılan moddur.  CDN kenar düğümüne sorgu dizesi istek sahibi kaynağa ilk istek ve önbellek varlık geçer.  Önbelleğe alınan varlık süresi doluncaya kadar kenar düğümden sunulan tüm istekler bu varlık için sorgu dizesi göz ardı eder.
* **Sorgu dizeleri içeren URL için önbelleğe almayı atla**: Bu modda, CDN kenar düğümüne sorgu dizeleri içeren istekleri önbelleğe alınmaz.  Kenar düğümüne doğrudan kaynaktan varlığı alır ve bunu her istek ile istek sahibi geçirir.
* **Her benzersiz URL'yi önbelleğe**: Bu mod, kendi önbelleği ile benzersiz bir varlık olarak bir sorgu dizesi her bir istekle değerlendirir.  Örneğin, bir istek için başlangıç yanıttan *foo.ashx?q=bar* o aynı sorgu dizesi ile sonraki önbellekler için döndürülen ve kenar düğümüne önbelleğe.  Bir istek için *foo.ashx?q=somethingelse* canlı bir ayrı varlık kendi süresiyle önbelleğe alınması.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Sorgu dizesini önbelleğe alma standart CDN profili ayarlarını değiştirme
1. CDN profili dikey penceresinden, yönetmek istediğiniz CDN uç noktası'ı tıklatın.
   
    ![CDN profili dikey uç noktaları](./media/cdn-query-string/cdn-endpoints.png)
   
    CDN uç noktası dikey pencere açılır.
2. Tıklatın **yapılandırma** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-query-string/cdn-config-btn.png)
   
    CDN yapılandırma dikey pencere açılır.
3. Bir ayar seçin **sorgu dizesini önbelleğe alma davranışı** açılır.
   
    ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string/cdn-query-string.png)
4. Seçiminizi yaptıktan sonra tıklatın **kaydetmek** düğmesi.

> [!IMPORTANT]
> Kaydın yayılması zaman yararlanırken ayar değişiklikleri hemen görünür olmayabilir.  <b>Akamai'den Azure CDN</b> profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır.  <b>Verizon'dan Azure CDN</b> profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.
> 
> 

