---
title: Metin Analizi nedir?
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'deki Metin Analizi hizmeti yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık bağlama özellikleri sunar.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: overview
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: 545d60207bbd1941920bc0e70096417c35486634
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634609"
---
# <a name="what-is-text-analytics"></a>Metin Analizi nedir?

Metin Analizi API’si ham metin üzerinde gelişmiş doğal dil işleme sağlayan bulut tabanlı bir hizmettir ve dört ana işlev içerir: Yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık bağlama.

Bu API, geliştirme projelerinizde kullanılmaya hazır olan buluttaki makine öğrenimi ve yapay zeka algoritmaları koleksiyonu [Microsoft Bilişsel Hizmetler](https://docs.microsoft.com/azure/cognitive-services/)'in bir parçasıdır.

## <a name="capabilities-in-text-analytics"></a>Metin Analizi Özellikleri

Metin analizi farklı anlamlara gelebilir ancak Bilişsel Hizmetler Metin Analizi API'si, aşağıdaki tabloda gösterilen dört farklı türde analiz gerçekleştirir.

| İşlemler| Açıklama | API'ler |
|-----------|-------------|------|
|[**Yaklaşım Analizi**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Ham metinde pozitif veya negatif yaklaşım analizi gerçekleştirerek müşterilerinizin markanız veya belirli bir konu hakkındaki düşüncelerini öğrenin. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir.<br /> Analiz modelleri, Microsoft tarafından sağlanan geniş kapsamlı gövde metinleri ve doğal dil teknolojileri kullanılarak önceden eğitilmiştir. API, [seçili dillerde](text-analytics-supported-languages.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Anahtar İfade Ayıklama**](how-tos/text-analytics-how-to-keyword-extraction.md) | Anahtar ifadeleri otomatik olarak ayıklayarak metnin önemli noktalarını hızla belirleyin. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür.  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Dil Algılama**](how-tos/text-analytics-how-to-language-detection.md) | 120 dil için giriş metninin yazıldığı dili algılar ve istekte gönderilen her belge için tek bir dil kodu bildirir. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) | 
|[**Varlık Tanıma (Önizleme)**](how-tos/text-analytics-how-to-entity-linking.md) | Metninizdeki kişiler, yerler, kuruluşlar, tarih/saat, miktarlar, yüzdeler ve para birimleri gibi varlıkları tanımlayın ve kategorilere ayırın. İyi bilinen varlıklar da tanınarak web üzerindeki ek bilgilere bağlantı verilir. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) |

## <a name="use-containers"></a>Kapsayıcılar kullanma

[Metin analizi kapsayıcılarını](how-tos/text-analytics-how-to-install-containers.md) anahtar ifadeleri ayıklamak için dili algılayın ve verilerinizi standart Docker kapsayıcıları yakın yükleyerek yaklaşım yerel olarak analiz edin.

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir. Analiz edilecek verileri gönderir ve çıktıları kodunuzda işlersiniz. Çözümleyiciler olduğu gibi kullanılır, ek yapılandırma veya özelleştirme gerçekleştirilmez.

1. [Erişim anahtarı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) için [kaydolun](how-tos/text-analytics-how-to-access-key.md). Anahtar her istekte geçirilmelidir.

