---
title: Sık sorulan sorular (SSS) hakkında Azure API otomatik öneri | Microsoft Docs
description: Azure üzerinde Azure Bilişsel hizmetleri otomatik öneri API ilgili sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 00b91728bcfec52ff30697f080d5c2619bab79a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354473"
---
# <a name="frequently-asked-questions-faq-about-autosuggest-api-cognitive-services"></a>Sorulan sorular (SSS) hakkında otomatik öneri API (Bilişsel hizmetler)
 
 Kavramları, kod ve otomatik öneri API'sine Azure Bilişsel hizmetler için ilgili senaryoları hakkında sık sorulan soruların yanıtlarını bulun.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Bing otomatik öneri API JavaScript'ten çağrılırken isteğe bağlı istemci üstbilgileri nasıl sağlarım?

Aşağıdaki üst bilgiler isteğe bağlıdır, ancak, bunları gerektiği gibi davran olduğunu öneririz. Bu üstbilgileri Bing otomatik öneri daha kesin sonuçlar döndürecek API yardımcı olur.

- X arama konumu
- X MSEdge ClientID
- X MSEdge ClientIP

Bing otomatik öneri API JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri, bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için bir CORS Ara sunucu aracılığıyla Bing otomatik öneri API isteği yapabilirsiniz. Bu tür bir proxy yanıttan sahip bir `Access-Control-Expose-Headers` üstbilgi bu whitelists yanıt üstbilgilerini ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nu yüklemek kolayca bizim [öğretici uygulama](tutorials/autosuggest.md) isteğe bağlı istemci üstbilgileri erişmek için. Zaten yoksa, birinci [Node.js yüklemek](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing otomatik öneri API uç noktası için HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Son olarak, aşağıdaki komutla CORS proxy başlatın:

    cors-proxy-server

Eğitmen uygulama kullanırken komut penceresini açık bırakın; pencereyi proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde şimdi görebilirsiniz `X-MSEdge-ClientID` üstbilgi (Diğerlerinin yanında) ve her istek için aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında soru mi? İsteme veya onun için oy göz önünde bulundurun bizim [kullanıcı sesi web sitesi](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Ayrıca bkz.

- [Yığın taşması: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
