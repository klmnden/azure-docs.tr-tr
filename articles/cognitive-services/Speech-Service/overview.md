---
title: Konuşma hizmeti (Önizleme) nedir? | Microsoft Docs
description: "Konuşma hizmeti, Microsoft'un Bilişsel hizmetler parçası ayrı olarak daha önce kullanılabilen birkaç Azure konuşma Hizmetleri birleştirmiştir: Bing konuşma (konuşma tanıma ve metin okuma oluşan), özel konuşma ve konuşma çeviri."
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: e7c09eee1634c52e78a523a7cc65641ea99f23e6
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356170"
---
# <a name="what-is-the-speech-service-preview"></a>Konuşma hizmeti (Önizleme) nedir?

Konuşma hizmeti, Microsoft'un Bilişsel hizmetler parçası ayrı olarak daha önce kullanılabilen birkaç Azure konuşma Hizmetleri birleştirmiştir: Bing konuşma (konuşma tanıma ve metin okuma oluşan), özel konuşma ve konuşma çeviri. Kendi precursors gibi Cortana ve Microsoft Office gibi diğer Microsoft ürünlerinde kullanılan teknolojileri konuşma hizmet tarafından desteklenir.

> [!NOTE]
> Konuşma Hizmet şu anda genel önizlemede değil. Buraya düzenli olarak güncelleştirmeleri belgeler, ek kod örnekleri ve daha fazla bilgi için dönün.

Bir aboneliği ile birleştirilmiş konuşma hizmeti geliştiricilerin kendi uygulamaları güçlü konuşma etkin özellikler sağlamak için kolay bir yol sağlar. Uygulamalarınızı şimdi sesli komut, transcription, yazdırma, konuşma birleştirici ve çeviri özelliğini.

|İşlev|Açıklama|
|-|-|
|Konuşmayı Metne Dönüştürme|Sürekli İnsan konuşma uygulamanız için giriş olarak kullanılan metne dönüştürür. İle tümleştirebilirsiniz [dil anlama hizmet](https://docs.microsoft.com/azure/cognitive-services/luis/) (kullanıcının istediğinden utterances türetilen HALUK).|
|Metin Okuma|Metin doğal görünen birleştirilen konuşma ses dosyalarına dönüştürür.|
|Konuşma&nbsp;çevirisi|Diğer diller konuşma çevirisinde metin veya konuşma çıkışı sağlar.|

## <a name="using-the-speech-service"></a>Konuşma hizmetini kullanarak

Konuşma hizmet iki yolla kullanılabilir hale getirilir. [SDK](speech-sdk.md) hemen ağ protokolleri ayrıntılarını soyutlar. [REST API](rest-apis.md) herhangi bir programlama dili ile çalışır ancak SDK tarafından sunulan tüm işlevleri sağlamaz.

|<br>Yöntem|Konuşma<br>Metni|Metni<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[SDK'ları](speech-sdk.md)|Evet|Hayır|Evet|Kitaplıkları geliştirmeyi kolaylaştıran belirli programlama dili için.|
|[REST](rest-apis.md)|Evet|Evet|Hayır|Konuşma uygulamanıza eklemek kolaylaştıran basit bir HTTP tabanlı API.|

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

[Metin konuşma](speech-to-text.md) (STT) veya konuşma tanıma API'si transcribes ses akışları metne uygulamanızı girdi olarak kabul edebilir. Uygulamanızı daha sonra Örneğin, bir belgeye metin girin veya onun üzerinde işlem bir komut.

Konuşma metin etkileşimli, konuşma için ayrı olarak da iyileştirilmiştir ve yazdırma senaryoları. Konuşma metin API için ortak kullanım durumları verilmiştir. 

* Tanı Ara sonuçlar olmadan bir komut gibi bir utterance kısa
* Sesli posta iletisi gibi uzun, daha önce kaydedilen bir utterance transcribe
* İçinde akış konuşma transcribe dikte için kısmi sonuçlar ile gerçek zamanlı
* Ne kullanıcılar yapmak bir konuşma doğal dil isteği göre istediğinize karar verin

Konuşma metin API geçici sonuçlarını ve gerçek zamanlı sürekli tanıma ile etkileşimli konuşma transcription destekler. Konuşma uç algılama, isteğe bağlı otomatik büyük/küçük harf ve noktalama işaretleri, uygunsuz metin maskeleme ve metin normalleştirme da destekler.

Metin akustik konuşma ve dil modelleri özel sözlük, gürültülü ortamları ve konuşarak farklı yöntemler uyacak şekilde özelleştirebilirsiniz.

## <a name="text-to-speech"></a>Metin Okuma

[Metin okuma](text-to-speech.md) (TTS) veya konuşma Birleştirici API dönüştürür düz metin doğal sesli okuma, bir ses dosyası uygulamanızda teslim edilir. Birden çok ses, cinsiyetiniz veya vurgu, değişen birçok desteklenen diller için kullanılabilir.

Sorunlu sözcükleri tam ses telaffuz belirtebilmeniz API konuşma Birleştirici işaretleme dili (SSML) etiketlerini destekler. SSML de (Vurgu, oranı, birim, cinsiyetiniz ve sıklık dahil) konuşma özelliklerini belirtmek sağ metin.

Metin okuma API için ortak kullanım durumları verilmiştir.

* Görme engelli kullanıcılar için bir alternatif ekran çıktı olarak konuşma çıkışı
* Gezinti gibi otomobil uygulamalar için isteyen ses
* Konuşma metin API birlikte konuşma kullanıcı arabirimleri

Metin okuma API için desteklenmeyen bir diyalekt gerekir veya yalnızca uygulamanız için benzersiz bir sesli istiyorsanız özel sesli modellerini destekler.

## <a name="speech-translation"></a>Konuşma Çevirisi

[Konuşma çeviri](speech-translation.md) API yakın gerçek zamanlı Akış ses çevirmek için veya kayıtlı konuşma işlemek için kullanılabilir. Çeviri akışında hizmet çeviri ilerlemeyi göstermek için kullanıcıya görüntülenen Ara sonuçlar döndürür. Sonuçları metin veya sesli olarak döndürülebilir.

Konuşma çeviri için kullanım örnekleri aşağıda verilmiştir.

* Bir "konuşma" çeviri mobil uygulama veya cihaz için seyahat edenler uygulama 
* Ses ve video kayıtlarını subtitling için otomatik çeviri sağlayın

## <a name="speech-devices-sdk"></a>Konuşma Cihazları SDK’sı

Birleştirilmiş konuşma hizmet başlanmasıyla, Microsoft ve onun ortakları konuşma özellikli cihazlar geliştirmek için en iyi hale getirilmiş bir tümleşik donanım/yazılım platformu sunar: [konuşma aygıtları SDK](speech-devices-sdk.md). Bu SDK, akıllı konuşma cihazlar tüm uygulama türleri için geliştirme için uygundur.

Ses yakalamayı tetikler işaret, marka benzersiz olmasını sağlamak konuşma aygıtları SDK'sı ortam kendi özelleştirilmiş Uyandırma word aygıtlarla oluşturmanıza olanak sağlar. Ayrıca, gürültü gizleme, alan uçta ses ve beamforming dahil olmak üzere daha doğru konuşma tanıma için çok kanallı kaynaklardan işleme üstün ses sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma hizmeti için ücretsiz deneme aboneliği anahtarı edinin.

> [!div class="nextstepaction"]
> [Konuşma hizmet ücretsiz deneyin](get-started.md)
