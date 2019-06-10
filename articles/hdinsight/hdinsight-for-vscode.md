---
title: Azure HDInsight araçları - Visual Studio kodu Hive, LLAP veya PySpark kullanın
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: d114a1e62ae0d28e7d4a3ad453d5d7bd3e1d5b7a
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427696"
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

3. ' % S '.hql (Hive sorguları) veya .py (Spark betiğini) dosya uzantısı ile yeni dosya adı.  Bu örnekte **HelloWorld.hql**.

## <a name="connect-to-hdinsight-cluster"></a>HDInsight kümesine bağlanma

Visual Studio Code'dan betikleri HDInsight kümelerine gönderebilirsiniz önce Azure hesabınıza bağlanın veya bir kümesini bağlamak gerekir (Ambari kullanarak kullanıcı adı/parola veya etki alanı hesabı alanına).  Azure'a bağlanmak için aşağıdaki adımları tamamlayın:

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Oturum açma**.

    ![Visual Studio Code oturum açma için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Oturum açma yönergeleri izleyin **çıkış** bölmesi.
    + Azure, genel ortam için **HDInsight: Oturum açma** komut tetikleme **Azure'da oturum aç** eylem HDInsight Gezgini ve bunun tersi de geçerlidir.

        ![Azure için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Diğer ortamlarda, oturum açma yönergeleri izleyin.

        ![Diğer ortam için oturum açma yönergeleri](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

   Bağlandıktan sonra Azure hesabınızın adını Visual Studio kod penceresinin sol alt durum çubuğunda gösterilir.  
  

<h2 id="linkcluster">Bağlantı oluşturun: Azure HDInsight</h2>

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


## <a name="create-link-generic-livy-endpoint"></a>Bağlantı oluşturun: Livy genel uç noktası

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

3. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

4. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **HDInsight: Ayarlanmış varsayılan küme**.  

5. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçları yapılandırma dosyasını otomatik olarak güncelleştirmek **. VSCode\settings.json**. 

   ![Kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Azure ortamı ayarlama

1. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

2. Menü çubuğundan gidin **görünümü** > **komut paleti...** girin **HDInsight: Azure ortamı ayarlama**.

3. Varsayılan oturum açma Giriş bir ortam seçin.

4. Bu arada, araç, varsayılan oturum açma girişi zaten kaydetti **. VSCode\settings.json**. Ayrıca doğrudan bu yapılandırma dosyasında güncelleştirebilirsiniz. 

   ![Kümesi varsayılan oturum açma girişi yapılandırma](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Etkileşimli Hive sorguları göndermek, Hive toplu betiklerden

Visual Studio Code için HDInsight araçları ile etkileşimli Hive sorguları göndermek ve batch betikleri HDInsight kümeleri için Hive.

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Dosyayı seçin **HelloWorld.hql** oluşturulan [önceki](#open-hdinsight-work-folder) Kod Düzenleyicisi'nde açılır.

3. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

4. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

5. Kod Düzenleyicisi'ni sağ tıklayın, **HDInsight: Etkileşimli hive** sorgu gönderin veya kısayol **Ctrl + Alt + ı**.  Seçin **HDInsight: Hive toplu** betiği gönderin veya kısayol **Ctrl + Alt + H**.  

6. Varsayılan Küme belirtmediyseniz kümeyi seçin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Sorgu sonuçları, birkaç dakika sonra yeni bir sekmede görünür.

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçları** paneli: Tüm sonucu yerel yol CSV, JSON veya Excel dosyası olarak kaydetmek veya çok satırlı seçmeniz yeterlidir.

    - **İLETİLERİ** paneli: Seçtiğinizde, **satırı** numarası çalışan kodun ilk satırını atlar.

## <a name="submit-interactive-pyspark-queries"></a>PySpark etkileşimli sorguları gönderme

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Yeni bir dosya oluşturun **HelloWorld.py** aşağıdaki [önceki](#open-hdinsight-work-folder) adımları.

3. Python için önkoşul yüklemediyseniz, bir Python uzantısı öneri iletişim alırsınız.  Yükleyin ve yüklemeyi tamamlamak için Visual Studio Code yeniden yükleyin.

    >![HDInsight için Visual Studio kod Python yükleme](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

4. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

5. Kopyalayıp betik dosyasına aşağıdaki kodu yapıştırın:
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

6. Kod Düzenleyicisi'ni sağ tıklayın, **HDInsight: PySpark etkileşimli** sorgu gönderin veya kısayol **Ctrl + Alt + ı**.  

7. Varsayılan Küme belirtmediyseniz kümeyi seçin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Sorgu sonuçları, birkaç dakika sonra yeni bir sekmede görünür.

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 

8. Aracı'nı da destekler **SQL yan tümcesi** sorgu.

   ![Python iş sonucu gönderme](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) alt durum sorguları zaman çalıştırdığınız çubuğunda sol taraftaki gönderme durumu görüntülenir. Diğer sorgular durum olduğunda paylaşmayın **PySpark Çekirdeği (meşgul)** .  

>[!NOTE]  
>Küme oturum bilgilerini koruyabilir. Aynı kümenin birden çok hizmet çağrısını üzerinde başvurulabilen şekilde tanımlanmış değişken, işlev ve karşılık gelen değerler oturumda tutulur. 

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 Spark2.2/2.3 ile desteklenmiyor

PySpark3 Spark 2.2 kümesi ve Spark2.3 kümesi ile artık desteklenmiyor; yalnızca "PySpark" Python için desteklenmiyor. Sorun spark'a 2.2/2.3 gönderir başarısız ile Python3 bilinir.

   ![Göndermek için python3 alma hatası](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Python2.x kullanmak için adımları izleyin: 

1. Python 2.7 için yerel bir bilgisayara yüklemeniz ve sistem yoluna ekleyin.

2. Visual Studio Code'u yeniden başlatın.

3. Python 2'ye tıklayarak geçiş **Python XXX** durum çubuğu sonra hedef Python'ı seçin.

   ![Python sürümü seçin](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)


## <a name="submit-pyspark-batch-job"></a>PySpark batch işi gönderme

1. Klasör yeniden **HDexample** oluşturulan [önceki](#open-hdinsight-work-folder) kapalı.  

2. Yeni bir dosya oluşturun **BatchFile.py** aşağıdaki [önceki](#open-hdinsight-work-folder) adımları.

3. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

4. Kopyalayıp betik dosyasına aşağıdaki kodu yapıştırın:

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
    | proxyUser | İş çalışırken bürünülecek kullanıcı | string | 
    | className | Uygulamanın Java/Spark temel sınıfı | string |
    | args | Uygulama için komut satırı bağımsız değişkenleri | dize listesi | 
    | jars | Bu oturumda kullanılmak üzere jar'lar | Dize listesi | 
    | pyFiles | Bu oturumda kullanılmak üzere Python dosyaları | Dize listesi |
    | files | Bu oturumda kullanılmak üzere dosyaları | Dize listesi |
    | driverMemory | Sürücü işlemi için kullanılacak bellek miktarını | string |
    | driverCores | Sürücü işlemi için kullanılacak çekirdek sayısı | int |
    | executorMemory | Yürütücü işlemi bellek miktarı | string |
    | executorCores | Her Yürütücü için kullanılacak çekirdek sayısı | int |
    | numExecutors | Bu oturum için başlatmak için Yürütücü sayısı | int |
    | archives | Bu oturumda kullanılmak üzere arşivleri | Dize listesi |
    | queue | YARN Kuyruğun adı gönderildi | string |
    | name | Bu oturumun adı | string |
    | conf | Spark yapılandırma özellikleri | Harita anahtarı val = |

    Yanıt gövdesi   
    Oluşturulan toplu iş nesnesi.

    | name | description | türü | 
    | :- | :- | :- | 
    | id | Oturum kimliği | int | 
    | appId | Bu oturumun uygulama kimliği |  String |
    | appInfo | Ayrıntılı uygulama bilgileri | Harita anahtarı val = |
    | log | Günlük satırları | dize listesi |
    | state |   Toplu işlem durumu | string |

>[!NOTE]
>Atanan livy config çıkış Bölmesi'nde görüntüler zaman betiği gönderin.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Azure HDInsight Gezgini'nden tümleştirin

**Azure HDInsight** Gezgini görünümü eklenmiştir. Göz atabilir ve kümelere doğrudan ile yönettiğiniz **Azure HDInsight**.

1. [Connect](#connect-to-hdinsight-cluster) henüz yapmadıysanız, Azure hesabınıza.

2. Menü çubuğundan gidin **görünümü** > **Gezgini**.

3. Sol bölmeden genişletin **AZURE HDINSİGHT**.  Kullanılabilir abonelikler ve (Spark, Hadoop ve HBase desteklenir) kümeleri listelenir. 

   ![Azure HDInsight abonelik](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Hive meta veri veritabanı ve tablo şemasını görüntülemek için kümeyi genişletin.

   ![Azure HDInsight kümesi](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="additional-features"></a>Ek Özellikler

Visual Studio Code için HDInsight aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Anahtar sözcüğü, yöntemleri, değişkenler vb. için öneriler açılır. Farklı simgeler farklı türde nesneyi temsil eder.

    ![Visual Studio kod IntelliSense nesne türleri için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaret**. Dil hizmeti ve Hive betiğinin düzenleme hatalarla altını çizip çizmeyeceğini.     
- **Söz dizimi vurgular**. Dil hizmeti, değişkenleri, anahtar sözcükler, veri türü, İşlevler ve benzeri ayırt etmek için farklı renkler kullanır. 

    ![Söz dizimi önemli Visual Studio Code için HDInsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)


## <a name="unlink-cluster"></a>Küme bağlantısını Kaldır

1. Menü çubuğundan gidin **görünümü** > **komut paleti...** yazıp enter **HDInsight: Bir küme bağlantısını**.  

2. Bağlantısını kesmek için kümeyi seçin.  

3. Gözden geçirme **çıkış** doğrulama için görünümü.  


## <a name="logout"></a>Oturumu Kapat  

Menü çubuğundan gidin **görünümü** > **komut paleti...** yazıp enter **HDInsight: Oturum kapatma**.  Alt sağ köşesinde bildiren bir açılır pencere olacak **oturum kapatma başarıyla!** .


## <a name="next-steps"></a>Sonraki adımlar
Bir tanıtım videosu Visual Studio Code için HDInsight'ı kullanmanın [Visual Studio Code için HDInsight](https://go.microsoft.com/fwlink/?linkid=858706)
