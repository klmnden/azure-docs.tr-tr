---
title: Özel ses öncelikli sanal Yardımcıları (Önizleme) - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Özellikler, özellikleri ve kısıtlamaları doğrudan satır konuşma kanal Bot Framework ve Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK) kullanarak, özel sesli öncelikli sanal Yardımcıları için genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: 1c5712fa8bbdb158992127f8f48d810a0a9b6f79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65603472"
---
# <a name="about-custom-voice-first-virtual-assistants-preview"></a>Özel ses öncelikli sanal Yardımcıları önizleme hakkında

Azure konuşma Hizmetleri kullanarak özel sanal Yardımcıları, geliştiricilere uygulamalarını ve deneyimler için doğal olarak, İnsan benzeri damıtarak konuşma bağlamında kullanılabilen arabirimlerini oluşturma olanağı sunun. Bot Framework'ün doğrudan satır konuşma kanal, ses, sesli etkileşim düşük gecikme süresi ve yüksek güvenilirlikle kullanıma sağlayan uyumlu bir bot için Eşgüdümlü, düzenlenmiş giriş noktası sunarak bu özelliklerini geliştirir. Bu botlar, doğal dil etkileşimi için Microsoft'un Language Understanding (LUIS) kullanabilirsiniz. Doğrudan satır konuşma tanıma, konuşma Yazılım Geliştirme Seti (SDK) kullanarak cihazları tarafından erişilir.

   ![Doğrudan hat konuşma düzenleme hizmeti flow kavramsal diyagramı](media/voice-first-virtual-assistants/overview.png "konuşma kanal akış")


Doğrudan satır konuşma ve özel ses öncelikli sanal Yardımcıları için ilişkili işlevselliği olan için ideal bir ek [sanal Yardımcısı çözüm ve kurumsal şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview). Doğrudan satır konuşma uyumlu herhangi bir bot ile çalışabilir ancak bu kaynakları damıtarak konuşma bağlamında kullanılabilen yüksek kaliteli deneyimler yanı sıra ortak destekleyici becerileri ve hızlı başlangıç için modelleri için yeniden kullanılabilir bir temel sağlar.


## <a name="core-features"></a>Temel özellikleri

| Kategori | Özellikler |
|----------|----------|
|[Özel Uyandırma sözcük](speech-devices-sdk-create-kws.md) | "Hey Contoso" gibi özel bir anahtar sözcüğü kullanılarak botlar konuşmaları şununla açabileceğinizi bilirsiniz Bu görev, bir özel Uyandırma sözcük ile yapılandırılabilir konuşma SDK'da özel Uyandırma word altyapısıyla gerçekleştirilir [burada oluşturabilen](speech-devices-sdk-create-kws.md). Doğrudan satır konuşma kanal başına cihaz karşı Uyandırma word etkinleştirme doğruluğunu artırır Hizmet tarafı Uyandırma word doğrulama içerir.
|[Konuşmayı metne dönüştürme](speech-to-text.md) | Gerçek zamanlı ses tanıma tanınan metin kullanarak doğrudan satır konuşma kanal içerir [konuşma metin](speech-to-text.md) Azure konuşma Services'dan. Transcribed gibi bu metin botunuzun hem de istemci uygulamanız için kullanılabilir.
|[Metin okuma](text-to-speech.md) | Botunuzun metinsel yanıtlarından oluşturulan kullanarak [metin okuma](text-to-speech.md) Azure konuşma Services'dan. Bu sentezi sonra istemci uygulamanıza bir ses akışı olarak kullanıma sunulacaktır. Microsoft, markanızı daha fazla bilgi edinmek için bir ses veren kendi özel, yüksek kaliteli sinir TTS ses oluşturma imkanı sunar [bizimle](mailto:mstts@microsoft.com).
|[Doğrudan hat konuşma](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) | Bot Framework içinde bir kanal doğrudan satır konuşma istemci uygulamanızın, uyumlu bir bot ve Azure konuşma Hizmetleri yeteneklerini arasında düzgün ve sorunsuz bir bağlantı sağlar. Botunuzun doğrudan satır konuşma kanal kullanmak için yapılandırma hakkında daha fazla bilgi için bkz. [, Bot Framework belgeleri sayfasında](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

## <a name="sample-code"></a>Örnek kod

Ses öncelikli sanal asistan oluşturmak için örnek kod, Github'da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde botunuzun bağlanmak için istemci uygulaması kapsar.

* [Ses öncelikli sanal asistan örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Hızlı Başlangıç: ilk ses sanal Yardımcıları (C#)](quickstart-virtual-assistant-csharp-uwp.md)
* [Hızlı Başlangıç: ses öncelikli sanal Yardımcıları (Java)](quickstart-virtual-assistant-java-jre.md)

## <a name="customization"></a>Özelleştirme

Azure konuşma Hizmetleri kullanılarak oluşturulan sanal Yardımcıları ses ilk tam çeşitlilikte özelleştirme seçenekleri için kullanılabilir kullanabilir [konuşma metin](speech-to-text.md), [metin okuma](text-to-speech.md), ve [özel anahtar sözcüğü Seçimi](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Özelleştirme seçenekleri farklı dil/bölge tarafından (bkz [desteklenen diller](supported-languages.md)).

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
* [Konuşma SDK'sı Al](speech-sdk.md)
* [Temel robot oluşturup](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Sanal Yardımcısı çözüm ve kurumsal şablon alma](https://github.com/Microsoft/AI)
