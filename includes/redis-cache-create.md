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
ms.openlocfilehash: 55bc2bd4e065c301f11a5fc4d3b58aa443b83e2d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
Önbellek oluşturmak için ilk olarak [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak oluştur** > **Veritabanları** > **Redis Cache**’ye tıklayın.

![Yeni önbellek](media/redis-cache-create/redis-cache-new-cache-menu.png)

**Yeni Redis Cache**’de yeni önbelleğinizin ayarlarını yapılandırın.

| Ayar      | Önerilen değer  | Açıklama |
| ------------ |  ------- | -------------------------------------------------- |
| **DNS adı** | Genel olarak benzersiz bir ad | Önbellek adı 1 - 63 karakter arasında bir dize olması ve yalnızca rakam, harf ve `-` karakterini içermelidir. Önbellek adı `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmazlar.  | 
| **Abonelik** | Aboneliğiniz | Bu yeni Azure Redis Cache’nin oluşturulduğu abonelik. | 
| **Kaynak Grubu** |  *TestResourceGroup* | Önbelleğinizin oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. | 
| **Konum** | Doğu ABD | Önbelleğinizi kullanacak olan diğer hizmetlerin yanında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
| **[Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cache/)** |  Temel C0 (250 MB Önbellek) |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [Azure Redis Cache’ye Genel Bakış](../articles/redis-cache/cache-overview.md). |
| **Panoya sabitle** |  Seçildi | Yeni önbelleği panonuza sabitleyerek kolay bulunur hale getirin. |

![Önbellek oluşturma](media/redis-cache-create/redis-cache-cache-create.png) 

Yeni önbellek seçenekleri yapılandırıldıktan sonra **Oluştur**’a tıklayın. 

Önbelleğin oluşturulması birkaç dakika sürebilir. Durumu denetlemek için panoda ilerlemeyi izleyebilirsiniz. Önbellek oluşturulduktan sonra yeni önbelleğiniz **Çalışır** durumdadır ve kullanıma hazırdır.

![Önbellek oluşturuldu](media/redis-cache-create/redis-cache-cache-created.png)

