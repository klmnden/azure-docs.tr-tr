---
title: "Azure CDN uç noktasında varlıkları önceden yükleme | Microsoft Docs"
description: "Azure CDN uç noktada önbelleğe alınmış içeriği önceden yükleme hakkında bilgi edinin."
services: cdn
documentationcenter: 
author: dksimpson
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mazha
ms.openlocfilehash: acd6eae12ff338c64cc8879aa8c27b226e3d2f84
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure CDN uç noktasında varlıkları önceden yükleme
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Varsayılan olarak, yalnızca talep edildiğinde varlıkların önbelleğe alınır. Sonuç olarak, her bölge ilk isteğinden sonraki istekleri uzun sürebilir. Uç sunucuların henüz içerik önbelleğe değil ve kaynak sunucuya istek iletmek gerekir çünkü nedenidir. İçeriği önceden yüklerken bu ilk isabet gecikmesini engelleyebilirsiniz.

Daha iyi bir müşteri deneyimi sağlamaya ek olarak, önbelleğe alınan varlıkları önceden yükleme da kaynak sunucu üzerindeki ağ trafiğini azaltabilir.

> [!NOTE]
> Varlıkları önceden yükleme büyük olayları veya kullanıcılar, yeni bir filmi sürüm veya bir yazılım güncelleştirmesi gibi çok sayıda eşzamanlı olarak kullanılabilir içeriği için kullanışlıdır.
> 
> 

Bu öğreticide, tüm Azure CDN uç düğümlerde önbelleğe alınmış içeriği önceden yükleme aracılığıyla açıklanmaktadır.

## <a name="to-pre-load-assets"></a>Varlıkları önceden yükleme için
1. İçinde [Azure portal](https://portal.azure.com), önceden yüklemek istediğiniz uç nokta içeren CDN profili göz atın. Profil bölmesi açılır.
    
2. Uç nokta listesinde'ı tıklatın. Endpoint bölmesini açar.
3. CDN uç noktası bölmesinden seçin **yük**.
   
    ![CDN uç noktası bölmesi](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    **Yük** bölmesini açar.
   
    ![CDN yük bölmesi](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. İçin **içerik yolu**, yüklemek istediğiniz her varlık tam yolunu girin (örneğin, `/pictures/kitten.png`).
   
   > [!TIP]
   > Daha fazla **içerik yolu** birden çok varlıkların listesi oluşturmanıza izin vermek için metin girerek başlattıktan sonra metin kutuları görüntülenir. Varlıklar listeden silmek için üç nokta (...) düğmesini seçin ve sonra seçin **silmek**.
   > 
   > Her içerik yolu aşağıdaki uygun göreli bir URL olmalıdır [normal ifadeler](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Tek bir dosya yolu yük: `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > - Sorgu dizesi tek bir dosyayı yükleme:`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";` 
   > 
   > Her varlık, kendi yolu olması gerekir. Ön yükleme varlıklar için joker karakter işlevi yoktur.
   > 
   > 
   
    ![Yük düğmesi](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. İçerik yolları girmeyi tamamladığınızda, seçin **yük**.
   

> [!NOTE]
> Her CDN profili dakikada 10 yük isteklerinin bir sınırlama yoktur. 50 eşzamanlı yolu aynı anda işlenebilir. Her yol 1024 karakterden oluşan bir yol uzunluğu sınırı vardır.
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Bir Azure CDN uç noktasını temizleme](cdn-purge-endpoint.md)
* [Azure CDN REST API Başvurusu: önceden bir uç nokta içerik'ı yükleme](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN REST API Başvurusu: bir uç noktasından içerik temizleme](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/purgecontent)

