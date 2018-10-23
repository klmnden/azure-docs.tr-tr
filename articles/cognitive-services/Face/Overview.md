---
title: Yüz Tanıma API’si hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Bu konu başlığında Yüz Tanıma API'si ve ilgili terimler açıklanmaktadır.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/11/2018
ms.author: sbowles
ms.openlocfilehash: 6c1e0d0a51bc01c581c05e7ce3215f5501b4be76
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310411"
---
# <a name="what-is-the-face-api-service"></a>Yüz Tanıma API’si hizmeti nedir?

Yüz Tanıma API'si, görüntü ve videolardaki insan yüzlerini analiz etmek için algoritmalar sağlayan bulut tabanlı bir hizmettir. Yüz Tanıma API’si iki işlev içerir: yüz algılama ve öznitelikler ile yüz algılama.

## <a name="face-detection"></a>Yüz algılama

Yüz Tanıma API’si, bir görüntüde yüksek duyarlıklı yüz konumu ile 64’e kadar insan yüzünü algılayabilir. Görüntü bir dosya (bayt akışı) veya geçerli bir URL ile belirtilebilir.

![Genel Bakış - Yüz Algılama](./Images/Face.detection.jpg)

Görüntüdeki yüz konumunu belirten yüz dikdörtgeni (sol, üst, genişlik ve yükseklik), algılanan her bir yüzle birlikte döndürülür. İsteğe bağlı olarak, yüz algılama; poz, cinsiyet, yaş, baş pozu, yüz kılı ve gözlük gibi yüzle ilgili bir dizi özniteliği ayıklar. Daha fazla bilgi için bkz. [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)

## <a name="face-recognition"></a>Yüz tanıma

İnsan yüzlerini tanımlayabilme özelliği; güvenlik, doğal kullanıcı arabirimi, görüntü içeriği analizi ve yönetimi, mobil uygulamalar ve robotlar da dahil olmak üzere birçok senaryoda oldukça önemlidir. Yüz Tanıma API'si dört yüz tanıma işlevi sağlar: yüz doğrulama, benzer yüzleri bulma, yüz gruplama ve kişi tanımlama.

### <a name="face-verification"></a>Yüz doğrulama

