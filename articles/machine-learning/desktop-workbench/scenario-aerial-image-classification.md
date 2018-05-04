---
title: Havadan görüntü sınıflandırma | Microsoft Docs
description: Havadan görüntü sınıflandırmasına gerçek dünya senaryoları için yönergeler sağlar
author: mawah
ms.author: mawah
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
services: machine-learning
ms.workload: data-services
ms.date: 12/13/2017
ms.openlocfilehash: 74cb72b4ddfbeace5e2409dfac4f4b6bf21ffa25
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="aerial-image-classification"></a>Havadan görüntü sınıflandırma

Bu örnek, Azure Machine Learning çalışma ekranı dağıtılmış eğitim ve görüntü sınıflandırma modelleri operationalization koordine etmek için nasıl kullanılacağını gösterir. Eğitim için iki yaklaşım kullanırız: iyileştirme (i) derin sinir ağı kullanarak bir [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) GPU küme ve (II) kullanarak [Microsoft Machine Learning Apache Spark (MMLSpark) için](https://github.com/Azure/mmlspark) paketini pretrained CNTK modelleri kullanarak featurize görüntüleri ve türetilen özelliklerini kullanarak sınıflandırıcı eğitmek için. Biz sonra eğitilmiş modeller paralel şekilde bulut kullanarak büyük görüntü ayarlar uygulamak bir [Azure Hdınsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) küme, bize eğitim ve operationalization hızına ekleyerek veya kaldırarak çalışan düğümleri ölçeklendirmek izin verme.

Bu örnek eğitim derin sinir ağları sıfırdan maliyetleri önlemek için pretrained modelleri yararlanır öğrenme aktarmak için iki yaklaşım vurgular. Derin sinir ağı genellikle yeniden eğitme GPU işlem gerektiriyor, ancak daha yüksek doğruluk için Eğitim kümesi yeterli büyüklükte olduğunda yol açabilir. Basit bir sınıflandırıcı featurized görüntülerinde eğitim GPU işlem gerektirmez, kendiliğinden hızlı ve rasgele ölçeklenebilir ve daha az parametre uygun. Bu yöntem, bu nedenle genellikle özel kullanım örnekleri için olduğu gibi birkaç eğitim örnekleri--kullanılamadığında mükemmel bir seçim kullanılır. 

Bu örnekte kullanılan Spark kümesi 40 çalışan düğümleri varsa ve çalışmaya ~$40/hr maliyetleri; Toplu AI küme kaynakları çalışmaya ~$10/hr maliyet ve sekiz GPU içerir. Bu kılavuzu tamamladıktan yaklaşık üç saat sürer. İşiniz bittiğinde, oluşturduğunuz ve ücretlerinin yansıtılmasını durdurmak kaynakları kaldırmak için temizleme yönergeleri izleyin.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Bu gerçek dünya senaryoları için ortak GitHub depo kod örnekleri, bu örnek için gerekli dahil olmak üzere tüm malzemeleri içerir:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>Kullanım örneği açıklaması

Bu senaryoda, 224 ölçer x 224 ölçer çizimleri havadan görüntülerini gösterilen kara türünü sınıflandırmak için makine öğrenimi modellerini eğitmek. Havadan görüntülerin düzenli aralıklarla kullanarak diğer önemli ortam eğilimlerini toplanan ve kara kullanım sınıflandırma modelleri urbanization, deforestation, wetlands kaybı izlemek için kullanılır. Biz ABD gelen görüntülerin göre eğitim ve doğrulama görüntü kümeleri hazırladınız Ulusal Tarım görüntülerin Program ve kara ABD tarafından yayımlanan etiketleri kullanma Ulusal kara kapak veritabanı. Her kara kullanım sınıfı örneği görüntülerinde burada gösterilir:

![Etiket örneği bölgeler her kara için kullanın](media/scenario-aerial-image-classification/example-labels.PNG)

Eğitim ve sınıflandırıcı model doğrulama sonra onu Middlesex ilçe, MA--giriş Microsoft'un yeni İngiltere araştırma ve geliştirme (NERD) merkezi--Kentsel eğilimler incelemek için bu modeller'ın nasıl kullanılabileceğini gösteren kapsayıcı havadan görüntülerine uygulayacağız geliştirme.

Öğrenme aktarımı kullanarak bir görüntü sınıflandırıcı üretmek için veri bilimcilerine genellikle yöntemleri aralıklı birden çok modelleri oluşturmak ve en fazla kullanıcı modelini seçin. Azure Machine Learning çalışma ekranı bilimcilerine eğitim farklı işlem ortamlar genelinde koordine, izlemek ve birden fazla modeli performansını karşılaştırın verileri yardımcı olmak ve seçilen model bulut büyük veri kümelerinde uygulanır.

## <a name="scenario-structure"></a>Senaryo yapısı

Bu örnekte, görüntü verilerini ve pretrained modelleri bir Azure depolama hesabında saklanır. Azure Hdınsight Spark kümesinde bu dosyaları okur ve MMLSpark kullanarak bir görüntü sınıflandırma modeli oluşturur. Eğitilen model ve onun tahminleri burada bunlar kullanılabilir analiz ve yerel olarak çalışan bir Jupyter not defteri tarafından görselleştirilen depolama hesabı, ardından yazılır. Azure Machine Learning çalışma ekranı komut dosyalarının Spark kümesinde uzaktan yürütme düzenler. Ayrıca, kullanıcının en kullanıcı modelini seçmek farklı yöntemler kullanarak eğitilmiş birden çok modelleri için doğruluğu ölçümleri izler.

![Havadan görüntü sınıflandırma gerçek dünya senaryosu için şeması](media/scenario-aerial-image-classification/scenario-schematic.PNG)

Bu adım adım yönergeler size oluşturulması ve bir Azure depolama hesabı ve Spark kümesi, veri aktarımı ve bağımlılık yükleme de dahil olmak üzere hazırlama göstererek başlar. Bunlar ardından eğitim işlerini başlatma ve sonuçta elde edilen modelleri performansını karşılaştırır açıklar. Son olarak, Spark kümesi büyük görüntü kümesinde seçilen model uygulamak ve yerel olarak tahmin sonuçlarını analiz etmek nasıl gösterilmektedir.


## <a name="set-up-the-execution-environment"></a>Yürütme ortamını ayarlama

Aşağıdaki yönergeler bu örneğin yürütme ortamı ayarlama işleminde size kılavuzluk eder.

### <a name="prerequisites"></a>Önkoşullar
- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir)
    - Hdınsight Spark kümesinde 40 çalışan düğümleri (toplam 168 çekirdekler) oluşturur. "Kullanım + kotalar" inceleyerek hesabınızı yeterli kullanılabilir çekirdeğe sahip olduğundan emin olun aboneliğinizin Azure portalında sekmesi.
       - Daha az çekirdek kullanılabilir varsa, sağlanan çalışanların sayısını azaltmak için Hdınsight küme şablonu değiştirebilir. Bunun için yönergeler "Hdınsight Spark kümesi oluşturma" bölümünde görüntülenir.
    - Bu örnek bir Batch AI Eğitim kümesi ile iki NC6 oluşturur (1 GPU, 6 vCPU) VM'ler. Hesabınızın yeterli kullanılabilir çekirdekler Doğu ABD bölgesinde "kullanım + kotalar" inceleyerek sahip olduğundan emin olun aboneliğinizin Azure portalında sekmesi.
