---
title: Bilgisayar görme API C# Öğreticisi | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde bilgisayar görme API'sini kullanan basit bir Windows uygulaması keşfedin. OCR gerçekleştirmek, küçük resim oluşturma ve görüntüdeki visual özellikleriyle çalışma.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/22/2017
ms.author: kefre
ms.openlocfilehash: f2aeb1734f8858ed8b80e5cdf48ef7e558703919
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352894"
---
# <a name="computer-vision-api-c35-tutorial"></a>Bilgisayar görme API C&#35; Öğreticisi

Optik karakter tanıma gerçekleştirmek, akıllı kırpılmış küçük resimler oluşturma artı algılamak, kategorilere ayırmak, etiket için bilgisayar görme API kullanır ve yüz, görüntüyü dahil olmak üzere visual özelliklerini açıklayan temel bir Windows uygulaması keşfedin. Bir resim URL'si veya yerel olarak depolanan dosya gönderebilmek örnek sağlar. WPF (Windows Presentation Foundation), .NET Framework'ün bir parçası ve görme API kullanan Windows için kendi uygulamanızı oluşturmak için bu açık kaynak örnek şablon olarak kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

#### <a name="platform-requirements"></a>Platform gereksinimleri

Aşağıdaki örnekte .NET Framework için geliştirilen kullanarak [Visual Studio 2015, Community Edition](https://www.visualstudio.com/downloads/).

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Bilgisayar görme API'sine abone olma ve aboneliği anahtarı alma 

Örnek oluşturmadan önce bilgisayarı görme API'sine Microsoft Bilişsel hizmetler (önceki adıyla proje Oxford) parçası olan abone olmalısınız. Abonelik ve anahtar yönetimi Ayrıntılar için bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide, hem birincil ve ikincil anahtar kullanılabilir. 

> [!NOTE]
> Öğretici abonelik anahtarlarında kullanmak üzere tasarlanmış **westcentralus** bölge. Bilgisayar görme ücretsiz izi kullanımda oluşturulan abonelik anahtarları **westcentralus** doğru çalışması için bölge. Abonelik anahtarlarınızı aracılığıyla Azure hesabınızı kullanarak oluşturulan varsa [ https://azure.microsoft.com/ ](https://azure.microsoft.com/), belirtmeniz gerekir **westcentralus** bölge. Dışında oluşturulan anahtarları **westcentralus** bölge çalışmaz.

#### <a name="get-the-client-library-and-example"></a>İstemci Kitaplığı ve örnek alın

Bilgisayar görme API'si istemci kitaplığı ve örnek uygulaması aracılığıyla bilgisayarınıza kopyalamak [SDK](https://www.github.com/microsoft/cognitive-vision-windows). ZIP yüklemeyin.

### <a name="Step1">1. adım: örnek yükleme</a>

GitHub masaüstünüzü WPF\VisionAPI WPF Samples.sln örnek açın.

### <a name="Step2">2. adım: örnek oluşturma</a>

* Ctrl + Shift + B tuşuna basın veya yapı Şerit menüsünde'nı tıklatın ve sonra yapı çözümü seçin.

### <a name="Step3">3. adım: örnek çalıştırma</a>

1. Yapı tamamlandıktan sonra basın **F5** veya **Başlat** örneği çalıştırmak için Şerit menüsünde.
2. Bilgisayar görme API kullanıcı arabirimi penceresi metin düzenleme kutusu "başlatmak için abonelik anahtarınızı Buraya Yapıştır" okuma bulun.
Abonelik anahtarınızı PC veya dizüstü bilgisayarınız üzerinde "Anahtarı Kaydet" düğmesini tıklatarak kalıcı hale getirmek seçebilirsiniz. Abonelik anahtarı sistemden silmek istediğinizde, PC ya da dizüstü bilgisayara kaldırmak için "anahtar Sil"'ı tıklatın.

    ![Görme abonelik anahtarı](../Images/Vision_UI_Subscription.PNG)

3. "Senaryo seçin" altında altı senaryolardan biri, kullanmak için tıklatın, ardından ekrandaki yönergeleri izleyin. Microsoft, karşıya yükleme ve bunları bilgisayar görme API ve ilgili hizmetler geliştirmek için kullanabilir görüntüleri alır. Bir görüntü göndererek, izlediğinizden onaylamanız bizim [kullanım kuralları Geliştirici](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).

    ![Görüntü arabirimi Çözümle](../Images/Analyze_Image_Example.PNG)

4. Bu örnek uygulama ile kullanılacak örnek görüntülerin vardır. Yüz API Windows Github deposuna üzerinde bu görüntüleri bulabilirsiniz [veri klasörü](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Lütfen bu görüntüleri kullanımını sözleşmesi altında lisanslanır Not [lisans görüntü](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md).

### <a name="Review">Gözden geçirin ve öğrenin</a>

Çalışan bir uygulama sahip olduğunuza göre bize Bu örnek uygulama Bilişsel hizmetler teknolojisi ile nasıl tümleşik çalıştığını gözden geçirin. Bu, bu uygulamanın üzerine yapı devam ya da Microsoft bilgisayar görme API kullanarak kendi uygulamanızı geliştirin daha kolay hale getirir.

Bu örnek uygulama bilgisayar görme API'si istemci kitaplığı, ince C# istemci için sarmalayıcı Microsoft bilgisayar görme API kullanır. Yukarıda açıklandığı gibi örnek uygulama yapılandırıldığında, istemci kitaplığı NuGet paket aldı. Başlıklı klasöründeki istemci kitaplığı kaynak kodu gözden geçirebilirsiniz "**istemci Kitaplığı**" altında **görme**, **Windows**, **istemci Kitaplığı**, hangi önkoşulları yukarıdaki indirilen dosya deposu bir parçasıdır.

Çözüm Gezgini'nde istemci kitaplığı kodu kullanmayı da bulabilirsiniz: altında **VisionAPI WPF_Samples**, genişletin **AnalyzePage.xaml** bulmak için **AnalyzePage.xaml.cs**, görüntünün görüntü analiz uç göndermek için kullanılan. Çift tıklayın. bunları şekilde xaml.cs dosyalarını açma Visual Studio'da yeni Windows.

Görme istemci kitaplığı bizim örnek uygulamasında nasıl kullanılan gözden geçirme, iki kod parçacıkları bakalım **AnalyzePage.xaml.cs**. Dosya "Anahtar örnek kodu BAŞLATIR burada" ve "Anahtar örnek kodu sona ERER burada" aşağıda parçacıkları çoğaltılamaz kod bulmanıza yardımcı olmak üzere gösteren kod açıklamaları içerir.

Giriş olarak bir resim URL'si veya ikili resim verileri (biçiminde bir sekizli akış) ile çalışabilmek için Çözümle uç noktadır. İlk olarak kullanarak bir bulma görme istemci kitaplığı kullanmanıza olanak sağlayan yönergesi.

```
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE STARTS HERE
                // Use the following namespace for VisionServiceClient 
                // ---------------------------------------------------------------------- 
                using Microsoft.ProjectOxford.Vision; 
                using Microsoft.ProjectOxford.Vision.Contract; 
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE ENDS HERE 
                // ----------------------------------------------------------------------

```
**UploadAndAnalyzeImage(...)**  Bu kod parçacığını istemci kitaplığı abonelik anahtarınızı ve bilgisayar görme API hizmeti Çözümle uç noktası için yerel olarak saklanan bir görüntü göndermek için nasıl kullanılacağını gösterir.

```
    private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------  
        //
        // Create Project Oxford Computer Vision API Service client
        //
        VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features
            //
            Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
         VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
            AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
            return analysisResult;
        }
    
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
        }
```
**AnalyzeUrl(...)**  Bu kod parçacığını istemci kitaplığı abonelik anahtarınızı ve bilgisayar görme API hizmeti Çözümle uç noktası için bir fotoğraf URL'si göndermek için nasıl kullanılacağını gösterir.

```
    private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------
    
        //
        // Create Project Oxford Computer Vision API Service client
        //
     VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        //
        // Analyze the url for all visual features
        //
        Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
        VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
        AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
     return analysisResult;
    }
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
```
**Diğer sayfalar ve uç noktaları** bilgisayar görme API hizmeti tarafından sunulan diğer uç noktaları ile etkileşim kurmak nasıl bir örnek sayfalarında bakarak görülebilir; örneği için OCR endpoint OCRPage.xaml.cs içinde yer alan kodun bir parçası olarak gösterilir 

### <a name="Related">İlgili Konular</a>
 * [Yüz API'si ile çalışmaya başlama](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 


