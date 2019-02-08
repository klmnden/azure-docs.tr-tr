---
title: Bing otomatik öneri sık sorulan sorular (SSS) - API'si
titlesuffix: Azure Cognitive Services
description: Bing otomatik öneri API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: HeidiSteen
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: cc14ceafaf0d9dd913cdef3864b0391f05d721e4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882259"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Sorulan sorular (SSS) hakkında Bing otomatik öneri API'si
 
 Kavramları, kod ve Azure Bilişsel hizmetler için otomatik öneri API'si için ilgili senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Bing otomatik öneri API'si JavaScript'ten çağırırken isteğe bağlı istemci üst bilgileri nasıl alırım?

Aşağıdaki üst bilgiler isteğe bağlıdır, ancak bunları gerektiği gibi ele almanız gerektiğini öneririz. Bu üst bilgiler, Bing otomatik öneri API'si daha doğru sonuçlar döndürebilir yardımcı olur.

- X arama konumu
- X MSEdge ClientID
- X-MSEdge-ClientIP

Bing otomatik öneri API'si JavaScript'ten çağırdığınızda, ancak, tarayıcınızın yerleşik güvenlik özellikleri, bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için Bing otomatik öneri API'si isteği bir CORS Ara sunucu aracılığıyla yapabilirsiniz. Böyle bir ara sunucu yanıtı sahip bir `Access-Control-Expose-Headers` üst bilgi, beyaz yanıt üst bilgileri ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nun yükleneceği kolaydır bizim [öğretici uygulama](tutorials/autosuggest.md) isteğe bağlı istemci üstbilgileri erişmek için. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing otomatik öneri API'si uç nokta HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? İsteme veya bunun için oy göz önünde bulundurun bizim [User Voice web sitesi](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Ayrıca bkz.

- [Yığın taşması: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
