---
title: Azure CDN, HTTP/2 desteği | Microsoft Docs
description: HTTP/2 ve CDN desteği hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: lichard
manager: erikre
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2d27cd54486a08e18fe74c852af29d5cf6432023
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737083"
---
# <a name="http2-support-in-azure-cdn"></a>Azure CDN, HTTP/2 desteği

HTTP/2 HTTP/1.1\ için büyük bir düzeltme'dir. Daha hızlı, web performansı, daha az yanıt süresi ve gelişmiş bir kullanıcı, bilinen HTTP yöntemleri, durum kodları ve semantiği korurken deneyimi sağlar. HTTP/2 HTTP ve HTTPS ile çalışmak için tasarlanmış olsa da, çok sayıda istemci web tarayıcıları yalnızca TLS üzerinden HTTP/2 desteği.

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

|Tarayıcı|Minimum Version|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-cdn"></a>Azure CDN'yi etkinleştirme HTTP/2 desteği

Şu anda, HTTP/2 desteği tüm Azure CDN profilleri için etkin değil. Müşterilerden daha fazla eylem gerekmiyor.

## <a name="next-steps"></a>Sonraki Adımlar

HTTP/2 avantajlarını iş başında görmek için bkz: [bu tanıtım akamai'den](https://http2.akamai.com/demo).

HTTP/2 hakkında daha fazla bilgi için aşağıdaki kaynakları ziyaret edin:

*   [HTTP/2 belirtimi giriş sayfası](https://http2.github.io/)
*   [Resmi HTTP/2 SSS](https://http2.github.io/faq/)
*   [Akamai tarafından sunulan HTTP/2 bilgi](https://http2.akamai.com/)

Azure CDN'ın kullanılabilir özellikleri hakkında daha fazla bilgi için bkz: [Azure CDN'ye genel bakış](https://azure.microsoft.com/documentation/articles/cdn-overview/).