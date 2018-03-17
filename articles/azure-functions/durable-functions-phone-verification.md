---
title: "İnsan etkileşimi ve zaman aşımlarına dayanıklı işlevlerinde - Azure"
description: "İnsan etkileşimi ve zaman aşımlarına dayanıklı işlevleri uzantısı'nda Azure işlevleri için nasıl ele alınacağını öğrenin."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: e0b919ae5ef0639c8afdc5f9b006d899c8dbc4c1
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Dayanıklı işlevlerinde - telefon doğrulama örnek insan etkileşimi

Bu örnek nasıl oluşturulacağını gösteren bir [dayanıklı işlevleri](durable-functions-overview.md) insan etkileşimi içerir düzenleme. Gerçek bir insanın otomatik işlemde söz konusu olduğunda, işlem kişiye bildirimler göndermek ve yanıtları zaman uyumsuz olarak almak mümkün olması gerekir. Kişinin kullanılamıyor olasılığı için de izin vermelidir. (Bu son zaman aşımları burada önemli hale gelmiştir parçasıdır.)

Bu örnek bir SMS telefonla doğrulama sistemi uygular. Bu tür akışları genellikle bir müşterinin telefon numarasını doğrulanırken veya çok faktörlü kimlik doğrulaması (MFA) için kullanılır. Tüm uygulama yapıldığından bu güçlü bir örnektir birkaç küçük işlevlerini kullanma. Bir veritabanı gibi bir dış veri depo gereklidir.

## <a name="prerequisites"></a>Önkoşullar

* ' Ndaki yönergeleri izleyin [yükleme dayanıklı işlevleri](durable-functions-install.md) örneğini kurmak için.
* Bu makalede, zaten gitti varsayar [Hello dizisi](durable-functions-sequence.md) örnek gözden geçirme.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Telefon doğrulama, son kullanıcılar, uygulamanızın istenmeyen posta gönderenlerin değildir ve kimlerin kendilerine olduklarından emin doğrulamak için kullanılır. Çok faktörlü kimlik doğrulama kullanıcı hesapları korsanların korumak için bir ortak kullanım durumdur. Kendi telefon doğrulama uygulama ile gerektirdiği iştir bir **durum bilgisi olan etkileşim** İnsan olma ile. Bir son kullanıcı biraz kod (örneğin, 4 basamaklı bir sayı) genellikle sağlanır ve yanıtlaması gerekir **makul bir sürede**.

(Diğer platformlarda diğer birçok bulut uç gibi) normal Azure işlevleri durum bilgisiz, açıkça yönetme bu tür etkileşimler içeren için bir veritabanı veya bazı diğer kalıcı durum dışarıdan depolamak. Ayrıca, etkileşim birlikte Eşgüdümlü olabilecek birden çok işlevlerini parçalanmış gerekir. Örneğin, bir kodu karar verme, herhangi bir yerde kalıcı yapma ve kullanıcının telefonuna göndermek için en az bir işlevi gerekir. Ayrıca, kullanıcıdan yanıt ve şekilde kod doğrulama yapmak için geri özgün işlev çağrısı için eşlemek için en az bir işlevi gerekir. Zaman aşımı güvenliğini sağlamak için önemli bir özelliği de olur. Bu hızlı bir şekilde oldukça karmaşık bir HAL alabilir.

Dayanıklı işlevler kullandığınızda bu senaryo karmaşıklığını önemli ölçüde azalır. Bu örnekte göreceğiniz gibi bir orchestrator işlevi kolayca ve tüm dış veri depoları içeren olmadan durum bilgisi olan etkileşim yönetebilirsiniz. Orchestrator işlevleri olduğundan *dayanıklı*, bu etkileşimli akışlar da yüksek oranda güvenilir.

## <a name="configuring-twilio-integration"></a>Twilio tümleştirmesini yapılandırma

[!INCLUDE [functions-twilio-integration](../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamasında aşağıdaki işlevleri açıklanmaktadır:

* **E4_SmsPhoneVerification**
* **E4_SendSmsChallenge**

Aşağıdaki bölümlerde, Azure portal geliştirme için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.
 
## <a name="the-sms-verification-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>SMS doğrulama düzenleme (Visual Studio Code ve Azure portal örnek kodu) 

**E4_SmsPhoneVerification** işlevini kullanan standart *function.json* orchestrator işlevler için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/function.json)]

İşlev uygulayan kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/run.csx)]

Başladıktan sonra bu orchestrator işlevi şunları yapar:

