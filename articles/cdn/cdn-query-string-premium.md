---
title: Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı denetlemek | Microsoft Docs
description: Azure CDN sorgu dizesini önbelleğe alma denetimleri web isteğine bir sorgu dizesi içerdiğinde nasıl dosyaları önbelleğe alınır. Bu makalede Azure CDN Premium Verizon üründen önbelleğe almayı sorgu dizesi.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mazha
ms.openlocfilehash: a1ef5f3e502b5383343dbec2c427d36054a3edc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Azure içerik teslim ağı (CDN), dosyaları bir sorgu dizesi içeren bir web isteği için nasıl önbelleğe kontrol edebilirsiniz. Sorgu dizesi olan bir web isteğinde sorgu dizesi, bir soru işareti (?) sonra oluşan istek bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır bir veya daha fazla anahtar-değer çiftleri içerebilir. Her anahtar-değer çifti ampersan tarafından ayrılmış (&). Örneğin, http:\//www.contoso.com/content.mov?field1=value1 & alan2 Value2. Bir isteğin sorgu dizesi içinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir. 

> [!IMPORTANT]
> Standart ve premium CDN ürünü için aynı sorgu dizesini önbelleğe alma işlevinin sağlasa da, kullanıcı arabirimi farklıdır. Bu makalede arabirim için **verizon'dan Azure CDN Premium**. Sorgu dizesi için bkz: Azure CDN standart ürünleriyle önbellekleme [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katmanı](cdn-query-string.md).
>


Üç sorgu dizesi modu kullanılabilir:

- **Standart önbellek**: varsayılan mod. Bu modda, CDN bulunma noktası (POP) düğüm sorgu dizeleri istek için kaynak sunucu üzerinde yapılan ilk istek geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar POP sunucusundan sunulan tüm sonraki istekleri varlığı için sorgu dizelerini yoksayabilir.

    >[!IMPORTANT] 
    > Bu hesaptaki herhangi bir yol için belirteci yetkilendirme etkinleştirilirse, standart Önbellek modu kullanılabilecek yalnızca modudur. 

- **Hayır-cache**: Bu modda, CDN POP düğümde sorgu dizeleri içeren istekleri önbelleğe alınmaz. POP düğüm doğrudan kaynak sunucudan varlığı alır ve her istek ile istek sahibi geçirir.

- **Önbellek benzersiz**: Bu modda, sorgu dizesi dahil olmak üzere benzersiz bir URL'ye sahip her isteği kendi önbelleği ile benzersiz bir varlık olarak kabul edilir. Örneğin, example.ashx?q=test1 için bir istek için kaynak sunucudan yanıt POP düğümde önbelleğe ve sonraki önbellekleri ile aynı sorgu dizesi döndürdü. Bir istek example.ashx?q=test2 için kendi yaşam süresi ayarı ile ayrı bir varlık olarak önbelleğe alınır.
   
    >[!IMPORTANT] 
    > Düşük bir önbellek isabet oranı neden sorgu dizesi bir oturum kimliği veya bir kullanıcı adı gibi her istek ile değiştirilecek parametreleri içerdiğinde, bu mod kullanmayın.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Sorgu dizesini önbelleğe alma premium CDN profili ayarlarını değiştirme
1. CDN profili açın ve ardından **Yönet**.
   
    ![CDN profili Yönet düğmesi](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **HTTP büyük** sekmesini ve ardından üzerine gelerek **önbellek ayarları** açılır menüsü. Tıklatın **sorgu dizesi önbelleğe alma**.
   
    Sorgu dizesini önbelleğe alma seçenekleri görüntülenir.
   
    ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string-premium/cdn-query-string.png)
3. Bir sorgu dizesi modunu seçin ve ardından **güncelleştirme**.

> [!IMPORTANT]
> Kaydın yayılması zaman alır çünkü önbelleği dize ayarları değişiklikleri hemen görünür olmayabilir. İçin **verizon'dan Azure CDN Premium** profilleri yayma işlemi genellikle 90 dakika içinde tamamlanır.
 

