---
title: Azure Machine Learning hizmetleri için model öğreticisi oluşturma (önizleme) | Microsoft Docs
description: Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu ikinci bölümde deneme konusu ele alınmaktadır.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/15/2018
ms.openlocfilehash: 77dcad0f3e49b601110f8700245aaf479bde1c4e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722788"
---
# <a name="tutorial-2-classify-iris---build-a-model"></a>Öğretici 2: Iris Sınıflandırma: Model derleme
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik bir veri bilimi ve gelişmiş bir analiz çözümüdür.

Bu öğretici **üç bölümden oluşan bir serinin ikinci bölümüdür**. Öğreticinin bu bölümünde Azure Machine Learning hizmetlerini kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Betikleri açma ve kodları gözden geçirme
> * Betikleri yerel bir ortamda yürütme
> * Çalıştırma geçmişlerini gözden geçirme
> * Betikleri yerel bir Azure CLI penceresinde yürütme
> * Betikleri yerel bir Docker ortamında yürütme
> * Betikleri uzak bir Docker ortamında yürütme
> * Betikleri bulut üzerindeki Azure HDInsight ortamında yürütme

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
- Bu [hızlı başlangıçta](../service/quickstart-installation.md) açıklandığı gibi, bir deneme hesabı ve Azure Machine Learning Workbench yüklenmiş olmalıdır
- Proje ve [Öğretici bölüm 1](tutorial-classifying-iris-part-1.md)’de hazırlanan Iris verileri
- Yüklü ve yerel ortamda çalışan bir Docker altyapısı. Docker Community Edition yeterlidir. Docker yükleme hakkında buradan bilgi alın: https://docs.docker.com/engine/installation/.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>iris_sklearn.py dosyasını ve yapılandırma dosyalarını gözden geçirme

1. Azure Machine Learning Workbench uygulamasını başlatın.

1. [Öğretici serisinin 1. Bölümünde](tutorial-classifying-iris-part-1.md) oluşturduğunuz **myIris** projesini açın.

2. Açılan projede en sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

   ![Azure Machine Learning Workbench projesini açma](media/tutorial-classifying-iris/2-project-open.png)

3. **iris_sklearn.py** Python betik dosyasını seçin. 

   ![Betik seçme](media/tutorial-classifying-iris/2-choose-iris_sklearn.png)

   Kod, Workbench içinde yeni bir metin düzenleyici sekmesinde açılır. Öğreticinin bu bölümü boyunca bu betiği kullanacaksınız. 

   >[!NOTE]
   >Bu basit proje sıklıkla güncelleştirildiğinden, gördüğünüz projeler yukarıdaki kodla aynı olmayabilir.
   
   ![Dosya açma](media/tutorial-classifying-iris/open_iris_sklearn.png)

