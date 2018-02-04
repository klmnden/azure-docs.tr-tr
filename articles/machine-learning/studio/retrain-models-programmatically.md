---
title: "Machine Learning modellerini program aracılığıyla yeniden eğitme | Microsoft Docs"
description: "Program aracılığıyla bir modeli yeniden eğitme ve Azure Machine Learning ile yeni eğitilen modelini kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye
ms.openlocfilehash: d228021564cdfe5c898c67cce0038b3ec36d014b
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="retrain-machine-learning-models-programmatically"></a>Machine Learning modellerini programlı olarak yeniden eğitme
Bu kılavuzda, bir Azure Machine Learning Web hizmeti C# ve makine öğrenme toplu yürütme hizmeti kullanarak program aracılığıyla yeniden eğitme öğreneceksiniz.

Model retrained sonra aşağıdaki izlenecek Tahmine dayalı web hizmetiniz modelinde güncelleştirmek nasıl göster:

* Machine Learning Web Hizmetleri portalında bir Klasik web hizmeti dağıttıysanız bkz [bir Klasik web hizmeti yeniden eğitme](retrain-a-classic-web-service.md). 
* Yeni bir web hizmetini dağıttıysanız bkz [makine öğrenme yönetimi cmdlet'lerini kullanarak yeni bir web hizmeti yeniden eğitme](retrain-new-web-service-using-powershell.md).

Yeniden eğitme işlemine genel bakış için bkz: [makine öğrenimi modeline yeniden eğitme](retrain-machine-learning-model.md).

Mevcut yeni Azure Resource Manager temelli web hizmetiniz ile başlatmak istiyorsanız, bkz: [var olan bir Tahmine dayalı web hizmetini yeniden eğitme](retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Eğitim denemenizi oluşturma
Bu örnek için kullanacağınız "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi" Microsoft Azure Machine Learning örnekleri gelen. 

Deneme oluşturmak için:

1. Oturum açın Microsoft Azure Machine Learning Studio. 
2. Üzerinde Pano sağ alt köşesindeki, tıklatın **yeni**.
3. Microsoft Samples örnek 5'i seçin.
4. Deneme tuvalinin üstündeki denemeyi yeniden adlandırmak için deneme adını seçin "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi".
5. Tür Census modeli.
6. Deneme tuvalinin altındaki tıklatın **çalıştırmak**.
7. Tıklatın **Ayarla web hizmeti** seçip **web hizmeti yeniden eğitme**. 

İlk deneme gösterir.
   
   ![İlk deneme.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Tahmine dayalı bir deneme oluşturma ve bir web hizmeti olarak Yayımla
Ardından bir Predicative deneme oluşturun.

1. Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti**. Bu model eğitilen Model olarak kaydeder ve web hizmeti giriş ve çıkış modülleri ekler. 
2. **Çalıştır**’a tıklayın. 
3. Denemeyi çalışması bittikten sonra tıklatın **Web hizmeti Dağıt [Klasik]** veya **Web hizmeti dağıtma [Yeni]**.

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Eğitim denemenizi eğitim web hizmeti olarak dağıtma
Eğitim modeli yeniden eğitme Retraining web hizmeti olarak oluşturulan eğitim denemenizi dağıtmanız gerekir. Bu web hizmeti gereken bir *Web hizmeti çıkış* bağlı Modülü  *[Train Model] [ train-model]*  yeni üretmek için modülü, eğitilmiş modeller.

1. Eğitim denemenizi dönmek için sol bölmede denemeler simgesini tıklatın, sonra Census modeli adlı denemeye tıklayın.  
2. Web hizmeti arama deneme öğeleri arama kutusuna yazın. 
3. Sürükleme bir *Web hizmeti girişi* deneme modülü tuvale ve çıktısını için bağlanmak *Clean Missing Data* modülü.  Bu yeniden eğitme verilerinizi özgün eğitim verilerinizi aynı şekilde işlenir sağlar.
4. İki *web hizmeti çıkış* deneme tuvale modüller. Çıkışına bağlayın *Train Model* biri ve çıktısını modülüne *Evaluate Model* diğer modülü. İçin web hizmeti çıktı **Train Model** bize yeni eğitilen modeli sağlar. Çıktı bağlı **Evaluate Model** bu modül animasyonun çıktı, performans sonuçları olduğu döndürür.
5. **Çalıştır**’a tıklayın. 

Sonraki eğitim denemenizi eğitilen bir modelin ve model değerlendirme sonuçlarını üreten bir web hizmeti olarak dağıtmanız gerekir. Bunu başarmak için sonraki eylemler kümesidir, Klasik web hizmeti veya yeni bir web hizmeti ile çalışma hakkında bağımlı.  

**Klasik web hizmeti**

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Web hizmeti Dağıt [Klasik]**. Web hizmeti **Pano** toplu iş yürütme için API anahtarı ve API Yardım sayfası görüntülenir. Yalnızca toplu iş yürütme yöntemi, eğitilmiş modeller oluşturmak için kullanılabilir.

**Yeni web hizmeti**

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Web hizmeti dağıtma [Yeni]**. Web hizmeti Azure Machine Learning Web Hizmetleri Portalı'nı dağıtma web hizmeti sayfası açılır. Web hizmetiniz için bir ad yazın ve ödeme planı seçin ve ardından **dağıtma**. Yalnızca toplu iş yürütme yöntemi eğitilmiş modeller oluşturmak için kullanılabilir.

Deneme çalışması, tamamlandıktan sonra her iki durumda da, sonuçta elde edilen iş akışı şu şekilde görünmelidir:

![Çalıştırdıktan sonra elde edilen iş akışı.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>Model BES kullanarak yeni verilerle yeniden eğitme
Bu örnekte, C# yeniden eğitme uygulaması oluşturmak için kullandığınız. Python veya R örnek kod, bu görevi gerçekleştirmek için de kullanabilirsiniz.

Yeniden eğitme API'lerini çağırmak için:

1. Visual Studio'da bir C# konsol uygulaması oluşturun: **yeni** > **proje** > **Visual C#** > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**.
2. Machine Learning Web hizmeti portalında oturum açın.
3. Klasik web hizmeti ile çalışıyorsanız, tıklatın **Klasik Web Hizmetleri**.
   1. Çalıştığınız web hizmeti tıklatın.
   2. Varsayılan uç noktasına tıklayın.
   3. Tıklatın **tüketen**.
   4. Ekranın alt kısmındaki **Tüket** sayfasında **örnek kod** 'yi tıklatın **toplu**.
   5. Bu yordamın 5 adıma geçin.
4. Yeni bir web hizmeti ile çalışıyorsanız, tıklatın **Web Hizmetleri**.
   1. Çalıştığınız web hizmeti tıklatın.
   2. Tıklatın **tüketen**.
   3. AT Tüket alt sayfası, buna **örnek kod** 'yi tıklatın **toplu**.
5. Toplu iş yürütme için örnek C# kodu kopyalayın ve ad alanı olduğu gibi kalır emin Program.cs dosyasına yapıştırın.

Açıklamaları belirtildiği gibi Microsoft.AspNet.WebApi.Client Nuget paketini ekleyin. Microsoft.WindowsAzure.Storage.dll başvuru eklemek için önce Microsoft Azure depolama hizmetleri için istemci kitaplığı yüklemeniz gerekebilir. Daha fazla bilgi için bkz: [Windows depolama hizmetleri](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Apikey ile yapılan bildirimini güncelleştirin
Bulun **apikey ile yapılan** bildirimi.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

İçinde **temel tüketim bilgileri** bölümünü **Tüket** sayfasında, birincil anahtarını bulun ve kopyalayın **apikey ile yapılan** bildirimi.

### <a name="update-the-azure-storage-information"></a>Azure depolama bilgilerini güncelleştir
BES örnek kod (örneğin "C:\temp\CensusIpnput.csv") yerel bir sürücüden bir dosyayı Azure Storage'a yükler, işler ve sonuçları Azure depolama birimine geri yazar.  

Bu görevi gerçekleştirmek için depolama hesabınızdan Klasik Azure portalı ve kodunda güncelleştirme karşılık gelen değerler için depolama hesabı adı, anahtar ve kapsayıcı bilgi almanız gerekir. 

1. Klasik Azure portalında oturum açın.
2. Sol gezinti sütununda tıklatın **depolama**.
3. Depolama hesapları listesinden bir retrained modelini depolamak için seçin.
4. Sayfanın alt kısmındaki tıklatın **erişim anahtarlarını Yönet**.
5. Kopyalayıp kaydedin **birincil erişim anahtarını** ve iletişim kutusunu kapatın. 
6. Sayfanın üstündeki **kapsayıcıları**.
7. Var olan bir kapsayıcı seçin veya yeni bir tane oluşturun ve ad kaydedin.

Bulun *StorageAccountName*, *StorageAccountKey*, ve *StorageContainerName* bildirimleri ve Azure portalından kaydettiğiniz değerlerini güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Ayrıca, girdi dosyası, kodda belirttiğiniz konumda kullanılabilir olduğundan emin olmalısınız. 

### <a name="specify-the-output-location"></a>Çıkış konumu belirtin
Çıkış konumu istek yükünde belirtirken, dosya uzantısını belirtilen *RelativeLocation* ilearner belirtilmelidir. 

Aşağıdaki örneğe bakın:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> Çıktı konumlarınıza adlarını web hizmeti çıkış modülleri eklenen sırasına göre bu kılavuzda gördüğünüzden farklı olabilir. Bu eğitim denemenizi iki çıkışları ile ayarlama olduğundan, sonuçlar her ikisi için depolama konumu bilgilerini içerir.  
> 
> 

![Çıktı yeniden eğitme][6]

Diyagram 4: çıkış yeniden eğitme.

## <a name="evaluate-the-retraining-results"></a>Yeniden eğitme sonuçları değerlendirin
Uygulamayı çalıştırdığınızda, çıktı değerlendirme sonuçlarını erişmek gerekli URL ve SAS belirteci içerir.

Birleştirerek retrained modeli performans sonuçlarını görebilirsiniz *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* içinçıktısonuçlarından*output2* (yukarıdaki yeniden eğitme çıkış görüntüde gösterildiği gibi) ve tam URL'sini tarayıcınızın adres çubuğuna yapıştırma.  

Yeni eğitilen model yeterince iyi var olan dosyayla gerçekleştirip gerçekleştirmeyeceğini belirlemek için sonuçlarını inceleyin.

Kopya *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* çıkış sonuçlarından, bunları yeniden eğitme işlemi sırasında kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Tıklayarak Tahmine dayalı web hizmetini dağıttıysanız **Web hizmeti Dağıt [Klasik]**, bkz: [bir Klasik web hizmeti yeniden eğitme](retrain-a-classic-web-service.md).

Tıklayarak Tahmine dayalı web hizmetini dağıttıysanız **Web hizmeti dağıtma [Yeni]**, bkz: [makine öğrenme yönetimi cmdlet'lerini kullanarak yeni bir web hizmeti yeniden eğitme](retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
