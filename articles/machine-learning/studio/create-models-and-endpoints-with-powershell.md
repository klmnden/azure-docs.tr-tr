---
title: "Birden çok model bir deneme oluşturma | Microsoft Docs"
description: "Birden çok Machine Learning modellerini ve web hizmeti uç noktaları aynı algoritmanın ancak farklı eğitim veri kümeleri oluşturmak için PowerShell kullanın."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: haining
ms.openlocfilehash: 4a4c1c417dabf32aa3a23fef22078ada0d01d9fa
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>PowerShell kullanarak bir denemeden çok sayıda Machine Learning modeli ve web hizmeti uç noktası oluşturma
Bir ortak makine öğrenimi sorun şudur: aynı eğitim iş akışını ve aynı algoritmasını kullanan çok sayıda modelleri oluşturmak istediğiniz. Ancak giriş olarak farklı eğitim veri kümesi olmasını istersiniz. Bu makalede yalnızca tek bir deneme kullanarak Azure Machine Learning Studio'da ölçekte bunu kullanmayı gösterir.

Örneğin, bir genel bisiklet kiralama Acentelik işletme sahibi varsayalım. Geçmiş verilere dayanan kiralama talep tahmin etmek için bir regresyon modeli oluşturmak istiyorsunuz. Dünya genelinde 1.000 kiralama konumunuz varsa ve her konum için bir veri kümesi derledik. Bunlar, date, time, hava durumu ve trafik gibi her bir konuma belirli önemli özellikleri içerir.

Bir kez tüm konumlar arasında tüm veri kümelerinin birleştirilmiş bir sürümünü kullanarak modelinizi eğitmek. Ancak, her konumlarınıza benzersiz bir ortam vardır. Bu nedenle daha iyi bir yaklaşım, ayrı ayrı her konum için veri kümesini kullanarak, regresyon modeli eğitmek için olacaktır. Bu şekilde, her eğitilen model hesaba farklı depolama boyutları, birim, coğrafi konum, popülasyon, bisiklet kolay trafiği ortamı ve daha fazla ele geçirebilir.

En iyi yaklaşım olabilir, ancak Azure Machine Learning ile her bir benzersiz bir konumu temsil eden 1.000 eğitim denemeler oluşturmak istemediğiniz. Bunaltıcı görev olmasının yanı sıra, aynı zamanda olup her deneme hepsi aynı eğitim veri kümesi dışında bileşenleri olacağından verimsiz gibi görünüyor.

Neyse ki, bunu kullanarak gerçekleştirebilirsiniz [Azure Machine Learning yeniden eğitme API](retrain-models-programmatically.md) ve görevle otomatikleştirme [Azure Machine Learning PowerShell](powershell-module.md).

