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
ms.openlocfilehash: a00c1f916d7e8753830bf87bef6897766acbc276
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53018387"
---
1. Önbellek oluşturmak için ilk olarak [Azure portalda](https://portal.azure.com) oturum açın. Ardından **kaynak Oluştur** > **veritabanları** > **Azure önbelleği için Redis**.

    ![Yeni önbellek](media/redis-cache-create/redis-cache-new-cache-menu.png)

2. İçinde **yeni Azure önbelleği için Redis**, yeni önbelleğiniz için ayarları yapılandırın.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS adı** | Genel olarak benzersiz bir ad | Önbellek adı. Önbellek adı 1 - 63 karakter arasında bir dize olması ve yalnızca rakam, harf ve `-` karakterini içermelidir. Önbellek adı `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmazlar.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni Azure Azure önbelleği için Redis örneği oluşturulduğu abonelik. | 
    | **Kaynak grubu** |  *TestResources* | Önbelleğinizin oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. | 
    | **Konum** | Doğu ABD | Önbelleğinizi kullanacak olan diğer hizmetlerin yanında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **[Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cache/)** |  Temel C0 (250 MB Önbellek) |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için [Azure Azure önbelleği için Redis genel bakış](../articles/azure-cache-for-redis/cache-overview.md). |
    | **Panoya sabitle** |  Seçildi | Yeni önbelleği panonuza sabitleyerek kolay bulunur hale getirin. |

    ![Önbellek oluşturma](media/redis-cache-create/redis-cache-cache-create.png) 

3. Yeni önbellek seçenekleri yapılandırıldıktan sonra **Oluştur**’u seçin. 

    Önbelleğin oluşturulması birkaç dakika sürebilir. Durumu denetlemek için panoda ilerlemeyi izleyebilirsiniz. Önbellek oluşturulduktan sonra durumunda **Çalışır** ibaresi bulunur ve kullanıma hazır olduğunu gösterir.

    ![Önbellek oluşturuldu](media/redis-cache-create/redis-cache-cache-created.png)

