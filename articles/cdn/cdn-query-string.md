---
title: Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katman denetim | Microsoft Docs
description: Azure CDN sorgu dizesini bir web isteğine bir sorgu dizesi içeren dosyaların önbelleğe nasıl alındığını denetimleri önbelleğe alma. Bu makalede Azure CDN standart ürünler, sorgu dizesi.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: magattus
ms.openlocfilehash: 2b9e56f8a0a023c8423426fee081a5a48ebda330
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593468"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katman
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Azure Content Delivery Network (CDN ile), bir web isteği için bir sorgu dizesi içeren dosyaların önbelleğe nasıl kontrol edebilirsiniz. Bir sorgu dizesi ile bir web isteğinde sorgu dizesi bir soru işareti (?) sonra gerçekleşen istek, bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır, bir veya daha fazla anahtar-değer çiftleri içerir. Her anahtar-değer çifti bir ampersan ile ayrılır (&). Örneğin, http:\//www.contoso.com/content.mov?field1=value1 & alan2 = value2. Bir isteğin sorgu dizesinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir. 

> [!IMPORTANT]
> Azure CDN standart ve premium ürünleri aynı sorgu dizesini önbelleğe alma işlevselliğini sağlar ancak kullanıcı arabirimi farklıdır. Bu makalede arabirimi **Azure CDN standart Microsoft gelen**, **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**. İle sorgu dizesini önbelleğe alma için **verizon'dan Azure CDN Premium**, bkz: [denetimi Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı](cdn-query-string-premium.md).

Üç sorgu dizesi modu kullanılabilir:

- **Sorgu dizelerini Yoksay**: Varsayılan modu. Bu modda, CDN bulunma noktası (POP) düğüm sorgu dizelerini istek sahibine kaynak sunucuya ilk istek üzerine geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar bulunma noktası ' sunulan tüm sonraki istekleri varlık için sorgu dizelerini yoksay.

- **Sorgu dizeleri için önbelleğe almayı atlama**: Bu modda, CDN POP düğümde sorgu dizeleriyle istekleri önbelleğe alınmaz. POP düğüm doğrudan kaynak sunucudan varlığı alır ve her istek ile istek sahibine göre geçirir.

- **Her benzersiz URL'yi önbelleğe al**: Bu modda, benzersiz bir URL, sorgu dizesi dahil olmak üzere her bir istekle kendi önbellek ile benzersiz bir varlık olarak kabul edilir. Örneğin, example.ashx?q=test1 için bir istek için kaynak sunucudan yanıt POP düğümde önbelleğe alınmış ve sonraki önbelleklerle aynı sorgu dizesi için döndürülen. Bir istek example.ashx?q=test2 için kendi yaşam süresi ayarı ayrı bir varlık olarak önbelleğe alınır.
   
    >[!IMPORTANT] 
    > Düşük bir isabetli önbellek okuması oranı olarak sonuçlanacağından, sorgu dizesi bir oturum kimliği veya bir kullanıcı adı gibi her bir istekle değiştirecek parametreleri içerdiğinde, bu mod kullanmayın.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Sorgu dizesini önbelleğe alma standart CDN profili için ayarları değiştirme
1. CDN profili açın ve yönetmek istediğiniz CDN uç noktası seçin.
   
   ![CDN profili uç noktaları](./media/cdn-query-string/cdn-endpoints.png)
   
2. Ayarlar altındaki sol bölmesinde **önbelleğe alma kuralları**.
   
    ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. İçinde **sorgu dizesini önbelleğe alma davranışı** listesinde, bir sorgu dizesi modu seçin ve ardından tıklayın **Kaydet**.
   
   ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Azure CDN'de yayılması zaman alacağından önbellek dize ayarları değişiklikleri hemen görünür olmayabilir:
> - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 10 dakika içinde tamamlanır. 



