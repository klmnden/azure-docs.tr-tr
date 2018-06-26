---
title: Windows ML - Bilişsel hizmetler özel görme ONNX modeliyle | Microsoft Docs
description: Bilişsel Hizmetleri'nden dışarı ONNX modelini kullanan bir Windows UWP uygulaması oluşturmayı öğrenin.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 0b128ba1800e74c20c09a9c5711c8473f1dd00d0
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939489"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Öğretici: özel görme ONNX modelden Windows ML (Önizleme) kullanın.

Windows ML (Önizleme) ile özel görme hizmetinden dışarı ONNX modelin nasıl kullanılacağını öğrenin.

Bu belgedeki bilgiler Windows ML özel görme hizmetiyle dışarı ONNX dosyasının nasıl kullanılacağını gösterir. Bir örnek Windows UWP uygulamasını sağlanmıştır. Köpekler ve kediler tanıyabilmesi eğitilen bir modelin örnekle dahil edilir. Adımları nasıl kendi modeli Bu örnek ile kullanabileceğiniz üzerinde de sağlanır.

> [!div class="checklist"]
> * Örnek uygulama hakkında
> * Örnek kod alın
> * Örneği çalıştırın
> * Kendi model kullanın

## <a name="prerequisites"></a>Önkoşullar

* Windows 10 cihaz ile:

    * Bir kamera.

    * Visual Studio 2017 sürüm 15.7 veya üstü __Evrensel Windows platformu geliştirme__ etkin iş yükü.

    * Geliştirici modu etkin. Daha fazla bilgi için bkz: [cihazınız geliştirme için etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belge.

* (İsteğe bağlı) Özel görme hizmetinden dışarı bir ONNX dosyası. Daha fazla bilgi için bkz: [modelinizi kullanmak için mobil aygıtlarla verme](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belge.

    > [!NOTE]
    > Kendi model kullanılacak adımları [kendi modelini kullanmak](#use-your-own-model) bölümü.

## <a name="about-the-example-app"></a>Örnek uygulama hakkında

Genel bir Windows UWP uygulaması uygulamasıdır. Kamera, model görüntülere sağlamak için Windows 10 cihazında kullanır. Etiketleri ve model tarafından döndürülen puanları video önizlemesi altında görüntülenir.

* Veri ancak kamera gelirken [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) kareleri tek tek ayıklamak için kullanılır. Çerçeve modeli Puanlama için gönderilir.

* Model eğitilmiş etiketleri ve nasıl emin resmi öğenin içeren olduğu gösteren bir float değeri döndürür.

### <a name="the-ui"></a>Kullanıcı Arabirimi

Örnek uygulama için kullanıcı Arabirimi kullanılarak oluşturulan __CaptureElement__ ve __TextBlock__ kontrol eder. CaptureElement kameradan video önizlemesi ve TextBlock modelden döndürülen sonuçları görüntüler. 

### <a name="the-model"></a>Modeli

Model (`cat-or-dog.onnx`) ile sağlanan örnek oluşturuldu ve Bilişsel Hizmetleri özel görme hizmeti kullanılarak eğitilmiş. Eğitim modeli sonra ONNX model olarak verilmedi. Bu hizmet kullanma hakkında daha fazla bilgi için bkz [nasıl oluşturulacağını sınıflandırıcı](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) ve [mobil cihazlarla kullanım için modelinizin dışarı](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belgeleri.

> [!IMPORTANT]
> Örnek ile sağlanan modeline köpek ve kat görüntüler, küçük bir kümesini ile eğitilmiş. Bu nedenle köpekler ve kediler algılamayı adresindeki dünyanın en iyi olmayabilir.

### <a name="the-model-class-file"></a>Model sınıfı dosyası

Bir Windows UWP uygulaması ONNX dosya eklediğinizde .cs dosyası oluşturur. Bu dosya aynı ada sahip `.onnx` dosyası (`cat-or-dog` Bu örnekte) ve C# modelden çalışmak için kullanılan sınıfları içerir. Bununla birlikte, oluşturulan sınıfın varlıklarda gibi adları olabilir `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Bu girişler güvenli bir şekilde adlandırabilirsiniz (sağ tıklatın, yeniden adlandırma) için bir kolay ad.

> [!NOTE]
> Kod örneği, aşağıdakiler için oluşturulan sınıf ve yöntemi adları yeniden düzenlenmiş sürümlere:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Kamera erişimi

__Yetenekleri__ sekmesinde `Package.appxmanifest` dosyası Web kamera ve mikrofon erişmesine izin vermek için yapılandırılır.

> [!NOTE]
> Bu örnek ses kullanılmıyor olsa bile, ı my aygıttaki kamerayı erişebilir önce etkinleştirmek vardı.

Uygulama varsa cihazınız arkası kamerayı almaya çalışır. Kullandığı [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) kamerasının video yakalanıyor başlatmak için sınıf. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) video çerçeveleri yakalamak ve bunları modele göndermek için kullanılır.

## <a name="get-the-example-code"></a>Örnek kod alın

Örnek uygulama kullanılabilir [ https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP ](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Örneği çalıştırın

1. Kullanım `F5` Visual Studio'dan uygulamayı başlatmak için anahtar. Geliştirici modunu etkinleştirmek için istenebilir. Daha fazla bilgi için bkz: [cihazınız geliştirme için etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belge.

2. İstendiğinde, kamera ve mikrofon aygıtınızda erişmek uygulama izin verme.

3. Köpek veya kat kamera gelin. Görüntünün bir köpek veya kat içerip içermediğini puanı uygulama önizlemede altında görüntülenir.

    > [!TIP]
    > Köpek veya kat kullanışlı yoksa köpek veya kat fotoğrafı kullanabilirsiniz.

## <a name="use-your-own-model"></a>Kendi model kullanın

Kendi model kullanmak için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Bu bölümdeki adımları geçerli modelin (kat veya dog.cs) yeniden adlandırın ve yeni model sınıfı ve yöntemi adlarını yeniden düzenleyin. Bu örnek modeli çakışmalarla adlandırma önlemek için yapılır.

1. Özel görme hizmetini kullanarak bir modeli eğitmek. Bir modeli eğitmek hakkında daha fazla bilgi için bkz: [nasıl oluşturulacağını sınıflandırıcı](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier).

2. Eğitim modeli ONNX modeli olarak dışarı aktarın. Bir model dışarı aktarma hakkında daha fazla bilgi için bkz: [modelinizi kullanmak için mobil aygıtlarla verme](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) belge.

3. Çözüm Gezgini'nde sağ __kat veya dog.cs__ ve ona yeniden adlandırın __kat veya dog.txt__. Yeniden adlandırmadan yeni model adı çakışmalarla engeller.

    > [!TIP]
    > Yeni modelde de sınıf adları için farklı adlar kullanabilirsiniz, ancak varolan adlarını yeniden daha kolaydır.

4. Çözüm Gezgini'nde sağ __VisionApp__ giriş ve ardından __Ekle__ > __varolan öğeyi...__ .

5. Model için bir sınıf oluşturmak için ONNX dosyayı içeri aktarmak ve ardından seçin __Ekle__ düğmesi. ONNX dosyasıyla aynı ada sahip yeni bir sınıf (ancak `.cs` uzantısı) Çözüm Gezgini'nde eklenir.

6. Oluşturulan .cs dosyasını açın ve aşağıdaki öğelerin adlarını bulun:

    > [!IMPORTANT]
    > Örneği kullanmak `cat-or-dog.txt` dosya sınıfları ve işlevleri tanımak için bir kılavuz olarak.

    * Giriş modeli tanımlayan sınıf. Oluşturulan ad benzer `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Bu sınıf için yeniden adlandırma __ModelInput__.
    * Model çıkış tanımlayan sınıf. Oluşturulan ad benzer `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Bu sınıf için yeniden adlandırma __ModelOutput__.
    * Modeli tanımlayan sınıf. Oluşturulan ad benzer `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Bu sınıf için yeniden adlandırma __modeli__.
    * Bir model oluşturur yöntemi. Oluşturulan ad benzer `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Bu yönteme yeniden adlandırma __CreateModel__.

7. Çözüm Gezgini'nde taşıma `.onnx` içine dosya __varlıklar__ klasör. 

8. Uygulama paketinde ONNX dosya eklemek için seçin `.onnx` dosya ve ayarlayın __yapı eylemi__ için __içerik__ özellikleri.

9. Açık __MainPage.xaml.cs__ dosya. Aşağıdaki satır ve dosya adını yeni bulmak `.onnx` dosyası:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Bu değişiklik yeni model çalışma zamanında yükler.

10. Derleme ve uygulamayı çalıştırın. Görüntüleri Puanlama amacıyla artık yeni modeli kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Diğer yolları dışarı aktarabilir ve bir özel görme modeli bulmak için aşağıdaki belgelere bakın:

* [Modelinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Dışarı aktarılan Tensorflow modeli bir Android uygulamasını kullanın](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Dışarı aktarılan CoreML modeli SWIFT iOS uygulamada kullanın](https://go.microsoft.com/fwlink/?linkid=857726)
* [Xamarin iOS uygulamasıyla CoreML modelinde kullanım dışarı](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

İle Windows ML ONNX modelleri kullanma hakkında daha fazla bilgi için bkz: [Windows ML ile uygulamanızı içine bir model tümleştirmek](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) belge.
