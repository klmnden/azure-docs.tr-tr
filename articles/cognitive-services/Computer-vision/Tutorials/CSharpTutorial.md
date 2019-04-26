---
title: 'Örnek: Bir görüntü işleme uygulamada keşfedinC#'
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel hizmetler görüntü işleme API'sini kullanan basit bir Windows uygulaması keşfedin. OCR gerçekleştirin, küçük resimler oluşturun ve bir görüntüdeki görsel özelliklerle çalışın.
services: cognitive-services
author: PatrickFarley
manager: nolachar
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 63b5130e3cade54a2fbc432b2391ad3ee1ea8a1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60408198"
---
# <a name="sample-explore-an-image-processing-app-with-c"></a>Örnek: Bir görüntü işleme uygulaması keşfedinC#

Optik karakter tanıma (OCR) gerçekleştirin, akıllı kırpılmış küçük resimler oluşturma artı algılamak, kategorilere ayırma, etiket için görüntü işleme kullanır ve bir resimdeki yüz, dahil olmak üzere, görsel özellikleri açıklayan temel bir Windows uygulaması keşfedin. Aşağıdaki örnek bir görüntü URL'si veya yerel ortamda depolanan dosya göndermenizi sağlar. Bu açık kaynak örneği ve görüntü işleme API'si, .NET Framework'ün bir parçası olan Windows Presentation Foundation (WPF) kullanarak Windows için kendi uygulamanızı oluşturmak için şablon olarak kullanabilirsiniz.

> [!div class="checklist"]
> * Örnek uygulamasını Github'dan alma
> * Açın ve örnek uygulamayı Visual Studio'da derleme
> * Örnek uygulamayı çalıştırın ve çeşitli senaryoları gerçekleştirmek için etkileşim
> * Örnek uygulamayı dahil çeşitli senaryoları keşfedin

## <a name="prerequisites"></a>Önkoşullar

Örnek uygulamayı keşfetmeyi önce aşağıdaki önkoşulların karşılamanızın emin olun:

* [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) veya üzerine sahip olmanız gerekir.
* Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

## <a name="get-the-sample-app"></a>Örnek uygulamayı alma

Görüntü işleme örnek uygulamasını Github'dan üzerinde kullanılabilir `Microsoft/Cognitive-Vision-Windows` depo. Bu depo da içeren `Microsoft/Cognitive-Common-Windows` deposu olarak bir Git alt modülüdür. Kullanarak ya da alt modülü de dahil olmak üzere bu depo, özyinelemeli olarak Kopyala için `git clone --recurse-submodules` komut satırından veya GitHub Masaüstü kullanarak komutu.

Örneğin, yinelemeli olarak kopyalama için depo görüntü işleme örnek uygulama için bir komut isteminden aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Vision-Windows.git
```

> [!IMPORTANT]
> Bu depo, bir ZIP olarak indir değil. Git alt modülleri bir deposu olarak bir ZIP indirirken içermez.

### <a name="get-optional-sample-images"></a>İsteğe bağlı bir örnek görüntü alma

Bulunan örnek görüntüleri isteğe bağlı olarak kullanabileceğiniz [yüz](../../Face/Overview.md) örnek uygulaması, GitHub üzerinde kullanılabilir `Microsoft/Cognitive-Face-Windows` depo. Bu örnek uygulama, bir klasör içerir `/Data`, birden çok kişi görüntülerini içerir. Bu depo, de, özyinelemeli olarak Kopyala için görüntü işleme örnek uygulama açıklanan yöntemlerle yapabilirsiniz.

Örneğin, yinelemeli olarak kopyalama için depo yüz örnek uygulama için bir komut isteminden aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Face-Windows.git
```

## <a name="open-and-build-the-sample-app-in-visual-studio"></a>Açın ve örnek uygulamayı Visual Studio'da derleme

