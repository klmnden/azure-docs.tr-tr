---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132545"
---
Bu örnek kullanılmasına [Twilio](https://www.twilio.com/) hizmeti bir cep telefonunuza SMS mesajları gönderebilir. Azure işlevleri için Twilio destek zaten [Twilio bağlama](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), ve örnek bu özelliği kullanır.

İhtiyacınız olan ilk şey bir Twilio hesabıdır. Ücretsiz, oluşturabilirsiniz https://www.twilio.com/try-twilio. Hesabınızı edindikten sonra aşağıdaki üç ekleme **uygulama ayarları** işlev uygulamanız için.

| Uygulama ayarı adı | Değer Açıklama |
| - | - |
| **TwilioAccountSid**  | Twilio hesabınızın SID'sini |
| **TwilioAuthToken**   | Twilio hesabınız için kimlik doğrulama belirteci |
| **TwilioPhoneNumber** | Twilio hesabınızla ilişkili telefon numarası. Bu, SMS mesajları göndermek için kullanılır. |