1. Olur, bir telefon numarasını alır *Gönder* SMS bildirim.
2. Çağrıları **E4_SendSmsChallenge** bir SMS iletisi göndermek için beklenen 4 basamaklı sınama kodu döndürür ve kullanıcı yedekleyin.
3. Dayanıklı bir Zamanlayıcı bu Tetikleyicileri geçerli zamandan 90 saniye oluşturur.
4. Zamanlayıcı ile paralel olarak bekler bir **SmsChallengeResponse** kullanıcıdan olay.

Kullanıcı dört rakamlı kodu içeren bir SMS mesajı alır. Bu aynı 4 basamaklı kodu doğrulama işlemini tamamlamak için yeniden orchestrator işlevi örneğine göndermek için 90 saniye sahiptirler. Yanlış kod gönderirseniz, bunlar sağa (aynı 90 saniyelik pencereye) almak için ek bir üç çalıştığında alın.

> [!NOTE]
> İlk, ancak bu orchestrator belirgin olmayabilir işlev tamamen belirleyici değil. Bunun nedeni, `CurrentUtcDateTime` özelliği Zamanlayıcı zaman aşımı süresini hesaplamak için kullanılır ve bu özellik her yeniden yürütme bu noktada orchestrator kodda aynı değeri döndürür. Bu aynı emin olmak önemlidir `winner` sonuçları için yinelenen her çağrısından `Task.WhenAny`.

> [!WARNING]
> Önemlidir [bir CancellationTokenSource kullanarak zamanlayıcılar iptal](durable-functions-timers.md) artık bunları ne zaman bir sınama yanıtı kabul yukarıdaki örnekte olduğu gibi süresi dolmak üzere ihtiyacınız varsa.

## <a name="send-the-sms-message"></a>SMS mesajı gönderme

**E4_SendSmsChallenge** işlevi, son kullanıcı için 4 basamaklı koduyla SMS mesajı göndermek için Twilio bağlama kullanır. *Function.json* şu şekilde tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/function.json)]

Ve 4 basamaklı sınama kodu oluşturur ve SMS mesajı gönderir. kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/run.csx)]

Bu **E4_SendSmsChallenge** işlevi yalnızca çağrıldığından bir kez işlem bozulsa veya yeniden. Birden çok SMS iletileri alma son kullanıcı istemediğiniz iyi olmasıdır. `challengeCode` Orchestrator işlevi her zaman doğru kodun ne olduğunu bilmesi için değer otomatik olarak kalıcı döndürür.

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki örnekte dahil HTTP tetiklemeli işlevleri kullanarak, aşağıdaki HTTP POST isteği göndererek orchestration başlatabilirsiniz:

```
POST http://{host}/orchestrators/E4_SmsPhoneVerification
Content-Length: 14
Content-Type: application/json

"+1425XXXXXXX"
```
```
HTTP/1.1 202 Accepted
Content-Length: 695
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"id":"741c65651d4c40cea29acdd5bb47baf1","statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

Orchestrator işlevi sağlanan telefon numarasını alır ve hemen rastgele oluşturulmuş 4 basamaklı doğrulama kodunu içeren bir SMS mesajı gönderir &mdash; Örneğin, *2168*. İşlev ardından yanıt 90 saniye bekler.

Kodla yanıt vermek için kullanabileceğiniz `RaiseEventAsync` iç işlev veya çağırma **sendEventUrl** yukarıda değiştirme 202 yanıt başvurulan HTTP POST Web kancası `{eventName}` olayın adı ile `SmsChallengeResponse`:

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Süreölçer süresi dolmadan önce bu gönderirseniz, orchestration tamamlandıktan ve `output` alan ayarlanmış `true`, başarılı bir doğrulama belirten.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```
```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Sona Zamanlayıcı izin vermek veya dört kez yanlış kodu girerseniz, sorgu durumunu ve bkz bir `false` çıkışı, orchestration işlevi telefon doğrulama başarısız olduğunu gösteren.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyasında bir Visual Studio projesi olarak orchestration şöyledir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek bazı dayanıklı İşlevler, gelişmiş özelliklerini özellikle gösterilmiştir `WaitForExternalEvent` ve `CreateTimer`. Bunlar birlikte birleştirilmesi gördünüz `Task.WaitAny` genellikle gerçek kişilerle etkileşim için yararlı bir güvenilir zaman aşımı sistem uygulamak için. Bir dizi kapsamlı kapsamını belirli konular teklif makaleleri okuyarak dayanıklı işlevleri kullanma hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Serideki ilk makale gidin](durable-functions-bindings.md)