2. JSON biçiminde verilerinizi yapılandırılmamış ham metin olarak içeren [bir istek düzenleyin](how-tos/text-analytics-how-to-call-api.md#json-schema).

3. İstediğiniz kaynağı ekleyerek isteği kayıt sırasında oluşturulan uç noktaya: yaklaşım analizi, anahtar ifade ayıklama, dil algılama veya varlık tanımlama.

4. Yanıtın akışını yapın veya yerel ortamda depolayın. Gönderilen isteğe bağlı olarak sonuçlar yaklaşım puanı, ayıklanan anahtar ifadelerden oluşan bir koleksiyon veya dil kodu olacaktır.

Çıktı tek bir JSON belgesi olarak döndürülür ve kimlikle birlikte gönderdiğiniz tüm metin belgelerinin sonucunu içerir. Sonuçları daha sonra analiz ederek, görselleştirerek veya kategorilere ayırarak eyleme dönüştürülebilir içgörüler elde edebilirsiniz.

Veriler hesabınızda depolanmaz. Metin Analizi API'si tarafından gerçekleştirilen işlemlerde durum bilgisi yoktur. Başka bir deyişle sağladığınız metin işlenir ve sonuçlar anında döndürülür.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Desteklenen diller

Bu bölüm, daha kolay bulunmasını sağlama amacıyla başka bir makaleye taşınmıştır. Bu içeriği [Metin Analizi API'sinde desteklenen diller](text-analytics-supported-languages.md) sayfasında bulabilirsiniz.

<a name="data-limits"></a>

## <a name="data-limits"></a>Veri sınırları

Tüm Metin Analizi API'si uç noktaları ham metin verisi kabul eder. Geçerli sınır belge başına 5000 karakterdir. Daha büyük belgeleri analiz etmeniz gerekiyorsa daha küçük parçalara bölebilirsiniz. Sınırı yine de yükseltmeye ihtiyacınız varsa gereksinimleriniz üzerinde konuşmak için [bize ulaşın](https://azure.microsoft.com/overview/sales-number/).

| Sınır | Değer |
|------------------------|---------------|
| Tek belge için maksimum boyut | 5000 karakter, `String.Length` ile ölçülür. |
| İsteğin tamamının maksimum boyutu | 1 MB |
| Bir istekte bulunabilecek maksimum belge sayısı | 1000 belge |

Hız sınırı dakikada 100 çağrıdır. Tek bir çağrıda daha fazla belge (en fazla 1000 belge) gönderebilirsiniz.

## <a name="unicode-encoding"></a>Unicode kodlama

Metin Analizi API'si, metin gösterimi ve karakter sayısı hesaplamaları için Unicode kodlamasını kullanır. İstekler UTF-8 ve UTF-16 olarak gönderilebilir, karakter sayısında fark olmayacaktır. Karakter uzunluğu için Unicode kod noktaları buluşsal değer olarak kullanılır ve metin analizi veri sınırları için eşdeğer kabul edilir. Karakter sayısını almak için `String.Length` kullanırsanız veri boyutunu ölçmek için bizimle aynı yöntemi kullanıyor olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/text-analytics/) deneyin. Dili algılamak (120'ye kadar), yaklaşım puanı hesaplamak veya anahtar ifadeleri ayıklamak için metin girişi (en fazla 5000 karakter) yapıştırabilirsiniz. Kaydolmanıza gerek yoktur.

API'yi doğrudan çağırmaya hazır olduğunuzda:

+ [Kaydolarak](how-tos/text-analytics-how-to-signup.md) erişim anahtarı alın ve [API'yi çağırma](how-tos/text-analytics-how-to-call-api.md) adımlarını gözden geçirin.

+ [Hızlı Başlangıç](quickstarts/csharp.md), C# dilinde yazılan REST API çağrılarına ilişkin bir adım adım kılavuzdur. Minimum kodla metin göndermeyi, analiz seçmeyi ve sonuçları görüntülemeyi öğrenin.

+ [API başvuru belgeleri](//go.microsoft.com/fwlink/?LinkID=759346) API'ler için teknik belgeler sağlar. Belgeler katıştırılmış çağrıları desteklediğinden API'leri belge sayfalarından çağırabilirsiniz.

+ [Dış İçerik ve Topluluk İçeriği](text-analytics-resource-external-community.md) sayfasında Metin Analizi'ni diğer araçlar ve teknolojilerle kullanmayı gösteren blog gönderilerinin ve videoların bir listesi bulunmaktadır.

## <a name="see-also"></a>Ayrıca bkz.

 [Bilişsel Hizmetler Belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)
