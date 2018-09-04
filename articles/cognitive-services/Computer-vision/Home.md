---
title: Bilişsel Hizmetler İçin Görüntü İşleme | Microsoft Docs
description: Azure Bilişsel Hizmetler'de görüntüleri işlemenize ve bilgiler döndürmenize yardımcı olması için Görüntü İşleme'de gelişmiş algoritmalar kullanın.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: v-deken
ms.openlocfilehash: ec3ffa8599192ecd9a4092b026a8d249f50c64c9
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248616"
---
# <a name="what-is-computer-vision"></a>Görüntü İşleme nedir?

Bulut tabanlı Görüntü İşleme Hizmeti, geliştiricilerin, görüntüleri işlemeye ve bilgiler döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar. Görüntü İşleme yalnızca JPEG ve PNG gibi popüler görüntü biçimleriyle çalışır. Bir görüntüyü analiz etmek için görüntü yükleyebilir veya görüntü URL'si belirtebilirsiniz. Görüntü İşleme algoritmaları sayesinde bir görüntünün içeriği, ihtiyaç duyduğunuz görsel özelliklere bağlı olarak çeşitli şekillerde analiz edilebilir. Örneğin; Görüntü İşleme, bir görüntüde yetişkinlere yönelik veya müstehcen içerik bulunup bulunmadığını belirleyebilir ya da görüntüdeki tüm yüzleri bulabilir.

Hizmeti çağırmak için [istemci kitaplıklarımızı](quickstarts-sdk/csharp-analyze-sdk.md) kullanarak veya aşağıdakileri gerçekleştirmek için [REST API](vision-api-how-to-topics/howtocallvisionapi.md)'yi doğrudan çağırarak Görüntü İşleme'yi uygulamanızda kullanabilirsiniz:

