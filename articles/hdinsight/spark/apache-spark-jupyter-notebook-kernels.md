---
title: Azure hdınsight'ta Spark kümeleri Jupyter not defteri için tekrar | Microsoft Docs
description: Kullanılabilir Azure hdınsight'ta Spark kümeleri ile Jupyter not defteri PySpark, PySpark3 ve Spark tekrar hakkında bilgi edinin.
keywords: spark, jupyter spark üzerinde jupyter not defteri
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0719e503-ee6d-41ac-b37e-3d77db8b121b
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: nitinme
ms.openlocfilehash: 58a0bf27109af3131bd102fd43e9367d267525f3
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31521543"
---
# <a name="kernels-for-jupyter-notebook-on-spark-clusters-in-azure-hdinsight"></a>Azure hdınsight'ta Spark kümeleri Jupyter not defteri için tekrar 

Hdınsight Spark kümeleri uygulamalarınızı test etme için Spark üzerinde Jupyter not defteri ile kullanabileceğiniz çekirdek sağlar. Bir çekirdek çalıştıran ve kodunuzu yorumlar bir programdır. Üç tekrar şunlardır:

- **PySpark** - Python2 içinde yazılmış uygulamalar için
- **PySpark3** - Python3 içinde yazılmış uygulamalar için
- **Spark** - Scala içinde yazılmış uygulamalar için

Bu makalede, bu tekrar ve bunları kullanmanın avantajları nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Spark Hdınsight üzerinde bir Jupyter not defteri oluşturma

