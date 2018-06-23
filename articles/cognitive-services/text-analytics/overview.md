---
title: Metin analizi API genel bakış - Azure Bilişsel hizmetler | Microsoft Docs
description: Metin analizi API düşünceleri analiz, anahtar tümcecik ayıklama ve dil algılama için Azure Bilişsel Services'de.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: db5ea943f270aa512afb508668aec90cc4c90df4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355192"
---
# <a name="what-is-text-analytics-api-version-20"></a>Metin analizi API sürüm 2.0 nedir?

Metin analizi API Gelişmiş doğal dil ham metni işleme sağlar ve dört ana işlevleri içeren bir bulut tabanlı hizmetidir: düşünceleri analiz, anahtar tümcecik ayıklama, dil algılama ve varlık bağlama.

API kaynaklarında desteğiyle [Microsoft Bilişsel Hizmetler](https://docs.microsoft.com/azure/cognitive-services/), machine learning ve bulut geliştirme projelerinizi taşımalarına tüketilebilir AI algoritmalara koleksiyonu.

## <a name="capabilities-in-text-analytics"></a>Metin analizi özellikleri

Metin analizi farklı anlamlara, ancak Bilişsel Hizmetleri'nde aşağıdaki tabloda açıklandığı şekilde analiz dört tür metin Analytics API sağlar.

| İşlemler| Açıklama | API'ler |
|-----------|-------------|------|
|[**Düşünceleri çözümleme**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Müşteriler, marka veya konu olumlu veya olumsuz düşünceleri hakkında ipuçları için ham metni çözümleyerek düşüncelerinizi bulun. Bu API burada 1 en pozitif, 0 ile 1 her belge için arasında düşünceleri puan döndürür.<br /> Analiz modeller Microsoft metin ve doğal dil teknolojilerinden kapsamlı bir gövdesi kullanarak pretrained. İçin [Seçilen diller](text-analytics-supported-languages.md), API çözümlemek ve doğrudan çağıran uygulama sonuçları döndüren sağlayan herhangi bir ham metin puan. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Anahtar tümcecik ayıklama**](how-tos/text-analytics-how-to-keyword-extraction.md) | Otomatik olarak hızlı bir şekilde ana noktalarını tanımlamak için anahtar tümcecikleri ayıklayın. Örneğin, giriş metni "yemek Lezzetli harika personel vardı", API döndürür ve ana Konuşmayı noktalarını: "yemek" ve "harika personeli".  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Dil algılama**](how-tos/text-analytics-how-to-language-detection.md) | En fazla 120 diller için giriş metni yazıldığı dili algılamak ve isteği gönderilen her belge için bir tek dil kodu rapor. Dil kodu puanı gücünü gösteren bir puan ile eşleştirilmiş. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) | 
|[**Varlık (Önizleme) bağlama**](how-tos/text-analytics-how-to-entity-linking.md) | Metin ve web hakkında daha fazla bilgi için bağlantı iyi bilinen varlıklar tanımlayın. Varlık bağlama tanır ve bir terim birini ayrı ayrı ayrılabilen varlıkları, fiilleri ve diğer sözcük formlarını kullanıldığında, açıklar. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) | 

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir: kodunuzda analiz ve tanıtıcı çıkış verileri gönderin. Çözümleyiciler olarak tüketilen-hiçbir ek yapılandırma veya özelleştirme değildir.

1. [Kaydolun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) için bir [erişim tuşu](how-tos/text-analytics-how-to-access-key.md). Anahtarı her istekte geçirilmelidir.

