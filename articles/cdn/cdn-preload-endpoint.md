---
title: Azure CDN uç noktasında varlıkları önceden yükleme | Microsoft Docs
description: Azure CDN uç noktasında önbelleğe alınmış içeriği önceden yükleme hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: magattus
ms.openlocfilehash: d2dc8ad1e4b7e429dc758a96e49aa4825ae108e5
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091326"
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure CDN uç noktasında varlıkları önceden yükleme
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Varsayılan olarak, yalnızca istenen varlıklar önbelleğe alınır. Uç sunucularda henüz içerik önbelleğe değil ve kaynak sunucu isteğini iletmeniz gerekir çünkü her bölgede ilk isteğinden sonraki istekler uzun sürebilir. Bu ilk isabet gecikmesini önlemek için varlıkları önceden yükleme. Daha iyi bir müşteri deneyimi sağlamaya ek olarak, önbelleğe alınan varlıkları önceden yükleme kaynak sunucu üzerindeki ağ trafiğini azaltabilir.

> [!NOTE]
> Varlıkları önceden yükleme büyük etkinliklerin veya yeni bir film yayın veya yazılım güncelleştirme gibi çok sayıda kullanıcı eşzamanlı olarak kullanılabilir içeriği için kullanışlıdır.
> 
> 

Bu öğreticide, tüm Azure CDN kenar düğümlerine önbelleğe alınmış içeriği önceden yükleme aracılığıyla açıklanmaktadır.

## <a name="to-pre-load-assets"></a>Varlıkları önceden yükleme için
1. İçinde [Azure portalında](https://portal.azure.com), önceden yükleme istediğiniz uç noktayı içeren CDN profiline göz atın. Profili bölmesi açılır.
    
2. Uç nokta listesinde tıklayın. Uç nokta bölmeyi açar.
3. CDN uç noktası bölmeden **yük**.
   
    ![CDN uç noktası bölmesi](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    **Yük** bölmesi açılır.
   
    ![CDN yükü bölmesi](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. İçin **içerik yolu**, yüklemek istediğiniz her varlık tam yolunu girin (örneğin, `/pictures/kitten.png`).
   
   > [!TIP]
   > Daha fazla metin girerek başlattıktan sonra **içerik yolu** birden çok varlık listesini oluşturmanıza izin vermek için metin kutuları görüntülenir. Varlıklar listeden silmek için üç nokta (...) düğmesini seçip **Sil**.
   > 
   > Her içerik yolu aşağıdaki uygun bir göreli URL olmalıdır [normal ifadeler](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Tek bir dosya yolu yükle: `^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$`  
   > - Sorgu dizesi tek bir dosyayı yükle: `^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$` 
   > 
   > Her varlık kendi yolu olması gerektiğinden, varlıkları önceden yükleme için joker karakter işlevi yoktur.
   > 
   > 
   
    ![Yük düğmesi](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. İçerik yolları girmeyi tamamladığınızda seçin **yük**.
   

> [!NOTE]
> Her CDN profili dakikada 10 yük istek sınırı yoktur ve 50 eş zamanlı yolları aynı anda işlenebilir. Her yol, 1024 karakterlik bir yol uzunluğu sınırı vardır.
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Bir Azure CDN uç noktasını temizleme](cdn-purge-endpoint.md)
* [Azure CDN REST API Başvurusu: önceden, bir uç nokta içerik yükleme](https://docs.microsoft.com/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN REST API Başvurusu: bir uç noktasından içerik temizleme](https://docs.microsoft.com/rest/api/cdn/endpoints/purgecontent)