4. Python betik kodunu inceleyerek kodlama stili hakkında bilgi edinin. 

   **iris_sklearn.py** betiği aşağıdaki görevleri gerçekleştirir:

   * **iris.dprep** adlı varsayılan veri hazırlama paketini yükleyerek bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) oluşturun. 

   * Sorunun çözülmesini zorlaştırmak için rastgele özellikler ekler. Iris yaklaşık %100 doğrulukla kolayca sınıflandırılan küçük bir veri kümesi olduğundan rastgele hale getirilmesi gerekir.

   * [scikit-learn](http://scikit-learn.org/stable/index.html) makine öğrenimi kitaplığını kullanarak bir lojistik regresyon modeli derler.  Bu kitaplık varsayılan olarak Azure Machine Learning Workbench ile birlikte gelir.

   * [Pickle](https://docs.python.org/3/library/pickle.html) kitaplığını kullanarak modeli `outputs` klasöründeki dosyada seri hale getirir. 
   
   * Serileştirilmiş modeli yükler ve sonra bellekte tekrar seri durumdan çıkarır.

   * Seri durumdan çıkarılmış modeli kullanarak yeni kayıtla ilgili bir tahminde bulunur. 

   * [matplotlib](https://matplotlib.org/) kitaplığını kullanarak iki grafik, bir karışıklık matrisi ve bir çok sınıflı alıcı çalıştırma özellikleri (ROC) eğrisi çizer ve sonra bunları `outputs` klasörüne kaydeder. Henüz yoksa bu kitaplığı ortamınıza yükleyebilirsiniz.

   * Çalıştırma geçmişindeki düzenli hale getirme hızını ve model doğruluğunu otomatik olarak çizer. Düzenleme hızını kaydetme ve doğruluğu günlüklerde modelleme işlemi boyunca `run_logger` nesnesi kullanılır. 


## <a name="run-irissklearnpy-in-your-local-environment"></a>iris_sklearn.py dosyasını yerel ortamınızda çalıştırma

1. Azure Machine Learning komut satırı arabirimini (CLI) başlatın:
   1. Azure Machine Learning Workbench’i başlatın.

   1. Workbench menüsünden **Dosya** > **Komut İstemini Aç**’ı seçin. 
   
   Azure Machine Learning komut satırı arabirimi (CLI) penceresi, Windows üzerindeki `C:\Temp\myIris\>` proje klasöründe başlatılır. Bu proje, öğreticinin 1. Bölümünde oluşturduğunuz projeyle aynıdır.

   >[!IMPORTANT]
   >Sonraki adımları gerçekleştirmek için bu CLI penceresini kullanmanız gerekir.

1. Henüz kitaplığınız yoksa, CLI penceresinde **matplotlib** adlı Python çizim kitaplığını yükleyin.

   **iris_sklearn.py** betiği iki Python paketine bağımlıdır: **scikit-learn** ve **matplotlib**.  Size kolaylık olması için **scikit-learn** paketi Azure Machine Learning Workbench’te yüklüdür. Ancak, henüz yüklemediyseniz **matplotlib** betiğini yüklemeniz gerekir.

   **matplotlib** yüklemeden devam ederseniz, bu öğreticideki kod hala başarılı bir şekilde çalışabilir. Ancak, kod geçmiş görselleştirmelerinde gösterilen şekilde karışıklık matrisi çıktısını ve çok sınıflı ROC eğrisi çizimlerini oluşturamaz.

   ```azurecli
   pip install matplotlib
   ```

   Bu yükleme yaklaşık bir dakika sürer.

1. Workbench uygulamasına dönün. 

1. **iris_sklearn.py** adlı sekmeyi bulun. 

   ![Betiği içeren sekmeyi bulun](media/tutorial-classifying-iris/2-iris_sklearn-tab.png)

1. Bu sekmenin araç çubuğunda yürütme ortamı olarak **yerel**’i ve çalıştırılacak betik olarak `iris_sklearn.py` öğesini seçin. Bunlar zaten seçilmiş olabilir.

   ![Yerel ve betik seçeneği](media/tutorial-classifying-iris/2-local-script.png)

1. Araç çubuğunun sağ tarafına giderek **Bağımsız Değişkenler** alanına `0.01` değerini girin. 

   Bu değer mantıksal regresyon modelinin düzenli hale getirme hızına karşılık gelir.

   ![Yerel ve betik seçeneği](media/tutorial-classifying-iris/2-local-script-arguments.png)

1. **Çalıştır** düğmesini seçin. Anında bir iş zamanlanır. İş, workbench penceresinin sağ tarafındaki **İşler** bölmesinde listelenir. 

   ![Yerel ve betik seçeneği](media/tutorial-classifying-iris/2-local-script-arguments-run.png)

   Bir süre sonra işin **Gönderiliyor** olan durumu **Çalışıyor** ve sonunda **Tamamlandı** olarak değişir.

1. **İşler** bölmesindeki iş durumu metninde **Tamamlandı** öğesini seçin. 

   ![sklearn çalıştırma](media/tutorial-classifying-iris/2-completed.png)

   Bir pencere açılır ve çalıştırmanın standart çıktı (stdout) metni görüntülenir. stdout metnini kapatmak için açılır pencerenin sağ üst kısmındaki **Kapat** (**x**) düğmesini seçin.

   ![Standart çıktı](media/tutorial-classifying-iris/2-standard-output.png)

9. **İşler** bölmesindeki aynı iş durumunda **Tamamlandı** durumunun ve başlangıç zamanının hemen üzerinde yer alan mavi renkli **iris_sklearn.py [n]** (_n_ çalıştırma numarasıdır) metnini seçin. **Çalıştırma Özellikleri** penceresi açılır ve bu çalıştırma için aşağıdaki bilgileri gösterir:
   - **Çalıştırma Özellikleri** bilgileri
   - **Çıkışlar**
   - **Ölçümler**
   - Varsa **Görselleştirmeler**
   - **Günlükler** 

   Çalıştırma tamamlandığında açılır pencerede aşağıdaki sonuçlar gösterilir:

   >[!NOTE]
   >Öğretici daha önce eğitim kümesine bazı rastgele seçimler eklediği için tam sonuçlarınız burada gösterilen sonuçlardan farklı olabilir.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```
    
10. **Çalıştırma Özellikleri** sekmesini kapatın ve **iris_sklearn.py** sekmesine dönün. 

11. Diğer çalıştırmalar için tekrarlayın. 

    **Bağımsız Değişkenler** alanına `0.001` ile `10` arasında bir dizi değer girin. **Çalıştır**'ı seçerek kodu birkaç kez daha yürütün. Değiştirdiğiniz bağımsız değişken değeri koddaki mantıksal regresyon modeline iletilir ve her seferinde farklı bulgular alınır.

## <a name="review-the-run-history-in-detail"></a>Çalıştırma geçmişinin ayrıntılarını gözden geçirme
Azure Machine Learning Workbench'te yürütülen her betik çalıştırma geçmişi kaydına dahil edilir. **Çalıştırmalar** görünümünü açarsanız belirli bir betiğin çalıştırma geçmişini görüntüleyebilirsiniz.

1. **Çalıştırmalar** listesini açmak için sol araç çubuğundaki **Çalıştırmalar** düğmesini (saat simgesi) seçin. Ardından **iris_sklearn.py** öğesini seçerek `iris_sklearn.py` öğesinin **Çalıştırma Panosunu** açın.

   ![Çalıştırma görünümü](media/tutorial-classifying-iris/run_view.png)

1. **Çalıştırma Panosu** sekmesi açılır. 

   Birden fazla çalıştırma sonucunda toplanan istatistikleri gözden geçirin. Graflar sekmenin üstünde işlenir. Her çalıştırma ardışık bir numaraya sahiptir ve çalıştırma ayrıntıları ekranın altındaki tabloda listelenir.

   ![Çalıştırma panosu](media/tutorial-classifying-iris/run_dashboard.png)

1. Tabloyu filtreleyip herhangi bir grafiğe tıklayarak her bir çalıştırmanın durumunu, süresini, kesinliğini ve kurallaştırma oranını görüntüleyebilirsiniz. 

1. **Çalıştırmalar** tablosundaki iki veya daha fazla çalıştırmanın yanında bulunan onay kutularını seçin. Ayrıntılı bir karşılaştırma bölmesi açmak için **Karşılaştır** düğmesini seçin. Yan yana karşılaştırmayı gözden geçirin. 

1. **Çalıştırma Panosu**’na geri dönmek için **Karşılaştırma** bölmesinin sol üst kısmındaki **Çalıştırma Listesi** geri düğmesini seçin.

   ![Çalıştırma listesine dönme](media/tutorial-classifying-iris/2-compare-back.png)

1. Ayrıntılarını görmek için çalıştırmalardan birini seçin. Seçilen çalıştırmanın istatistikleri **Çalıştırma Özellikleri** bölümünde listelenir. Çıktı klasörüne yazılmış olan dosyalar **Çıktılar** bölümünde listelenir ve oradan indirilebilir.

   ![Çalıştırma ayrıntıları](media/tutorial-classifying-iris/run_details.png)

   Karışıklık matrisi ve çok sınıflı ROC eğrisi çizimleri **Görselleştirmeler** bölümünde oluşturulur. Tüm günlük dosyalarına **Günlükler** bölümünden de ulaşabilirsiniz.


## <a name="run-scripts-in-local-docker-environments"></a>Betikleri yerel Docker ortamlarında yürütme

İsteğe bağlı olarak betikleri yerel Docker kapsayıcısıyla çalıştırabilirsiniz. Docker gibi ek yürütme ortamlarını yapılandırabilir ve betiğinizi bu ortamlarda çalıştırabilirsiniz. 

>[!NOTE]
>Bir betiği uzak Azure VM'sindeki veya Azure HDInsight Spark kümesindeki Docker kapsayıcısına göndermek için [Ubuntu tabanlı Azure Veri Bilimi Sanal Makinesi veya HDInsight kümesi oluşturma yönergelerini](how-to-create-dsvm-hdi.md) inceleyebilirsiniz.

1. Henüz yapmadıysanız Docker’ı yükleyip Windows veya MacOS makinenizde yerel olarak başlatın. Daha fazla bilgi için https://docs.docker.com/install/ sayfasındaki Docker yükleme yönergelerine bakın. Community sürümü yeterlidir.

1. Soldaki bölmeden **Klasör** simgesini seçerek projenizin **Dosyalar** listesini açın. `aml_config` klasörünü genişletin. 

2. **docker-python**, **docker-spark** ve **yerel** gibi birden fazla önceden yapılandırılmış ortam mevcuttur. 

   Her ortamda `docker.compute` (hem **docker-python** hem de **docker-spark** için) ve `docker-python.runconfig` gibi iki dosya vardır. Her dosyayı incelediğinizde belirli seçeneklerin metin düzenleyici ile yapılandırılabildiğini göreceksiniz.  

   Temizlemek için açık olan metin düzenleyicilerin sekmelerinde **Kapat** (**x**) öğesini seçin.

3. **docker-python** ortamını kullanarak **iris_sklearn.py** betiğini çalıştırın: 

   - Sol araç çubuğundaki **Saat** simgesini seçerek **Çalıştırmalar** bölmesini açın. **Tüm Çalıştırmalar**’ı seçin. 

   - **Tüm Çalıştırmalar** sekmesinin en üst kısmındaki hedeflenen ortam bölümünde varsayılan **yerel** değerinin yerine **docker-python** değerini seçin. 

   - Ardından sağ tarafa geçerek çalıştırılacak betik için **iris_sklearn.py** dosyasını seçin. 

   - Betikte belirtilmiş bir varsayılan değer olduğundan **Bağımsız Değişkenler** alanını boş bırakın. 

   - **Çalıştır** düğmesini seçin.

4. Yeni bir işin başladığını gözlemleyin. İş, workbench penceresinin sağ tarafındaki **İşler** bölmesinde listelenir.

   Docker üzerinde ilk kez çalışıyorsanız işlemin tamamlanması fazladan birkaç dakika sürer. 

   Arka planda, Azure Machine Learning Workbench yeni bir Docker dosyası derler. 
   Yeni dosya, `docker.compute` dosyasında belirtilen temel Docker görüntüsüne ve `conda_dependencies.yml` dosyasında belirtilen bağımlılık Python paketlerine başvurur. 
   
   Docker altyapısı aşağıdaki görevleri gerçekleştirir:

    - Azure’dan temel görüntüyü indirir.
    - `conda_dependencies.yml` dosyasında belirtilen Python paketlerini yükler.
    - Bir Docker kapsayıcısı başlatır.
    - Çalıştırma yapılandırmasına bağlı olarak proje klasörünün yerel kopyasını kopyalar veya kopyaya başvurur.      
    - `iris_sklearn.py` betiğini yürütür.

   İşlemin sonunda **yerel** ortamını hedeflemenizle tam olarak aynı sonuçları görmeniz gerekir.

5. Şimdi Spark'ı deneyelim. Docker temel görüntüsünde, PySpark betiğini yürütmek üzere kullanabileceğiniz, önceden yüklenmiş ve yapılandırılmış bir Spark örneği bulunur. Bu temel görüntü, Spark'ı yükleyip yapılandırmanıza gerek kalmadan Spark programınızı geliştirmek ve test etmek için kullanacağınız kolay bir yöntemdir. 

   `iris_spark.py` dosyasını açın. Bu betik `iris.csv` veri dosyasını yükler ve Spark Machine Learning kitaplığındaki lojistik regresyon algoritmasını kullanarak Iris veri kümesini sınıflandırır. Şimdi çalıştırma ortamını **docker-spark**, betiği ise **iris_spark.py** olarak değiştirin ve sonra tekrar çalıştırın. Docker kapsayıcısı içinde Spark oturumunun oluşturulması ve başlatılması gerektiğinden bu işlem biraz daha uzun sürer. stdout değerinin `iris_spark.py` için stdout değerinden farklı olduğunu görebilirsiniz.

6. Birkaç çalıştırma daha başlatın ve farklı bağımsız değişkenler kullanın. 

7. Spark Machine Learning kitaplığı kullanılarak derlenen lojistik regresyon modelini görmek için `iris_spark.py` dosyasını açın. 

8. **İşler** bölmesiyle etkileşimde bulunarak farklı yürütme ortamlarında gerçekleştirdiğiniz çalıştırmaların çalıştırma geçmişi liste görünümü ve çalıştırma ayrıntıları görünümünü inceleyin.

## <a name="run-scripts-in-the-cli-window"></a>Betikleri CLI penceresinde çalıştırma

1. Azure Machine Learning komut satırı arabirimini (CLI) başlatın:
   1. Azure Machine Learning Workbench’i başlatın.

   1. Workbench menüsünden **Dosya** > **Komut İstemini Aç**’ı seçin. 
   
   CLI istemi Windows `C:\Temp\myIris\>` proje klasöründe başlar. Bu, öğreticinin 1. Bölümünde oluşturduğunuz projedir.

   >[!IMPORTANT]
   >Sonraki adımları gerçekleştirmek için bu CLI penceresini kullanmanız gerekir.

1. CLI penceresinde Azure oturumunu açın. [az login hakkında daha fazla bilgi edinin](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   Zaten oturum açmış olabilirsiniz. Bu durumda bu adımı atlayabilirsiniz.

   1. Komut isteminde şunu girin:
      ```azurecli
      az login
      ```

      Bu komut, https://aka.ms/devicelogin adresinden tarayıcınızda kullanmak için bir kod döndürür.

   1. Tarayıcınızda https://aka.ms/devicelogin sayfasına gidin.

   1. Sorulduğunda, CLI'dan aldığınız kodu tarayıcınıza girin.

   Workbench uygulaması ve CLI, Azure kaynaklarında kimlik doğrulaması gerçekleştirmek için bağımsız kimlik bilgisi önbelleği kullanır. Oturum açtıktan sonra, önbelleğe alınmış belirtecin süresi doluncaya kadar tekrar kimlik doğrulaması yapmanız gerekmez. 

1. Kuruluşunuzda birden çok Azure aboneliği (kuruluş ortamı) varsa, kullanılacak aboneliği ayarlamanız gerekir. Aboneliğinizi bulun, abonelik kimliğini kullanarak ayarlayın ve sonra sınayın.

   1. Bu komutu kullanarak erişiminizin olduğu her bir Azure aboneliğini listeleyin:
   
      ```azurecli
      az account list -o table
      ```

      **az account list** komutu, oturum açma bilgilerinizle kullanılabilen aboneliklerin listesini döndürür. 
      Birden fazla varsa, kullanmak istediğiniz aboneliğin abonelik kimliği değerini tanımlayın.

   1. Varsayılan hesap olarak kullanmak istediğiniz Azure aboneliğini ayarlayın:
   
      ```azurecli
      az account set -s <your-subscription-id>
      ```
      Burada \<your-subscription-id\>, kullanmak istediğiniz aboneliğin kimlik değeridir. Köşeli ayraçları dahil etmeyin.

   1. Geçerli aboneliğin ayrıntılarını isteyerek yeni abonelik ayarını onaylayın. 

      ```azurecli
      az account show
      ```    

1. Henüz kitaplığınız yoksa, CLI penceresinde **matplotlib** adlı Python çizim kitaplığını yükleyin.

   ```azurecli
   pip install matplotlib
   ```

1. CLI penceresinde **iris_sklearn.py** betiğini bir deney olarak gönderin.

   iris_sklearn.py betiği yerel işlem bağlamına göre yürütülür.

   + Windows'da:
     ```azurecli
     az ml experiment submit -c local .\iris_sklearn.py
     ```

   + MacOS'ta:
     ```azurecli
     az ml experiment submit -c local iris_sklearn.py
     ```
   
   Çıktınız aşağıdakine benzer olmalıdır:
    ```text
    RunId: myIris_1521077190506
    
    Executing user inputs .....
    ===========================
    
    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]
    
    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679
        
    ==========================================
    Serialize and deserialize using the outputs folder.
    
    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
     [ 1 37 12]
     [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.
    
    Execution Details
    =================
    RunId: myIris_1521077190506
    ```

1. Çıktıyı gözden geçirin. Betiği çalıştırmak için Workbench kullandığınızda elde ettiğiniz çıktının ve sonuçların aynısını alırsınız. 

1. CLI penceresinde **iris_sklearn.py** adlı Python betiğini yine bir Docker yürütme ortamı kullanarak (makinenizde Docker yüklüyse) çalıştırın.

   + Kapsayıcınız Windows üzerindeyse: 
     |Yürütme<br/>environment|Windows komutu|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python .\iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark .\iris_spark.py 0.1`|

   + Kapsayıcınız MacOS üzerindeyse: 
     |Yürütme<br/>environment|Windows komutu|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark iris_spark.py 0.1`|

1. Workbench’e geri dönün ve:
   1. Proje dosyalarını listelemek için sol bölmedeki klasör simgesini seçin.
   
   1. **run.py** adlı Python betiğini açın. Bu betik çeşitli düzenleme hızları üzerinde döngü oluşturmak için yararlıdır. 

   ![Çalıştırma listesine dönme](media/tutorial-classifying-iris/2-runpy.png)

1. Denemeyi bu hızlarla birden çok kez çalıştırın. 

   Bu betik `10.0` düzenleme hızıyla (gerçekten büyük bir sayı) bir `iris_sklearn.py` işi başlatır. Daha sonra betik sonraki çalıştırmada hızı yarıya indirir ve hız `0.005` değerinin altına inene kadar ikiye bölmeye devam eder. 

   Betik aşağıdaki kodu içerir:

   ![Çalıştırma listesine dönme](media/tutorial-classifying-iris/2-runpy-code.png)

1. **run.py** betiğini komut satırından aşağıdaki gibi çalıştırın:

   ```cmd
   python run.py
   ```

   Bu komut iris_sklearn.py betiğini farklı düzenli hale getirme oranlarıyla birden çok kez gönderir

   `run.py` tamamlandığında Workbench'teki çalıştırma geçmişi liste görünümünde farklı ölçümlerin grafiklerini görebilirsiniz.

## <a name="run-scripts-in-a-remote-docker-container"></a>Betikleri uzak bir Docker kapsayıcısında çalıştırma
Betiğinizi bir Linux uzak makinesinde bulunan Docker kapsayıcısında yürütmek için ilgili uzak makineye SSH erişimine (kullanıcı adı ve parola) sahip olmanız gerekir. Ayrıca, makinede Docker altyapısı yüklü ve çalışır durumda olmalıdır. Böyle bir Linux makineye sahip olmanın en kolay yolu Azure'da bir Ubuntu tabanlı Veri Bilimi Sanal Makinesi (DSVM) oluşturmaktır. [Azure ML Workbench’te kullanılacak bir Ubuntu DSVM oluşturma](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal) hakkında bilgi edinin.

>[!NOTE] 
>CentOS tabanlı DSVM *desteklenmez*.

1. VM oluşturulduktan sonra `.runconfig` ve `.compute` dosyalarının bir çiftini oluşturarak VM’yi yürütme ortamı olarak ekleyebilirsiniz. Dosyaları oluşturmak için aşağıdaki komutu kullanın. 

 Yeni işlem hedefini `myvm` olarak adlandıralım.
 
   ```azurecli
   az ml computetarget attach remotedocker --name myvm --address <your-IP> --username <your-username> --password <your-password>
   ```
   
   >[!NOTE]
   >IP adresi alanı `vm-name.southcentralus.cloudapp.azure.com` gibi genel olarak adreslenebilir tam etki alanı adı da (FQDN) olabilir. FQDN’yi DSVM’ye eklemek ve IP adresi yerine kullanmak iyi bir uygulamadır. VM’yi maliyet tasarrufu için bir noktada kapatma seçeneğine sahip olduğunuzdan bu uygulama faydalıdır. Ayrıca sanal makineyi yeniden başlattığınızda da IP adresi değişmiş olabilir.

   >[!NOTE]
   >Kullanıcı adı ve parola kimlik doğrulamasına ek olarak `--private-key-file` ve (isteğe bağlı olarak) `--private-key-passphrase` seçeneklerini kullanarak bir özel anahtar ve karşılık gelen parolayı belirtebilirsiniz.

   Ardından, bu komutu çalıştırarak **myvm** işlem hedefini hazırlayın.
   
   ```azurecli
   az ml experiment prepare -c myvm
   ```
   
   Yukarıdaki komut Docker görüntüsünü sanal makinede oluşturarak betikleri çalıştırmaya hazır hale getirir.
   
   >[!NOTE]
   >`myvm.runconfig` içinde `false` olan varsayılan `PrepareEnvironment` değerini `true` olarak da değiştirebilirsiniz. Bu değişiklik, Docker kapsayıcısını ilk çalıştırma kapsamında otomatik olarak hazırlar.

2. `aml_config` altında oluşturulan `myvm.runconfig` dosyasını düzenleyin ve çerçevenin varsayılan `PySpark` değerini `Python` olarak değiştirin:

   ```yaml
   Framework: Python
   ```
   >[!NOTE]
   >PySpark da işe yarasa da, Python betiğinizi çalıştırmak için gerçekten bir Spark oturumu gerekli değilse Python kullanmak daha verimlidir.

3. CLI penceresinde, bu kez iris_sklearn.py betiğini uzak bir Docker kapsayıcısında çalıştırmak üzere _myvm_ hedefini kullanarak, daha önce çalıştırdığınız komutu girin:
   ```azurecli
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   Komut, uzak Linux VM üzerinde gerçekleşmesi dışında `docker-python` ortamındaymışsınız gibi yürütülür. CLI penceresinde aynı çıktı bilgileri görüntülenir.

4. Şimdi kapsayıcıda Spark kullanmayı deneyelim. Dosya Gezgini'ni açın. `myvm.runconfig` dosyasının bir kopyasını oluşturun ve `myvm-spark.runconfig` adını verin. Yeni dosyayı düzenleyerek `Python` olan `Framework` ayarını `PySpark` olarak değiştirin:
   ```yaml
   Framework: PySpark
   ```
   `myvm.compute` dosyasında değişiklik yapmayın. Spark yürütmesi için aynı sanal makine üzerindeki aynı Docker görüntüsü kullanılır. `myvm-spark.runconfig` içindeki `Target` alanı aynı `myvm.compute` dosyaya `myvm` adıyla işaret eder.

5. Uzak Docker kapsayıcısının içinde çalışan Spark örneğinde **iris_spark.py** betiğini çalıştırmak için aşağıdaki komutu girin:
   ```azureli
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="run-scripts-in-hdinsight-clusters"></a>Betikleri HDInsight kümelerinde çalıştırma
Bu betiği bir HDInsight Spark kümesinde de çalıştırabilirsiniz. [Azure ML Workbench’te kullanılacak bir HDInsight Spark Kümesi oluşturma](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) hakkında bilgi edinin.

>[!NOTE] 
>HDInsight kümesi birincil depolama olarak Azure Blob kullanmalıdır. Azure Data Lake depolamanın kullanılması henüz desteklenmemektedir.

1. Azure HDInsight Spark kümesine erişiminiz varsa burada gösterilen şekilde bir HDInsight çalıştırma yapılandırması oluşturun. Parametrelere HDInsight kümesinin adını, HDInsight kullanıcı adınızı ve parolanızı girin. 

   Bir HDInsight kümesini işaret eden işlem hedefi oluşturmak için aşağıdaki komutu kullanın:

   ```azurecli
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <your-username> --password <your-password>
   ```

   HDInsight kümesini hazırlamak için şu komutu çalıştırın:

   ```
   az ml experiment prepare -c myhdi
   ```

   Küme baş düğümü FQDN'si genellikle `<your_cluster_name>-ssh.azurehdinsight.net` şeklindedir.

   >[!NOTE]
   >`username`, HDInsight kurulumu sırasında tanımlanan küme SSH kullanıcı adıdır. Varsayılan değer `sshuser` şeklindedir. Bu değer, kümenin yönetim web sitesine erişim sağlamak için sağlama sırasında oluşturulan diğer kullanıcı olan `admin` değildir. 

2. HDInsight kümesinde **iris_spark.py** betiğini şu komutla çalıştırın:

   ```azurecli
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Betiği uzak HDInsight kümesinde yürüttüğünüzde `admin` kullanıcı hesabını kullanarak `https://<your_cluster_name>.azurehdinsight.net/yarnui` konumundaki Yet Another Resource Negotiator (YARN) iş yürütme ayrıntılarını da görüntüleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar
Üç kısımlı öğretici serisinin bu ikinci kısmında şunları yapmayı öğrendiniz:
> [!div class="checklist"]
> * Workbench’te betikleri açma ve kodu gözden geçirme
> * Betikleri yerel bir ortamda yürütme
> * Çalıştırma geçmişini gözden geçirme
> * Betikleri yerel bir Docker ortamında yürütme

Şimdi bu öğretici serisinde, gerçek zamanlı bir web hizmeti olarak oluşturduğunuz mantıksal regresyon modelini dağıtabileceğiniz üçüncü bölümü deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici 3 - Modelleri dağıtma](tutorial-classifying-iris-part-3.md)
