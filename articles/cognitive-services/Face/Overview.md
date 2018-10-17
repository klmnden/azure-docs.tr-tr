---
title: Yüz Tanıma API’si Hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Sözlükte, Yüz Tanıma API’si Hizmeti ile çalışırken karşılaşabileceğiniz terimler açıklanmaktadır.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 15de899be5ab85e9fe84ba1b6284bc9419fcf8a1
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123479"
---
# <a name="what-is-the-face-api-service"></a>Yüz Tanıma API’si Hizmeti nedir?

En gelişmiş yüz tanıma algoritmalarını sağlayan bulut tabanlı bir hizmet olan Yüz Tanıma API’si Hizmeti’ne hoş geldiniz. Yüz Tanıma API’si iki işlev içerir: yüz algılama ve öznitelikler ile yüz algılama.

## <a name="face-detection"></a>Yüz Algılama

Yüz Tanıma API’si, bir görüntüde yüksek duyarlıklı yüz konumu ile 64’e kadar insan yüzünü algılar. Ayrıca görüntü, dosya tarafından bayt olarak veya geçerli URL’de belirtilebilir.

![Genel Bakış - Yüz Algılama](./Images/Face.detection.jpg)

Görüntüdeki yüz konumunu belirten yüz dikdörtgeni (sol, üst, genişlik ve yükseklik), algılanan her bir yüzle birlikte döndürülür. İsteğe bağlı olarak, yüz algılama; poz, cinsiyet, yaş, baş pozu, yüz kılı ve gözlük gibi yüzle ilgili bir dizi özniteliği ayıklar. Daha fazla bilgi için bkz. [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)

## <a name="face-recognition"></a>Yüz Tanıma

Yüz tanıma; güvenlik, doğal kullanıcı arabirimi, görüntü içeriği analizi ve yönetimi, mobil uygulamalar ve robotlar da dahil olmak üzere birçok senaryoda yaygın olarak kullanılır. Dört yüz tanıma işlevi sağlanır: yüz doğrulama, benzer yüzleri bulma, yüz gruplama ve kişi tanımlama.

### <a name="face-verification"></a>Yüz Doğrulama

