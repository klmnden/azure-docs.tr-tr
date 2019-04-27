---
title: 'Öğretici: Azure Hizmetleri - Custom Vision tanımak için özel logo algılayıcısı kullanın'
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, bir logo algılama senaryonun parçası olarak Azure özel görüntü işleme kullanan bir örnek uygulamayı adım adım. Custom Vision uçtan uca uygulama sunmak için diğer bileşenlerle nasıl kullanıldığını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: pafarley
ms.openlocfilehash: 259787a90b61b171f391dc02276214f17a57d0d3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60606662"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Öğretici: Azure hizmeti logo kameradan resim tanıma

Bu öğreticide, daha büyük bir senaryonun parçası olarak Azure özel görüntü işleme kullanan bir örnek uygulama hakkında bilgi edineceksiniz. Yapay ZEKA Visual sağlama uygulaması, mobil platformlar için Xamarin.Forms uygulaması, Azure hizmet logoları kamera resimlerini analiz eder ve sonra kullanıcının Azure hesabı için gerçek Hizmetleri dağıtır. Burada, özel görüntü işleme diğer bileşenleri ile koordinasyon halinde kullanışlı bir uçtan uca uygulama sunun için nasıl kullandığını öğreneceksiniz. Kendiniz için tüm uygulama senaryosu çalıştırmak veya yalnızca özel görüntü işleme kurulumun bir parçası tamamlayın ve uygulama bunu nasıl kullandığını keşfedin.

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:

> [!div class="checklist"]
> - Azure hizmet logoları tanımak için özel nesne algılayıcısı oluşturun.
> - Uygulamanızı Azure görüntü işleme ve özel görüntü işleme bağlanın.
> - Azure uygulama Hizmetleri'nden dağıtmak için bir Azure hizmet sorumlusu hesabı oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Visual Studio için Xamarin iş yükü (bkz [yükleme Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- Bir iOS veya Android öykünücüsü Visual Studio için
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (isteğe bağlı)

## <a name="get-the-source-code"></a>Kaynak kodu alma

