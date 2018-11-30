---
title: Azure HDInsight araçları - Visual Studio kodu Hive, LLAP veya PySpark kullanın | Microsoft Docs
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: 9603751db01eaffdf9fbe26164aed53017c5e23c
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499534"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code için Azure HDInsight araçları kullanma

Nasıl kullanacağınızı öğrenin [Visual Studio Code için Azure HDInsight Araçları](https://docs.microsoft.com/azure/hdinsight/hdinsight-for-vscode) oluşturmak ve göndermek amacıyla (VS Code) [Apache Hive](https://hive.apache.org/) batch işlerini, etkileşimli Apache Hive sorgularını ve PySpark betiklerini. Azure HDInsight araçları, VS Code tarafından desteklenen platformlardaki yüklenebilir. Bunlar arasında Windows, Linux ve MacOS yer alır. Farklı platformlar için önkoşulları bulabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakiler gereklidir:

- Bir HDInsight kümesi. Bir küme oluşturmak için bkz: [HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono, yalnızca Linux ve macOS için gereklidir.

## <a name="install-the-hdinsight-tools"></a>HDInsight Araçları'nı yükleme
   
Önkoşullar yüklendikten sonra VS Code için Azure HDInsight araçları yükleyebilirsiniz. 

### <a name="to-install-azure-hdinsight-tools"></a>Azure HDInsight Araçları'nı yüklemek için

1. Visual Studio Code'u açın.

2. Sol bölmede seçin **uzantıları**. Arama kutusuna **HDInsight**.

3. Yanındaki **Azure HDInsight Araçları**seçin **yükleme**. Birkaç saniye sonra **yükleme** düğmesi değişiklikleri **yeniden**.

4. Seçin **yeniden** etkinleştirmek için **Azure HDInsight Araçları** uzantısı.

5. Seçin **Reload Window** onaylamak için. Gördüğünüz **Azure HDInsight Araçları** içinde **uzantıları** bölmesi.

   ![HDInsight için Visual Studio kod Python yükleme](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>HDInsight çalışma alanını açın

Azure'a bağlanabilmesi için VS Code'da bir çalışma alanı oluşturun.

### <a name="to-open-a-workspace"></a>Bir çalışma alanını açmak için

1. Üzerinde **dosya** menüsünde **Klasör Aç**. Ardından iş klasörünüz olarak var olan bir klasörü belirtin veya yeni bir tane oluşturun. Klasör, sol bölmede görünür.

2. Sol bölmeden **yeni dosya** iş klasörü yanındaki simge.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)

3. ' % S '.hql (Hive sorguları) veya .py (Spark betiğini) dosya uzantısı ile yeni dosya adı. 

## <a name="connect-to-hdinsight-cluster"></a>HDInsight kümesine bağlanma

VS koddan betikleri HDInsight kümelerine gönderebilirsiniz önce Azure hesabınıza bağlanın veya bir kümesini bağlamak gerekir (Ambari kullanarak kullanıcı adı/parola veya etki alanı hesabı alanına).

### <a name="to-connect-to-azure"></a>Azure'a bağlanmak için

1. Bunları henüz yoksa, yeni bir çalışma klasörü ve yeni betik dosyası oluşturun.

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından bağlam menüsünde seçin **HDInsight: oturum açma**. Ayrıca girebilirsiniz **Ctrl + Shift + P**yazıp enter **HDInsight: oturum açma**.

    ![Visual Studio Code oturum açma için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Oturum açmak için oturum açma yönergeleri izleyin. **çıkış** bölmesi.
    + Genel ortam için Azure HDInsight oturum açma tetikleyecek işlemde oturum açın.

        ![Azure için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Diğer ortamlarda, oturum açma yönergelerini izleyin.

        ![Diğer ortam için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

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
    - Kümesi yapılandırması

<h3 id="linkcluster">Bir kümeye bağlanmak için</h3>

Normal bir küme kullanarak bağlayabilirsiniz bir [Apache Ambari](https://ambari.apache.org/) yönetilen kullanıcı adı veya bir etki alanı kullanıcı adı kullanarak bir kurumsal güvenlik paketi güvenli Hadoop kümesi bağlantısını (gibi: user1@contoso.com).
1. Seçerek komut paletini açın **CTRL + SHIFT + P**yazıp enter **HDInsight: küme bağlantı**.

   ![bağlantı kümesi komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. HDInsight girin küme URL'si, kullanıcı adı -> giriş -> giriş parola seçin -> küme türü -> bunu gösterir başarı bilgisi doğrulama geçirilmiş.
   
   ![bağlantı kümesi iletişim kutusu](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanılır. 
   
3. Komutunu kullanarak bir bağlı küme görebilirsiniz **listesi küme**. Artık bağlantılı bu küme için bir komut gönderebilirsiniz.

   ![bağlı küme](./media/hdinsight-for-vscode/linked-cluster.png)

4. Ayrıca, girişini yaparak tarafından bir küme kesebilir **HDInsight: küme bağlantısını** komut paletinden.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>Genel bir Apache Livy uç noktasını bağlamak için

1. Seçerek komut paletini açın **CTRL + SHIFT + P**yazıp enter **HDInsight: küme bağlantı**.
2. Seçin **genel Livy uç nokta**.
3. Genel Livy uç noktası girin. Örneğin: http://10.172.41.42:18080.
4. Seçin **temel** genel Livy uç noktası için Aksi takdirde yetkilendirme seçtiğinizde **hiçbiri**.
5. Giriş kullanıcı adı seçin, **temel** step4 içinde.
6. Giriş parola seçin, **temel** step4 içinde.
7. Genel livy uç noktası başarıyla bağlanıldı.

   ![Bağlantılı genel livy küme](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>HDInsight kümeleri listeleme

Bağlantıyı sınamak için HDInsight kümelerinizi listeleyebilirsiniz:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>HDInsight kümeleri, Azure aboneliği altında listelemek için
1. Bir çalışma alanını açın ve ardından Azure'a bağlanın. Daha fazla bilgi için [açık HDInsight çalışma alanı](#open-hdinsight-workspace) ve [Azure'a bağlanma](#connect-to-hdinsight-cluster).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: liste küme** bağlam menüsünden. 

3. HDInsight kümeleri görünür **çıkış** bölmesi.

    ![Bir varsayılan küme yapılandırmasını ayarlayın](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Bir varsayılan küme
1. Bir çalışma alanını açın ve Azure'a bağlanın. Bkz: [açık HDInsight çalışma alanı](#open-hdinsight-workspace) ve [Azure'a bağlanma](#connect-to-hdinsight-cluster).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: varsayılan küme**. 

3. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçları yapılandırma dosyasını otomatik olarak güncelleştirmek **. VSCode\settings.json**. 

   ![Kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Azure ortamı ayarlama
1. Seçerek komut paletini açın **CTRL + SHIFT + P**.

2. Girin **HDInsight: Azure ortamı ayarlama**.

3. "Azure" veya "AzureChina" varsayılan oturum açma giriş olarak gibi bir ortam seçin.

4. Bu arada, araç, varsayılan oturum açma girişi zaten kaydetti **. VSCode\settings.json**. Ayrıca doğrudan bu yapılandırma dosyasında güncelleştirmeniz. 

   ![Kümesi varsayılan oturum açma girişi yapılandırma](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Etkileşimli Hive sorguları göndermek, Hive toplu betiklerden

VS Code için HDInsight araçları ile etkileşimli Hive sorguları, Hive toplu betiklerden HDInsight kümelerine gönderebilirsiniz.

1. Yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.

2. Azure hesabı veya bağlantı kümeye bağlanın.

3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Kod Düzenleyicisi'ni sağ tıklayın, **HDInsight: etkileşimli Hive** sorgu gönderin veya kısayol **Ctrl + Alt + ı**. Seçin **HDInsight: Hive toplu** betiği gönderin veya kısayol **Ctrl + Alt + H**. 

5. Varsayılan Küme belirtmediyseniz kümeyi seçin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Sorgu sonuçları, birkaç dakika sonra yeni bir sekmede görünür.

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **SONUÇLAR** paneli: Sonucun tamamını CSV, JSON veya Excel dosyası olarak yerel yola kaydedebilir veya yalnızca birkaç satır seçebilirsiniz.

    - **İLETİLER** paneli: **Satır** numarasını seçtiğinizde, çalıştırılmakta olan betiğin birinci satırına atlar.

## <a name="submit-interactive-pyspark-queries"></a>PySpark etkileşimli sorguları gönderme

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>HDInsight Spark kümeleri etkileşimli PySpark sorguları göndermek için.

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
4. Bu betik vurgulayın. Ardından Kod Düzenleyicisi'ni sağ tıklatın ve seçin **HDInsight: PySpark etkileşimli**, veya kısayolu **Ctrl + Alt + ı**.

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

### <a name="to-disable-environment-check"></a>Ortam denetimi devre dışı bırakmak için

Varsayılan olarak, HDInsight araçları ortamı denetler ve bağımlı paketleri yüklemek, PySpark etkileşimli sorgular gönderin. Ortam denetimi devre dışı bırakmak için ayarlanmış **hdinsight.disablePysparkEnvironmentValidation** için **Evet** altında **kullanıcı ayarları**.

   ![Ortam Denetimi ayarları ayarlayın](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check.png)

Alternatif olarak, **devre dışı doğrulama** düğmesini iletişim kutusu açılır.

   ![İletişim kutusundan ortam denetimi ayarlama](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check-dialog.png)

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 Spark2.2/2.3 ile desteklenmiyor

PySpark3 Spark 2.2 kümesi ve Spark2.3 kümesi ile artık desteklenmiyor; yalnızca "PySpark" Python için desteklenmiyor. Sorun spark'a 2.2/2.3 gönderir başarısız ile Python3 bilinir.

   ![Göndermek için python3 alma hatası](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Python2.x kullanmak için adımları izleyin: 

1. Python 2.7 için yerel bir bilgisayara yüklemeniz ve sistem yoluna ekleyin.

2. VSCode yeniden başlatın.

3. Python 2'ye tıklayarak geçiş **Python XXX** durum çubuğu sonra hedef Python'ı seçin.

   ![Python sürümü seçin](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

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
4. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: PySpark Batch**, veya kısayolu **Ctrl + Alt + H**. 

5. Hangi, PySpark işi göndermek bir kümeyi seçin. 

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Bir Python işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS code'da penceresi. **Spark UI URL** ve **Yarn UI URL** de gösterilir. URL, iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.

## <a name="apache-livy-configuration"></a>Apache Livy yapılandırma

[Apache Livy](https://livy.incubator.apache.org/) yapılandırma desteklenir, sırasında ayarlanabilir **. VSCode\settings.json** çalışma alanı klasörünün içinde. Şu anda livy yapılandırma yalnızca Python betiğini destekler. Daha fazla bilgi [Livy Benioku](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Livy yapılandırma tetikleme**
   
Bulabileceğiniz **dosya** menüsünde **tercihleri**ve **ayarları** bağlam menüsünde. Tıklayın **çalışma alanı ayarlarını** sekmesinde başlatabilir livy yapılandırmasını ayarlayın.

Bir dosya da gönderebilir, bildirim .vscode klasörü çalışma klasörü otomatik olarak eklenir. Livy yapılandırma tıklayarak bulabilirsiniz **.vscode\settings.json**.

+ Proje ayarları:

    ![Livy yapılandırma](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Ayarları için **driverMomory** ve **executorMomry**, birim, örneğin 1 g veya 1024 m ile ayarlayın. 

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
    | appId | Bu oturumun uygulama kimliği |  Dize |
    | appInfo | Ayrıntılı uygulama bilgileri | Harita anahtarı val = |
    | Günlük | Günlük satırları | dize listesi |
    | durum |   Toplu işlem durumu | dize |

>[!NOTE]
>Atanan livy config çıkış Bölmesi'nde görüntüler zaman betiği gönderin.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Azure HDInsight Gezgini'nden tümleştirin

Azure HDInsight, sol bölmenin eklendi. Göz atabilir ve kümeye doğrudan yönetin.

1. Genişletin **AZURE HDINSİGHT**değil oturum, içinde gösterecektir **Azure'da oturum aç...**  bağlantı.

    ![Bağlantı resmi oturum açın](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Tıklayın **Azure'da oturum aç**, sağ alt kısmında oturum açma bağlantısı ve kod açılır.

    ![Diğer ortam için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Tıklayın **açın & Kopyala** düğmesi, tarayıcı, açılır kodu yapıştırın, tıklayın **devam** düğmesi, başarılı oturum açma hakkında ipucu görür.

4. Oturum açtıktan sonra kullanılabilen abonelikler ve (Spark, Hadoop ve HBase desteklenir) kümeleri listelenir **AZURE HDINSİGHT**. 

   ![Azure HDInsight abonelik](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Hive meta veri veritabanı ve tablo şemasını görüntülemek için kümeyi genişletin.

   ![Azure HDInsight kümesi](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Ek özellikler

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

* [Apache Spark uygulamalar VPN üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri Görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Visual Studio Code için PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md)
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Apache Spark: BI araçlarıyla HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Yemek İnceleme sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](spark/apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](spark/apache-spark-job-debugging.md)



