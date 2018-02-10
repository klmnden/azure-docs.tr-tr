---
title: "Var olan bir Tahmine dayalı web hizmetini yeniden eğitme | Microsoft Docs"
description: "Bir model yeniden eğitme ve Azure Machine Learning ile yeni eğitilen modelini kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: raymondl
ms.openlocfilehash: 7b59dd1cd43bb95e52e271a44e321810fa6867cc
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="retrain-an-existing-predictive-web-service"></a>Var olan bir Tahmine dayalı web hizmetini yeniden eğitme
Bu belge aşağıdaki senaryoyu yeniden eğitme işlemini açıklar:

* Eğitim denemenizi ve kullanıma hazır hale getirilmiş web hizmeti olarak dağıttığınız bir tahmini deneme var.
* Yeni veriniz varsa, Puanlama gerçekleştirmek için kullanılacak Tahmine dayalı web hizmetiniz istiyor.

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

Denemeler ve varolan web hizmeti ile başlayarak, aşağıdaki adımları izlemeniz gerekir:

1. Modeli güncelleştirin.
   1. Web hizmeti girişleri ve çıkışları için izin vermek için eğitim denemenizi değiştirin.
   2. Eğitim denemenizi yeniden eğitme bir web hizmeti olarak dağıtın.
   3. Yeniden eğitme modeli için eğitim deneme toplu yürütme hizmeti (BES) kullanın.
2. Tahmine dayalı denemeye güncelleştirmek için Azure Machine Learning PowerShell cmdlet'lerini kullanın.
   1. Azure Resource Manager hesabınızda oturum açın.
   2. Web hizmeti tanımının alın.
   3. Web hizmeti tanımının JSON olarak dışarı aktarın.
   4. JSON ilearner blob'una referansı güncelleştirin.
   5. JSON web hizmeti tanımının alın.
   6. Web hizmeti ile yeni bir web hizmeti tanımının güncelleştirin.

## <a name="deploy-the-training-experiment"></a>Eğitim denemenizi dağıtma
Eğitim denemenizi yeniden eğitme web hizmeti olarak dağıtmak için web hizmeti girişleri ve çıkışları modele eklemeniz gerekir. Bağlanarak bir *Web hizmeti çıkış* deneme modülüne  *[Train Model] [ train-model]*  modülü, Tahmine dayalı denemenizde kullanabileceğiniz yeni bir eğitilen modeli oluşturmak eğitim denemenizi etkinleştirin. Varsa bir *Evaluate Model* modülü, çıktı olarak değerlendirme sonuçları almak için web hizmeti çıkış de iliştirebilirsiniz.

Eğitim denemenizi güncelleştirmek için:

1. Bağlantı bir *Web hizmeti giriş* modülü, veri girişi için (örneğin, bir *Clean Missing Data* Modülü). Tipik olarak, giriş verilerinizi özgün eğitim verilerinizi aynı şekilde işlendiğinden emin olmak istersiniz.
2. Bağlantı bir *Web hizmeti çıkış* çıktısını modülüne, *Train Model* modülü.
3. Varsa bir *Evaluate Model* modülü ve istiyorsanız değerlendirme sonuçlarını çıkarmak, bağlanmak bir *Web hizmeti çıkış* çıktısını modülüne, *Evaluate Model* modülü.

Denemenizi çalıştırın.

Ardından, eğitim denemenizi eğitilen bir modelin ve model değerlendirme sonuçlarını üreten bir web hizmeti olarak dağıtmanız gerekir.  

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı**ve ardından **Web hizmeti dağıtma [Yeni]**. Azure Machine Learning Web Hizmetleri portalı açılarak **Web hizmeti Dağıt** sayfası. Web hizmetiniz için bir ad yazın, ödeme planı seçin ve ardından **dağıtma**. Eğitilmiş modeller oluşturmak için yalnızca toplu iş yürütme yöntemi kullanabilirsiniz.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Yeni veri modeliyle BES kullanarak yeniden eğitme
Bu örnekte, biz C# yeniden eğitme uygulaması oluşturmak için kullanıyorsunuz. Python veya R örnek kod, bu görevi gerçekleştirmek için de kullanabilirsiniz.

Yeniden eğitme API'lerini çağırmak için:

1. Visual Studio'da bir C# konsol uygulaması oluşturun: **yeni** > **proje** > **Visual C#** > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**.
2. Machine Learning Web Hizmetleri portalında oturum açın.
3. Üzerinde çalıştığınız web hizmeti tıklatın.
4. Tıklatın **tüketen**.
5. Ekranın alt kısmındaki **Tüket** sayfasında **örnek kod** 'yi tıklatın **toplu**.
6. Toplu iş yürütme için örnek C# kodu kopyalayın ve Program.cs dosyasına yapıştırın. Ad alanı olduğu gibi kalır emin olun.

Açıklamaları belirtildiği gibi Microsoft.AspNet.WebApi.Client NuGet paketini ekleyin. Microsoft.WindowsAzure.Storage.dll başvuru eklemek için önce yüklemeniz gerekebilir [Azure depolama hizmetleri için İstemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage).

Aşağıdaki ekran görüntüsü gösterildiği **Tüket** Azure Machine Learning Web Hizmetleri portalında sayfası.

![Sayfa kullanma][1]

### <a name="update-the-apikey-declaration"></a>Apikey ile yapılan bildirimini güncelleştirin
Bulun **apikey ile yapılan** bildirimi:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

