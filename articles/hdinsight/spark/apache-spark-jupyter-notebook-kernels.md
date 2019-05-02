---
title: Azure HDInsight Spark kümeleri Jupyter not defteri için çekirdekler
description: Azure HDInsight Spark kümelerinde kullanılabilen Jupyter notebook için PySpark PySpark3 ve Spark çekirdekler hakkında bilgi edinin.
keywords: spark, jupyter, spark üzerinde jupyter notebook
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: fed8791fbc7cc7f049a1161fb3903c7f6d42d4e8
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689303"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Azure HDInsight, Apache Spark kümeleri Jupyter not defteri için çekirdekler 

HDInsight Spark kümeleri Jupyter not defteri ile kullanabileceğiniz çekirdekler sağlar [Apache Spark](https://spark.apache.org/) uygulamalarınızı test etmek için. Bir çekirdek, çalışan ve kodunuzu yorumlar bir programdır. Üç çekirdek şunlardır:

- **PySpark** - Python2 içinde yazılmış uygulamalar için.
- **PySpark3** - Python3 dilinde yazılmış uygulamalar için.
- **Spark** - Scala içinde yazılmış uygulamalar için.

Bu makalede, bu çekirdekler ve bunları kullanmanın avantajları nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight, Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Spark HDInsight üzerinde bir Jupyter not defteri oluşturma

1. Gelen [Azure portalında](https://portal.azure.com/), kümenizi açın.  Bkz: [kümeleri Listele ve Göster](../hdinsight-administer-use-portal-linux.md#showClusters) yönergeler için. Kümeye yeni bir portal dikey penceresinde açılır.

2. Gelen **hızlı bağlantılar** bölümünde **küme panoları** açmak için **küme panoları** dikey penceresi.  Görmüyorsanız **hızlı bağlantılar**, tıklayın **genel bakış** dikey penceresinde sol menüden.

    ![Spark üzerinde Jupyter notebook](./media/apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Spark üzerinde Jupyter notebook") 

3. Tıklayın **Jupyter not defteri**. İstenirse, küme için yönetici kimlik bilgilerini girin.
   
   > [!NOTE]  
   > Spark kümesinde Jupyter not defterine aşağıdaki URL'yi tarayıcınızda açarak da ulaşabilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`


3. Tıklayın **yeni**ve ardından ya da **Pyspark**, **PySpark3**, veya **Spark** bir not defteri oluşturmak için. Spark Scala uygulamaları çekirdeğe, PySpark çekirdeği Python2 uygulamalar ve PySpark3 çekirdek Python3 uygulamalar için kullanın.
   
    ![Spark üzerinde Jupyter notebook için çekirdekler](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "için Spark üzerinde Jupyter not defteri çekirdekleri") 

4. Bir not defteri seçtiğiniz çekirdek ile açılır.

## <a name="benefits-of-using-the-kernels"></a>Çekirdekler kullanmanın avantajları

HDInsight Spark kümeleri Jupyter not defteri ile yeni çekirdekler kullanmanın bazı avantajları şunlardır.

- **Önceden ayarlanmış Bağlamlar**. İle **PySpark**, **PySpark3**, veya **Spark** çekirdekleri gerektirmeyen, uygulamalarla çalışmaya başlamadan önce Spark veya Hive bağlamları açıkça ayarlamak. Bunlar, varsayılan olarak kullanılabilir. Şu bağlamlarda şunlardır:
   
  * **sc** - Spark bağlamı için
  * **sqlContext** - Hive bağlamı için
   
    Bu nedenle, gibi içerikler için aşağıdaki deyimleri çalıştırın gerekmez:
   
         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)
   
    Bunun yerine, önceden ayarlanmış Bağlamlar doğrudan uygulamanızda kullanabilirsiniz.

- **Hücre işlevlerini**. PySpark çekirdeği bazı önceden tanımlanmış "işlevlerini" ile çağırabileceğiniz özel komutlar olduğu sağlar `%%` (örneğin, `%%MAGIC` `<args>`). Sihirli komutu bir kod hücresi içinde ilk sözcük ve içeriği birden fazla satır için izin gerekir. Sihirli word hücredeki ilk sözcük olmalıdır. Magic bile açıklamalar önce herhangi bir şey ekleme, bir hataya neden olur.     İşlevlerini hakkında daha fazla bilgi için bkz. [burada](https://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    Aşağıdaki tabloda, çekirdekler kullanılabilen farklı işlevlerini listeler.

   | Magic | Örnek | Açıklama |
   | --- | --- | --- |
   | Yardım |`%%help` |Örnek ve açıklaması ile kullanılabilir tüm işlevlerini bir tablo oluşturur |
   | bilgi |`%%info` |Çıkışlar oturum bilgilerini geçerli Livy uç noktası |
   | yapılandırılmak |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Oturum oluşturmak için parametreler yapılandırır. Force bayrağını (-f) bir oturum zaten, oturum bırakılan ve yeniden sağlayan oluşturulduysa zorunludur. Bakmak [Livy'nın POST /sessions istek gövdesi](https://github.com/cloudera/livy#request-body) için geçerli parametrelerin bir listesi. Parametreler, içinde bir JSON dizesi geçirilmelidir ve sonraki satırda Sihirli sonra örnek sütununda gösterildiği gibi olması gerekir. |
   | SQL |`%%sql -o <variable name>`<br> `SHOW TABLES` |Bir Hive sorgusu sqlContext karşı yürütür. Varsa `-o` parametresi geçirilir, sorgu sonucu kalıcı hale getirilir %% yerel Python bağlamı olarak bir [Pandas](https://pandas.pydata.org/) veri çerçevesi. |
   | yerel |`%%local`<br>`a=1` |Sonraki satırların tüm kodu yerel olarak yürütülür. Kod bile kullandığınız çekirdek bağımsız olarak geçerli Python2 kodu olmalıdır. Bu nedenle, seçtiyseniz, **PySpark3** veya **Spark** kullanırsanız, Not defterini oluşturulurken çekirdekler `%%local` Sihirli bir hücre, söz konusu hücrenin yalnızca geçerli Python2 kodunuz olmalıdır... |
   | günlükler |`%%logs` |Geçerli oturumda Livy günlükleri çıkarır. |
   | delete |`%%delete -f -s <session number>` |Belirli bir oturum geçerli Livy uç noktasının siler. Başlatılan oturum çekirdek için silinemiyor. |
   | temizle |`%%cleanup -f` |Bu not defterinin oturumunu de dahil olmak üzere geçerli Livy uç noktası için tüm oturumları siler. Force bayrağını -f zorunludur. |

   > [!NOTE]  
   > De kullanabilirsiniz işlevlerini yanı sıra PySpark çekirdeği tarafından eklenen [yerleşik Ipython işlevlerini](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics)de dahil olmak üzere `%%sh`. Kullanabileceğiniz `%%sh` kümesi baş düğümünde betikleri ve kod bloğunu çalıştırmak için magic.

1. **Otomatik görselleştirme**. **Pyspark** çekirdek, Hive ve SQL sorgusu çıkışını otomatik olarak görselleştirir. Birkaç farklı türde görselleştirmeler içeren tablo, pasta, çizgi, alan, çubuğu arasında seçim yapabilirsiniz.

## <a name="parameters-supported-with-the-sql-magic"></a>Parametreler ile desteklenen %% sql Sihri
`%%sql` Sihirli sorguları çalıştırdığınızda, aldığınız çıktı türünü denetlemek için kullanabileceğiniz farklı parametreleri destekler. Aşağıdaki tabloda çıkış listeler.

| Parametre | Örnek | Açıklama |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Sorgu sonucu sızmak için bu parametreyi kullanın %% yerel Python bağlamı olarak bir [Pandas](https://pandas.pydata.org/) veri çerçevesi. Belirttiğiniz bir değişken adı dataframe değişkenin adıdır. |
| -q |`-q` |Görselleştirmeler hücre için devre dışı bırakmak için bunu kullanın. Bir hücrenin içeriğinin otomatik olarak görselleştirin ve yalnızca bir veri çerçevesi yakalayabilir ve ardından kullanmak istediğiniz istemiyorsanız `-q -o <VARIABLE>`. Sonuçları yakalama olmadan görselleştirmeler etkinleştirmek istiyorsanız (örneğin, bir SQL sorgusu gibi çalışan bir `CREATE TABLE` deyimi), kullanın `-q` belirtmeden bir `-o` bağımsız değişken. |
| -m |`-m <METHOD>` |Burada **yöntemi** ya da **ele** veya **örnek** (varsayılan değer **ele**). Yöntem ise **ele**, çekirdek MAXROWS (daha sonra bu tabloda açıklanmıştır) tarafından belirtilen sonuç veri kümesini üst öğelerinden seçer. Yöntem ise **örnek**, çekirdek rastgele öğeler göre veri kümesinin örnekleri `-r` parametresi, ardından bu tabloda açıklanır. |
| -r |`-r <FRACTION>` |Burada **KESİR** 0,0 ile 1,0 arasında kayan noktalı bir sayıdır. SQL sorgusu için örnek yöntem ise `sample`, çekirdek için ayarladığınız sonucun öğelerin belirtilen kesir rastgele örnekler sonra. Örneğin, bir SQL sorgusu bağımsız değişkenleriyle Çalıştır `-m sample -r 0.01`, %1 sonucu satırların rastgele Örneklendi ve sonra. |
| -n |`-n <MAXROWS>` |**MAXROWS** bir tamsayı değeri. Çekirdek çıkış satır sayısını sınırlayan **MAXROWS**. Varsa **MAXROWS** negatif bir sayı olduğu gibi **-1**, sonuç kümesinde satır sayısı sınırlı değildir. |

**Örnek:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

Yukarıdaki ifade şunları yapar:

* Tüm kayıtları seçer **hivesampletable**.
* -Q, kullandığımızdan otomatik görselleştirme devre dışı bırakır.
* Kullandığımızdan `-m sample -r 0.1 -n 500` rastgele %10 hivesampletable satırların örnekleri ve sonuç için 500 satır kümesi boyutunu sınırlar.
* Son olarak, kullandık çünkü `-o query2` de adlı dataframe'e çıktıyı kaydeder **sorgu2**.

## <a name="considerations-while-using-the-new-kernels"></a>Yeni çekirdekler kullanırken dikkat edilmesi gerekenler

Kullandığınız hangi çekirdek çalıştıran not defterlerini bırakarak küme kaynaklarını tüketir.  Bu çekirdekleri ile bağlamları hazır olduğundan, basitçe not defterlerini çıkmadan bağlamı sonlandırmaz ve küme kaynaklarını kullanımda bu nedenle devam eder. İyi bir uygulama kullanmaktır **Kapat ve Durdur** not defterinin seçeneğinden **dosya** bağlam sonlandırır ve not defteri çıkar Not Defteri kullanarak tamamladığınızda menüsü.     

## <a name="show-me-some-examples"></a>Bazı örnekler Göster

Jupyter not defteri açtığınızda iki klasör kullanılabilir kök düzeyinde bakın.

* **PySpark** klasörü yeni örnek not defterleri bulunur **Python** çekirdek.
* **Scala** klasörü yeni örnek not defterleri bulunur **Spark** çekirdek.

Açabileceğiniz **00 - [okuma BENİ ilk] Spark Sihirli çekirdek Özellikler** not defterinden **PySpark** veya **Spark** kullanılabilir farklı işlevlerini hakkında bilgi edinmek için klasör. HDInsight Spark kümeleri ile Jupyter not defterlerini kullanarak farklı senaryoları elde öğrenmek için iki klasör altında kullanılabilir başka bir örnek dizüstü de kullanabilirsiniz.

## <a name="where-are-the-notebooks-stored"></a>Not defterlerini nerede depolanır?

Kümenizi Azure depolama varsayılan depolama hesabı olarak kullanıyorsa, Jupyter not defterleri altında depolama hesabına kaydedilir **/HdiNotebooks** klasör.  Not defterleri, metin dosyalarını ve Jupyter içinde oluşturduğunuz klasörler, depolama hesabından erişilebilir.  Örneğin, bir klasör oluşturmak için Jupyter kullanırsanız **Klasörüm'ün** ve bir not defteri **myfolder/mynotebook.ipynb**, bu not defteri adresindeki erişebileceğiniz `/HdiNotebooks/myfolder/mynotebook.ipynb` depolama hesabında.  Depolama hesabınıza doğrudan bir not defteri yüklerseniz tersi de diğer bir deyişle, true ise `/HdiNotebooks/mynotebook1.ipynb`, Jupyter not defterini de görülebilir.  Hatta kümesi silindikten sonra not defterleri depolama hesabında kalır.

> [!NOTE]  
> Varsayılan depolama alanı olarak Azure Data Lake Store ile HDInsight kümeleri, not defterlerini ilişkili depolama alanında depolamayın.

Not defterleri, depolama hesabına kaydedilir şekilde uyumludur [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Bu nedenle, aşağıdaki kod parçacığında gösterildiği gibi kullanabileceğiniz kümesine SSH yönetimi komutları dosya:

    hdfs dfs -ls /HdiNotebooks                               # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter

Olup küme Azure Depolama'da veya Azure Data Lake Storage varsayılan depolama hesabı olarak kullanan bağımsız olarak not defterleri de küme baş düğümüne kaydedilir `/var/lib/jupyter`.

## <a name="supported-browser"></a>Desteklenen tarayıcı

HDInsight Spark kümeleri Jupyter not defterleri yalnızca Google Chrome üzerinde desteklenir.

## <a name="feedback"></a>Geri Bildirim
Yeni çekirdekler aşama gelişen içinde olan ve zaman içinde yetişkin. Bu da bu çekirdekler yetişkin olarak API'leri değişebilir gelebilir. Bu yeni çekirdekler kullanırken sahip olduğunuz herhangi bir Geri bildiriminiz veriyoruz. Bu, bu çekirdekler'ın son sürümünde şekillendirmeye kullanışlıdır. Yorumlar/Görüşleriniz altında bırakabilirsiniz **açıklamaları** bu makalenin alt kısmındaki bölümde.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
