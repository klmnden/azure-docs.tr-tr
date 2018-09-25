---
title: Azure ön kapısı hizmeti - HTTP2 desteği | Microsoft Docs
description: Bu makale Azure ön kapısı hizmetinde HTTP/2 desteği hakkında daha fazla bilgi edinmenize yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 4282c9e9b660476992ba6f948bc5e408e9b064a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968620"
---
# <a name="http2-support-in-azure-front-door-service"></a>Azure ön kapısı hizmetinde HTTP/2 desteği
HTTP/2 HTTP/1.1 büyük bir düzeltme'dir. Daha hızlı, web performansı, daha az yanıt süresi ve gelişmiş bir kullanıcı, bilinen HTTP yöntemleri, durum kodları ve semantiği korurken deneyimi sağlar. HTTP/2 HTTP ve HTTPS ile çalışmak için tasarlanmış olsa da, çok sayıda istemci web tarayıcıları yalnızca Aktarım Katmanı Güvenliği (TLS) üzerinden HTTP/2 desteği.

### <a name="http2-benefits"></a>HTTP/2 avantajları

HTTP/2'in avantajları şunlardır:

*   **Çoğullama ve eşzamanlılık**

    HTTP 1.1 kullanarak, birden çok kaynak isteği gerçekleştiren birden çok TCP bağlantı gerektirir ve her bağlantı ilişkili performansa sahiptir. HTTP/2 tek bir TCP bağlantı üzerinde istenmesi birden fazla kaynak sağlar.

*   **Üst bilgi sıkıştırma**

    Hizmet kaynakları için HTTP üstbilgilerini sıkıştırarak kablo süresi önemli ölçüde azaltılır.

*   **Stream bağımlılıkları**

    Stream bağımlılıklar, hangi kaynakların önceliğe sahip sunucuya göstermek için istemcide izin verin.


## <a name="http2-browser-support"></a>HTTP/2 tarayıcı desteği

Tüm bilinen tarayıcılar HTTP/2 desteği, geçerli sürümlerinde uyguladınız. Tarayıcılar HTTP/1.1 otomatik olarak geri dönüş, desteklenmeyen.

|Tarayıcı|En düşük sürüm|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-front-door-service"></a>Azure ön kapısı hizmetinde HTTP/2 desteği etkinleştirme

Şu anda, HTTP/2 desteği için tüm ön kapısı yapılandırmaları etkin değil. Müşterilerden daha fazla eylem gerekmiyor.

## <a name="next-steps"></a>Sonraki Adımlar

HTTP/2 hakkında daha fazla bilgi için aşağıdaki kaynakları ziyaret edin:

- [HTTP/2 belirtimi giriş sayfası](https://http2.github.io/)
- [Resmi HTTP/2 SSS](https://http2.github.io/faq/)
- Bilgi edinmek için nasıl [ön kapı oluşturmak](quickstart-create-front-door.md).
- Bilgi [ön kapısı işleyişi](front-door-routing-architecture.md).