İçinde **temel tüketim bilgileri** bölümünü **Tüket** sayfasında, birincil anahtarını bulun ve kopyalayın **apikey ile yapılan** bildirimi.

### <a name="update-the-azure-storage-information"></a>Azure depolama bilgilerini güncelleştir
BES örnek kod, yerel bir sürücüden (örneğin, "C:\temp\CensusIpnput.csv") bir dosyayı Azure Storage'a yükler, işler ve sonuçları Azure depolama birimine geri yazar.  

Denemenizi çalıştırdıktan sonra sonuçta elde edilen iş akışı aşağıdakine benzer olmalıdır:

![Çalıştırdıktan sonra elde edilen iş akışı][4]

1. Azure Portal’da oturum açın.
2. Sol gezinti sütununda tıklatın **daha fazla hizmet**, arama **depolama hesapları**ve seçin.
3. Depolama hesapları listesinden bir retrained modelini depolamak için seçin.
4. Sol gezinti sütununda tıklatın **erişim anahtarları**.
5. Kopyalayıp kaydedin **birincil erişim anahtarını**.
6. Sol gezinti sütununda tıklatın **kapsayıcıları**.
7. Var olan bir kapsayıcıyı seçin veya yeni bir tane oluşturun ve ad kaydedin.

Bulun *StorageAccountName*, *StorageAccountKey*, ve *StorageContainerName* bildirimlerini ve portaldan kaydettiğiniz değerlerini güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ayrıca, girdi dosyası kodda belirttiğiniz konumda kullanılabilir olduğundan emin olmalısınız.

### <a name="specify-the-output-location"></a>Çıkış konumu belirtin
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

Yeniden eğitme çıktısı örneği verilmiştir:

![Çıktı yeniden eğitme][6]

## <a name="evaluate-the-retraining-results"></a>Yeniden eğitme sonuçları değerlendirin
Uygulamayı çalıştırdığınızda, çıktı URL ve değerlendirme sonuçlarını erişmek için gerekli olan paylaşılan erişim imzaları belirteci içerir.

Birleştirerek retrained modeli performans sonuçlarını görebilirsiniz *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* için çıktı sonuçlarından *output2* (yukarıdaki yeniden eğitme çıkış görüntüde gösterildiği gibi) ve tam URL'sini tarayıcınızın adres çubuğuna yapıştırma.  

Yeni eğitilen model yeterince iyi var olan dosyayla gerçekleştirip gerçekleştirmeyeceğini belirlemek için sonuçlarını inceleyin.

Kopya *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* çıkış sonuçlarından.

## <a name="retrain-the-web-service"></a>Web hizmeti yeniden eğitme
Yeni bir web hizmeti yeniden eğitme, yeni eğitilen model başvurmak için Tahmine dayalı web hizmeti tanımının güncelleştirin. Web hizmeti tanımının bir iç web hizmetinin eğitilen modeli gösterimini ve doğrudan değiştirilebilir değil. Tahmine dayalı denemenizi ve değil eğitim denemenizi için web hizmeti tanımının alırsınız emin olun.

## <a name="sign-in-to-azure-resource-manager"></a>Azure kaynak yöneticisi için oturum açın
Önce Azure hesabınıza PowerShell ortamında kullanarak kaydolmalısınız [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet'i.

## <a name="get-the-web-service-definition-object"></a>Web hizmeti tanımının nesnesini alın
Ardından, Web hizmeti tanımının nesne çağırarak alma [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet'i.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Var olan bir web hizmetini kaynak grubu adını belirlemek için web Hizmetleri, aboneliğinizde görüntülemek için herhangi bir parametre olmadan Get-AzureRmMlWebService cmdlet'ini çalıştırın. Web hizmeti bulun ve ardından, web hizmeti kimliğini arayın Kaynak grubunun adını dördüncü kimliği hemen sonra öğedir *resourceGroups* öğesi. Aşağıdaki örnekte, kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, var olan bir web hizmetini kaynak grubu adını belirlemek için Azure Machine Learning Web Hizmetleri portalında oturum açın. Web hizmeti seçin. Kaynak grubu adı beşinci web hizmetinin URL'sini hemen sonra öğesidir *resourceGroups* öğesi. Aşağıdaki örnekte, kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Web hizmeti tanımının nesneyi JSON olarak dışarı aktarma
Yeni eğitilen model kullanılacak eğitilen model tanımını değiştirmek için önce kullanmanız gerekir [verme AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) JSON biçiminde bir dosyaya dışarı aktarmak için cmdlet.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Güncelleştirme ilearner blob referansı
[Eğitilen model] varlıkları bulun, güncelleştirme *URI* değeri *locationInfo* ilearner blob URI'si düğümle. URI birleştirerek oluşturulan *BaseLocation* ve *RelativeLocation* çağrısı yeniden eğitme BES çıktısından.

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

## <a name="import-the-json-into-a-web-service-definition-object"></a>JSON bir Web hizmeti tanımının nesnesine aktarın
Kullanmalısınız [alma AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) değiştirilen JSON dosyasını predicative deneme güncelleştirmek için kullanabileceğiniz geri bir Web hizmeti tanımının nesnesine dönüştürmek için cmdlet.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Web hizmeti güncelleştir
Son olarak, [güncelleştirme AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) Tahmine dayalı denemeye güncelleştirmek için cmdlet.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
