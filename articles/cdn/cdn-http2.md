---
title: "Azure CDN HTTP/2 desteği | Microsoft Docs"
description: "HTTP/2 ve CDN desteği hakkında bilgi edinin."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 4f8dd685c3ae89535217d7a17a01c5129ca7e6e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="http2-support-in-azure-cdn"></a>Azure CDN HTTP/2 desteği

HTTP/2 HTTP/1.1\ için önemli bir düzeltme olur. Daha hızlı web performans, daha az yanıt süresi ve gelişmiş bir kullanıcı, bilinen HTTP yöntemleri, durum kodları ve semantiği korurken deneyimi sağlar. HTTP/2 HTTP ve HTTPS ile çalışmak üzere tasarlanmıştır ancak çok sayıda istemci web tarayıcıları TLS yalnızca HTTP/2 destekler.

###<a name="http2-benefits"></a>HTTP/2 avantajları

HTTP/2 avantajları şunlardır:

*   **Çoğullama ve eşzamanlılık**

    HTTP 1.1 kullanarak, birden çok birden çok kaynak istekleri birden fazla TCP bağlantısı gerektirir ve her bağlantı ile ilişkili performans yüke sahiptir. HTTP/2 tek bir TCP bağlantı üzerinde istenmesi için birden çok kaynak sağlar.

*   **Üstbilgi sıkıştırma**

    Sunulacak kaynaklar için HTTP üstbilgileri sıkıştırarak hattaki süresi önemli ölçüde azalır.

*   **Akış bağımlılıkları**

    Akış bağımlılıkları sunucuya göstermek istemci önceliğe sahip olduğunuz kaynakların izin verin.


##<a name="http2-browser-support"></a>HTTP/2 tarayıcı desteği

Tüm önde gelen tarayıcılar HTTP/2 desteği geçerli sürümlerine uyguladık. HTTP/1.1 için otomatik olarak geri dönüş desteklenmeyen tarayıcılar olacaktır.

|Tarayıcı|En düşük sürüm|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

##<a name="enabling-http2-support-in-azure-cdn"></a>Azure CDN HTTP/2 desteğini etkinleştirme

HTTP/2 desteği şu anda etkin mi **akamai'den Azure CDN** ve **verizon'dan Azure CDN** profilleri. Başka bir eylem müşterilerden gereklidir.

##<a name="next-steps"></a>Sonraki Adımlar

HTTP/2 avantajları uygulamada görmek için bkz: [akamai'den bu demo](https://http2.akamai.com/demo).

HTTP/2 hakkında daha fazla bilgi için aşağıdaki kaynaklara ziyaret edin:

*   [HTTP/2 belirtimi giriş sayfası](https://http2.github.io/)
*   [Resmi HTTP/2 ile ilgili SSS](https://http2.github.io/faq/)
*   [Akamai HTTP/2 bilgileri](https://http2.akamai.com/)

Azure CDN'ın kullanılabilir özellikler hakkında daha fazla bilgi için bkz: [Azure CDN'ye genel bakış](https://azure.microsoft.com/documentation/articles/cdn-overview/).