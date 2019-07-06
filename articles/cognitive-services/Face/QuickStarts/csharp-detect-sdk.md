---
title: "Hızlı Başlangıç: Azure yüz .NET SDK'sı ile görüntüdeki yüzleri algılayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure yüz SDK'sı ile kullanacağınız C# görüntüdeki yüzleri algılamak için.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: bf4d11a18932d20e7dcc7580ebe5aa4e060c5a88
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606770"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-net-sdk"></a>Hızlı Başlangıç: Yüz tanıma .NET SDK'sı ile bir resimdeki yüz algılama

Bu hızlı başlangıçta, yüz tanıma kullanacağı hizmeti SDK'sı ile C# bir resimdeki İnsan yüzlerini algılamak için. Yüz tanıma projede bu hızlı başlangıçtaki kod çalışma örneği için bkz [Bilişsel hizmetler görüntü csharp hızlı başlangıçlar](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face) github deposu.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da yeni bir oluşturma **konsol uygulaması (.NET Framework)** adlandırın ve proje **FaceDetection**. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Tıklayın **Gözat** sekmenize **ön sürümü dahil et**; ardından bulun ve aşağıdaki paketi yükleyin:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)
1. Projeniz için tüm NuGet paketlerini en son sürümlerine yüklediğinizden emin olun. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Tıklayın **güncelleştirmeleri** sekmesini ve en son sürümlerini görünen herhangi bir paket yükleyin.

## <a name="add-face-detection-code"></a>Yüz algılama kodu ekleyin

Yeni projenin açın *Program.cs* dosya. Burada, görüntülerini yükle ve yüz algılama için gereken kodu ekleyeceksiniz.

### <a name="include-namespaces"></a>Ad alanlarını ekleme

Aşağıdaki `using` deyimlerini *Program.cs* dosyanızın en üstüne ekleyin.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=1-7)]

### <a name="add-essential-fields"></a>Gerekli alanları ekleyin

Ekleme **Program** sınıfı şu alanlara sahip. Bu veriler yüz tanıma Hizmeti'ne bağlanmayı ve giriş verilerinin alınacağı belirtir. Güncellemeniz gerekecektir `subscriptionKey` abonelik anahtarınız ve değerini bir alanla değiştirme gerekebilir `faceEndpoint` doğru bölge tanımlayıcısı içeren dize. Ayrıca ayarlamanız gerekir `localImagePath` ve/veya `remoteImageUrl` gerçek işaret yollara değerleri görüntü dosyaları.

`faceAttributes` Belirli tür öznitelik yalnızca bir dizi bir alandır. Algılanan yüzeylere hakkında almak için hangi bilgilerin, belirtin.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=9-34)]

### <a name="create-and-use-the-face-client"></a>Oluşturma ve yüz istemci kullanma

Ardından, ekleme **ana** yöntemi **Program** aşağıdaki kodla sınıfı. Bu, yüz tanıma API'si istemci ayarlama ayarlar.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=36-41)]

Ayrıca **ana** yöntemi, bir uzak ve yerel görüntüde yüz algılama için yeni oluşturulan yüz istemci kullanmak için aşağıdaki kodu ekleyin. Algılama yöntemleri sonraki tanımlanır. 

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=43-50)]

### <a name="detect-faces"></a>Yüz algılama

**Program** sınıfına aşağıdaki yöntemi ekleyin. Yüz tanıma hizmeti istemcisi, bir URL tarafından başvurulan uzak bir görüntüdeki yüzleri algılamak için kullanır. Kullandığı `faceAttributes` alan&mdash; **DetectedFace** eklenen nesneleri `faceList` (Bu durumda, geçerlilik süresi ve cinsiyet içinde) belirtilen özniteliğe sahip olacaktır.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=52-74)]

Benzer şekilde, eklemek **DetectLocalAsync** yöntemi. Yüz tanıma hizmeti istemcisi, bir dosya yolu tarafından başvurulan bir yerel görüntüdeki yüzleri algılamak için kullanır.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=76-101)]

### <a name="retrieve-and-display-face-attributes"></a>Alıp yüz öznitelikleri görüntüleyin

Ardından, tanımlama **GetFaceAttributes** yöntemi. Öznitelik ilgili bilgileri içeren bir dize döndürür.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=103-116)]

Son olarak, tanımlama **DisplayAttributes** konsol çıktısı için yüz özniteliği veri yazmak için yöntemi. Ardından, ad alanı ve sınıf kapatabilirsiniz.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=118-125)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Başarılı bir yanıt cinsiyet ve yaş her yüz için görüntüyü görüntüler. Örneğin:

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yüz tanıma API'si hizmeti, hem yerel hem de uzak görüntülerde yüzleri algılamak için kullanabileceğiniz basit bir .NET konsol uygulaması oluşturdunuz. Ardından, nasıl, yüz tanıma bilgileri kullanıcıya sezgisel bir şekilde sunabilirsiniz görmek için daha kapsamlı bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Görüntüdeki yüzleri analiz edin ve algılamak için bir WPF uygulaması oluşturma](../Tutorials/FaceAPIinCSharpTutorial.md)
