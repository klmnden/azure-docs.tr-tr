---
title: "Azure Hdınsight araçları - Hive, LLAP veya pySpark için Visual Studio Code kullanma | Microsoft Docs"
description: "Azure Hdınsight araçları Visual Studio Code için oluşturma, sorgular ve komut dosyaları göndermek için nasıl kullanacağınızı öğrenin."
Keywords: "VScode, Azure Hdınsight araçları, Hive, Python, PySpark, Spark, Hdınsight, Hadoop, LLAP, etkileşimli Hive, etkileşimli sorgu"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: jgao
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 36ce117076ed5c15ddff850485d8f8912ec53caf
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="use-azure-hdinsight-tool-for-visual-studio-code"></a>Visual Studio kodunu Azure Hdınsight aracını kullanın

Azure Hdınsight araçları Visual Studio Code (VSCode) oluşturmak, Hive toplu işleri, etkileşimli Hive sorguları ve pySpark betikleri göndermek için nasıl kullanacağınızı öğrenin. Azure Hdınsight araçları, Windows, Linux ve MacOS dahil olmak üzere VSCode tarafından desteklenen platformlardaki yüklenebilir. Farklı platformlar için Önkoşullar bulabilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki öğeler, bu makalede tamamlamak için gereklidir:

- Bir Hdınsight kümesi.  Bir küme oluşturmak için bkz: [Hdınsight kullanmaya başlama]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono, yalnızca Linux ve MacOS için gereklidir.

## <a name="install-the-hdinsight-tools"></a>Hdınsight Araçları'nı yükleme
   
Önkoşulları yükledikten sonra Azure Hdınsight araçları için VSCode yükleyebilirsiniz. 

**Yükleme Azure Hdınsight araçları**

1. Açık **Visual Studio Code**.
2. Tıklatın **uzantıları** sol bölmede. Girin **Hdınsight** arama kutusuna.
3. Tıklatın **yükleme** yanına **Azure Hdınsight Araçları**. Birkaç saniye sonra **yükleme** düğmesi değiştirilecek **yeniden**.
4. Tıklatın **yeniden** etkinleştirmek için **Azure Hdınsight Araçları** uzantısı.
5. Tıklatın **yeniden yükleme penceresi** onaylamak için. Gördüğünüz **Azure Hdınsight Araçları** uzantıları bölmesinde.

   ![Hdınsight Visual Studio kod Python yükleme için](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Hdınsight çalışma alanını açın

Azure'a bağlanabilmesi için önce bir çalışma alanı içinde VSCode oluşturmak.

**Bir çalışma alanını açmak için**

1. Gelen **dosya** menüsünde tıklatın **Klasör Aç**, varolan bir klasörü belirtmek veya çalışma klasörü olarak yeni bir klasör oluşturun. Klasör soldaki bölmede görünür.
2. Sol bölmeden tıklatın **yeni dosya** iş klasörünün yanındaki simge.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)
3. (Hive sorguları) .hql veya .py (Spark betik) dosya uzantısı ile yeni dosya adı. Bildirim bir **XXXX_hdi_settings.json** yapılandırma dosyası için çalışma klasörü otomatik olarak eklenir.
4. Açık **XXXX_hdi_settings.json** gelen **EXPLORER**, veya seçmek için komut dosyası Düzenleyicisi sağ **yapılandırma kümesi**. Oturum açma girişi, varsayılan küme ve iş gönderme parametreleri, dosya aşağıdaki örnekte gösterildiği gibi yapılandırabilirsiniz. Ayrıca kalan parametreler boş bırakabilirsiniz.

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Betikler VSCode Hdınsight kümelerine gönderebilir önce Azure hesabınızda bağlanmanız.

**Azure'a bağlanmak için**

1. Yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası oluşturun.
2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: oturum açma** ve bağlam menüsünden. Tuşlarına da basabilirsiniz **CTRL + SHIFT + P** ve girerek **Hdınsight: oturum açma**.

    ![Visual Studio Code için Hdınsight araçları oturum açın](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)
