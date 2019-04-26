---
title: Yeniden eğitme ve bir web hizmetini dağıtma
titleSuffix: Azure Machine Learning Studio
description: Yeni eğitilen makine öğrenme modeli Azure Machine Learning Studio'da kullanmak için bir web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/14/2019
ms.openlocfilehash: 903f2700ad127c9bcc69e69ee125ba62fccf52e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60196443"
---
# <a name="retrain-and-deploy-a-machine-learning-model"></a>Yeniden eğitme ve makine öğrenme modeli dağıtma

Yeniden eğitme doğru ve bağlı kullanılabilir en alakalı verileri olarak makine öğrenimi modelleri kalmasını sağlamak için bir yoludur. Bu makalede, yeniden eğitme ve bir machine learning Studio'da yeni bir web hizmeti olarak modeli dağıtma gösterilmektedir. Bir Klasik web hizmetini yeniden eğitme arıyorsanız [bu nasıl yapılır makalesine bakın.](retrain-classic-web-service.md)

Bu makalede, özel olarak dağıtılan bir Tahmine dayalı web hizmeti zaten sahip olduğunuzu varsayar. Tahmine dayalı web hizmeti, zaten yoksa [bir Studio web hizmeti dağıtma hakkında bilgi edinin.](publish-a-machine-learning-web-service.md)

Yeniden eğitme ve bir machine learning yeni web hizmeti dağıtmak için aşağıdaki adımları:

1. Dağıtım bir **web hizmetini yeniden eğitme**
1. Kullanarak yeni bir modeli eğitmek, **web hizmetini yeniden eğitme**
1. Mevcut güncelleştirme **Tahmine dayalı denemeye** yeni modeli kullanmak için

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="deploy-the-retraining-web-service"></a>Yeniden eğitme web hizmetini dağıtma

Yeniden eğitme bir web hizmeti parametreleri gibi yeni veri kümesi modelinizi yeniden eğitme ve sonrası için Kaydet olanak sağlar. Bağladığınızda bir **Web hizmeti çıkış** için bir **modeli eğitme**, eğitim denemesini kullanabilmeniz için yeni bir modeli çıkarır.

Yeniden eğitme bir web hizmetini dağıtmak için aşağıdaki adımları kullanın:

1. Bağlama bir **Web hizmeti giriş** modülü, veri girişi için. Genellikle, girişinizi özgün eğitim verilerinizi aynı şekilde işlendiğinden emin olmak istersiniz.
1. Bağlama bir **Web hizmeti çıkış** çıktısına modülü, **modeli eğitme**.
1. Varsa bir **Evaluate Model** modülü bağlanabilir bir **Web hizmeti çıkış** değerlendirme sonuçlarını çıkarmak için Modülü
1. Denemenizi çalıştırın.

    Denemenizi çalıştırdıktan sonra elde edilen iş akışı aşağıdaki görüntüye benzer olmalıdır:

    ![Sonuçta elde edilen iş akışı](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE04.png)

    Şimdi, eğitim denemesini eğitilen bir modelin ve model değerlendirme sonuçlarını veren bir yeniden eğitme web hizmeti olarak dağıtalım.

1. Deneme tuvalinin altındaki tıklatın **Web hizmeti ayarlama**
1. Seçin **[Yeni] Web hizmetini dağıtma**. Azure Machine Learning Web Hizmetleri portalı açılır **Web hizmeti Dağıt** sayfası.
1. Web hizmetiniz için bir ad yazın ve ödeme planı seçin.
1. **Dağıt**'ı seçin.

## <a name="retrain-the-model"></a>Modeli yeniden eğitme

Bu örnekte, C# yeniden eğitme uygulama oluşturmak için kullanıyoruz. Python veya R örnek kod, bu görevi gerçekleştirmek için de kullanabilirsiniz.

Yeniden eğitme API'lerini çağırmak için aşağıdaki adımları kullanın:

1. Oluşturma bir C# konsol uygulaması Visual Studio'da: **Yeni** > **proje** > **Visual C#**   >  **Windows Klasik Masaüstü**  >   **Konsol uygulaması (.NET Framework)**.
1. Machine Learning Web Hizmetleri portalında oturum açın.
1. Üzerinde çalıştığınız web hizmeti.
1. Tıklayın **tüketen**.
1. Sayfanın alt kısmında **Tüket** sayfasında **örnek kodu** bölümünde **Batch**.
1. Toplu iş yürütme için örnek C# kodu kopyalayın ve Program.cs dosyasına yapıştırın. Ad alanı değişmeden kalmasını sağlayın.

Yorumlar bölümünde belirtildiği gibi System.NET.http.Formatting, NuGet paketini ekleyin. Microsoft.WindowsAzure.Storage.dll başvuru eklemek için yüklemeniz gerekebilir [Azure depolama hizmetleri için İstemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage).

Aşağıdaki ekran görüntüsü gösterildiği **Tüket** Azure Machine Learning Web Hizmetleri portalında sayfası.

![Sayfa kullanma](media/retrain-machine-learning/machine-learning-retrain-models-consume-page.png)

### <a name="update-the-apikey-declaration"></a>Apikey bildirimini güncelleştirin

Bulun **apikey** bildirimi:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