Sağlanan web uygulamasını kullanmak istiyorsanız, kopyalama veya uygulamanın kaynak kodunu indirin [AI Visual sağlama](https://github.com/Microsoft/AIVisualProvision) github deposu. Açık *Source/VisualProvision.sln* dosyasını Visual Studio'da. Daha sonra uygulamayı çalıştırabilirsiniz bazı proje dosyaları düzenleme.

## <a name="create-an-object-detector"></a>Bir nesne algılayıcısı oluşturma

Oturum [Custom Vision Web sitesi](https://customvision.ai/) ve yeni bir proje oluşturun. Bir nesne algılama projesi belirtin ve Logo etki alanını kullan; Bu logo algılama için en iyi duruma getirilmiş bir algoritma kullandığınız hizmet sağlar. 

![Chrome tarayıcıda Custom Vision Web sitesindeki yeni proje penceresi](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Ardından, Azure hizmet logoları görüntüleri karşıya yükleme ve bunları el ile etiketleme logosu algılama algoritması eğitin. AIVisualProvision depo kullanabileceğiniz eğitim görüntü kümesi içerir. Web sitesinde seçin **görüntüleri ekleme** düğmesini **eğitim resmi** sekmesi. Ardından **belgeleri/resimler/Training_DataSet** depo klasörü. El ile her bir resim, logolar etiketlemek ihtiyacınız olacak şekilde bu projeyi yalnızca test ediyorsanız, yalnızca bir alt kümesini görüntüleri karşıya yüklemek isteyebilirsiniz. Kullanmayı planladığınız her etiket en az 15 örnekleri yükleyin.

Eğitim resmi karşıya yüklenmesinin ardından, ilk bir görüntü seçin. Bu etiketleme penceresi açılır. Kutularında çizme ve her logo her görüntü için etiketler atayabilirsiniz. 

![Özel görüntü işleme Web sitesinde etiketleme logosu](media/azure-logo-tutorial/tag-logos.png)

Uygulama, belirli bir etiketi dizelerle çalışacak şekilde yapılandırılır. Tanımlarında bulabilirsiniz *Source\VisualProvision\Services\Recognition\RecognitionService.cs* dosyası:

[!code-csharp[Tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?range=18-33)]

Görüntü etiketi sonra bir sonraki etiket için sağa gidin. İşiniz bittiğinde etiketleme pencereyi kapatın.

## <a name="train-the-object-detector"></a>Nesne algılayıcısı eğitin

Sol bölmede ayarlamak **etiketleri** geçin **etiketli** görüntülerinizi görüntülenecek. Sonra modeli eğitmek için sayfanın üst kısmındaki yeşil düğmeyi seçin. Bunun yapılması, yeni görüntüleri aynı etiketleri tanımak için algoritma sağlanır. Ayrıca bazı doğruluğu puanları oluşturmak için var olan görüntülerinizin modeli test eder.

![Custom Vision eğitim resmi sekmesinde Web. Bu ekran görüntüsünde, eğit düğmesine ana hatlarıyla açıklanmıştır](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Tahmin URL'sini alma

Modelinizi eğitildi sonra uygulamanızla tümleştirmek hazır olursunuz. Bunu yapmak için uç nokta URL'si (uygulama sorgular modelinizin adresi) ve tahmin anahtarı (tahmin isteği için uygulama erişimi vermek için) almanız gerekir. Üzerinde **performans** sekmesinde **tahmin URL** sayfanın üstünde düğme.

![Bir URL adresi ve API anahtarı görüntüleyen Prediction API'deki pencere gösteren özel görüntü işleme Web sitesi](media/azure-logo-tutorial/cusvis-endpoint.png)

Görüntü dosyası URL'yi kopyalayın ve **tahmin anahtar** uygun alanlara değer *Source\VisualProvision\AppSettings.cs* dosyası:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=22-26)]

## <a name="examine-custom-vision-usage"></a>Custom Vision kullanımını inceleyin

Açık *Source/VisualProvision/Services/Recognition/CustomVisionService.cs* uygulama Custom Vision anahtarını ve uç nokta URL'nizi nasıl kullandığını görmek için dosya. **PredictImageContentsAsync** yöntemi (için zaman uyumsuz görev management) belirteci iptal birlikte bir görüntü dosyasının bayt akışı alır, Custom Vision tahmin API çağırır ve tahmin sonuçlarını döndürür. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?range=12-28)]

Bu sonuç biçimi alır bir **PredictionResult** örneği, kendisi listesini içeren **tahmin** örnekleri. A **tahmin** algılanan bir etiket ve sınırlayıcı kutusunun konumuna görüntü içerir.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?range=3-12)]

Uygulama bu verileri nasıl işlediği hakkında daha fazla bilgi için başlayın **GetResourcesAsync** yöntemi. Bu yöntem tanımlanan *Source/VisualProvision/Services/Recognition/RecognitionService.cs* dosya.  

## <a name="add-computer-vision"></a>Görüntü işleme ekleme

Custom Vision bölümü öğreticinin tamamlanmış demektir. Uygulamayı çalıştırmak istiyorsanız, görüntü işleme hizmeti de tümleştirmek gerekir. Uygulama logosu Algılama işlemini tamamlamak için görüntü işleme metin tanıma özelliğini kullanır. Bir Azure logosu görünümünü tarafından tanınacak *veya* yanında yazdırılan metin. Özel işleme modelleri, görüntü işleme kullanan görüntü veya video üzerinde belirli işlemlerin gerçekleştirilmesi.

Görüntü işleme hizmeti için bir anahtarı ve uç nokta URL'si almak için abone olun. Bu adım hakkında daha fazla yardım için bkz: [Abonelik anahtarları edinme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe).

![Azure portalında, seçili Hızlı Başlangıç menüsünde görüntü işleme hizmeti. API uç nokta URL'si olarak bir bağlantı anahtarları için özetlenen](media/azure-logo-tutorial/comvis-keys.png)

Ardından, açık *Source\VisualProvision\AppSettings.cs* dosya ve doldurma `ComputerVisionEndpoint` ve `ComputerVisionKey` doğru değerlere sahip değişkenler.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=28-32)]

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Uygulama Hizmetleri Azure aboneliğinize dağıtmak için bir Azure hizmet sorumlusu hesabı gerektirir. Bir hizmet sorumlusu, rol tabanlı erişim denetimi kullanarak uygulama için belirli izinleri için temsilci seçme sağlar. Daha fazla bilgi için bkz. [hizmet sorumluları Kılavuzu](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals).

Bir hizmet sorumlusu Azure Cloud Shell veya Azure CLI kullanarak aşağıda gösterildiği gibi oluşturabilirsiniz. Başlamak için oturum açın ve kullanmak istediğiniz aboneliği seçin.

```console
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Ardından, hizmet sorumlusu oluşturun. (Bu işlemin tamamlanması biraz zaman alabilir.)

```console
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

İşlemin başarıyla tamamlanmasından sonra aşağıdaki görmelisiniz gerekli kimlik bilgilerini de dahil olmak üzere JSON çıkışını.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```

Not `clientId` ve `tenantId` değerleri. Uygun alanları eklemek *Source\VisualProvision\AppSettings.cs* dosya.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=8-16)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bu noktada, uygulama erişimini verdiniz:

- eğitilen bir özel görüntü işleme modeli
- Görüntü işleme hizmeti
- bir hizmet sorumlusu hesabı

Uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Çözüm Gezgini'nde seçin **VisualProvision.Android** proje veya **VisualProvision.iOS** proje. Ana araç çubuğundaki aşağı açılan menüden bir karşılık gelen bir öykünücü veya bağlı bir mobil cihaz seçin. Ardından, uygulamayı çalıştırın.

    > [!NOTE]
    > Bir iOS öykünücüyü çalıştırmak için bir MacOS cihazı gerekir.

1. İlk ekranda, hizmet sorumlusu istemci kimliği, Kiracı kimliği ve parola girin. Seçin **oturum açma** düğmesi.

    > [!NOTE]
    > Bazı öykünücülerde **oturum açma** düğmesi etkinleştirilmemiş olabilir bu adımı. Böyle bir durumda, uygulamayı durdurun açın *Source/VisualProvision/Pages/LoginPage.xaml* dosya, bulma `Button` etiketli öğe **oturum açma düğmesi**şu satırı kaldırın ve ardından uygulamayı çalıştırın yeniden.
    >  ```xaml
    >  IsEnabled="{Binding IsValid}"
    >  ```
    
    ![Hizmet sorumlusu kimlik bilgileri için alanları gösteren uygulama ekranı](media/azure-logo-tutorial/app-credentials.png)

1. Sonraki ekranda, aşağı açılan menüden Azure aboneliğinizi seçin. (Bu menü, hizmet sorumlusu erişimi olan abonelikleri tümünün içermelidir.) Seçin **devam** düğmesi. Bu noktada, uygulama cihazın kamera ve fotoğraf depolama erişim vermek isteyebilir. Erişim izinleri verin.

    ![Hedef Azure aboneliğinin bir açılan alan gösteren uygulama ekranı](media/azure-logo-tutorial/app-az-subscription.png)


1. Cihazınızda kamerayı etkinleştirilir. Fotoğraf, eğitilmiş Azure hizmeti logoları birinin yararlanın. Dağıtım penceresi, yeni hizmetler için bir bölge ve kaynak grubu (Azure portalından dağıtımı yaptığınız şekilde) seçmek için isteyecektir. 

    ![Smartphone kamera ekran iki kağıt kesmeler Azure logoları odaklanır.](media/azure-logo-tutorial/app-camera-capture.png)

    ![Dağıtım bölge ve kaynak grubu için alanları gösteren bir uygulama ekranı](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm bu senaryonun adımları izleyen ve uygulama hesabınıza Azure hizmetlerini dağıtmak için kullanılan, Git [Azure portalında](https://ms.portal.azure.com/). Hizmetleri kullanmak istemiyorsanız, iptal.

Özel görüntü ile kendi nesne algılama projenizi oluşturmayı planlıyorsanız, bu öğreticide oluşturduğunuz logosu algılama projesini silmek isteyebilirsiniz. Ücretsiz deneme için özel görüntü işleme için yalnızca iki proje sağlar. Logo algılama proje silmek için [Custom Vision Web sitesi](https://customvision.ai)açın **projeleri** ve altında Çöp Kutusu simgesini seçin **Yeni Projem**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ayarlayın ve logoları mobil kamera görüntülerde algılamak için özel görüntü işleme hizmeti kullanan tam özellikli bir Xamarin.Forms uygulaması incelediniz. Ardından, kendi uygulamasını oluşturduğunuzda, güçlü ve doğru yapabileceğiniz bir özel görüntü işleme modeli oluşturmak için en iyi adımları öğrenin.

> [!div class="nextstepaction"]
> [Nasıl sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md)
