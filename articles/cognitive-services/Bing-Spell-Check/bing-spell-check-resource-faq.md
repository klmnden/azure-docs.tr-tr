---
title: Sorulan sorular hakkında Bing yazım denetimi APİ'si
titlesuffix: Azure Cognitive Services
description: Azure'da Bing yazım denetimi API'si hakkında sık sorulan sorulara yanıtlar alın.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: e6662ffcbab9ea274a67bc4437ca1600f1625ff1
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801514"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Sorulan sorular hakkında Bing yazım denetimi APİ'si

 Kavramları, kod ve Bing yazım denetimi API'si, Azure üzerinde Microsoft Bilişsel hizmetler için ilgili senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Bing yazım denetimi API'si JavaScript'ten çağırırken isteğe bağlı istemci üst bilgileri nasıl alırım?

Aşağıdaki üst bilgiler isteğe bağlıdır, ancak bunları gerektiği gibi ele almanız gerektiğini öneririz. Bu üst bilgiler, Bing yazım denetimi API'si daha doğru sonuçlar yardımcı olur.

- X arama konumu
- X MSEdge ClientID
- X MSEdge Clientıp

Bing yazım denetimi API'si JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri, bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için Bing yazım denetimi API'si isteği bir CORS Ara sunucu aracılığıyla yapabilirsiniz. Böyle bir ara sunucu yanıtı sahip bir `Access-Control-Expose-Headers` üst bilgi, beyaz yanıt üst bilgileri ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nun yükleneceği kolaydır [öğretici uygulama](tutorials/spellcheck.md) isteğe bağlı istemci üstbilgileri erişmek için. Bu, zaten yoksa, birinci [Node.js yükleme](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing yazım denetimi API'si uç nokta HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Son olarak, CORS Ara sunucusu aşağıdaki komutla başlatın:

    cors-proxy-server

Öğretici uygulamayı kullanırken komut penceresini açık bırakın; pencereyi kapattıktan proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde artık görebilirsiniz `X-MSEdge-ClientID` üst bilgisi (diğerlerinin arasında) ve her istek için aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? İsteme veya bunun için oy göz önünde bulundurun [UserVoice web sitesi](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