2. [Bir istek formüle](how-tos/text-analytics-how-to-call-api.md#json-schema) ham yapılandırılmamış metin JSON olarak verilerinizi içeren.

3. İstek sırasında oluşturulan uç sonrası kaydolma, istenen kaynak ekleme: düşünceleri analiz, anahtar tümcecik ayıklama, dil algılama veya varlık kimliği.

4. Akış veya yanıt yerel olarak depolayın. İstek bağlı olarak sonuçlar düşünceleri puanı, ayıklanan anahtar tümcecikleri koleksiyonu veya bir dil kodu olur.

Sonuçlar, gönderilen, her metin belgesi için tek bir JSON belgesi kimliği temel alınarak gibi çıktı döndürülür Daha sonra çözümlemek, görselleştirme veya eyleme dönüştürülebilir Öngörüler sonuçları kategorilere ayırma.

Veri hesabınızda depolanmaz. Metin analizi API'si tarafından gerçekleştirilen işlemler sağladığınız metni işlenir ve sonuçları hemen döndürülür yani durumsuz.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Desteklenen diller

Bu bölüm için ayrı bir makale için daha iyi bulunabilirliği taşındı. Başvurmak [desteklenen diller metin Analytics API](text-analytics-supported-languages.md) bu içerik için.

<a name="data-limits"></a>

## <a name="data-limits"></a>Veri sınırları

Tüm metin Analytics API uç noktaları ham metin verileri kabul ediyor. Geçerli sınır her belge için 5.000 karakterdir; büyük belgelerde analiz etmeniz gerekiyorsa, bunları daha küçük parçalara bölmek. Daha yüksek bir sınır hala gerekliyse [Bize Ulaşın](https://azure.microsoft.com/overview/sales-number/) böylece biz gereksinimlerinizi tartışın.

| Sınır | Değer |
|------------------------|---------------|
| Tek bir belgenin en büyük boyutu | ölçülen gibi 5.000 karakterleri `String.Length`. |
| Tüm istek en büyük boyutu | 1 MB |
| Bir istek belgelerde maksimum sayısı | 1.000 belgeleri |

Hız sınırı dakika başına 100 çağrıdır. Tek bir çağrı (en fazla 1000 belge) belgelerde büyük miktarlarda gönderebilirsiniz unutmayın.

## <a name="unicode-encoding"></a>Unicode kodlama

Metin analizi API metin gösterimi ve karakter sayısı hesaplamalar için Unicode kodlama kullanır. İstekler, UTF-8 ve UTF-16 karakter sayısı ölçülebilir hiçbir farklılıkları ile gönderilebilir. Unicode kod buluşsal yöntemi karakter uzunluğu için kullanılır ve metin analytics veri sınırları amaçları doğrultusunda eşdeğer olarak değerlendiriliyor. Kullanırsanız `String.Length` karakter sayısı almak için kullandığımız veri boyutu ölçmek için aynı yöntemi kullanıyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

Öncelikle, deneyin [etkileşimli demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Bir metin giriş (5.000 karakter maksimum) dili (en fazla 120) algıla, düşünceleri puanını hesaplamak için Yapıştır veya anahtar tümcecikleri ayıklar. Hiçbir kayıt gerek yoktur.

API doğrudan çağırmak hazır olduğunuzda:

+ [Kaydolun](how-tos/text-analytics-how-to-signup.md) bir erişim anahtarı ve adımları için gözden [API çağırma](how-tos/text-analytics-how-to-call-api.md).

+ [Hızlı Başlangıç](quickstarts/csharp.md) bir kılavuz REST API çağrıları C# dilinde yazılmış olan. Metin gönderme, analiz seçin ve çok az kod sonuçları görüntülemek öğrenin.

+ [API başvuru belgeleri](//go.microsoft.com/fwlink/?LinkID=759346) API için teknik belgeler sağlar. Böylece her belge sayfasından API'si çağırabilirsiniz belgelerine katıştırılmış çağrıları destekler.

+ [Dış & topluluk içeriği](text-analytics-resource-external-community.md) blog gönderileri ve videolar metin analizi diğer araçları ve teknolojileri ile nasıl kullanılacağını gösteren bir liste sağlar.

## <a name="see-also"></a>Ayrıca bkz.

 [Bilişsel hizmetler belge sayfası](https://docs.microsoft.com/azure/cognitive-services/)
