---
title: Duygu tanıma API'si C# Öğreticisi | Microsoft Docs
description: Görüntüdeki yazıtipleri ile ifade edilen duygular tanımak için Bilişsel hizmetler duygu tanıma API'si kullanan temel bir Windows uygulaması keşfedin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/23/2017
ms.author: anroth
ms.openlocfilehash: f015e5720f65d0951a77de76ce8882a6dcdc1c3b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352462"
---
# <a name="emotion-api-in-c35-tutorial"></a>Duygu tanıma API'si c&#35; Öğreticisi

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Görüntüdeki yazıtipleri ile ifade edilen duygular tanımak için duygu tanıma API'si kullanan temel bir Windows uygulaması keşfedin. Bir resim URL'si veya yerel olarak depolanan dosya gönderebilmek örnek sağlar. WPF (Windows Presentation Foundation), .NET Framework'ün bir parçası ve duygu tanıma API'si kullanan Windows için kendi uygulamanızı oluşturmak için bu açık kaynak örnek şablon olarak kullanabilirsiniz.

## <a name="Prerequisites">Önkoşullar</a>
#### <a name="platform-requirements"></a>Platform gereksinimleri  
Aşağıdaki örnekte .NET Framework için geliştirilen kullanarak [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Duygu API'sine abone olma ve aboneliği anahtarı alma  
Örnek oluşturmadan önce Microsoft Bilişsel hizmetler parçası olan duygu tanıma API'si abone olmalısınız. Bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide, hem birincil ve ikincil anahtar kullanılabilir. API anahtar gizli tutmak için en iyi uygulamaları takip ettiğinizden emin olun ve güvenliğini sağlayın.  

#### <a name="get-the-client-library-and-example"></a>İstemci Kitaplığı ve örnek alın  
Duygu tanıma API'si istemci kitaplığı aracılığıyla karşıdan yüklenebilir [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). Tercih ettiğiniz bir klasöre ayıklanacak indirilen ZIP dosyasının gerekir, çok sayıda kullanıcı Visual Studio 2015 klasörü seçin.
## <a name="Step1">1. adım: örnek açın</a>
1.  Microsoft Visual Studio 2015'ı başlatın ve tıklatın **dosya**seçin **açık**, ardından **proje/çözüm**.
2.  İndirilen duygu tanıma API'si dosyalarını kaydettiğiniz klasöre göz atın. Tıklayın **duygu**, ardından **Windows**ve ardından **örnek WPF** klasör.
3.  Adlı Visual Studio 2015 çözümü (.sln) dosyasını açmak için çift **EmotionAPI WPF Samples.sln**. Bu çözümü Visual Studio'da açın.

## <a name="Step2">2. adım: örnek oluşturma</a>
1. İçinde **Çözüm Gezgini**, sağ tıklatın **başvuruları** seçip **NuGet paketlerini Yönet**.

  ![Açık Nuget Paket Yöneticisi](../Images/EmotionNuget.png)

2.  **NuGet Paket Yöneticisi** penceresi açılır. İlk seçin **Gözat** sol üst köşedeki sonra arama kutusuna "Newtonsoft.Json" seçin **Newtonsoft.Json** paketini ve tıklayın **yükleme**.  

  ![NuGet paketine göz atın](../Images/EmotionNugetBrowse.png)  

3.  Ctrl + Shift + B tuşuna basın veya tıklatın **yapı** Şerit menüsünde seçip **yapı çözümü**.

## <a name="Step3">3. adım: örnek çalıştırma</a>
1.  Yapı tamamlandıktan sonra basın **F5** veya **Başlat** örneği çalıştırmak için Şerit menüsünde.
2.  Duygu tanıma API'si penceresiyle bulun **metin kutusu** okuma "**başlatmak için abonelik anahtarınızı buraya yapıştırın**". Abonelik anahtarınızı ekran görüntüsü gösterildiği gibi metin kutusuna yapıştırın. Abonelik anahtarınızı PC veya dizüstü bilgisayarınız üzerinde "Anahtarı Kaydet" düğmesini tıklatarak kalıcı hale getirmek seçebilirsiniz. Abonelik anahtarı sistemden silmek istediğinizde, PC ya da dizüstü bilgisayara kaldırmak için "anahtar Sil"'ı tıklatın.
  
  ![Duygu tanıma işlevselliğini arabirimi](../Images/EmotionKey.png)

3.  Altında "**seçin senaryo**"kullanın ya da iki senaryo tıklatıp"**bir akış kullanarak duygu algılama**"veya"**bir URL'yi kullanarak duygu algılama**", ardından yönergeleri izleyin Ekran. Microsoft, karşıya yükleme ve bunları duygu API ve ilgili hizmetler geliştirmek için kullanabilir görüntüleri alır. Bir görüntü göndererek, izlediğinizden onaylamanız bizim [kullanım kuralları Geliştirici](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Bu örnek uygulama ile kullanılacak örnek görüntülerin vardır. Bu görüntüleri bulabilirsiniz [yüz API Github depo](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) altında **veri** klasör. Lütfen bu görüntüleri kullanımını Orta kullanmak sözleşmesi Bu örnekte test ancak yeniden yayımlama için kullanılacak Tamam olduğu anlamına gelir lisanslı unutmayın.

## <a name="Review">Gözden geçirin ve öğrenin</a>
Çalışan bir uygulama sahip olduğunuza göre bize nasıl Bu örnek uygulamayı Microsoft Bilişsel hizmetleriyle tümleştirilen gözden geçirin. Bu, bu uygulamanın üzerine yapı devam ya da Microsoft duygu tanıma API'si kullanılarak kendi uygulamanızı geliştirin daha kolay hale getirir. 

Bu örnek uygulama duygu tanıma API'si istemci kitaplığı ince C# istemci için sarmalayıcı Microsoft duygu API kullanır. Yukarıda açıklandığı gibi örnek uygulama yapılandırıldığında, istemci kitaplığı NuGet paket aldı. Başlıklı klasöründeki istemci kitaplığı kaynak kodu gözden geçirebilirsiniz "[istemci Kitaplığı](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)" altında **duygu**, **Windows**, **istemci Kitaplığı** , hangi indirilen dosya deposu parçası yukarıda belirtilen [Önkoşullar](#Prerequisites).
 
Ayrıca istemci kitaplığı kodda kullanma bulabilirsiniz **Çözüm Gezgini**: altında **EmotionAPI WPF_Samples**, genişletin **DetectEmotionUsingStreamPage.xaml** için bulun **DetectEmotionUsingStreamPage.xaml.cs**, yerel olarak saklanan bir dosyaya gözatmak için kullanılan veya genişletin **DetectEmotionUsingURLPage.xaml** bulmak için  **DetectEmotionUsingURLPage.xaml.cs**, bir resim URL'si karşıya yüklenirken kullanılan. Çift tıklayın. bunları şekilde xaml.cs dosyalarını açma Visual Studio'da yeni Windows. 

Duygu istemci kitaplığı bizim örnek uygulamasında nasıl kullanılan gözden geçirme, iki kod parçacıkları bakalım **DetectEmotionUsingStreamPage.xaml.cs** ve **DetectEmotionUsingURLPage.xaml.cs**. Her dosya "Anahtar örnek kodu BAŞLATIR burada" ve "Anahtar örnek kodu sona ERER burada" aşağıda parçacıkları çoğaltılamaz kod bulmanıza yardımcı olmak üzere gösteren kod açıklamaları içerir.

Giriş olarak bir resim URL'si veya ikili resim verileri (biçiminde bir sekizli akış) ile çalışabilmek için duygu API'dir. İki seçenek aşağıda incelenen. Her iki durumda da ilk kullanarak bir bulduğunuz duygu istemci kitaplığı kullanmanıza olanak sağlayan yönergesi. 
```csharp

            // ----------------------------------------------------------------------- 
            // KEY SAMPLE CODE STARTS HERE 
            // Use the following namespace for EmotionServiceClient 
            // ----------------------------------------------------------------------- 
            using Microsoft.ProjectOxford.Emotion; 
            using Microsoft.ProjectOxford.Emotion.Contract; 
            // ----------------------------------------------------------------------- 
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------- 
```
#### <a name="detectemotionusingurlpagexamlcs"></a>DetectEmotionUsingURLPage.xaml.cs 

Bu kod parçacığını istemci kitaplığı abonelik anahtarınızı ve fotoğraf URL'si duygu API Hizmeti'ne göndermek için nasıl kullanılacağını gösterir. 

```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            window.Log("EmotionServiceClient is created");

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                //
                // Detect the emotions in the URL
                //
                Emotion[] emotionResult = await emotionServiceClient.RecognizeAsync(url);
                return emotionResult;
            }
            catch (Exception exception)
            {
                window.Log("Detection failed. Please make sure that you have the right subscription key and proper URL to detect.");
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingstreampagexamlcs"></a>DetectEmotionUsingStreamPage.xaml.cs 

Aşağıda gösterilen abonelik anahtarınızı ve duygu tanıma API'si için yerel olarak saklanan bir görüntü gönderme olur. 


```csharp


            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                Emotion[] emotionResult;
                using (Stream imageFileStream = File.OpenRead(imageFilePath))
                {
                    //
                    // Detect the emotions in the URL
                    //
                    emotionResult = await emotionServiceClient.RecognizeAsync(imageFileStream);
                    return emotionResult;
                }
            }
            catch (Exception exception)
            {
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
<!--
## <a name="Related">Related Topics</a>
[Emotion API Overview](.)
-->
