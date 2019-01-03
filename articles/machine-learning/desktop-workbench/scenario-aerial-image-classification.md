---
title: Havadan görünüm sınıflandırması | Microsoft Docs
description: Havadan görünüm sınıflandırması üzerinde gerçek dünya senaryosu için yönergeler sağlar
author: mawah
ms.author: mawah
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
ms.component: core
services: machine-learning
ms.workload: data-services
ms.date: 12/13/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9dd4ca4b4f156823aff3b8a475e06ea63f98be6c
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975313"
---
# <a name="aerial-image-classification"></a>Havadan görünüm sınıflandırması

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Bu örnek dağıtılmış eğitimi ve kullanıma hazır hale getirme, görüntü sınıflandırma modellerini koordine etmek için Azure Machine Learning Workbench'i kullanma işlemini gösterir. Eğitim için iki yaklaşım kullanıyoruz: iyileştirme (i) bir derin sinir ağı kullanarak bir [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) GPU kümesi ve (ii) kullanarak [(MMLSpark) Apache Spark için Microsoft Machine Learning](https://github.com/Azure/mmlspark) için paket CNTK modelleri kullanan kullanarak özellik kazandırın görüntüleri ve türetilmiş özelliklerini kullanarak sınıflandırıcılar eğitmek için. Biz ardından eğitilen modelleri paralel şekilde büyük görüntü kümesi kullanarak bulut geçerli bir [Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) küme, eğitim ve kullanıma hazır hale getirme hızına ekleyerek veya kaldırarak çalışan düğümlerini ölçeklendirmek bize izin verme.

Bu örnekte, sıfırdan eğitimi derin sinir ağı maliyetleri önlemek için modelleri kullanan yararlanan öğrenme aktarmak için iki yaklaşım vurgular. Derin sinir ağı genellikle yeniden eğitme GPU işlem gerektirir, ancak daha yüksek doğruluk için Eğitim kümesi yeterince büyük olduğunda yol açabilir. Basit bir sınıflandırıcı özellikleri tespit görüntülerindeki eğitim GPU işlem gerektirmez, kendiliğinden hızlı ve rasgele olarak ölçeklenebilir ve daha az parametreleri en uygun. Genellikle özel kullanım örnekleri için olduğu gibi birkaç eğitim örnekleri--kullanılabilir olduğunda bu yöntem bu nedenle bir mükemmel seçimdir. 

Bu örnekte kullanılan Spark kümesi, 40 çalışan düğümleri varsa ve çalışmaya ~$40/hr maliyetleri; Batch AI kümesi kaynakları sekiz GPU'ları içerir ve çalışılacak ~$10/hr maliyeti. Bu izlenecek yolu tamamlamak yaklaşık üç saat sürüyor. İşlemi tamamladığınızda, oluşturduğunuz ve ücretlerinin yansıtılmasını durdurmak kaynakları kaldırmak için temizleme yönergeleri izleyin.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Bu örnek için gerekli, kod örnekleri dahil olmak üzere tüm malzemeler, bu gerçek dünya senaryosu için ortak GitHub deposu içerir:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>Kullanım örneği açıklaması

Bu senaryoda, biz 224 ölçüm x 224 ölçüm çizimleri havadan görüntülerini gösterilen land türünü sınıflandırmak için makine öğrenimi modellerini eğitin. Havadan tanımayı diğer önemli ortam eğilimleri kullanarak düzenli aralıklarla toplanan ve land kullanım sınıflandırma modellerini urbanization, deforestation wetlands kaybı izlemek için kullanılır. Gözünüzde ABD gelen temel eğitim ve doğrulama görüntü kümeleri hazırladığımız Ulusal Tarım tanımayı Program ve land ABD yayımlanan etiketleri kullanma Ulusal Land kapak veritabanı. Her land kullanım sınıfı örnek görüntüleri aşağıda gösterilmiştir:

![Örnek bölgeler her land için etiket kullanma](media/scenario-aerial-image-classification/example-labels.PNG)

Eğitim ve sınıflandırıcı model doğrulama sonra biz bunu Middlesex ilçe, MA--giriş Microsoft'un yeni İngiltere araştırma ve geliştirme (NERD) merkezi--bu modeller urban eğilimler incelemek için nasıl kullanılabileceğini göstermek için kapsayıcı havadan görüntülerine uygulanır geliştirme.

