---
title: "Öğretici: Görüntü - duygu tanıma API'si, yüz temel duyguları tanıyın,C#"
titlesuffix: Azure Cognitive Services
description: Bir görüntüdeki yüzlerde bulunan ifadeleri tanımak için basit bir Windows uygulamasını keşfedin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 01/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: da605ec4013fb11606f99f3d9a2dcfcfcab00d3b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163336"
---
# <a name="tutorial-recognize-emotions-on-a-face-in-an-image"></a>Öğretici: Resimdeki yüz temel duyguları tanıyın.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bir görüntüdeki yüzlerde bulunan ifadeleri tanımak için Duygu Tanıma API'sini kullanan basit bir Windows uygulamasını keşfedin. Aşağıdaki örnek bir görüntü URL'si veya yerel ortamda depolanan dosya göndermenizi sağlar. Bu açık kaynak örneği Duygu Tanıma API'si ve .NET Framework'ün bir parçası olan WPF (Windows Presentation Foundation) hizmetini kullanarak kendi Windows uygulamanızı derlemek için bir şablon olarak kullanabilirsiniz.

## <a name="Prerequisites">Önkoşullar</a>
#### <a name="platform-requirements"></a>Platform gereksinimleri  
Aşağıdaki örnek [Visual Studio 2015 Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) kullanılarak .NET Framework için geliştirilmiştir.  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Duygu Tanıma API’sine abone olma ve abonelik anahtarı alma  
Örneği oluşturmadan önce, Microsoft Bilişsel Hizmetler’in parçası olan Duygu Tanıma API'sine abone olmanız gerekir. Bkz. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide, hem birincil hem de ikincil anahtar kullanılabilir. API anahtarınızın gizliliğini ve güvenliğini korumak için en iyi yöntemleri uyguladığınızdan emin olun.  

#### <a name="get-the-client-library-and-example"></a>İstemci kitaplığını ve örneği alma  
Duygu Tanıma API'si istemci kitaplığını [SDK](https://www.github.com/microsoft/cognitive-emotion-windows) aracılığıyla indirebilirsiniz. İndirilen zip dosyasının istediğiniz bir klasöre ayıklanması gerekir, çoğu kullanıcı, Visual Studio 2015 klasörünü seçer.
## <a name="Step1">1. adım: Örnek Aç</a>
1.  Microsoft Visual Studio 2015'i başlatıp **Dosya**, **Aç** ve **Proje/Çözüm** yolunu izleyin.
2.  İndirdiğiniz Duygu Tanıma API'si dosyalarını kaydettiğiniz klasörü açın. **Emotion**, **Windows** ve ardından **Sample-WPF** klasörüne tıklayın.
3.  **EmotionAPI-WPF-Samples.sln** adlı Visual Studio 2015 Çözümü (.sln) dosyasına çift tıklayarak açın. Çözüm Visual Studio'da açılır.

