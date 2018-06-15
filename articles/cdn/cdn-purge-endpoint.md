---
title: Bir Azure CDN uç noktasını temizleme | Microsoft Docs
description: Bir Azure CDN uç noktasından tüm önbelleğe alınmış içeriği temizlenecek öğrenin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 262a8f7385ba5f74d21991772599540260a145fc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765175"
---
# <a name="purge-an-azure-cdn-endpoint"></a>Bir Azure CDN uç noktasını temizleme
## <a name="overview"></a>Genel Bakış
Varlığın yaşam süresi (TTL) süresi doluncaya kadar azure CDN uç düğümleri varlıklar önbelleğe alır.  Bir istemci kenar düğümden varlık istediğinde varlığın TTL süresi dolduğunda, kenar düğümüne istemci isteği sunmak için varlığı yeni güncelleştirilmiş bir kopyasını alır ve depolama önbelleği yenileyin.

Kullanıcılarınızın her zaman en yeni kopyasını varlıklarınızı Al emin olmak için en iyi uygulama varlıklarınızı her bir güncelleştirme sürümüdür ve bunları yeni URL'ler olarak yayımlayabilirsiniz.  CDN hemen sonraki istemci istekleri için yeni varlıklar alır.  Bazen önbelleğe alınmış içeriği tüm kenar düğümlerinden temizlemek ve bunları tüm yeni güncelleştirilmiş varlıklar almak için zorlamak isteyebilirsiniz.  Bu, web uygulamanızı veya hızlı bir şekilde yanlış bilgiler içeren güncelleştirme varlıklar güncelleştirmeleri nedeniyle olabilir.

> [!TIP]
> Temizleme, CDN uç sunucularda önbelleğe alınmış içeriği yalnızca temizler olduğunu unutmayın.  Proxy sunucuları ve yerel tarayıcı önbellekleri gibi tüm aşağı akış önbellekleri yine dosyayı önbelleğe alınmış bir kopyasını tutabilir.  Bir dosyanın yaşam süresi ayarladığınızda unutmamak önemlidir.  Dosyanızı en son sürümünü güncelleştirmeniz her zaman benzersiz bir ad verip tarafından ya da yararlanarak istemek için bir aşağı akış istemci zorlayabilirsiniz [sorgu dizesi önbelleğe alma](cdn-query-string.md).  
> 
> 

Bu öğreticide, bir uç nokta tüm kenar düğümlerinden varlıklar temizleme aracılığıyla açıklanmaktadır.

## <a name="walkthrough"></a>Kılavuz
1. İçinde [Azure Portal](https://portal.azure.com), temizlemek istediğiniz uç nokta içeren CDN profili göz atın.
2. CDN profili dikey penceresinden Temizleme düğmesini tıklatın.
   
    ![CDN profili dikey penceresi](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Temizleme dikey pencere açılır.
   
    ![CDN temizleme dikey penceresi](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Temizleme dikey penceresinde, URL aşağı açılır listeden temizlemek istediğiniz hizmeti adresi seçin.
   
    ![Form Temizle](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Tıklayarak temizleme dikey penceresine alabilirsiniz **Temizleme** CDN uç noktası dikey düğmesi.  Bu durumda, **URL** alan, belirli bir uç hizmeti adresiyle önceden doldurulmuş haldedir olacaktır.
   > 
   > 
4. Edge düğümlerden temizlemek istediğiniz hangi varlıkları seçin.  Tüm varlıklar temizlemek isterseniz, tıklatın **Tümünü Temizle** onay kutusu.  Aksi takdirde, temizlemek istediğiniz her varlık yolunu yazın **yolu** metin kutusu. Yolu biçimleri desteklenir.
    1. **Tek URL Temizleme**: temizleme tek varlığı ile veya olmadan dosya uzantısı, örneğin, tam URL belirterek`/pictures/strasbourg.png`; `/pictures/strasbourg`
    2. **Joker Temizleme**: yıldız işareti (\*) joker karakter olarak kullanılabilir. Tüm klasörleri, alt klasörler ve dosyaları bir uç nokta altında Temizleme `/*` yolunu veya temizleme tüm alt klasörleri ve klasör belirterek belirli bir klasör altındaki dosyalar ve ardından `/*`, örn.,`/pictures/*`.  Bu joker temizleme akamai'den Azure CDN tarafından şu anda desteklenmiyor unutmayın. 
    3. **Kök etki alanı Temizleme**: kök yolundaki "/" ile uç noktasının Temizle.
   
   > [!TIP]
   > Yollar için temizleme belirtilmeli ve şunları sığacak göreli bir URL olmalıdır [normal ifade](https://msdn.microsoft.com/library/az24scfc.aspx). **Tümünü Temizle** ve **joker Temizleme** tarafından desteklenmeyen **akamai'den Azure CDN** şu anda.
   > > Tek URL temizleme `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Sorgu dizesi `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Joker Temizleme `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Daha fazla **yolu** birden çok varlıkların listesi oluşturmanıza izin vermek için metni girdikten sonra metin kutuları görüntülenir.  Üç nokta (...) düğmesini tıklatarak listeden varlıklar silebilirsiniz.
   > 
5. Tıklatın **Temizleme** düğmesi.
   
    ![Düğme Temizle](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Temizleme istekleri almak yaklaşık 2-3 ile işlemek için dakika **verizon'dan Azure CDN** (standart ve premium) ve yaklaşık 7 dakika ile **akamai'den Azure CDN**.  Azure CDN profili düzeyinde herhangi bir anda istekleri Temizle 50 eşzamanlı bir sınıra sahiptir. 
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN uç noktasında varlıkları önceden yükleme](cdn-preload-endpoint.md)
* [Azure CDN REST API Başvurusu - temizlemek veya bir uç nokta önceden yükleme](https://msdn.microsoft.com/library/mt634451.aspx)