Böylece örnek uygulamayı inceleme veya çalıştırmadan önce Visual Studio bağımlılıkları çözümlemek örnek uygulamayı ilk olarak oluşturmanız gerekir. Açın ve örnek uygulamayı oluşturmak için aşağıdaki adımları uygulayın:

1. Visual Studio çözüm dosyasını açın `/Sample-WPF/VisionAPI-WPF-Samples.sln`, Visual Studio'da.
1. Visual Studio çözümünü iki proje içerdiğinden emin olun:  

   * SampleUserControlLibrary
   * VisionAPI WPF Örnekleri  

   Kopyalanan yinelemeli olarak seçtiğiniz SampleUserControlLibrary proje kullanılamıyorsa, onaylayın `Microsoft/Cognitive-Vision-Windows` depo.
1. Visual Studio'da ya da Ctrl + Shift + B tuşlarına basın veya seçin **derleme** Şerit menüsünde seçip **Çözümü Derle** çözümü derlemek için.

## <a name="run-and-interact-with-the-sample-app"></a>Çalıştırın ve örnek uygulaması ile etkileşim kurma

Nasıl, siz ve görüntü işleme istemci kitaplığı ile oluşturma küçük resimleri veya etiketleme görüntüleri gibi çeşitli görevleri gerçekleştirirken etkileşim görmek için örnek uygulamayı çalıştırabilirsiniz. Çalıştırın ve örnek uygulaması ile etkileşim kurmak için aşağıdaki adımları uygulayın:

1. Derleme tamamlandıktan sonra tuşlarına basın **F5** veya **hata ayıklama** Şerit menüsünde seçip **hata ayıklamayı Başlat** örnek uygulamayı çalıştırmak için.
1. Örnek uygulama görüntülendiğinde seçin **abonelik anahtarı Yönetimi** abonelik anahtarı Yönetimi sayfasını görüntülemek için Gezinti bölmesinden.
   ![Abonelik anahtarı Yönetimi sayfası](../Images/Vision_UI_Subscription.PNG)  
1. Abonelik anahtarınızı girin **abonelik anahtarı**.
1. Uç nokta URL'sini atlama `/vision/v1.0`, görüntü işleme kaynak için abonelik anahtarınızı **uç nokta**.  
   Örneğin, görüntü işleme ücretsiz deneme aboneliği anahtarı kullanıyorsanız, Batı Orta ABD Azure bölgesi için şu uç nokta URL'sini girin: `https://westcentralus.api.cognitive.microsoft.com`
1. Abonelik anahtarını ve uç nokta girmek istemiyorsanız URL gelecek sefer örnek uygulamayı çalıştırdığınızda seçin **Kaydet ayarı** abonelik anahtarını ve uç nokta URL'si bilgisayarınıza kaydedin. Daha önce kaydettiğiniz abonelik anahtarını ve uç nokta URL'nizi silmek istiyorsanız, seçin **ayarı silmek**.

   > [!NOTE]
   > Yalıtılmış Depolama, örnek uygulamayı kullanır ve `System.IO.IsolatedStorage`, abonelik anahtarını ve uç nokta URL'nizi depolamak için.

