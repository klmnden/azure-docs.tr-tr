---
title: "Azure Machine Learning hizmetleri için model derleme (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu öğretici bir deneyimin 2. bölümüdür."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/25/2017
ms.openlocfilehash: 976407daee45e2f3a8360c1316227cc3399ad43e
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="classifying-iris-part-2-build-a-model"></a>Iris sınıflandırma bölüm 2: Model derleme
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici üç bölümden oluşan bir serinin ikinci bölümüdür. Öğreticinin bu bölümünde Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Workbench ile çalışma
> * Betikleri açma ve kodları gözden geçirme
> * Betikleri yerel bir ortamda yürütme
> * Çalıştırma geçmişini gözden geçirme
> * Betikleri yerel bir Docker ortamında yürütme
> * Betikleri yerel bir Azure CLI penceresinde yürütme
> * Betikleri uzak bir Docker ortamında yürütme
> * Betikleri bulut üzerindeki HDInsight ortamında yürütme

Bu öğreticide kolaylık sağlamak için değişmeyen [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak macOS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici serisinin ilk bölümünü tamamlamış olmanız gerekir. Bu öğreticideki adımları uygulamaya başlamadan önce [Veri öğreticisini hazırlama](tutorial-classifying-iris-part-1.md) bölümündeki talimatları izleyerek Azure Machine Learning kaynaklarını oluşturun ve Azure Machine Learning Workbench uygulamasını yükleyin.

İsteğe bağlı olarak betikleri yerel Docker kapsayıcısıyla çalıştırabilirsiniz. Bunun için bir Docker altyapısının (Community Edition yeterlidir) Windows veya macOS makinenizde yüklü ve başlatılmış olması gerekir. [Docker yükleme talimatları](https://docs.docker.com/engine/installation/) hakkında daha fazla bilgi edinin.

Bir betiği uzak Azure VM'sindeki veya HDInsight Spark kümesindeki Docker kapsayıcısına göndermek istiyorsanız [Ubuntu tabanlı Azure Veri Bilimi Sanal Makinesi veya HDI Kümesi oluşturma talimatlarını](how-to-create-dsvm-hdi.md) inceleyebilirsiniz.

## <a name="review-irissklearnpy-and-configuration-files"></a>iris_sklearn.py dosyasını ve yapılandırma dosyalarını gözden geçirin
1. **Azure Machine Learning Workbench** uygulamasını başlatın ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra Azure Machine Learning Workbench uygulamasının sol taraftaki araç çubuğunda yer alan **Dosyalar** düğmesine (klasör simgesi) tıklayın.

3. **iris_sklearn.py** dosyasını seçin. Python kodu Workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

   ![dosya aç](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Bu basit proje sıklıkla güncelleştirildiğinden gördüğünüz projeler yukarıdaki kodla aynı olmayabilir.

4. Python betik kodunu gözden geçirerek kodlama stili hakkında bilgi edinin. Betiğin aşağıdaki görevleri gerçekleştirdiğine dikkat edin:

   - **iris.dprep** veri hazırlama paketini yükleyerek [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) öğesini oluşturur. 

        >[!NOTE]
        >Örnek projeyle birlikte gelen `iris.dprep` veri hazırlama paketini kullanıyoruz. Paketin, bu öğreticinin 1. bölümünde derlediğiniz `iris-1.dprep` dosyasıyla aynı olması gerekir.

   - Sorunun çözülmesini zorlaştırmak için rastgele özellikler ekler. (Iris, yaklaşık %100 kesinlikle kolayca sınıflandırılabilen küçük bir veri kümesi olduğundan rastgele hale getirilmesi gerekir.)

   - [scikit-learn](http://scikit-learn.org/stable/index.html) makine öğrenimi kitaplığını kullanarak basit bir Lojistik Regresyon modeli derler. 

   - [pickle](https://docs.python.org/2/library/pickle.html) kitaplığını kullanarak modeli `outputs` klasöründeki bir dosyaya seri hale getirir, yükler ve seri durumdan çıkararak belleğe geri alır

   - Seri durumdan çıkarılmış modeli kullanarak yeni kayıtla ilgili bir tahminde bulunur. 

   - [matplotlib](https://matplotlib.org/) kitaplığını kullanarak karışıklık matrisi ve çok sınıflı ROC eğrisi olmak üzere iki grafik çizer ve bunları `outputs` klasörüne kaydeder.

   - Kurallaştırma oranını ve model kesinliğini günlüklere kaydetmek için işlem boyunca `run_logger` nesnesi kullanılır ve günlükler otomatik olarak çalıştırma geçmişinde silinir.


## <a name="execute-irissklearnpy-script-in-local-environment"></a>iris_sklearn.py betiğini yerel ortamda çalıştırma

Şimdi **iris_sklearn.py** betiğini ilk kez çalıştırmak üzere hazırlayalım. Bu betik için scikit-learn ve matplotlib paketleri gerekir. **scikit-learn**, Azure ML Workbench tarafından önceden yüklenmiştir. Ancak **matplotlib** paketini yüklememiz gerekir. 

1. Azure Machine Learning Workbench'te **Dosya** menüsüne tıklayın ve **Komut İstemini Aç**'ı seçerek komut istemini başlatın. Bu komut satırı arabirimine Azure Machine Learning Workbench CLI penceresi veya kısaca CLI penceresi diyoruz.

2. CLI penceresine aşağıdaki komutu yazarak **matplotlib** Python paketini yükleyin. Bu işlemin bir dakikadan kısa sürede tamamlanması gerekir.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Yukarıdaki `pip install` komutunu atlarsanız `iris_sklearn.py` içindeki komut başarıyla çalışır ancak geçmiş görselleştirmelerinde gösterilen şekilde karışıklık matrisi çıktısını ve çok sınıflı ROC eğrisi çizimlerini oluşturmaz.

3. Workbench uygulama penceresine dönün. 

4. **iris_sklearn.py** sekmesinin sol üst kısmında, kaydet simgesinin yanında bulunan açılır menüye tıklayarak **Yapılandırmayı Çalıştır** bölümünü seçin.  Yürütme ortamı olarak **yerel** seçeneğini, çalıştırılacak betik olarak da `iris_sklearn.py` seçeneğini belirleyin.

5. Daha sonra aynı sekmenin sağ tarafına geçerek **Bağımsız Değişkenler** alanına `0.01` değerini girin. 

   ![görüntü](media/tutorial-classifying-iris/run_control.png)

6. **Çalıştır** düğmesine tıklayın. Anında bir iş zamanlanır. İş, Workbench penceresinin sağ tarafındaki **İşler** panelinde listelenir. 

7. Bir süre sonra işin durumu **Gönderiliyor**'dan **Çalışıyor** ve kısa bir süre sonra da **Tamamlandı** olarak değişir.

   ![sklearn öğesini çalıştırma](media/tutorial-classifying-iris/run_sklearn.png)

8. İşler panelinde iş durumu metnindeki **Tamamlandı** kelimesine tıklayın. Bir pencere açılır ve çalışan betiğin standart çıktı (stdout) metni görüntülenir. stdout metnini kapatmak için açılan pencerenin sağ üst kısmındaki **X** düğmesine tıklayın.

9. İşler panelindeki aynı iş durumunda **Tamamlandı** durumunun ve başlangıç zamanının hemen üzerinde yer alan mavi renkli **iris_sklearn.py [n]** (_n_ çalıştırma numarasıdır) metnine tıklayın. **Çalıştırma Özellikleri** sayfası açılır ve ilgili çalıştırmaya ait Çalıştırma Özellikleri bilgileri, **Çıktı** dosyaları, **Görselleştirmeler** ve **Günlükler** gösterilir. 

   Çalıştırma tamamlandığında açılır pencerede aşağıdaki sonuçlar gösterilir:

   >[!NOTE]
   >Önceden belirlenen eğitim kümesine rastgele seçimler eklediğimiz için sonuçlarınız burada gösterilenden farklı olabilir.

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
   Confusion matrix and ROC curve plotted. See them in Run History details page.
   ```

10. **Çalıştırma Özellikleri** sekmesini kapatın ve **iris_sklearn.py** sekmesine dönün. 

11. Ek çalıştırmaları gerçekleştirin. 

    **Bağımsız Değişkenler** alanına `0.001` ile `10` arasında farklı sayısal değerler girin. **Çalıştır**'a tıklayarak kodu birkaç kez daha yürütün. Değiştirdiğiniz bağımsız değişken değeri koddaki Lojistik Regresyon algoritmasına iletilir ve her seferinde farklı sonuçlar alınır.

## <a name="review-run-history-in-detail"></a>Çalıştırma Geçmişinin ayrıntılarını gözden geçirme
Azure Machine Learning Workbench'te yürütülen her betik çalıştırma geçmişi kaydına dahil edilir. **Çalıştırmalar** görünümünü açarak belirli bir betiğin çalıştırma geçmişini görüntüleyebilirsiniz.

1. Sol araç çubuğundaki **Çalıştırmalar** düğmesine (saat simgesi) tıklayarak **Çalıştırmalar** listesini açın. Ardından **iris_sklearn.py** öğesine tıklayarak `iris_sklearn.py` öğesinin **Çalıştırma Panosunu** açın.

   ![görüntü](media/tutorial-classifying-iris/run_view.png)

2. **Çalıştırma Panosu** sekmesi açılır. Birden fazla çalıştırma sonucunda toplanan istatistikleri gözden geçirin. Grafikler sekmenin en üstünde oluşturulur ve sayfanın alt tarafındaki tabloda her çalıştırma ayrıntılarıyla birlikte yer alır.

   ![görüntü](media/tutorial-classifying-iris/run_dashboard.png)

3. Tabloyu filtreleyip grafiklere tıklayarak her bir çalıştırmanın durumunu, süresini, kesinliğini ve kurallaştırma oranını görüntüleyebilirsiniz. 

4. **Çalıştırmalar** tablosundan iki veya üç çalıştırma seçip **Karşılaştır** düğmesine tıklayarak ayrıntılı karşılaştırma sayfasını açabilirsiniz. Yan yana karşılaştırmayı gözden geçirin. Karşılaştırma sayfasının sol üst kısmındaki **Çalıştırma listesi** geri düğmesine tıklayarak **Çalıştırma Panosu**'na dönün.

5. Ayrıntılarını görmek için çalıştırmalardan birine tıklayın. Seçilen çalıştırmanın istatistikleri _Çalıştırma Özellikleri_ bölümünde listelenir. Çıktı klasörüne yazılmış olan dosyalar **Çıktı** bölümünde listelenir ve indirilebilir.

   ![görüntü](media/tutorial-classifying-iris/run_details.png)

   Karışıklık matrisi ve çok sınıflı ROC eğrisi çizimleri **Görselleştirmeler** bölümünde oluşturulur. Tüm günlük dosyalarına **Günlükler** bölümünden de ulaşabilirsiniz.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Betikleri yerel Docker ortamında yürütme

Azure ML, Docker gibi ek yürütme ortamlarını kolayca yapılandırmanızı ve betiğinizi bu ortamlarda çalıştırmanızı sağlar. 

>[!IMPORTANT]
>Bu adımı tamamlamak için Docker altyapısının yerel ortamınızda yüklü ve başlatılmış olması gerekir. Daha fazla bilgi için yükleme kılavuzuna bakın.

1. Sol araç çubuğundaki klasör simgesini seçerek projenizin **Dosyalar** listesini açın. `aml_config` klasörünü genişletin. 

2. **docker-python**, **docker-spark** ve **yerel** gibi birden fazla önceden yapılandırılmış ortam olduğuna dikkat edin. 

   Her ortamda `docker-python.compute` ve `docker-python.runconfig` olmak üzere iki dosya vardır. Her dosya türünü incelediğinizde belirli seçeneklerin metin düzenleyici ile yapılandırılabileceğini göreceksiniz.  

   Karışıklığı önlemek için açık metin düzenleyici sekmelerini kapatın (X).

3. **iris_sklearn.py** betiğini **docker-python** ortamını kullanarak çalıştırın. 

   - Sol araç çubuğundaki saat simgesine tıklayarak **Çalıştırmalar** panelini açın. **Tüm Çalıştırmalar**'a tıklayın. 
   - **Tüm Çalıştırmalar** sekmesinin en üst kısmındaki hedeflenen ortam bölümünde varsayılan **yerel** değerinin yerine **docker-python** değerini seçin. 
   - Ardından sağ tarafa geçerek çalıştırılacak betik için **iris_sklearn.py** dosyasını belirleyin. 
   - Betikte belirtilmiş bir varsayılan değer olduğundan **Bağımsız Değişkenler** alanını boş bırakın. 
   - **Çalıştır** düğmesine tıklayın.

4. Yeni bir iş başlatılır ve Workbench penceresinin sağ tarafındaki **İşler** panelinde gösterilir.

   Docker üzerinde ilk kez çalışıyorsanız işlemin tamamlanması fazladan birkaç dakika sürebilir. 

   Azure Machine Learning Workbench arka planda `docker.compute` dosyasında belirtilen temel Docker görüntüsüne ve `conda_dependencies.yml` dosyasında belirtilen bağımlı Python paketlerine başvuran yeni bir Docker dosyası derler. Docker altyapısı ardından temel görüntüyü Azure'dan indirir, `conda_dependencies.yml` dosyasında belirtilen Python paketlerini yükler ve bir Docker kapsayıcısı başlatır. Ardından proje klasörünün yerel kopyasını kopyalar (veya çalıştırma yapılandırmasına bağlı olarak başvuru oluşturur) ve ardından `iris_sklearn.py` betiğini yürütür. İşlemin sonunda **yerel** hedefleme ile tam olarak aynı sonucu almanız gerekir.

5. Şimdi Spark'ı deneyelim. Docker temel görüntüsünde önceden yüklenmiş ve yapılandırılmış bir Spark örneği bulunur. Bu sayede içinde bir PySpark betiği yürütebilirsiniz. Bu, Spark'ı yükleyip yapılandırmanıza gerek kalmadan Spark programınızı geliştirmek ve test etmek için kullanacağınız kolay bir yöntemdir. 

   `iris_spark.py` dosyasını açın. Bu betik `iris.csv` veri dosyasını yükler ve Spark ML kitaplığındaki Lojistik Regresyon algoritmasını kullanarak Iris veri kümesini sınıflandırır. Şimdi çalıştırma ortamını **docker-spark**, betiği ise **iris_spark.py** olarak değiştirin ve tekrar çalıştırın. Docker kapsayıcısı içinde Spark oturumunun oluşturulması ve başlatılması gerektiğinden bu işlem biraz daha uzun sürer. stdout değerinin `iris_spark.py` için stdout değerinden farklı olduğunu görebilirsiniz.

6. Birkaç çalıştırma daha gerçekleştirin ve farklı bağımsız değişkenler kullanın. 

7. Spark ML kitaplığı kullanılarak derlenen basit Lojistik Regresyon modelini görmek için `iris_spark.py` dosyasını açın. 

8. **İşler** paneliyle etkileşimde bulunarak farklı yürütme ortamlarında gerçekleştirdiğiniz çalıştırmaların çalıştırma geçmişi liste görünümü ve çalıştırma ayrıntıları görünümünü inceleyin.

## <a name="execute-scripts-in-the-azure-ml-cli-window"></a>Betikleri Azure ML CLI penceresinde yürütme

1. Azure Machine Learning Workbench'te **Dosya** menüsüne ve **Komut İstemi Aç**'a tıklayarak komut satırı penceresini başlatın. Komut isteminiz `C:\Temp\myIris\>` istemiyle proje klasöründe ve başlatılır.

   >[!Important]
   >Aşağıdaki adımları gerçekleştirmek için komut satırı penceresini (Workbench'ten başlatılan) kullanmanız gerekir:

2. Komut istemini (CLI) kullanarak Azure'da oturum açın. 

   Workbench uygulaması ve CLI, Azure kaynaklarında kimlik doğrulaması gerçekleştirmek için bağımsız kimlik bilgisi önbelleği kullanır. Bu işlemi yalnızca bir kez yapmanız gerekir ve önbelleğe alınan belirteç süresi dolana kadar geçerli olacaktır. **az account list** komutu, oturum açma bilgilerinizle kullanılabilen aboneliklerin listesini döndürür. Birden fazla oturum açma bilgisi varsa kullanmak istediğiniz aboneliğin kimlik değerini kullanın ve **az set account -s** komutuyla birlikte abonelik kimliği değerini belirterek bunu varsayılan hesap olarak ayarlayın. Ardından account show komutunu kullanarak ayarı doğrulayın.

   ```azurecli
   REM login using aka.ms/devicelogin site.
   az login
   
   REM list all Azure subscriptions you have access to. 
   az account list -o table
   
   REM set the current Azure subscription to the one you want to use.
   az set account -s <subscriptionId>
   
   REM verify your current subscription is set correctly
   az account show
   ```

3. Kimlik doğrulamasından geçtikten ve geçerli Azure abonelik bağlamını ayarladıktan sonra CLI penceresine aşağıdaki komutları girerek matplotlib kitaplığını yükleyin ve Python betiğini çalıştırılacak deney olarak gönderin.

   ```azurecli
   REM You don't need to do this if you have installed matplotlib locally from the previous steps.
   pip install matplotlib
   
   REM Kick off an execution of the iris_sklearn.py file against local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Çıktıyı gözden geçirin. Bu öğreticide Workbench kullanarak çalıştırdığınız betikle aynı çıktının ve sonucun döndürüldüğüne dikkat edin. 

5. Bilgisayarınızda Docker yüklüyse aynı betiği Docker yürütme ortamını kullanarak çalıştırın.

   ```azurecli
   REM Execute iris_sklearn.py in local Docker container Python environment.
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM Execute iris_spark.py in local Docker container Spark environment.
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Azure Machine Learning Workbench'te sol araç çubuğundaki Klasör simgesine tıklayarak proje dosyalarını listeleyin ve **run.py** adlı Python betiğini açın. 

   Bu betik farklı kurallaştırma oranlarında döngü oluşturmak ve deneyi bu oranlarla birden fazla kez çalıştırmak için kullanışlıdır. Bu betik `10.0` kurallaştırma oranına (oldukça büyük bir sayıdır) sahip bir `iris_sklearn.py` işi başlatır ve oranı sonraki her çalıştırmada yarıya indirerek `0.005` olana kadar devam eder. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   **run.py** betiğini komut satırından başlatmak için aşağıdaki komutları çalıştırın:

   ```cmd
   REM Submit iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   `run.py` tamamlandığında Azure Machine Learning Workbench'teki çalıştırma geçmişi liste görünümünde bir grafik görüntülenir.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Uzak makinedeki bir Docker kapsayıcısında yürütme
Betiğinizi bir Linux uzak makinesinde bulunan Docker kapsayıcısında yürütmek için ilgili uzak makineye SSH erişimine (kullanıcı adı ve parola) sahip olmanız gerekir. Ayrıca uzak makinede Docker altyapısı yüklü ve çalışır durumda olmalıdır. Böyle bir Linux makineye sahip olmanın en kolay yolu Azure'da bir [Ubuntu tabanlı Veri Bilimi Sanal Makinesi (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) oluşturmaktır. (CentOS tabanlı DSVM desteklenmez.) 

1. Sanal makine oluşturulduktan sonra aşağıdaki komutu kullanarak `.runconfig` ve `.compute` dosyası çifti oluşturup yürütme ortamı olarak ekleyebilirsiniz. Bu örnekte yeni ortamı `myvm` olarak adlandıralım.
 
   ```azurecli
   REM create myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >IP Adresi alanı `vm-name.southcentralus.cloudapp.azure.com` gibi genel olarak adreslenebilir FQDN (tam etki alanı adı) da olabilir. Maliyet tasarrufu açısından sanal makineyi kapatmak isteyebileceğiniz için burada IP Adresi yerine FQDN değerini DSVM örneğinize ekleyip burada kullanmak faydalı olacaktır. Ayrıca sanal makineyi yeniden başlattığınızda da IP adresi değişmiş olabilir.

   Ardından aşağıdaki komutu çalıştırarak Docker görüntüsünü sanal makineye uygulayın ve betikleri çalıştırmaya hazır hale getirin.
   
   ```azurecli
   REM prepare the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >`myvm.runconfig` içindeki `PrepareEnvironment` varsayılan `false` değerini `true` olarak da değiştirebilirsiniz. Bu komut Docker kapsayıcısını ilk çalıştırmada otomatik olarak hazırlar.

2. `aml_config` altında oluşturulan `myvm.runconfig` dosyasını düzenleyin ve çerçevenin varsayılan `PySpark` değerini `Python` olarak değiştirin:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Çerçeve ayarını PySpark olarak da bırakabilirsiniz. Ancak Python betiğinizi çalıştırmak için gerçekten bir Spark oturumuna ihtiyacınız yoksa çok verimli olmayacaktır.

3. CLI penceresine daha önce çalıştırdığınız komutu girin ancak bu durumda hedef olarak _myvm_ belirtin:
   ```azurecli
   REM execute iris_sklearn.py in remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Komut `docker-python` ortamı kullanıyor gibi yürütülür ancak yürütme işlemi uzak Linux sanal makinesinde gerçekleştirilir. CLI penceresinde aynı çıktı bilgileri görüntülenir.

4. Şimdi kapsayıcıda Spark'ı deneyelim. Dosya gezginini açın (temel dosya değiştirme komutlarına hakimseniz bunu CLI penceresinde de gerçekleştirebilirsiniz). `myvm.runconfig` dosyasının bir kopyasını oluşturun ve `myvm-spark.runconfig` adını verin. Yeni dosyayı düzenleyerek `Python` olan `Framework` ayarını `PySpark` olarak değiştirin:
   ```yaml
   "Framework": "PySpark"
   ```
   `myvm.compute` dosyasında değişiklik yapmayın. Spark yürütmesi için aynı sanal makine üzerindeki aynı Docker görüntüsü kullanılır. `myvy-spark.runconfig` içindeki `target` alanı aynı `myvm.compute` dosyaya `myvm` adıyla işaret eder.

5. Uzak Docker kapsayıcısındaki Spark örneğinde çalıştırmak için aşağıdaki komutu yazın:
   ```azureli
   REM execute iris_spark.py in Spark instance on remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Betiği bir HDInsight kümesinde yürütme
Bu betiği gerçek bir Spark kümesinde de çalıştırabilirsiniz. 

1. Azure HDInsight Spark kümesine erişiminiz varsa gösterilen şekilde bir HDI çalıştırma yapılandırması oluşturun. Parametrelere HDInsight kümesinin adını, HDInsight kullanıcı adınızı ve parolanızı girin. Aşağıdaki komutu kullanın:

   ```azurecli
   REM create a compute target that points to a HDI cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepare the HDI cluster
   az ml experiment prepare -c myhdi
   ```

   Küme baş düğümü tam etki alanı adı (FQDN) genelde `<cluster_name>-ssh.azurehdinsight.net` şeklindedir.

   >[!NOTE]
   >Burada `username`, küme SSH kullanıcı adıdır. HDI sağlama sırasında değiştirmediyseniz varsayılan değer `sshuser` olacaktır. Kümenin yönetim web sitesine erişim sağlamak için sağlama sırasında oluşturulan diğer kullanıcı olan `admin` değildir. 

2. Aşağıdaki komutu çalıştırdığınızda betik HDInsight kümesinde çalışır:

   ```azurecli
   REM execute iris_spark on the HDI cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Betiği uzak HDI kümesinde yürüttüğünüzde `admin` kullanıcı hesabını kullanarak `https://<cluster_name>.azurehdinsight.net/yarnui` YARN iş yürütme ayrıntılarını da görüntüleyebilirsiniz.


## <a name="next-steps"></a>Sonraki Adımlar
Üç bölümden oluşan öğretici serisinin bu ikinci bölümünde Azure Machine Learning hizmetlerini kullanarak aşağıdakileri gerçekleştirmeyi öğrendiniz:
> [!div class="checklist"]
> * Azure Machine Learning Workbench ile çalışma
> * Betikleri açma ve kodları gözden geçirme
> * Betikleri yerel bir ortamda yürütme
> * Çalıştırma geçmişini gözden geçirme
> * Betikleri yerel bir Docker ortamında yürütme
> * Betikleri yerel bir Azure CLI penceresinde yürütme
> * Betikleri uzak bir Docker ortamında yürütme
> * Betikleri bulut üzerindeki HDInsight ortamında yürütme

Artık serinin üçüncü bölümüne geçmeye hazırsınız. Burada Lojistik Regresyon modelini oluşturduk, şimdi de bunu gerçek zamanlı bir web hizmeti olarak dağıtalım.

> [!div class="nextstepaction"]
> [Model dağıtma](tutorial-classifying-iris-part-3.md)