Yüz Tanıma API’si doğrulaması, tek bir algılanan yüzden bir kişi nesnesine kimlik doğrulaması veya algılanan iki yüze karşı kimlik doğrulaması gerçekleştirir. Daha ayrıntılı bilgi için bkz. [Yüz - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Benzer Yüzleri Bulma

Hedef bir algılanan yüz ve arama yapılacak bir dizi aday yüzü varken hizmet, hedef yüze en benzer görünen küçük bir dizi yüz bulur. İki çalışma modu (`matchFace` ve `matchPerson`) desteklenir. `matchPerson`, [Yüz - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) API’sinden türetilen aynı kişi eşiği uygulandıktan sonra benzer yüzleri döndürür. `matchFace` modu, aynı kişi eşiğini yoksayar ve en benzer aday yüzlerini döndürür. Aşağıdaki örneği düşünün, burada aday yüzleri listelenir.
![Genel Bakış - Benzer Yüzleri Bulma](./Images/FaceFindSimilar.Candidates.jpg) Sorgu yüzü: ![Genel Bakış - Benzer Yüzleri Bulma](./Images/FaceFindSimilar.QueryFace.jpg)

Dört benzer yüzü bulmak için `matchPerson` modu, sorgu yüzü ile aynı kişiye ait olan (a) ve (b) öğesini döndürür. `matchFace` modu, benzerlik düşük olsa da tam olarak dört adayı, başka bir deyişle (a), (b), (c) ve (d) öğesini döndürür. Daha fazla bilgi için bkz. [Yüz - Benzer Olanları Bulma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Yüz Gruplama

Bir dizi bilinmeyen yüz varken yüz gruplama API’si otomatik olarak bunları benzerliğe göre birçok gruba ayırır. Her grup, özgün bilinmeyen yüz kümesinin dağınık bir alt kümesi olup benzer yüzler içerir. Ayrıca aynı gruptaki tüm yüzler, aynı kişi nesnesine ait olarak değerlendirilebilir. Daha fazla bilgi için bkz. [Yüz - Gruplama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)

### <a name="face-identification"></a>Yüz Belirleme

Yüz Tanıma API’si, algılanan bir yüze ve bir kişi veritabanına (LargePersonGroup/PersonGroup olarak tanımlanır) göre kişileri belirlemek için kullanılabilir. Bu veritabanını önceden oluşturun; veritabanı zaman içinde düzenlenebilir.

Aşağıdaki şekil, "myfriends" adlı bir LargePersonGroup/PersonGroup örneğidir. Her grup en fazla 1.000.000/10.000 kişi nesnesi içerebilir. Bu arada, her kişi nesnesinin en fazla 248 kayıtlı yüzü olabilir.

![Genel Bakış - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Bir LargePersonGroup/PersonGroup oluşturulup eğitildikten sonra, gruba ve yeni bir algılanan yüze karşı tanımlama gerçekleştirilebilir. Yüz, grupta bir kişi nesnesi olarak belirlenirse kişi nesnesi döndürülür.

Kişi tanımlaması hakkında daha fazla bilgi için aşağıdaki API kılavuzlarına bakın:

[Yüz - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup Kişisi - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
[LargePersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup Kişisi - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Yüz Depolama

Yüz Depolama, Yüz Tanıma API’si ile tanımlama veya benzerlik eşleştirmesi için LargePersonGroup/PersonGroup Kişisi nesnelerini ([PersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) veya LargeFaceList/FaceList nesnelerini ([FaceList - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) kullanırken Standart bir aboneliğin ek kalıcı yüzleri depolamasına olanak sağlar. Depolanan görüntüler, 1000 yüz başına 0,5 $ üzerinden ücretlendirilir ve bu fiyat, günlere eşit olarak dağıtılır. Ücretsiz katman abonelikleri ücretsizdir, ancak toplam 1.000 kişiyle sınırlıdır.

Yüz Depolama için Fiyatlandırma günlere eşit olarak dağıtılır. Örneğin, hesabınızda ayın ilk yarısı boyunca her gün için 10.000 adet kalıcı yüz kullanılıp, ayın ikinci yarısı boyunca hiç kalıcı yüz kullanmadıysa, yalnızca depolama yapılan günlerdeki 10.000 adet yüz için faturalandırılırsınız. Alternatif olarak, ayın her günü 1.000 adet yüzü birkaç saatliğine kalıcı hale getirmeniz ve bunları her gece silmeniz durumunda da günlük 1.000 adet kalıcı yüz için faturalandırılırsınız.

## <a name="getting-started-tutorials"></a>Başlangıç Öğreticileri

Aşağıdaki öğreticilerde, Yüz Tanıma API’si temel işlevleri ve abonelik işlemleri gösterilmektedir:

- [CSharp’ta Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinCSharpTutorial.md)
- [Android için Java’da Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Python’da Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Örnek Uygulamalar

Yüz Tanıma API’sinden yararlanan şu örnek uygulamalara bakın.

- [Microsoft Yüz Tanıma API’si: Windows İstemci Kitaplığı ve Örneği](https://github.com/Microsoft/Cognitive-Face-Windows)
  - Birçok Yüz algılama, analiz ve tanımlama senaryosunu gösteren WPF örnek uygulaması.
- [FamilyNotes UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Senaryoyu paylaşan bir aile notu aracılığıyla konuşma, Cortana, mürekkep ve kamera kullanımını gösteren Evrensel Windows Platformu (UWP) örnek uygulaması.
- [Video Karesi Analizi Örneği](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Yüz Tanıma, Görüntü İşleme ve Duygu Tanıma API’lerini kullanarak gerçek zamanlıya yakın canlı video akışlarının analizini gösteren Evrensel Windows Platformu (UWP) örnek uygulaması.

## <a name="related-topics"></a>İlgili Konular

- [Yüz Tanıma API’si Sürüm 1.0 Yayın Notları](ReleaseNotes.md)
- [Görüntüdeki Yüzleri Algılama](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Görüntüdeki Yüzleri Belirleme](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
