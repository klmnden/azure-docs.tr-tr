---
title: Azure Machine Learning hizmeti için model öğreticisi dağıtma
description: Bu eksiksiz öğreticide Azure Machine Learning hizmetinin uçtan uca nasıl kullanılacağı gösterilmektedir. Bu üçüncü bölümde dağıtım modeli ele alınmaktadır.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/13/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2eb6eb5090b0a68a189e2d4f1148d3238bc3ee0d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946621"
---
# <a name="tutorial-3-classify-iris-deploy-a-model"></a>Öğretici 3: Iris Sınıflandırma: Model dağıtma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning (önizleme) uzman veri bilimcilerine yönelik tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür. Veri bilimcileri bu çözümü kullanarak veri hazırlayabilir, denemeler geliştirebilir ve bulut ölçeğinde modeller dağıtabilir.

Bu öğretici **üç bölümden oluşan bir serinin üçüncü bölümüdür**. Öğreticinin bu bölümünde aşağıdakileri yapmak için Machine Learning (önizleme) kullanırsınız:

> [!div class="checklist"]
> * Model dosyasını bulma.
> * Puanlama betiği ve şema dosyası oluşturma.
> * Ortamı hazırlama.
> * Gerçek zamanlı bir web hizmeti oluşturma.
> * Gerçek zamanlı web hizmetini çalıştırma.
> * Çıktı blob verilerini inceleme. 

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
- Bu [hızlı başlangıçta](quickstart-installation.md) açıklandığı gibi, bir deneme hesabı ve Azure Machine Learning Workbench yüklenmiş olmalıdır
- [Öğreticinin 2. bölümündeki](tutorial-classifying-iris-part-2.md) sınıflandırma modeli
- Yerel ortamda Docker altyapısının yüklenmiş ve çalışıyor olması gerekir

## <a name="download-the-model-pickle-file"></a>Model pickle dosyasını indirme
Öğreticinin önceki bölümünde **iris_sklearn.py** betiği Machine Learning Workbench'te yerel olarak çalıştırılmıştı. Bu eylem lojistik regresyon modelini popüler Python nesne seri hale getirme paketi olan [pickle](https://docs.python.org/3/library/pickle.html) kullanarak seri hale getirmişti. 

1. Machine Learning Workbench uygulamasını açın. Sonra, öğretici serisinin önceki bölümlerinde oluşturduğunuz **myIris** projesini açın.

1. Proje açıldıktan sonra sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

1. **iris_sklearn.py** dosyasını seçin. Python kodu workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

1. **iris_sklearn.py** dosyasını gözden geçirerek pickle dosyasının oluşturulduğu yeri bulun. Ctrl+F kısayolunu seçerek **Bul** iletişim kutusunu açın ve sonra Python kodunda **pickle** sözcüğünü bulun.

   Bu kod parçacığı, pickle çıktı dosyasının nasıl oluşturulduğunu gösterir. Pickle çıktı dosyası diskte **model.pkl** olarak adlandırılır. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

1. Önceki bir çalıştırmanın çıktı dosyalarının arasında model pickle dosyasını bulun.
   
   **iris_sklearn.py** betiğini çalıştırdığınızda model dosyası **outputs** klasörüne **model.pkl** adıyla kaydedilmişti. Bu klasör yerel proje klasörünüzde değil betiği çalıştırmayı seçtiğiniz yürütme ortamında bulunur. 
   
   a. Dosyayı bulmak için sol bölmedeki **Çalıştırmalar** düğmesini (saat simgesi) seçerek **Tüm Çalıştırmalar** listesini açın. 

   b. **Tüm Çalıştırmalar** sekmesi açılır. Çalıştırmalar tablosunda hedefin **yerel**, betik adının ise **iris_sklearn.py** olduğu bir çalıştırmayı seçin. 

   c. **Çalıştırma Özellikleri** bölmesi açılır. Bölmenin sağ üst kısmındaki **Çıktılar** bölümünü inceleyin.

   d. Pickle dosyasını indirmek için **model.pkl** dosyasının yanındaki onay kutusunu işaretleyip **İndir**’i seçin. Dosyayı proje klasörünüzün kök dizinine kaydedin. Sonraki adımlarda bu dosya gereklidir.

   ![Pickle dosyasını indirme](media/tutorial-classifying-iris/download_model.png)

   `outputs` klasörü hakkında daha fazla bilgi için [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md) makalesine bakın.

