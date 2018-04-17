---
title: Azure Hdınsight araçları - Hive, LLAP veya pySpark için Visual Studio Code kullanma | Microsoft Docs
description: Visual Studio Code Azure Hdınsight araçları oluşturmak ve sorgular ve komut dosyaları göndermek için nasıl kullanılacağını öğrenin.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
manager: ''
editor: jgao
tags: azure-portal
ms.assetid: ''
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 0074486d3d7fb58bc6e3adcbe4245ec53e7e4cde
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Azure Hdınsight araçları Visual Studio kodunu kullanın

Azure Hdınsight araçları Visual Studio kodunu (VS Code) oluşturun ve Hive toplu işleri, etkileşimli Hive sorguları ve pySpark betikleri göndermek için nasıl kullanılacağını öğrenin. Azure Hdınsight araçları, VS kodu tarafından desteklenen platformlarda yüklenebilir. Bunlar, Windows, Linux ve macOS içerir. Farklı platformlar için Önkoşullar bulabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler, bu makaledeki adımları tamamlamak için gereklidir:

- Bir Hdınsight kümesi.  Bir küme oluşturmak için bkz: [Hdınsight kullanmaya başlama]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono, yalnızca Linux ve macOS için gereklidir.

## <a name="install-the-hdinsight-tools"></a>Hdınsight Araçları'nı yükleme
   
Önkoşulları yükledikten sonra Azure Hdınsight araçları için VS Code yükleyebilirsiniz. 

**Yükleme Azure Hdınsight araçları**

1. Visual Studio Code'u açın.

2. Sol bölmede seçin **uzantıları**. Arama kutusuna **Hdınsight**.

3. Yanına **Azure Hdınsight Araçları**seçin **yükleme**. Birkaç saniye sonra **yükleme** düğmesi değişiklikler **yeniden**.

4. Seçin **yeniden** etkinleştirmek için **Azure Hdınsight Araçları** uzantısı.

5. Seçin **yeniden yükleme penceresi** onaylamak için. Gördüğünüz **Azure Hdınsight Araçları** içinde **uzantıları** bölmesi.

   ![Hdınsight Visual Studio kod Python yükleme için](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Hdınsight çalışma alanını açın

Azure'a bağlanabilmesi için önce bir çalışma alanı VS Code'da oluşturun.

**Bir çalışma alanını açmak için**

1. Üzerinde **dosya** menüsünde, select **Klasör Aç**. Daha sonra iş klasörünüz olarak varolan bir klasörü atayın veya yeni bir tane oluşturun. Klasör sol bölmesinde görüntülenir.

2. Sol bölmede seçin **yeni dosya** iş klasörünün yanındaki simge.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)

3. (Hive sorguları) .hql veya .py (Spark betik) dosya uzantısı ile yeni dosya adı. Dikkat bir **XXXX_hdi_settings.json** yapılandırma dosyası için çalışma klasörü otomatik olarak eklenir.

4. Açık **XXXX_hdi_settings.json** gelen **EXPLORER**, veya komut dosyası Düzenleyicisi'ni seçmek için sağ **yapılandırma kümesi**. Oturum açma girişi, varsayılan küme ve dosyayı örnekte gösterildiği gibi iş gönderme parametrelerini yapılandırabilirsiniz. Ayrıca kalan parametreler boş bırakabilirsiniz.

## <a name="connect-to-hdinsight-cluster"></a>Hdınsight kümesine bağlanma

Komut dosyaları VS koddan Hdınsight kümelerine gönderebilir önce Azure hesabınıza bağlanın ya da bir küme bağlamak gerekir (Ambari kullanarak kullanıcı adı/parola veya etki alanı hesabı alanına).

**Azure'a bağlanmak için**

