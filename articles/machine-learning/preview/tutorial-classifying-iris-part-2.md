---
title: "Azure Machine Learning hizmetleri için model derleme (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu ikinci bölümde deneme konusu ele alınmaktadır."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 11/06/2017
ms.openlocfilehash: f3b4b41593e0956e98f05c7f8d1c71632a489e56
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="classify-iris-part-2-build-a-model"></a>Iris sınıflandırma bölüm 2: Model derleme
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici üç bölümden oluşan bir serinin ikinci bölümüdür. Öğreticinin bu bölümünde Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Workbench’i kullanma.
> * Betikleri açma ve kodları gözden geçirme.
> * Betikleri yerel bir ortamda yürütme.
> * Çalıştırma geçmişini gözden geçirme.
> * Betikleri yerel bir Docker ortamında yürütme.
> * Betikleri yerel bir Azure CLI penceresinde yürütme.
> * Betikleri uzak bir Docker ortamında yürütme.
> * Betikleri bulut üzerindeki Azure HDInsight ortamında yürütme.

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak Mac OS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici serisinin ilk bölümünü tamamlayın. Bu öğreticideki adımları uygulamaya başlamadan önce [Veri öğreticisini hazırlama](tutorial-classifying-iris-part-1.md) bölümündeki talimatları izleyerek Azure Machine Learning kaynaklarını oluşturun ve Azure Machine Learning Workbench uygulamasını yükleyin.