## <a name="Step2">2. adım: Örnek oluşturma</a>
1. **Çözüm Gezgini**'nde **Başvurular**'a sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin.

  ![NuGet Paket Yöneticisi'ni açın](../Images/EmotionNuget.png)

2.  **Nuget Paket Yöneticisi** penceresi açılır. Önce sol üst köşeden **Gözat**'ı seçin, ardından arama kutusuna “Newtonsoft.Json” yazın, **Newtonsoft.Json** paketini seçin ve **Yükle**'ye tıklayın.  

  ![NuGet paketine göz atma](../Images/EmotionNugetBrowse.png)  

3.  Ctrl+Shift+B tuşlarına basın veya şerit menüsünde **Derle**'ye tıklayıp **Çözümü Derle**'yi seçin.

## <a name="Step3">3. adım: Örneği çalıştırın</a>
1.  Derleme tamamlandıktan sonra örneği çalıştırmak için **F5** tuşuna basın veya şerit menüsündeki **Başlat** düğmesine tıklayın.
2.  "**Paste your subscription key here to start**" (Başlamak için abonelik anahtarınızı buraya yapıştırın) yazan **metin kutusuna** sahip olan Duygu Tanıma API'si penceresini bulun. Abonelik anahtarınızı aşağıdaki ekran görüntüsünde gördüğünüz gibi metin kutusuna yapıştırın. "Anahtarı Kaydet" düğmesine tıklayarak abonelik anahtarınızın masaüstü veya dizüstü bilgisayarınızda kalmasını sağlayabilirsiniz. Abonelik anahtarını sisteminizden silmek istediğinizde "Anahtarı Sil"e tıklayarak masaüstü veya dizüstü bilgisayarınızdan kaldırabilirsiniz.

  ![Duygu Tanıma İşlevi Arabirimi](../Images/EmotionKey.png)

3.  "**Select Scenario**" (Senaryo seçin) bölümünde “**Detect emotion using a stream**” (Akış kullanarak duygu tanıma) veya “**Detect emotion using a URL**” (URL kullanarak duygu tanıma) senaryolarından birini seçip ekrandaki talimatları izleyin. Microsoft, yüklediğiniz resimleri alır ve bunları Duygu Tanıma API'si ile ilgili hizmetleri geliştirmek için kullanabilir. Görüntü göndererek [Geliştirici Kullanım Kurallarımıza](https://azure.microsoft.com/support/legal/developer-code-of-conduct/) uyduğunuzu onaylamış olursunuz.
4.  Bu örnek uygulamayla kullanabileceğiniz örnek görüntüler mevcuttur. Bu görüntüleri bulabilirsiniz [yüz tanıma API'si GitHub deposunu](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) altında **veri** klasör. Bu görüntülerin Adil Kullanım lisansına sahip olduğunu ve bu örnekte test için kullanılabileceğini ancak yeniden yayımlanamayacağını unutmayın.

## <a name="Review">Gözden Geçirme ve Öğrenme</a>
Artık çalışan bir uygulamanız olduğuna göre örnek uygulamanın Microsoft Bilişsel Hizmetlerle nasıl tümleştirildiğini inceleyebilirsiniz. Bu adım, Microsoft Duygu Tanıma API'sini kullanarak bu uygulamayı derlemeye devam etmenizi veya kendi uygulamanızı geliştirmenizi kolaylaştıracaktır.

Bu örnek uygulamada Duygu Tanıma API'si İstemci Kitaplığı ve Microsoft Duygu Tanıma API'si için basit bir C# istemci sarmalaması kullanılmaktadır. Örnek uygulamayı yukarıda anlatılan şekilde derlediğinizde İstemci Kitaplığını bir NuGet paketinden aldınız. **Emotion**, **Windows**, **Client Library** dizinindeki "[Client Library](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)" klasöründe bulunan İstemci Kitaplığı kaynak kodunu gözden geçirebilirsiniz. Bu dosya, [Ön koşullar](#Prerequisites) bölümünde bahsedilen dosya deposuyla birlikte indirilmiştir.

İstemci Kitaplığı kodda kullanmayı da bulabilirsiniz **Çözüm Gezgini**: Altında **EmotionAPI WPF_Samples**, genişletme **DetectEmotionUsingStreamPage.xaml** bulunacak **DetectEmotionUsingStreamPage.xaml.cs**, tarama için kullanılan bir yerel olarak depolanan dosya veya genişletin **DetectEmotionUsingURLPage.xaml** bulunacak **DetectEmotionUsingURLPage.xaml.cs**, bir resim URL'si karşıya yüklenirken kullanılır. .xaml.cs dosyalarına çift tıklayarak Visual Studio'da yeni pencerelerde açın.

Duygu Tanıma İstemci Kitaplığının örnek uygulamada nasıl kullanıldığını incelerken **DetectEmotionUsingStreamPage.xaml.cs** ve **DetectEmotionUsingURLPage.xaml.cs** dosyalarından alınan iki kod parçacığına da bakalım. Aşağıda gösterilen kod parçacıklarını bulmanıza yardımcı olmak için dosyalara “KEY SAMPLE CODE STARTS HERE” (Anahtar kodu örneği başlangıcı) ve “KEY SAMPLE CODE ENDS HERE” (Anahtar kodu örneği sonu) şeklinde kod açıklamaları eklenmiştir.

Duygu Tanıma API'si giriş olarak görüntü URL'si veya ikili görüntü verisi (sekizli akış biçiminde) kullanabilir. İki seçenek aşağıda incelenmiştir. İki durumda da ilk olarak Duygu Tanıma İstemci Kitaplığını kullanmanızı sağlayan bir using yönergesiyle karşılaşırsınız.
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

Bu kod parçacığı, Duygu Tanıma API'si hizmetine abonelik anahtarınızı ve fotoğraf URL'si göndermek için İstemci Kitaplığının nasıl kullanıldığını göstermektedir.

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

Aşağıda Duygu Tanıma API'sine abonelik anahtarınızı ve yerel ortamda depolanan bir görüntüyü gönderme adımları gösterilmektedir.


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
