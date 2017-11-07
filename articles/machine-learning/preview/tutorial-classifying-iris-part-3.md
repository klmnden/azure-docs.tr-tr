---
title: "Azure Machine Learning hizmetleri için model dağıtma (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu 3. bölümde model dağıtma ele alınmıştır."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/27/2017
ms.openlocfilehash: 048d734277f855086a48ad00a52b873adbf419b4
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="classifying-iris-part-3-deploy-a-model"></a>Iris sınıflandırma bölüm 3: Model dağıtma
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici üç bölümden oluşan bir serinin üçüncü bölümüdür. Öğreticinin bu bölümünde Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Model dosyasını bulma
> * Puanlama betiği ve şema dosyası oluşturma
> * Ortamı hazırlama
> * Gerçek zamanlı bir web hizmeti oluşturma
> * Gerçek zamanlı web hizmetini çalıştırma
> * Çıktı blob verilerini inceleme 

 Bu öğreticide kolaylık sağlamak için değişmeyen [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak macOS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici serisinin ilk iki bölümünü tamamlayın:

1. [Veri öğreticisini hazırlama](tutorial-classifying-iris-part-1.md) bölümündeki talimatları izleyerek Azure Machine Learning kaynaklarını oluşturun ve Azure Machine Learning Workbench uygulamasını yükleyin.

2. [Model derleme öğreticisi](tutorial-classifying-iris-part-2.md) adımlarını izleyerek Azure Machine Learning'de bir lojistik regresyon modeli oluşturun.

3. Yerel ortamda Docker altyapısının yüklenmiş ve çalışıyor olması gerekir. Alternatif olarak Azure'daki bir Azure Container Service kümesine dağıtım yapabilirsiniz.

## <a name="download-the-model-pickle-file"></a>Model pickle dosyasını indirme
Öğreticinin önceki bölümünde **iris_sklearn.py** betiği Azure Machine Learning Workbench'te yerel olarak çalıştırılmıştı. Bu eylem lojistik regresyon modelini popüler Python nesne seri hale getirme paketi olan **[pickle](https://docs.python.org/2/library/pickle.html)** kullanarak seri hale getirmişti. 

1. **Azure Machine Learning Workbench** uygulamasını başlatın ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra Azure Machine Learning Workbench uygulamasının sol taraftaki araç çubuğunda yer alan **Dosyalar** düğmesine (klasör simgesi) tıklayın.

3. **iris_sklearn.py** dosyasını seçin. Python kodu Workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

4. **iris_sklearn.py** dosyasını gözden geçirerek pickle dosyasının oluşturulduğu yeri bulun. Ctrl+F kısayolunu kullanarak bul iletişim kutusunu açın ve Python kodunda **pickle** kelimesini bulun.

   Bu kod parçacığı, pickle çıktı dosyasının nasıl oluşturulduğunu gösterir. Pickle çıktı dosyasının diskte **model.pkl** olarak adlandırıldığına dikkat edin. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Önceki bir çalıştırmanın çıktı dosyalarının arasında model pickle dosyasını bulun.
   
   **iris_sklearn.py** betiğini çalıştırdığınızda model dosyası **outputs** klasörüne **model.pkl** adıyla kaydedilmişti. Bu klasör yerel proje klasörünüzde değil betiği çalıştırmayı seçtiğiniz yürütme ortamında bulunur. 
   
   - Dosyayı bulmak için Azure Machine Learning Workbench uygulamasını kullanın ve sol araç çubuğunda yer alan **Çalıştırmalar** düğmesine (saat simgesi) tıklayarak **Tüm Çalıştırmalar** listesini açın.  
   - **Tüm Çalıştırmalar** sekmesi açılır. Çalıştırmalar tablosunda hedefin **yerel**, betik adının ise **iris_sklearn.py** olduğu bir çalıştırmayı seçin. 
   - **Çalıştırma Özellikleri** sayfası açılır. Sayfanın sağ üst kısmındaki **Çıktılar** bölümünü inceleyin. 
   - **model.pkl** dosyasının yanındaki onay kutusunu seçip **İndir** düğmesine tıklayarak pickle dosyasını indirin. Proje klasörünüzün kök dizinine kaydedin. Sonraki adımlar için gereklidir.

   ![pickle dosyasını indirme](media/tutorial-classifying-iris/download_model.png)

   **outputs** klasörü hakkında daha fazla bilgi için [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md) makalesine bakın.

## <a name="get-scoring-and-schema-files"></a>Puanlama ve şema dosyalarını alma
Web hizmetini model dosyasıyla birlikte dağıtmak için puanlama betiğine ve isteğe bağlı olarak web hizmeti giriş verileri için bir şemaya ihtiyacınız vardır. Puanlama betiği geçerli klasörde yer alan **model.pkl** dosyasını yükler ve yeni tahmin edilen Iris sınıfını üretmek için kullanır.  

1. **Azure Machine Learning Workbench** uygulamasını başlatın ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra Azure Machine Learning Workbench uygulamasının sol taraftaki araç çubuğunda yer alan **Dosyalar** düğmesine (klasör simgesi) tıklayın.

3. **iris_score.py** dosyasını seçin. Python betiği açılır. Bu dosya puanlama dosyası olarak kullanılır.

   ![Puanlama Dosyası](media/tutorial-classifying-iris/model_data_collection.png)

4. Şema dosyasını almak için betiği çalıştırın. Komut çubuğundan **yerel** ortamı ve **iris-score.py** betiğini seçip **Çalıştır** düğmesine tıklayın. 

5. Bu betik modelin ihtiyaç duyduğu giriş verileri şemasını alan **çıktılar** klasöründe bir JSON dosyası oluşturur.

6. Machine Learning Workbench penceresinin sağ tarafındaki İşler bölmesine dikkat edin. En son **iris-score.py** işinin yeşil **Tamamlandı** durumunu göstermesini bekleyin. Ardından **iris-score.py** çalıştırmasıyla ilgili çalıştırma ayrıntılarını görmek için en son iş çalıştırmasına ait **iris-score.py [1]** bağlantısına tıklayın. 

7. Çalıştırma Özellikleri sayfasının **Çıktılar** bölümünde yeni oluşturulan **service_schema.json** dosyasını seçin. Dosyayı **işaretleyin** ve **İndir**’e tıklayın. Dosyayı projenizin kök klasörüne tıklayın.

8. **iris-score.py** betiğini açtığınız önceki sekmeye dönün. 

   Web hizmetinden model girişlerini ve tahminlerini almanızı sağlayan veri koleksiyonuna dikkat edin. Aşağıdaki noktalar veri koleksiyonuyla yakından ilgilidir:

9. Dosyanın en üstünde yer alan ve model veri koleksiyonu işlevini içeren ModelDataCollector sınıfını içeri aktaran kodu gözden geçirin:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. **init()** işlevindeki ModelDataCollector örneği oluşturan aşağıdaki kod satırlarını gözden geçirin:

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

>[!NOTE]
>Modelleri dağıtmak için bir Azure aboneliğine sahip erişiminiz olması gerekir.

## <a name="prepare-to-operationalize-locally"></a>Yerel olarak kullanıma hazır hale getirme
Yerel bilgisayarınızdaki Docker kapsayıcılarında çalıştırmak için _yerel mod_ dağıtımını kullanın.

Geliştirme ve test için _yerel modu_ kullanabilirsiniz. Modeli hazır hale getirmek için aşağıdaki adımları tamamlamak üzere Docker motorunun yerel ortamda çalışıyor olması gerekir. Komut yardımı için komutlarınızın sonuna `-h` bayrağı ekleyebilirsiniz.

>[!NOTE]
>Yerel ortamınızda Docker altyapısı yoksa bile dağıtım için Azure’da bir küme oluşturarak işleme devam edebilirsiniz. Ücret ödemeye devam etmemek için öğreticiden sonra kümeyi silmeyi unutmayın.

1. Azure Machine Learning Workbench'te komut satırı arabirimini açın. Bunun için Dosya menüsünden **Komut İstemi Aç**'a tıklayın.

   Komut satırı istemi geçerli proje klasörünüzün bulunduğu **c:\temp\myIris>** konumunda açılır.

2. **Microsoft.ContainerRegistry** adlı Azure kaynak sağlayıcısının aboneliğinize kayıtlı olduğundan emin olun. 3. adımda bir ortam oluşturabilmeniz için bu kaynak sağlayıcısını kaydedin. Aşağıdaki komutu kullanarak önceden kaydedilip kaydedilmediğini kontrol edebilirsiniz:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Aşağıdakine benzer bir çıktı görmeniz gerekir: 
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
   Kayıt birkaç dakika sürebilir ve durumunu yukarıdaki **az provider list** komutuyla veya aşağıdaki komutla kontrol edebilirsiniz:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   Çıktının üçüncü satırında **"registrationState": "Registering"** ifadesi gösterilir. Birkaç dakika bekleyin ve çıktıda **"registrationState": "Registered"** gösterilene kadar show komutunu tekrarlayın.

3. Ortamı oluşturun. Bu adımın her ortam için bir kez çalıştırılması gerekir (örneğin, geliştirme ortamı için bir kez ve üretim için bir kez). Bu ilk ortam için _yerel modu_ kullanın. (Aşağıdaki komutta `-c` veya `--cluster` anahtarını kullanarak daha sonra _küme modunda_ bir ortam oluşturabilirsiniz.)

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Ekrandaki talimatları izleyerek Docker görüntülerini depolamak için bir depolama hesabı, Docker görüntülerini listelemek için bir ACR (Azure Container Registry) hesabı ve telemetri verilerini toplamak için bir AppInsight hesabı sağlayın. `-c` anahtarını kullandıysanız bir ACS (Azure Container Service) kümesi de oluşturulur.
   
   Küme adından ortamı tanımlayabilirsiniz. Konumun, Azure portalından oluşturduğunuz Model yönetim hesabıyla aynı konumda olması gerekir.

4. Model Yönetimi hesabı oluşturma (bu tek seferlik bir kurulumdur)  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Model Yönetimi hesabını ayarlama  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Ortamı ayarlayın.
Kurulum tamamlandıktan sonra aşağıdaki komutu kullanarak hazır hale getirmek için gerekli olan ortam değişkenlerini ayarlayın. Daha önce 4. adımda kullandığınız ortam adının aynısını kullanın. Kurulum işlemi tamamlandıktan sonra komut penceresinde verilen kaynak grubu adının aynısını kullanın.
   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Yerel web hizmeti dağıtımı için hazır hale getirme ortamınızı doğru şekilde yapılandırdığınızdan emin olmak için aşağıdaki komutu girin:

   ```azurecli
   az ml env show
   ```

Şimdi gerçek zamanlı web hizmetini oluşturmaya hazırsınız.

>[!NOTE]
>Sonraki web hizmeti dağıtımları için Model Yönetimi hesabınızı ve ortamınızı tekrar kullanabilirsiniz. Her web hizmeti için tekrar oluşturmanıza gerek yoktur. Bir hesap veya ortamla ilişkilendirilmiş birden fazla web hizmeti olabilir.

## <a name="create-a-real-time-web-service-in-one-command"></a>Tek komutla gerçek zamanlı bir web hizmeti oluşturma
1. Gerçek zamanlı bir web hizmeti oluşturmak için aşağıdaki komutu kullanın:

   ```azurecli
   az ml service create realtime -f iris-score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Bu komut daha sonra kullanabileceğiniz bir web hizmeti kimliği oluşturur.

   **az ml service create realtime** komutuyla aşağıdaki anahtarlar kullanılır:
   * -n: uygulama adı, tamamı küçük harfle yazılmalıdır.
   * -f: puanlama betik dosyası adı
   * --model-file: model dosyası, bu örnekte pickle model.pkl dosyası
   * -r: modelin türü, bu durumda bir Python modeli
   * --collect-model-data true: veri koleksiyonunu etkinleştirin

   >[!IMPORTANT]
   >Hizmet adının (aynı zamanda yeni Docker görüntüsünün adıdır) tamamının küçük harf olması gerekir, aksi halde hata alırsınız. 

2. Komutu çalıştırdığınızda model ve puanlama dosyası, ortam kurulumu sırasında oluşturduğunuz depolama hesabına yüklenir. Dağıtım işlemi; modeliniz, şemanız ve içindeki puanlama dosyasıyla bir Docker görüntüsünü derleyip ACR kayıt defterine iletir: **\<ACR_adı\>.azureacr.io/\<görüntüadı\>:\<sürüm\>**. 

   Ardından bu görüntüyü yerel bilgisayarınıza çeker ve bu görüntüyle bir Docker kapsayıcısı başlatır. Ortamınız küme modunda yapılandırılmışsa Docker kapsayıcısı ACS Kubernetes kümesine dağıtılır.

   Dağıtımın bir parçası olarak yerel makinenizde web hizmeti için bir HTTP REST uç noktası oluşturulur. Birkaç dakika sonra komutun başarıyla tamamlandığını belirten bir ileti görüntüler ve web hizmetiniz kullanıma hazır hale gelir!

3. Çalışan Docker kapsayıcısını **docker ps** komutunu kullanarak görebilirsiniz:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-using-separate-commands"></a>Ayrı komutlar kullanarak gerçek zamanlı bir web hizmeti oluşturma
Yukarıda gösterilen **az ml service create realtime** komutunun bir alternatifi olarak, adımları ayrı ayrı da gerçekleştirebilirsiniz. İlk olarak modeli kaydedin, sonra bildirimi oluşturun, Docker görüntüsünü derleyin ve web hizmetini oluşturun. Bu adım adım yaklaşım, her adımda daha fazla esneklik sağlar. Ayrıca, önceki adımda oluşturulan varlıkları tekrar kullanarak yalnızca ihtiyacınız olduğunda yeniden derleyebilirsiniz.

1. pickle dosyasının adını belirterek modeli kaydedin.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Bu komut bir model kimliği oluşturur.

2. Bildirim oluşturma

   Bildirim oluşturmak için bu komutu kullanın ve önceki adımdan gelen model kimliği çıktısını sağlayın:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f iris-score.py -r python -i <model ID> -s service_schema.json
   ```
   Bu komut bir bildirim kimliği oluşturur.

3. Docker görüntüsü oluşturma

   Docker görüntüsü oluşturmak için bu komutu kullanın ve önceki adımdan gelen bildirim kimliği değeri çıktısını sağlayın:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   Bu komut bir Docker görüntüsü kimliği oluşturur.
   
4. Hizmeti oluşturma

   Hizmeti oluşturmak için listelenen komutu kullanın ve önceki adımdan gelen görüntü kimliği çıktısını sağlayın:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Bu komut bir web hizmeti kimliği oluşturur.

Şimdi web hizmetini çalıştırmaya hazırsınız.

## <a name="run-the-real-time-web-service"></a>Gerçek zamanlı web hizmetini çalıştırma

Dört rastgele sayıdan oluşan bir diziyi içeren JSON kodlamalı bir kayıt ileterek, çalışan **irisapp** web hizmetini test edin.

1. Web hizmeti oluşturma adımları basit veriler içerir. Yerel modda çalışırken **az ml service show realtime** komutunu çağırabilirsiniz. Bu çağrı, hizmeti test etmeniz için yararlı olan örnek bir run komutu alır. Bu komut ayrıca hizmeti kendi özel uygulamanıza dahil etmek için kullanabileceğiniz puanlama URL’sini alır:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Hizmeti test etmek için döndürülen hizmet çalıştırma komutunu yürütün.

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Çıktı tahmin edilen sınıf olan **"2"** şeklinde olur. (Elde ettiğiniz sonuç farklı olabilir.) 

3. Hizmeti CLI dışından çalıştırmak isterseniz kimlik doğrulaması anahtarlarını edinmeniz gerekir:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Toplanan verileri Azure blob depolama alanında görüntüleyin

1. [Azure portal](https://portal.azure.com) oturum açın.

2. Depolama Hesaplarınızı bulun. Bunun için **Diğer Hizmetler**'e tıklayın

3. Arama kutusuna **Depolama hesapları** yazın ve **Enter** tuşuna basın

4. **Depolama hesapları** arama sayfasından ortamınızla eşleşen **Depolama hesabı** kaynağını seçin. 

   > [!TIP]
   > Kullanılan depolama hesabını belirlemek için: Azure Machine Learning Workbench'i açın, üzerinde çalıştığınız projeyi seçin ve **Dosya** menüsünden komut satırı istemi açın. Komut satırı isteminde `az ml env show -v` yazın ve *storage_account* değerini kontrol edin. Bu değer depolama hesabınızın adıdır

5. **Depolama hesabı** sayfası açıldıktan sonra sol taraftaki listeden **Kapsayıcılar** öğesine tıklayın. **modeldata** adlı kapsayıcıyı bulun. 
 
   Veri yoksa, verilerin depolama hesabına dolmaya başlaması için ilk web hizmeti isteğinden sonra en az 10 dakika bekleyin.

   Veriler aşağıdaki kapsayıcı yoluyla bloblara akar:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Bu verileri Azure bloblarından kullanabilirsiniz. Hem Microsoft yazılımlarını hem de aşağıdaki açık kaynak araçları kullanmaya yönelik çeşitli araçlar mevcuttur:

   - Azure ML Workbench: csv dosyasını veri kaynağı olarak ekleyerek Azure ML Workbench'te açın. 
   - Excel: Günlük csv dosyalarını çalışma sayfası olarak açın.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Bloblardaki csv verilerinden çekilen verilerle grafik oluşturun.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): csv verilerini bir Hive tablosuna yükleyin ve doğrudan bloba SQL sorgusu gönderin.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): csv verilerinin büyük kısmıyla bir veri çerçevesi oluşturun.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Sonraki Adımlar
Üç bölümden oluşan öğretici serisinin bu üçüncü bölümünde Azure Machine Learning hizmetlerini kullanarak aşağıdakileri gerçekleştirmeyi öğrendiniz:
> [!div class="checklist"]
> * Model dosyasını bulma
> * Puanlama betiği ve şema dosyası oluşturma
> * Ortamı hazırlama
> * Gerçek zamanlı bir web hizmeti oluşturma
> * Gerçek zamanlı web hizmetini çalıştırma
> * Çıktı blob verilerini inceleme 

Farklı işlem ortamlarında başarıyla bir eğitim betiği çalıştırdınız, bir model oluşturdunuz, modeli seri hale getirdiniz ve modeli Docker tabanlı web hizmetinden kullanıma hazır hale getirdiniz. 

Gelişmiş veri hazırlık adımlarına geçmeye hazırsınız:
> [!div class="nextstepaction"]
> [Gelişmiş veri hazırlama](tutorial-bikeshare-dataprep.md)