1. Altında **bir senaryo seçmek** Gezinti bölmesinde, örnek uygulamayı şu anda dahil senaryolardan birini seçin:  

   | Senaryo | Açıklama |
   |----------|-------------|
   |Resmi çözümleme | Kullanan [analiz görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yerel veya uzak bir resmi çözümleme işlemi. Görsel özellikler ve analiz için dil seçin ve hem yansıma hem de sonuçları bakın.  |
   |Etki alanı modeli ile resmi çözümleme | Kullanır [listesi alana özgü modellere](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) içinden seçebilirsiniz, etki alanı modellerini listelemek için işlemi ve [tanımak etki alanı belirli içerik](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) işlemi, yerel veya uzak bir görüntü kullanarak bir analiz etmek için Seçilen etki alanı modeli. Dil çözümleme için de seçebilirsiniz. |
   |Görüntü açıklayın | Kullanan [açıklayan görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fe) okunabilir açıklamasını yerel veya uzak bir görüntü oluşturma işlemi. Ayrıca, açıklama dilini seçebilirsiniz. |
   |Etiket Oluştur | Kullanan [etiket görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1ff) yerel veya uzak bir görüntünün görsel özellikleri etiketlemek için işlemi. Ayrıca, etiketleri için kullanılan dili seçebilirsiniz. |
   |Metin (OCR) tanıması | Kullanan [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) görüntüden tanıması ve ayıklamak için işlem yazdırılan metin. Kullanılacak dili seçin veya görüntü işleme otomatik dil algılama sağlar. |
   |Metin V2 tanımak (İngilizce) | Kullanan [metni tanı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) ve [tanımak metin işleminin sonucunu Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) işlemlerini zaman uyumsuz olarak tanımak ve bir görüntüden yazdırılan veya el yazısı metinleri ayıklamak için. |
   |Küçük resmini Al | Kullanan [küçük resim alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) için yerel veya uzak bir görüntüyü küçük resim oluşturma işlemi. |

   Aşağıdaki ekran görüntüsünde bir örnek görüntü çözümledikten sonra analiz görüntü senaryosu için sağlanan sayfası gösterilmektedir.
   ![Analiz görüntü sayfasının ekran görüntüsü](../Images/Analyze_Image_Example.PNG)

## <a name="explore-the-sample-app"></a>Örnek uygulamayı İnceleme

Visual Studio çözümünü görüntü işleme örnek uygulama için iki proje içerir:

* SampleUserControlLibrary  
  SampleUserControlLibrary projesi, birden çok Bilişsel hizmetler örnekleri tarafından paylaşılan işlevselliği sağlar. Proje aşağıdakileri içerir:
  * SampleScenarios  
    Bir UserControl örnekleri için başlık çubuğu, Gezinti bölmesinde ve içerik bölmesi gibi standart bir sunu sağlar. Görüntü işleme örnek uygulaması senaryosu sayfaları, görüntüler ve abonelik anahtarını ve uç nokta URL'si gibi senaryoları arasında paylaşılan bilgiler erişmek için MainWindow.xaml penceresinde bu denetimi kullanır.
  * SubscriptionKeyPage  
    Örnek uygulama için bir abonelik anahtarı ve uç nokta URL'si girmek için standartlaştırılmış bir düzen sağlayan bir sayfa. Görüntü işleme örnek uygulama, abonelik anahtarını yönetmek için bu sayfayı kullanır ve uç nokta URL'si senaryo sayfaları tarafından kullanılır.
  * VideoResultControl  
    Video ile ilgili bilgileri için standartlaştırılmış bir sunu sağlar bir UserControl. Görüntü işleme örnek uygulaması, bu denetim kullanmaz.
* VisionAPI WPF Örnekleri  
  Görüntü işleme örnek uygulaması için ana proje, bu proje için görüntü işleme tüm ilginç işlevselliğini içerir. Proje aşağıdakileri içerir:
  * AnalyzeInDomainPage.xaml  
    Etki alanı modeli senaryo ile analiz görüntü için senaryo sayfası.
  * AnalyzeImage.xaml  
    Analiz görüntü Senaryo için senaryo sayfası.
  * DescribePage.xaml  
    Görüntü açıklayan senaryo için senaryo sayfası.
  * ImageScenarioPage.cs  
    Tüm örnek uygulamada senaryo sayfaları türetildiği ImageScenarioPage sınıfı. Bu sınıf, kimlik bilgileri sağlama ve tüm senaryo sayfaları tarafından paylaşılan çıktı biçimlendirme gibi işlevleri yönetir.
  * MainWindow.xaml  
    Örnek uygulama ana penceresi, SampleScenarios denetim SubscriptionKeyPage ve senaryo sayfaları sunmak için kullanır.
  * OCRPage.xaml  
    Metin tanıma (OCR) senaryosu için senaryo sayfası.
  * RecognizeLanguage.cs  
    RecognizeLanguage sınıfı örnek uygulamada çeşitli yöntemler tarafından desteklenen diller hakkında bilgi sağlar.
  * TagsPage.xaml  
    Etiketler oluşturma senaryosu için senaryo sayfası.
  * TextRecognitionPage.xaml  
    Tanı metin V2 (İngilizce) senaryosu için senaryo sayfası.
  * ThumbnailPage.xaml  
    Küçük resim alma senaryosu için senaryo sayfası.

### <a name="explore-the-sample-code"></a>Örnek kodu inceleyin

Anahtar bölümlerini örnek kod ile başlayan açıklama blokları Çerçeveli `KEY SAMPLE CODE STARTS HERE` ve ile sona erdi `KEY SAMPLE CODE ENDS HERE`, örnek uygulamayı İnceleme kolaylaştırır. Örnek kod bu anahtar bölümlerinin görüntü işleme API'si istemci kitaplığı çeşitli görevleri gerçekleştirmek için nasıl kullanılacağını öğrenmek için en uygun kodu içerir. Arayabilirsiniz `KEY SAMPLE CODE STARTS HERE` görüntü işleme örnek uygulamada kod en uygun bölümleri arasında hareket etmek için Visual Studio'da. 

Örneğin, `UploadAndAnalyzeImageAsync` izleyerek ve AnalyzePage.xaml içinde bulunan yöntemi, harekete geçirerek yerel görüntüyü çözümlemek için istemci kitaplığını kullanma işlemini gösterir `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemi.

```csharp
private async Task<ImageAnalysis> UploadAndAnalyzeImageAsync(string imageFilePath)
{
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE STARTS HERE
    // -----------------------------------------------------------------------

    //
    // Create Cognitive Services Vision API Service client.
    //
    using (var client = new ComputerVisionClient(Credentials) { Endpoint = Endpoint })
    {
        Log("ComputerVisionClient is created");

        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features.
            //
            Log("Calling ComputerVisionClient.AnalyzeImageInStreamAsync()...");
            VisualFeatureTypes[] visualFeatures = GetSelectedVisualFeatures();
            string language = (_language.SelectedItem as RecognizeLanguage).ShortCode;
            ImageAnalysis analysisResult = await client.AnalyzeImageInStreamAsync(imageFileStream, visualFeatures, null, language);
            return analysisResult;
        }
    }

    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE ENDS HERE
    // -----------------------------------------------------------------------
}
```

### <a name="explore-the-client-library"></a>İstemci kitaplığını keşfedin

Bu örnek uygulama, ölçülü kaynak kullanan C# istemci için bir sarmalayıcı görüntü işleme API'si, Azure Bilişsel hizmetler görüntü işleme API'si istemci kitaplığını kullanır. İstemci Kitaplığı nuget içinde kullanılabilir [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) paket. Visual Studio uygulamasını yapılandırıldığında, istemci kitaplığının karşılık gelen NuGet paketinden alınır. İstemci Kitaplığı'ndaki kaynak kodunu da görüntüleyebilirsiniz `/ClientLibrary` klasörü `Microsoft/Cognitive-Vision-Windows` depo.

İstemci kitaplığının işlevselliği etrafında ortalar `ComputerVisionClient` içinde sınıf `Microsoft.Azure.CognitiveServices.Vision.ComputerVision` ad alanı tarafından kullanılan modelleri çalışırken `ComputerVisionClient` görüntü işleme ile etkileşim kurulurken sınıfı bulunur `Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models` ad alanı. Aşağıdaki örnek uygulaması ile dahil çeşitli XAML senaryo sayfaları'nda bulabilirsiniz `using` yönergesi bu ad alanları için:

```csharp
// -----------------------------------------------------------------------
// KEY SAMPLE CODE STARTS HERE
// Use the following namespace for ComputerVisionClient.
// -----------------------------------------------------------------------
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
// -----------------------------------------------------------------------
// KEY SAMPLE CODE ENDS HERE
// -----------------------------------------------------------------------
```

İle sunulan çeşitli yöntemler hakkında daha fazla bilgi edineceksiniz `ComputerVisionClient` ile görüntü işleme örnek uygulamasını dahil senaryoları keşfederken sınıfı.

## <a name="explore-the-analyze-image-scenario"></a>Analiz görüntü Senaryo keşfedin

Bu senaryo AnalyzePage.xaml sayfası tarafından yönetilir. Görsel özellikler ve analiz için dil seçin ve hem yansıma hem de sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak, aşağıdaki yöntemlerden birini kullanarak yapar:

* UploadAndAnalyzeImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemi.
* AnalyzeUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.AnalyzeImageAsync` yöntemi.

`UploadAndAnalyzeImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, ardından senaryosu sayfasında seçili dil ve görsel özellikleri alır. Çağrı `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemini `Stream` dosya, görsel özellikler ve dil, ardından sonuç olarak döndüren bir `ImageAnalysis` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`AnalyzeUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Bu senaryo sayfasında seçili dil ve görsel özellikleri alır. Çağrı `ComputerVisionClient.AnalyzeImageInStreamAsync` resim URL'si, görsel özellikler ve dil geçirme yöntemi, ardından döndürür sonucu olarak bir `ImageAnalysis` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="explore-the-analyze-image-with-domain-model-scenario"></a>Etki alanı Model senaryosuna analiz görüntüsüyle keşfedin

Bu senaryo AnalyzeInDomainPage.xaml sayfası tarafından yönetilir. Gibi bir etki alanı modeli seçin `celebrities` veya `landmarks`ve görüntünün bir etki alanına özgü analizi gerçekleştirmek ve hem yansıma hem de sonuçları görmek için dili. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* GetAvailableDomainModelsAsync  
  Bu yöntem, görüntü işleme kullanılabilir bir etki alanı modelleri listesini alır ve dolduran `_domainModelComboBox` sayfasında ComboBox denetimi kullanarak `ComputerVisionClient.ListModelsAsync` yöntemi.
* UploadAndAnalyzeInDomainImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` yöntemi.
* AnalyzeInDomainUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.AnalyzeImageByDomainAsync` yöntemi.

`UploadAndAnalyzeInDomainImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, ardından senaryosu sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` yöntemini `Stream` dosya için etki alanı modeli ve dil adını ardından sonuç olarak döndüren bir `DomainModelResults` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`AnalyzeInDomainUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Bu senaryo sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.AnalyzeImageByDomainAsync` resim URL'si, görsel özellikler ve dil geçirme yöntemi, ardından döndürür sonucu olarak bir `DomainModelResults` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="explore-the-describe-image-scenario"></a>Görüntü açıklayan senaryo keşfedin

Bu senaryo DescribePage.xaml sayfası tarafından yönetilir. Görüntüsünün insanlar tarafından okunabilen bir açıklamasını oluşturmak için bir dil seçin ve hem yansıma hem de sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndDescribeImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.DescribeImageInStreamAsync` yöntemi.
* DescribeUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.DescribeImageAsync` yöntemi.

`UploadAndDescribeImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, ardından senaryosu sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.DescribeImageInStreamAsync` yöntemini `Stream` dosya için aday (Bu durumda, 3) ve dil sayısı ardından sonuç olarak döndüren bir `ImageDescription` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`DescribeUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Bu senaryo sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.DescribeImageAsync` geçirme resim URL'si, aday (Bu durumda, 3) ve dil sayısı, ardından döner sonucu olarak bir `ImageDescription` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="explore-the-generate-tags-scenario"></a>Etiketler oluşturma senaryosu keşfedin

Bu senaryo TagsPage.xaml sayfası tarafından yönetilir. Görüntünün görsel özellikleri etiketlemek için bir dil seçin ve görüntü hem sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndGetTagsForImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.TagImageInStreamAsync` yöntemi.
* GenerateTagsForUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.TagImageAsync` yöntemi.

`UploadAndGetTagsForImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, ardından senaryosu sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.TagImageInStreamAsync` yöntemini `Stream` dosya ve dil için ardından sonuç olarak döndüren bir `TagResult` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`GenerateTagsForUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Bu senaryo sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.TagImageAsync` resim URL'si hem de dil, ardından döner sonucu olarak bir `TagResult` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="explore-the-recognize-text-ocr-scenario"></a>Metin tanıma (OCR) senaryo keşfedin

Bu senaryo OCRPage.xaml sayfası tarafından yönetilir. Tanıması ve bir görüntüden yazdırılan metin ayıklamak için bir dil seçin ve hem yansıma hem de sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndRecognizeImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.RecognizePrintedTextInStreamAsync` yöntemi.
* RecognizeUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.RecognizePrintedTextAsync` yöntemi.

`UploadAndRecognizeImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, ardından senaryosu sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.RecognizePrintedTextInStreamAsync` yönlendirme algılanmadı belirten ve geçirme yöntemi `Stream` dosya ve dil için ardından sonuç olarak döndüren bir `OcrResult` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`RecognizeUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Bu senaryo sayfasında seçtiğiniz dile alır. Çağrı `ComputerVisionClient.RecognizePrintedTextAsync` yönlendirme algılanmadı belirten ve resim URL'si hem de dil yöntemi ardından döndürür sonucu olarak bir `OcrResult` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="explore-the-recognize-text-v2-english-scenario"></a>Tanı metin V2 (İngilizce) senaryosu keşfedin

