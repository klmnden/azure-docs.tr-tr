---
title: Mevcut bir Tahmine dayalı web hizmetini yeniden eğitme | Microsoft Docs
description: Modeli yeniden eğitme ve Azure Machine Learning'de yeni eğitim modeli kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.custom: (previous ms.author yahajiza)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.openlocfilehash: 2a259f1f8a82c3bd54fd7ba821eb096a85659e3f
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821089"
---
# <a name="retrain-an-existing-predictive-web-service"></a>Mevcut bir Tahmine dayalı web hizmetini yeniden eğitme
Bu belge aşağıdaki senaryoyu yeniden eğitme anlatmaktadır:

* Eğitim denemesini ve bir çalışır hale getirilen web hizmeti olarak dağıttığınız bir Tahmine dayalı denemeye var.
* Yeni verilere sahip olduğunuz kendi puanlamaların gerçekleştirilmesinin anlatıldığı üzere kullanmak için Tahmine dayalı web hizmetinizi istiyor.

> [!NOTE]
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md).

Denemeler ve web hizmetiniz ile başlayarak, şu adımları izlemesi gerekir:

1. Modeli güncelleştirin.
   1. Eğitim denemenizi Web hizmeti giriş ve çıkışları için izin verecek şekilde değiştirin.
   2. Eğitim denemesini yeniden eğitme web hizmeti olarak dağıtın.
   3. Modeli yeniden eğitme için eğitim denemesini'nın toplu yürütme hizmeti (BES) kullanın.
2. Tahmine dayalı denemeye güncelleştirmek için Azure Machine Learning PowerShell cmdlet'lerini kullanın.
   1. Azure Resource Manager hesabınızda oturum açın.
   2. Web hizmet tanımı Al
   3. Web hizmet tanımı JSON olarak verin.
   4. JSON ilearner blob başvurusunu güncelleştirin.
   5. JSON web hizmet tanımı aktarın.
   6. Web hizmeti ile yeni bir web hizmeti tanımı güncelleştirin.

## <a name="deploy-the-training-experiment"></a>Eğitim denemesini dağıtma
Eğitim denemesini yeniden eğitme bir web hizmeti olarak dağıtmak için web hizmeti giriş ve çıkışları modele eklemeniz gerekir. Bağlayarak bir *Web hizmeti çıkış* modülünü deneme 's *[modeli eğitme][train-model]* modülü için eğitim denemesini etkinleştirin Tahmine dayalı denemenizde kullanabileceğiniz yeni bir eğitilen modeli oluşturur. Varsa bir *Evaluate Model* modülü, çıktı olarak değerlendirme sonuçlarını almak için web hizmeti çıkış ayrıca iliştirebilirsiniz.

Eğitim denemenizi güncelleştirmek için:

1. Bağlama bir *Web hizmeti giriş* modülü, veri girişi için (örneğin, bir *eksik verileri temizleme* Modülü). Genellikle, girişinizi özgün eğitim verilerinizi aynı şekilde işlendiğinden emin olmak istersiniz.
2. Bağlama bir *Web hizmeti çıkış* çıktısına modülü, *modeli eğitme* modülü.
3. Varsa bir *Evaluate Model* modülü ve istiyorsanız değerlendirme sonuçlarını çıkarmak, bağlanmak bir *Web hizmeti çıkış* çıktısına modülü, *Evaluate Model* modülü.

Denemenizi çalıştırın.

Ardından, eğitim denemesini eğitilen bir modelin ve model değerlendirme sonuçları üreten bir web hizmeti olarak dağıtmanız gerekir.

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı**ve ardından **Web hizmeti dağıtma [Yeni]**. Azure Machine Learning Web Hizmetleri portalı açılır **Web hizmeti Dağıt** sayfası. Web hizmetiniz için bir ad yazın, ödeme planı seçin ve ardından **Dağıt**. Eğitilen modelleri oluşturmak için yalnızca toplu yürütme yöntemi kullanabilirsiniz.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>BES kullanarak yeni bir veri modeli yeniden eğitme
Bu örnekte, C# yeniden eğitme uygulama oluşturmak için kullanıyoruz. Python veya R örnek kod, bu görevi gerçekleştirmek için de kullanabilirsiniz.

Yeniden eğitme API'lerini çağırmak için:

1. Visual Studio'da C# konsol uygulaması oluşturun: **yeni** > **proje** > **Visual C#** > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**.
2. Machine Learning Web Hizmetleri portalında oturum açın.
3. Üzerinde çalıştığınız web hizmeti.
4. Tıklayın **tüketen**.
5. Sayfanın alt kısmında **Tüket** sayfasında **örnek kodu** bölümünde **Batch**.
6. Toplu iş yürütme için örnek C# kodu kopyalayın ve Program.cs dosyasına yapıştırın. Ad alanı değişmeden kalmasını sağlayın.

Yorumlar bölümünde belirtildiği gibi System.NET.http.Formatting, NuGet paketini ekleyin. Microsoft.WindowsAzure.Storage.dll başvuru eklemek için önce yüklemeniz gerekebilir [Azure depolama hizmetleri için İstemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage).

Aşağıdaki ekran görüntüsü gösterildiği **Tüket** Azure Machine Learning Web Hizmetleri portalında sayfası.

