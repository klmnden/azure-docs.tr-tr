---
title: Duygu tanıma API'si genel bakış | Microsoft Docs
description: Microsoft, bulut tabanlı son teknoloji ürünü duygu tanıma algoritması, Bilişsel hizmetler, duygu tanıma API'si ile daha kişiselleştirilmiş uygulamalar oluşturmak için kullanın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 210990b0f436fd75cb36e71ea28928c457a5232e
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573563"
---
# <a name="what-is-emotion-api"></a>Duygu Tanıma API'si nedir?

> [!IMPORTANT]
> Duygu tanıma API'si, 30 Ekim 2017'de kullanım dışı bırakıldı. İşlevi, şimdi parçasıdır [yüz tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/face/).

Microsoft'un bulut tabanlı duygu tanıma algoritması daha kişiselleştirilmiş uygulamalar oluşturmanıza olanak tanıyan Microsoft duygu tanıma API'si, Hoş Geldiniz.

### <a name="emotion-recognition"></a>Duygu tanıma

Duygu tanıma API'si beta, girdi olarak bir görüntü alır ve yüz tanıma API'si ile güvenle yüz için bir sınırlayıcı kutu yanı sıra resimdeki her yüz için duyguları bir dizi döndürür. Algılanan duygular mutluluk, üzüntü, şaşkınlık, kızgınlık, Korku, küçümseme, iğrenme veya nötr ' dir. Bu duyguları aynı temel yüz ifadelerini, kültürler arası ve evrensel olarak iletildiği burada duygu tanıma API'si tarafından tanımlanır. 

**Sonuçları yorumlayarak destek sağlama:** 

Puanları normalleştirilmiş olarak duygu tanıma API'si sonuçlarını yorumlama içinde algılanan duygu tanıma duygu en yüksek puanı olarak yorumlanıp birine toplanacak. Kullanıcılar daha yüksek bir güven eşiği gereksinimlerine bağlı olarak, uygulama içinde ayarlamak tercih edebilirsiniz. 

API Başvurusu duygu algılama hakkında daha fazla bilgi için bkz: 
  * Temel: bir kullanıcı yüz tanıma API'sine zaten çağırdı, bunlar yüz dikdörtgeni giriş olarak gönderebilir ve temel katmanı kullanın. [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standart: bir kullanıcı yüz dikdörtgeni göndermez, standart modda kullanmalısınız.  [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Duygu tanıma API'si ile Akış videosunu yorumlama hakkında bir örnek için bkz. [videoları gerçek zamanlı analiz etme](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
