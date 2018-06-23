---
title: Sık sorulan sorular Bing hakkında yazım denetimi API'si - Azure Bilişsel hizmetler | Microsoft Docs
description: Bing yazım denetleme API'si hakkında genel soruların yanıtları için Azure üzerinde alın.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 87b1f3ed3e0aaa9f3c3c804dc9eac3ee60b4a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354947"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Bing yazım hakkında sorular API denetleyin sorulan

 Kavramları, kod ve Microsoft Azure üzerinde Bilişsel hizmetler için Bing yazım denetleme API ilgili senaryoları hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Bing yazım denetleme API JavaScript'ten çağrılırken isteğe bağlı istemci üstbilgileri nasıl sağlarım?

Aşağıdaki üst bilgiler isteğe bağlıdır, ancak, bunları gerektiği gibi davran olduğunu öneririz. Bu üstbilgileri Bing yazım denetleme API dönüş daha doğru sonuçlar yardımcı olur.

- X arama konumu
- X MSEdge ClientID
- X MSEdge ClientIP

Bing yazım denetleme API JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri, bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için bir CORS Ara sunucu aracılığıyla Bing yazım denetleme API isteği yapabilirsiniz. Bu tür bir proxy yanıttan sahip bir `Access-Control-Expose-Headers` üstbilgi bu whitelists yanıt üstbilgilerini ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nu yüklemek kolayca [öğretici uygulama](tutorials/spellcheck.md) isteğe bağlı istemci üstbilgileri erişmek için. Zaten yoksa, birinci [Node.js yüklemek](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing yazım denetleme API uç noktası için HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Son olarak, aşağıdaki komutla CORS proxy başlatın:

    cors-proxy-server

Eğitmen uygulama kullanırken komut penceresini açık bırakın; pencereyi proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde şimdi görebilirsiniz `X-MSEdge-ClientID` üstbilgi (Diğerlerinin yanında) ve her istek için aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında soru mi? İsteme veya onun için oy göz önünde bulundurun [UserVoice web sitesi](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
