---
title: 'Öğretici: Windows ML ile ONNX modeli kullanma - Özel Görüntü İşleme Hizmeti'
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’den dışarı aktarılan bir ONNX modelini kullanan Windows UWP uygulamasının nasıl oluşturulacağını öğrenin.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 3a9e9bc92ce38c4bb8d6d83c8017fa223342e7d2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365613"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Öğretici: Windows ML (önizleme) ile Özel Görüntü İşleme hizmetinden ONNX modeli kullanma

Windows ML (önizleme) ile Özel Görüntü İşleme hizmetinden dışarı aktarılan bir ONNX modelinin nasıl kullanılacağını öğrenin.

Bu belgedeki bilgiler, Windows ML ile Özel Görüntü İşleme Hizmeti’nden dışarı aktarılan bir ONNX dosyasının nasıl kullanılacağını göstermektedir. Örnek bir Windows UWP uygulaması sağlanmıştır. Örneğe, köpek ve kedileri tanıyabilen bir eğitimli model eklenmiştir. Bu örnek ile kendi modelinizi nasıl kullanabileceğinize ilişkin adımlar da sağlanmıştır.

> [!div class="checklist"]
> * Örnek uygulama hakkında
> * Örnek kodunu alma
> * Örneği çalıştırma
> * Kendi modelinizi kullanma

## <a name="prerequisites"></a>Ön koşullar