Bu senaryo TextRecognitionPage.xaml sayfası tarafından yönetilir. Tanıma modu ve zaman uyumsuz olarak tanınması ve bir görüntüden yazdırılan veya el yazısı metinleri ayıklamak için bir dil seçin ve görüntü hem sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndRecognizeImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `RecognizeAsync` yöntemi ve parametreli bir temsilci geçirmek için `ComputerVisionClient.RecognizeTextInStreamAsync` yöntemi.
* RecognizeUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `RecognizeAsync` yöntemi ve parametreli bir temsilci geçirmek için `ComputerVisionClient.RecognizeTextAsync` yöntemi.
* Bu yöntem RecognizeAsync işleme hem de zaman uyumsuz çağırma `UploadAndRecognizeImageAsync` ve `RecognizeUrlAsync` arayarak sonuçlar için yoklama yanı sıra yöntemleri `ComputerVisionClient.GetTextOperationResultAsync` yöntemi.

Görüntü işleme örnek uygulamasındaki diğer senaryolara, bu işlemi başlatmak için bir yöntem çağrılır ancak farklı durumunu denetleyin ve bu işlemin sonuçlarını döndürmek için çağrılan zaman uyumsuz senaryodur. Bu senaryoda mantıksal akışı diğer senaryolarda, biraz farklıdır.