Aktarımlı kullanarak bir görüntü sınıflandırıcı üretmek için veri bilimcilerine genellikle bir dizi yöntem ile birden çok modelleri ve en iyi performansa sahip modelin seçilme. Azure Machine Learning Workbench, veri uzmanları, farklı işlem ortamlarında eğitim koordine etmek, izlemek ve performansını birden çok modeli karşılaştırma yardımcı ve seçtiğiniz model bulut büyük veri kümelerinde uygulanır.

## <a name="scenario-structure"></a>Senaryo yapısı

Bu örnekte, görüntü verilerini ve modeli kullanan bir Azure depolama hesabında saklanır. Bir Azure HDInsight Spark kümesi, bu dosyaları okur ve MMLSpark'ı kullanarak bir görüntü sınıflandırma modeli oluşturur. Ardından eğitilen model ve kendi Öngörüler burada edilebilmeleri analiz ve yerel olarak çalışan bir Jupyter not defteri tarafından görselleştirilen depolama hesabına yazılır. Azure Machine Learning Workbench'te betikleri Spark kümesi üzerinde uzaktan yürütülmesini düzenler. Ayrıca, kullanıcının en iyi performansa sahip modelin seçilme izin farklı yöntemler kullanılarak geliştirilen birden çok modellerinin doğruluğu ölçümlerini izler.

![Havadan sınıflandırma gerçek dünya senaryosu için şeması](media/scenario-aerial-image-classification/scenario-schematic.PNG)

Bu adım adım yönergeleri size oluşturma ve hazırlama Azure depolama hesabı ve veri aktarımı ve bağımlılık yükleme dahil olmak üzere bir Spark kümesi aracılığıyla göstererek başlayın. Bunlar ardından eğitim işleri başlatmak ve ortaya çıkan modeller performansını karşılaştırmak nasıl açıklar. Son olarak, bir büyük görüntü kümesi üzerinde Spark kümesi seçtiğiniz model uygulamak ve yerel olarak tahmin sonuçlarını analiz etme göstermektedir.


## <a name="set-up-the-execution-environment"></a>Yürütme ortamını ayarlama

Aşağıdaki yönergeler bu örneğin yürütme ortamı ayarlama işleminde size kılavuzluk eder.

### <a name="prerequisites"></a>Önkoşullar
- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir)
    - 40 çalışan düğümleri (toplam 168 çekirdek), bir HDInsight Spark kümesi oluşturacaksınız. Hesabınızın yeterli sayıda kullanılabilir çekirdek "kullanım ve kotalar" gözden geçirerek sahip olduğundan emin olun Azure portalında aboneliğinizi için sekmesinde.
       - Daha az çekirdeğe sahip, sağlanan çalışanların sayısını azaltmak için HDInsight küme şablonu değiştirebilir. Bu yönergeler, "HDInsight Spark kümesi oluşturma" bölümünün altında görüntülenir.
    - Bu örnek, bir Batch AI eğitimi kümesi ile iki NC6 oluşturur. (1 GPU, 6 vCPU) VM'ler. Hesabınızın yeterli sayıda kullanılabilir çekirdek Doğu ABD bölgesinde "kullanım ve kotalar" gözden geçirerek sahip olduğundan emin olun Azure portalında aboneliğinizi için sekmesinde.
- [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md)
    - İzleyin [yükleme ve oluşturma Hızlı Başlangıç](../desktop-workbench/quickstart-installation.md) deneme ve Model yönetim hesapları oluşturma ve Azure Machine Learning Workbench'i yükleyin.