1. Bunları zaten yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası oluşturun.

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından bağlam menüsünden seçin **Hdınsight: oturum açma**. Ayrıca girebilirsiniz **Ctrl + Shift + P**ve ardından girin **Hdınsight: oturum açma**.

    ![Visual Studio Code için Hdınsight araçları oturum açın](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Oturum açmak için oturum açma yönergeleri izleyin **çıkış** bölmesi.

    **Azure:** ![Visual Studio Code oturum açma bilgileri için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Bağlandıktan sonra Azure hesap adınızı VS Code penceresinin sol alt durum çubuğunda gösterilir. 

    > [!NOTE]
    > Bilinen Azure kimlik doğrulaması sorun nedeniyle, bir tarayıcı özel modda veya incognito modda açmanız gerekir. Azure hesabınızda etkin iki faktör varsa, PIN kimlik doğrulaması yerine telefon kimlik doğrulaması kullanarak önerilir.
  

4. Kod Düzenleyicisi'ni bağlam menüsünü açmak için sağ tıklatın. Bağlam menüsünden aşağıdaki görevleri gerçekleştirebilirsiniz:

    - Oturumu kapat
    - Liste kümeleri
    - Set varsayılan kümeleri
    - Etkileşimli Hive sorguları gönderme
    - Hive toplu komut gönderme
    - Etkileşimli PySpark sorguları gönderme
    - PySpark toplu iş komut gönderme
    - Küme yapılandırmaları

**Bir küme bağlamak için**

Yönetilen Ambari kullanıcı adı kullanarak normal bir küme bağlama, ayrıca güvenlik hadoop kümesi etki alanı kullanıcı adı kullanarak bağlantı (örneğin: user1@contoso.com).
1. Komut paletini seçerek açmak **CTRL + SHIFT + P**ve ardından girin **Hdınsight: küme bağlantı**.

   ![bağlantı küme komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Hdınsight girin kümesi URL'sini -> kullanıcı adı -> giriş giriş parola Seç -> küme türü -> bunu gösterir başarı bilgisi doğrulama aktarılırsa.
   
   ![bağlantı küme iletişim](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanın. 
   
3. Komutunu kullanarak bir bağlı küme görebilirsiniz **listesi küme**. Şimdi, bir komut dosyası bu bağlantılı kümeye gönderebilirsiniz.

   ![bağlantılı küme](./media/hdinsight-for-vscode/linked-cluster.png)

4. Giriş yapma tarafından bir küme kesebilirsiniz **Hdınsight: küme bağlantısını** komutu paletindeki.

## <a name="list-hdinsight-clusters"></a>Hdınsight kümeleri listesi

Bağlantıyı sınamak için Hdınsight kümelerinizi listeleyebilirsiniz:

**Hdınsight kümeleri, Azure aboneliğinizin altında listelemek için**
1. Bir çalışma alanını açın ve Azure'a bağlayın. Daha fazla bilgi için bkz: [açık Hdınsight çalışma](#open-hdinsight-workspace) ve [Azure Bağlan](#connect-to-azure).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: liste küme** ve bağlam menüsünden. 

3. Hive ve Spark kümeleri görünür **çıkış** bölmesi.

    ![Bir varsayılan küme yapılandırmasını ayarlayın](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Varsayılan kümesi
1. Bir çalışma alanını açın ve Azure'a bağlayın. Bkz: [açık Hdınsight çalışma](#open-hdinsight-workspace) ve [Azure Bağlan](#connect-to-azure).

2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: varsayılan küme**. 

3. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçlar otomatik olarak yapılandırma dosyasını güncelleştirme **XXXX_hdi_settings.json**. 

   ![kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Azure ortamı ayarlayın 
1. Komut paletini seçerek açmak **CTRL + SHIFT + P**.

2. Girin **Hdınsight: Azure ortamı ayarlayın**.

3. Bir yolu varsayılan oturum açma girişi olarak Azure ve AzureChina seçin.

4. Aracı zaten kaydedildi, varsayılan oturum açma girişi bu sırada, **XXXX_hdi_settings.json**. Ayrıca doğrudan yapılandırma dosyasındaki güncelleştirmeniz. 

   ![kümesi varsayılan oturum açma giriş yapılandırması](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Etkileşimli Hive sorguları gönderme

VS Code'da için Hdınsight araçları ile etkileşimli Hive sorguları Hdınsight etkileşimli sorgu kümelerine gönderebilir.

1. Bunları zaten yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.

2. Azure hesabınıza bağlanın ve zaten yapmadıysanız varsayılan küme yapılandırın.

3. Kopyalama ve Hive dosyanıza aşağıdaki kodu yapıştırın ve dosyayı kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: Hive etkileşimli** sorgu. Araçları, bağlam menüsünü kullanarak tüm komut dosyası yerine kod bloğunu göndermenize olanak sağlar. Kısa süre içinde sorgu sonuçları yeni bir sekmede görüntülenir.

   ![Etkileşimli Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçları** paneli: tüm sonucu yerel yol CSV, JSON veya Excel dosyası olarak kaydetmek veya yalnızca birden fazla satır seçin.

    - **İLETİLERİ** paneli: seçtiğinizde, **satır** numarası, çalışan betik ilk satırını atlar.

Etkileşimli sorgu çalıştırmanın daha çok daha az zaman alır [Hive toplu iş çalıştıran](#submit-hive-batch-scripts).

## <a name="submit-hive-batch-scripts"></a>Hive toplu komut gönderme

1. Bunları zaten yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.

2. Azure hesabınıza bağlanın ve zaten yapmadıysanız varsayılan küme yapılandırın.

3. Kopyalama ve Hive dosyanıza aşağıdaki kodu yapıştırın ve dosyayı kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: Hive toplu** Hive işi göndermek için. 

4. Göndermek istediğiniz kümeyi seçin.  

    Hive işi gönderdikten sonra gönderme başarı bilgileri ve JobId görünür **çıkış** paneli. Hive işi da açılır **WEB tarayıcısı**, gerçek zamanlı iş günlükleri ve durumunu gösterir.

   ![Hive işi sonucu Gönder](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

[Etkileşimli Hive sorguları gönderme](#submit-interactive-hive-queries) toplu iş gönderme çok daha az zaman alır.

## <a name="submit-interactive-pyspark-queries"></a>Etkileşimli PySpark sorguları gönderme
VS Code'da için Hdınsight araçları Ayrıca, Spark kümeleri etkileşimli PySpark sorguları göndermek sağlar.
1. Bunları zaten yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası .py uzantılı oluşturun.

2. Henüz yapmadıysanız Azure hesabınıza bağlanın.

3. Kopyalayın ve betik dosyasına aşağıdaki kodu yapıştırın:
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
4. Bu komut dosyalarını vurgulayın. Daha sonra komut dosyası Düzenleyicisi'ni sağ tıklatın ve seçin **Hdınsight: PySpark etkileşimli**.

5. Henüz yüklemediyseniz **Python** VS code'da uzantı Seç **yükleme** düğme aşağıdaki çizimde gösterildiği gibi:

    ![Hdınsight Visual Studio kod Python yükleme için](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Henüz yapmadıysanız Python ortamı sisteminizde yükleyin. 
   - Windows için indirme ve yükleme [Python](https://www.python.org/downloads/). Emin olun `Python` ve `pip` sistem yolu.

   - MacOS ve Linux için yönergeler için bkz: [için Visual Studio Code PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md).

7. PySpark sorgunuzu gönderebilirsiniz kurmak için bir kümeyi seçin. Hemen sonra sorgu sonucu yeni sağ sekmesinde gösterilir:

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Aracı da destekler **SQL yan tümcesi** sorgu.

   ![Python sonuç iş gönderme](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) alt durum çubuğunda sorgular çalıştırdığınız zaman sol gönderme durumu görüntülenir. Durum diğer sorgular Gönder yok **PySpark Çekirdeği (meşgul)**. 

>[!NOTE]
>Kümeleri oturum bilgilerini koruyabilir. Aynı kümenin birden fazla hizmeti çağrıları arasında başvurulabilir şekilde tanımlanan değişken, işlevi ve karşılık gelen değerler oturumda tutulur. 
 

## <a name="submit-pyspark-batch-job"></a>PySpark toplu iş gönderme

1. Bunları zaten yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası .py uzantılı oluşturun.

2. Önceden yapmadıysanız Azure hesabınıza bağlanın.

3. Kopyalayın ve betik dosyasına aşağıdaki kodu yapıştırın:

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
4. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: PySpark toplu**. 

5. PySpark işi göndermek için bir kümeye seçin. 

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Python işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS Code penceresinde. **Spark UI URL** ve **Yarn UI URL** da gösterilir. URL iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.


   


## <a name="additional-features"></a>Ek Özellikler

Hdınsight VS Code için aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Öneriler anahtar sözcüğü, yöntemleri, değişkenler ve benzeri için açılır. Farklı simgeler farklı tip nesneyi temsil eder.

    ![Visual Studio kod IntelliSense nesne türleri için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaret**. Dil hizmeti Hive betiğini düzenleme hatalarla altını çizer.     
- **Sözdizimi vurgular**. Dil hizmeti farklı renkler değişkenleri, anahtar sözcükler, veri türü, İşlevler ve benzeri ayırt etmek için kullanır. 

    ![Visual Studio Code sözdizimi vurgular için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* VS Code'da Hdınsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight’ta Microsoft Power BI ile Hive verileri görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Etkileşimli sorgu Hive verileri Azure hdınsight'ta Power BI ile görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Visual Studio kodunu PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md)
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](./hdinsight-connect-hive-zeppelin.md)

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



