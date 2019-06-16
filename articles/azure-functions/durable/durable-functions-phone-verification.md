---
title: İnsan etkileşimi ve zaman aşımları dayanıklı işlevler - Azure
description: Azure işlevleri için insan etkileşimi ve dayanıklı işlevler uzantısını zaman aşımları nasıl ele alınacağını öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: cf43e29e967ee6f920eb38feb9c73d70f9621ea4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62123323"
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Dayanıklı işlevler - telefon doğrulama örneği, insan etkileşimi

Bu örnek nasıl oluşturulacağını gösterir. bir [dayanıklı işlevler](durable-functions-overview.md) insan etkileşimi içeren düzenleme. Gerçek bir insanın otomatik bir işlemde katılan her işlem kişiye bildirimler göndermek ve yanıtları zaman uyumsuz olarak almak mümkün olması gerekir. Ayrıca kişi kullanılamaz olma olasılığını için izin vermelidir. (Bu son zaman aşımları burada önemli hale gelmiştir parçasıdır.)

Bu örnek, bir SMS telefonla doğrulama sistemini kullanır. Bu tür akışlar, genellikle bir müşterinin telefon numarasını doğrularken veya multi-Factor authentication (MFA) için kullanılır. Tüm uygulama yapıldığı için bu güçlü bir örnektir birkaç küçük işlevlerini kullanma. Bir veritabanı gibi herhangi bir dış veri deposuna gereklidir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Telefon doğrulama, son kullanıcılar, uygulamanızın istenmeyen posta gönderenlere değildir ve bunların söyledikleri kim olduğunu doğrulamak için kullanılır. Multi-Factor authentication, kullanıcı hesaplarını korsanların korumaya yönelik yaygın bir kullanım örneği. Kendi telefon doğrulama uygulama ile onun gerektirdiği zorluktur bir **durum bilgisi olan etkileşimi** ile İnsan görüntülenir. Son kullanıcı, genellikle bazı kod (örneğin, 4 basamaklı bir sayı) sağlanır ve yanıt vermesi gereken **makul bir süre içinde**.

(Birçok diğer bulut uç noktaları diğer platformlarda olduğu gibi) normal Azure işlevleri durum bilgisiz olduğundan, açıkça yönetme bu tür etkileşimler içeren için bir veritabanı veya bazı diğer kalıcı durumu harici olarak depolamak. Ayrıca, etkileşim birlikte Eşgüdümlü olabileceğini birden çok işlevler bölünmesi gerekir. Örneğin, bir kod üzerinde karar, herhangi bir yerde kalıcı hale getirmeniz ve kullanıcının telefonuna göndermek için en az bir işlevi gerekir. Ayrıca, kullanıcıdan bir yanıt almak ve kod doğrulama yapmak için geri orijinal işlev çağrısı şekilde eşlemek için en az bir diğer işlevi gerekir. Bir zaman aşımı, ayrıca güvenliğini sağlamak için önemli bir yönü olur. Bu hızlı bir şekilde oldukça karmaşık bir HAL alabilir.

Dayanıklı işlevler kullandığınızda bu senaryonun karmaşıklığı önemli ölçüde azalır. Bu örnekte göreceğiniz gibi bir düzenleyici işlevi kolayca olmadan herhangi bir dış veri depoları içeren ve durum bilgisi olan etkileşimi yönetebilirsiniz. Orchestrator işlevleri olduğundan *dayanıklı*, bu etkileşimli akışlar da yüksek oranda güvenilir.

## <a name="configuring-twilio-integration"></a>Twilio tümleştirmesi yapılandırma

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>İşlevleri

Bu makalede örnek uygulama aşağıdaki işlevler anlatılmaktadır:

* **E4_SmsPhoneVerification**
* **E4_SendSmsChallenge**

Aşağıdaki bölümlerde, betik C# ve JavaScript için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

## <a name="the-sms-verification-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>SMS doğrulama düzenleme (Visual Studio Code ve Azure portalı örnek kodu)

**E4_SmsPhoneVerification** işlevini kullanan standart *function.json* orchestrator işlevleri için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/function.json)]

İşlev uygulayan kod şu şekildedir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/index.js)]

Başladıktan sonra bu Düzenleyici işlevi şunları yapar:

1. Olacak, bir telefon numarasını alır *Gönder* SMS bildirimi.
2. Çağrıları **E4_SendSmsChallenge** bir SMS iletisi göndermek için beklenen 4 basamaklı sınama kodu döndürür ve kullanıcı yedekleyin.
3. Dayanıklı bir Zamanlayıcı bu tetikleyiciler, geçerli zamandan 90 saniye oluşturur.
4. Zamanlayıcı ile paralel olarak bekler bir **SmsChallengeResponse** kullanıcıdan olay.

Kullanıcı dört rakamlı bir kod içeren bir SMS iletisi alır. Doğrulama sürecini tamamlamanız için geri orchestrator işlevi örneğine, aynı 4 rakamlı bir kod gönderilecek 90 saniye sahiptirler. Bunlar yanlış kod gönderirseniz, bunlar sağ (aynı 90 saniyelik pencereye) almak için ek bir üç deneme alın.

> [!NOTE]
> İlk, ancak bu orchestrator belirgin olmayabilir işlevi belirleyici tamamen. Bunun nedeni, `CurrentUtcDateTime` (.NET) ve `currentUtcDateTime` (JavaScript) özellikleri, Zamanlayıcı sona erme zamanını hesaplamak için kullanılır ve bu özellikler, her yeniden yürütme bu noktada orchestrator kod üzerinde aynı değeri döndürür. Bu aynı emin olmak önemlidir `winner` sonuçları yinelenen her çağrıdan `Task.WhenAny` (.NET) veya `context.df.Task.any` (JavaScript).

> [!WARNING]
> Önemlidir [zamanlayıcılar iptal](durable-functions-timers.md) artık bunları ne zaman bir sınama yanıtı kabul yukarıdaki örnekte olduğu gibi süresi dolacak şekilde ihtiyacınız varsa.

## <a name="send-the-sms-message"></a>SMS mesajı gönderin

**E4_SendSmsChallenge** işlevi, son kullanıcıya 4 basamaklı koduyla SMS mesajı göndermek için Twilio bağlama kullanır. *Function.json* şu şekilde tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/function.json)]

Ve 4 basamaklı sınama kodu oluşturur ve SMS mesajı gönderir kod aşağıdaki gibidir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/index.js)]

Bu **E4_SendSmsChallenge** işlevi yalnızca çağrıldığından bir kez işlem çökse bile veya yeniden. Son kullanıcı birden çok SMS iletileri almak istemediğiniz için iyidir. `challengeCode` Değeri otomatik olarak kalıcı orchestrator işlevi her zaman doğru kod nedir bilmesi için döndürür.

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnekte bulunan bir HTTP tetiklemeli işlevleri'ni kullanarak, aşağıdaki HTTP POST isteği göndererek düzenleme başlayabilirsiniz:

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

Orchestrator işlevi sağlanan telefon numarasını alır ve rastgele oluşturulmuş 4 basamaklı doğrulama kodunu içeren bir SMS iletisi gönderir hemen &mdash; gibi *2168*. İşlev yanıt 90 saniye bekler.

Kodu ile yanıt vermek için kullanabileceğiniz [ `RaiseEventAsync` (.NET) veya `raiseEvent` (JavaScript)](durable-functions-instance-management.md) iç işlev veya çağırma **sendEventUrl** yukarıdaki 202 yanıt olarak başvurulan bir HTTP POST Web kancası , değiştirme `{eventName}` olay adıyla `SmsChallengeResponse`:

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Zamanlayıcı süresi dolmadan önce bu gönderirseniz, orchestration tamamlandıktan ve `output` ayarlanmış `true`, başarılı doğrulama belirten.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Zamanlayıcı sona izin veya dört kez yanlış kod girerseniz, sorgu durumu ve bkz bir `false` telefon doğrulama başarısız olduğunu gösteren çıkışı, orchestration işlevi.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyası olarak Visual Studio projesi içinde düzenleme şu şekildedir:

> [!NOTE]
> Yüklemek ihtiyacınız olacak `Microsoft.Azure.WebJobs.Extensions.Twilio` aşağıdaki örnek kodu çalıştırmak için Nuget paketi.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte bazı gelişmiş özelliklerini dayanıklı İşlevler, özellikle gösterilmiştir `WaitForExternalEvent` ve `CreateTimer`. Bunlar ile birleştirilmesi gördüğünüz `Task.WaitAny` genellikle gerçek kişilerle etkileşime geçmek için yararlı olan bir güvenilir bir zaman aşımı sistemi uygulamak için. Dayanıklı işlevler bir dizi belirli konular ayrıntılı kapsamını sunan makaleleri okuyarak kullanma hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Serideki ilk makaleye gidin](durable-functions-bindings.md)