`UploadAndRecognizeImageAsync` Yöntemi içinde belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`, sonra çağıran `RecognizeAsync` geçirme yöntemi:

* Parametreli bir zaman uyumsuz metot için bir lambda ifadesi `ComputerVisionClient.RecognizeTextInStreamAsync` yöntemi ile `Stream` dosya ve parametre olarak tanıma modu için de `GetHeadersAsyncFunc`.
* Bir lambda ifadesi almak bir temsilci için `Operation-Location` yanıt üst bilgisi değeri `GetOperationUrlFunc`.

`RecognizeUrlAsync` Yöntem çağrılarını `RecognizeAsync` geçirme yöntemi:

* Parametreli bir zaman uyumsuz metot için bir lambda ifadesi `ComputerVisionClient.RecognizeTextAsync` uzak görüntü ve parametre olarak tanıma modu URL ile bir yöntem içinde `GetHeadersAsyncFunc`.
* Bir lambda ifadesi almak bir temsilci için `Operation-Location` yanıt üst bilgisi değeri `GetOperationUrlFunc`.

Zaman `RecognizeAsync` yöntemi tamamlandığında, her ikisi de `UploadAndRecognizeImageAsync` ve `RecognizeUrlAsync` sonucu olarak yöntemleri döndürür bir `TextOperationResult` örneği. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`RecognizeAsync` Yöntemini çağırır parametreli temsilci için `ComputerVisionClient.RecognizeTextInStreamAsync` veya `ComputerVisionClient.RecognizeTextAsync` yöntemi geçirilen `GetHeadersAsyncFunc` ve yanıt bekler. Yöntemi ardından geçirilen temsilci çağıran `GetOperationUrlFunc` almak için `Operation-Location` yanıt yanıt üstbilgi değeri. Bu değer, geçirilen yöntemi sonuçlarını almak için kullanılan URL'dir `GetHeadersAsyncFunc` gelen görüntü işleme.

`RecognizeAsync` Sonra yöntemi çağırır `ComputerVisionClient.GetTextOperationResultAsync` yöntemini hizmetinden alınan URL `Operation-Location` yanıt üst bilgisi, durum ve geçirilen yöntemin sonucunu almak için `GetHeadersAsyncFunc`. Yöntemi, başarıyla veya başarısız, tamamlandığını durumu içermiyorsa `RecognizeAsync` yöntem çağrılarını `ComputerVisionClient.GetTextOperationResultAsync` 3 daha fazla kez, çağrıları arasında 3 saniye bekleniyor. `RecognizeAsync` Yöntemi çağıran yönteme sonuçlarını döndürür.

## <a name="explore-the-get-thumbnail-scenario"></a>Küçük resim alma senaryosu keşfedin

Bu senaryo ThumbnailPage.xaml sayfası tarafından yönetilir. Akıllı kırpma kullanılıp kullanılmayacağını belirtmek ve istenen yüksekliğini ve genişliğini bir görüntüden bir küçük resim oluşturmaya belirtin ve hem yansıma hem de sonuçları bakın. Senaryo sayfası resminin kaynağını bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndThumbnailImageAsync  
  Bu yöntem, görüntü gerekir kodlanmış olarak yerel görüntüler için kullanılan bir `Stream` ve görüntü işleme çağırarak gönderilen `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntemi.
