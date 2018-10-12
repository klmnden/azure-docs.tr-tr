---
title: Bir Azure CDN uç noktasını temizleme | Microsoft Docs
description: Bir Azure CDN uç noktasından tüm önbelleğe alınan içeriği temizlemek öğrenin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: magattus
ms.openlocfilehash: a3777533fc967e1974b99375496dd3777fa9fb3a
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093859"
---
# <a name="purge-an-azure-cdn-endpoint"></a>Bir Azure CDN uç noktasını temizleme
## <a name="overview"></a>Genel Bakış
Azure CDN kenar düğümlerine, varlığın yaşam süresi (TTL) dolmadan varlıklar önbelleğe alır.  Bir istemci uç düğümden varlık istediğinde varlığın TTL süresi dolduğunda, kenar düğümüne istemci isteğe hizmet vermek için varlığı yeni güncelleştirilmiş bir kopyasını alır ve depolama önbelleği yenileyin.

Kullanıcılarınızın her zaman en son kopyasını varlıklarınızı Al emin olmak için en iyi her güncelleştirme için varlıklarınızı sürümüdür ve bunları yeni URL olarak yayımlayın.  CDN hemen sonraki istemci istekleri için yeni varlıkları alır.  Bazen tüm kenar düğümlerinden önbelleğe alınmış içeriği temizlemek ve bunları tüm yeni güncelleştirilmiş varlıkları almak için zorlamak isteyebilirsiniz.  Bu, web uygulamanıza veya hızlı bir şekilde yanlış bilgiler içeren güncelleştirme varlıklarına güncelleştirmeleri nedeniyle olabilir.

> [!TIP]
> Temizleme, CDN uç sunucularda önbelleğe alınmış içeriği yalnızca temizler olduğunu unutmayın.  Proxy sunucusu veya yerel tarayıcı önbellekler gibi tüm aşağı akış önbellekleri önbelleğe alınmış bir dosyanın kopyasını hala tutabilir.  Bir dosyanın yaşam süresi ayarladığınızda unutmamak önemlidir.  Dosyanın en son sürümü her güncelleştirdiğinizde tarafından benzersiz bir ad verin ya da yararlanarak istemek için bir aşağı akış istemci zorlayabilirsiniz [sorgu dizesini önbelleğe alma](cdn-query-string.md).  
> 
> 

Bu öğreticide, bir uç nokta tüm kenar düğümlerinden varlıkları temizleme aracılığıyla açıklanmaktadır.

## <a name="walkthrough"></a>Kılavuz
1. İçinde [Azure portalı](https://portal.azure.com), temizlemek istediğiniz uç noktayı içeren CDN profiline göz atın.
2. CDN profili dikey penceresinden Temizle düğmesine tıklayın.
   
    ![CDN profili dikey penceresi](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Temizleme dikey penceresi açılır.
   
    ![CDN temizleme dikey penceresi](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Temizleme dikey penceresinde, URL açılır listeden temizlemek istediğiniz hizmeti adresi seçin.
   
    ![Form Temizle](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Tıklayarak temizleme dikey penceresine alabilirsiniz **Temizleme** CDN uç noktası dikey penceresinde düğmesi.  Bu durumda, **URL** alan, belirli bir uç nokta hizmet adresiyle önceden doldurulmuş olacaktır.
   > 
   > 
4. Kenar düğümlerinden temizlemek istediğiniz hangi varlıkları seçin.  Tüm varlıkları silmek isterseniz tıklayın **Tümünü Temizle** onay kutusu.  Aksi takdirde her varlık, temizlemek istediğiniz yolu yazın **yolu** metin. Yolu aşağıdaki biçimleri desteklenir.
    1. **Tek URL Temizleme**: temizleme tek varlığı ile veya olmadan dosya uzantısı, örneğin, tam bir URL belirterek`/pictures/strasbourg.png`; `/pictures/strasbourg`
    2. **Joker Temizleme**: yıldız işareti (\*) joker karakter olarak kullanılabilir. Tüm klasörleri, alt klasörler ve dosyaları bir uç nokta altında Temizleme `/*` tüm alt klasörleri ve klasör belirterek belirli bir klasör altındaki dosyalar ardında yer alan yolu ya da Temizleme `/*`, örneğin,`/pictures/*`.  Unutmayın. Bu joker temizleme Azure CDN from Akamai tarafından şu anda desteklenmiyor. 
    3. **Kök etki alanı Temizleme**: kök yolda "/" ile uç nokta temizleme.
   
   > [!TIP]
   > Yolları için temizleme belirtilmelidir ve aşağıdaki uyan bir göreli URL olmalıdır [normal ifade](https://msdn.microsoft.com/library/az24scfc.aspx). **Tümünü Temizle** ve **joker Temizleme** desteklenmeyen **akamai'den Azure CDN** şu anda.
   > > Tek URL temizleme `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Sorgu dizesi `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Joker Temizleme `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Daha fazla **yolu** metin kutuları, birden çok varlık listesini oluşturmanıza izin vermek için metin girdikten sonra görünür.  Üç nokta (…) düğmesine tıklayarak listeden varlıkları silebilirsiniz.
   > 
5. Tıklayın **Temizleme** düğmesi.
   
    ![Temizle düğmesi](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Temizleme istekleri Al yaklaşık 2-3 ile işlemek için dakika **verizon'dan Azure CDN** (standart ve premium) ve yaklaşık 7 dakika ile **akamai'den Azure CDN**.  Azure CDN, 50 eş zamanlı istekleri belirli bir zamanda profil düzeyinde temizleme sınırı vardır. 
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN uç noktasında varlıkları önceden yükleme](cdn-preload-endpoint.md)
* [Azure CDN REST API Başvurusu - temizlemek veya bir uç nokta önceden yükleme](https://msdn.microsoft.com/library/mt634451.aspx)