İsteğe bağlı olarak betikleri yerel Docker kapsayıcısıyla çalıştırabilirsiniz. Bunun için bir Docker altyapısının (Community Edition yeterlidir) Windows veya Mac OS makinenizde yüklü ve başlatılmış olması gerekir. Docker yükleme hakkında daha fazla bilgi için bkz. [Docker yükleme yönergeleri](https://docs.docker.com/engine/installation/).

Bir betiği uzak Azure VM'sindeki veya Azure HDInsight Spark kümesindeki Docker kapsayıcısına göndermek için [Ubuntu tabanlı Azure Veri Bilimi Sanal Makinesi veya HDInsight kümesi oluşturma yönergelerini](how-to-create-dsvm-hdi.md) inceleyebilirsiniz.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>iris_sklearn.py dosyasını ve yapılandırma dosyalarını gözden geçirme
1. Azure Machine Learning Workbench uygulamasını ve öğretici serisinin önceki bölümünde oluşturduğunuz **myIris** projesini açın.

2. Proje açıldıktan sonra en sol bölmedeki **Dosyalar** düğmesine (klasör simgesi) tıklayarak proje klasörünüzdeki dosya listesini açın.

3. **iris_sklearn.py** dosyasını seçin. Python kodu workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

   ![Dosya açma](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Bu basit proje sıklıkla güncelleştirildiğinden, gördüğünüz projeler yukarıdaki kodla aynı olmayabilir.

4. Python betik kodunu gözden geçirerek kodlama stili hakkında bilgi edinin. Betik aşağıdaki görevleri gerçekleştirir:

   - **iris.dprep** veri hazırlama paketini yükleyerek [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) öğesini oluşturur. 

        >[!NOTE]
        >Örnek projeyle birlikte gelen `iris.dprep` veri hazırlama paketini kullanın. Bu paket, bu öğreticinin 1. bölümünde derlediğiniz `iris-1.dprep` dosyasıyla aynı olmalıdır.

   - Sorunun çözülmesini zorlaştırmak için rastgele özellikler ekler. Iris, yaklaşık %100 doğrulukla kolayca sınıflandırılan küçük bir veri kümesi olduğundan rastgele hale getirilmesi gerekir.

   - [scikit-learn](http://scikit-learn.org/stable/index.html) makine öğrenimi kitaplığını kullanarak bir lojistik regresyon modeli derler. 

   - [Pickle](https://docs.python.org/2/library/pickle.html) kitaplığını `outputs` klasöründeki dosyaya ekleyerek modeli seri hale getirir. Betik daha sonra modeli yükler ve bellekte yeniden seri durumdan çıkarır.

   - Seri durumdan çıkarılmış modeli kullanarak yeni kayıtla ilgili bir tahminde bulunur. 

   - [matplotlib](https://matplotlib.org/) kitaplığını kullanarak iki grafik, bir karışıklık matrisi ve bir çok sınıflı alıcı çalıştırma özellikleri (ROC) eğrisi çizer ve sonra bunları `outputs` klasörüne kaydeder.

   - Düzenleme hızını kaydetme ve doğruluğu günlüklerde modelleme işlemi boyunca `run_logger` nesnesi kullanılır. Günlükler çalıştırma geçmişinde otomatik olarak çizilir.


## <a name="execute-irissklearnpy-script-in-a-local-environment"></a>iris_sklearn.py betiğini yerel ortamda çalıştırma

Şimdi **iris_sklearn.py** betiğini ilk kez çalıştırmak üzere hazırlayalım. Bu betik için **scikit-learn** ve **matplotlib** paketleri gerekir. **scikit-learn** paketi Azure Machine Learning Workbench’te zaten yüklüdür. Yine de **matplotlib** paketini yüklemeniz gerekir. 

1. Azure Machine Learning Workbench'te **Dosya** menüsünü ve sonra **Komut İstemini Aç**'ı seçerek komut istemini açın. Bu komut satırı arabirimi penceresi, *Azure Machine Learning Workbench CLI penceresi* veya kısaca *CLI penceresi* olarak adlandırılır.

2. CLI penceresine aşağıdaki komutu girerek **matplotlib** Python paketini yükleyin. Bu işlemin bir dakikadan kısa sürede tamamlanması gerekir.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Önceki `pip install` komutunu atlarsanız `iris_sklearn.py` içindeki kod başarılı bir şekilde çalışır. Yalnızca `iris_sklearn.py` komutunu çalıştırırsanız kod geçmiş görselleştirmelerinde gösterilen şekilde karışıklık matrisi çıktısını ve çok sınıflı ROC eğrisi çizimlerini oluşturmaz.

3. Workbench uygulama penceresine dönün. 

4. **iris_sklearn.py** sekmesinin üst kısmındaki araç çubuğunda, **Kaydet** simgesinin yanındaki açılır menüyü seçerek açın ve ardından **Yapılandırmayı Çalıştır**’ı seçin. Yürütme ortamı olarak **yerel** seçeneğini belirleyin ve sonra çalıştırılacak betik olarak `iris_sklearn.py` girin.

5. Ardından, araç çubuğunun sağ tarafına giderek **Bağımsız Değişkenler** alanına `0.01` değerini girin. 

   ![Çalıştırma denetimi](media/tutorial-classifying-iris/run_control.png)

6. **Çalıştır** düğmesini seçin. Anında bir iş zamanlanır. İş, workbench penceresinin sağ tarafındaki **İşler** bölmesinde listelenir. 

7. Bir süre sonra işin **Gönderiliyor** olan durumu **Çalışıyor** ve sonra **Tamamlandı** olarak değişir.

   ![sklearn çalıştırma](media/tutorial-classifying-iris/run_sklearn.png)

8. **İşler** bölmesindeki iş durumu metninde **Tamamlandı** öğesini seçin. Bir pencere açılır ve çalışan betiğin standart çıktı (stdout) metni görüntülenir. stdout metnini kapatmak için açılır pencerenin sağ üst kısmındaki **Kapat** (**x**) düğmesini seçin.

9. **İşler** bölmesindeki aynı iş durumunda **Tamamlandı** durumunun ve başlangıç zamanının hemen üzerinde yer alan mavi renkli **iris_sklearn.py [n]** (_n_ çalıştırma numarasıdır) metnini seçin. **Çalıştırma Özellikleri** penceresi açılır ve bu çalıştırma için aşağıdaki bilgileri gösterir:
   - **Çalıştırma Özellikleri** bilgileri
   - **Çıkış** dosyaları
   - Varsa **Görselleştirmeler**
   - **Günlükler** 

   Çalıştırma tamamlandığında açılır pencerede aşağıdaki sonuçlar gösterilir:

   >[!NOTE]
   >Daha önce eğitim kümesine bazı rastgele seçimler eklediğimiz için tam sonuçlarınız burada gösterilen sonuçlardan farklı olabilir.

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

11. Ek çalıştırmaları gerçekleştirin. 

    **Bağımsız Değişkenler** alanına `0.001` ile `10` arasında farklı sayısal değerler girin. **Çalıştır**'ı seçerek kodu birkaç kez daha yürütün. Değiştirdiğiniz bağımsız değişken değeri koddaki lojistik regresyon algoritmasına iletilir ve her seferinde farklı bulgular alınır.

## <a name="review-the-run-history-in-detail"></a>Çalıştırma geçmişinin ayrıntılarını gözden geçirme
Azure Machine Learning Workbench'te yürütülen her betik çalıştırma geçmişi kaydına dahil edilir. **Çalıştırmalar** görünümünü açarsanız belirli bir betiğin çalıştırma geçmişini görüntüleyebilirsiniz.

1. **Çalıştırmalar** listesini açmak için sol araç çubuğundaki **Çalıştırmalar** düğmesini (saat simgesi) seçin. Ardından **iris_sklearn.py** öğesini seçerek `iris_sklearn.py` öğesinin **Çalıştırma Panosunu** açın.

   ![Çalıştırma görünümü](media/tutorial-classifying-iris/run_view.png)

2. **Çalıştırma Panosu** sekmesi açılır. Birden fazla çalıştırma sonucunda toplanan istatistikleri gözden geçirin. Graflar sekmenin üstünde işlenir. Her çalıştırma ardışık bir numaraya sahiptir ve çalıştırma ayrıntıları ekranın altındaki tabloda listelenir.

   ![Çalıştırma panosu](media/tutorial-classifying-iris/run_dashboard.png)

3. Tabloyu filtreleyip herhangi bir grafiğe tıklayarak her bir çalıştırmanın durumunu, süresini, kesinliğini ve kurallaştırma oranını görüntüleyebilirsiniz. 

4. **Çalıştırmalar** tablosundan iki veya üç çalıştırma belirleyip **Karşılaştır** düğmesini seçerek ayrıntılı karşılaştırma bölmesini açabilirsiniz. Yan yana karşılaştırmayı gözden geçirin. **Karşılaştırma** bölmesinin sol üst kısmındaki **Çalıştırma listesi** geri düğmesini seçerek **Çalıştırma Panosu**'na dönün.

5. Ayrıntılarını görmek için çalıştırmalardan birini seçin. Seçilen çalıştırmanın istatistikleri **Çalıştırma Özellikleri** bölümünde listelenir. Çıktı klasörüne yazılmış olan dosyalar **Çıktılar** bölümünde listelenir ve oradan indirilebilir.

   ![Çalıştırma ayrıntıları](media/tutorial-classifying-iris/run_details.png)

   Karışıklık matrisi ve çok sınıflı ROC eğrisi çizimleri **Görselleştirmeler** bölümünde oluşturulur. Tüm günlük dosyalarına **Günlükler** bölümünden de ulaşabilirsiniz.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Betikleri yerel Docker ortamında yürütme

Machine Learning sayesinde Docker gibi ek yürütme ortamlarını kolayca yapılandırabilir ve betiğinizi bu ortamlarda çalıştırabilirsiniz. 

>[!IMPORTANT]
>Bu adımı tamamlamak için Docker altyapısının yerel ortamınızda yüklü ve başlatılmış olması gerekir. Daha fazla bilgi için Docker yükleme yönergelerine bakın.

1. Soldaki bölmeden **Klasör** simgesini seçerek projenizin **Dosyalar** listesini açın. `aml_config` klasörünü genişletin. 

2. **docker-python**, **docker-spark** ve **yerel** gibi birden fazla önceden yapılandırılmış ortam mevcuttur. 

   Her ortamda `docker-python.compute` ve `docker-python.runconfig` olmak üzere iki dosya vardır. Her dosyayı incelediğinizde belirli seçeneklerin metin düzenleyici ile yapılandırılabildiğini göreceksiniz.  

   Temizlemek için açık olan metin düzenleyicilerin sekmelerinde **Kapat** (**x**) öğesini seçin.

3. **docker-python** ortamını kullanarak **iris_sklearn.py** betiğini çalıştırın: 

   - Sol araç çubuğundaki **Saat** simgesini seçerek **Çalıştırmalar** bölmesini açın. **Tüm Çalıştırmalar**’ı seçin. 
   - **Tüm Çalıştırmalar** sekmesinin en üst kısmındaki hedeflenen ortam bölümünde varsayılan **yerel** değerinin yerine **docker-python** değerini seçin. 
   - Ardından sağ tarafa geçerek çalıştırılacak betik için **iris_sklearn.py** dosyasını seçin. 
   - Betikte belirtilmiş bir varsayılan değer olduğundan **Bağımsız Değişkenler** alanını boş bırakın. 
   - **Çalıştır** düğmesini seçin.

4. Yeni bir işin başladığını gözlemleyin. İş, workbench penceresinin sağ tarafındaki **İşler** bölmesinde listelenir.

   Docker üzerinde ilk kez çalışıyorsanız işlemin tamamlanması fazladan birkaç dakika sürebilir. 

   Arka planda, Azure Machine Learning Workbench yeni bir docker dosyası derler. 
   Yeni dosya, `docker.compute` dosyasında belirtilen temel Docker görüntüsüne ve `conda_dependencies.yml` dosyasında belirtilen bağımlılık Python paketlerine başvurur. 
   
   Docker altyapısı aşağıdaki görevleri gerçekleştirir:

    - Azure’dan temel görüntüyü indirir.
    - `conda_dependencies.yml` dosyasında belirtilen Python paketlerini yükler.
    - Bir Docker kapsayıcısı başlatır.
    - Çalıştırma yapılandırmasına bağlı olarak proje klasörünün yerel kopyasını kopyalar veya kopyaya başvurur.      
    - `iris_sklearn.py` betiğini yürütür.

   İşlemin sonunda **yerel** ortamını hedeflemenizle tam olarak aynı sonucu görmeniz gerekir.

5. Şimdi Spark'ı deneyelim. Docker temel görüntüsünde önceden yüklenmiş ve yapılandırılmış bir Spark örneği bulunur. Bu örnek sayesinde, görüntü içinde bir PySpark betiği yürütebilirsiniz. Bu, Spark'ı yükleyip yapılandırmanıza gerek kalmadan Spark programınızı geliştirmek ve test etmek için kullanacağınız basit bir yöntemdir. 

   `iris_spark.py` dosyasını açın. Bu betik `iris.csv` veri dosyasını yükler ve Spark Machine Learning kitaplığındaki lojistik regresyon algoritmasını kullanarak Iris veri kümesini sınıflandırır. Şimdi çalıştırma ortamını **docker-spark**, betiği ise **iris_spark.py** olarak değiştirin ve sonra tekrar çalıştırın. Docker kapsayıcısı içinde Spark oturumunun oluşturulması ve başlatılması gerektiğinden bu işlem biraz daha uzun sürer. stdout değerinin `iris_spark.py` için stdout değerinden farklı olduğunu görebilirsiniz.

6. Birkaç çalıştırma daha gerçekleştirin ve farklı bağımsız değişkenler kullanın. 

7. Spark Machine Learning kitaplığı kullanılarak derlenen lojistik regresyon modelini görmek için `iris_spark.py` dosyasını açın. 

8. **İşler** bölmesiyle etkileşimde bulunarak farklı yürütme ortamlarında gerçekleştirdiğiniz çalıştırmaların çalıştırma geçmişi liste görünümü ve çalıştırma ayrıntıları görünümünü inceleyin.

## <a name="execute-scripts-in-the-azure-machine-learning-cli-window"></a>Betikleri Azure Machine Learning CLI penceresinde yürütme

1. Azure Machine Learning Workbench'te komut satırı penceresini açın, **Dosya** menüsünü ve sonra **Komut İstemini Aç**'ı seçin. Komut isteminiz `C:\Temp\myIris\>` istemiyle proje klasöründe başlatılır.

   >[!IMPORTANT]
   >Aşağıdaki adımları gerçekleştirmek için komut satırı penceresini (Workbench'ten açılan) kullanmanız gerekir.

2. Komut istemini kullanarak Azure'da oturum açın. 

   Workbench uygulaması ve CLI, Azure kaynaklarında kimlik doğrulaması gerçekleştirmek için bağımsız kimlik bilgisi önbelleği kullanır. Bu işlemi yalnızca bir kez yapmanız gerekir ve önbelleğe alınan belirteç süresi dolana kadar geçerli olacaktır. **az account list** komutu, oturum açma bilgilerinizle kullanılabilen aboneliklerin listesini döndürür. Birden fazla varsa, istenen abonelikteki kimlik değerini kullanın. Bu aboneliği **az account set -s** komutuyla birlikte kullanılacak varsayılan hesap olarak ayarlayın ve sonra abonelik kimliği değerini sağlayın. Ardından account **show** komutunu kullanarak ayarı doğrulayın.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Kimlik doğrulaması tamamlandıktan ve geçerli Azure abonelik bağlamını ayarladıktan sonra CLI penceresine aşağıdaki komutları girerek **matplotlib** kitaplığını yükleyin ve Python betiğini çalıştırılacak deneme olarak gönderin.

   ```azurecli
   REM you don't need to run this command if you have installed matplotlib locally from the previous steps
   pip install matplotlib
   
   REM kicks off an execution of the iris_sklearn.py file against the local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Çıktıyı gözden geçirin. Betiği çalıştırmak için Workbench kullandığınızda elde ettiğiniz çıktıyı ve sonuçları alırsınız. 

5. Bilgisayarınızda Docker yüklüyse aynı betiği Docker yürütme ortamını kullanarak tekrar çalıştırın.

   ```azurecli
   REM executes iris_sklearn.py in the local Docker container Python environment
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM executes iris_spark.py in the local Docker container Spark environment
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Workbench'te sol bölmedeki **Klasör** simgesini seçerek proje dosyalarını listeleyin ve **run.py** adlı Python betiğini açın. 

   Bu betik çeşitli düzenleme hızları üzerinde döngü oluşturmak için yararlıdır. Denemeyi bu hızlarla birden çok kez çalıştırın. Bu betik `10.0` düzenleme hızıyla (gerçekten büyük bir sayı) bir `iris_sklearn.py` işi başlatır. Daha sonra betik sonraki çalıştırmada hızı yarıya indirir ve hız `0.005` değerinin altına inene kadar ikiye bölmeye devam eder. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   **run.py** betiğini komut satırından açmak için aşağıdaki komutları çalıştırın:

   ```cmd
   REM submits iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   `run.py` tamamlandığında Workbench'teki çalıştırma geçmişi liste görünümünde bir grafik görüntülenir.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Uzak makinedeki bir Docker kapsayıcısında yürütme
Betiğinizi bir Linux uzak makinesinde bulunan Docker kapsayıcısında yürütmek için ilgili uzak makineye SSH erişimine (kullanıcı adı ve parola) sahip olmanız gerekir. Ayrıca, uzak makinede Docker altyapısı yüklü ve çalışır durumda olmalıdır. Böyle bir Linux makineye sahip olmanın en kolay yolu Azure'da bir Ubuntu tabanlı Veri Bilimi Sanal Makinesi (DSVM) oluşturmaktır. [Azure ML Workbench’te kullanılacak bir Ubuntu DSVM oluşturma](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal) hakkında bilgi edinin.

>[!NOTE] 
>CentOS tabanlı DSVM *desteklenmez*.

1. VM oluşturulduktan sonra `.runconfig` ve `.compute` dosyalarının bir çiftini oluşturursanız VM’yi yürütme ortamı olarak ekleyebilirsiniz. Dosyaları oluşturmak için aşağıdaki komutu kullanın. Bu örnekte yeni ortamı `myvm` olarak adlandıralım.
 
   ```azurecli
   REM creates an myvm compute target
   az ml computetarget attach remotedocker --name myvm --address <IP address> --username <username> --password <password>
   ```
   
   >[!NOTE]
   >IP adresi alanı `vm-name.southcentralus.cloudapp.azure.com` gibi genel olarak adreslenebilir tam etki alanı adı da (FQDN) olabilir. FQDN’yi DSVM’ye eklemek ve IP adresi yerine burada kullanmak iyi bir uygulamadır. VM’yi maliyet tasarrufu için bir noktada kapatma seçeneğine sahip olduğunuzdan bu uygulama faydalıdır. Ayrıca sanal makineyi yeniden başlattığınızda da IP adresi değişmiş olabilir.

   Daha sonra, aşağıdaki komutu çalıştırarak Docker görüntüsünü sanal makineye uygulayın ve betikleri çalıştırmaya hazır hale getirin:
   
   ```azurecli
   REM prepares the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >`myvm.runconfig` içindeki `PrepareEnvironment` varsayılan `false` değerini `true` olarak da değiştirebilirsiniz. Bu değişiklik, komut Docker kapsayıcısını ilk çalıştırmada otomatik olarak hazırlar.

2. `aml_config` altında oluşturulan `myvm.runconfig` dosyasını düzenleyin ve çerçevenin varsayılan `PySpark` değerini `Python` olarak değiştirin:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Çerçeve ayarını PySpark olarak bırakırsanız bu da işe yaramalıdır. Ancak Python betiğinizi çalıştırmak için gerçekten bir Spark oturumuna ihtiyacınız yoksa çok verimli olmayacaktır.

3. CLI penceresine daha önce çalıştırdığınız komutu girin ancak bu durumda hedef olarak _myvm_ belirtin:
   ```azurecli
   REM executes iris_sklearn.py in a remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Komut, uzak Linux VM üzerinde gerçekleşmesi dışında `docker-python` ortamındaymışsınız gibi yürütülür. CLI penceresinde aynı çıktı bilgileri görüntülenir.

4. Şimdi kapsayıcıda Spark kullanmayı deneyelim. Dosya Gezgini'ni açın. Temel dosya değiştirme komutlarına hakimseniz bunu CLI penceresinden de yapabilirsiniz. `myvm.runconfig` dosyasının bir kopyasını oluşturun ve `myvm-spark.runconfig` adını verin. Yeni dosyayı düzenleyerek `Python` olan `Framework` ayarını `PySpark` olarak değiştirin:
   ```yaml
   "Framework": "PySpark"
   ```
   `myvm.compute` dosyasında değişiklik yapmayın. Spark yürütmesi için aynı sanal makine üzerindeki aynı Docker görüntüsü kullanılır. `myvm-spark.runconfig` içindeki `target` alanı aynı `myvm.compute` dosyaya `myvm` adıyla işaret eder.

5. Uzak Docker kapsayıcısındaki Spark örneğinde çalıştırmak için aşağıdaki komutu yazın:
   ```azureli
   REM executes iris_spark.py in a Spark instance on a remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Betiği bir HDInsight kümesinde yürütme
Bu betiği bir HDInsight Spark kümesinde de çalıştırabilirsiniz. [Azure ML Workbench’te kullanılacak bir HDInsight Spark Kümesi oluşturma](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) hakkında bilgi edinin.

>![NOT] HDInsight kümesi birincil depolama olarak Azure Blob kullanmalıdır. Azure Data Lake depolamanın kullanılması henüz desteklenmemektedir.

1. Azure HDInsight Spark kümesine erişiminiz varsa burada gösterilen şekilde bir HDInsight çalıştırma yapılandırması oluşturun. Parametrelere HDInsight kümesinin adını, HDInsight kullanıcı adınızı ve parolanızı girin. Aşağıdaki komutu kullanın:

   ```azurecli
   REM creates a compute target that points to a HDInsight cluster
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <username> --password <password>

   REM prepares the HDInsight cluster
   az ml experiment prepare -c myhdi
   ```

   Küme baş düğümü FQDN'si genellikle `<cluster_name>-ssh.azurehdinsight.net` şeklindedir.

   >[!NOTE]
   >Burada `username`, küme SSH kullanıcı adıdır. HDInsight ayarı sırasında değiştirmezseniz varsayılan değer `sshuser` olacaktır. Bu değer, kümenin yönetim web sitesine erişim sağlamak için sağlama sırasında oluşturulan diğer kullanıcı olan `admin` değildir. 

2. Aşağıdaki komutu çalıştırdığınızda betik HDInsight kümesinde çalışır:

   ```azurecli
   REM executes iris_spark on the HDInsight cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Betiği uzak HDInsight kümesinde yürüttüğünüzde `admin` kullanıcı hesabını kullanarak `https://<cluster_name>.azurehdinsight.net/yarnui` konumundaki Yet Another Resource Negotiator (YARN) iş yürütme ayrıntılarını da görüntüleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu ikinci bölümünde Azure Machine Learning hizmetlerini kullanarak aşağıdakileri gerçekleştirmeyi öğrendiniz:
> [!div class="checklist"]
> * Azure Machine Learning Workbench’i kullanma.
> * Betikleri açma ve kodları gözden geçirme.
> * Betikleri yerel bir ortamda yürütme.
> * Çalıştırma geçmişini gözden geçirme.
> * Betikleri yerel bir Docker ortamında yürütme.
> * Betikleri yerel bir Azure CLI penceresinde yürütme.
> * Betikleri uzak bir Docker ortamında yürütme.
> * Betikleri bulut üzerindeki HDInsight ortamında yürütme.

Artık serinin üçüncü bölümüne geçmeye hazırsınız. Burada lojistik regresyon modelini oluşturdunuz, şimdi de bunu gerçek zamanlı bir web hizmeti olarak dağıtalım.

> [!div class="nextstepaction"]
> [Model dağıtma](tutorial-classifying-iris-part-3.md)
