---
title: Duygu tanıma API'si genel bakış | Microsoft Docs
description: Bilişsel hizmetler duygu tanıma API'si ile daha kişiselleştirilmiş uygulamaları geliştirmek için Microsoft modern, bulut tabanlı duygu tanıma algoritmasını kullanın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 8383370cba3f78060e809f444f4ad3dab7380f4e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352564"
---
# <a name="what-is-emotion-api"></a>Duygu tanıma API'si nedir?

> [!IMPORTANT]
> Duygu tanıma API'si 30 Ekim 2017 üzerinde kullanımdan kaldırılmıştır. İşlevsellik şimdi parçası olan [yüz API](https://docs.microsoft.com/en-us/azure/cognitive-services/face/).

Microsoft'un bulut tabanlı duygu tanıma algoritması ile daha kişiselleştirilmiş uygulamalar oluşturmanıza olanak sağlayan Microsoft duygu tanıma API'si Hoş Geldiniz.

### <a name="emotion-recognition"></a>Duygu tanıma

Duygu tanıma API'si beta görüntünün bir girdi olarak alır ve yüz API'SİNDEN yüz için sınırlayıcı kutu yanı sıra, her yüz görüntüdeki duygular kümesi arasında güven döndürür. Algılanan duygular mutluluk, sadness, beklenmedik biçimde, anger, Korku, contempt, disgust veya nötr olabilir. Aynı temel yüz ifadeleri, bu duygular cross-culturally ve evrensel iletilen burada duygu tanıma API'si tarafından tanımlanır. 

**Sonuçları yorumlayarak:** 

Puanları normalleştirilmiş olarak duygu API'sinden sonuçları yorumlayarak içinde algılanan duygu yüksek puanı duygu olarak yorumlanması gerektiğini bir toplanacak. Kullanıcılar kendi gereksinimlerine bağlı olarak, uygulama içinde daha yüksek bir güven eşiği ayarlamaya seçebilirsiniz. 

API Başvurusu duygu algılama hakkında daha fazla bilgi için bkz: 
  * Temel: bir kullanıcı zaten yüz API çağırdı ve varsa, bunlar yüz dikdörtgenin giriş olarak gönderme temel katmana kullanın. [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standart: bir kullanıcı bir yazıtipi dikdörtgen göndermez, standart modda kullanmalısınız.  [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Akış video duygu tanıma API'si ile yorumlama hakkında bir örnek için bkz: [gerçek zamanlı analiz videolar nasıl](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
