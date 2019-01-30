---
title: Duygu Tanıma API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bulut tabanlı duygu tanıma algoritmalarını kullanarak daha kişisel uygulamalar oluşturabilirsiniz.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: overview
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 555b03be679b0d1ea61371d22ec9865e0e72b558
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215801"
---
# <a name="what-is-the-emotion-api"></a>Duygu Tanıma API'si nedir?

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Microsoft'un bulut tabanlı duygu tanıma algoritmasıyla daha kişisel uygulamalar derlemenizi sağlayan Microsoft Duygu Tanıma API'sine hoş geldiniz.

### <a name="emotion-recognition"></a>Duygu Tanıma

Duygu Tanıma API’sinin beta sürümü bir görüntüyü giriş olarak alır ve görüntüdeki her bir yüz için bir dizi duygu arasından seçilen güven puanlarının yanı sıra, Yüz Tanıma API’sinden yüzler için sınırlayıcı kutular döndürür. Algılanan duygular mutluluk, üzüntü, şaşkınlık, kızgınlık, korku, küçümseme, iğrenme veya nötr şeklindedir. Bu duygular aynı temel yüz ifadeleriyle farklı kültürler için evrensel olarak belirtilir ve Duygu Tanıma API'si tarafından tanımlanır.

**Sonuçları Yorumlama:**

Duygu Tanıma API'si sonuçlarını yorumlama sırasında algılanan duygunun en yüksek puana sahip olan duygu olarak yorumlanması gerekir. Bunun nedeni tek bir sonuç elde etmek için puanların normalleştirilmesidir. Kullanıcılar ihtiyaçlarına göre uygulamalarında daha yüksek bir güvenilirlik eşiği ayarlayabilir.

Duygu tanıma hakkında daha fazla bilgi için API Başvurusu sayfasına bakın:
  * Temel: Bir kullanıcı yüz tanıma API'sine zaten çağırdı, yüz dikdörtgeni girdi olarak gönderin ve temel katmanı kullanın. [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standart: Bir kullanıcı yüz dikdörtgeni göndermez, Standart mod kullanmanız gerekir.  [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Duygu Tanıma API'si ile video akışı yorumlama örneği için bkz. [Videoları gerçek zamanlı olarak analiz etme](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
