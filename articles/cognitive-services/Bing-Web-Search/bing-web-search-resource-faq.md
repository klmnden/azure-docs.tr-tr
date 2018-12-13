---
title: Sık sorulan sorular (SSS) - Bing Web araması API'si
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si kullanma hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: troubleshooting
ms.date: 10/06/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 9098ddc780e6adefa8430fe601e3f4d06634972a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252992"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

 Kavramları, kod ve Azure üzerinde Microsoft Bilişsel hizmetler için Bing Web araması API'si için ilgili senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="response-headers-in-javascript"></a>JavaScript içinde yanıt üstbilgileri

Aşağıdaki üst bilgiler, Bing Web araması API'si alınan yanıtları oluşabilir.

|||
|-|-|
|`X-MSEdge-ClientID`|Bing kullanıcıya atanmış benzersiz kimliği|
|`BingAPIs-Market`|İsteği gerçekleştirmek için kullanılan Pazar|
|`BingAPIs-TraceId`|Bu istek (desteği gibi) için Bing API'si sunucusundaki günlük girişi|

İstemci kimliği kalıcı hale getirmek ve sonraki istekleri ile döndürmek özellikle önemlidir. Bunu yaptığınızda, arama, arama sonuçlarını sıralamasını bağlamını geçmiş kullanın ve tutarlı bir kullanıcı deneyimi de sağlar.

Bing Web araması API'si JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri (CORS), bu üstbilgi değerlerini erişmesini engelleyebilir.

Üst bilgilerine erişmek için Bing Web araması API'si isteği bir CORS Ara sunucu aracılığıyla yapabilirsiniz. Böyle bir ara sunucudan gelen yanıtta, yanıt üst bilgilerini beyaz listeye alan ve JavaScript’in kullanımına sunan `Access-Control-Expose-Headers` üst bilgisi bulunur.

İzin vermek için bir CORS Ara Sunucusu'nun yükleneceği kolaydır bizim [öğretici uygulama](tutorial-bing-web-search-single-page-app.md) isteğe bağlı istemci üstbilgileri erişmek için. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing Web araması API'si uç nokta HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

## <a name="response-headers-in-production"></a>Üretimde yanıt üstbilgileri

Önceki yanıtında açıklanan CORS Ara yaklaşımı, geliştirme, test ve öğrenme için uygundur.

Ancak, bir üretim ortamında, Bing Web araması API'si kullanan Web sayfası olarak aynı etki alanında bir sunucu tarafı betik barındırmamalısınız. Bu betik aslında istek üzerine Web sayfası JavaScript API çağrılarını yapın ve istemciye üst bilgiler dahil olmak üzere tüm sonuçları geçirin. Bir kaynak (sayfa ve komut dosyası) iki Kaynakları paylaşarak CORS oyuna gelmez ve özel üstbilgi JavaScript Web sayfasındaki erişilebilir.

Sunucu tarafı betik ihtiyaç duyduğu yalnızca bu yaklaşım ayrıca API anahtarınızı genel maruz kalma riskinizi beri önler. Betik, isteğin yetkilendirilip emin olmak için başka bir yöntemi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? İsteme veya bunun için oy göz önünde bulundurun bizim [User Voice web sitesi](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Ayrıca bkz.

 [Yığın taşması: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/bing-api)
