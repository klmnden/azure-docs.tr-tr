---
title: Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı denetim | Microsoft Docs
description: Azure CDN sorgu dizesini bir web isteğine bir sorgu dizesi içeren dosyaların önbelleğe nasıl alındığını denetimleri önbelleğe alma. Bu makalede, sorgu dizesini önbelleğe alma, ürünün Verizon'dan Azure CDN Premium açıklanır.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: magattus
ms.openlocfilehash: 2f0a361d53489e22ccc8e41406e5b86b423ea2f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324836"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Azure Content Delivery Network (CDN ile), bir web isteği için bir sorgu dizesi içeren dosyaların önbelleğe nasıl kontrol edebilirsiniz. Bir sorgu dizesi ile bir web isteğinde sorgu dizesi bir soru işareti (?) sonra gerçekleşen istek, bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır, bir veya daha fazla anahtar-değer çiftleri içerir. Her anahtar-değer çifti bir ampersan ile ayrılır (&). Örneğin, http:\//www.contoso.com/content.mov?field1=value1 & alan2 = value2. Bir isteğin sorgu dizesinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir. 

> [!IMPORTANT]
> Standart ve premium CDN ürünü için aynı sorgu dizesini önbelleğe alma işlevinin sağlasa da, kullanıcı arabirimi farklıdır. Bu makalede arabirimi **verizon'dan Azure CDN Premium**. Azure CDN standart ürünleriyle önbelleğe alma için sorgu dizesi bkz [denetimi Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katman](cdn-query-string.md).
>


Üç sorgu dizesi modu kullanılabilir:

- **Standart-cache**: Varsayılan modu. Bu modda, CDN bulunma noktası (POP) düğüm sorgu dizelerini istek sahibine kaynak sunucuya ilk istek üzerine geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar bulunma noktası sunucudan sunulan tüm sonraki istekleri için varlık sorgu dizelerini yoksay.

    >[!IMPORTANT] 
    > Bu hesapta herhangi bir yol için yetkilendirme belirteci etkinleştirilirse, standart Önbellek modu kullanılabilecek tek moddur. 

- **no-cache**: Bu modda, CDN POP düğümde sorgu dizeleriyle istekleri önbelleğe alınmaz. POP düğüm doğrudan kaynak sunucudan varlığı alır ve her istek ile istek sahibine göre geçirir.

- **Önbellek benzersiz**: Bu modda, benzersiz bir URL, sorgu dizesi dahil olmak üzere her bir istekle kendi önbellek ile benzersiz bir varlık olarak kabul edilir. Örneğin, example.ashx?q=test1 için bir istek için kaynak sunucudan yanıt POP düğümde önbelleğe alınmış ve sonraki önbelleklerle aynı sorgu dizesi için döndürülen. Bir istek example.ashx?q=test2 için kendi yaşam süresi ayarı ayrı bir varlık olarak önbelleğe alınır.
   
    >[!IMPORTANT] 
    > Düşük bir isabetli önbellek okuması oranı olarak sonuçlanacağından, sorgu dizesi bir oturum kimliği veya bir kullanıcı adı gibi her bir istekle değiştirecek parametreleri içerdiğinde, bu mod kullanmayın.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Sorgu dizesini önbelleğe alma premium CDN profili için ayarları değiştirme
1. CDN profili açın ve ardından tıklayın **Yönet**.
   
    ![CDN profili Yönet düğmesi](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    CDN yönetim portalına açılır.
2. Üzerine **HTTP büyük** sekmesine ve ardından üzerine **önbellek ayarları** açılır menüsü. Tıklayın **sorgu dizesini önbelleğe alma**.
   
    Sorgu dizesini önbelleğe alma seçenekleri görüntülenir.
   
    ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string-premium/cdn-query-string.png)
3. Bir sorgu dizesi modunu seçin, ardından tıklatın **güncelleştirme**.

> [!IMPORTANT]
> Kaydın CDN'de yayılması zaman alacağından, önbellek dize ayarları değişiklikleri hemen görünmeyebilir. Yayma genellikle 10 dakika içinde tamamlanır.
 

