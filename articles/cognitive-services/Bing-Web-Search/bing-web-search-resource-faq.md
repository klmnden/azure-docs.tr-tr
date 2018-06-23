---
title: Azure üzerinde Bing Web araması API hakkında sorular (SSS) sık sorulan | Microsoft Docs
description: Azure üzerinde Microsoft Bilişsel Hizmetleri Bing Web arama API ilgili sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 321f571c48f2231d1ced43848cdefd17adaa1a08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351671"
---
# <a name="frequently-asked-questions-faq-about-bing-web-search-api-cognitive-services"></a>Bing Web arama API (Bilişsel hizmetler) hakkında sık sorulan sorular (SSS)
 
 Kavramları, kod ve Microsoft Azure üzerinde Bilişsel hizmetler için Bing Web arama API'sine ilgili senaryoları hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="response-headers-in-javascript"></a>JavaScript yanıt üstbilgileri

Aşağıdaki üst bilgiler Bing Web arama API yanıtlarının ortaya çıkabilir.

|||
|-|-|
|`X-MSEdge-ClientID`|Bing kullanıcıya atanan benzersiz kimliği|
|`BingAPIs-Market`|İsteği gerçekleştirmek için kullanılan Pazar|
|`BingAPIs-TraceId`|Bu istek (için destek) için Bing API'si sunucusundaki günlük girişi|

İstemci Kimliğini kalıcı hale getirmek ve sonraki istekleri ile dönmek özellikle önemlidir. Bunu yaptığınızda, arama arama sonuçlarını sıralamasını, bağlam geçmiş kullanın ve tutarlı bir kullanıcı deneyimi de sağlar.

Bing Web arama API JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri (CORS), bu üstbilgi değerlerini erişmesini engelleyebilir.

Üstbilgileri erişmek için bir CORS Ara sunucu aracılığıyla Bing Web arama API isteği yapabilirsiniz. Bu tür bir proxy yanıttan sahip bir `Access-Control-Expose-Headers` üstbilgi bu whitelists yanıt üstbilgilerini ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nu yüklemek kolayca bizim [öğretici uygulama](tutorial-bing-web-search-single-page-app.md) isteğe bağlı istemci üstbilgileri erişmek için. Zaten yoksa, birinci [Node.js yüklemek](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing Web arama API uç noktası için HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Son olarak, aşağıdaki komutla CORS proxy başlatın:

    cors-proxy-server

Eğitmen uygulama kullanırken komut penceresini açık bırakın; pencereyi proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde şimdi görebilirsiniz `X-MSEdge-ClientID` üstbilgi (Diğerlerinin yanında) ve her istek için aynı olduğunu doğrulayın.

## <a name="response-headers-in-production"></a>Üretimde yanıt üstbilgileri

Önceki yanıtında açıklanan CORS proxy sınama ve öğrenme geliştirme için uygun bir yaklaşımdır. 

Bir üretim ortamında, ancak, aynı etki alanında Bing Web arama API'sini kullanan Web sayfası olarak sunucu tarafı komut dosyası ana. Bu komut dosyası gerçekten Web sayfası JavaScript gelen istek üzerine API çağrıları yapmak ve istemciye üstbilgileri dahil olmak üzere tüm sonuçları geçmesi gerekir. İki kaynak (sayfa ve komut dosyası) bir kaynak Paylaş beri CORS oyuna gelmez ve özel üstbilgi acessible Web sayfasında JavaScript olduğundan. 

Sunucu tarafı komut dosyası, gereken yalnızca bu yaklaşım da API anahtarınıza ortak maruz beri önler. Komut dosyası isteğine yetki emin olmak için başka bir yöntemi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında soru mi? İsteme veya onun için oy göz önünde bulundurun bizim [kullanıcı sesi web sitesi](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Ayrıca bkz.

 [Yığın taşması: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/bing-api)