> [!NOTE]
> Örneğinizi daha hızlı çalışmasını sağlamak için 1000 konumlardan 10 sayısını azaltın. Ancak aynı ilkeleri ve yordamları 1.000 konumlara uygulanır. Ancak, 1.000 veri kümeleri eğitmek istiyorsanız, paralel olarak aşağıdaki PowerShell betikleri çalıştırmak isteyebilirsiniz. Bunun nasıl yapılacağını bu makalenin kapsamı dışındadır, ancak PowerShell örnekleri çoklu iş parçacığı İnternette bulabilirsiniz.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Eğitim denemenizi ayarlayın
Örneği kullanmak [eğitim denemenizi](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) alanında [Cortana Intelligence Galerisi](http://gallery.cortanaintelligence.com). Bu deneme açın, [Azure Machine Learning Studio](https://studio.azureml.net) çalışma.

> [!NOTE]
> Bu örnek ile birlikte takip etmek için ücretsiz bir çalışma alanı yerine standart bir çalışma alanı kullanmak isteyebilirsiniz. -10 uç noktaları - toplam için her bir müşteri için bir uç nokta oluşturun ve boş bir çalışma alanı 3 uç noktalar ile sınırlı olduğundan standart bir çalışma alanı gerektirir. Yalnızca bir ücretsiz çalışma alanı varsa, yalnızca th konumları için izin vermek için komut dosyaları yalnızca değiştirin.
> 
> 

Denemeyi kullanan bir **veri almak** eğitim veri kümesi almak için Modülü *customer001.csv* bir Azure depolama hesabından. Tüm bisiklet kiralama konumlardan eğitim veri kümeleri toplanır ve bunları arasında değişen dosya adları aynı blob depolama konumunda depolanan varsayalım *rentalloc001.csv* için *rentalloc10.csv*.

![görüntü](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Unutmayın bir **Web hizmeti çıkış** modülü eklenmiştir **Train Model** modülü.
Bu deneme bir web hizmeti olarak dağıtıldığında, uç nokta çıkış eğitilen model bir .ilearner dosyası biçiminde döndürür ilişkili.

Ayrıca URL tanımlayan bir web hizmeti parametresini ayarlama unutmayın, **veri içeri aktarma** modülü kullanır. Bu parametre her konum için modeli eğitmek için tek tek eğitim veri kümesi belirtmek için kullanmanıza olanak sağlar.
Bunu yaptıktan diğer yolları vardır. Bir SQL Azure veritabanından veri almak için bir web hizmeti parametresi ile bir SQL sorgusu kullanabilirsiniz. Veya, kullanabileceğiniz bir **Web hizmeti girişi** modülü bir veri kümesinde web hizmetine iletmek için.

![görüntü](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Şimdi, varsayılan değerini kullanarak bu eğitim denemenizi Şimdi Çalıştır *rental001.csv* eğitim veri kümesi olarak. Çıktısını görüntülerseniz **değerlendir** Modülü (çıktı ve Seç'i tıklatın **Görselleştir**), bir iyi performansı elde görebilirsiniz *AUC* 0.91 =. Bu noktada, bu eğitim denemenizi dışında bir web hizmeti dağıtmak hazırsınız.

## <a name="deploy-the-training-and-scoring-web-services"></a>Eğitim ve puanlama web hizmetlerini dağıtma
Eğitim web hizmeti dağıtmak için **Web hizmetinin ayarı** deneme tuvalinin düğmesine tıklayın ve ardından **Web hizmeti Dağıt**. "" Bisiklet kiralama Eğitim". Bu web hizmeti çağrısı

Şimdi Puanlama web hizmeti dağıtmanız gerekir.
Bunu yapmak için tıklatın **Web hizmetinin ayarı** seçin ve tuvalin altındaki **Tahmine dayalı Web hizmeti**. Bu bir Puanlama deneme oluşturur.
Bir web hizmeti olarak çalışması için birkaç küçük ayarlamalar yapmanız gerekir. Giriş verilerinden Etiket sütununda "cnt" kaldırın ve yalnızca örnek kimliği ve karşılık gelen tahmin edilen değerin çıkışı sınırlamak.

Bu iş kendiniz kaydetmek için açabilirsiniz [Tahmine dayalı denemeye](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) zaten hazırlandı galerisinde.

Web hizmetini dağıtma için Tahmine dayalı denemeyi çalıştırın, ardından **Web hizmeti Dağıt** tuvale aşağıdaki düğmesine. Puanlama web hizmeti "Bisiklet kiralama Puanlama" adı ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>PowerShell ile 10 aynı web hizmeti uç noktalarını oluşturma
Bu web Hizmeti'nin varsayılan uç noktası ile birlikte gelir. Ancak güncelleştirilemiyor bu yana varsayılan uç olarak ilgilendiğiniz değil. Yapmanız gerekenler 10 ek uç noktalar, her konum için bir tane oluşturmaktır. Bu PowerShell ile yapabilirsiniz.

İlk olarak, PowerShell ortamını ayarlama:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Ardından, aşağıdaki PowerShell komutunu çalıştırın:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

10 uç noktaları oluşturulur ve bunların tümü aynı eğitilmiş içeren artık model üzerinde eğitilmiş *customer001.csv*. Bunları Azure Portalı'nda görüntüleyebilirsiniz.

![görüntü](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>PowerShell kullanarak eğitim veri kümelerini kullanan uç güncelleştir
Sonraki adım benzersiz olarak her müşterinin ayrı veri üzerinde eğitilmiş modeller ile uç noktaları güncelleştirmektir. Ancak bu modellerden üretmek için ihtiyacınız ilk **bisiklet kiralama eğitim** web hizmeti. Geri dönelim **bisiklet kiralama eğitim** web hizmeti. 10 farklı modelleri oluşturmak için BES uç noktasında 10 kez veri kümeleriyle 10 farklı eğitim çağırmanız gerekir. Kullanım **InovkeAmlWebServiceBESEndpoint** Bunu yapmak için PowerShell cmdlet.

Blob depolama hesabınızda kimlik bilgilerini sağlamanız gerekecektir `$configContent`. Yani, alanları en `AccountName`, `AccountKey`, ve `RelativeLocation`. `AccountName` De görüldüğü gibi hesap adlarını biri olabilir **Azure portal** (*depolama* sekmesi). Bir depolama hesabında tıkladığınızda, `AccountKey` basarak bulunabilir **erişim anahtarlarını Yönet** düğmesini kısımda ve kopyalama *birincil erişim anahtarını*. `RelativeLocation` Depolama alanınızın göre yeni bir model depolanacağı yoludur. Örneğin, yol `hai/retrain/bike_rental/` adlı bir kapsayıcı aşağıdaki komut dosyasını işaret `hai`, ve `/retrain/bike_rental/` alt klasörler. Şu anda, alt klasörler UI portal üzerinden oluşturamaz, ancak vardır [birkaç Azure depolama gezginleri](../../storage/common/storage-explorers.md) bunu izin verir. Depolama alanınızı yeni eğitilmiş modeller (.iLearner dosyaları) gibi depolamak yeni bir kapsayıcı oluşturmanız önerilir: depolama sayfanızdan tıklatın **Ekle** düğmesini kısımda ve adlandırın `retrain`. Özet olarak, aşağıdaki komut dosyası için gerekli değişiklikleri ilgilidir `AccountName`, `AccountKey`, ve `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> BES uç nokta bu işlem için yalnızca desteklenen moddur. RRS eğitilmiş modeller üretmek için kullanılamaz.
> 
> 

10 farklı BES iş yapılandırma json dosyalarını oluşturmak yerine, yukarıda gördüğünüz gibi dinamik olarak yapılandırma dizesi oluşturun. Kendisine akış *jobConfigString* parametresinin **InvokeAmlWebServceBESEndpoint** cmdlet'i. Gerçekten diskte bir kopyasını tutmak için gerek yoktur.

Her şey yolunda giderse, bir süre sonra 10 .iLearner dosyaları, gelen görmelisiniz *model001.ilearner* için *model010.ilearner*, Azure depolama hesabınızdaki. Web hizmeti uç noktalarını kullanarak bu modeller ile Puanlama 10 güncelleştirmeye hazır artık **düzeltme eki AmlWebServiceEndpoint** PowerShell cmdlet'i. Yeniden programlı olarak daha önce oluşturduğunuz varsayılan olmayan uç noktalar yalnızca düzeltme unutmayın.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Bu oldukça hızlı bir şekilde çalıştırmanız gerekir. Yürütme sona erdiğinde, başarıyla 10 Tahmine dayalı web hizmeti uç noktalarını oluşturdunuz. Her biri benzersiz bir kiralama konuma tek eğitim denemenizi tümünden belirli veri kümesi eğitilmiş bir modeli içerir. Bunu doğrulamak için kullanarak bu uç noktalarını çağırma deneyebilirsiniz **InvokeAmlWebServiceRRSEndpoint** cmdlet, aynı girdi verileriyle sağlama. Modeller farklı eğitim kümeleriyle eğitilmiş beri farklı tahmin sonuçlarını görmek beklemeniz gerekir.

## <a name="full-powershell-script"></a>Tam PowerShell Betiği
Burada, tam kaynak kodunu listesi verilmiştir:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