- [Batch AI](https://github.com/Azure/BatchAI) Python SDK'sını ve Azure CLI
    - Aşağıdaki bölümlerde tamamlamak [Batch AI tarifleri Benioku](https://github.com/Azure/BatchAI/tree/master/recipes):
        - "Önkoşullar"
        - "Oluşturmak ve uygulamanızı Azure Active Directory (AAD)"
        - "("Azure CLI kullanarak çalışma tarif içeren"altında) BatchAI kaynak sağlayıcılarını kaydetme"
        - "Azure Batch AI Yönetimi istemcisini yükleyin"
        - "Azure Python SDK'sını Yükle"
    - Kayıt istemci kimliği, gizli anahtarı ve oluşturma yönlendirilirsiniz Azure Active Directory uygulamasının Kiracı kimliği. Bu öğreticide daha sonra bu kimlik bilgilerini kullanır.
    - Azure Machine Learning Workbench ve Azure Batch AI, bu makalenin yazıldığı tarih itibarıyla Azure CLI'ın ayrı çatalları kullanın. Anlaşılsın diye, "Azure Machine Learning Workbench'ten başlatılan bir CLI" olarak CLI'yi Workbench'in sürümünü ve (kod Batch AI'ı içerir) genel yayın sürümü "Azure CLI" diyoruz
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy), ücretsiz Azure depolama hesapları arasında dosya aktarımını koordine yardımcı programı
    - AzCopy yürütülebilir dosyayı içeren klasör sisteminizin yol ortam değişkenine olduğundan emin olun. (Ortam değişkenleri değiştirme yönergeleri kullanılabilir [burada](https://support.microsoft.com/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Bir SSH istemcisi; öneririz [PuTTY](http://www.putty.org/).

Bu örnek, bir Windows 10 bilgisayarında test edilmiştir; Azure veri bilimi sanal makineleri dahil olmak üzere bir Windows makineden çalıştırılabilmesi için olmalıdır. Azure CLI'yı bir MSI göre yüklendiği [bu yönergeleri](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials). Küçük değişiklikler (örneğin, değişiklikleri filepaths) gerekli macOS üzerinde bu örnek çalışırken.

### <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama

Bu örnek, bir HDInsight Spark kümesi ve ilgili konak dosyaları için bir Azure depolama hesabı gerektirir. Yeni bir Azure kaynak grubunda bu kaynakları oluşturmak için bu yönergeleri izleyin:

#### <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna, "Havadan görüntü sınıflandırması" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın
 
#### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

1. Azure Machine Learning Workbench projenizde yüklendikten sonra açık bir komut satırı arabirimi (dosya tıklayarak CLI) -> komut istemini Aç.
    CLI'ın bu sürümü, öğreticinin çoğu için kullanın. (Belirtilen yerlerde, Batch AI kaynak hazırlamak için CLI'ın başka bir sürümü açın istenir.)

1. Komut satırı arabiriminden aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

    ```
    az login
    ```

    URL ve sağlanan geçici bir kod türü ziyaret istenir; Web sitesi, Azure hesabı kimlik bilgilerinizi ister.
    
1. Oturum açma tamamlandığında, CLI için geri dönün ve hangi Azure abonelikleri Azure hesabınızda kullanılabilir olduğunu belirlemek için aşağıdaki komutu çalıştırın:

    ```
    az account list
    ```

    Bu komut, Azure hesabınızla ilişkili tüm abonelikleri listeler. Kullanmak istediğiniz aboneliğin Kimliğini bulun. Yazma abonelik Kimliğini aşağıdaki komutta belirtilen yerlerde etkin aboneliği komutu yürüterek ayarlayın:

    ```
    az account set --subscription [subscription ID]
    ```

1. Bu örnekte oluşturulan Azure kaynaklarını bir Azure kaynak grubu içinde birlikte depolanır. Benzersiz kaynak grubu adı seçin ve Not belirtilen yerlerde, sonra Azure kaynak grubu oluşturmak için her iki komutu yürütün:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>Depolama hesabı oluşturma

Artık konakları HDInsight Spark tarafından erişilen dosyalar proje depolama hesabı oluşturacağız.

1. Benzersiz bir depolama hesabı adı seçin ve Not aşağıda belirtilen yerlerde `set` komutunu ve ardından her iki komutu yürüterek bir Azure depolama hesabı oluşturun:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. Aşağıdaki komutu çalıştırarak depolama hesabı anahtarlarını listele:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Değerini kayıt `key1` aşağıdaki komutta depolama anahtarı olarak ardından değerini depolamak için komutu çalıştırın.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Adlı bir dosya paylaşımı oluşturma `baitshare` depolama hesabınızda aşağıdaki komutla:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. Sık kullandığınız metin düzenleyicinizde yük `settings.cfg` Azure Machine Learning Workbench projenin "Code" alt dizinden dosya ve depolama hesabı adını ve anahtarını gösterildiği gibi ekleyin. Kaydet ve Kapat `settings.cfg` dosya.
1. Zaten yapmadıysanız, indirme ve yükleme [AzCopy](https://aka.ms/downloadazcopy) yardımcı programı. AzCopy yürütülebilir "AzCopy" yazmaya ve belgelerini göstermek için Enter tuşuna basarak sistem yolunuzda olduğundan emin olun.
1. Örnek veri modelleri kullanan ve model eğitim betikleriniz, depolama hesabınıza uygun yerlere kopyalamak için aşağıdaki komutları yürütün:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Dosya aktarımı yaklaşık bir saat almak için bekler. Beklerken, aşağıdaki bölüme geçebilirsiniz: başka bir komut satırı arabirimi aracılığıyla Workbench'i açın ve geçici değişkenlerin yeniden tanımlamanız gerekebilir.

#### <a name="create-the-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

Bir HDInsight kümesi oluşturmak için önerilen yöntem, bu proje, "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" alt klasöründe bulunan HDInsight Spark kümesine resource manager şablonu kullanır.

1. HDInsight Spark kümesi bu projenin "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" alt "template.json" dosyayı şablonudur. Varsayılan olarak, şablon 40 çalışan düğümleri ile bir Spark kümesi oluşturur. Bu sayıyı ayarlamanız gerekir, şablonu, sık kullandığınız metin düzenleyicisinde açın ve "40" sürümünün tüm örneklerini dilediğiniz çalışan düğüm sayısı ile değiştirin.
    - Daha sonra seçtiğiniz çalışan düğümlerinin sayısını küçükse, bellek yetersiz hatalarla karşılaşabilirsiniz. Bellek hatalarını mücadele etmek için eğitim ve kullanıma hazır hale getirme betikleri kullanılabilir verilerin bir alt kümesi üzerinde bu belgenin sonraki bölümlerinde açıklandığı şekilde çalıştırabilirsiniz.
2. Benzersiz bir ad ve parola için HDInsight kümesi ve bunları yazma seçin aşağıdaki komutta belirtilen yerlerde: Ardından komutlar göndererek kümesi oluşturabilirsiniz:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Kümenizin dağıtım (sağlama ve betik eylemi yürütme dahil) en fazla 30 dakika sürebilir.

### <a name="set-up-batch-ai-resources"></a>Batch AI kaynaklarını ayarlama

Depolama hesabının dosya aktarımı ve Spark Küme dağıtımı için tamamlamak beklerken, Batch AI ağ dosya sunucusu (NFS) ve GPU kümesi hazırlayabilirsiniz. Bir Azure CLI komut istemi açın ve aşağıdaki komutu çalıştırın:

```
az --version 
```

Onaylayın `batchai` yüklü modülleri arasında listelenir. Aksi takdirde, farklı bir komut satırı arabirimi (örneğin, biri Workbench açık) kullanıyor olabilir.

Ardından, sağlayıcısı kaydı başarıyla tamamlandığını denetleyin. (Sağlayıcı kaydı en fazla beş dakika sürer ve hala son tamamlanan devam eden olabilir [Batch AI kurulum yönergelerini](https://github.com/Azure/BatchAI/tree/master/recipes).) Hem "Microsoft.Batch" ve "Microsoft.BatchAI" aşağıdaki komutun çıktısında "Kaydedildi" durumuyla görüntülendiğini doğrulayın:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Aksi durumda, aşağıdaki sağlayıcısı kayıt komutları çalıştırın ve kayıt tamamlanması yaklaşık 15 dakika bekleyin.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

Daha önce kaynak grubunu ve depolama hesabı oluşturma sırasında kullanılan değerlerle ayraçlı ifadeler değiştirmek için aşağıdaki komutları değiştirin. Ardından, değerler komutları yürüterek değişkenleri olarak depola:
```
az account set --subscription [subscription ID]
set AZURE_RESOURCE_GROUP=[resource group name]
set STORAGE_ACCOUNT_NAME=[storage account name]
set STORAGE_ACCOUNT_KEY=[storage account key]
az configure --defaults location=eastus
az configure --defaults group=%AZURE_RESOURCE_GROUP%
```

Azure Machine Learning projenizi içeren klasörü belirleyin (örneğin, `C:\Users\<your username>\AzureML\aerialimageclassification`). Köşeli parantez içindeki değeri klasör yolu (sondaki eğik çizgi yok ile) değiştirin ve komutu yürütün:
```
set PATH_TO_PROJECT=[The filepath of your project's root directory]
```
Bu öğretici için gerekli Batch AI kaynak oluşturmak artık hazırsınız.

#### <a name="prepare-the-batch-ai-network-file-server"></a>Batch AI ağ dosya sunucusu hazırlama

Eğitim verilerinizi bir ağ dosya sunucusunda, Batch AI kümesi erişir. Veri erişimi daha hızlı bir NFS ile bir Azure dosya paylaşımı veya Azure Blob depolama alanından dosyalara erişirken several-fold olabilir.

1. Bir ağ dosya sunucusu oluşturmak için aşağıdaki komutu yürütün:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p "Dem0Pa$$w0rd" --vm-size Standard_DS2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Aşağıdaki komutu kullanarak, ağ dosya sunucusu sağlama durumunu kontrol edin:

    ```
    az batchai file-server list
    ```

    Ağ dosya sunucusu "landuseclassifier" adlı "provisioningState" başarılı"," kullanıma hazırdır. Sağlama yaklaşık beş dakika sürmesi beklenir.
1. ("FileServerPublicIp" özelliği "mountSettings" altında) önceki komutun çıkışında, NFS IP adresini bulun. IP aşağıdaki komutta belirtilen yerlerde adres, sonra komutu yürüterek değeri depolamak yazma:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. En sevdiğiniz SSH aracı kullanarak (Aşağıdaki örnek komut kullandığı [PuTTY](http://www.putty.org/)), bu projenin yürütme `prep_nfs.sh` eğitim ve doğrulama görüntü aktarmak için NFS komut ayarlar vardır.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Veri indirme ve ayıklama devam eden güncelleştirmelerin shell penceresini arasında kadar hızlı bir şekilde materyali kaydırırsanız merak etmeyin.

İstenirse, veri aktarımı gibi en sevdiğiniz SSH aracı ile dosya sunucusu için oturum açmayı ve denetimi planlanmış proceeded onaylayabilirsiniz `/mnt/data` dizin içeriği. İki klasör training_images ve validation_images bulmanız gerekir, her göre land adlı alt klasörler ile içeren kategorileri kullanın.  Eğitim ve doğrulama kümeleri ~ 44 içermelidir k ve ~ 11 k görüntüleri, sırasıyla.

#### <a name="create-a-batch-ai-cluster"></a>Batch AI kümesi oluşturma

1. Aşağıdaki komutu gönderdikten tarafından küme oluşturun:

    ```
    az batchai cluster create -n landuseclassifier2 -u demoUser -p "Dem0Pa$$w0rd" --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 --storage-account-name %STORAGE_ACCOUNT_NAME% 
    ```

1. Kümenizi sağlama durumu kullanıcının denetlemek için aşağıdaki komutu kullanın:

    ```
    az batchai cluster list
    ```

    Ayırma durumu kümenin birimden olarak yeniden boyutlandırılıyor gelen değişiklikleri "landuseclassifier" adlı, işleri göndermek mümkündür. Ancak, işleri kümedeki tüm sanal makineler "Hazırlama" durumu bıraktıysanız verene kadar çalışmasını başlatmayın. Küme "hata" özelliği null değilse, küme oluşturma sırasında bir hata oluştu ve bu kullanılmamalıdır.

#### <a name="record-batch-ai-training-credentials"></a>Kayıt Batch AI eğitimi kimlik bilgileri

Küme ayırma için beklerken açın `settings.cfg` "Code" alt dizini bu projenin, tercih ettiğiniz metin düzenleyicisinde dosyasından. Aşağıdaki değişkenleri kendi kimlik bilgileriyle güncelleştirin:
- `bait_subscription_id` (36 karakterden oluşan Azure abonelik Kimliğinizi)
- `bait_aad_client_id` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Uygulama/istemci kimliği)
- `bait_aad_secret` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Uygulama gizli anahtarı)
- `bait_aad_tenant` ("Önkoşullar" bölümünde belirtilen Azure Active Directory Kiracı kimliği)
- `bait_region` (Bu makalenin yazıldığı tarih itibarıyla, tek seçenektir: eastus)
- `bait_resource_group_name` (daha önce seçtiğiniz kaynak grubu)

Bu değerleri atadıktan sonra aşağıdaki metni settings.cfg dosyanızın değiştirilen satırları benzemelidir:

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

Batch AI kaynak oluşturma komutları yürütüldüğü CLI penceresi artık kapatabilirsiniz. Bu öğreticideki tüm adımların Azure Machine Learning Workbench'ten başlatılan bir CLI kullanın.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Azure Machine Learning Workbench yürütme ortamını hazırlama

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>HDInsight kümesi, bir Azure Machine Learning Workbench işlem hedefi olarak kaydetme

HDInsight kümesi oluşturma işlemi tamamlandıktan sonra küme projeniz için işlem hedefi olarak şu şekilde kaydedin:

1.  Azure Machine Learning komut satırı arabiriminden aşağıdaki komutu yürütün:

    ```
    az ml computetarget attach cluster --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD%
    ```

    Bu komut iki dosya ekler `myhdi.runconfig` ve `myhdi.compute`, projenizin için `aml_config` klasör.

1. Açık `myhdi.compute` sık kullandığınız metin düzenleyicinizde dosya. Değiştirme `yarnDeployMode: cluster` okunacak satır `yarnDeployMode: client`, ardından dosyayı kaydedip kapatın.
1. Kullanılacak HDInsight ortamınızı hazırlamak için aşağıdaki komutu çalıştırın:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Yerel bağımlılıkları yükler

CLI, Azure Machine Learning Workbench uygulamasını açın ve aşağıdaki komutu gönderdikten ile yerel yürütme için gerekli bağımlılıkları yükler:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Veri edinme ve anlama

Bu senaryo genel kullanıma açık hava tanımayı verileri kullanan [Ulusal Tarım tanımayı Program](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) 1 ölçerin çözünürlükte. Biz kara etiket göre sıralanır ve özgün NAIP verilerden kırpılmış 224 piksel x 224 piksel PNG dosya kümelerini oluşturulmasını [Ulusal Land kapsayan veritabanı](https://www.mrlc.gov/data/references/national-land-cover-database-2011-nlcd2011). "Developed" etiketli bir örnek görüntü tam boyutuyla gösterilmektedir:

![Geliştirilmiş olan örnek kutucuk](media/scenario-aerial-image-classification/sample-tile-developed.png)

~ 44 sınıfı dengeli kümesi k ve 11 k görüntüleri kullanılan model eğitiminin ve doğrulama, sırasıyla. Biz döşemesini Middlesex ilçe, MA--giriş Microsoft'un yeni İngiltere araştırma ve geliştirme (NERD) merkezi model dağıtımı ~ 67 k görüntüye ayarlayın gösterir. Bu görüntü kümeleri nasıl oluşturulmuş daha fazla bilgi için bkz: [utandırıcı derecede paralel görüntü sınıflandırması git deposu](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Middlesex ilçe, Massachusetts konumu](media/scenario-aerial-image-classification/middlesex-ma.png)

Kurulumu sırasında oluşturduğunuz depolama hesabına Bu örnekte kullanılan havadan kümeleri aktarıldı. Eğitim, doğrulama ve kullanıma hazır hale getirme görüntüleri tüm 224 piksel x 224 piksel PNG metrekare başına bir pikselin çözünürlükte dosyalarıdır. Eğitim ve doğrulama görüntüleri kendi land kullanım etikete göre alt klasörler halinde. (Bilinmeyen land etiket kullanıma hazır hale getirme görüntülerin ve belirsiz; çoğu durumda bu görüntüleri bazıları birden fazla land türler bulunur.) Bu görüntü kümeleri nasıl oluşturulmuş daha fazla bilgi için bkz: [utandırıcı derecede paralel görüntü sınıflandırması git deposu](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

Örnek görüntüleri, Azure depolama hesabını (isteğe bağlı) görüntülemek için:
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
1. Ekranın üstündeki arama çubuğunda depolama hesabınızın adını arayın. Depolama hesabınızdaki arama sonuçlarını tıklayın.
2. Depolama hesabının ana bölmede "BLOB" bağlantısına tıklayın.
3. "Eğit" adlı kapsayıcıdaki tıklayın Dizinleri göre land adlı kullanmak listesini görmeniz gerekir.
4. İçerdiği görüntülerin listesini yüklemek için bu dizinleri birini tıklatın.
5. Herhangi bir görüntü tıklayın ve resmi görmek için indirin.
6. İsterseniz, "test" ve "middlesexma2016 de bunların içeriğini görüntülemek için" adlı kapsayıcılarında tıklayın.

## <a name="modeling"></a>Modelleme

### <a name="training-models-with-azure-batch-ai"></a>Azure Batch AI eğitim modeli

`run_batch_ai.py` Workbench projesini "Code\02_Modeling" alt klasöründe betik, bir Batch AI eğitimi işi göndermek için kullanılır. Bu işlem, bir görüntü sınıflandırıcı (AlexNet veya ResNet Imagenet üzerinde kullanan 18) kullanıcı tarafından seçmiş DNN retrains. Yeniden eğitme derinliği de belirtilebilir: ağ yalnızca son katmanı yeniden eğitme azaltmak overfitting eğitim örnek çalışırken, tüm ağ hassas ayar yapma (ya da AlexNet, tam olarak bağlı katmanları), kullanılabilir olduğunda daha büyük modeline yol açabilir Eğitim kümesi yeterince büyük olduğunda performans.

Eğitim işi tamamlandığında bu betik modelin (birlikte, modelin tamsayı çıkış ve dize etiketleri arasındaki eşlemeyi açıklayan bir dosya) ve Öngörüler blob depolamaya kaydeder. Eğitim dönemler üzerinde hata oranı iyileştirme timecourse ayıklanacak BAIT işin günlük dosyası ayrıştırılır. Hata oranı geliştirme timecourse AML Workbench'in çalıştırma geçmişi özelliği daha sonra günlüğe kaydedilir.

Eğitilen model kullanan bir model türünü ve yeniden eğitme derinliği için bir ad seçin. Yazma seçimlerinizi aşağıdaki komutta belirtilen yerlerde sonra bir Azure ML komut satırı arabiriminden komutu yürüterek yeniden eğitme başlatın:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Azure Machine Learning tamamlanması yaklaşık yarım saat gerçekleştirilecek çalıştırma bekler. Farklı yöntemleri ile geliştirilen modellerinin performansını karşılaştırabilmek birkaç benzer komutları (çıkış model adını kullanan bir model türü ve yeniden eğitme derinliği değişen) çalıştırmanızı öneririz.

### <a name="training-models-with-mmlspark"></a>MMLSpark eğitim modeli

`run_mmlspark.py` Workbench projesini "Code\02_Modeling" alt klasöründe betik eğitmek için kullanılan bir [MMLSpark](https://github.com/Azure/mmlspark) görüntü sınıflandırması için model. Komut dosyası birinci featurizes eğitim DNN kullanan bir görüntü sınıflandırıcı Imagenet veri (AlexNet veya 18-katman ResNet) kullanarak görüntüleri ayarlayın. Betik, görüntülerin özellikleri tespit sonra (rastgele bir orman ya da bir Lojistik regresyon modeli) görüntüleri sınıflandırmak için bir MMLSpark modeli eğitmek için kullanır. Test görüntü kümesi özellikleri tespit ise ve puanlanmış eğitilen modeli. Test kümesindeki modelin tahmin doğruluğunu hesaplanır ve Azure Machine Learning Workbench'in çalıştırma geçmişi özelliği günlüğe kaydedilir. Son olarak, eğitim MMLSpark modeli ve test kümesi üzerinde kendi Öngörüler BLOB depolamaya kaydedilir.

Eğitilen model kullanan bir model türünü ve bir MMLSpark model türü için bir benzersiz çıkış model adı seçin. Yazma seçimlerinizi aşağıdaki komut şablonunda belirtilen yerlerde sonra bir Azure ML komut satırı arabiriminden komutu yürüterek yeniden eğitme başlatın:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Ek bir `--sample_frac` parametre eğitme ve modeli kullanılabilir verilerin bir alt kümesi ile test etmek için kullanılabilir. Küçük örnek kesir kullanarak, çalışma zamanı ve bellek gereksinimleri doğruluktan eğitilen model azaltır. (Örneğin, bir çalıştırma ile `--sample_frac 0.1` yaklaşık 20 dakika sürmesi beklenir.) Bu ve diğer parametreler hakkında daha fazla bilgi için çalıştırma `python Code\02_Modeling\run_mmlspark.py -h`.

Kullanıcılar, bu betik, farklı giriş parametreleriyle birkaç kez çalıştırmak için önerilir. Performans elde edilen modelleri ardından Azure Machine Learning Workbench'in çalıştırma geçmişi özelliği karşılaştırılabilir.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Model performansını Workbench'i çalıştırma geçmişi özelliğini kullanarak karşılaştırma

İki yürüttünüz veya daha fazla eğitim herhangi bir türde çalışır sonra workbench'teki çalıştırma geçmişi özelliği boyunca sol menü çubuğundaki saat simgesine tıklayarak gidin. Seçin `run_mmlspark.py` betikleri sol konumunda listesinden. Tüm çalıştırmalar için test kümesi doğruluğunu karşılaştıran bir bölme yükler. Daha fazla ayrıntı görmek için aşağı kaydırın ve tek bir çalıştırmanın adına tıklayın.

## <a name="deployment"></a>Dağıtım

Eğitilen Modellerinizi birini havadan görüntüleri döşeme Middlesex ilçe, MA uzaktan yürütme kullanarak HDInsight üzerinde uygulamak için istenen modelinizin adı aşağıdaki komutu ekleyin ve yürütebilirsiniz:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Ek bir `--sample_frac` parametresi, kullanılabilir verilerin bir alt kümesi ile modeli hazır hale getirmek için kullanılabilir. Küçük örnek kesir kullanarak tahmin bütünlük çoğaltamaz çalışma zamanı ve bellek gereksinimlerini azaltır. Bu ve diğer parametreler hakkında daha fazla bilgi için çalıştırma `python Code\03_Deployment\batch_score_spark -h`.

Bu betik modelin Öngörüler depolama hesabınıza yazar. Sonraki bölümde açıklandığı gibi Öngörüler incelenebilir.

## <a name="visualization"></a>Görselleştirme

Workbench projesini "Code\04_Result_Analysis" alt "modeli tahmin analizi" Jupyter not defteri modeline ait tahminlerin görselleştirir. Yük ve Not Defteri gibi çalıştırın:
1. Workbench'te proje açın ve simgesi ("dosyalar") dizin listeleme yüklemek için sol taraftaki menüden boyunca klasörü tıklatın.
2. "Code\04_Result_Analysis" alt klasörüne gidin ve "Tahmin analizi Model" adlı not defterini tıklayın Not defterinin Önizleme işleme görüntülenmesi gerekir.
3. "Not Defteri sunucusu not defterini yüklenecek Başlat" tıklayın.
4. İlk hücrenin belirtilen yerlerde çözümlemek istediğiniz sonuçları model adını girin.
5. Tıklayın "hücre Çalıştır -> tüm" Not defterinde tüm hücreleri yürütülecek.
6. Analizler ve görselleştirmeler sunduğu hakkında daha fazla bilgi edinmek için Not defterini birlikte okuyun.

## <a name="cleanup"></a>Temizleme
Örnek tamamladıktan sonra Azure komut satırı arabiriminden aşağıdaki komutu çalıştırarak oluşturduğunuz kaynakların tümünü silmenizi öneririz:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Başvurular

- [Utandırıcı derecede paralel görüntü sınıflandırması depo](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Veri kümesi oluşturma yeridir görüntüleri ve etiketleri açıklar
- [MMLSpark](https://github.com/Azure/mmlspark) GitHub deposu
   - Model eğitimi ve değerlendirmesi MMLSpark ile ek örneklerini içerir

## <a name="conclusions"></a>Sonuçları

Azure Machine Learning Workbench, veri bilimcileri kodlarını uzak işlem hedeflerde kolayca dağıtın yardımcı olur. Bu örnekte, bir HDInsight kümesi üzerinde uzaktan yürütme yerel MMLSpark eğitim kod dağıtıldığı ve yerel bir betik bir Azure Batch AI, GPU kümesi üzerinde bir eğitim işi başlatıldı. Azure Machine Learning Workbench'in çalıştırma geçmişi özelliği, birden çok performans izlenir ve bize en doğru model belirlemenize yardımcı olmuştur. Workbench'in Jupyter not defterleri özellik bizim modelleri Öngörüler etkileşimli ve grafik bir ortamda görselleştirmenize yardımcı olmuştur.

## <a name="next-steps"></a>Sonraki adımlar
Bu örnek ile daha derine inin için:
- Azure Machine Learning Workbench'in çalıştırma geçmişi özelliği dişli simgeleri hangi ölçümleri ve grafikleri görüntülenen seçmek için tıklayın.
- Çağırma deyimleri için örnek betikler inceleyin `run_logger`. Her ölçümü nasıl kaydedilen anladığınızı denetleyin.
- Çağırma deyimleri için örnek betikler inceleyin `blob_service`. Nasıl eğitilen modelleri anlamak ve Öngörüler depolanır ve buluttan alınan denetleyin.
- Blob depolama hesabınızda oluşturulan kapsayıcı içeriğini keşfedin. Hangi komut anlamak veya komut dosyalarının her bir grup oluşturmak için sorumludur emin olun.
- Model hiperparametreleri değiştirmek için ya da farklı bir MMLSpark model türü eğitmek için eğitim betiği değiştirin. Değişikliklerinizi artan veya azalan modelin doğruluğunu belirlemek için çalıştırma geçmişi özelliğini kullanın.
