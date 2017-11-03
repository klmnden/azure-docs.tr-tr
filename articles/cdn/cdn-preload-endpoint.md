---
title: "Azure CDN uç noktasında varlıkları önceden yükleme | Microsoft Docs"
description: "Azure CDN uç noktada önbelleğe alınmış içeriği önceden yükleme hakkında bilgi edinin."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure CDN uç noktasında varlıkları önceden yükleme
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Varsayılan olarak, bunlar istendiği varlıklar ilk önbelleğe alınır. Bunun anlamı her bölge ilk istekten uzun sürebilir, uç sunucuların değil gerekeceğinden, içeriği önbelleğe'yı ve kaynak sunucuya istek iletmek gerekir. İçeriği önceden yüklerken bu ilk isabet gecikmesini önler.

Daha iyi bir müşteri deneyimi sağlamaya ek olarak, önbelleğe alınan varlıkları önceden yükleme da kaynak sunucu üzerindeki ağ trafiğini azaltabilir.

> [!NOTE]
> Varlıkları önceden yükleme büyük olayları veya kullanıcılar, yeni bir filmi sürüm veya bir yazılım güncelleştirmesi gibi çok sayıda eşzamanlı olarak kullanılabilir içeriği için kullanışlıdır.
> 
> 

Bu öğreticide, tüm Azure CDN uç düğümlerde önbelleğe alınmış içeriği önceden yükleme aracılığıyla açıklanmaktadır.

## <a name="walkthrough"></a>Kılavuz
1. İçinde [Azure Portal](https://portal.azure.com), önceden yüklemek istediğiniz uç nokta içeren CDN profili göz atın.  Profili dikey penceresi açılır.
2. Uç nokta listesinde'ı tıklatın.  Uç nokta dikey pencere açılır.
3. CDN uç noktası dikey penceresinden Yükle düğmesini tıklayın.
   
    ![CDN uç noktası dikey penceresi](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Yük dikey pencere açılır.
   
    ![CDN yük dikey penceresi](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Yüklemek istediğiniz her varlık tam yolunu girin (örn., `/pictures/kitten.png`) içinde **yolu** metin kutusu.
   
   > [!TIP]
   > Daha fazla **yolu** birden çok varlıkların listesi oluşturmanıza izin vermek için metni girdikten sonra metin kutuları görüntülenir.  Üç nokta (...) düğmesini tıklatarak listeden varlıklar silebilirsiniz.
   > 
   > Yolları aşağıdaki uygun göreli bir URL olmalıdır [normal ifade](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Tek bir dosya yolu yük `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Sorgu dizesi tek bir dosyayı yükleme`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Her varlık, kendi yolu olması gerekir.  Ön yükleme varlıklar için joker karakter işlevi yoktur.
   > 
   > 
   
    ![Yük düğmesi](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Tıklatın **yük** düğmesi.
   
    ![Yük düğmesi](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Her CDN profili dakikada 10 yük isteklerinin bir sınırlama yoktur. İstek başına 50 yollara izin verilir. Her yol 1024 karakterden oluşan bir yol uzunluğu sınırı vardır.
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Bir Azure CDN uç noktasını temizleme](cdn-purge-endpoint.md)
* [Azure CDN REST API Başvurusu - temizlemek veya bir uç nokta önceden yükleme](https://msdn.microsoft.com/library/mt634451.aspx)