1. Gelen [Azure portal](https://portal.azure.com/), kümenizi açın.  Bkz: [listesi ve Göster kümeleri](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters) yönergeleri için. Kümeye yeni bir portal dikey penceresinde açılır.

2. Gelen **hızlı bağlantılar** 'yi tıklatın **küme panolar** açmak için **küme panolar** dikey.  Görmüyorsanız, **hızlı bağlantılar**, tıklatın **genel bakış** dikey sol menüden.

    ![Jupyter not defteri Spark üzerinde](./media/apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Spark üzerinde Jupyter not defteri") 

3. Tıklatın **Jupyter not defteri**. İstenirse, küme için yönetici kimlik bilgilerini girin.
   
   > [!NOTE]
   > Tarayıcınızda aşağıdaki URL'yi açarak Spark kümesinde Jupyter Not Defteri de ulaşabilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

3. Tıklatın **yeni**ve ardından ya da **Pyspark**, **PySpark3**, veya **Spark** bir not defteri oluşturmak için. Spark Scala uygulamaları için çekirdek, PySpark çekirdeği Python2 uygulamaları için ve PySpark3 çekirdek Python3 uygulamalar için kullanın.
   
    ![Spark üzerinde Jupyter not defteri için tekrar](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "için Spark Jupyter not defterlerinde çekirdekler") 

4. Bir not defteri seçtiğiniz çekirdek ile açılır.

## <a name="benefits-of-using-the-kernels"></a>Tekrar kullanmanın yararları

Spark Hdınsight kümeleri Jupyter not defteri ile yeni tekrar kullanmanın bazı avantajları şunlardır.

- **Bağlamları önceden**. İle **PySpark**, **PySpark3**, veya **Spark** çekirdekleri, gerek yoktur, uygulamalarla çalışmaya başlamadan önce Spark veya Hive bağlamları açıkça ayarlayın. Bu, varsayılan olarak kullanılabilir. Bu içerikler şunlardır:
   
   * **sc** - Spark bağlamı için
   * **sqlContext** - Hive bağlamı için
   
   Bu nedenle, deyimlerini bağlamları ayarlamak için aşağıdaki gibi çalıştırın gerekmez:
   
          sc = SparkContext('yarn-client')
          sqlContext = HiveContext(sc)
   
   Bunun yerine, önceden ayarlanmış bağlamları doğrudan uygulamanızda kullanabilirsiniz.

- **Hücre sihirleri**. Bazı önceden tanımlanmış "sihirleri" ile çağırabilir özel komutlar olduğu PySpark çekirdeği sağlar `%%` (örneğin, `%%MAGIC` <args>). Sihirli komutu kod hücresini ilk sözcüğü ve içeriği için birden çok satır izin gerekir. Sihirli word hücre ilk Word'de olmalıdır. Hatta açıklamaları Sihirli önce herhangi bir şey ekleme bir hataya neden olur.     Sihirler hakkında daha fazla bilgi için bkz: [burada](http://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    Aşağıdaki tabloda farklı sihirler tekrar kullanılabilir listeler.

   | Özel numarası | Örnek | Açıklama |
   | --- | --- | --- |
   | yardım |`%%help` |Örnek ve açıklama ile tüm kullanılabilir sihirler oluşan bir tablo oluşturur |
   | bilgileri |`%%info` |Geçerli Livy uç noktası için çıktıları oturum bilgilerini |
   | yapılandır |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Oturum oluşturma için parametre yapılandırır. Force bayrağını (-f) bir oturum zaten, oturumun bırakılan ve yeniden sağlayan oluşturulduysa zorunludur. Bakmak [Livy'nın POST /sessions iste gövde](https://github.com/cloudera/livy#request-body) için geçerli parametrelerin bir listesi. Parametreleri JSON dizesi olarak geçirilmesi gerekir ve bir sonraki satırında, örnek sütununda gösterildiği gibi Sihirli sonra olması gerekir. |
   | SQL |`%%sql -o <variable name>`<br> `SHOW TABLES` |Bir Hive sorgusu sqlContext yürütür. Varsa `-o` parametresi geçirilir, sorgunun sonucu kalıcı hale getirilir %% yerel Python bağlamı olarak bir [Pandas](http://pandas.pydata.org/) dataframe. |
   | yerel |`%%local`<br>`a=1` |Sonraki satırların tüm kodda yerel olarak yürütülür. Kodu dahi, kullanmakta olduğunuz çekirdek yedeklemiş geçerli Python2 kodu olmalıdır. Bu nedenle, seçtiğiniz olsa bile **PySpark3** veya **Spark** kullanırsanız, Not Defteri oluşturma sırasında tekrar `%%local` Sihirli bir hücreye, o hücre yalnızca geçerli Python2 kod olmalıdır... |
   | günlükler |`%%logs` |Günlükleri geçerli Livy oturumu için çıkarır. |
   | sil |`%%delete -f -s <session number>` |Belirli bir oturum geçerli Livy uç noktasının siler. Başlatılan oturum çekirdek için silinemiyor. |
   | temizle |`%%cleanup -f` |Bu not defterinin oturum dahil olmak üzere geçerli Livy uç noktası için tüm oturumları siler. Force bayrağını -f zorunludur. |

   > [!NOTE]
   > PySpark çekirdeği tarafından eklenen sihirleri ek olarak da kullanabilirsiniz [yerleşik IPython sihirler](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics)gibi `%%sh`. Kullanabileceğiniz `%%sh` küme headnode betikleri ve kod bloğunu çalıştırmak için Sihirli.
   >
   >
2. **Otomatik görselleştirme**. **Pyspark** çekirdek otomatik olarak Hive ve SQL sorguları çıktısını visualizes. Tablo, pasta, satır, alan, çubuk dahil olmak üzere görselleştirmeleri birkaç farklı türde arasında seçim yapabilirsiniz.

## <a name="parameters-supported-with-the-sql-magic"></a>Desteklenen parametreler %% sql Sihirli
`%%sql` Sihirli sorguları çalıştırdığınızda, aldığınız çıktı türünü denetlemek için kullanabileceğiniz farklı parametreleri destekler. Aşağıdaki tabloda çıkış listeler.

| Parametre | Örnek | Açıklama |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Sorgunun sonucu olarak devam ettirmek için bu parametreyi kullanın %% yerel Python bağlamı olarak bir [Pandas](http://pandas.pydata.org/) dataframe. Belirttiğiniz değişken adı dataframe değişkenin adıdır. |
| -q |`-q` |Hücre görsel kapatmak için bunu kullanın. Bir hücrenin içeriğinin otomatik görselleştirmek ve yalnızca bir dataframe yakalamak ve ardından kullanmak istediğiniz istemiyorsanız `-q -o <VARIABLE>`. Sonuçları yakalama olmadan görselleştirmeleri devre dışı bırakma istiyorsanız (örneğin, bir SQL sorgusu gibi çalıştırmak için bir `CREATE TABLE` deyimi), kullanın `-q` belirtmeden bir `-o` bağımsız değişkeni. |
| -m |`-m <METHOD>` |Burada **yöntemi** ya **ele** veya **örnek** (varsayılan değer **ele**). Yöntem ise **ele**, çekirdek MAXROWS (daha sonra bu tabloda açıklanan) tarafından belirtilen sonuç veri kümesinin üst öğeden seçer. Yöntem ise **örnek**, çekirdek rastgele göre veri kümesinin öğeleri örnekleri `-r` sonraki bu tabloda açıklanan parametresi. |
| -r |`-r <FRACTION>` |Burada **KESİR** 0,0 ile 1,0 arasında bir kayan noktalı sayı. SQL sorgusu için örnek yöntemi ise `sample`, çekirdek için ayarladığınız sonuç öğelerinin belirtilen kesir rastgele örnekler sonra. Örneğin, bir SQL sorgusu bağımsız değişkenlerle çalıştırırsanız `-m sample -r 0.01`, %1 sonuç satır rastgele örneklenen sonra. |
| -n |`-n <MAXROWS>` |**MAXROWS** bir tamsayı değil. Çekirdek çıkış satır sayısını sınırlar **MAXROWS**. Varsa **MAXROWS** negatif bir sayı olduğu gibi **-1**, sonuç kümesi satır sayısı sınırlı değildir. |

**Örnek:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

Yukarıdaki ifade şunları yapar:

* Tüm kayıtları seçer **hivesampletable**.
* -Q, kullandığımız için otomatik görselleştirme devre dışı bırakır.
* Biz kullandığından `-m sample -r 0.1 -n 500` rastgele örnekler % 10 ' hivesampletable satırları ve sonuç 500 satır kümesi boyutu sınırlar.
* Son olarak, biz kullanıldığından `-o query2` de adlı bir dataframe çıktıyı kaydeder **sorgu2**.

## <a name="considerations-while-using-the-new-kernels"></a>Yeni tekrar kullanırken dikkat edilecek noktalar

Kullandığınız, hangi çekirdek çalıştıran not defterlerini bırakarak küme kaynaklarını kullanır.  Bu çekirdekleri ile bağlamları hazır olduğundan, sadece not defterlerini çıkma bağlam KILL değil ve küme kaynaklarını kullanımda bu nedenle devam eder. İyi bir uygulama kullanmaktır **Kapat ve Durdur** not defterinin seçeneğinden **dosya** bağlam sonlandırır ve not defteri çıkar Not Defteri kullanarak bittiğinde menüsü.     

## <a name="show-me-some-examples"></a>Bazı örnekler Göster

Jupyter not defteri açtığınızda, iki klasör kullanılabilir kök düzeyinde görürsünüz.

* **PySpark** klasörü bulunan yeni örnek not defterlerini **Python** çekirdek.
* **Scala** klasörü bulunan yeni örnek not defterlerini **Spark** çekirdek.

Açabilirsiniz **00 - [okuma önce BENİ] Spark Sihirli çekirdek Özellikler** dizüstü bilgisayarınızı **PySpark** veya **Spark** kullanılabilir farklı sihirler hakkında bilgi edinmek için klasör. Hdınsight Spark kümeleri ile Jupyter not defterlerini kullanarak farklı senaryolar elde öğrenmek için iki klasör altında kullanılabilir diğer bir örnek dizüstü de kullanabilirsiniz.

## <a name="where-are-the-notebooks-stored"></a>Not defterlerini depolandığı?

Kümenizi varsayılan depolama hesabı olarak Azure Storage kullanıyorsa, Jupyter not defterleri depolama hesabı altında kaydedilir **/HdiNotebooks** klasör.  Dizüstü bilgisayarlar, metin dosyaları ve Jupyter içinde oluşturduğunuz klasörler depolama hesabından erişilebilir.  Örneğin, bir klasör oluşturmak için Jupyter kullanırsanız **Klasörüm'ün** ve dizüstü **myfolder/mynotebook.ipynb**, o not defteri konumunda erişebilirsiniz `/HdiNotebooks/myfolder/mynotebook.ipynb` depolama hesabındaki.  Doğrudan depolama hesabınız için bir not defteri karşıya ters ayrıca diğer bir deyişle, true ise `/HdiNotebooks/mynotebook1.ipynb`, Jupyter Not Defteri de görülebilir.  Küme bile silindikten sonra not defterlerini depolama hesabında kalır.

> [!NOTE]
> Hdınsight kümeleri varsayılan depolama alanı olarak Azure Data Lake Store ile ilişkili depolama not defterlerini depolamayın.
>

Dizüstü bilgisayarlar depolama hesabına kaydedilir ile HDFS uyumlu yoludur. Bunu, SSH kullanabilirsiniz kümesine yönetimi komutları aşağıdaki kod parçacığında gösterildiği gibi dosyası varsa:

    hdfs dfs -ls /HdiNotebooks                               # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter

Olup küme Azure Storage veya Azure Data Lake Store varsayılan depolama hesabı olarak kullanan bağımsız olarak, dizüstü bilgisayarlar ayrıca küme headnode kaydedilir `/var/lib/jupyter`.

## <a name="supported-browser"></a>Desteklenen tarayıcı

Spark Hdınsight kümeleri Jupyter not defterlerini yalnızca Google Chrome üzerinde desteklenir.

## <a name="feedback"></a>Geri Bildirim
Yeni tekrar aşama gelişen olan ve zaman içinde yetişkin. Bu aynı zamanda bu tekrar yetişkin olarak API'leri değişebilir anlamına gelebilir. Bu yeni tekrar kullanırken sahip herhangi bir geri bildirim veriyoruz. Bu, son sürümünde, bu tekrar şekillendirmeye yararlıdır. Yorumlar/geribildirim altında bırakabilirsiniz **açıklamaları** bu makalenin alt kısmına.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