## <a name="get-the-scoring-script-and-schema-files"></a>Puanlama betiği ve şema dosyalarını alma
Model dosyası ile birlikte web hizmetini dağıtmak için bir Puanlama komut dosyası da gerekir. İsteğe bağlı olarak, web hizmeti giriş verileri için bir şema gerekir. Puanlama betiği geçerli klasörde yer alan **model.pkl** dosyasını yükler ve yeni tahminler üretmek için kullanır.

1. Machine Learning Workbench uygulamasını açın. Sonra, öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

1. Proje açıldıktan sonra sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

1. **score_iris.py** dosyasını seçin. Python betiği açılır. Bu dosya puanlama dosyası olarak kullanılır.

   ![Puanlama dosyası](media/tutorial-classifying-iris/model_data_collection.png)

1. Şema dosyasını almak için betiği çalıştırın. Komut çubuğundan **yerel** ortamını ve **score_iris.py** betiğini seçip **Çalıştır** seçeneğini belirleyin. 

   Bu betik modelin ihtiyaç duyduğu giriş verileri şemasını alan **çıktılar** bölümünde bir JSON dosyası oluşturur.

1. **Proje Panosu** bölmesinin sağ tarafındaki **İşler** bölmesini inceleyin. En son **score_iris.py** işinin yeşil **Tamamlandı** durumunu göstermesini bekleyin. Ardından, çalıştırma ayrıntılarını görmek için en son iş çalıştırmasına ait **score_iris.py** bağlantısını seçin. 

1. **Çalıştırma Özellikleri** bölmesinin **Çıktılar** bölümünde yeni oluşturulan **service_schema.json** dosyasını seçin. Dosya adının yanındaki onay kutusunu ve sonra **İndir**’i seçin. Dosyayı projenizin kök klasörüne tıklayın.

1. **score_iris.py** betiğini açtığınız önceki sekmeye dönün. Veri koleksiyonu kullanarak, web hizmetinden model girişlerini ve tahminlerini alabilirsiniz. Aşağıdaki adımlar veri koleksiyonuyla yakından ilgilidir.

1. Model veri koleksiyonu işlevini içerdiği için **ModelDataCollector** dosya içeri aktarım sınıfının en üstündeki kodu gözden geçirin:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

1. **init()** işlevindeki **ModelDataCollector** örneği oluşturan aşağıdaki kod satırlarını gözden geçirin:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
    ```

1. **run(input_df)** işlevindeki giriş ve tahmin verilerini toplayan aşağıdaki kod satırlarını gözden geçirin:

    ```python
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

Artık ortamınızı modeli kullanıma hazır hale getirmeye hazırsınız.

## <a name="prepare-to-operationalize-locally-for-development-and-testing-your-service"></a>Yerel olarak operasyonel hale getirme [Hizmetinizi geliştirme ve test etme için]
Yerel bilgisayarınızdaki Docker kapsayıcılarında çalıştırmak için _yerel mod_ dağıtımını kullanın.

Geliştirme ve test için _yerel modu_ kullanabilirsiniz. Modeli hazır hale getirmek için aşağıdaki adımları tamamlamak üzere Docker motorunun yerel ortamda çalışıyor olması gerekir. Karşılık gelen yardım iletisini görüntülemek için her bir komutun sonunda `-h` bayrağını kullanabilirsiniz.

>[!NOTE]
>Yerel ortamınızda Docker altyapısı yoksa bile dağıtım için Azure’da bir küme oluşturarak işleme devam edebilirsiniz. Ücret ödemeye devam etmemek için öğreticiden sonra kümeyi silebilir veya yeniden kullanmak için tutabilirsiniz.

>[!NOTE]
>Yerel ortamda dağıtılan web hizmetleri, Azure Portal'ın hizmetler listesinde gösterilmez. Bunlar yerel makinede Docker’da çalıştırılır.

1. Komut satırı arabirimini (CLI) açın.
   Machine Learning Workbench uygulamasının **Dosya** menüsünde **Komut İstemini Aç**’ı seçin.

   Komut satırı istemi geçerli proje klasörünüzün bulunduğu **c:\temp\myIris>** konumunda açılır.