- [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md)
    - İzleyin [yükleme ve hızlı başlangıç oluşturma](../service/quickstart-installation.md) Azure Machine Learning çalışma ekranı yükleyip deneme ve Model yönetim hesapları oluşturun.
- [Toplu AI](https://github.com/Azure/BatchAI) Python SDK'sı ve Azure CLI 2.0
    - Aşağıdaki bölümlerde tamamlamak [toplu AI tarif Benioku](https://github.com/Azure/BatchAI/tree/master/recipes):
        - "Önkoşullar"
        - "Oluşturun ve Azure Active Directory (AAD) uygulamanız Al"
        - "BatchAI kaynak sağlayıcıları register" (altında "Azure CLI 2.0" kullanarak tarif çalıştırın)
        - "Azure Batch AI Management istemcisi Yükle"
        - "Azure Python SDK Yükle"
    - Kayıt istemci kimliği, gizli ve Kiracı kimliği Azure Active Directory uygulamasının oluşturmak üzere yönlendirilir. Bu öğreticide daha sonra bu kimlik bilgilerini kullanır.
    - Bu makalenin yazıldığı sırada, Azure CLI 2.0 ayrı çatallarını Azure Machine Learning çalışma ekranı ve Azure Batch AI kullanın. Daha anlaşılır olması için biz "Azure Machine Learning çalışma ekranından başlatılan CLI" olarak CLI çalışma ekranı'nın sürümü ve (toplu AI içeren) genel yayın sürümü "Azure CLI 2.0." bakın
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy), bir ücretsiz Azure depolama hesapları arasında dosya aktarımı Eşgüdümleme yardımcı programı
    - AzCopy yürütülebilir içeren klasör, sisteminizin PATH ortam değişkeni üzerinde olduğundan emin olun. (Ortam değişkenleri değiştirme yönergeleri kullanılabilir [burada](https://support.microsoft.com/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Bir SSH istemcisi; öneririz [PuTTY](http://www.putty.org/).

Bu örnek, bir Windows 10 PC'de test edilmiştir; Azure veri bilimi sanal makineler de dahil olmak üzere tüm Windows makineden çalıştırılabilmesi için olması gerekir. Azure CLI 2.0 göre bir MSI yüklendiği [bu yönergeleri](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials). Küçük değişiklikler (örneğin, filepaths değişiklikler) gerekli macOS üzerinde bu örnek çalıştırılırken.

### <a name="set-up-azure-resources"></a>Azure kaynakları ayarlamak

Bu örnek, Hdınsight Spark kümesinde ve ana bilgisayar ilgili dosyaları için bir Azure depolama hesabı gerektirir. Bu kaynaklar yeni bir Azure kaynak grubu oluşturmak için bu yönergeleri izleyin:

#### <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında, **+** oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Hava görüntü sınıflandırması" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın
 
#### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

1. Azure Machine Learning çalışma ekranı projenizde yüklendikten sonra açık bir komut satırı arabirimi (dosya tıklatarak CLI) -> komut istemi açın.
    CLI'ın bu sürümü öğretici çoğunluğu için kullanın. (Belirtilen yerlerde, başka bir sürümü toplu AI kaynakları hazırlamak için CLI açmak için sorulur.)

1. Komut satırı arabiriminden aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

    ```
    az login
    ```

    URL ve sağlanan geçici kodu türü ziyaret istenir; Web sitesi Azure hesabı kimlik bilgilerinizi ister.
    
1. Oturum açma tamamlandığında, CLI dönün ve hangi Azure abonelikleri Azure hesabınızda kullanılabilir olduğunu belirlemek için aşağıdaki komutu çalıştırın:

    ```
    az account list
    ```

    Bu komut Azure hesabınızla ilişkili tüm abonelikleri listeler. Kullanmak istediğiniz abonelik Kimliğini bulun. Yazma abonelik kimliği aşağıdaki komutta gösterilen yerde sonra etkin bir aboneliğiniz komutunu yürüterek ayarlayın:

    ```
    az account set --subscription [subscription ID]
    ```

1. Bu örnekte oluşturulan Azure kaynaklarını birlikte bir Azure kaynak grubu içinde depolanır. Benzersiz kaynak grubu adı seçin ve okuma ve yazma belirtilen yerlerde, Azure kaynak grubu oluşturmak için her iki komut yürütün:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>Depolama hesabı oluşturma

Ana bilgisayar tarafından Hdınsight Spark erişilmelidir dosyaları proje depolama hesabı artık oluşturuyoruz.

1. Benzersiz depolama hesabı adı seçin ve okuma ve yazma aşağıda gösterilen yerde `set` komutu sonra her iki komutları yürüterek Azure depolama hesabı oluşturun:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. Aşağıdaki komutu çalıştırarak depolama hesabı anahtarlarını listele:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Değerini kaydedin `key1` aşağıdaki komutta depolama anahtarı olarak değeri depolamak için komutu çalıştırın.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Adlı bir dosya paylaşımı oluşturmak `baitshare` aşağıdaki komutla depolama hesabınızdaki:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. Sık kullandığınız metin düzenleyicinizde yük `settings.cfg` Azure Machine Learning çalışma ekranı projenin "Code" alt dizinden dosya ve depolama hesabı adı ve anahtar gösterildiği gibi ekleyin. Kaydet ve Kapat `settings.cfg` dosya.
1. Zaten yapmadıysanız, indirin ve yükleyin [AzCopy](http://aka.ms/downloadazcopy) yardımcı programı. AzCopy yürütülebilir "AzCopy" yazarak ve belgelerinde göstermek için Enter tuşuna basarak, sistem yolunda olduğundan emin olun.
1. Tüm örnek verileri, pretrained modelleri ve model eğitim betikler depolama hesabınızdaki uygun konumlara kopyalamak için aşağıdaki komutları yürütün:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Dosya aktarımı yaklaşık bir saat olması için bekler. Beklerken, aşağıdaki bölümüne geçebilirsiniz: çalışma ekranı aracılığıyla başka bir komut satırı arabirimi açın ve geçici değişkenleri yeniden tanımlamanız gerekebilir.

#### <a name="create-the-hdinsight-spark-cluster"></a>Hdınsight Spark kümesi oluşturma

Hdınsight kümesi oluşturmak için önerilen yöntem, bu proje "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" alt klasöründe bulunan Hdınsight Spark küme resource manager şablonu kullanır.

1. Hdınsight Spark küme bu projenin "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" alt "template.json" dosyasını şablonudur. Varsayılan olarak, şablon 40 çalışan düğümleri ile bir Spark kümesi oluşturur. Bu sayıyı ayarlamanız gerekir, sık kullandığınız metin düzenleyicinizde şablonu açın ve "40"'ın tüm örneklerini tercih ettiğiniz çalışan düğüm sayısı ile değiştirin.
    - Daha sonra seçtiğiniz çalışan düğüm sayısı küçükse, bellek yetersiz hatalarla karşılaşabilirsiniz. Bellek hataları mücadele etmek için eğitim ve operationalization betikleri kullanılabilir verilerin bir alt kümesinde bu belgenin sonraki bölümlerinde açıklandığı gibi çalışabilir.
2. Benzersiz bir ad ve parola Hdınsight için Küme ve bunları yazma seçin aşağıdaki komutta gösterilen yerde: Küme komutları vererek oluşturursunuz:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Kümenizin dağıtım (sağlama ve betik eylemi yürütme dahil) en fazla 30 dakika sürebilir.

### <a name="set-up-batch-ai-resources"></a>Toplu AI kaynakları ayarlamak

Depolama hesabı dosya aktarımı ve Spark Küme dağıtımı için tamamlamak beklerken toplu AI ağ dosya sunucusu (NFS) ve GPU küme hazırlayabilirsiniz. Bir Azure CLI 2.0 komut istemi açın ve aşağıdaki komutu çalıştırın:

```
az --version 
```

Onaylayın `batchai` yüklü modülleri arasında listelenir. Aksi durumda, farklı komut satırı arabirimi (örneğin, bir çalışma ekranı açılan) kullanıyor olabilir.

Ardından, sağlayıcı kaydı başarıyla tamamlanıp tamamlanmadığını denetleyin. (Sağlayıcı kaydı en fazla beş dakika sürer ve hala son tamamladıysanız devam eden olabilir [toplu AI kurulum yönergelerini](https://github.com/Azure/BatchAI/tree/master/recipes).) "Microsoft.Batch" ve "Microsoft.BatchAI" aşağıdaki komut çıktısı "Kaydedildi" durumu görünür olduğunu doğrulayın:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Yoksa, aşağıdaki sağlayıcıyı kayıt komutları çalıştırmak ve tamamlamak kayıt ~ 15 dakika bekleyin.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

Daha önce kaynak grubu ve depolama hesabı oluşturma sırasında kullanılan değerlerle ayraçlı ifadeler değiştirmek için aşağıdaki komutları değiştirin. Ardından, değerleri, komutları yürüterek değişkenleri olarak depola:
```
az account set --subscription [subscription ID]
set AZURE_RESOURCE_GROUP=[resource group name]
set STORAGE_ACCOUNT_NAME=[storage account name]
set STORAGE_ACCOUNT_KEY=[storage account key]
az configure --defaults location=eastus
az configure --defaults group=%AZURE_RESOURCE_GROUP%
```

Azure Machine Learning projeyi içeren klasörü belirleyin (örneğin, `C:\Users\<your username>\AzureML\aerialimageclassification`). Köşeli parantez içindeki değer klasörün filepath (ile hiçbir eğik) değiştirin ve komutu yürütün:
```
set PATH_TO_PROJECT=[The filepath of your project's root directory]
```
Şimdi Bu öğretici için gerekli toplu AI kaynakları oluşturmaya hazırsınız.

#### <a name="prepare-the-batch-ai-network-file-server"></a>Toplu iş AI ağ dosya sunucusunu hazırlama

Toplu AI kümenizi eğitim verilerinizi bir ağ dosya sunucusundaki erişir. Veri erişimi daha hızlı bir Azure dosya paylaşımı veya Azure Blob Storage ve NFS gelen dosyalara erişirken several-fold olabilir.

1. Bir ağ dosya sunucusu oluşturmak için aşağıdaki komutu yürütün:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p "Dem0Pa$$w0rd" --vm-size Standard_DS2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Aşağıdaki komutu kullanarak, ağ dosya sunucunuzun sağlama durumunu kontrol edin:

    ```
    az batchai file-server list
    ```

    Ağ dosya sunucusu "landuseclassifier" adlı "provisioningState" "başarılı zaman" kullanıma hazırdır. Yaklaşık beş dakika sürer sağlama bekler.
1. ("MountSettings" altında "fileServerPublicIp" özelliği) önceki komutunun çıktısı, NFS IP adresini bulun. IP aşağıdaki komutta gösterilen yerde adres, sonra komutu yürüterek değerini depolama yazma:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. Sık kullanılan SSH aracını kullanarak (Aşağıdaki örnek komut kullanır [PuTTY](http://www.putty.org/)), bu projenin yürütme `prep_nfs.sh` eğitim ve doğrulama görüntü aktarmak için NFS komut ayarlar vardır.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Veri yükleme ve ayıklama ilerleme güncelleştirmeleri Kabuğu penceresini kadar hızlı bir şekilde okunmaz kaydırırsanız endişelenmeniz gerekmez.

İsterseniz, veri aktarımı gibi sık kullanılan SSH aracınızla dosya sunucusuna oturum açmayı ve denetimi planlanmış proceeded onaylayabilirsiniz `/mnt/data` dizin içeriği. İki klasör, training_images ve validation_images bulmanız gerekir, her göre kara adlı alt klasörler içeren kategorileri kullanın.  Eğitim ve doğrulama kümeleri ~ 44 içermelidir k ve ~ 11 k görüntüleri, sırasıyla.

#### <a name="create-a-batch-ai-cluster"></a>Bir toplu AI kümesi oluşturma

1. Aşağıdaki komutu gönderdikten küme oluşturun:

    ```
    az batchai cluster create -n landuseclassifier2 -u demoUser -p "Dem0Pa$$w0rd" --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 --storage-account-name %STORAGE_ACCOUNT_NAME% 
    ```

1. Kümenizi sağlama durumu kullanıcının denetlemek için aşağıdaki komutu kullanın:

    ```
    az batchai cluster list
    ```

    Ayırma durumu kümeyi için sabit için yeniden boyutlandırma gelen "landuseclassifier" değişiklikleri adlı olduğunda iş göndermek mümkündür. Ancak, işleri kümedeki tüm sanal makineleri "Hazırlama" durumu bıraktıysanız kadar çalışmaya başlatmayın. Kümenin "hataları" özelliği boş değilse, küme oluşturma sırasında bir hata oluştu ve onu kullanılmamalıdır.

#### <a name="record-batch-ai-training-credentials"></a>Kayıt toplu AI eğitim kimlik bilgileri

Küme ayırma için beklerken açmak `settings.cfg` metin düzenleyiciyi bu projede "Code" alt dosyasından. Aşağıdaki değişkenler kendi kimlik bilgilerinizle güncelleştirin:
- `bait_subscription_id` (36 karakter Azure abonelik Kimliğinizi)
- `bait_aad_client_id` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Uygulama/istemci kimliği)
- `bait_aad_secret` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Uygulama gizli anahtarı)
- `bait_aad_tenant` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Kiracı kimliği)
- `bait_region` (Bu makalenin yazıldığı sırada tek seçenektir: eastus)
- `bait_resource_group_name` (daha önce seçtiğiniz kaynak grubu)

Bu değerleri atadıktan sonra değiştirilen satırları settings.cfg dosyanızın aşağıdaki metni benzemelidir:

```
[Settings]
    # Credentials for the Azure Storage account
    storage_account_name = yoursaname
    storage_account_key = kpIXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXQ==
    
    # Batch AI training credentials
    bait_subscription_id = 0caXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX9c3
    bait_aad_client_id = d0aXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX7f8
    bait_aad_secret = ygSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX6I=
    bait_aad_tenant = 72fXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXb47
    bait_region = eastus
    bait_resource_group_name = yourrgname
```

Kaydet ve Kapat `settings.cfg`.

Artık, toplu AI kaynak oluşturma komutlarını yürütüldüğü CLI pencereyi kapatabilirsiniz. Bu öğreticide sonraki tüm adımların Azure Machine Learning çalışma ekranından başlatılan CLI kullanın.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Azure Machine Learning çalışma ekranı yürütme ortamını hazırlayın

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Hdınsight kümesi bir Azure Machine Learning çalışma ekranı işlem hedefi olarak Kaydet

Hdınsight küme oluşturma tamamlandıktan sonra küme projeniz için bir işlem hedefi olarak şu şekilde kaydedin:

1.  Azure Machine Learning komut satırı arabiriminden aşağıdaki komutu yürütün:

    ```
    az ml computetarget attach cluster --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD%
    ```

    Bu komut, iki dosya ekler `myhdi.runconfig` ve `myhdi.compute`, projenizin için `aml_config` klasör.

1. Açık `myhdi.compute` sık kullandığınız metin düzenleyicinizde dosya. Değiştirme `yarnDeployMode: cluster` okumak için satır `yarnDeployMode: client`, sonra dosyayı kaydedip kapatın.
1. Hdınsight ortamınızı kullanmak için hazırlamak için aşağıdaki komutu çalıştırın:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Yerel bağımlılıkları yükler

Azure Machine Learning çalışma ekranından CLI açın ve aşağıdaki komutu gönderdikten yerel yürütme için gerekli bağımlılıkları yükler:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Veri edinme ve anlama

Bu senaryo, genel kullanıma açık havadan görüntülerin verilerden kullanır [Ulusal Tarım görüntülerin Program](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) 1 ölçer çözünürlükte. Biz özgün NAIP verilerden kırpılacak ve kara kullanım etiketlerden göre sıralanmış 224 piksel x 224 piksel PNG dosyaları kümesi oluşturmuş [Ulusal kara kapak veritabanı](https://www.mrlc.gov/nlcd2011.php). "Developed" etiketli bir örnek görüntüyü tam boyutuyla gösterilir:

![Geliştirilmiş kara örnek parçasına](media/scenario-aerial-image-classification/sample-tile-developed.png)

~ 44 sınıfı dengeli kümesi k ve 11 k görüntüleri kullanılan model eğitim ve doğrulama için sırasıyla. Model dağıtımı ~ 67 k görüntüsüne döşemesini Middlesex ilçe, MA--giriş Microsoft'un yeni İngiltere araştırma ve geliştirme (NERD) merkezi ayarlamak göstermektedir. Bu görüntü kümeleri nasıl oluşturulan daha fazla bilgi için bkz: [utandırıcı derecede paralel görüntü sınıflandırma git deposu](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Middlesex ilçe, Massachusetts konumu](media/scenario-aerial-image-classification/middlesex-ma.png)

Kurulum sırasında bu örnekte kullanılan havadan görüntü kümeleri oluşturduğunuz depolama hesabını aktarıldı. Eğitim, doğrulama ve operationalization görüntüleri tüm 224 piksel x 224 piksel PNG metre kare başına bir piksel çözünürlükte dosyalarıdır. Eğitim ve doğrulama görüntüleri kendi kara kullanım etikete göre alt klasörler halinde düzenlenmiştir. (Kara etiket operationalization görüntülerinin bilinmeyen ve çoğu durumda belirsiz; bu görüntüleri bazıları birden çok kara türü içeriyor.) Bu görüntü kümeleri nasıl oluşturulan daha fazla bilgi için bkz: [utandırıcı derecede paralel görüntü sınıflandırma git deposu](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

Azure depolama alanınızı görüntülerinde hesabını (isteğe bağlı) örnek görüntülemek için:
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
1. Ekranın üstünde arama çubuğunda depolama hesabınızın adını arayın. Depolama hesabınız arama sonuçlarında tıklayın.
2. Depolama hesabının ana bölmesinde "BLOB'lar" bağlantısını tıklatın.
3. "Tren." adlı kapsayıcı'ı tıklatın Kara göre adlı dizinlerin listesini kullanın görmeniz gerekir.
4. İçerdiği görüntülerin listesi yüklemek için bu dizinleri birini tıklatın.
5. Herhangi bir görüntü tıklatın ve görüntüyü görüntülemek için indirin.
6. İsterseniz, "test" ve "middlesexma2016 içerikleri de görüntülemek için" adlı kapsayıcılarında'ı tıklatın.

## <a name="modeling"></a>Modelleme

### <a name="training-models-with-azure-batch-ai"></a>Azure Batch AI eğitim modelleriyle

`run_batch_ai.py` Çalışma ekranı projesinin "Code\02_Modeling" alt komut dosyası toplu AI eğitim iş vermek için kullanılır. Bu işin bir görüntü sınıflandırıcı (AlexNet veya ResNet ImageNet üzerinde pretrained 18) kullanıcı tarafından seçilen DNN retrains. Yeniden eğitme derinliği de belirtilebilir: ağ yalnızca son katmanı yeniden eğitme azaltmak overfitting birkaç eğitim örnekleri kullanılabilir, tüm ağ hassas ayar yapma çalışırken (veya AlexNet, tam olarak bağlı Katmanlar) olduğunda büyük modeline yol açabilir Eğitim kümesi yeterli büyüklükte olduğunda performans.

Eğitim işi tamamlandığında, bu komut dosyası (yanı sıra modelin tamsayı çıkış ve dize etiketleri arasında eşleme açıklayan bir dosya) modeli ve tahminleri blob depolama alanına kaydeder. YEMİ işin günlük dosyası, eğitim dönemlerinde üzerinden hata oranı geliştirme timecourse ayıklamak için ayrıştırılır. Hata oranı geliştirme timecourse AML çalışma'nin çalıştırma geçmişi özelliği daha sonra görüntülemek için günlüğe kaydedilir.

Eğitilen model pretrained model türünü ve yeniden eğitme derinliği için bir ad seçin. Yazma seçimlerinizi aşağıdaki komutta gösterilen yerde sonra bir Azure ML komut satırı arabiriminden komutunu yürüterek yeniden eğitme başlayın:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Azure Machine Learning tamamlamak için yaklaşık yarım saat yapılacak çalıştırmak bekler. Böylece farklı yöntemlerle eğitilmiş modeller performansını karşılaştırabilirsiniz (çıktı model adı, pretrained model türünü ve yeniden eğitme derinliği değişen) birkaç benzer komutları çalıştırmanızı öneririz.

### <a name="training-models-with-mmlspark"></a>Eğitim modelleriyle MMLSpark

`run_mmlspark.py` Komut dosyası çalışma ekranı projesinin "Code\02_Modeling" alt eğitmek için kullanılan bir [MMLSpark](https://github.com/Azure/mmlspark) görüntü sınıflandırması için model. Komut dosyası ilk featurizes eğitim DNN pretrained bir görüntü sınıflandırıcı ImageNet veri kümesine (AlexNet veya 18 katman ResNet) kullanarak görüntüleri ayarlayın. Komut dosyası featurized görüntüleri (rastgele bir orman ya da bir Lojistik regresyon modeli) görüntüleri sınıflandırmak için bir MMLSpark modeli eğitmek için sonra kullanır. Test görüntü kümesi featurized ise ve eğitilen model ile skoru. Sınama kümesi üzerinde modelinin tahminleri doğruluğunu hesaplanır ve Azure Machine Learning çalışma ekranı'nin çalıştırma geçmişi özelliğini oturum. Son olarak, eğitilen MMLSpark modeli ve sınama kümesi üzerinde kendi tahminleri BLOB depolamaya kaydedilir.

Eğitilen model pretrained model türünü ve MMLSpark model türü için bir benzersiz çıkış model adı seçin. Yazma seçimlerinizi aşağıdaki komut şablonda belirtilen yerlerde sonra bir Azure ML komut satırı arabiriminden komutunu yürüterek yeniden eğitme başlayın:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Ek bir `--sample_frac` parametresi, eğitmek ve bir veri alt kümesini kullanılabilir modeliyle test etmek için kullanılabilir. Küçük bir örnek kesir kullanarak eğitilen model doğruluğundan ödün verme pahasına çalışma zamanı ve bellek gereksinimlerini azaltır. (Örneğin, bir Farklı Çalıştır ile `--sample_frac 0.1` yaklaşık 20 dakika sürmesi beklendiğinde.) Bu ve diğer parametreleri hakkında daha fazla bilgi için Çalıştır `python Code\02_Modeling\run_mmlspark.py -h`.

Kullanıcılar, bu komut dosyası, farklı giriş parametreleriyle birkaç kez çalıştırmak için önerilir. Sonuçta elde edilen modelleri performansını sonra Azure Machine Learning çalışma ekranı'nin çalıştırma geçmişi özelliği karşılaştırılabilir.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Çalışma ekranı çalıştırma geçmişi özelliğini kullanarak modeli performans karşılaştırma

İki çalıştırdıktan veya daha fazla eğitim iki tür çalıştıran sonra çalışma ekranındaki çalıştırma geçmişi özelliğini sol menü çubuğunu boyunca saat simgesini tıklatarak gidin. Seçin `run_mmlspark.py` soldaki betikleri listesinden. Bir bölme tüm çalışmaları için test kümesi doğruluğu karşılaştırma yükler. Daha fazla ayrıntı görmek için aşağı kaydırın ve ayrı bir çalışma adına tıklayın.

## <a name="deployment"></a>Dağıtım

Eğitilmiş modeller birini Middlesex ilçe, MA uzaktan yürütme kullanarak döşeme havadan görüntüleri uygulamak için istenen modelinizin ad aşağıdaki komutu ekleyin ve onu yürütün:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Ek bir `--sample_frac` parametresi, kullanılabilir verilerin bir alt kümesini modeliyle faaliyete geçirmek için kullanılabilir. Küçük bir örnek kesir kullanarak çalışma zamanı ve bellek gereksinimlerini tahmin bütünlük ödün verme pahasına azaltır. Bu ve diğer parametreleri hakkında daha fazla bilgi için Çalıştır `python Code\03_Deployment\batch_score_spark -h`.

Bu komut, depolama hesabınıza modelinin tahminleri yazar. Sonraki bölümde açıklandığı gibi tahminleri incelenebilir.

## <a name="visualization"></a>Görselleştirme

Çalışma ekranı projesinin "Code\04_Result_Analysis" alt "Tahmin analiz modeli" Jupyter not defteri bir modelin tahminleri visualizes. Yük ve Not Defteri gibi çalıştırın:
1. Proje içinde çalışma ekranı açın ve klasör dizin listeleme yüklemek üzere sol menüdeki boyunca ("dosyalar") simgesine tıklayın.
2. "Code\04_Result_Analysis" alt klasöre gidin ve "Tahmin analiz modeli" adlı dizüstü bilgisayar'ı tıklatın Not Defteri Önizleme çizmeye görüntülenmesi gerekir.
3. "Başlat Not Not Defteri yüklemek için sunucu" seçeneğini tıklatın.
4. İlk hücreye sonuçları bloğunu çözümlemek istediğiniz model adını girin.
5. Tıklayın "hücre Çalıştır -> tüm" Not defterinde tüm hücreleri yürütülecek.
6. Analizleri ve onu sunar görselleştirmeleri hakkında daha fazla bilgi edinmek için Not Defteri ile birlikte okuyun.

## <a name="cleanup"></a>Temizleme
Örnek tamamlandığında, Azure komut satırı arabiriminden aşağıdaki komutu çalıştırarak oluşturmuş olduğunuz tüm kaynakları silmenizi öneririz:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Başvurular

- [Utandırıcı derecede paralel görüntü sınıflandırma deposu](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Veri kümesi oluşturma ücretsiz görüntüler ve etiketleri açıklar
- [MMLSpark](https://github.com/Azure/mmlspark) GitHub deposunu
   - Model eğitimi ve değerlendirmesi MMLSpark ile ek örneklerini içerir

## <a name="conclusions"></a>Sonuçları

Azure Machine Learning çalışma ekranı kolayca kendi kodunuzu uzak işlem hedeflerde dağıtmak veri bilimcilerine yardımcı olur. Bu örnekte, yerel MMLSpark eğitim kod Hdınsight kümesinde uzaktan yürütme için dağıtıldı ve yerel bir komut dosyası bir Azure Batch AI GPU küme eğitim işinde başlattı. Azure Machine Learning çalışma ekranı'nin çalıştırma geçmişi özelliği, birden fazla modeli performansını izlenen ve bize en doğru modeli tanımlamaya yardım. Çalışma ekranı 's Jupyter not defterlerini özelliği bir etkileşimli, grafiksel ortamda bizim modelleri tahminleri görselleştirmenize yardımcı oldu.

## <a name="next-steps"></a>Sonraki adımlar
Bu örnek daha derin çalışmak için:
- Azure Machine Learning çalışma ekranı'nin çalıştırma geçmişi özelliği hangi grafikleri ve ölçümleri görüntüleneceğini seçmek için dişli simgeleri tıklatın.
- Çağırma deyimleri için örnek komut dosyalarını inceleyin `run_logger`. Her ölçüm nasıl kaydedilen anlamak denetleyin.
- Çağırma deyimleri için örnek komut dosyalarını inceleyin `blob_service`. Nasıl eğitilmiş modeller anlamak ve tahminleri depolanır ve buluttan alınır denetleyin.
- Blob depolama hesabınızdaki oluşturulan kapsayıcıları içeriğini keşfedin. Hangi komut dosyası anlamak veya komut dosyaları her grubu oluşturmaktan sorumlu olduğundan emin olun.
- Farklı bir MMLSpark model türü eğitmek için veya model hyperparameters değiştirmek için eğitim komut dosyasını değiştirin. Değişikliklerinizi artırılabilir veya modelin doğruluğunu azaltılabilir olup olmadığını belirlemek için çalıştırma geçmişi özelliğini kullanın.
