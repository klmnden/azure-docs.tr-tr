---
title: Yüz API hizmetine genel bakış | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Terimler sözlüğü açıklanmaktadır yüz API hizmetiyle çalışacak şekilde karşılaşabileceğiniz.
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: fb1d14ff80bf53adc3008d79cc998739ffffde1b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048678"
---
# <a name="what-is-face-api"></a>Yüz Tanıma API'si nedir?

Microsoft yüz API en gelişmiş yüz algoritmaları sağlayan bulut tabanlı bir hizmet Hoş Geldiniz. Yüz API iki ana işlevleri vardır: yüz algılama özniteliklere sahip ve yüz tanıma.

## <a name="face-detection"></a>Yüz Algılama

Yüz API Yüksek duyarlılık yüz konumla en fazla 64 İnsan yüzeyleri görüntüdeki algılar. Ve görüntü dosya bayt ya da geçerli bir URL ile belirtilebilir.

![Genel Bakış - yüz algılama](./Images/Face.detection.jpg)

Yüz dikdörtgen (sol, üst, genişlik ve yükseklik) görüntü yüz konumda her yanı sıra döndürülür gösteren yüz algılandı. İsteğe bağlı olarak, yüz algılama yüz ilgili öznitelikleri poz, cinsiyetiniz, geçerlilik süresi, head poz, yüz artı ve gözlük gibi bir dizi ayıklar. Daha fazla bilgi için bkz: [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Yüz tanıma

Yüz tanıma yaygın olarak güvenlik, doğal kullanıcı arabirimi, görüntü içerik çözümlemesi ve yönetimi, mobil uygulamaları ve robotics dahil olmak üzere birçok senaryolarda kullanılır. Dört yüz tanıma işlevler sunulur: yüz doğrulama, bulma benzer yazıtipleri, yüz gruplandırma ve kişi kimliği.

### <a name="face-verification"></a>Yüz Doğrulama

Yüz API doğrulama bir iki algılanan yüzeyleri karşı kimlik doğrulaması veya kimlik bir kişinin nesneye bir algılanan yüz gerçekleştirir. Daha ayrıntılı bilgi için bkz: [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Benzer yüz bulma

Verilen bir hedef algılanan yüz ve bulmak için aday yüzeyleri kümesi, hizmet için hedef yüz en benzer yazıtipleri, küçük bir kümesini bulur. İki çalışma modu, `matchFace` ve `matchPerson` desteklenir. `matchPerson` modunu döndürür benzer yüzeyleri türetilmiş aynı kişi eşik uyguladıktan sonra [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). `matchFace` mod aynı kişi eşik yoksayar ve üst benzer adayı yüzeyleri döndürür. Aşağıdaki örnek almak, aday yüzeyleri listelenir.
![Genel Bakış - yüz Benzerini Bul](./Images/FaceFindSimilar.Candidates.jpg) ve sorgu yüz ![genel bakış - yüz Benzerini Bul](./Images/FaceFindSimilar.QueryFace.jpg)

Dört benzer yüz bulmak için `matchPerson` modu döndürür (a) ve (b sorgu yüz ile aynı kişiye ait). `matchFace` mod (a), (b), (c) ve (d), tam olarak dört adayları düşük durumunda bile benzerlik döndürür. Daha fazla bilgi için bkz: [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Yüz Gruplama

Bilinmeyen yüzeyleri bir kümesi düşünüldüğünde, API otomatik olarak gruplandırma yüz bunları üzerinde benzerliğe göre birkaç gruba böler. Her, özgün bilinmeyen yazıtipinin ayrık uygun bir alt ayarlama ve benzer yüzeyleri içeren grubudur. Ve aynı gruptaki tüm yüzeyleri aynı kişiye nesnesine ait kabul edilebilir. Daha fazla bilgi için bkz: [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Yüz Tanımlama

Yüz API algılanan yüzey üzerinde temel Kişiler ve (LargePersonGroup/PersonGroup tanımlanan) kişiler veritabanı tanımlamak için kullanılabilir. Bu veritabanını önceden oluşturmak, zaman içinde düzenlenebilir.

Aşağıdaki şekil, "myfriends" adlı bir LargePersonGroup/PersonGroup örneğidir. Her Grup 1.000.000/10.000 kişi nesneleri içerebilir. Bu sırada, her kişi nesnesi kayıtlı kadar 248 yüzeyleri sahip olabilir.

![Genel Bakış - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

LargePersonGroup/PersonGroup oluşturup eğitilmiş sonra tanımlama grubu ve yeni algılanan yüz karşı gerçekleştirilebilir. Yüz Grup kişi nesne olarak tanımlanır, kişi nesnesi döndürülür.

Kişi tanımlama hakkında daha fazla bilgi için aşağıdaki API kılavuzlara bakın:

[Yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 
 [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - eğitimi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Yüz Depolama

Yüz depolama LargePersonGroup/PersonGroup kişi nesneleri kullanırken kalıcı yüzeyleri ek depolamak standart bir abonelik sağlar ([PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup kişi - Ekleme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) veya LargeFaceLists/FaceLists ([FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) kimliği veya yüz API ile eşleşen benzerlik. Saklı görüntüleri 0,5 başına 1000 yüzeyleri adresindeki ücretlendirilen ve bu oran günlük olarak eşit olarak bölünür. Ücretsiz katmanı abonelikleri ücretsiz ancak 1.000 toplam kişi sınırlıdır.

Yüz depolama için fiyatlandırma günlük eşit olarak bölünür. Örneğin, ayın ilk yarı ve hiçbiri için 10.000 kalıcı yüzeyleri hesabınızı her gün kullanılan ikinci yarısındaki gün boyunca yalnızca 10.000 yüzeyleri için fatura depolanır. Her gün ay sırasında birkaç saat boyunca 1.000 yüzeyleri kalıcı ve bunları her gece silin, alternatif olarak, hala 1.000 kalıcı yüzeyleri için her gün fatura.

## <a name="getting-started-tutorials"></a>Öğreticiler Başlarken

Aşağıdaki öğreticiler yüz API temel işlevleri ve abonelikleri işlemleri gösterilmektedir:

- [CSharp öğreticide API yüzeyi ile çalışmaya başlama](Tutorials/FaceAPIinCSharpTutorial.md)
- [Android öğretici için Java API yüzeyi ile Başlarken](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Python öğreticide API yüzeyi ile çalışmaya başlama](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Örnek Uygulamalar

Yüz API'si kullanan bu örnek uygulamalar göz atın.

- [Microsoft yüz API: Windows istemci kitaplığı ve örnek](https://github.com/Microsoft/Cognitive-Face-Windows)
  - Yüz algılama, analiz ve kimlik birkaç senaryo gösterilmektedir WPF örnek uygulaması.
- [FamilyNotes UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Konuşma, Cortana, mürekkep ve senaryo paylaşımı ailesi Not aracılığıyla kamera kullanımını gösteren Evrensel Windows Platformu (UWP) örnek uygulaması.
- [Video çerçeve analizi örneği](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Evrensel Windows Platformu (UWP) örnek uygulaması canlı video akışları yüz, bilgisayar görme ve duygu tanıma API'leri kullanarak yakın gerçek zamanlı analiz etme gösterir.

## <a name="related-topics"></a>İlgili Konular

- [Yüz API sürümü 1.0 sürüm notları](ReleaseNotes.md)
- [Görüntüde nasıl algılanacağını yüzler](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Görüntüde yüzeyleri belirleme](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
