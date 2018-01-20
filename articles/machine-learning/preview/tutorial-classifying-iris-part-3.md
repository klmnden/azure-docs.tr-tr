---
title: "Azure Machine Learning hizmetleri için model dağıtma (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu üçüncü bölümde dağıtım modeli ele alınmaktadır."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 11/29/2017
ms.openlocfilehash: 97cd46819a4547ec743270871bcb6b4eef3eb365
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="classify-iris-part-3-deploy-a-model"></a>Iris sınıflandırma bölüm 3: Model dağıtma
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerine yönelik tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür. Veri bilimcileri bu çözümü kullanarak veri hazırlayabilir, denemeler geliştirebilir ve bulut ölçeğinde modeller dağıtabilir.

Bu öğretici üç bölümden oluşan bir serinin üçüncü bölümüdür. Öğreticinin bu bölümünde Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Model dosyasını bulma.
> * Puanlama betiği ve şema dosyası oluşturma.
> * Ortamı hazırlama.
> * Gerçek zamanlı bir web hizmeti oluşturma.
> * Gerçek zamanlı web hizmetini çalıştırma.
> * Çıktı blob verilerini inceleme. 

 Bu öğreticide zamansız bir [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak Mac OS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici serisinin ilk iki bölümünü tamamlayın:

   * [Veri hazırlama öğreticisi](tutorial-classifying-iris-part-1.md) içindeki talimatları izleyerek Machine Learning kaynaklarını oluşturun ve Azure Machine Learning Workbench uygulamasını yükleyin.

   * [Model derleme öğreticisi](tutorial-classifying-iris-part-2.md) adımlarını izleyerek Azure Machine Learning'de bir lojistik regresyon modeli oluşturun.

Yerel ortamda bir Docker altyapısının yüklenmiş ve çalışıyor olması gerekir. Alternatif olarak Azure'daki bir Azure Container Service kümesine dağıtım yapabilirsiniz.

## <a name="download-the-model-pickle-file"></a>Model pickle dosyasını indirme
Öğreticinin önceki bölümünde **iris_sklearn.py** betiği Machine Learning Workbench'te yerel olarak çalıştırılmıştı. Bu eylem lojistik regresyon modelini popüler Python nesne seri hale getirme paketi olan [pickle](https://docs.python.org/2/library/pickle.html) kullanarak seri hale getirmişti. 

1. Machine Learning Workbench uygulamasını başlatın ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

3. **iris_sklearn.py** dosyasını seçin. Python kodu workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

4. **iris_sklearn.py** dosyasını gözden geçirerek pickle dosyasının oluşturulduğu yeri bulun. Ctrl+F kısayolunu seçerek **Bul** iletişim kutusunu açın ve sonra Python kodunda **pickle** sözcüğünü bulun.

   Bu kod parçacığı, pickle çıktı dosyasının nasıl oluşturulduğunu gösterir. Pickle çıktı dosyası diskte **model.pkl** olarak adlandırılır. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Önceki bir çalıştırmanın çıktı dosyalarının arasında model pickle dosyasını bulun.
   
   **iris_sklearn.py** betiğini çalıştırdığınızda model dosyası **outputs** klasörüne **model.pkl** adıyla kaydedilmişti. Bu klasör yerel proje klasörünüzde değil betiği çalıştırmayı seçtiğiniz yürütme ortamında bulunur. 
   
   - Dosyayı bulmak için sol bölmedeki **Çalıştırmalar** düğmesini (saat simgesi) seçerek **Tüm Çalıştırmalar** listesini açın.  
   - **Tüm Çalıştırmalar** sekmesi açılır. Çalıştırmalar tablosunda hedefin **yerel**, betik adının ise **iris_sklearn.py** olduğu bir çalıştırmayı seçin. 
   - **Çalıştırma Özellikleri** bölmesi açılır. Bölmenin sağ üst kısmındaki **Çıktılar** bölümünü inceleyin. 
   - Pickle dosyasını indirmek için **model.pkl** dosyasının yanındaki onay kutusunu işaretleyip **İndir** düğmesini seçin. Proje klasörünüzün kök dizinine kaydedin. Sonraki adımlarda bu dosya gereklidir.

   ![Pickle dosyasını indirme](media/tutorial-classifying-iris/download_model.png)

   `outputs` klasörü hakkında daha fazla bilgi için [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md) makalesine bakın.

## <a name="get-the-scoring-script-and-schema-files"></a>Puanlama betiği ve şema dosyalarını alma
Web hizmetini model dosyasıyla birlikte dağıtmak için puanlama betiğine ve isteğe bağlı olarak web hizmeti giriş verileri için bir şemaya ihtiyacınız vardır. Puanlama betiği geçerli klasörde yer alan **model.pkl** dosyasını yükler ve yeni tahmin edilen Iris sınıfını üretmek için kullanır.  

1. Azure Machine Learning Workbench uygulamasını başlatın ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

3. **score_iris.py** dosyasını seçin. Python betiği açılır. Bu dosya puanlama dosyası olarak kullanılır.

   ![Puanlama dosyası](media/tutorial-classifying-iris/model_data_collection.png)

4. Şema dosyasını almak için betiği çalıştırın. Komut çubuğundan **yerel** ortamını ve **score_iris.py** betiğini seçip **Çalıştır** düğmesine tıklayın. 

5. Bu betik modelin ihtiyaç duyduğu giriş verileri şemasını alan **çıktılar** bölümünde bir JSON dosyası oluşturur.

6. **Proje Panosu** bölmesinin sağ tarafındaki **İşler** bölmesini inceleyin. En son **score_iris.py** işinin yeşil **Tamamlandı** durumunu göstermesini bekleyin. Ardından, **score_iris.py** çalıştırmasıyla ilgili çalıştırma ayrıntılarını görmek için en son iş çalıştırmasına ait **score_iris.py [1]** bağlantısını seçin. 

7. **Çalıştırma Özellikleri** bölmesinin **Çıktılar** bölümünde yeni oluşturulan **service_schema.json** dosyasını seçin.  Dosya adının yanındaki onay kutusunu ve sonra **İndir**’i seçin. Dosyayı projenizin kök klasörüne tıklayın.

8. **score_iris.py** betiğini açtığınız önceki sekmeye dönün. Veri koleksiyonu kullanarak, web hizmetinden model girişlerini ve tahminlerini alabilirsiniz. Aşağıdaki adımlar veri koleksiyonuyla yakından ilgilidir.

9. Model veri koleksiyonu işlevini içerdiği için **ModelDataCollector** dosya içeri aktarım sınıfının en üstündeki kodu gözden geçirin:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. **init()** işlevindeki **ModelDataCollector** örneği oluşturan aşağıdaki kod satırlarını gözden geçirin:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. **run(input_df)** işlevindeki giriş ve tahmin verilerini toplayan aşağıdaki kod satırlarını gözden geçirin:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

Artık ortamınızı modeli kullanıma hazır hale getirmeye hazırsınız.



## <a name="prepare-to-operationalize-locally"></a>Yerel olarak kullanıma hazır hale getirme
Yerel bilgisayarınızdaki Docker kapsayıcılarında çalıştırmak için _yerel mod_ dağıtımını kullanın.

Geliştirme ve test için _yerel modu_ kullanabilirsiniz. Modeli hazır hale getirmek için aşağıdaki adımları tamamlamak üzere Docker motorunun yerel ortamda çalışıyor olması gerekir. Komut Yardımı için komutlarınızın sonuna `-h` bayrağı ekleyebilirsiniz.

>[!NOTE]
>Yerel ortamınızda Docker altyapısı yoksa bile dağıtım için Azure’da bir küme oluşturarak işleme devam edebilirsiniz. Ücret ödemeye devam etmemek için öğreticiden sonra kümeyi silmeyi unutmayın.

1. Komut satırı arabirimini (CLI) açın.
   Azure Machine Learning Workbench uygulamasının **Dosya** menüsünde **Komut İstemini Aç**’ı seçin.

   Komut satırı istemi geçerli proje klasörünüzün bulunduğu **c:\temp\myIris>** konumunda açılır.

2. Ortamı oluşturun. Bu adımı her ortam için bir kez çalıştırmanız gerekir. Örneğin, geliştirme ortamı için bir kez ve üretim için bir kez çalıştırın. Bu ilk ortam için _yerel modu_ kullanın. Aşağıdaki komutta `-c` veya `--cluster` anahtarını kullanarak daha sonra _küme modunda_ bir ortam oluşturabilirsiniz.

   Aşağıdaki kurulum komutu için, abonelik üzerinde Katkıda Bulunan erişimine sahip olmanız gerektiğini aklınızda bulundurun. Bu erişiminiz yoksa, en azından içine dağıtım yaptığınız kaynak grubu üzerinde Katkıda Bulunan erişime ihtiyacınız vardır. İkincisini yapmak için, kurulum komutunun içinde `-g` bayrağını kullanıp kaynak grubunu adını belirtmelisiniz. 

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Ekrandaki talimatları izleyerek Docker görüntülerini depolamak için bir depolama hesabı, Docker görüntülerini listeleyen bir Azure kapsayıcı kayıt defteri ve telemetri verilerini toplayan bir AppInsight hesabı sağlayın. `-c` anahtarını kullandıysanız bir Azure Container Service kümesi de oluşturulur.
   
   Küme adından ortamı tanımlayabilirsiniz. Konumun, Azure portalından oluşturduğunuz Model Yönetimi hesabıyla aynı konumda olması gerekir.

3. Bir Model Yönetimi hesabı oluşturun. (Tek seferlik bir kurulumdur.)  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
4. Model Yönetimi hesabını ayarlayın.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

5. Ortamı ayarlayın.

   Kurulum tamamlandıktan sonra aşağıdaki komutu kullanarak ortamı çalışır duruma getirmek için gereken ortam değişkenlerini ayarlayın. 2. adımda, daha önce kullandığınız aynı ortam adı kullanın. Kurulum işlemi tamamlandıktan sonra komut penceresinde verilen kaynak grubu adının aynısını kullanın.

   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

6. Yerel web hizmeti dağıtımı için hazır hale getirme ortamınızı doğru şekilde yapılandırdığınızdan emin olmak için aşağıdaki komutu girin:

   ```azurecli
   az ml env show
   ```

Şimdi gerçek zamanlı web hizmetini oluşturmaya hazırsınız.

>[!NOTE]
>Sonraki web hizmeti dağıtımları için Model Yönetimi hesabınızı ve ortamınızı tekrar kullanabilirsiniz. Her web hizmeti için tekrar oluşturmanıza gerek yoktur. Bir hesap veya ortamla ilişkilendirilmiş birden fazla web hizmeti olabilir.

## <a name="create-a-real-time-web-service-in-one-command"></a>Tek komutla gerçek zamanlı bir web hizmeti oluşturma
1. Gerçek zamanlı bir web hizmeti oluşturmak için aşağıdaki komutu kullanın:

   ```azurecli
   az ml service create realtime -f score_iris.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true -c aml_config\conda_dependencies.yml
   ```
   Bu komut daha sonra kullanabileceğiniz bir web hizmeti kimliği oluşturur.

   **az ml service create realtime** komutuyla aşağıdaki anahtarlar kullanılır:
   * `-n`: Tamamı küçük harfli olması gereken uygulama adı.
   * `-f`: Puanlama betik dosyası adı.
   * `--model-file`: Model dosyası. Bu örnekte, serileştirilmiş model.pkl dosyası kullanılır.
   * `-r`: Modelin türü. Bu örnekte bir Python modelidir.
   * `--collect-model-data true`: Veri koleksiyonunu etkinleştirir.
   * `-c`: Ek paketlerin belirtildiği conda bağımlılıkları dosyasının yolu.

   >[!IMPORTANT]
   >Hizmet adının (aynı zamanda yeni Docker görüntüsünün adıdır) tamamının küçük harf olması gerekir. Aksi takdirde bir hata alırsınız. 

2. Komutu çalıştırdığınızda model ve puanlama dosyası, ortam kurulumu sırasında oluşturduğunuz depolama hesabına yüklenir. Dağıtım işlemi; modeliniz, şemanız ve içindeki puanlama dosyasıyla bir Docker görüntüsünü derleyip Azure kapsayıcı kayıt defterine iletir: **\<ACR_name\>.azureacr.io/\<imagename\>:\<version\>**. 

   Komut daha sonra bu görüntüyü yerel bilgisayarınıza çeker ve bu görüntüyle bir Docker kapsayıcısı başlatır. Ortamınız küme modunda yapılandırılmışsa Docker kapsayıcısı Azure Cloud Services Kubernetes kümesine dağıtılır.

   Dağıtımın bir parçası olarak yerel makinenizde web hizmeti için bir HTTP REST uç noktası oluşturulur. Birkaç dakika sonra komutun başarıyla tamamlandığını belirten bir ileti görüntüler ve web hizmetiniz kullanıma hazır hale gelir.

3. Çalışan Docker kapsayıcısını görmek için **docker ps** komutunu kullanın:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Ayrı komutlar kullanarak gerçek zamanlı bir web hizmeti oluşturma
Daha önce gösterilen **az ml service create realtime** komutunun bir alternatifi olarak, adımları ayrı ayrı da gerçekleştirebilirsiniz. 

İlk olarak modeli kaydedin. Sonra bildirimi oluşturun, Docker görüntüsünü derleyin ve web hizmetini oluşturun. Bu adım adım yaklaşım, her adımda daha fazla esneklik sağlar. Ayrıca, önceki adımda oluşturulan varlıkları tekrar kullanarak yalnızca ihtiyacınız olduğunda yeniden derleyebilirsiniz.

1. pickle dosyasının adını belirterek modeli kaydedin.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Bu komut bir model kimliği oluşturur.

2. Bir bildirim oluşturun.

   Bildirim oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen model kimliği çıktısını sağlayın:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
   ```
   Bu komut bir bildirim kimliği oluşturur.

3. Bir Docker görüntüsü oluşturun.

   Docker görüntüsü oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen bildirim kimliği değeri çıktısını sağlayın. Ayrıca isteğe bağlı olarak `-c` anahtarını kullanarak conda bağımlılıklarını ekleyebilirsiniz.

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID> -c amlconfig\conda_dependencies.yml
   ```
   Bu komut bir Docker görüntüsü kimliği oluşturur.
   
4. Hizmeti oluşturun.

   Hizmet oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen görüntü kimliği çıktısını sağlayın:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Bu komut bir web hizmeti kimliği oluşturur.

Şimdi web hizmetini çalıştırmaya hazırsınız.

## <a name="run-the-real-time-web-service"></a>Gerçek zamanlı web hizmetini çalıştırma

Çalışmakta olan **irisapp** web hizmetini test etmek için, dört rastgele sayıdan oluşan JSON kodlamalı bir kayıt kullanın:

1. Web hizmeti örnek veriler içerir. Yerel modda çalışırken **az ml service usage realtime** komutunu çağırabilirsiniz. Bu çağrı, hizmeti test etmek için kullanılması yararlı olan örnek bir run komutu alır. Çağrı ayrıca hizmeti kendi özel uygulamanıza dahil etmek için kullanabileceğiniz puanlama URL’sini alır:

   ```azurecli
   az ml service usage realtime -i <web service ID>
   ```

2. Hizmeti test etmek için döndürülen hizmet çalıştırma komutunu yürütün:

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Çıktı tahmin edilen sınıf olan **"2"** şeklinde olur. (Elde ettiğiniz sonuç farklı olabilir.) 

3. Hizmeti CLI dışından çalıştırmak için kimlik doğrulaması anahtarlarını edinmeniz gerekir:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Toplanan verileri Azure Blob depolama alanında görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesaplarınızı bulun. Bunun için **Diğer Hizmetler**'i seçin.

3. Arama kutusuna **Depolama hesapları**’nı girin ve ardından **Enter** düğmesini seçin.

4. **Depolama hesapları** arama kutusundan ortamınızla eşleşen **Depolama hesabı** kaynağını seçin. 

   > [!TIP]
   > Hangi depolama alanının kullanımda olduğunu belirlemek için:
   > 1. Azure Machine Learning Workbench’i açın.
   > 2. Üzerinde çalıştığınız projeyi seçin.
   > 3. **Dosya** menüsünden bir komut satırı istemi açın.
   > 4. Komut satırı istemine `az ml env show -v` girin ve *storage_account* değerini denetleyin. Bu değer, depolama hesabınızın adıdır.

5. **Depolama hesabı** bölmesi açıldıktan sonra soldaki listeden **Kapsayıcılar**’ı seçin. **modeldata** adlı kapsayıcıyı bulun. 
 
   Herhangi bir veri görmüyorsanız, verilerin depolama hesabına dolmaya başladığını görmek için ilk web hizmeti isteğinden sonra 10 dakikaya kadar beklemeniz gerekebilir.

   Veriler aşağıdaki kapsayıcı yoluyla bloblara akar:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Bu verileri Azure Blob depolamadan kullanabilirsiniz. Hem Microsoft yazılımlarını hem de aşağıdaki açık kaynak araçları kullanan çeşitli araçlar mevcuttur:

   - Azure Machine Learning: CSV dosyasını veri kaynağı olarak ekleyerek CSV dosyasını açın. 
   - Excel: Günlük CSV dosyalarını çalışma sayfası olarak açın.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Bloblardaki CSV verilerinden çekilen verilerle grafik oluşturun.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): CSV verilerini bir Hive tablosuna yükleyin ve doğrudan bloba SQL sorgusu gönderin.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): CSV verilerinin büyük kısmıyla bir veri çerçevesi oluşturun.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu üçüncü bölümünde Azure Machine Learning hizmetlerini kullanarak aşağıdakileri gerçekleştirmeyi öğrendiniz:
> [!div class="checklist"]
> * Model dosyasını bulma.
> * Puanlama betiği ve şema dosyası oluşturma.
> * Ortamı hazırlama.
> * Gerçek zamanlı bir web hizmeti oluşturma.
> * Gerçek zamanlı web hizmetini çalıştırma.
> * Çıktı blob verilerini inceleme. 

Farklı işlem ortamlarında başarıyla bir eğitim betiği çalıştırdınız, bir model oluşturdunuz, modeli seri hale getirdiniz ve modeli Docker tabanlı web hizmetinden kullanıma hazır hale getirdiniz. 

Artık gelişmiş veri hazırlama adımlarına geçmeye hazırsınız:
> [!div class="nextstepaction"]
> [Gelişmiş veri hazırlama](tutorial-bikeshare-dataprep.md)