1. **Microsoft.ContainerRegistry** adlı Azure kaynak sağlayıcısının aboneliğinize kayıtlı olduğundan emin olun. 3. adımda bir ortam oluşturabilmeniz için önce bu kaynak sağlayıcısını kaydetmeniz gerekir. Aşağıdaki komutu kullanarak önceden kaydedilip kaydedilmediğini kontrol edebilirsiniz:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Şunun gibi bir çıktı görmeniz gerekir: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   **Microsoft.ContainerRegistry** kayıtlı değilse aşağıdaki komutu kullanarak kaydedebilirsiniz:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   Kayıt işlemi birkaç dakika sürebilir. Durumunu denetlemek için önceki **az provider list** komutunu veya aşağıdaki komutu kullanın:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   Çıktının üçüncü satırında **"registrationState": "Registering"** ifadesi gösterilir. Birkaç dakika bekleyin ve çıktıda **"registrationState": "Registered"** gösterilene kadar **show** komutunu tekrarlayın.

   >[!NOTE] 
   Bir ACS kümesine dağıtıyorsanız, aynı yaklaşımı kullanarak **Microsoft.ContainerService** kaynak sağlayıcısını da kaydetmeniz gerekir.

1. Ortamı oluşturun. Bu adımı her ortam için bir kez çalıştırmanız gerekir. Örneğin, geliştirme ortamı için bir kez ve üretim için bir kez çalıştırın. Bu ilk ortam için _yerel modu_ kullanın. Aşağıdaki komutta `-c` veya `--cluster` anahtarını kullanarak daha sonra _küme modunda_ bir ortam oluşturabilirsiniz.

   Aşağıdaki kurulum komutu için, abonelik üzerinde Katkıda Bulunan erişimine sahip olmanız gerekir. Buna sahip değilseniz, en azından içine dağıtım yaptığınız kaynak grubuna Katkıda Bulunan erişiminizin olması gerekir. İkinci durumda, kurulum komutunun içinde `-g` bayrağını kullanıp kaynak grubunu adını belirtmelisiniz. 

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Ekrandaki talimatları izleyerek Docker görüntülerini depolamak için bir depolama hesabı, Docker görüntülerini listeleyen bir Azure kapsayıcı kayıt defteri ve telemetri verilerini toplayan bir Azure Application Insights hesabı sağlayın. `-c` anahtarını kullanırsanız komut ek olarak bir Container Service kümesi oluşturur.
   
   Küme adından ortamı tanımlayabilirsiniz. Konumun, Azure portalından oluşturduğunuz Model Yönetimi hesabıyla aynı konumda olması gerekir.

   Ortamın başarıyla kurulduğundan emin olmak üzere durumu denetlemek için şu komutu kullanın:

   ```azurecli
   az ml env show -n <deployment environment name> -g <existing resource group name>
   ```

   5. adımda ortamı ayarlamanızdan önce “Hazırlama Durumu”nda “Başarılı” (gösterildiği gibi) değerinin olduğundan emin olun:

   ![Hazırlama Durumu](media/tutorial-classifying-iris/provisioning_state.png)
 
1. Bu öğreticinin önceki kısımlarında bir Model Yönetimi hesabı oluşturmadıysanız şimdi yapın. Tek seferlik bir kurulumdur.
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
1. Model Yönetimi hesabını ayarlayın.
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

1. Ortamı ayarlayın.

   Kurulum tamamlandıktan sonra aşağıdaki komutu kullanarak ortamı çalışır duruma getirmek için gereken ortam değişkenlerini ayarlayın. Daha önce 3. adımda kullandığınız ortam adının aynısını kullanın. Kurulum işlemi tamamlandıktan sonra komut penceresinde verilen kaynak grubu adının aynısını kullanın.

   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

1. Yerel web hizmeti dağıtımı için hazır hale getirme ortamınızı doğru şekilde yapılandırdığınızdan emin olmak için aşağıdaki komutu girin:

   ```azurecli
   az ml env show
   ```

Şimdi gerçek zamanlı web hizmetini oluşturmaya hazırsınız.

>[!NOTE]
>Sonraki web hizmeti dağıtımları için Model Yönetimi hesabınızı ve ortamınızı tekrar kullanabilirsiniz. Her web hizmeti için tekrar oluşturmanıza gerek yoktur. Bir hesap veya ortamla ilişkilendirilmiş birden fazla web hizmeti olabilir.

