---
title: Azure HDInsight araçları - Visual Studio Code için Hive, LLAP veya pySpark kullanın
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
author: jejiang
editor: jasonwhowell jgao
ms.service: HDInsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 7bf74155cba65d2b5abdc80103a46047aec1b5b7
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592409"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code için Azure HDInsight araçları kullanma

Oluşturma ve Hive toplu işlerini, etkileşimli Hive sorgularını ve pySpark betiklerini Gönder (VS Code) Visual Studio Code için Azure HDInsight araçları kullanmayı öğrenin. Azure HDInsight araçları, VS Code tarafından desteklenen platformlardaki yüklenebilir. Bunlar arasında Windows, Linux ve MacOS yer alır. Farklı platformlar için önkoşulları bulabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakiler gereklidir:

- Bir HDInsight kümesi. Bir küme oluşturmak için bkz: [HDInsight ile çalışmaya başlama]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono, yalnızca Linux ve macOS için gereklidir.

## <a name="install-the-hdinsight-tools"></a>HDInsight Araçları'nı yükleme
   
Önkoşullar yüklendikten sonra VS Code için Azure HDInsight araçları yükleyebilirsiniz. 

**Yükleme, Azure HDInsight araçları**

1. Visual Studio Code'u açın.

2. Sol bölmede seçin **uzantıları**. Arama kutusuna **HDInsight**.

3. Yanındaki **Azure HDInsight Araçları**seçin **yükleme**. Birkaç saniye sonra **yükleme** düğmesi değişiklikleri **yeniden**.

4. Seçin **yeniden** etkinleştirmek için **Azure HDInsight Araçları** uzantısı.

5. Seçin **Reload Window** onaylamak için. Gördüğünüz **Azure HDInsight Araçları** içinde **uzantıları** bölmesi.

   ![HDInsight için Visual Studio kod Python yükleme](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>HDInsight çalışma alanını açın

Azure'a bağlanabilmesi için VS Code'da bir çalışma alanı oluşturun.

**Bir çalışma alanını açmak için**

1. Üzerinde **dosya** menüsünde **Klasör Aç**. Ardından iş klasörünüz olarak var olan bir klasörü belirtin veya yeni bir tane oluşturun. Klasör, sol bölmede görünür.

2. Sol bölmeden **yeni dosya** iş klasörü yanındaki simge.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)

3. ' % S '.hql (Hive sorguları) veya .py (Spark betiğini) dosya uzantısı ile yeni dosya adı. Dikkat bir **XXXX_hdi_settings.json** yapılandırma dosyası için iş klasörünün otomatik olarak eklenir.

4. Açık **XXXX_hdi_settings.json** gelen **GEZGİNİ**, ya da Kod Düzenleyicisi'ni seçmek için sağ **yapılandırma kümesi**. Oturum açma girdisi ve varsayılan küme dosya örnekte gösterildiği gibi iş gönderme parametresi yapılandırabilirsiniz. Kalan parametreler boş da bırakabilirsiniz.

## <a name="connect-to-hdinsight-cluster"></a>HDInsight kümesine bağlanma

VS koddan betikleri HDInsight kümelerine gönderebilirsiniz önce Azure hesabınıza bağlanın veya bir kümesini bağlamak gerekir (Ambari kullanarak kullanıcı adı/parola veya etki alanı hesabı alanına).

**Azure'a bağlanmak için**

