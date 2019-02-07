---
title: 'Öğretici: Azure Hizmetleri - Custom Vision tanımak için özel logo algılayıcısı kullanın'
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, bir logo algılama senaryonun parçası olarak Azure özel görüntü işleme kullanan bir örnek uygulamayı adım adım. Custom Vision uçtan uca uygulama sunmak için diğer bileşenlerle nasıl kullanıldığını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pafarley
ms.openlocfilehash: 520e1981544f6ad0a6f1412a47dc8ea50fdd53a5
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773164"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Öğretici: Azure hizmeti logo kameradan resim tanıma

Bu öğreticide, daha büyük bir senaryonun parçası olarak Azure özel görüntü işleme kullanan bir örnek uygulamayı adım adım. Yapay ZEKA Visual sağlama uygulaması, mobil platformlar için Xamarin.Forms uygulaması, Azure hizmet logoları kamera resimlerini analiz eder ve sonra kullanıcının Azure hesabı için gerçek Hizmetleri dağıtır. Burada, özel görüntü işleme diğer bileşenleri ile koordinasyon halinde kullanışlı bir uçtan uca uygulama sunun için nasıl kullandığını öğreneceksiniz. Tüm uygulama için kendi başınıza çalıştırmak isteyebilirsiniz veya yalnızca özel görüntü işleme kurulumun bir parçası tamamlayın ve nasıl uygulama kullanacağını keşfedin.

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:

> [!div class="checklist"]
> - Azure hizmet logoları tanımak için özel nesne algılayıcısı oluşturma
> - Azure görüntü işleme ve özel görüntü işleme için uygulamanızı bağlayın
> - Azure uygulama Hizmetleri'nden dağıtmak için bir Azure hizmet sorumlusu hesabı oluşturun

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Visual Studio için Xamarin iş yükü (bkz [yükleme Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- Bir iOS veya Android öykünücüsü Visual Studio için
- [Azure komut satırı arabirimi (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (isteğe bağlı)

## <a name="get-the-source-code"></a>Kaynak kodu alma

Sağlanan web uygulamasını kullanmak istiyorsanız, kopyalama veya uygulamanın kaynak kodunu indirin [AI Visual sağlama](https://github.com/Microsoft/AIVisualProvision) github deposu. Açık *Source/VisualProvision.sln* dosyasını Visual Studio'da. Daha sonra gerekli düzenlemeleri bazı proje dosyaları uygulamayı çalıştırmak için yapacağınız.

## <a name="create-an-object-detector"></a>Bir nesne algılayıcısı oluşturma

Oturum [Custom Vision Web sitesi](https://customvision.ai/) ve yeni bir proje oluşturun. Bir nesne algılama projesi belirtin ve Logo etki alanını kullan; Bu logo algılama için en iyi duruma getirilmiş bir algoritma kullanmak için hizmet sağlar. 

![Chrome tarayıcıda Custom Vision Web sitesindeki yeni proje iletişim kutusu penceresi](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Ardından, Azure hizmet logoları görüntüleri karşıya yükleme ve bunları el ile etiketleme logosu algılama algoritması eğitmek gerekir. AIVisualProvision depo kullanabileceğiniz eğitim görüntü kümesi içerir. Web sitesinde seçin **görüntüleri ekleme** düğmesine **eğitim resmi** sekmesine ve ardından gidin **belgeleri/resimler/Training_DataSet** deposunun klasör. El ile her bir resim, logolar etiketlemek ihtiyacınız olacak şekilde bu projeyi yalnızca sınıyorsanız, yalnızca bir alt kümesini görüntüleri karşıya yüklemek istediğiniz. En az 15 örneklerini kullanmayı planladığınız her bir etiketin yüklemeniz gerekir.

Eğitim resmi yükledikten sonra ilk bir görüntü seçin. Bu etiketleme penceresi açılır. Kutularında çizme ve her logo her görüntü için etiketler atayabilirsiniz. 

![Custom Vision Web sitesinde uygulanan etiketlere sahip logoları görüntüsü](media/azure-logo-tutorial/tag-logos.png)

Uygulama, belirli bir etiketi dizeleri ile çalışmak üzere yapılandırıldı; tanımlarında bkz *Source\VisualProvision\Services\Recognition\RecognitionService.cs* dosyası:

[!code-csharp[tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?range=18-33)]

Görüntü etiketlediğinize göre bir etiket için sağa gidin. İşiniz bittiğinde etiketleme penceresi dışında çıkın.

## <a name="train-the-object-detector"></a>Nesne algılayıcısı eğitin

Sol bölmede ayarlamak **etiketleri** geçin **etiketli**, tüm görüntülerinizin görmeniz gerekir. Sonra modeli eğitmek için sayfanın üst kısmındaki yeşil düğmeyi tıklayın. Bu yeni görüntüleri aynı etiketleri tanımak için algoritma sağlanır. Ayrıca bazı doğruluğu puanları oluşturmak için var olan görüntülerinizin modeli test eder.

![Custom Vision eğitim resmi sekmesinde Web; Train düğme vurgulanana](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Tahmin URL'sini alma

Modelinizi eğitildi sonra uygulamanızla tümleştirmek hazır olursunuz. Bunu yapmak için uç nokta URL'si (uygulama sorgular modelinizin adresi) ve tahmin anahtarı (tahmin isteği için uygulama erişimi vermek için) almanız gerekir. İçinde **performans** sekmesinde **tahmin URL** sayfanın üstünde düğme.

![Custom Vision Web sitesi bir Prediction API'deki ekranlı gösteren bir URL adresi ve API anahtarı](media/azure-logo-tutorial/cusvis-endpoint.png)

Görüntü dosyası URL'yi kopyalayın ve `Prediction-key` uygun alanlara değer *Source\VisualProvision\AppSettings.cs* dosyası:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=22-26)]

## <a name="examine-custom-vision-usage"></a>Custom Vision kullanımını inceleyin

Açık *Source/VisualProvision/Services/Recognition/CustomVisionService.cs* Custom Vision anahtarını ve uç nokta URL'nizi uygulamada kullanılan nasıl görmek için dosya. **PredictImageContentsAsync** yöntemi (için zaman uyumsuz görev management) belirteci iptal birlikte bir görüntü dosyasının bayt akışı alır, Custom Vision tahmin API çağırır ve tahmin sonuçlarını döndürür. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?range=12-28)]

Bu sonuç biçimi alır bir **PredictionResult** örneği, kendisi listesini içeren **tahmin** örnekleri. A **tahmin** algılanan bir etiket ve sınırlayıcı kutusunun konumuna görüntü içerir.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?range=3-12)]

Uygulama bu verileri nasıl işlediği hakkında daha fazla bilgi edinmek istiyorsanız, başlangıç **GetResourcesAsync** tanımlanmış yöntemini *Source/VisualProvision/Services/Recognition/RecognitionService.cs* dosya. 

## <a name="add-computer-vision"></a>Görüntü işleme ekleme

Öğreticiyi Custom Vision kısmı tamamlandı, ancak uygulamayı çalıştırmak istiyorsanız, görüntü işleme hizmeti de tümleştirmek için gerekir. Uygulama logosu Algılama işlemini tamamlamak için görüntü işleme'nın metin tanıma özelliği kullanır; bir Azure logosu görünümünü tarafından tanınacak _veya_ yanında yazdırılan metin. Özel işleme modelleri, görüntü işleme görüntü veya video üzerinde belirli işlemlerin gerçekleştirilmesi için önceden eğitildi.

Yalnızca bir anahtarı ve uç nokta URL'si almak için görüntü işleme hizmeti için abone olun. Bkz: [Abonelik anahtarları edinme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe) bu adımla ilgili yardıma ihtiyacınız varsa.

![Görüntü işleme hizmeti Azure portalında, seçili Hızlı Başlat menüsü API uç nokta URL'si olarak bir bağlantı anahtarları için vurgulanan](media/azure-logo-tutorial/comvis-keys.png)

Ardından, açın *Source\VisualProvision\AppSettings.cs* dosya ve doldurma `ComputerVisionEndpoint` ve `ComputerVisionKey` doğru değerlere sahip değişkenler.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=28-32)]


## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Uygulama Hizmetleri Azure aboneliğinize dağıtmak için bir Azure hizmet sorumlusu hesabı gerektirir. Bir hizmet sorumlusu, rol tabanlı erişim denetimi kullanarak uygulama için belirli izinleri için temsilci seçme sağlar. Bkz: [hizmet sorumluları Kılavuzu](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals) daha fazla bilgi edinmek istiyorsanız.

Azure Cloud Shell veya Azure CLI (gibi) kullanarak bir hizmet sorumlusu oluşturabilirsiniz. İlk olarak oturum açın ve kullanmak istediğiniz aboneliği seçin.

```console
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Ardından, hizmet sorumlusu (tamamlanması biraz zaman alabilir unutmayın) oluşturun.

```console
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

İşlemin başarıyla tamamlanmasından sonra gerekli kimlik bilgilerini içeren çıkış aşağıdaki JSON görmeniz gerekir.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```
Not `clientId`, ve `tenantId` değerleri. Uygun alanları eklemek *Source\VisualProvision\AppSettings.cs* dosya.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=8-16)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bu noktada, uygulama erişimini verdiniz:
* eğitilen bir özel görüntü işleme modeli
* Görüntü işleme hizmeti
* bir hizmet sorumlusu hesabı 

Uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Çözüm Gezgini'nde VisualProvision.Android veya VisualProvision.iOS projeyi seçin ve karşılık gelen bir öykünücü veya bağlı bir mobil aygıt ana araç çubuğundaki açılır menüye seçin (çalıştırmak için bir MacOS cihazı gerektiğini unutmayın bir iOS öykünücü). Ardından, uygulamayı çalıştırın.

1. Yüklenen ilk ekranda, hizmet sorumlusu istemci kimliği, Kiracı kimliği ve parola girin. Tıklayın **oturum açma** düğmesi.

    > [!NOTE]
    > Bazı öykünücülerde **oturum açma** düğmeyi Bu adımda değil etkinleştirin. Böyle bir durumda, uygulamayı durdurun açın _Source/VisualProvision/Pages/LoginPage.xaml_ dosya, bulma `Button` öğesi, oturum açma düğmesi olarak etiketlenen ve satırı kaldırın:
      ```xaml
      IsEnabled="{Binding IsValid}"
      ```
    
    > Ardından, uygulamayı yeniden çalıştırın.

    ![alanlar için hizmet sorumlusu kimlik bilgileri uygulama ekranı](media/azure-logo-tutorial/app-credentials.png)

1. Sonraki ekranda, Azure aboneliğinizi (Bu, hizmet sorumlusu erişimi olan abonelikleri tümünün içermelidir) bulunan açılan menüden seçin. Tıklayın **devam** düğmesi.

    ![Hedef Azure aboneliğinin bir açılan alan uygulama ekranı](media/azure-logo-tutorial/app-az-subscription.png)

    Bu noktada uygulama, kamera ve fotoğraf depolama cihazlara erişim vermek isteyebilir. Bu izinleri kabul etmiş olursunuz.

1. Ardından, cihaz kameranızı etkinleştirir. Bir Azure hizmeti logoları, eğitilmiş bir fotoğraf yararlanın. Bir dağıtım iletişim kutusu (Azure portalından dağıtımı yaptığınız gibi) yeni hizmetleri için bir bölge ve kaynak grubu seçin ister. 

    ![smartphone kamera ekranla iki kağıt kesmeler Görünümü'nde Azure logoları](media/azure-logo-tutorial/app-camera-capture.png)

    ![alanlar dağıtım bölge ve kaynak grubu girişi için bir uygulama ekranı](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Tüm bu senaryonun adımları izleyen ve uygulama hesabınıza Azure hizmetlerini dağıtmak için kullanılan gitmek mutlaka [Azure portalında](https://ms.portal.azure.com/) bittiğinde, ne zaman ve yok kullanmak istediğiniz hizmetleri iptal.

Ayrıca, kendi nesne algılama proje özel görüntü ile gelecekte oluşturmayı planlıyorsanız, bu öğreticide oluşturduğunuz logosu algılama projesini silmek isteyebilirsiniz. Ücretsiz deneme için özel görüntü işleme, iki proje için sağlar. Üzerinde [Custom Vision Web sitesi](https://customvision.ai), gitmek **projeleri** çöp kutusu altında seçin **Yeni Projem**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ayarlayın ve logoları mobil kamera görüntülerde algılamak için özel görüntü işleme hizmeti kullanan tam özellikli bir Xamarin.Forms uygulaması incelediniz. Ardından, böylece kendi uygulamasını oluşturduğunuzda, güçlü ve doğru yapabileceğiniz bir özel görüntü işleme modeli oluşturmaya yönelik en iyi uygulamaları öğrenin.

> [!div class="nextstepaction"]
> [Nasıl sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md)