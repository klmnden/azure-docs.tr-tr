---
title: Azure Market SEO Kılavuzu
description: Arama motoru iyileştirmesi (SEO) en üst düzeye konusunda rehberlik yapmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: pabutler
ms.openlocfilehash: f5b956ed1197e3898c9536bda3a93a41e8ee35c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935138"
---
# <a name="azure-marketplace-seo-guidance"></a>Azure Market SEO Kılavuzu

Bu makalede, ürününüzün bulunabilirliği arama işlevleri aracılığıyla en üst düzeye çıkarmak açıklanmaktadır [Azure Marketi](https://azuremarketplace.microsoft.com) ve [AppSource](https://appsource.microsoft.com). 


## <a name="general-explanation-of-algorithm"></a>Genel bir algoritma açıklaması

Microsoft marketleri için sitenin arama özellikleri destekleyen Azure Search kullanır. Algoritma terimi sıklığı – ters belge sıklığına bağlıdır ([TF-IDF](https://en.wikipedia.org/wiki/Tf–idf)). Standart [Lucene çözümleyici](https://lucene.apache.org/core/) kullanılır.

Genel olarak, tüm metin alanları, kategoriler ve sektörler ve ilgi weightage dahil. Uygulamalar tarafından ancak uygulamanızda sık sık kullanılan özelleştirilmiş koşulları arama daha yüksek bir eşleşme puanıyla oluşturur. Bu nedenle "VM" gibi terimler dahil olmak üzere, "Azure arama" çok daha özel ancak küçük avantajı sunar.
Dikkate alınması gereken en uygun alanları aşağıda verilmiştir.

 
|  Alan                   | Önem derecesi | Rehber                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Teklif Adı               |  Yüksek      | Tam veya arama ile tam bir eşleştirme yakın sorgu yüksek derecelendirme ortaya çıkarır.                       |
| Yayımcı adı           |  Yüksek      | Tam veya arama ile tam bir eşleştirme yakın sorgu yüksek derecelendirme ortaya çıkarır.                       |
| Kısa açıklama        |  Orta    | Uygulamalar ve yayımcı adlandırma verilen adları neredeyse yüksek derecelendirme garanti, en uygun olmayabilir. Bu durumda, kısa bir açıklama kritik öneme sahiptir. Metin noktasına ve kısa tutun. En iyi sonuç için anahtar sözcükleri ve beklenen arama terimlerini eklenmelidir.  Örneğin "Bu bir Dynamics 365 üzerinde tam olarak oluşturulmuş en iyi perakende POS" daha az etkili "için Dynamics 365" perakende POS (satış noktası).  | 
| Uzun açıklama         |  Düşük       | Açıklama daha ayrıntılı gitmek için bir yol sunar. En etkili açıklamaları makul ve anahtar sözcükleri kullanılır.  Anahtar sözcükler kullanılarak bir için öz açıklamaları birden fazla uzun uzun metin yararlı olacaktır. "IoT" gibi yapma emin anahtar koşulları açıklama yok.  |
| Ürün Kategorileri       | Orta     |  Ürün kategorileri birleşimi yayımcı seçenekleri ve Microsoft tarafından belirlenir. Bu kategorilerin uygun şekilde seçin, böylece kullanıcılar doğru kategorisinde uygulamalarını kolayca bulabilirsiniz. |
|  |  |  |


## <a name="other-tips"></a>Diğer ipuçları

-   Arama alır yoğun bir kullanıcı etkinliği önerir. Bu, uygulama adı/yayımcısı karşı eşleşme önceliklendirir. Kısa açıklama arama terimi yayımcı/uygulama adı ile tam bir eşleşme olmadığında için anahtar alanı olur.
-   Belgeleri indirmek için arama weightage içinde yer almaz.
-   Kullanımı ve gerçek uygulamaları edinme, arama de sıralama etkiler. Örneğin, bir büyük ölçüde daha fazla kullanıcının sahip olduğu iki eşdeğer uygulamalar daha yüksek derecelendirme alırsınız.
