---
title: include dosyası
description: include dosyası
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 6a96bd7c3d7d02f181a7d7513edb3bb39881274f
ms.sourcegitcommit: 7de1432648c4ff3bcd09530c079418477d9f4d00
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35719447"
---
1. Önbellek oluşturmak için ilk olarak [Azure portalda](https://portal.azure.com) oturum açın. Ardından **Kaynak oluştur** > **Veritabanları** > **Redis Cache**’i seçin.

    ![Yeni önbellek](media/redis-cache-create/redis-cache-new-cache-menu.png)

2. **Yeni Redis Cache**’de yeni önbelleğinizin ayarlarını yapılandırın.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS adı** | Genel olarak benzersiz bir ad | Önbellek adı. Önbellek adı 1 - 63 karakter arasında bir dize olması ve yalnızca rakam, harf ve `-` karakterini içermelidir. Önbellek adı `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmazlar.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni Azure Redis Cache örneğinin oluşturulduğu abonelik. | 
    | **Kaynak grubu** |  *TestResources* | Önbelleğinizin oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. | 
    | **Konum** | Doğu ABD | Önbelleğinizi kullanacak olan diğer hizmetlerin yanında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **[Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cache/)** |  Temel C0 (250 MB Önbellek) |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [Azure Redis Cache’ye Genel Bakış](../articles/redis-cache/cache-overview.md). |
    | **Panoya sabitle** |  Seçildi | Yeni önbelleği panonuza sabitleyerek kolay bulunur hale getirin. |

    ![Önbellek oluşturma](media/redis-cache-create/redis-cache-cache-create.png) 

3. Yeni önbellek seçenekleri yapılandırıldıktan sonra **Oluştur**’u seçin. 

    Önbelleğin oluşturulması birkaç dakika sürebilir. Durumu denetlemek için panoda ilerlemeyi izleyebilirsiniz. Önbellek oluşturulduktan sonra durumunda **Çalışır** ibaresi bulunur ve kullanıma hazır olduğunu gösterir.

    ![Önbellek oluşturuldu](media/redis-cache-create/redis-cache-cache-created.png)