![Sayfa kullanma][1]

### <a name="update-the-apikey-declaration"></a>Apikey bildirimini güncelleştirin
Bulun **apikey** bildirimi:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

İçinde **temel tüketim bilgileri** bölümünü **Tüket** sayfasında birincil anahtarını bulun ve kopyalayıp **apikey** bildirimi.

### <a name="update-the-azure-storage-information"></a>Azure depolama bilgilerini güncelleştir
BES örnek kodu (örneğin, "C:\temp\CensusIpnput.csv") yerel sürücüden bir dosyayı Azure Storage'a yükler, işler ve sonuçları, Azure Depolama'ya geri yazar.

Denemenizi çalıştırdıktan sonra elde edilen iş akışı aşağıdakine benzer olmalıdır:

![Çalıştırdıktan sonra elde edilen iş akışı][4]

1. Azure Portal’da oturum açın.
2. Sol gezinti sütununda **diğer hizmetler**, arama **depolama hesapları**ve bu seçeneği belirleyin.
3. Depolama hesapları listesinden bir retrained modelini depolamak için seçin.
4. Sol gezinti sütununda **erişim anahtarları**.
5. Kopyalayıp kaydedin **birincil erişim anahtarı**.
6. Sol gezinti sütununda **kapsayıcıları**.
7. Var olan bir kapsayıcıyı seçin veya yeni bir tane oluşturun ve adını kaydedin.

Bulun *StorageAccountName*, *StorageAccountKey*, ve *StorageContainerName* bildirimleri ve Portalı'ndan kaydedilmiş değerleri güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Giriş dosyası, kod içinde belirttiğiniz konumda kullanılabilir olduğunu da emin olmanız gerekir.

### <a name="specify-the-output-location"></a>Çıkış konumunu belirtme
İstek yükünde belirtilen dosya uzantısını çıkış konumu belirttiğinizde *RelativeLocation* olarak belirtilmelidir `ilearner`. Aşağıdaki örneğe bakın:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Yeniden eğitme çıktının bir örneği verilmiştir:

![Çıktı yeniden eğitme][6]

## <a name="evaluate-the-retraining-results"></a>Yeniden eğitme sonuçları değerlendirin
Uygulamayı çalıştırdığınızda, çıktı URL'yi ve değerlendirme sonuçlarını erişmek gerekli olan paylaşılan erişim imzaları belirteci içerir.

Performans sonuçlarını retrained modelinin birleştirerek gördüğünüz *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* içinçıkışsonuçlarından*output2* (önceki yeniden eğitme çıkış resimde gösterildiği gibi) ve tam URL'sini tarayıcınızın adres çubuğuna yapıştırma.

Yeni eğitim modeli yeterince iyi var olan dosyayla gerçekleştirip gerçekleştirmediğini belirlemek için sonuçları inceleyin.

Kopyalama *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* çıkış sonuçlarından.

## <a name="retrain-the-web-service"></a>Web hizmetini yeniden eğitme
Yeni bir web hizmetini yeniden eğitme, Tahmine dayalı web hizmeti tanımının yeni eğitim modeli başvuru güncelleştirin. Web hizmet tanımı eğitilmiş modelinin web hizmeti bir iç temsiline ve doğrudan değiştirilebilir değil. Web hizmeti tanımı, Tahmine dayalı denemeye ve, eğitim denemesini almakta olduğunu doğrulayın.

## <a name="sign-in-to-azure-resource-manager"></a>Azure Resource Manager'a oturum açın
İlk kez Azure hesabınızda PowerShell ortamında kullanarak oturum açmalısınız [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet'i.

## <a name="get-the-web-service-definition-object"></a>Web hizmet tanımı nesnesini alın
Ardından, Web hizmeti tanımı nesnesi çağırarak alın [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet'i.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Mevcut bir web hizmetini kaynak grubu adını belirlemek için aboneliğinizde web hizmetleri görüntülemek için herhangi bir parametre olmadan Get-AzureRmMlWebService cmdlet'ini çalıştırın. Web hizmeti bulun ve ardından, web hizmeti kimliğini arayın Kaynak grubu adını dördüncü kimliği hemen sonra öğedir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, mevcut bir web hizmetini kaynak grubu adını belirlemek için Azure Machine Learning Web Hizmetleri portalında oturum açın. Web hizmeti seçin. Kaynak grubu adı beşinci web hizmetinin URL'sini hemen sonra öğesidir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Web servis tanımı nesnesi, JSON olarak dışarı aktarma
Yeni eğitilen modelini kullanmak için eğitilen model tanımı değiştirmek için önce kullanmalısınız [dışarı aktarma AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) JSON biçiminde bir dosyaya vermek için cmdlet'i.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Güncelleştirme ilearner blob başvurusu
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

## <a name="import-the-json-into-a-web-service-definition-object"></a>JSON bir Web hizmeti tanımının nesnesine içe aktarın.
Kullanmalısınız [alma AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) değiştirilmiş bir JSON dosyası predicative denemeyi güncelleştirmeniz kullanabileceğiniz geri bir Web hizmeti tanımının nesnesine dönüştürmek için cmdlet'i.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek
Son olarak, [güncelleştirme AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) Tahmine dayalı denemeye güncelleştirmek için cmdlet.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
