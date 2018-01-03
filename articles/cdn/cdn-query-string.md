---
title: "Azure içerik teslim ağı önbelleğe alma davranışını sorgu dizeleriyle denetleme | Microsoft Docs"
description: "Azure CDN sorgu dizesini önbelleğe alma denetimleri sorgu dizeleri içerdiğinde nasıl dosyaları önbelleğe alınır."
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
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 9ffd05a0eb4d976dc40a1c5d45fd22ebf9bd4db1
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Denetim Azure içerik teslim ağı önbelleğe alma davranışını sorgu dizeleriyle etkinleştirmek
> [!div class="op_single_selector"]
> * [Standart](cdn-query-string.md)
> * [Verizon'dan Azure CDN Premium](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Azure içerik teslim ağı (CDN), dosyaları bir sorgu dizesi içeren bir web isteği için nasıl önbelleğe kontrol edebilirsiniz. Sorgu dizesi olan bir web isteğinde sorgu dizesi, bir soru işareti (?) sonra oluşan istek bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır bir veya daha fazla anahtar-değer çiftleri içerebilir. Her anahtar-değer çifti ampersan tarafından ayrılmış (&). Örneğin, `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Bir isteğin sorgu dizesi içinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir. 

> [!IMPORTANT]
> Standart ve premium CDN ürünü için aynı sorgu dizesini önbelleğe alma işlevinin sağlasa da, kullanıcı arabirimi farklıdır.  Bu makalede arabirim için **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**. İle sorgu dizesi önbelleğe alma için **verizon'dan Azure CDN Premium**, bkz: [CDN önbelleğe alma davranışını denetleme istekleri sorgu dizeleriyle - Premium](cdn-query-string-premium.md).

Üç sorgu dizesi modu kullanılabilir:

- **Sorgu dizelerini yoksayabilir**: varsayılan mod. Bu modda, CDN kenar düğümüne sorgu dizeleri istek sahibi kaynağa yapılan ilk istek geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar kenar düğümden sunulan tüm sonraki istekleri varlığı için sorgu dizelerini yoksayabilir.
- **Sorgu dizeleri için önbelleğe almayı atla**: Bu modda, CDN kenar düğümüne sorgu dizeleri içeren istekleri önbelleğe alınmaz. Kenar düğümüne doğrudan kaynaktan varlığı alır ve bunu her istek ile istek sahibi geçirir.
- **Her benzersiz URL'yi önbelleğe**: Bu modda, sorgu dizesi dahil olmak üzere benzersiz bir URL'ye sahip her isteği kendi önbelleği ile benzersiz bir varlık olarak kabul edilir. Örneğin, bir istek için başlangıç yanıttan `example.ashx?q=test1` kenar düğümüne önbelleğe ve sonraki önbellekleri ile aynı sorgu dizesi döndürdü. Bir istek için `example.ashx?q=test2` ayrı bir varlık kendi yaşam süresi ayarı ile önbelleğe alınır.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Sorgu dizesini önbelleğe alma standart CDN profili ayarlarını değiştirme
1. CDN profili açın ve sonra yönetmek istediğiniz CDN uç noktası seçin.
   
   ![CDN profili uç noktaları](./media/cdn-query-string/cdn-endpoints.png)
   
2. Sol bölmede ayarları altında tıklatın **kuralları önbelleğe alma**.
   
    ![Kuralları düğmesini CDN önbelleğe alma](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. İçinde **sorgu dizesini önbelleğe alma davranışı** listesinde, bir sorgu dizesi modunu seçin ve ardından **kaydetmek**.
   
   ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Kaydın yayılması zaman alır çünkü önbelleği dize ayarları değişiklikleri hemen görünür olmayabilir. **Akamai'den Azure CDN** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. **Verizon'dan Azure CDN** profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.


