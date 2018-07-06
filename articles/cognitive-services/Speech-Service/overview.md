---
title: Konuşma hizmeti (Önizleme) nedir? | Microsoft Docs
description: 'Konuşma hizmeti, Microsoft Bilişsel hizmetler, parçası ayrı olarak önceden kullanılabilen çeşitli Azure konuşma Hizmetleri sahip: Bing konuşma (metin okuma ve konuşma tanıma kapsayan), özel konuşma tanıma ve konuşma çevirisi.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2c1fce35749dee12caec0bcd15a9eccdf81b8d1d
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856782"
---
# <a name="what-is-the-speech-service-preview"></a>Konuşma hizmeti (Önizleme) nedir?

Konuşma hizmeti, Microsoft Bilişsel hizmetler, parçası ayrı olarak önceden kullanılabilen çeşitli Azure konuşma Hizmetleri sahip: Bing konuşma (metin okuma ve konuşma tanıma kapsayan), özel konuşma tanıma ve konuşma çevirisi. Kendi precursors gibi konuşma tanıma hizmeti, Cortana ve Microsoft Office gibi diğer Microsoft ürünlerinde kullanılan teknolojileri tarafından desteklenir.

> [!NOTE]
> Konuşma hizmeti, şu anda genel Önizleme aşamasındadır. Burada düzenli olarak güncelleştirmeleri belgeler, ek kod örnekleri ve daha fazlası için döndürür.

Bir aboneliği ile birleşik konuşma hizmeti geliştiricilerin kendi uygulamaları güçlü konuşma özellikli özellikler sağlamak için kolay bir yol sağlar. Uygulamalarınızı, sesli komut, döküm, yazdırma, konuşma sentezi ve çeviri artık özelliğini.