## <a name="create-a-real-time-web-service-in-one-command"></a>Tek komutla gerçek zamanlı bir web hizmeti oluşturma
1. Gerçek zamanlı bir web hizmeti oluşturmak için aşağıdaki komutu kullanın:

   ```azurecli
   az ml service create realtime -f score_iris.py --model-file model.pkl -s ./output/service_schema.json -n irisapp -r python --collect-model-data true -c aml_config\conda_dependencies.yml
   ```
   Bu komut daha sonra kullanabileceğiniz bir web hizmeti kimliği oluşturur. Dizüstü bilgisayar kullanılıyorsa çıkış dizinini atlayın.

   **az ml service create realtime** komutuyla aşağıdaki anahtarlar kullanılır:

   * `-f`: Puanlama betik dosyası adı.

   * `--model-file`: Model dosyası. Bu örnekte, serileştirilmiş model.pkl dosyası kullanılır.

   * `-s`: Hizmet şeması. Önceki adımda **score_iris.py** betiği yerel olarak çalıştırılarak bu şema oluşturulmuştu.

   * `-n`: Tamamı küçük harfli olması gereken uygulama adı.

   * `-r`: Modelin çalışma zamanı. Bu örnekte bir Python modelidir. Geçerli çalışma zamanları: `python` ve `spark-py`.

   * `--collect-model-data true`: Bu anahtar, veri koleksiyonunu etkinleştirir.

   * `-c`: Ek paketlerin belirtildiği conda bağımlılıkları dosyasının yolu.

   >[!IMPORTANT]
   >Hizmet adının (aynı zamanda yeni Docker görüntüsünün adıdır) tamamının küçük harf olması gerekir. Aksi takdirde bir hata alırsınız. 

1. Komutu çalıştırdığınızda model ve puanlama dosyaları, ortam kurulumu sırasında oluşturduğunuz depolama hesabına yüklenir. Dağıtım işlemi; modeliniz, şemanız ve içindeki puanlama dosyasıyla bir Docker görüntüsünü derleyip Azure kapsayıcı kayıt defterine iletir: **\<ACR_adı\>.azurecr.io/\<görüntü adı\>:\<sürüm\>**. 

   Komut daha sonra bu görüntüyü yerel bilgisayarınıza çeker ve bu görüntüyle bir Docker kapsayıcısı başlatır. Ortamınız küme modunda yapılandırılmışsa Docker kapsayıcısı Azure Cloud Services Kubernetes kümesine dağıtılır.

   Dağıtımın bir parçası olarak yerel makinenizde web hizmeti için bir HTTP REST uç noktası oluşturulur. Birkaç dakika sonra komutun başarıyla tamamlandığını belirten bir ileti görüntüler. Web hizmetiniz için eyleme hazırdır!

1. Çalışan Docker kapsayıcısını görmek için **docker ps** komutunu kullanın:

   ```azurecli
   docker ps
   ```

## <a name="optional-alternative-create-a-real-time-web-service-by-using-separate-commands"></a>[İsteğe bağlı alternatif] Ayrı komutlar kullanarak gerçek zamanlı bir web hizmeti oluşturma
Daha önce gösterilen **az ml service create realtime** komutunun bir alternatifi olarak, adımları ayrı ayrı da gerçekleştirebilirsiniz. 

İlk olarak modeli kaydedin. Sonra bildirimi oluşturun, Docker görüntüsünü derleyin ve web hizmetini oluşturun. Bu adım adım yaklaşım, her adımda daha fazla esneklik sağlar. Ayrıca, önceki adımlarda oluşturulan varlıkları tekrar kullanarak yalnızca ihtiyacınız olduğunda yeniden derleyebilirsiniz.

1. pickle dosyasının adını belirterek modeli kaydedin.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Bu komut bir model kimliği oluşturur.

1. Bir bildirim oluşturun.

   Bildirim oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen model kimliği çıktısını sağlayın:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s ./output/service_schema.json -c aml_config\conda_dependencies.yml
   ```
   Bu komut bir bildirim kimliği oluşturur.  Dizüstü bilgisayar kullanılıyorsa çıkış dizinini atlayın.

1. Bir Docker görüntüsü oluşturun.

   Docker görüntüsü oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen bildirim kimliği değeri çıktısını sağlayın. Ayrıca isteğe bağlı olarak `-c` anahtarını kullanarak conda bağımlılıklarını ekleyebilirsiniz.

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID> 
   ```
   Bu komut bir Docker görüntüsü kimliği oluşturur.
   