* ThumbnailUrlAsync  
  Bu yöntem, görüntü URL'sini gönderilen görüntü işleme çağırarak uzak görüntüleri için kullanılan `ComputerVisionClient.GenerateThumbnailAsync` yöntemi.

`UploadAndThumbnailImageAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Örnek uygulamayı yerel bir görüntü çözümlüyor olduğundan, görüntü işleme, görüntü içeriğini göndermek vardır. Belirtilen yerel dosyayı açar `imageFilePath` olarak okumak için bir `Stream`. Çağrı `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntemi, genişlik, yükseklik geçirme `Stream` dosya ve akıllı kırpma kullanıp kullanmayacağınızı sonra sonuç olarak döndüren bir `Stream`. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

`RecognizeUrlAsync` Yöntemi yeni bir oluşturur `ComputerVisionClient` örneğini, belirtilen abonelik anahtarını ve uç nokta URL'sini kullanarak. Çağrı `ComputerVisionClient.GenerateThumbnailAsync` yöntemini, genişlik, yükseklik, görüntünün URL'sini ve akıllı kırpma kullanıp kullanmayacağınızı ardından döndürür sonucu olarak bir `Stream`. Devralınan yöntemleri `ImageScenarioPage` sınıfı sunmak ve döndürülen sonuçlarda senaryosu sayfasında.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, kopyaladığınız klasörü silin `Microsoft/Cognitive-Vision-Windows` depo. Örnek görüntüleri kullanmayı seçtiyseniz, ayrıca hangi kopyaladığınız klasörü silin `Microsoft/Cognitive-Face-Windows` depo.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yüz tanıma API'si ile çalışmaya başlama](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