|İşlev|Açıklama|
|-|-|
|Konuşmayı Metne Dönüştürme|Sürekli İnsan konuşma uygulamanız için giriş olarak kullanılabilecek metne dönüştürür. Tümleştirilebilir [Language Understanding hizmeti](https://docs.microsoft.com/azure/cognitive-services/luis/) kullanıcının amacını konuşma türetme (LUIS).|
|Metin Okuma|Metin doğal görünen Sentezlenen konuşma ses dosyalarına dönüştürür.|
|Konuşma&nbsp;çeviri|Diğer diller için konuşma çevirisi ile metin ve konuşma çıktısını sağlayın.|

## <a name="using-the-speech-service"></a>Konuşma hizmeti kullanma

Konuşma hizmeti iki şekilde kullanılabilir. [SDK'sı](speech-sdk.md) ağ protokolleri ayrıntılarını dengelediği. [REST API](rest-apis.md) herhangi bir programlama dilinde çalışır, ancak SDK tarafından sunulan tüm işlevleri sağlamaz.

|<br>Yöntem|Konuşma<br>Metni|Metni<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[SDK'ları](speech-sdk.md)|Evet|Hayır|Evet|Geliştirmeyi kolaylaştıran belirli programlama dilleri için kitaplıkları.|
|[REST](rest-apis.md)|Evet|Evet|Hayır|Konuşma uygulamanıza eklemek kolaylaştıran bir basit HTTP tabanlı API'ler.|

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

[Konuşmayı metne dönüştürme](speech-to-text.md) (STT) veya konuşma tanıma API'si dönüştürür ses akışları metne uygulamanızı girdi olarak kabul edebilir. Daha sonra uygulamanız, örneğin, metni bir belgeye girebilir veya komut olarak üzerinde işlem yapabilir.

Konuşmayı metne dönüştürme etkileşimli, konuşma için ayrı ayrı da iyileştirilmiştir ve dikte senaryoları. Konuşmayı metne dönüştürme API'si için ortak kullanım durumları verilmiştir. 

* Geçici sonucu olmayan bir komutu gibi kısa bir utterance tanıması
* Sesli posta iletisi gibi uzun, daha önce kaydedilen bir utterance özelliği
* Akış konuşma dilinde konuşmaların dikte için kısmi sonuçlar ile gerçek zamanlı
* Yapmak konuşmada geçen bir doğal dil isteğinin göre istediklerini belirleme

Konuşmayı metne dönüştürme API'si, gerçek zamanlı sürekli tanıma ve Ara sonuçlar ile etkileşimli konuşma transkripsiyonu destekler. Ayrıca konuşma sonu algılama, isteğe bağlı otomatik büyük harfe çevirme ve noktalama, küfür maskeleme ve metin normalleştirmeyi destekler.

Konuşmadan metne akustik ve dil modellerini özel sözlük gürültülü ortamlarda ve konuşma farklı şekilde uyum sağlayacak şekilde özelleştirebilirsiniz.

## <a name="text-to-speech"></a>Metin Okuma

[Metin okuma](text-to-speech.md) (TTS) veya konuşma sentezi API dönüştürür düz metin ses dosyası uygulamanızda teslim görünen doğal konuşma. Birden çok ses, cinsiyet veya vurgu, değişen birçok desteklenen diller için kullanılabilir.

API destekler [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md) sorunlu sözcükleri tam fonetik telaffuz belirtebilmeniz için etiketler. SSML ayrıca metnin içinde konuşma özelliklerini gösterebilir (vurgu, hız, ses düzeyi, cinsiyet ve sıklık gibi).

Metin okuma API'si için ortak kullanım durumları verilmiştir.

* Görme engelli kullanıcılar için alternatif ekran çıktı olarak konuşma çıkışındaki
* Ses isteyen gezintisi gibi araç uygulamalar
* Konuşmayı metne dönüştürme API'si ile uyumlu etkileşimli kullanıcı arabirimleri

Metin okuma API'si, yalnızca uygulamanız için benzersiz bir ses istiyorsanız veya desteklenmeyen bir diyalekt için gerekir, destekler [özel sesli modelleri](how-to-customize-voice-font.md).

## <a name="speech-translation"></a>Konuşma Çevirisi

[Konuşma çevirisi](speech-translation.md) API neredeyse gerçek zamanlı Akış ses çevirmek veya kayıtlı konuşma işlemek için kullanılabilir. Akış çeviri hizmeti çeviri ilerlemeyi göstermek için kullanıcıya görüntülenecek Ara sonuçlar döndürür. Sonuçlar metin veya ses olarak döndürülebilir.

Konuşma çevirisi için kullanım örnekleri aşağıda verilmiştir.

* "Konuşma" çeviri mobil uygulama veya cihaz için yolculara uygulayın 
* Ses ve video kayıtlarını subtitling için otomatik çevirileri sağlayın

## <a name="speech-devices-sdk"></a>Konuşma Cihazları SDK’sı

Birleşik konuşma hizmeti sunulmasıyla birlikte, Microsoft ve ortaklarından konuşma özellikli cihazlar geliştirmek için en iyi duruma getirilmiş bir tümleşik donanım/yazılım platformu sunar: [konuşma cihaz SDK'sı](speech-devices-sdk.md). Bu SDK, tüm uygulama türleri için akıllı konuşma cihazları geliştirmeye uygundur.

Ses yakalamayı tetikler işaret markanız için benzersiz olmasını sağlayın konuşma cihaz SDK'sı bir özelleştirilmiş Uyandırma sözcük ile kendi çevresel cihazları oluşturmanıza olanak tanır. Ayrıca, gürültü bastırma, uzak alan sesi ve hüzmeleme dahil olmak üzere daha doğru konuşma tanıma için çok kanallı kaynaklardan üstün ses işleme özelliği sağlar.

SDK'sı, 443 numaralı bağlantı noktasını kullanarak web yuvaları üzerinde temel alır.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma hizmeti için bir ücretsiz deneme aboneliği anahtar alın.

> [!div class="nextstepaction"]
> [Konuşma hizmeti ücretsiz olarak deneyin](get-started.md)