* Aşağıdaki özellikleri içeren bir Windows 10 cihazı:

    * Kamera.

    * __Evrensel Windows Platformu geliştirmesi__ iş yükü etkinleştirilmiş Visual Studio 2017 15.7 veya sonraki bir sürüm.

    * Etkinleştirilmiş geliştirici modu. Daha fazla bilgi için [Geliştirme için cihazınızı etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belgesine bakın.

* (İsteğe bağlı) Özel Görüntü İşleme hizmetinden dışarı aktarılmış bir ONNX dosyası. Daha fazla bilgi için [Mobil cihazlarla kullanmak üzere modelinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belgesine bakın.

    > [!NOTE]
    > Kendi modelinizi kullanmak için, [Kendi modelinizi kullanma](#use-your-own-model) bölümündeki adımları izleyin.

## <a name="about-the-example-app"></a>Örnek uygulama hakkında

Uygulama, genel bir Windows UWP uygulamasıdır. Modele görüntü sağlamak için Windows 10 cihazınızdaki kamerayı kullanır. Model tarafından döndürülen etiketler ve puanlar, video önizlemesinin altında görüntülenir.

* Kameradan veriler geldikçe tek tek kareleri ayıklamak için [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) kullanılır. Kareler daha sonra puanlama için modele gönderilir.

* Model, eğitildiği etiketleri ve görüntünün söz konusu öğeyi içerdiğinden ne kadar emin olunduğunu belirten bir kayan nokta değeri döndürür.

### <a name="the-ui"></a>Kullanıcı Arabirimi

__CaptureElement__ ve __TextBlock__ denetimleri kullanılarak örnek uygulama için kullanıcı arabirimi oluşturulur. CaptureElement, kameradaki videonun bir önizlemesini görüntüler ve TextBlock ise modelden döndürülen sonuçları görüntüler. 

### <a name="the-model"></a>Model

Örnekle birlikte sağlanan model (`cat-or-dog.onnx`), Bilişsel Hizmetler Özel Görüntü İşleme hizmeti kullanılarak oluşturulmuş ve eğitilmiştir. Eğitilmiş model daha sonra bir ONNX modeli olarak dışa aktarılmıştır. Bu hizmeti kullanma hakkında daha fazla bilgi için [Sınıflandırıcı oluşturma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) ve [Mobil cihazlarla kullanmak üzere modelinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belgelerine bakın.

> [!IMPORTANT]
> Örnekle birlikte sağlanan model, küçük bir dizi köpek ve kedi görüntüleri ile eğitilmiştir. Bu nedenle köpek ve kedileri tanıma konusunda dünyanın en iyisi olmayabilir.

### <a name="the-model-class-file"></a>Model sınıfı dosyası

Bir Windows UWP uygulamasına ONNX dosyası eklediğinizde bir .cs dosyası oluşturulur. Bu dosya, `.onnx` dosyasıyla aynı ada sahiptir (bu örnekte `cat-or-dog`) ve C#’tan modelle birlikte çalışmak için kullanılan sınıfları içerir. Ancak oluşturulan sınıftaki varlıklar `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput` gibi adlara sahip olabilir. Bu girişleri güvenli bir şekilde kolay bir adla yeniden adlandırabilirsiniz (sağ tıkla, yeniden adlandır).

> [!NOTE]
> Örnek kod, oluşturulan sınıf ve yöntem adlarını aşağıdaki şekilde yeniden düzenlemiştir:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Kamera erişimi

`Package.appxmanifest` dosyasındaki __Özellikler__ sekmesi, web kamerasına ve mikrofona erişim sağlanacak şekilde yapılandırılır.

> [!NOTE]
> Bu örnekte ses kullanılmıyor olsa da, cihazımda kameraya erişebilmem için önce mikrofonu etkinleştirmem gerekiyordu.

Uygulama, varsa, cihazınızın arkasındaki kamerayı almaya çalışır. Kameradan video yakalamaya başlamak için [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) sınıfını kullanır. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader), video karelerini yakalamak ve bunları modele göndermek için kullanılır.

## <a name="get-the-example-code"></a>Örnek kodunu alma

Örnek uygulamaya [https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP) konumundan erişilebilir.

## <a name="run-the-example"></a>Örneği çalıştırma

1. Visual Studio’dan uygulamayı başlatmak için `F5` tuşunu kullanın. Geliştirici modunu etkinleştirmeniz istenebilir. Daha fazla bilgi için [Geliştirme için cihazınızı etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belgesine bakın.

2. İstendiğinde, uygulamanın cihazınızdaki kamera ve mikrofona erişmesine izin verin.

3. Kamerayı bir köpeğe veya kediye yönlendirin. Görüntünün bir köpek veya kedi içerip içermediğine yönelik puan, uygulamadaki önizlemenin altında görüntülenir.

    > [!TIP]
    > Elinizin altında köpek ve kedi yoksa, bir köpek ya da kedi fotoğrafı kullanabilirsiniz.

## <a name="use-your-own-model"></a>Kendi modelinizi kullanma

Kendi modelinizi kullanmak için aşağıdaki adımları uygulayın:

> [!IMPORTANT]
> Bu bölümdeki adımlar, geçerli modeli (cat-or-dog.cs) yeniden adlandırır ve yeni modelin sınıf ve yöntem adlarını yeniden düzenler. Bunun amacı, örnek modelle adlandırma çakışmalarını önlemektir.

1. Özel Görüntü İşleme hizmetini kullanarak model eğitin. Modeli eğitme hakkında bilgi için bkz. [Sınıflandırıcı oluşturma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier).

2. Eğitilen modeli, ONNX modeli olarak dışarı aktarın. Modeli dışarı aktarma hakkında bilgi için [Mobil cihazlarla kullanmak üzere modelinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belgesine bakın.

3. Çözüm Gezgini’nde __cat-or-dog.cs__ dosyasına sağ tıklayın ve __cat-or-dog.txt__ olarak yeniden adlandırın. Yeniden adlandırılması, yeni modelle ad çakışmalarını engeller.

    > [!TIP]
    > Yeni modelde sınıf adları için farklı adlar da kullanabilirsiniz, ancak mevcut adların yeniden kullanılması daha kolaydır.

4. Çözüm Gezgini’nde __VisionApp__ girişine sağ tıklayın ve __Ekle__ > __Mevcut öğe...__ seçeneklerini belirleyin.

5. Modele yönelik bir sınıf oluşturmak için, içeri aktarılacak ONNX dosyasını seçin ve __Ekle__ düğmesini seçin. Çözüm Gezgini’ne ONNX dosyasıyla aynı ada (ancak `.cs` uzantısına) sahip yeni bir sınıf eklenir.

6. Oluşturulan .cs dosyasını açın ve aşağıdaki öğelerin adlarını bulun:

    > [!IMPORTANT]
    > Sınıfları ve işlevleri tanımak için kılavuz olarak örnek `cat-or-dog.txt` dosyasını kullanın.

    * Model girişini tanımlayan sınıf. Oluşturulan ad, `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput` adına benzeyebilir. Bu sınıfı __ModelInput__ olarak yeniden adlandırın.
    * Model çıkışını tanımlayan sınıf. Oluşturulan ad, `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput` adına benzeyebilir. Bu sınıfı __ModelOutput__ olarak yeniden adlandırın.
    * Modeli tanımlayan sınıf. Oluşturulan ad, `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model` adına benzeyebilir. Bu sınıfı __Model__ olarak yeniden adlandırın.
    * Modeli oluşturan yöntem. Oluşturulan ad, `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model` adına benzeyebilir. Bu yöntemi __CreateModel__ olarak yeniden adlandırın.

7. Çözüm Gezgini’nde `.onnx` dosyasını __Varlıklar__ klasörüne taşıyın. 

8. Uygulama paketine ONNX dosyasını dahil etmek için `.onnx` dosyasını seçin ve özelliklerde __Derleme Eylemi__’ni __İçerik__ olarak ayarlayın.

9. __MainPage.xaml.cs__ dosyasını açın. Aşağıdaki satırı bulun ve dosya adını yeni `.onnx` dosyası olarak değiştirin:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Bu değişiklik, çalışma zamanında yeni modeli yükler.

10. Uygulamayı derleyin ve çalıştırın. Görüntüleri puanlamak için artık yeni modeli kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Başka dışarı aktarma yollarını keşfetmek ve Özel Görüntü İşleme modelini kullanmak için aşağıdaki belgelere bakın:

* [Verilerinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Android uygulamasında dışarı aktarılan Tensorflow modelini kullanma](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Swift iOS uygulamasında dışarı aktarılan CoreML modelini kullanma](https://go.microsoft.com/fwlink/?linkid=857726)
* [Xamarin ile iOS uygulamasında dışarı aktarılan CoreML modelini kullanma](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Windows ML ile ONNX modellerini kullanma hakkında daha fazla bilgi için [Windows ML ile uygulamanızda bir modeli tümleştirme](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) belgesine bakın.