3. Oturum açma yönergeleri izleyin **çıkış** oturum açmak için bölmesi.

    **Azure:** ![Visual Studio Code oturum açma bilgileri için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Bağlantı kurulduktan sonra Azure hesap adınızı VSCode penceresinin sol alttaki durum çubuğunda gösterilir. 

    > [!NOTE]
    > Özel mod veya bilinen Azure kimlik doğrulama sorunu nedeniyle incognito mod ile tarayıcıyı açın. Azure hesabınızda etkin iki faktör varsa, PIN yerine telefon kimlik doğrulamasını kullanmak için önerilir.
  

4. Bağlam menüsünü açmak için komut dosyası Düzenle sağ tıklayın. Bağlam menüsünden aşağıdaki görevleri gerçekleştirebilirsiniz:

    - oturum kapatma
    - Liste kümeleri
    - Varsayılan kümesi
    - Etkileşimli Hive sorguları gönderme
    - Hive toplu betiği Gönder
    - Etkileşimli PySpark sorguları gönderme
    - PySpark toplu betiği Gönder
    - Kümesi yapılandırması

## <a name="list-hdinsight-clusters"></a>Hdınsight kümeleri listesi

Bağlantıyı sınamak için Hdınsight kümelerinizi listeleyebilirsiniz:

**Hdınsight kümeleri, Azure aboneliğinizin altında listelemek için**
1. Bir çalışma alanını açın ve Azure'a bağlayın. Bkz: [açık Hdınsight çalışma](#open-hdinsight-workspace) ve [Azure Bağlan](#connect-to-azure).
2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: liste küme** ve bağlam menüsünden. 
3. Hive ve Spark kümeleri görünür **çıkış** bölmesi.

    ![kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Varsayılan kümesi
1. Bir çalışma alanını açın ve Azure'a bağlayın. Bkz: [açık Hdınsight çalışma](#open-hdinsight-workspace) ve [Azure Bağlan](#connect-to-azure).
2. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: varsayılan küme**. 
3. Geçerli komut dosyası için varsayılan küme olarak bir küme seçin. Araçlar otomatik olarak yapılandırma dosyasını güncelleştirme **XXXX_hdi_settings.json**. 

   ![kümesi varsayılan küme yapılandırması](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-azure-environment"></a>Azure ortamı ayarlayın 
1. Tuşuna basarak komut paletini açmak **CTRL + SHIFT + P**.
2. Girin **Hdınsight: Azure ortamı ayarlayın**.
3. Bir yolu varsayılan oturum açma girişi olarak Azure ve AzureChina seçin.
4. Bu sırada, sunduğumuz aracı zaten varsayılan oturum açma giriş olarak seçilen kaydedilmiş **XXXX_hdi_settings.json**. Ayrıca doğrudan yapılandırma dosyasındaki güncelleştirmeniz. 

   ![kümesi varsayılan oturum açma giriş yapılandırması](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Etkileşimli Hive sorguları gönderme

VSCode için Hdınsight araçları, Hdınsight etkileşimli sorgu kümelerine etkileşimli Hive sorguları göndermek etkinleştirin.

1. Yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.
2. Azure hesabınıza bağlanın ve bunu yapmadıysanız varsayılan küme yapılandırın.
3. Kopyalama ve Hive dosyanıza aşağıdaki kodu yapıştırın, ardından dosyayı kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: Hive etkileşimli** sorgu. Araçları, bağlam menüsünü kullanarak tüm komut dosyası yerine kod bloğunu göndermenize olanak sağlar. Hemen sonra sorgu sonucu yeni bir sekmede gösterilir:

   ![Etkileşimli Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçları** paneli: tüm sonucu yerel yolu CSV, JSON, EXCEL kaydetmek veya yalnızca birden fazla satır seçin.
    - **İLETİLERİ** paneli: tıklatarak **satır** numarası, çalışan betik ilk satırını atlar.

İçin karşılaştırma [Hive toplu iş çalıştıran](#submit-hive-batch-scripts), etkileşimli sorgu çok daha kısa sürer.

## <a name="submit-hive-batch-scripts"></a>Hive toplu komut gönderme

1. Yoksa, yeni bir çalışma klasörü ve yeni bir Hive betik dosyası oluşturun.
2. Azure hesabınıza bağlanın ve bunu yapmadıysanız varsayılan küme yapılandırın.
3. Kopyalama ve Hive dosyanıza aşağıdaki kodu yapıştırın, ardından dosyayı kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: Hive toplu** Hive işi göndermek için. 
4. Göndermek istediğiniz kümeyi seçin.  

    Hive işi gönderdikten sonra gönderme başarı bilgileri ve JobId görüntülenir **çıkış** paneli. Ve açar **WEB tarayıcısı** iş gerçek zamanlı günlüğe kaydeden ve gösterilen durumu.

   ![Hive işi sonucu Gönder](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

İçin karşılaştırma [etkileşimli Hive sorguları gönderme](#submit-interactive-hive-queries), toplu iş çok uzun sürüyor.

## <a name="submit-interactive-pyspark-queries"></a>Etkileşimli PySpark sorguları gönderme
VSCode için Hdınsight araçları ayrıca Spark kümeleri etkileşimli PySpark sorguları göndermek etkinleştirin.
1. Yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası .py uzantılı oluşturun.
2. Bunu yapmadıysanız Azure hesabınıza bağlanın.
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
4. Bu komut dosyalarını vurgulayın ve komut dosyası Düzenleyicisi'ni sağ tıklatın ve ardından **Hdınsight: PySpark etkileşimli**.
5. Aşağıdaki **yükleme** değil yüklediyseniz düğmesini **Python** VSCode uzantı.
    ![Hdınsight Visual Studio kod Python yükleme için](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Bunu yüklemezseniz, sisteminizdeki python ortamı ayarlayın. 
   - Windows için indirme ve yükleme [Python](https://www.python.org/downloads/). Emin olun `Python` ve `pip` sisteminizdeki yolu.
   - MacOS ve Linux için yönerge bkz [ayarlamak yukarı PySpark etkileşimli ortamı için Visual Studio Code](set-up-pyspark-interactive-environment.md).
7. PySpark sorgunuzu göndermek için bir kümeyi seçin. Hemen sonra sorgu sonucu şu yeni sekmede gösterilir:

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Aracımız de sorgu destekler **SQL yan tümcesi**.

   ![Python sonuç iş gönderme](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) sorgu çalıştırırken alt durum çubuğunda sol tarafta gönderme durumunu görüntüler. Durum olduğunda diğer sorgular gönderemiyor **PySpark Çekirdeği (meşgul)**, aksi halde, çalışan askıda kalır.
9. Bizim kümeleri oturumu koruyabilirsiniz. Örneğin, **bir = 100**zaten bu oturumu kümede tutmak, artık yalnızca çalıştırdığınız **yazdırma bir** kümeye.
 

## <a name="submit-pyspark-batch-job"></a>PySpark toplu iş gönderme

1. Yoksa, yeni bir çalışma klasörü ve yeni bir komut dosyası .py uzantılı oluşturun.
2. Bunu yapmadıysanız Azure hesabınıza bağlanın.
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
5. PySpark işi göndermek için bir kümeyi seçin. 

   ![Python iş sonucu Gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Python işi gönderdikten sonra gönderim günlükleri görüntülenir **çıkış** VSCode penceresinde. **Spark UI URL** ve **Yarn UI URL** da gösterilir. URL iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.


## <a name="additional-features"></a>Ek Özellikler

Hdınsight VSCode için aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Öneriler Sil'i anahtar sözcüğü, yöntem, değişkenler vb. Farklı simgeler farklı tip nesneyi temsil eder:

    ![Visual Studio kod IntelliSense nesne türleri için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaret**. Dil hizmeti Hive betiğini düzenleme hatalarla altını çizer.     
- **Sözdizimi vurgular**. Dil hizmeti değişkenleri, anahtar sözcükler, veri türü, İşlevler, vb. ayırt etmek için farklı bir renk kullanır. 

    ![Visual Studio Code sözdizimi vurgular için Hdınsight araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* Hdınsight için VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Visual Studio kodunu PySpark etkileşimli ortamını ayarlama](set-up-pyspark-interactive-environment.md)
* [Oluşturma ve Spark Scala uygulamaları göndermek amacıyla Intellij için Azure araç seti kullanın](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](hdinsight-apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](./hdinsight-connect-hive-power-bi.md).
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](./hdinsight-connect-hive-zeppelin.md).

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](hdinsight-apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark akış: Gerçek zamanlı akış uygulamaları oluşturmak için hdınsight'ta Spark kullanma](hdinsight-apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Oluşturma ve uygulamaları çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](hdinsight-apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="managing-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](hdinsight-apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](hdinsight-apache-spark-job-debugging.md)



