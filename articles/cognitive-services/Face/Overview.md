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
ms.openlocfilehash: 2f5f57f0978adbdf33ed4ce25ba9b32247ea0484
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56455985"
---
# <a name="what-is-the-azure-face-api"></a>Azure Yüz Tanıma API’si nedir?

Azure Yüz Tanıma API'si görüntülerdeki insan yüzlerini algılamaya, tanımaya ve analiz etmeye yönelik algoritmalar sağlayan bilişsel bir hizmettir. İnsan yüzü bilgilerini işleyebilme özelliği; güvenlik, doğal kullanıcı arabirimi, görüntü içeriği analizi ve yönetimi, mobil uygulamalar ve robotlar gibi birçok farklı yazılım senaryosunda oldukça önemlidir.

Yüz Tanıma API'si farklı işlevler sağlar. Bunlardan her biri aşağıdaki bölümlerde ele alınmıştır. İçin okumaya devam her hakkında daha fazla bilgi edinin.

## <a name="face-detection"></a>Yüz algılama

Yüz Tanıma API'si görüntüdeki insan yüzlerini algılayabilir ve bunların konumunu gösteren dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama, yüz tanıma ile ilgili özniteliklere poz, baş poz, cinsiyet, yaş, duygu tanıma, bıyığın ve gözlük gibi bir dizi ayıklayabilirsiniz.

![Yüzlerinin çevresine dikdörtgenler çizilmiş, yaş ve cinsiyetlerin de görüntülendiği bir kadın ve erken resmi](./Images/Face.detection.jpg)

> [!NOTE] 
> Yüz algılama özelliği [Görüntü İşleme API'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) aracılığıyla da sağlanır ama yüz verileriyle başka işlemler de yapmak isterseniz Yüz Tanıma API'sini (bu hizmet) kullanmalısınız. 

Yüz algılama hakkında daha fazla bilgi için bkz. [Algılama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Yüz doğrulama

Doğrulama API'si, algılanan iki yüz arasında veya algılanan tek bir yüzden bir kişi nesnesine kimlik doğrulaması gerçekleştirir. Pratikte, iki yüzün aynı kişiye ait olup olmadığını değerlendirir. Güvenlik senaryolarında bu yaralı olabilir. Daha fazla bilgi için bkz. [Doğrulama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Benzer Bulma API'si bir hedef yüz ile bir dizi aday yüzü alır ve hedef yüze en çok benzeyenlerle daha küçük bir yüz kümesi bulur. **matchPerson** ve **matchFace** olmak üzere iki çalışma modu desteklenir. **matchPerson** modu aynı kişi için filtreleme yaptıktan sonra benzer yüzleri döndürür ([Doğrulama API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) kullanır). **matchFace** modu aynı kişi filtresini yoksayar ve aynı kişiye ait olabilecek veya olmayabilecek benzer aday yüzlerin listesini döndürür.

Aşağıdaki örnekte, hedef yüz şudur:

![Gülümseyen kadın](./Images/FaceFindSimilar.QueryFace.jpg)

Ve aday yüzler de şunlardır:

![Beş tane gülümseyen kişi görüntüsü. a) ve b) görüntüleri aynı kişiye aittir](./Images/FaceFindSimilar.Candidates.jpg)

Dört benzer yüzü bulmak için **matchPerson** modu, hedef yüz olarak aynı kişiyi gösteren (a) ve (b) girişlerini döndürür. **matchFace** modu, bazıları hedefle aynı kişiye ait olmasa veya benzerlik düşük olsa bile tam olarak dört adayı, başka bir deyişle (a), (b), (c) ve (d) öğesini döndürür. Daha fazla bilgi için bkz. [Benzer Bulma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Gruptaki yüzlerin tümü büyük olasılıkla aynı kişiye aittir ama tek bir kişi için birkaç farklı grup olabilir (başka bir faktöre, örneğin ifadeye göre ayrıştırılabilir). Daha fazla bilgi için bkz. [Gruplama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Kişileri tanıma

Tanımlama API'si, algılanan yüzü bir kişi veritabanına göre tanımlamak için kullanılabilir. Bu özellik, fotoğraf yönetimi yazılımında otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturabilir ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki görüntüde "myfriends" adlı bir veritabanı örneği gösterilir. Her grup en çok 1.000.000 farklı kişi nesnesi içerebilir ve her kişi nesnesinin kaydedilmiş en çok 248 yüzü olabilir.

![Farklı kişiler için 3 sütunu ve her kişi için 3 yüz görüntüsü satırı olan bir kılavuz](./Images/person.group.clare.jpg)

Veritabanı oluşturulup eğitildikten sonra, yeni algılanan yüzün bulunduğu grupta tanımlama yapabilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi tanımlama hakkında daha fazla bilgi için bkz. [Tanımlama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="use-containers"></a>Kapsayıcıları kullanma

[Yüz tanıma kapsayıcınızı kullanmak](face-how-to-install-containers.md) algılamak için tanı ve yüzleri, verilerinizi daha yakın standartlaştırılmış bir Docker kapsayıcısı yükleyerek belirleyin.

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalarda, Yüz Tanıma API'sinin kullanılabileceği birkaç yol gösterilir.

- [Microsoft yüz tanıma API'si: Windows istemci kitaplığı ve örnek](https://github.com/Microsoft/Cognitive-Face-Windows) -yüz algılama, analiz ve kimlik çeşitli senaryolar gösterir bir WPF uygulaması.
- [FamilyNotes UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes) - Bir aile not paylaşım senaryosunda yüz tanımlamayı konuşma, Cortana, mürekkep ve kamerayla birlikte kullanan Evrensel Windows Platformu (UWP) uygulaması.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel hizmetler gibi yüz tanıma hizmeti kullanan geliştiriciler Microsoft'un müşteri verilerini ilkeleri haberdar olmanız gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/en-us/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Kodda basit bir yüz algılama senaryosu uygulamak için hızlı başlangıcı izleyin.
- [Hızlı Başlangıç: .NET SDK'sı ile kullanarak görüntüdeki yüzleri algılayın C# ](quickstarts/csharp.md) (diğer diller kullanılabilir)