1. Hizmeti oluşturun.

   Hizmet oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen görüntü kimliği çıktısını sağlayın:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Bu komut bir web hizmeti kimliği oluşturur.

Şimdi web hizmetini çalıştırmaya hazırsınız.

## <a name="run-the-real-time-web-service"></a>Gerçek zamanlı web hizmetini çalıştırma

Çalışmakta olan **irisapp** web hizmetini test etmek için, dört rastgele sayıdan oluşan JSON kodlamalı bir kayıt kullanın.

1. Web hizmeti örnek veriler içerir. Yerel modda çalışırken **az ml service usage realtime** komutunu çağırabilirsiniz. Bu çağrı, hizmeti test etmek için kullanabileceğiniz örnek bir çalıştırma komutu alır. Çağrı ayrıca hizmeti kendi özel uygulamanıza dahil etmek için kullanabileceğiniz puanlama URL’sini alır.

   ```azurecli
   az ml service usage realtime -i <web service ID>
   ```

1. Hizmeti test etmek için döndürülen hizmet çalıştırma komutunu yürütün:
    
   ```azurecli
   az ml service run realtime -i <web service ID> -d '{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}'
   ```

   Çıktı, tahmin edilen sınıf olan **"Iris-setosa"** şeklindedir. (Elde ettiğiniz sonuç farklı olabilir.) 

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Toplanan verileri Azure Blob depolama alanında görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Depolama hesaplarınızı bulun. Bunun için **Tüm Hizmetler**'i seçin.

1. Arama kutusuna **Depolama hesapları**’nı girin ve ardından Enter düğmesini seçin.

1. **Depolama hesapları** arama kutusundan ortamınızla eşleşen **Depolama hesabı** kaynağını seçin. 

   > [!TIP]
   > Hangi depolama alanının kullanımda olduğunu belirlemek için:
   > 1. Machine Learning Workbench’i açın.
   > 1. Üzerinde çalıştığınız projeyi seçin.
   > 1. **Dosya** menüsünden bir komut satırı istemi açın.
   > 1. Komut satırı istemine `az ml env show -v` girin ve *storage_account* değerini denetleyin. Bu değer, depolama hesabınızın adıdır.

1. **Depolama hesabı** bölmesi açıldıktan sonra **Hizmetler** bölümünden **Bloblar**’ı seçin. **modeldata** adlı kapsayıcıyı bulun. 
 
   Herhangi bir veri görmüyorsanız, verilerin depolama hesabına dolmaya başladığını görmek için ilk web hizmeti isteğinden sonra 10 dakikaya kadar beklemeniz gerekebilir.

   Veriler aşağıdaki kapsayıcı yoluyla bloblara akar:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

1. Bu verileri Azure Blob depolamadan kullanabilirsiniz. Hem Microsoft yazılımlarını hem de aşağıdaki açık kaynak araçları kullanan çeşitli araçlar mevcuttur:

   * Machine Learning: CSV dosyasını veri kaynağı olarak ekleyerek CSV dosyasını açın.

   * Excel: Günlük CSV dosyalarını çalışma sayfası olarak açın.

   * [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Bloblardaki CSV verilerinden çekilen verilerle grafik oluşturun.

   * [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): CSV verilerini bir Hive tablosuna yükleyin ve doğrudan bloba SQL sorguları gönderin.

   * [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): CSV verilerinin büyük kısmıyla bir DataFrame oluşturun.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu üçüncü bölümünde Machine Learning kullanarak aşağıdakileri gerçekleştirmeyi öğrendiniz:
> [!div class="checklist"]
> * Model dosyasını bulma.
> * Puanlama betiği ve şema dosyası oluşturma.
> * Ortamı hazırlama.
> * Gerçek zamanlı bir web hizmeti oluşturma.
> * Gerçek zamanlı web hizmetini çalıştırma.
> * Çıktı blob verilerini inceleme. 

Bir eğitim betiğini çeşitli işlem ortamlarında başarıyla çalıştırdınız. Ayrıca Docker tabanlı web hizmeti aracılığıyla bir model oluşturdunuz, serileştirdiniz ve çalıştır duruma getirdiniz. 

Artık gelişmiş veri hazırlama adımlarına geçmeye hazırsınız:
> [!div class="nextstepaction"]
> [Öğretici 4: Gelişmiş veri hazırlama](tutorial-bikeshare-dataprep.md)