Yüz doğrulama, tek bir algılanan yüzden bir kişi nesnesine veya algılanan iki yüze karşı kimlik doğrulaması gerçekleştirir. Daha ayrıntılı bilgi için bkz. [Yüz - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-faces"></a>Benzer yüzleri bulma

Hedef bir algılanan yüz ve arama yapılacak bir dizi aday yüzü varken hizmet, hedef yüze en benzer görünen küçük bir dizi yüz bulur. **matchFace** ve **matchPerson** olmak üzere iki çalışma modu desteklenir. **matchPerson** modu, [Yüz - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) API’sinden türetilen aynı kişi eşiği uygulandıktan sonra benzer yüzleri döndürür. **matchFace** modu, aynı kişi eşiğini yoksayar ve en benzer aday yüzlerini döndürür. Aşağıdaki örnekte aday yüzleri listelenmiştir.
![Genel Bakış - Benzer Yüzleri Bulma](./Images/FaceFindSimilar.Candidates.jpg) Sorgulanan yüz budur.
![Genel Bakış - Benzer Yüzleri Bulma](./Images/FaceFindSimilar.QueryFace.jpg)

Dört benzer yüzü bulmak için **matchPerson** modu, sorgu yüzü olarak aynı kişiyi tanımlayan (a) ve (b) girişlerini döndürür. **matchFace** modu, bazılarında benzerlik düşük olsa da tam olarak dört adayı, başka bir deyişle (a), (b), (c) ve (d) öğesini döndürür. Daha fazla bilgi için bkz. [Yüz - Benzer Olanları Bulma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Yüz gruplama

Bir dizi bilinmeyen yüz varken yüz gruplama API’si otomatik olarak bunları benzerliğe göre birçok gruba ayırır. Her grup, özgün bilinmeyen yüz kümesinin dağınık bir alt kümesi olup benzer yüzler içerir. Aynı gruptaki tüm yüzler, aynı kişiye ait olarak değerlendirilebilir. Daha fazla bilgi için bkz. [Yüz - Gruplama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)

### <a name="person-identification"></a>Kişileri tanıma

Yüz Tanıma API’si, algılanan bir yüze ve bir kişi veritabanına göre kişileri belirlemek için kullanılabilir. Bu veritabanını önceden oluşturabilir ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki şekil, "myfriends" adlı bir veritabanı örneğidir. Her grup en fazla 1.000.000/10.000 farklı kişi nesnesi içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Genel Bakış - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Bir veritabanı oluşturulup eğitildikten sonra, gruba ve yeni bir algılanan yüze karşı tanımlama gerçekleştirilebilir. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi tanımlaması hakkında daha fazla bilgi için aşağıdaki API kılavuzlarına bakın:

[Yüz - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)  
[PersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)  
[PersonGroup Person - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)  
[PersonGroup - Eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)  
[LargePersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)  
[LargePersonGroup Person - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)  
[LargePersonGroup - Eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)  

#### <a name="face-storage-and-pricing"></a>Yüz depolama ve fiyatlandırma

Yüz Depolama, Yüz Tanıma API’si ile tanımlama veya benzerlik eşleştirmesi için LargePersonGroup/PersonGroup Kişisi nesnelerini ([PersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) veya LargeFaceList/FaceList nesnelerini ([FaceList - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) kullanırken Standart bir aboneliğin ek kalıcı yüzleri depolamasına olanak sağlar. Depolanan görüntüler, 1000 yüz başına 0,50 ABD doları üzerinden ücretlendirilir ve bu fiyat, günlere eşit olarak dağıtılır. Ücretsiz katman abonelikleri toplam 1.000 kişiyle sınırlıdır.

Örneğin, hesabınızda ayın ilk yarısı boyunca her gün için 10.000 adet kalıcı yüz kullanılıp, ayın ikinci yarısı boyunca hiç kalıcı yüz kullanmadıysa, yalnızca depolama yapılan günlerdeki 10.000 adet yüz için faturalandırılırsınız. Alternatif olarak, ayın her günü 1.000 adet yüzü birkaç saatliğine kalıcı hale getirmeniz ve bunları her gece silmeniz durumunda da günlük 1.000 adet kalıcı yüz için faturalandırılırsınız.

## <a name="sample-apps"></a>Örnek uygulamalar

Yüz Tanıma API’sinden yararlanan şu örnek uygulamalara bakın.

- [Microsoft Yüz Tanıma API’si: Windows İstemci Kitaplığı ve Örneği](https://github.com/Microsoft/Cognitive-Face-Windows)
  - Birçok Yüz algılama, analiz ve tanımlama senaryosunu gösteren WPF örnek uygulaması.
- [FamilyNotes UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Senaryoyu paylaşan bir aile notu aracılığıyla konuşma, Cortana, mürekkep ve kamera kullanımını gösteren Evrensel Windows Platformu (UWP) örnek uygulaması.
- [Video Karesi Analizi Örneği](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Yüz Tanıma, Görüntü İşleme ve Duygu Tanıma API’lerini kullanarak gerçek zamanlıya yakın canlı video akışlarının analizini gösteren Win32 örnek uygulaması.

## <a name="tutorials"></a>Öğreticiler
Aşağıdaki öğreticilerde, Yüz Tanıma API’sinin temel işlevleri ve abonelik işlemleri gösterilmektedir:
- [CSharp’ta Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinCSharpTutorial.md)
- [Android için Java’da Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Python’da Yüz Tanıma API’sini Kullanmaya Başlama Öğreticisi](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

Basit bir Yüz Tanıma API'si senaryosu uygulamak için bir hızlı başlangıcı deneyin.
- [Hızlı Başlangıç: C# kullanarak bir görüntüdeki yüzleri algılama](quickstarts/csharp.md) (başka diller de mevcuttur)
