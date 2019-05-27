---
title: Konuşma Transkripsiyonu - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma Transkripsiyonu, gerçek zamanlı konuşma tanıma, konuşmacı tanıma ve diarization birleştiren konuşma Hizmetleri, Gelişmiş bir özelliktir. Konuşma Transkripsiyonu, yüz yüze toplantılar konuşmacıları ayırt olanağı fotoğrafını için idealdir, bu, kimlerin ne ve ne zaman, sonraki adımlara göre toplantı ve hızlıca odaklanmak için izin verme katılımcıları takip söylediğiniz bilmenizi sağlar. Bu özellik ayrıca erişilebilirliği arttırır. Döküm ile etkin olarak işitme zorluğu yaşayan katılımcıları görüşebilirsiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: a31921e229a4bbfccd6fdd871666aad1eeef3232
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243858"
---
# <a name="what-is-conversation-transcription"></a>Konuşma tanıma nedir?

Konuşma Transkripsiyonu, gerçek zamanlı konuşma tanıma, konuşmacı tanıma ve diarization birleştiren konuşma Hizmetleri, Gelişmiş bir özelliktir. Konuşma Transkripsiyonu, yüz yüze toplantılar konuşmacıları ayırt olanağı fotoğrafını için idealdir, bu, kimlerin ne ve ne zaman, sonraki adımlara göre toplantı ve hızlıca odaklanmak için izin verme katılımcıları takip söylediğiniz bilmenizi sağlar. Bu özellik ayrıca erişilebilirliği arttırır. Döküm ile etkin olarak işitme zorluğu yaşayan katılımcıları görüşebilirsiniz.   

Konuşma Transkripsiyonu doğru tanıma ile sektör ve şirkete özel sözlük anlamak için uyarlayabileceğiniz özelleştirilebilir konuşma modelleri sunar. Ayrıca, konuşma Transkripsiyonu çok mikrofonu cihazlar için deneyimini iyileştirmek için konuşma cihaz SDK'sı ile eşleştirin.

>[!NOTE]
> Şu anda, konuşma tanıma, küçük toplantılar için önerilir. Uygun ölçekte büyük toplantılar için konuşma Transkripsiyonu genişletmek istiyorsanız, lütfen bizimle iletişime geçin.

Bu diyagram, donanım, yazılım ve konuşma Transkripsiyonu birlikte çalışma hizmetlerini gösterir.

![İçeri aktarma konuşma Transkripsiyonu diyagramı](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Döngüsel bir yedi mikrofon dizi belirli geometri yapılandırmayla gereklidir. Belirtimi ve tasarım ayrıntıları için bkz. [Microsoft konuşma cihaz SDK'sı mikrofon](https://aka.ms/cts/microphone). Daha fazla bilgi edinin veya Geliştirme Seti satın almak için bkz: [alma Microsoft konuşma cihaz SDK'sı](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Konuşma Transkripsiyonu ile çalışmaya başlama

Konuşma Transkripsiyonu ile kullanmaya başlamak için yapmanız gereken üç adım vardır.

1. Kullanıcıların ses örnekleri toplar.
2. Kullanıcı sesli örnekleri kullanarak kullanıcı profilleri oluşturma
3. Kullanıcıları (kişi) tanımlamak ve konuşma konuşmaların konuşma SDK'sını kullanma

## <a name="collect-user-voice-samples"></a>Kullanıcı sesli örnekleri Topla

İlk adım, her kullanıcının ses kayıtlarını toplamaktır. Arka plan gürültüsü olmadan Sessiz bir ortamda kullanıcının konuşma kaydedilmelidir. Önerilen ses her örnek için iki dakika 30 saniye arasında uzunluğudur. Uzun Ses örneği konuşmacıları tanımlamak için kullanıldığında geliştirilmiş doğruluğuna neden olur. Ses 16 KHz örnekleme hızı mono kanalıyla olması gerekir.

Ses kaydedilir ve depolanan size kalmıştır--olduğunu nasıl yukarıda sözü edilen yönergeler dışında güvenli bir veritabanında önerilir. Sonraki bölümde, bu ses Speech SDK'sı ile konuşmacıları tanımak için kullanılan kullanıcı profilleri oluşturmak için nasıl kullanıldığını inceleyelim.

## <a name="generate-user-profiles"></a>Kullanıcı profilleri oluşturma

Ardından, sesli kayıtlar göndermek ihtiyacınız olacak ses doğrulamak ve kullanıcı profilleri oluşturmak için imza oluşturma hizmetinin derledik. [İmza oluşturma hizmetinin](https://aka.ms/cts/signaturegenservice) oluşturur ve kullanıcı profilleri alma sağlayan REST API'ler kümesidir.

Bir kullanıcı profili oluşturmak için kullanmanız gerekecektir `GenerateVoiceSignature` API. Belirtimi ayrıntıları ve örnek kodu kullanılabilir:

> [!NOTE]
> Konuşma Transkripsiyonu "en-US" ve "zh-CN" aşağıdaki bölgelerde kullanılabilir: `centralus` ve `eastasia`.

* [REST belirtimi](https://aka.ms/cts/signaturegenservice)
* [Konuşma Transkripsiyonu kullanma](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Konuşmaların dökümünü alın ve konuşmacıları belirleyin

Konuşma Transkripsiyonu, çok kanallı ses akışları ve kullanıcı profilleri döküm oluşturma ve konuşmacıları belirlemek için giriş olarak bekler. Ses ve kullanıcı profil verileri, konuşma cihaz SDK'sını kullanarak konuşma Transkripsiyonu hizmetine gönderilir. Daha önce belirtildiği gibi döngüsel bir yedi mikrofon dizisi ve konuşma cihaz SDK'sı konuşma Transkripsiyonu kullanmak için gereklidir.

>[!NOTE]
> Belirtimi ve tasarım ayrıntıları için bkz. [Microsoft konuşma cihaz SDK'sı mikrofon](https://aka.ms/cts/microphone). Daha fazla bilgi edinin veya Geliştirme Seti satın almak için bkz: [alma Microsoft konuşma cihaz SDK'sı](https://aka.ms/cts/getsdk).

Konuşma Transkripsiyonu konuşma cihaz SDK'sı ile kullanmayı öğrenmek için bkz [konuşma transkripsiyonu kullanmayı](https://aka.ms/cts/howto).


## <a name="quick-start-with-a-sample-app"></a>Örnek uygulama ile hızlı başlangıç

Microsoft konuşma cihaz SDK'sı olan Hızlı Başlangıç örnek bir uygulama için tüm cihaz ilgili örnek. Konuşma Transkripsiyonu, bunlardan biridir. İçinde bulabilirsiniz [konuşma cihaz SDK'sı android Hızlı Başlangıç](https://aka.ms/sdsdk-quickstart) örnek uygulama ve kaynak koduna referans.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı hakkında daha fazla bilgi edinin](speech-devices-sdk.md)