1. Bunları henüz yoksa, yeni bir çalışma klasörü ve yeni betik dosyası oluşturun.

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından bağlam menüsünde seçin **HDInsight: oturum açma**. Ayrıca girebilirsiniz **Ctrl + Shift + P**yazıp enter **HDInsight: oturum açma**.

    ![Visual Studio Code için HDInsight araçları oturum açın](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Oturum açmak için oturum açma yönergeleri izleyin. **çıkış** bölmesi.

    **Azure:** ![oturum açma bilgisi Visual Studio Code için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Bağlandıktan sonra Azure hesap adınızı VS Code penceresinin sol alt durum çubuğunda gösterilir. 

    > [!NOTE]
    > Bir bilinen Azure kimlik doğrulama sorunu nedeniyle bir tarayıcı özel modda veya gizli modda açmanız gerekir. Azure hesabınızı etkin iki faktör varsa, PIN kimlik doğrulaması yerine telefon kimlik doğrulaması kullanılması önerilir.
  

4. Kod Düzenleyicisi'ni bağlam menüsünü açmak için sağ tıklayın. Bağlam menüsünden aşağıdaki görevleri gerçekleştirebilirsiniz:

    - Oturumu kapat
    - Kümeleri listeleme
    - Kümesi varsayılan kümeleri
    - Etkileşimli Hive sorguları gönderme
    - Hive toplu iş betikleri gönderme
    - PySpark etkileşimli sorguları gönderme
    - PySpark toplu betiklerden gönderin
    - Küme yapılandırmaları

<a id="linkcluster"></a>**Bir kümeye bağlanmak için**

Ambari yönetilen kullanıcı adı kullanarak normal bir küme bağlantısını, ayrıca etki alanı kullanıcı adı kullanarak bir güvenlik hadoop kümesini bağlamak (örneğin: user1@contoso.com).
1. Seçerek komut paletini açın **CTRL + SHIFT + P**yazıp enter **HDInsight: küme bağlantı**.

   ![bağlantı kümesi komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. HDInsight girin küme URL'si, kullanıcı adı -> giriş -> giriş parola seçin -> küme türü -> bunu gösterir başarı bilgisi doğrulama geçirilmiş.
   
   ![bağlantı kümesi iletişim kutusu](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanılır. 
   
3. Komutunu kullanarak bir bağlı küme görebilirsiniz **listesi küme**. Artık bağlantılı bu küme için bir komut gönderebilirsiniz.

   ![bağlı küme](./media/hdinsight-for-vscode/linked-cluster.png)

4. Ayrıca, girişini yaparak tarafından bir küme kesebilir **HDInsight: küme bağlantısını** komut paletinden.

## <a name="list-hdinsight-clusters"></a>HDInsight kümeleri listeleme

Bağlantıyı sınamak için HDInsight kümelerinizi listeleyebilirsiniz:

**HDInsight kümeleri, Azure aboneliği altında listelemek için**
1. Bir çalışma alanını açın ve ardından Azure'a bağlanın. Daha fazla bilgi için [açık HDInsight çalışma alanı](#open-hdinsight-workspace) ve [Azure'a bağlanma](#connect-to-azure).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: liste küme** bağlam menüsünden. 

3. Hive ve Spark kümeleri görünür **çıkış** bölmesi.

    ![Bir varsayılan küme yapılandırmasını ayarlayın](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Bir varsayılan küme
1. Bir çalışma alanını açın ve Azure'a bağlanın. Bkz: [açık HDInsight çalışma alanı](#open-hdinsight-workspace) ve [Azure'a bağlanma](#connect-to-azure).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: varsayılan küme**. 

3. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçları yapılandırma dosyasını otomatik olarak güncelleştirmek **XXXX_hdi_settings.json**. 

   ![Kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Azure ortamı ayarlama 
1. Seçerek komut paletini açın **CTRL + SHIFT + P**.

2. Girin **HDInsight: Azure ortamı ayarlama**.

3. Bir yolu, Azure ve AzureChina varsayılan oturum açma giriş seçin.

4. Bu arada, araç, varsayılan oturum açma girişi zaten kaydetti **XXXX_hdi_settings.json**. Ayrıca doğrudan bu yapılandırma dosyasında güncelleştirmeniz. 

   ![Kümesi varsayılan oturum açma girişi yapılandırma](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Etkileşimli Hive sorguları gönderme

VS Code için HDInsight araçları ile etkileşimli Hive sorguları HDInsight etkileşimli sorgu kümelerine gönderebilirsiniz.

1. Yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.

2. Azure hesabınıza bağlanın ve henüz yapmadıysanız varsayılan kümeyi yapılandırın.

3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Betik düzenleyiciye sağ tıklayın ve **HDInsight: Hive Interactive** seçeneğini belirleyerek sorguyu gönderin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Kısa süre içinde sorgu sonuçları yeni bir sekmede görüntülenir.

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **SONUÇLAR** paneli: Sonucun tamamını CSV, JSON veya Excel dosyası olarak yerel yola kaydedebilir veya yalnızca birkaç satır seçebilirsiniz.

    - **İLETİLER** paneli: **Satır** numarasını seçtiğinizde, çalıştırılmakta olan betiğin birinci satırına atlar.

Etkileşimli sorgu çalıştırılması, [bir Hive toplu işinin çalıştırılmasından](#submit-hive-batch-scripts) daha kısa sürer.

## <a name="submit-hive-batch-scripts"></a>Hive toplu iş betikleri gönderme

1. Yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.

2. Azure hesabınıza bağlanın ve henüz yapmadıysanız varsayılan kümeyi yapılandırın.

3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Betik düzenleyiciye sağ tıklayın ve **HDInsight: Hive Toplu İş** seçeneğini belirleyerek bir Hive işi gönderin. 

4. Göndermek istediğiniz kümeyi seçin.  

    Bir Hive işi göndermenizin ardından, **ÇIKTI** panelinde gönderim başarısı bilgileri ve iş kimliği görüntülenir. Hive işi, gerçek zamanlı iş günlüklerini ve durumunu gösteren **WEB TARAYICISI**’nı da açar.

   ![Hive işi sonucu gönderme](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

[Etkileşimli Hive sorguları gönderme](#submit-interactive-hive-queries), toplu iş göndermeden çok daha kısa sürer.

## <a name="submit-interactive-pyspark-queries"></a>PySpark etkileşimli sorguları gönderme
VS Code için HDInsight araçları, etkileşimli PySpark sorgular kümelerine Spark göndermenize olanak sağlar.
1. Bunları henüz yoksa, yeni bir çalışma klasörü ve yeni betik dosyası .py uzantısıyla oluşturun.

2. Henüz yapmadıysanız, Azure hesabınıza bağlanın.

3. Kopyalayıp betik dosyasına aşağıdaki kodu yapıştırın:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Bu betikler vurgulayın. Ardından Kod Düzenleyicisi'ni sağ tıklatın ve seçin **HDInsight: PySpark etkileşimli**.

5. Henüz yüklemediyseniz **Python** VS code'da uzantısı seçin **yükleme** düğme aşağıdaki çizimde gösterildiği gibi:

    ![HDInsight için Visual Studio kod Python yükleme](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Henüz yapmadıysanız sisteminizde Python ortamını yükleyin. 
   - Windows için indirme ve yükleme [Python](https://www.python.org/downloads/). Ardından emin `Python` ve `pip` , sistem yolu.

   - MacOS ve Linux için yönergeler için bkz: [Visual Studio Code için PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md).

7. PySpark sorgunuzu gönderebilirsiniz yapılacak bir kümeyi seçin. Hemen sonra sorgu sonucu yeni doğru sekmede gösterilmektedir:

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Aracı'nı da destekler **SQL yan tümcesi** sorgu.

   ![Python iş sonucu gönderme](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) alt durum sorguları zaman çalıştırdığınız çubuğunda sol taraftaki gönderme durumu görüntülenir. Diğer sorgular durum olduğunda paylaşmayın **PySpark Çekirdeği (meşgul)**. 

>[!NOTE]
>Küme oturum bilgilerini koruyabilir. Aynı kümenin birden çok hizmet çağrısını üzerinde başvurulabilen şekilde tanımlanmış değişken, işlev ve karşılık gelen değerler oturumda tutulur. 
 

## <a name="submit-pyspark-batch-job"></a>PySpark batch işi gönderme

1. Bunları henüz yoksa, yeni bir çalışma klasörü ve yeni betik dosyası .py uzantısıyla oluşturun.

2. Zaten yapmadıysanız, Azure hesabınıza bağlanın.

3. Kopyalayıp betik dosyasına aşağıdaki kodu yapıştırın:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: PySpark Batch**. 

5. Hangi, PySpark işi göndermek bir kümeyi seçin. 

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Bir Python işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS code'da penceresi. **Spark UI URL** ve **Yarn UI URL** de gösterilir. URL, iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.

>[!NOTE]
>PySpark3 (HDI spark 2.2 kümesi olan) Livy içinde 0.4 artık desteklenmiyor. Yalnızca "PySpark" python için desteklenir. Spark 2.2 için gönderme sorun başarısız ile python3 bilinir.
   
## <a name="livy-configuration"></a>Livy yapılandırma
Livy yapılandırması desteklenir, çalışma alanı klasörü içinde proje ayarları ayarlanabilir. Daha fazla bilgi [Livy Benioku](https://github.com/cloudera/livy/blob/master/README.rst ).

+ Proje ayarları:

    ![Livy yapılandırma](./media/hdinsight-for-vscode/hdi-livyconfig.png)

+ Desteklenen Livy yapılandırmalar:   

    **POST /batches**   
    İstek Gövdesi

    | ad | açıklama | type | 
    | :- | :- | :- | 
    | dosya | Yürütülecek uygulamanın içeren dosya | yol (gerekli) | 
    | Proxyuserpassword | İş çalışırken bürünülecek kullanıcı | dize | 
    | className | Uygulamanın Java/Spark temel sınıfı | dize |
    | args | Uygulama için komut satırı bağımsız değişkenleri | dize listesi | 
    | jar dosyaları dışındaki | Bu oturumda kullanılmak üzere jar'lar | Dize listesi | 
    | pyFiles | Bu oturumda kullanılmak üzere Python dosyaları | Dize listesi |
    | dosya görüntüle | Bu oturumda kullanılmak üzere dosyaları | Dize listesi |
    | driverMemory | Sürücü işlemi için kullanılacak bellek miktarını | dize |
    | driverCores | Sürücü işlemi için kullanılacak çekirdek sayısı | int |
    | executorMemory | Yürütücü işlemi bellek miktarı | dize |
    | executorCores | Her Yürütücü için kullanılacak çekirdek sayısı | int |
    | numExecutors | Bu oturum için başlatmak için Yürütücü sayısı | int |
    | arşivleri | Bu oturumda kullanılmak üzere arşivleri | Dize listesi |
    | kuyruk | YARN Kuyruğun adı gönderildi | dize |
    | ad | Bu oturumun adı | dize |
    | conf | Spark yapılandırma özellikleri | Harita anahtarı val = |

    Yanıt Gövdesi   
    Oluşturulan toplu iş nesnesi.

    | ad | açıklama | type | 
    | :- | :- | :- | 
    | id | Oturum kimliği | int | 
    | Uygulama Kimliği | Bu oturumun uygulama kimliği |  Dize |
    | appInfo | Ayrıntılı uygulama bilgileri | Harita anahtarı val = |
    | Günlük | Günlük satırları | dize listesi |
    | durum |   Toplu işlem durumu | dize |


## <a name="additional-features"></a>Ek Özellikler

VS Code için HDInsight aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Anahtar sözcüğü, yöntemleri, değişkenler vb. için öneriler açılır. Farklı simgeler farklı türde nesneyi temsil eder.

    ![Visual Studio kod IntelliSense nesne türleri için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaret**. Dil hizmeti ve Hive betiğinin düzenleme hatalarla altını çizip çizmeyeceğini.     
- **Söz dizimi vurgular**. Dil hizmeti, değişkenleri, anahtar sözcükler, veri türü, İşlevler ve benzeri ayırt etmek için farklı renkler kullanır. 

    ![Söz dizimi önemli Visual Studio Code için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* VS Code için HDInsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak amacıyla Intellij için Azure Araç Seti'ni kullanma](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçlarını kullanın](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight’ta Microsoft Power BI ile Hive verileri görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Visual Studio Code için PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md)
* [Azure HDInsight Hive sorguları çalıştırmak için Zeppelin'i kullanma ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](spark/apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](spark/apache-spark-job-debugging.md)



