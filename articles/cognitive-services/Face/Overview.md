---
title: Yüz Tanıma API’si nedir?
titleSuffix: Azure Cognitive Services
description: Yüz Tanıma hizmetini kullanarak görüntülerdeki yüzleri algılamayı ve analiz etmeyi öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: c45fd508c14c368c6c9057b9fdeea8df9d8a52c3
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65905693"
---
# <a name="what-is-the-azure-face-api"></a>Azure Yüz Tanıma API’si nedir?

Azure Bilişsel hizmetler yüz tanıma API'si, görüntülerdeki İnsan yüzlerini analiz algılamak ve tanımak için kullanılan algoritmalar sağlar. İnsan yüz bilgi işleme yeteneği, birçok farklı yazılım senaryolarda önemlidir. Güvenlik, doğal kullanıcı arabirimi, içerik analizi ve yönetimi, mobil uygulamalar ve robotlara ilişkin örnek senaryolar verilmiştir.

Yüz tanıma API'si, birçok farklı işlevleri sağlar. Her işlev, aşağıdaki bölümlerde gösterilmiştir. İçin okumaya devam bunlar hakkında daha fazla bilgi edinin.

## <a name="face-detection"></a>Yüz algılama

Yüz tanıma API'si bir resimdeki İnsan yüzlerini algılar ve konumlarını dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama, yüz tanıma ile ilgili özniteliklere bir dizi ayıklayabilirsiniz. Baş poz, cinsiyet, yaş, duygu tanıma, bıyığın ve gözlük verilebilir.

> [!NOTE]
> Yüz algılama özelliğini de aracılığıyla [görüntü işleme API'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home). İsterseniz, yüz veri işlemleriyle daha da bu makalede ele alınan hizmet yüz tanıma API'sini kullanın.

![Bir kadın ve bir ADAM, yüzleri ve yaş ve görüntülenen cinsiyet geçici olarak çizilen dikdörtgenler görüntüsü](./Images/Face.detection.jpg)

Yüz algılama hakkında daha fazla bilgi için bkz. [yüz algılama](concepts/face-detection.md) kavramları makalesi. Ayrıca bkz: [algılama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) başvuru belgeleri.

## <a name="face-verification"></a>Yüz doğrulama

Doğrulama API'si, algılanan iki yüz arasında veya algılanan tek bir yüzden bir kişi nesnesine kimlik doğrulaması gerçekleştirir. Pratikte, iki yüzün aynı kişiye ait olup olmadığını değerlendirir. Bu özellik, büyük olasılıkla güvenlik senaryolarda yararlıdır. Daha fazla bilgi için [yüz tanıma](concepts/face-recognition.md) Kavramları kılavuzu veya [doğrulayın API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) başvuru belgeleri.

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Bulma benzer API, bir hedef yüz hedef yüze benzeyen yüzleri daha küçük bir kümesini bulmak için aday yüzler kümesi ile karşılaştırır. İki çalışma modu, matchPerson ve matchFace, desteklenir. Kullanarak aynı kişi için filtreler sonra benzer yüzlerden matchPerson modunu döndürür [doğrulayın API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). MatchFace modu kişinin aynı filtre yok sayar. Bu, aynı kişiye ait olmayabilir veya aday benzer yüzlerden oluşan bir liste döndürür.

Aşağıdaki örnek, hedef yüz gösterir:

![Gülümseyen kadın](./Images/FaceFindSimilar.QueryFace.jpg)

Ve aday yüzler de şunlardır:

![Beş tane gülümseyen kişi görüntüsü. Görüntüleri bir ve b aynı kişi göster.](./Images/FaceFindSimilar.Candidates.jpg)

Dört benzer yüzleri bulmak için matchPerson modunu döndürür. bir ve hedef yüz aynı kişi göster b. MatchFace modunu döndürür, b, c ve d, tam olarak dört adayları, bazı hedef aynı kişi değildir veya düşük benzerlik sahip olsa bile. Daha fazla bilgi için [yüz tanıma](concepts/face-recognition.md) Kavramları kılavuzu veya [Bul benzer API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) başvuru belgeleri.

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Tüm bir grubu yüzlerini aynı kişiye ait olasılığı düşüktür. Tek bir kişi için birkaç farklı grupları olabilir. Grupları ifade gibi başka bir faktör olarak örneğin ayrılır. Daha fazla bilgi için [yüz tanıma](concepts/face-recognition.md) Kavramları kılavuzu veya [grubu API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) başvuru belgeleri.

## <a name="person-identification"></a>Kişileri tanıma

Tanımlamak API, algılanan bir yüz kişinin bir veritabanında tanımlamak için kullanılır. Bu özellik, fotoğraf Yönetimi yazılım otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturmak ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki görüntüde "myfriends." adlı bir veritabanı örneği gösterilmektedir. Her grup, 1 milyona kadar farklı kişi nesnelerini içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Her üç satır yüz görüntülerin farklı kişiler için üç sütun içeren bir kılavuz](./Images/person.group.clare.jpg)

Bir veritabanı oluşturulur ve eğitim sonra yeni algılanan yüzleri gruba göre tanımlama gerçekleştirebilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişileri tanımlama hakkında daha fazla bilgi için bkz. [yüz tanıma](concepts/face-recognition.md) Kavramları kılavuzu veya [tanımlamak API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) başvuru belgeleri.

## <a name="use-containers"></a>Kapsayıcıları kullanma

[Yüz tanıma kapsayıcınızı kullanmak](face-how-to-install-containers.md) algılamak için tanı ve verilerinizi daha yakın standartlaştırılmış bir Docker kapsayıcısı yükleyerek yüzleri belirleyin.

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalar yüz tanıma API'sini kullanmayı birkaç yolu gösterilmektedir:

- [Microsoft yüz tanıma API'si: Windows istemci kitaplığı ve örnek](https://github.com/Microsoft/Cognitive-Face-Windows) yüz algılama, analiz ve kimlik çeşitli senaryolar gösterir bir WPF uygulaması.
- [FamilyNotes UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes) kullandığı kimlik konuşma, Cortana, mürekkep ve kamera ailesi Not paylaşımı senaryosunda yanı sıra yüz Evrensel Windows Platformu (UWP) uygulamasıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel hizmetler kaynakları gibi yüz tanıma hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin farkında olmanız gerekir. Daha fazla bilgi için [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Kodda bir yüz algılama senaryoyu uygulamak için bir hızlı başlangıcı izleyin:

- [Hızlı Başlangıç: .NET SDK'sı ile kullanarak bir resimdeki yüz algılama C# ](quickstarts/csharp.md). Diğer diller mevcut değildir.