- [İçgörü edinmek için görüntüleri analiz etme](#analyzing-images-for-insight)
- [Görüntülerdeki metinleri ayıklama](#extracting-text-from-images)
- [Görüntü içeriklerini denetleme](#moderating-content-in-images)

## <a name="analyzing-images-for-insight"></a>İçgörü edinmek için görüntüleri analiz etme

Görüntülerinizin görsel özelliklerini ve niteliklerini algılamak ve bunlar hakkında içgörüler sunmak için Görüntü İşleme'yi kullanarak görüntüleri analiz edebilirsiniz. Yerel görüntüleri analiz etmek üzere bir görüntünün içeriklerini karşıya yükleyebilir veya uzak görüntüleri analiz etmek üzere bir görüntünün URL'sini belirtebilirsiniz.

Görüntü İşleme bir görüntüyü analiz ederken şu eylemleri gerçekleştirebilir:

| Eylem | Açıklama |
| ------ | ----------- |
|**[Görsel özellikleri etiketleme](quickstarts/csharp-analyze.md)**|2.000'i aşkın tanınabilir nesne, canlı, manzara ve eylem temelinde, bir görüntüde yer alan görsel özellikleri tanımlayın ve etiketleyin. Belirsiz veya herkesçe bilinmeyen etiketler söz konusu olduğunda, sunulan yanıt, etiketin anlamının bilinen bir ortama ilişkin bağlamda açıklığa kavuşturulması için "ipuçları" sağlar. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzer öğeleri de kapsar.|
|**[Bir görüntüyü kategorilere ayırma](quickstarts/csharp-analyze.md)**|Üst/alt öğe kalıtım hiyerarşileri içeren bir [kategori sınıflandırması](Category-Taxonomy.md) kullanarak bir görüntüyü bütünüyle tanımlayın ve kategorilere ayırın. Kategoriler tek başına veya yeni etiketleme modellerimizle birlikte kullanılabilir.<br/>Şu anda, görüntülerin etiketlenmesi ve kategorilere ayrılması için yalnızca İngilizce desteklenmektedir.|
|**[Bir görüntüyü açıklama](quickstarts/csharp-analyze.md)**|Tam cümleler kullanarak bir görüntünün tamamı için okunabilir açıklamalar oluşturun. Görüntü İşleme algoritmaları, görüntüde tanımlanan nesneleri temel alan çeşitli açıklamalar oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.<br/>Resim yazıları oluşturmak üzere bu teknolojinin kullanıldığı bir bot örneğine [GitHub](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption)'dan ulaşılabilir.|
|**[Yüz algılama](quickstarts/csharp-analyze.md)** |Bir görüntüdeki yüzleri algılayın ve algılanan her bir yüz için bilgiler sunun. Görüntü İşleme algılanan her bir yüz için koordinatları, dikdörtgeni, cinsiyeti ve yaşı döndürür.<br/>Görüntü İşleme, [Yüz Tanıma](/azure/cognitive-services/face/)'da bulunan işlevlerin bir alt kümesini sunar ve Yüz tanımanın yanı sıra poz algılama gibi daha ayrıntılı analiz işlemleri için Yüz Tanıma hizmetini kullanabilirsiniz.|
|**[Görüntü türünü algılama](quickstarts/csharp-analyze.md)**|Bir görüntü ile ilgili özellikleri (görüntünün çizim olup olmaması veya küçük resim olup olmama olasılığı gibi) algılayın.|
|**[Etki alanına özgü içeriği algılama](quickstarts/python-domain.md)**|Bir görüntüde yer alan, ünlüler ve önemli yerler gibi etki alanına özgü içerikleri algılamak ve tanımak için etki alanı modelleri kullanın. Örneğin; bir görüntüde insanlar yer alıyorsa Görüntü İşleme, görüntüde algılanan kişilerin ünlü olup olmadığını belirlemek üzere hizmetle birlikte ünlülere yönelik bir etki alanı modeli kullanabilir.|
|**[Renk düzenini algılama](quickstarts/csharp-analyze.md)**|Bir görüntüdeki renk kullanımını analiz edin. Görüntü İşleme, bir görüntünün siyah-beyaz olup olmadığını belirlemenin yanı sıra renkli görüntüler için baskın renkleri ve vurgu renklerini tanıyabilir.|
|**[Küçük resim oluşturma](quickstarts/csharp-thumb.md)**|Görüntülere uygun küçük resimler oluşturmak üzere söz konusu görüntülerin içeriklerini analiz edin. Görüntü İşleme öncelikle yüksek kalitede bir küçük resim oluşturur ve ardından, *ilgi bölgesini* (ROI) belirlemek için görüntüdeki nesneleri analiz eder. Bu işlemden sonra, Görüntü İşleme, görüntüyü ilgili bölgesine ilişkin gereksinimlere uyacak şekilde kırpar. Oluşturulan küçük resim, ihtiyaçlarınız doğrultusunda, özgün görüntünün en boy oranından farklı bir en boy oranı kullanılarak sunulabilir.|

## <a name="extracting-text-from-images"></a>Görüntülerdeki metinleri ayıklama

Bir görüntüdeki [metinleri OCR kullanarak](quickstarts/csharp-print-text.md) makine tarafından okunabilen bir karakter akışı halinde ayıklamak üzere Görüntü İşleme'yi kullanabilirsiniz. Gerekirse, OCR, tanınan metnin yönünü yatay görüntü ekseninde derece olarak düzeltir ve her bir sözcük için çerçeve koordinatlarını verir. OCR 25 dili destekler ve ayıklanan metnin dilini otomatik olarak algılar.

Ayrıca görüntülerdeki [basılı veya el yazısıyla yazılmış metinleri de algılayabilirsiniz](quickstarts/csharp-hand-text.md). Görüntü İşleme; makbuz, poster, kartvizit, mektup ve beyaz tahtalar gibi farklı yüzeylere ve arka planlara sahip çeşitli nesnelerin görüntülerindeki basılı ve el yazısıyla yazılmış metinleri algılayabilir ve ayıklayabilir. Şu anda, basılı ve el yazısıyla yazılmış metinleri tanıma özelliği önizlemededir ve yalnızca İngilizce dilinde kullanılabilmektedir.  

## <a name="moderating-content-in-images"></a>Görüntü içeriklerini denetleme

Görüntü İşleme'yi, görüntüde yetişkinlere yönelik veya müstehcen içerik bulunup bulunmama olasılığını derecelendirerek ve her ikisi için güvenilirlik puanı oluşturarak [yetişkinlere yönelik veya müstehcen içerikleri algılamak](quickstarts/csharp-analyze.md) üzere kullanabilirsiniz. Yetişkinlere yönelik veya müstehcen içeriklerin algılanmasına yönelik filtre, tercihleriniz doğrultusunda bir kaydırıcı ölçeği üzerinde ayarlanabilir.

## <a name="image-requirements"></a>Görüntü gereksinimleri

Görüntü İşleme, şu gereksinimleri karşılayan görüntüleri analiz edebilir:

- Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntünün boyutlarının 50 x 50 pikselden büyük olması gerekir  
  OCR için, görüntünün boyutlarının 40 x 40 ve 3200 x 3200 piksel arasında olması ve görüntünün, 10 megapikselden büyük olmaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Hızlı başlangıçlarımızdan biriyle Görüntü İşleme'yi kullanmaya başlayın:

- [Resim çözümleme](/quickstarts-sdk/csharp-analyze-sdk.md)
- [El yazısı metni ayıklama](/quickstarts-sdk/csharp-hand-text-sdk.md)
- [Küçük resim oluşturma](/quickstarts-sdk/csharp-thumb-sdk.md)
