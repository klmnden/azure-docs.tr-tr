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
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: ba9c28f0e6df25b101b45edf836d0b95056cbc6f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings---premium"></a>Denetim Azure içerik teslim ağı önbelleğe alma davranışını sorgu dizeleriyle - Premium
> [!div class="op_single_selector"]
> * [Standart](cdn-query-string.md)
> * [Verizon'dan Azure CDN Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Azure içerik teslim ağı (CDN), dosyaları bir sorgu dizesi içeren bir web isteği için nasıl önbelleğe kontrol edebilirsiniz. Sorgu dizesi olan bir web isteğinde sorgu dizesi bu sonra oluşan isteği bölümüdür `?` karakter. Bir sorgu dizesi tarafından ayrılmış bir veya daha fazla parametre içeren bir `&` karakter. Örneğin, `http://www.domain.com/content.mov?data1=true&data2=false`. Bir istekte birden fazla sorgu dizesi parametresi varsa, parametrelerin sırası önemli değildir. 

> [!IMPORTANT]
> Standart ve premium CDN ürünü için aynı sorgu dizesini önbelleğe alma işlevinin sağlasa da, kullanıcı arabirimi farklıdır.  Bu makalede arabirim için **verizon'dan Azure CDN Premium**. İle sorgu dizesi önbelleğe alma için **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**, bkz: [CDN önbelleğe alma davranışını denetleme, sorgu dizeleri içeren istekleri](cdn-query-string.md).
>

Üç sorgu dizesi modu kullanılabilir:

- **Standart önbellek**: varsayılan mod. Bu modda, CDN kenar düğümüne sorgu dizeleri istek sahibi kaynağa yapılan ilk istek geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar kenar düğümden sunulan tüm sonraki istekleri varlığı için sorgu dizelerini yoksayabilir.
- **Hayır-cache**: Bu modda, CDN kenar düğümüne sorgu dizeleri içeren istekleri önbelleğe alınmaz. Kenar düğümüne doğrudan kaynaktan varlığı alır ve bunu her istek ile istek sahibi geçirir.
- **Önbellek benzersiz**: Bu modda, sorgu dizesi dahil olmak üzere benzersiz bir URL'ye sahip her isteği kendi önbelleği ile benzersiz bir varlık olarak kabul edilir. Örneğin, bir istek için başlangıç yanıttan `example.ashx?q=test1` kenar düğümüne önbelleğe ve sonraki önbellekleri ile aynı sorgu dizesi döndürdü. Bir istek için `example.ashx?q=test2` ayrı bir varlık kendi yaşam süresi ayarı ile önbelleğe alınır.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Sorgu dizesini önbelleğe alma premium CDN profili ayarlarını değiştirme
1. CDN profili açın ve ardından **Yönet**.
   
    ![CDN profili Yönet düğmesi](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **HTTP büyük** sekmesini ve ardından üzerine gelerek **önbellek ayarları** açılır menüsü. Tıklatın **sorgu dizesi önbelleğe alma**.
   
    Sorgu dizesini önbelleğe alma seçenekleri görüntülenir.
   
    ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string-premium/cdn-query-string.png)
3. Bir sorgu dizesi modunu seçin ve ardından **güncelleştirme**.

> [!IMPORTANT]
> Kaydın yayılması zaman alır çünkü önbelleği dize ayarları değişiklikleri hemen görünür olmayabilir. İçin **verizon'dan Azure CDN Premium** profilleri yayma işlemi genellikle 90 dakika içinde tamamlanır, ancak bazı durumlarda daha uzun sürebilir.
 

