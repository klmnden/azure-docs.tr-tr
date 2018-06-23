---
title: Konuşmacı tanıma API'si | Microsoft Docs
description: Konuşmacı doğrulama ve Bilişsel hizmetler Konuşmacı tanıma API'si Konuşmacı kimliği için gelişmiş algoritmalar kullanın.
services: cognitive-services
author: dwlin
manager: zhang
ms.service: cognitive-services
ms.component: speaker-recognition
ms.topic: article
ms.date: 03/20/2016
ms.author: dwlin
ms.openlocfilehash: 6d5e4e4bbe0cb5e57d2556f680ffcf8d16ee1818
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352168"
---
# <a name="speaker-recognition-api"></a>Konuşmacı Tanıma API’si

Microsoft Konuşmacı tanıma API'leri hoşgeldiniz. Konuşmacı tanıma API'leri Konuşmacı doğrulama ve Konuşmacı kimliği için en gelişmiş algoritmalar sağlayan bulut tabanlı apı'leridir. Konuşmacı tanıma iki kategoride bölünmüş: Konuşmacı doğrulama ve Konuşmacı kimliği.


## <a name="speaker-verification"></a>Konuşmacı Doğrulama


Ses parmak izi gibi bir kişiyi tanımlamak için kullanılan benzersiz özelliklere sahiptir.  Erişim denetimi ve kimlik doğrulama senaryoları için sinyal ortaya çıktı yeni bir yenilikçi aracı gibi sesli – müşteriler için kimlik doğrulama deneyimi basitleştirir güvenlik aslında bir düzeye desteklemekteyiz.

Konuşmacı doğrulama API'leri otomatik olarak doğrulayın ve bunların ses veya konuşma kullanan kullanıcıları doğrula.

### <a name="enrollment"></a>Kayıt

Konuşmacı doğrulama için kayıt metin kayıt ve doğrulama aşamaları sırasında kullanmak için bir özel parola deyimi seçmenizi konuşmacılar gereksinim anlamına gelir, bağlıdır. 

Kayıt, konuşmacı sesli belirli bir deyimi belirten kaydedilir ve daha sonra birtakım özellikleri ayıklanır ve seçilen tümcecik tanınır. Birlikte ayıklanan özellikleri ve seçilen form benzersiz sesli imza tümce.

### <a name="verification"></a>Doğrulama
###
Doğrulama, bir giriş ses ve tümcecik kayıt ait ses imza ve tümcecik karşı karşılaştırılır – aynı kişiden olup olmadığı ve doğru tümcecik belirten varsa doğrulamak için.

Konuşmacı doğrulama hakkında daha fazla ayrıntı için lütfen API'sine başvurun [Konuşmacı - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## <a name="speaker-identification"></a>Konuşmacı Tanıma

Konuşmacı tanıma API'leri olası konuşmacılar grubu verilen bir ses dosyası konuşarak kişi otomatik olarak tanımlayabilirsiniz. Giriş Ses konuşmacılar sağlanan grup karşı eşleştirilmiş ve bulunan eşleşmesi durumda Konuşmacı kimlik döndürülür.

Tüm konuşmacılar ilk sisteme kayıtlı kullanıcıların sesli almak için bir kayıt işleminde gidin ve oluşturulan bir sesli yazdırın olması gerekir.


### <a name="enrollment"></a>Kayıt

Konuşmacı tanımlama için kayıt metin, ses ne Konuşmacı diyor üzerinde herhangi bir kısıtlama olduğu anlamına gelir, bağımsızdır. Konuşmacı sesli kaydedilir ve benzersiz sesli imza oluşturmak için bir dizi özellik ayıklanır. 


### <a name="recognition"></a>Oney Belgesi

Konuşmacılar, olası grubunun birlikte bilinmeyen Konuşmacı ses tanıma sırasında sağlanır. Giriş sesli karşılaştırıldığında, sesli belirlemek için tüm konuşmacılar karşı olduğunu ve Konuşmacı kimliğini bulunan bir eşleşme varsa, döndürülür.


Konuşmacı tanımlama hakkında daha fazla ayrıntı için lütfen API'sine başvurun [Konuşmacı - kimliği](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).
