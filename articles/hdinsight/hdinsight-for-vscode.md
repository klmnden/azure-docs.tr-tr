---
title: Azure HDInsight araçları - Visual Studio kodu Hive, LLAP veya PySpark kullanın
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: ebc41fc74d24708a177bf554029df8384c49df05
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657247"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code için Azure HDInsight araçları kullanma

Oluşturmak ve için Apache Spark, Apache Hive toplu işlerini, etkileşimli Hive sorgularını ve PySpark betiklerini göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin. İlk olarak Visual Studio Code'da HDInsight Araçları'nı yükleme açıklayacağız ve ardından Hive ve Spark işleri göndermek nasıl alacağız.  

Azure HDInsight araçları, Windows, Linux ve Macos'ta içeren Visual Studio Code tarafından desteklenmeyen platformlarda yüklenebilir. Aşağıda farklı platformlar için önkoşulları bulabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakiler gereklidir:

- Bir HDInsight kümesi. Bir küme oluşturmak için bkz: [HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono, yalnızca Linux ve macOS için gereklidir.
- [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) Visual Studio Code için.
- [Visual Studio Code için PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md).
- Adlı bir yerel dizin **HDexample**.  Bu makalede **C:\HD\HDexample**.

## <a name="install-azure-hdinsight-tools"></a>Azure HDInsight Araçları'nı yükleme

Önkoşulları tamamladıktan sonra Visual Studio Code için Azure HDInsight araçları yükleyebilirsiniz.  Azure HDInsight Araçları'nı yüklemek için aşağıdaki adımları tamamlayın:

1. Visual Studio Code'u açın.

2. Menü çubuğundan gidin **görünümü** > **uzantıları**.

3. Arama kutusuna **HDInsight**.

4. Seçin **Azure HDInsight Araçları** seçin ve arama sonuçlarını **yükleme**.  

   ![HDInsight için Visual Studio kod Python yükleme](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Seçin **yeniden** etkinleştirmek için **Azure HDInsight Araçları** yüklendikten sonra uzantı.


## <a name="open-hdinsight-work-folder"></a>HDInsight iş klasörü aç

Bir çalışma klasörü açmak için aşağıdaki adımları tamamlayın ve Visual Studio Code'da bir dosya oluşturun:

1. Menü çubuğundan gidin **dosya** > **Klasör Aç...**   >  **C:\HD\HDexample**, ardından **Klasör Seç** düğmesi. Klasör görünür **Gezgini** soldaki görünümü.

2. Gelen **Gezgini** görüntülemek için klasörü seçin **HDexample**, ardından **yeni dosya** iş klasörü yanındaki simge.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)

3. Yeni bir dosya ile ya da ad `.hql` (Hive sorguları) veya `.py` (Spark betiğini) dosya uzantısı.  Bu örnekte **HelloWorld.hql**.

## <a name="set-the-azure-environment"></a>Azure ortamı ayarlama

1. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

2. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Azure ortamı ayarlama**.

3. Varsayılan oturum açma Giriş bir ortam seçin.

4. Bu arada, araç, varsayılan oturum açma girişi zaten kaydetti **. VSCode\settings.json**. Ayrıca doğrudan bu yapılandırma dosyasında güncelleştirebilirsiniz. 

   ![Kümesi varsayılan oturum açma girişi yapılandırma](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Azure hesabına bağlanma

Visual Studio Code'dan betikleri HDInsight kümelerine gönderebilirsiniz önce Azure hesabınıza bağlanın veya bir kümesini bağlamak gerekir (Ambari kullanarak kullanıcı adı/parola veya etki alanı hesabı alanına).  Azure'a bağlanmak için aşağıdaki adımları tamamlayın:

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Oturum açma**.

    ![Visual Studio Code oturum açma için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Oturum açma yönergeleri izleyin **çıkış** bölmesi.
    + Azure, genel ortam için **HDInsight: Oturum açma** komut tetikleme **Azure'da oturum aç** eylem HDInsight Gezgini ve bunun tersi de geçerlidir.

        ![Azure için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Diğer ortamlarda, oturum açma yönergeleri izleyin.

        ![Diğer ortam için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

   Bağlandıktan sonra Azure hesabınızın adını Visual Studio kod penceresinin sol alt durum çubuğunda gösterilir.  
  

## <a name="link-a-cluster"></a>Küme bağlantı

### <a name="link-azure-hdinsight"></a>Bağlantı: Azure HDInsight

Normal bir küme kullanarak bağlayabilirsiniz bir [Apache Ambari](https://ambari.apache.org/) yönetilen kullanıcı adı veya bir etki alanı kullanıcı adı kullanarak bir kurumsal güvenlik paketi güvenli Hadoop kümesi bağlantısını (gibi: user1@contoso.com).

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Küme bağlantı**.

   ![bağlantı kümesi komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Bağlı küme türü seçin **Azure HDInsight**.

3. HDInsight kümesi URL'si girin.

4. Ambari kullanıcı adını girin, varsayılan **yönetici**.

5. Ambari parolasını girin.

6. Küme türü seçin.

7. Gözden geçirme **çıkış** doğrulama için görünümü.

   > [!NOTE]  
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanılır.  



### <a name="link-generic-livy-endpoint"></a>Bağlantı: Livy genel uç noktası

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Küme bağlantı**.

2. Bağlı küme türü seçin **genel Livy uç nokta**.

3. Genel Livy uç noktası girin. Örneğin: http\:/ / 10.172.41.42:18080.

4. Yetkilendirme türünü seçin **temel** veya **hiçbiri**.  Varsa **temel**, ardından:  
    &emsp;bir. Ambari kullanıcı adını girin, varsayılan **yönetici**.  
    &emsp;b. Ambari parolasını girin.

5. Gözden geçirme **çıkış** doğrulama için görünümü.

## <a name="list-hdinsight-clusters"></a>HDInsight kümeleri listeleme

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Liste küme**.

2. İstediğiniz aboneliği seçin.

3. Gözden geçirme **çıkış** görünümü.  Görünüm, bağlantılı kümesi ve Azure aboneliğiniz kapsamındaki tüm kümelere gösterir.

    ![Bir varsayılan küme yapılandırmasını ayarlayın](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Varsayılan kümesi

1. Klasörü yeniden açın **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Dosyayı seçin **HelloWorld.hql** oluşturulan [önceki](#open-hdinsight-work-folder) Kod Düzenleyicisi'nde açılır.

3. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **HDInsight: Ayarlanmış varsayılan küme**.  

4. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

5. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçları yapılandırma dosyasını otomatik olarak güncelleştirmek **. VSCode\settings.json**. 

   ![Kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Etkileşimli Hive sorguları göndermek, Hive toplu betiklerden

Visual Studio Code için HDInsight araçları ile etkileşimli Hive sorguları göndermek ve batch betikleri HDInsight kümeleri için Hive.

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Dosyayı seçin **HelloWorld.hql** oluşturulan [önceki](#open-hdinsight-work-folder) Kod Düzenleyicisi'nde açılır.


3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

5. Kod Düzenleyicisi'ni sağ tıklayın, **HDInsight: Etkileşimli hive** sorgu gönderin veya kısayol **Ctrl + Alt + ı**.  Seçin **HDInsight: Hive toplu** betiği gönderin veya kısayol **Ctrl + Alt + H**.  

6. Varsayılan Küme belirtmediyseniz kümeyi seçin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Sorgu sonuçları, birkaç dakika sonra yeni bir sekmede görünür.

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçları** paneli: Tüm sonucu yerel yol CSV, JSON veya Excel dosyası olarak kaydetmek veya çok satırlı seçmeniz yeterlidir.

    - **İLETİLERİ** paneli: Seçtiğinizde, **satırı** numarası çalışan kodun ilk satırını atlar.

## <a name="submit-interactive-pyspark-queries"></a>PySpark etkileşimli sorguları gönderme

PySpark etkileşimli sorgular, aşağıdaki adımları izleyerek gönderebilirsiniz:

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Yeni bir dosya oluşturun **HelloWorld.py** aşağıdaki [önceki](#open-hdinsight-work-folder) adımları.

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

4. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

5. Tüm kod seçin ve Kod Düzenleyicisi'ni sağ tıklayın, **HDInsight: PySpark etkileşimli** sorgu gönderin veya kısayol **Ctrl + Alt + ı**.

   ![pyspark etkileşimli sağ tıklayın.](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Varsayılan Küme belirtmediyseniz kümeyi seçin. Birkaç dakika sonra **Python etkileşimli sonuçları** yeni bir sekmede görünür. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. 

   ![pyspark etkileşimli python etkileşimli penceresi](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. ENTER **"%% bilgisi"** ve tuşuna **Shift + Enter** iş bilgilerini görmek için. (İsteğe bağlı)

   ![iş bilgilerini görüntüle](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Aracı'nı da destekler **Spark SQL** sorgu.

   ![Pyspark etkileşimli sonucunu görüntüle](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Sorgu çalıştırırken gönderme durumu alt durum çubuğunda sol tarafında görünür. Diğer sorgular durum olduğunda paylaşmayın **PySpark Çekirdeği (meşgul)** .  

   > [!NOTE] 
   >
   > Zaman **Python uzantısı etkin** (varsayılan ayar işaretli) ayarlarında işaretlenmediği eski pencerenin gönderilen pyspark etkileşim sonuçları kullanır.
   >
   > ![pyspark etkileşimli python uzantısını devre dışı](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>PySpark batch işi gönderme

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Yeni bir dosya oluşturun **BatchFile.py** aşağıdaki [önceki](#open-hdinsight-work-folder) adımları.

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

4. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

5. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **HDInsight: PySpark Batch**, veya kısayolu **Ctrl + Alt + H**. 

6. Hangi, PySpark işi göndermek bir kümeyi seçin. 

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Bir Python işi gönderdikten sonra gönderme günlükleri görünür **çıkış** Visual Studio Code penceresi. **Spark UI URL** ve **Yarn UI URL** de gösterilir. URL, iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.

## <a name="apache-livy-configuration"></a>Apache Livy yapılandırma

[Apache Livy](https://livy.incubator.apache.org/) yapılandırma desteklenir, sırasında ayarlanabilir **. VSCode\settings.json** çalışma alanı klasörüne. Şu anda livy yapılandırma yalnızca Python betiğini destekler. Daha fazla bilgi [Livy Benioku](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Livy yapılandırma tetikleme**

Yöntem 1  
1. Menü çubuğundan gidin **dosya** > **tercihleri** > **ayarları**.  
2. İçinde **arama ayarları** metin kutusuna girin **HDInsight iş Sumission: Livy Conf**.  
3. Seçin **settings.json içinde düzenleme** ilgili arama sonucu.

Yöntem 2   
Bir dosya göndermek, .vscode klasörü otomatik olarak iş klasöre eklenen dikkat edin. Livy yapılandırma tıklayarak bulabilirsiniz **.vscode\settings.json**.

+ Proje ayarları:

    ![Livy yapılandırma](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Ayarları için **driverMomory** ve **executorMomry**, birim, örneğin 1 g veya 1024 m ile ayarlayın. 

+ Desteklenen Livy yapılandırmalar:   

    **POST /batches**   
    İstek Gövdesi

    | name | description | türü | 
    | :- | :- | :- | 
    | file | Yürütülecek uygulamanın içeren dosya | yol (gerekli) | 
    | proxyUser | İş çalışırken bürünülecek kullanıcı | dize | 
    | className | Uygulamanın Java/Spark temel sınıfı | dize |
    | args | Uygulama için komut satırı bağımsız değişkenleri | dize listesi | 
    | jars | Bu oturumda kullanılmak üzere jar'lar | Dize listesi | 
    | pyFiles | Bu oturumda kullanılmak üzere Python dosyaları | Dize listesi |
    | files | Bu oturumda kullanılmak üzere dosyaları | Dize listesi |
    | driverMemory | Sürücü işlemi için kullanılacak bellek miktarını | dize |
    | driverCores | Sürücü işlemi için kullanılacak çekirdek sayısı | int |
    | executorMemory | Yürütücü işlemi bellek miktarı | dize |
    | executorCores | Her Yürütücü için kullanılacak çekirdek sayısı | int |
    | numExecutors | Bu oturum için başlatmak için Yürütücü sayısı | int |
    | archives | Bu oturumda kullanılmak üzere arşivleri | Dize listesi |
    | queue | YARN Kuyruğun adı gönderildi | dize |
    | name | Bu oturumun adı | dize |
    | conf | Spark yapılandırma özellikleri | Harita anahtarı val = |

    Yanıt gövdesi   
    Oluşturulan toplu iş nesnesi.

    | name | description | türü | 
    | :- | :- | :- | 
    | id | Oturum kimliği | int | 
    | appId | Bu oturumun uygulama kimliği |  Dize |
    | appInfo | Ayrıntılı uygulama bilgileri | Harita anahtarı val = |
    | log | Günlük satırları | dize listesi |
    | state |   Toplu işlem durumu | dize |

>[!NOTE]
>Atanan livy config çıkış Bölmesi'nde görüntüler zaman betiği gönderin.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Azure HDInsight Gezgini'nden tümleştirin

**Azure HDInsight** Gezgini görünümü eklenmiştir. Göz atabilir ve kümelere doğrudan ile yönettiğiniz **Azure HDInsight**.

1. [Connect](#connect-to-azure-account) için Azure hesap veya henüz yapmadıysanız bir kümesini bağlarsınız.

2. Menü çubuğundan gidin **görünümü** > **Gezgini**.

3. Sol bölmeden genişletin **AZURE HDINSİGHT**.  Kullanılabilir abonelikler ve (Spark, Hadoop ve HBase desteklenir) kümeleri listelenir. 

   ![Azure HDInsight abonelik](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Hive meta veri veritabanı ve tablo şemasını görüntülemek için kümeyi genişletin.

   ![Azure HDInsight kümesi](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Önizleme Hive tablosu
Hive tablosu, kümelere doğrudan ile Önizleme **Azure HDInsight** Gezgini.
1. [Connect](#connect-to-azure-account) henüz yapmadıysanız, Azure hesabınıza.

2. Tıklayın **Azure** en soldaki sütuna simge.

3. Sol bölmede, AZURE HDINSİGHT'ı genişletin. Kullanılabilir abonelikler ve kümeler listelenir.

4. Hive meta veri veritabanı ve tablo şemasını görüntülemek için kümeyi genişletin.

5. Hive tablosunda, ör. hivesampletable sağ tıklayın. Seçin **Önizleme**. 

   ![HDInsight vscode Önizleme hive tablosu](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. **Önizleme sonuçlarını** penceresi açılacak.

   ![HDInsight vscode Önizleme sonuçları penceresi](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- **Sonuçları** paneli

   Tüm sonucu yerel yol CSV, JSON veya Excel dosyası olarak kaydetmek veya çok satırlı seçmeniz yeterlidir.

- **İLETİLERİ** paneli
   1. Tablodaki satır sayısını 100 satırı büyük olduğunda, iletiyi gösterir: **Hive tablosu için ilk 100 satırı görüntülenen**.
   2. Tablodaki satır sayısını ya da 100 satırları eşit olduğunda, iletiyi gösterir: **satırları 60 Hive tablosu için görüntülenen**.
   3. Tablodaki içerik olmadığında iletiyi gösterir: **0 satır Hive tablosu için görüntülenen**.

>[!NOTE]
>
>Linux'ta, tablo veri kopyalama etkinleştirmek için xclip yükleyin.
>
>![HDInsight Linux'ta vscode için](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>Ek Özellikler

Visual Studio Code için HDInsight aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Anahtar sözcüğü, yöntemleri, değişkenler vb. için öneriler açılır. Farklı simgeler farklı türde nesneyi temsil eder.

    ![Visual Studio kod IntelliSense nesne türleri için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaret**. Dil hizmeti ve Hive betiğinin düzenleme hatalarla altını çizip çizmeyeceğini.     
- **Söz dizimi vurgular**. Dil hizmeti, değişkenleri, anahtar sözcükler, veri türü, İşlevler ve benzeri ayırt etmek için farklı renkler kullanır. 

    ![Söz dizimi önemli Visual Studio Code için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Küme kullanıcılarla **okuyucu** **yalnızca** **rol** artık işi HDInsight kümesine göndermek veya Hive veritabanı görüntüleyin. Rolünüz için yükseltmek için küme yöneticinize başvurun gerek [ **HDInsight** **küme** **işleci** ](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) içinde [ Azure portalında](https://ms.portal.azure.com/). Ambari kimlik bilgilerini biliyorsanız, aşağıdaki yönergeleri izleyerek Küme elle bağlantı kurabilir.

### <a name="browse-hdinsight-cluster"></a>HDInsight küme Gözat  

Bir HDInsight kümesi genişletmek için Azure HDInsight explorer'ı tıklatarak, küme için yalnızca okuyucu rolüne varsa kümesini bağlamak için istenir. Ambari kimlik bilgileri aracılığıyla kümeye bağlanmak için aşağıdaki adımları izleyin. 

### <a name="submit-job-to-hdinsight-cluster"></a>HDInsight kümesi için iş gönderme

Bir HDInsight kümesine işi gönderiliyor, küme için yalnızca okuyucu rolüne varsa kümesini bağlamak için istenir. Ambari kimlik bilgileri aracılığıyla kümeye bağlanmak için aşağıdaki adımları izleyin. 

### <a name="link-to-cluster"></a>Kümeye bağlantı

1.  Ambari kullanıcı adını girin 
2.  Ambari kullanıcı parolasını girin.

   ![Visual Studio kodu kullanıcı adı için HDInsight araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Visual Studio kod parola için HDInsight araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>HDInsight kullanabilirsiniz: Bağlı küme denetlemek için Küme listesi.
>
>![Bağlantılı kod okuyucusunun Visual Studio için HDInsight araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-reader-linked.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake depolama Gen2'ye (ADLS Gen2)

### <a name="browse-an-adls-gen2-account"></a>ADLS hesabı Gen2'ye göz atın

Bir Gen2 ADLS hesabını genişletmek için Azure HDInsight explorer'ı tıklatarak, depolama girmeniz istenecektir **erişim anahtarı** Azure hesabınızda Gen2 depolama erişimi varsa. ADLS Gen2 hesabı erişim anahtarı başarıyla doğrulandıktan sonra Genişletilmiş otomatik olarak oluşturulacak. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>ADLS Gen2 ile HDInsight kümesi için işlerini gönderme

Bir HDInsight kümesine ADLS Gen2 ile iş gönderirken depolama girmeniz istenecektir **erişim anahtarı** Azure hesabınızda Gen2 depolamaya yazma erişimi varsa.  İş, başarıyla erişim anahtarı başarıyla doğrulandıktan sonra gönderilir. 

![Visual Studio kod AccessKey için HDInsight araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Depolama hesabı için erişim anahtarı Azure portalından alabilirsiniz. Bilgi için [erişim anahtarlarını görüntüleme ve kopyalama](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys).

## <a name="unlink-cluster"></a>Küme bağlantısını Kaldır

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** yazıp enter **HDInsight: Bir küme bağlantısını**.  

2. Bağlantısını kesmek için kümeyi seçin.  

3. Gözden geçirme **çıkış** doğrulama için görünümü.  

## <a name="sign-out"></a>Oturumu kapat  

Menü çubuğundan gidin **görünümü** > **komut paleti...** yazıp enter **HDInsight: Oturum kapatma**.  Alt sağ köşesinde bildiren bir açılır pencere olacak **oturum kapatma başarıyla!** .


## <a name="next-steps"></a>Sonraki adımlar
Bir tanıtım videosu Visual Studio Code için HDInsight'ı kullanmanın [Visual Studio Code için HDInsight](https://go.microsoft.com/fwlink/?linkid=858706)
