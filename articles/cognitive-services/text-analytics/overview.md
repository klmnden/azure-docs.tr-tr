---
title: Metin Analizi'ne genel bakış - Azure Bilişsel hizmetler | Microsoft Docs
description: Yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık bağlama için Azure Bilişsel hizmetler metin analizleri.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: a514618713afe2306b6fb99b2f8c038aeac56009
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092212"
---
# <a name="what-is-text-analytics"></a>Metin analizi nedir?

Metin analizi, Gelişmiş doğal dil işleme için yapılandırılmamış ham metin hizmetidir. Dört ana işlev içerir: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık bağlama.

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

[Öğrenmek](how-tos/text-analytics-how-to-sentiment-analysis.md) müşterilerin markanız veya konunun pozitif veya negatif yaklaşım hakkında ipuçları için ham metni çözümleyerek düşündüklerini. Bu API 1 en pozitif olduğu arasında 0 ve 1 her belge için bir yaklaşım puanı döndürür.<br />
Analiz modelleri, metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak kullanan. İçin [seçili dilleri](text-analytics-supported-languages.md), API analiz edin ve puan sağlayan herhangi bir ham metin.

## <a name="extract-key-phrases"></a>Anahtar ifadeleri ayıklama

Otomatik olarak [anahtar tümcecikleri ayıklayın](how-tos/text-analytics-how-to-keyword-extraction.md) ana noktaları hızlıca belirlemek için. Örneğin, "yemek tat ve harika personeli vardı" giriş metni belirtilen metin analizi hizmeti ana konuşma noktalarını döndürür: "yemek" ve "harika personel".

## <a name="detect-language"></a>Dili algılama

En fazla 120 dil için [algılamak](how-tos/text-analytics-how-to-language-detection.md) hangi dil giriş metni olarak yazılır ve rapor istekte gönderilen her belge için bir tek dil kodu. Dil kodu puanı gücünü gösteren bir puanı ile eşleştirilir.

## <a name="identify-linked-entities-preview"></a>Bağlı varlıkları (Önizleme) tanımlama

[Tanımlamak](how-tos/text-analytics-how-to-entity-linking.md) metin ve web üzerinde daha fazla bilgi için bağlantı bilinen varlıklar. Varlık bağlama tanır ve bir terim biri Meydanı, fiiller ve diğer sözcük biçimlerini olarak kullanıldığında belirgin hale getirir.

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir: kodunuzda veri analizi ve tutamacı çıkış gönderin. Çözümleyici olarak kullandığı-hiçbir ek yapılandırma veya özelleştirme değildir.

1. [Kaydolun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) için bir [erişim anahtarı](how-tos/text-analytics-how-to-access-key.md). Anahtarı her isteği geçirilmelidir.

2. [Bir isteği oluşturma](how-tos/text-analytics-how-to-call-api.md#json-schema) verilerinizi ham yapılandırılmamış metin olarak içeren bir JSON.

3. Kayıt sırasında oluşturulan uç noktasına istek sonrasında, aramak istediğiniz API'yi ekleme: yaklaşım analizi, anahtar ifade ayıklama, dil algılama veya varlık kimliği.

4. Stream veya yanıtı yerel olarak depolayın. İstek bağlı olarak, sonuçları bir yaklaşım puanı, ayıklanan anahtar ifadeleri koleksiyonunu veya bir dil kodu olur.

Size gönderilen, her bir metin belgesi için sonuçları içeren tek bir JSON belge kimliği temel alınarak gibi bir çıktı döndürülür Analiz görselleştirin veya eyleme dönüştürülebilir Öngörüler haline sonuçları kategorilere ayırın.

Metin analizi hizmeti tarafından gerçekleştirilen işlemler, durum bilgisi bulunmaz. Hesabınızdaki veri depolanmaz.

<a name="data-limits"></a>

## <a name="specifications"></a>Belirtimler

### <a name="supported-languages"></a>Desteklenen diller

Bkz: [desteklenen diller metin Analytics'te](text-analytics-supported-languages.md).

### <a name="data-limits"></a>Veri sınırları

Tüm metin analizi hizmet uç noktaları, ham metin verileri kabul edin. Her belge için geçerli sınır 5.000 karakterdir; daha büyük belgelere çözümlemeniz gerekiyorsa, bunları daha küçük öbeklere ayırmak. Yine de daha yüksek bir sınıra ihtiyacınız varsa [bizimle](https://azure.microsoft.com/overview/sales-number/) böylece gereksinimlerinizi tartışabiliriz.

| Sınır | Değer |
|------------------------|---------------|
| Tek bir belgenin en büyük boyutu | ölçülen olarak 5000 karakter `String.Length`. |
| Tüm istek üst sınırı | 1 MB |
| Belgelerde bir istek sayısı | 1.000 belge |

Hız sınırı, dakika başına 100 çağrılarıdır. Çok sayıda (en fazla 1000 belge) tek bir çağrı belgelerde gönderdiğiniz unutmayın.

### <a name="unicode-encoding"></a>Unicode kodlama

Metin analizi hizmeti metin gösterimi ve karakter sayısı hesaplamalar için Unicode kodlaması kullanır. UTF-8 veya UTF-16 karakter sayısı hiç ölçülebilir farklılıkları istekleri gönderebilirsiniz. Kullanırsanız `String.Length` karakter sayısını almak için veri boyutu ölçmek için kullandığımız aynı yöntem kullanmakta olduğunuz.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, deneyin [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/text-analytics/). (En fazla 120) dili algılayın, yaklaşım puanını hesaplamak, anahtar ifadeleri ayıklamak için bir metin girişi (5000 karakter maksimum) yapıştırın veya bağlı varlıklar tanımlayın. İstemiyorum'u gereklidir.

Metin analizi hizmeti doğrudan çağırmak hazır olduğunuzda:

+ [Kaydolun](how-tos/text-analytics-how-to-signup.md) erişim için adımları gözden geçirin ve anahtar [API'yi çağırıp](how-tos/text-analytics-how-to-call-api.md).

+ [Hızlı Başlangıç](quickstarts/csharp.md) olan C# dilinde yazılmış bir REST API Kılavuzu çağırır. Metin gönderin, analiz seçin ve olabildiğince az kodla sonuçlarını görüntüleme hakkında bilgi edinin.

+ [API başvuru belgeleri](//go.microsoft.com/fwlink/?LinkID=759346) REST API'leri için teknik belgeler sağlar. Her belgeler sayfasından API'yi çağırabilmek belgeleri katıştırılmış çağrılarını destekler.

+ [Dış & topluluk içeriği](text-analytics-resource-external-community.md) blog gönderileri ve diğer araçlar ve teknolojiler ile metin analizi kullanmayı gösteren videoların bir listesini sağlar.

## <a name="see-also"></a>Ayrıca bkz.

 [Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)
