---
title: Konuşmacı Tanıma nedir?
titlesuffix: Azure Cognitive Services
description: Konuşmacı Tanıma API'si ile konuşmacı doğrulama ve konuşmacı belirleme için gelişmiş algoritmalar kullanın.
services: cognitive-services
author: dwlin
manager: cgronlun
ms.service: cognitive-services
ms.component: speaker-recognition
ms.topic: overview
ms.date: 03/20/2016
ms.author: dwlin
ms.openlocfilehash: 13a95aff8b2b0d5dad0574e6107958a20576702a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227342"
---
# <a name="speaker-recognition-api"></a>Konuşmacı Tanıma API’si

Konuşmacı Tanıma API’lerine hoş geldiniz. Konuşmacı Tanıma API’leri, konuşmacı doğrulama ve konuşmacı belirleme için en gelişmiş algoritmaları sağlayan bulut tabanlı API’lerdir. Konuşmacı Tanıma, iki kategoriye ayrılabilir: konuşmacı doğrulama ve konuşmacı belirleme.


## <a name="speaker-verification"></a>Konuşmacı Doğrulama


Ses, tıpkı parmak izi gibi bir kişiyi tanımlamak için kullanılabilen benzersiz özelliklere sahiptir.  Erişim denetimi ve kimlik doğrulama senaryoları için sinyal olarak ses kullanımı, yeni bir yenilikçi araç olarak ortaya çıkmış olup temelde müşteriler için kimlik doğrulaması deneyimini kolaylaştıran bir üst düzey güvenlik sunar.

Konuşmacı Doğrulama API’leri, ses veya konuşmalarını kullanarak otomatik şekilde kullanıcıları doğrulayabilir ve kullanıcıların kimliğini doğrulayabilir.

### <a name="enrollment"></a>Kayıt

Konuşmacı doğrulama için kayıt, metne bağımlıdır; başka bir deyişle, konuşmacıların hem kayıt hem de doğrulama aşamalarında kullanılacak belirli bir parolayı seçmesi gerekir. 

Kayıt sırasında konuşmacının belirli bir tümceciği söylerken sesi kaydedilir daha sonra birçok özellik ayıklanır ve seçilen tümcecik tanınır. Ayıklanan özellikler ve seçilen tümcecik birlikte benzersiz bir ses imzasını oluşturur.

### <a name="verification"></a>Doğrulama
###
Doğrulama aşamasında bir giriş sesi ve tümcecik, kaydın ses imzası ve tümceciği ile karşılaştırılarak aynı kişiden olup olmadığı ve doğru tümceciğin söylenip söylenmediğini doğrulanır.

Konuşmacı doğrulama hakkında daha fazla ayrıntı için lütfen [Konuşmacı - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) API’sine bakın.

## <a name="speaker-identification"></a>Konuşmacı Belirleme

Konuşmacı Belirleme API’leri, bir grup olası konuşmacı grubu varken ses dosyasında konuşan kişiyi otomatik olarak belirleyebilir. Giriş sesi, sağlanan konuşmacı grubu ile eşlenir ve bir eşleşme bulunması durumunda, konuşmacının kimliği döndürülür.

Tüm konuşmacıların seslerinin sisteme kaydolması ve ses izlerinin oluşturulması için bir kayıt işleminden geçmesi gerekir.


### <a name="enrollment"></a>Kayıt

Konuşmacı belirleme için kayıt metinden bağımsızdır; başka bir deyişle, konuşmacının ses kaydında söyleyecekleri üzerinde bir kısıtlama yoktur. Konuşmacının sesi kaydedilir ve benzersiz bir ses imzası oluşturmak için birçok özellik ayıklanır. 


### <a name="recognition"></a>Tanıma

Tanıma sırasında, olası konuşmacı grubu ile birlikte, bilinmeyen konuşmacının sesi de sağlanır. Sesin kime ait olduğunu belirlemek için giriş sesi, tüm konuşmacılarla karşılaştırılır ve bir eşleşme bulunursa konuşmacının kimliği döndürülür.


Konuşmacı belirleme hakkında daha fazla ayrıntı için lütfen [Konuşmacı - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e) API’sine bakın.
