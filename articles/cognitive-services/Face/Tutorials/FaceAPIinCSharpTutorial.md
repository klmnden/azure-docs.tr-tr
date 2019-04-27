---
title: "Öğretici: Algılama ve .NET SDK'sı ile bir resimdeki yüz veri görüntüleme"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, algılamak ve bir görüntüdeki yüzleri çerçeve için yüz tanıma API'si kullanan bir Windows uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: pafarley
ms.openlocfilehash: 6a60afc45894518f92115976876ddd50efa1e410
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815369"
---
# <a name="tutorial-create-a-wpf-app-to-display-face-data-in-an-image"></a>Öğretici: Bir resimdeki yüz verileri görüntülemek için bir WPF uygulaması oluşturma

Bu öğreticide, Azure yüz tanıma API'si, .NET İstemci SDK'sı ile bir resimdeki yüz algılama ve ardından bu verileri kullanıcı Arabiriminde sunmak için nasıl kullanılacağını öğreneceksiniz. Durum çubuğunda yüzü açıklamasını görüntüler yüzleri algılar ve her yüz etrafında bir çerçeve çizen basit bir Windows Presentation Framework (WPF) uygulaması oluşturacaksınız. 

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - WPF uygulaması oluşturma
> - Yüz tanıma API'si istemci Kitaplığı'nı yükleyin
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme
> - Durum çubuğunda vurgulanan yüz açıklamasını görüntüle

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)

Tam örnek kodu [Bilişsel yüz CSharp örneğinde](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) github deposu.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 


## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Yeni bir WPF uygulaması projesi oluşturmak için aşağıdaki adımları izleyin.

1. Visual Studio'da yeni proje iletişim kutusunu açın. Genişletin **yüklü**, ardından **Visual C#** , ardından **WPF uygulaması (.NET Framework)**.
1. Uygulamaya **FaceTutorial** adını verin ve **Tamam**'a tıklayın.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**; ardından, bulun ve aşağıdaki paketi yükleyin:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

Bu bölümde, yüz tanıma özgü özelliklerini gerekmeden uygulamayı temel çerçevesini ekleyeceksiniz.

### <a name="create-the-ui"></a>Kullanıcı Arabirimi oluşturma

Açık *MainWindow.xaml* ve içeriğini aşağıdaki kodla değiştirin&mdash;bu UI pencere oluşturur. Unutmayın `FacePhoto_MouseMove` ve `BrowseButton_Click` olay işleyicileri, daha sonra tanımlayacaksınız.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?range=1-18)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

Açık *MainWindow.xaml.cs* ve diğer gerekli ad alanları ile birlikte istemci kitaplığı ad ekleyin. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=1-12)]

Ardından, aşağıdaki kodda Ekle **MainWindow** sınıfı. Bu, oluşturur bir **FaceClient** kendiniz girin abonelik anahtarını kullanarak örneği. Bölge dizesi de ayarlamanız gerekir `faceEndpoint` aboneliğiniz için doğru bir bölgeye (bkz [yüz tanıma API'si belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) tüm bölge uç noktalar listesi).

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=18-46)]

Ardından aşağıdaki kodu yapıştırın **MainWindow** yöntemi.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=50-61)]

Son olarak, ekleme **BrowseButton_Click** ve **FacePhoto_MouseMove** sınıfı yöntemleri. Bu olay işleyicileri içinde bildirilen karşılık *MainWindow.xaml*. **BrowseButton_Click** yöntemi oluşturur bir **OpenFileDialog**, kullanıcının bir .jpg görüntüsü seçmesine olanak sağlar. Daha sonra görüntüyü ana penceresinde görüntüler. Kalan kodunu ekleyecek **BrowseButton_Click** ve **FacePhoto_MouseMove** sonraki adımlarda. Ayrıca unutmayın `faceList` başvuru&mdash;listesini **DetectedFace** nesneleri. Uygulamanızı nerede ve depolamak çağrı gerçek yüz verileri budur.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=64-90,146)]

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=148-150,187)]

### <a name="try-the-app"></a>Uygulamayı deneyin

Uygulamanızı test etmek için menüde **Başlat**'a basın. Uygulama penceresi açıldığında, tıklayın **Gözat** sol alt köşedeki. A **Dosya Aç** iletişim kutusu görüntülenmelidir. Dosya sisteminden bir görüntü seçin ve pencerenin görüntülendiğini doğrulayın. Ardından, sonraki adıma ilerleyin ve uygulamayı kapatın.

![Yüzlerin değiştirilmemiş resmini gösteren ekran görüntüsü](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Görüntü karşıya yükleme ve yüz algılama

Uygulamanızı çağırarak yüzleri algılar **FaceClient.Face.DetectWithStreamAsync** sarmalar yöntemi [Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) yerel görüntü yüklemek için REST API.

Aşağıdaki yöntemi ekleyin **MainWindow** sınıfı, aşağıda **FacePhoto_MouseMove** yöntemi. Bu bir listesini almak için yüz öznitelikleri tanımlar ve gönderilen resim dosyasına okuyan bir **Stream**. Hem de bu nesnelere geçtikten sonra **DetectWithStreamAsync** yöntem çağrısı.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=189-226)]

## <a name="draw-rectangles-around-faces"></a>Yüzleri dikdörtgenler çizme

Sonra görüntüde algılanan her yüz çevresinde bir dikdörtgen çizmek için kod ekleyeceksiniz. İçinde **MainWindow** sınıfı, sonunda aşağıdaki kodu ekleyin **BrowseButton_Click** yöntemi, sonra `FacePhoto.Source = bitmapSource` satır. Bu, algılanan yüzeylere çağrısından listesini doldurur **UploadAndDetectFaces**. Ardından her yüz çevresinde bir dikdörtgen çizer ve değiştirilen resmi ana penceresinde görüntüler.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=92-145)]

## <a name="describe-the-faces"></a>Yüzleri açıklayın

Aşağıdaki yöntemi ekleyin **MainWindow** sınıfı, aşağıda **UploadAndDetectFaces** yöntemi. Bu, alınan yüz öznitelikleri yüzü açıklayan bir dizeye dönüştürür.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=228-286)]

## <a name="display-the-face-description"></a>Yüz açıklamasını görüntüleme

Aşağıdaki kodu ekleyin **FacePhoto_MouseMove** yöntemi. Bu olay işleyicisi yüz tanıma açıklama dizesinde görüntüler `faceDescriptionStatusBar` İmleç bir algılanan yüz dikdörtgeni geldiğinde.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=151-186)]


## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüz içeren bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Her bir görüntüdeki yüzleri kırmızı bir dikdörtgen görmeniz gerekir. Bir yüz dikdörtgeni üzerinde fareyi hareket ettirin, açıklaması, yüz tanıma, durum çubuğunda görüntülenmesi gerekir.

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, temel işlemi, yüz tanıma hizmeti .NET SDK'sını kullanma hakkında bilgi edindiniz ve algılamak ve bir görüntüdeki yüzleri çerçeve için bir uygulama oluşturdunuz. Ardından, yüz algılama ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
