---
title: Bing otomatik öneri sık sorulan sorular (SSS) - API'si
titlesuffix: Azure Cognitive Services
description: Bing otomatik öneri API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 84f1b0555922119e9de4addc3d51ac233e7bae65
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831373"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Sorulan sorular (SSS) hakkında Bing otomatik öneri API'si
 
 Kavramları, kod ve Azure Bilişsel hizmetler için otomatik öneri API'si için ilgili senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Bing otomatik öneri API'si JavaScript'ten çağırırken isteğe bağlı istemci üst bilgileri nasıl alırım?

Aşağıdaki üst bilgiler isteğe bağlıdır, ancak bunları gerektiği gibi ele almanız gerektiğini öneririz. Bu üst bilgiler, Bing otomatik öneri API'si daha doğru sonuçlar döndürebilir yardımcı olur.

- X arama konumu
- X MSEdge ClientID
- X MSEdge Clientıp

Bing otomatik öneri API'si JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri, bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için Bing otomatik öneri API'si isteği bir CORS Ara sunucu aracılığıyla yapabilirsiniz. Böyle bir ara sunucu yanıtı sahip bir `Access-Control-Expose-Headers` üst bilgi, beyaz yanıt üst bilgileri ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nun yükleneceği kolaydır bizim [öğretici uygulama](tutorials/autosuggest.md) isteğe bağlı istemci üstbilgileri erişmek için. Bu, zaten yoksa, birinci [Node.js yükleme](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing otomatik öneri API'si uç nokta HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Son olarak, CORS Ara sunucusu aşağıdaki komutla başlatın:

    cors-proxy-server

Öğretici uygulamayı kullanırken komut penceresini açık bırakın; pencereyi kapattıktan proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde artık görebilirsiniz `X-MSEdge-ClientID` üst bilgisi (diğerlerinin arasında) ve her istek için aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? İsteme veya bunun için oy göz önünde bulundurun bizim [User Voice web sitesi](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Ayrıca bkz.

- [Yığın taşması: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
