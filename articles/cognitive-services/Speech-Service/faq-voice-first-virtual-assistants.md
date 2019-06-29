---
title: Doğrudan satır konuşma hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Ses öncelikli sanal Yardımcıları doğrudan satır konuşma kanalını kullanma hakkında en yaygın soruların yanıtlarını alın.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.openlocfilehash: 2c669f00ae65667f85976aca218ce51d630159ee
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465494"
---
# <a name="voice-first-virtual-assistants-preview-frequently-asked-questions"></a>Ses öncelikli sanal Yardımcıları önizleme: Sık sorulan sorular

Bu belgede sorularınızın yanıtlarını bulamazsanız, kullanıma [diğer destek seçenekleri](support.md).

## <a name="general"></a>Genel

**S: Doğrudan satır konuşma nedir?**

**C:** `DialogServiceConnector` Çift yönlü, zaman uyumsuz iletişim için Bot Framework doğrudan satır konuşma kanalı bağlı botlar Speech SDK'sı sağlar. Bu kanal konuşma metin ve metin okuma ses içinde tam olarak olur, damıtarak konuşma bağlamında kullanılabilen deneyimleri sesli botlar sağlayan Azure konuşma Hizmetleri Eşgüdümlü erişimini sağlar. Uyandırma sözcükler ve sözcük doğrulama uyandırmak için destekle, bu çözüm, marka ya da ürün için özelleştirilebilir ses öncelikli sanal Yardımcıları oluşturmak mümkün kılar.

**S: Nasıl kullanmaya başlarım?**

**C:** En iyi şekilde başından itibaren bir ses öncelikli sanal asistan oluşturma başlamak olan [temel bir Bot Framework bot oluşturma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0). Ardından, botunuzun için bağlama [doğrudan satır konuşma kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

## <a name="debugging"></a>Hata Ayıklama

**S: 401 hatası bağlanırken almak ve hiçbir şey çalışır. Konuşma abonelik anahtarımı geçerli biliyorum. Ne var ne yok?**

**C:** Önizleme'de, doğrudan satır konuşma hangi Aboneliklerde kullanabileceğiniz belirli sınırlamaları vardır. Kullandığınızdan emin olun **konuşma** kaynak (Microsoft.CognitiveServicesSpeechServices, "Okuma") ve *değil* **Bilişsel Hizmetler** kaynak () Microsoft.CognitiveServicesAllInOne, "Tüm Bilişsel Hizmetler"). Yalnızca [konuşma Hizmetleri bölgelerin bir alt](regions.md#voice-first-virtual-assistants) doğrudan satır konuşma şu anda desteklenmiyor.

![doğrudan hat konuşma için aboneliği düzeltmek](media/voice-first-virtual-assistants/faq-supported-subscription.png "uyumlu bir konuşma tanıma aboneliğinin örneği")

**S: , Doğrudan satır konuşma tanıma metin geri dönebilir, ancak benim bot bir '1011 değerinin kodunu' hatası ve hiçbir şey görüyorum. Neden?**

**C:** Bu hata, doğrudan satır konuşma tanıma ve bot arasında bir iletişim sorununu gösterir. Açtığınızdan emin olun [doğrudan satır konuşma kanal bağlı](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech), [akış protokolü desteği eklendi](https://aka.ms/botframework/addstreamingprotocolsupport) (desteğiyle ilgili Web yuvası) bot ve botunuzun gelen yanıt sonra onay Kanal gelen istekleri.

**S: Bu kod adımlar da işe yaramazsa ve/veya bir DialogServiceConnector kullanırken farklı bir hata alıyorum. Ne yapmalıyım?**

**C:** Dosya tabanlı günlüğe kaydetme, önemli ölçüde daha fazla ayrıntı sağlar ve destek istekleri artırmanıza yardımcı olabilir. Bu işlevselliği etkinleştirmek için bkz: [dosya günlüğünü kullanma](how-to-use-logging.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