İçinde **temel tüketim bilgileri** bölümünü **Tüket** sayfasında birincil anahtarını bulun ve kopyalayıp **apikey** bildirimi.

### <a name="update-the-azure-storage-information"></a>Azure depolama bilgilerini güncelleştir

BES örnek kodu (örneğin, "C:\temp\CensusInput.csv") yerel sürücüden bir dosyayı Azure Storage'a yükler, işler ve sonuçları, Azure Depolama'ya geri yazar.

1. Azure portalda oturum açma
1. Sol gezinti sütununda **diğer hizmetler**, arama **depolama hesapları**ve bu seçeneği belirleyin.
1. Depolama hesapları listesinden bir retrained modelini depolamak için seçin.
1. Sol gezinti sütununda **erişim anahtarları**.
1. Kopyalayıp kaydedin **birincil erişim anahtarı**.
1. Sol gezinti sütununda **kapsayıcıları**.
1. Var olan bir kapsayıcıyı seçin veya yeni bir tane oluşturun ve adını kaydedin.

Bulun *StorageAccountName*, *StorageAccountKey*, ve *StorageContainerName* bildirimleri ve Portalı'ndan kaydedilmiş değerleri güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Giriş dosyası, kod içinde belirttiğiniz konumda kullanılabilir olduğunu da emin olmanız gerekir.

### <a name="specify-the-output-location"></a>Çıkış konumunu belirtme

İstek yükünde belirtilen dosya uzantısını çıkış konumu belirttiğinizde *RelativeLocation* olarak belirtilmelidir `ilearner`.

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Yeniden eğitme çıktının bir örneği aşağıda verilmiştir:

![Çıktı yeniden eğitme](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE06.png)

### <a name="evaluate-the-retraining-results"></a>Yeniden eğitme sonuçları değerlendirin

Uygulamayı çalıştırdığınızda, çıktı URL'yi ve değerlendirme sonuçlarını erişmek gerekli olan paylaşılan erişim imzaları belirteci içerir.

Performans sonuçlarını retrained modelinin birleştirerek gördüğünüz *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* içinçıkışsonuçlarından*output2* ve tam URL'sini tarayıcınızın adres çubuğuna yapıştırma.

Yeni eğitim modeli, var olan bir daha iyi sonuç verdiğini belirlemek için sonuçları inceleyin.

Kaydet *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* çıkış sonuçlarından.

## <a name="update-the-predictive-experiment"></a>Tahmine dayalı denemeye güncelleştir

### <a name="sign-in-to-azure-resource-manager"></a>Azure Resource Manager'a oturum açın

İlk olarak, Azure hesabınıza PowerShell ortamında kullanarak oturum açın [Connect AzAccount](/powershell/module/az.profile/connect-azaccount) cmdlet'i.

### <a name="get-the-web-service-definition-object"></a>Web hizmet tanımı nesnesini alın

Ardından, Web hizmeti tanımı nesnesi çağırarak alın [Get-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservice) cmdlet'i.

    $wsd = Get-AzMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Mevcut bir web hizmetini kaynak grubu adını belirlemek için aboneliğinizde web hizmetleri görüntülemek için herhangi bir parametre olmadan Get-AzMlWebService cmdlet'ini çalıştırın. Web hizmeti bulun ve ardından, web hizmeti kimliğini arayın Kaynak grubu adını dördüncü kimliği hemen sonra öğedir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, mevcut bir web hizmetini kaynak grubu adını belirlemek için Azure Machine Learning Web Hizmetleri portalında oturum açın. Web hizmeti seçin. Kaynak grubu adı beşinci web hizmetinin URL'sini hemen sonra öğesidir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237

### <a name="export-the-web-service-definition-object-as-json"></a>Web servis tanımı nesnesi, JSON olarak dışarı aktarma

Yeni eğitilen modelini kullanmak için eğitilen model tanımı değiştirmek için önce kullanmalısınız [dışarı aktarma AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/export-azmlwebservice) JSON biçiminde bir dosyaya vermek için cmdlet'i.

    Export-AzMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

### <a name="update-the-reference-to-the-ilearner-blob"></a>Güncelleştirme ilearner blob başvurusu

[Eğitilen model] varlıkları bulun, güncelleştirme *URI* değerini *locationInfo* düğümle ilearner blob URI'si. URI birleştirerek oluşturulur *BaseLocation* ve *RelativeLocation* çağrı yeniden eğitme BES çıktısından.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

### <a name="import-the-json-into-a-web-service-definition-object"></a>JSON bir Web hizmeti tanımının nesnesine içe aktarın.

Kullanma [alma AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/import-azmlwebservice) değiştirilmiş bir JSON dosyası predicative denemeyi güncelleştirmeniz kullanabileceğiniz geri bir Web hizmeti tanımının nesnesine dönüştürmek için cmdlet'i.

    $wsd = Import-AzMlWebService -InputFile "C:\temp\mlservice_export.json"

### <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek

Son olarak, [güncelleştirme AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/update-azmlwebservice) Tahmine dayalı denemeye güncelleştirmek için cmdlet.

    Update-AzMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini yönetme veya birden çok denemeleri çalıştırması izlemek hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Web Hizmetleri portalını keşfedin](manage-new-webservice.md)
* [Deneme yinelemelerini yönetme](manage-experiment-iterations.md)
