---
title: Metin analizi API'si nedir? -Özellikleri-
titleSuffix: Azure Cognitive Services
description: Yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma için Azure Bilişsel Hizmetler'in sunduğu metin analizi API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 04/03/2019
ms.author: aahi
ms.openlocfilehash: 7d52585b51af09c430130141c3680b5630f7b95e
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417280"
---
# <a name="what-is-text-analytics-api"></a>Metin analizi API'si nedir?

Metin analizi API'si, Gelişmiş doğal dil işleme ham metin üzerinde sağlayan ve dört ana işlev içerir ve bulut tabanlı bir hizmet olduğundan: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma.

API parçasıdır [Azure Bilişsel Hizmetler](https://docs.microsoft.com/azure/cognitive-services/), makine öğrenimi ve yapay ZEKA algoritmalarının geliştirme projeleriniz için bulutta bir koleksiyonu.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

Metin analizi farklı anlamlara gelebilir, ancak Bilişsel hizmetler, metin analizi API'si aşağıda açıklandığı gibi analiz dört tür sağlar.

## <a name="sentiment-analysis"></a>Duygu Analizi
Kullanım [yaklaşım analizi](how-tos/text-analytics-how-to-sentiment-analysis.md) müşterilerin markanız veya konunun pozitif veya negatif yaklaşım hakkında ipuçları için ham metni çözümleyerek düşündüklerini bulunacak. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir.<br /> Analiz modelleri, Microsoft tarafından sağlanan geniş kapsamlı gövde metinleri ve doğal dil teknolojileri kullanılarak önceden eğitilmiştir. API, [seçili dillerde](text-analytics-supported-languages.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. Kullanabileceğiniz [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c9) API veya [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama
Otomatik olarak [anahtar tümcecikleri ayıklayın](how-tos/text-analytics-how-to-keyword-extraction.md) ana noktaları hızlıca belirlemek için. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. Kullanabileceğiniz [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6) burada API veya [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="language-detection"></a>Dil Algılama
Yapabilecekleriniz [giriş metni olarak yazıldığı dili algılamak](how-tos/text-analytics-how-to-language-detection.md) ve çok çeşitli diller, çeşitleri, diyalektler ve bölge/kültürel bazı diller, istekte gönderilen her belge için bir tek dil kodu bildirin. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. Kullanabileceğiniz [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c7) API veya [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="named-entity-recognition"></a>Adlandırılmış Varlık Tanıma
[Tanımlamak ve varlıkları kategorilere](how-tos/text-analytics-how-to-entity-linking.md) metninizi kişiler, yerler, kuruluşlar olarak tarih/saat, miktarlar, yüzde, para birimleri ve daha fazla. İyi bilinen varlıklar da tanınarak web üzerindeki ek bilgilere bağlantı verilir. Kullanabileceğiniz [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/5ac4251d5b4ccd1554da7634) API.

## <a name="use-containers"></a>Kapsayıcıları kullanma

[Metin analizi kapsayıcılarını](how-tos/text-analytics-how-to-install-containers.md) anahtar ifadeleri ayıklamak için dili algılayın ve verilerinizi standart Docker kapsayıcıları yakın yükleyerek yaklaşım yerel olarak analiz edin.

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir. Analiz edilecek verileri gönderir ve çıktıları kodunuzda işlersiniz. Çözümleyiciler olduğu gibi kullanılır, ek yapılandırma veya özelleştirme gerçekleştirilmez.

1. [Erişim anahtarı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) için [kaydolun](how-tos/text-analytics-how-to-access-key.md). Anahtar her istekte geçirilmelidir.

2. JSON biçiminde verilerinizi yapılandırılmamış ham metin olarak içeren [bir istek düzenleyin](how-tos/text-analytics-how-to-call-api.md#json-schema).

3. İstediğiniz kaynağı ekleyerek isteği kayıt sırasında oluşturulan uç noktaya: yaklaşım analizi, anahtar ifade ayıklama, dil algılama veya varlık tanımlama.

4. Yanıtın akışını yapın veya yerel ortamda depolayın. Gönderilen isteğe bağlı olarak sonuçlar yaklaşım puanı, ayıklanan anahtar ifadelerden oluşan bir koleksiyon veya dil kodu olacaktır.

Çıktı tek bir JSON belgesi olarak döndürülür ve kimlikle birlikte gönderdiğiniz tüm metin belgelerinin sonucunu içerir. Sonuçları daha sonra analiz ederek, görselleştirerek veya kategorilere ayırarak eyleme dönüştürülebilir içgörüler elde edebilirsiniz.

Veriler hesabınızda depolanmaz. Metin Analizi API'si tarafından gerçekleştirilen işlemlerde durum bilgisi yoktur. Başka bir deyişle sağladığınız metin işlenir ve sonuçlar anında döndürülür.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Metin analizi için birden çok programlama deneyimi düzeyleri

Programlamada deneyiminiz olmasa bile işlemlerinizi, metin analizi API'sini kullanmaya başlayabilirsiniz. Deneyim düzeyiniz uyacak şekilde farklı şekillerde metin analizi API'sini nasıl kullanabileceğinizi öğrenmek için bu öğreticileri kullanın. 

* Gerekli en düşük programlama:
    * [Metin analizi API'sini kullanın ve MS yorumların bir Yammer grubunda yaklaşım tanımlamak için Flow](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Power BI, müşteri geri bildirimi çözümlemek için metin analizi API'si ile tümleştirme](tutorials/tutorial-power-bi-key-phrases.md)
* Önerilen programlama deneyimi:
    * [Azure Databricks kullanarak akış verileri üzerinde yaklaşım analizi](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Metinleri çevirin, duyguları çözümleyin ve konuşma sentezlemek için bir Flask uygulaması derleme](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Desteklenen diller

Bu bölüm, daha kolay bulunmasını sağlama amacıyla başka bir makaleye taşınmıştır. Bu içeriği [Metin Analizi API'sinde desteklenen diller](text-analytics-supported-languages.md) sayfasında bulabilirsiniz.

<a name="data-limits"></a>

## <a name="data-limits"></a>Veri sınırları

Tüm Metin Analizi API'si uç noktaları ham metin verisi kabul eder. Her belge için geçerli sınır 5.120 karakterdir; daha büyük belgelere çözümlemeniz gerekiyorsa, bunları daha küçük öbeklere ayırmak. Sınırı yine de yükseltmeye ihtiyacınız varsa gereksinimleriniz üzerinde konuşmak için [bize ulaşın](https://azure.microsoft.com/overview/sales-number/).

| Sınır | Değer |
|------------------------|---------------|
| Tek belge için maksimum boyut | ölçülen gibi 5.120 karakter [ `StringInfo.LengthInTextElements` ](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements). |
| İsteğin tamamının maksimum boyutu | 1 MB |
| Bir istekte bulunabilecek maksimum belge sayısı | 1000 belge |

Dakika başına ikinci ve 1000 istek başına 100 istek oranı sınırlıdır. Çok sayıda (en fazla 1000 belge) tek bir çağrı belgelerde gönderebilirsiniz.

## <a name="unicode-encoding"></a>Unicode kodlama

Metin Analizi API'si, metin gösterimi ve karakter sayısı hesaplamaları için Unicode kodlamasını kullanır. İstekler UTF-8 ve UTF-16 olarak gönderilebilir, karakter sayısında fark olmayacaktır. Karakter uzunluğu için Unicode kod noktaları buluşsal değer olarak kullanılır ve metin analizi veri sınırları için eşdeğer kabul edilir. Kullanırsanız [ `StringInfo.LengthInTextElements` ](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) karakter sayısını almak için veri boyutu ölçmek için kullandığımız aynı yöntemi kullanıyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

+ [Kaydolarak](how-tos/text-analytics-how-to-signup.md) erişim anahtarı alın ve [API'yi çağırma](how-tos/text-analytics-how-to-call-api.md) adımlarını gözden geçirin.

+ [Hızlı Başlangıç](quickstarts/csharp.md), C# dilinde yazılan REST API çağrılarına ilişkin bir adım adım kılavuzdur. Minimum kodla metin göndermeyi, analiz seçmeyi ve sonuçları görüntülemeyi öğrenin. Tercih ederseniz, ile başlayabilirsiniz [Python hızlı](quickstarts/python.md) yerine.

+ Bu biraz daha ayrıntılı açıklayalım [yaklaşım analizi Öğreticisi](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) Azure Databricks kullanarak.

+ Blog gönderileri ve diğer araçlar ve teknolojiler ile metin analizi API'sini kullanma hakkında daha fazla video listemize göz atın bizim [& topluluk içeriği dış sayfa](text-analytics-resource-external-community.md).
