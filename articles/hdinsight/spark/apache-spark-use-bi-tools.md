---
title: "Spark Azure Hdınsight'ta veri görselleştirme araçları kullanarak BI | Microsoft Docs"
description: "Hdınsight kümelerinde Apache Spark BI'ı kullanarak analiz için verileri görselleştirme araçlarını kullanın"
keywords: "Apache spark BI, spark BI, spark veri görselleştirme, spark iş zekası"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: d1d5405a635b9f990f53b2bf32c8270a71a0f344
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark Azure Hdınsight ile verileri görselleştirme araçlarını kullanarak BI

Hdınsight kümelerinde Apache Spark BI'ı kullanarak bir ham örnek veri kümesi analiz etmek için Power BI ve Tableau gibi veri görselleştirme araçlarını kullanmayı öğrenin.

> [!NOTE]
> Bu makalede açıklanan BI araçları ile bağlantı Spark 2.1 Azure Hdınsight 3.6 Önizleme üzerinde desteklenmiyor. Yalnızca Spark sürüm 1.6 ve 2.0 (Hdınsight 3.4, 3.5 sırasıyla) desteklenir.
>

Bu öğretici, bir Hdınsight Spark kümesinde Jupyter not defteri olarak da kullanılabilir. Not Defteri deneyimi Python parçacıkları dizüstü çalıştırmadan olanak sağlar. Gelen öğretici bir not defteri içinde gerçekleştirmek için bir Spark kümesi oluşturma, Jupyter not defteri başlatın (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ve ardından not defteri çalıştırın **HDInsight.ipynb Apache Spark kullanım BI araçlarıyla** altında **Python** klasör.

## <a name="prerequisites"></a>Ön koşullar

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Spark veri görselleştirme için verileri hazırlama

Bu bölümde, kullanırız [Jupyter](https://jupyter.org) ham örnek verileri işlemek ve bir tablo olarak kaydetmek işlerini çalıştırmak için Hdınsight Spark kümesinde dizüstü bilgisayarınızı. Örnek, bir .csv dosyası (hvac.csv) kullanılabilir varsayılan olarak tüm kümelerde verilerdir. Verilerinizi bir tablo olarak kaydedildikten sonra sonraki bölümde BI araçları tabloya bağlanmak ve veri görselleştirmeleri gerçekleştirmek için kullanırız.

> [!NOTE]
> Adımları bu makaledeki yönergeleri tamamladıktan sonra gerçekleştirdiğiniz varsa [bir Hdınsight Spark kümesinde etkileşimli sorgular gerçekleştirme](apache-spark-load-data-run-query.md), aşağıdaki adım 8'e atlayabilirsiniz.
>

1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   

2. Spark kümesi dikey penceresinden **Küme Panosu**’na ve ardından **Jupyter Notebook**’a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Apache Spark BI için bir Jupyter not defteri oluşturma](./media/apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "için Apache Spark BI Jupyter not defteri oluşturma")

4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Apache Spark BI için not defteri için ad](./media/apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "için Apache Spark BI dizüstü bilgisayar için bir ad sağlayın")

5. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Bunu yapmak için imleci hücre ve tuşuna koyun **SHIFT + ENTER**.

        from pyspark.sql import *

6. Örnek verilerini geçici bir tabloya yükleyin. HDInsight’ta bir Spark kümesi oluşturduğunuzda **hvac.csv** örnek veri dosyası, **\HdiSamples\HdiSamples\SensorSampleData\hvac** altındaki ilişkili depolama hesabına kopyalanır.

    Aşağıdaki kod parçacığında ve tuşuna boş bir hücreye yapıştırın **SHIFT + ENTER**. Bu kod parçacığında veriler adı verilen bir tabloya kaydeder **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Tablo başarıyla oluşturulduğunu doğrulayın. Kullanabileceğiniz `%%sql` Hive çalıştırmak için Sihirli sorgular doğrudan. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Aşağıda gösterildiği gibi bir çıktı bakın:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    False altında olan tabloları **isTemporary** sütun olacak şekilde meta depo içinde saklanan ve BI Araçları'ndan erişilebilen hive tablosu. Bu öğreticide, biz bağlanmak **hvac** oluşturduğumuz tablo.

8. Tablo hedeflenen verileri içerdiğini doğrulayın. Not defterindeki boş hücreye aşağıdaki kod parçacığında ve tuşuna kopyalama **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Kaynakları serbest bırakmak için Not Defteri kapatın. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın.

## <a name="powerbi"></a>Power BI için Spark veri görselleştirme kullanın

> [!NOTE]
> Bu bölüm, yalnızca Hdınsight 3.4 üzerinde Spark 1.6 ve Hdınsight 3.5 Spark 2.0 için geçerlidir.
>
>

Verileri tablo olarak kaydettikten sonra veri bağlanmak ve raporlar, panolar vb. oluşturmak üzere görselleştirmek için Power BI'ı kullanabilirsiniz.

1. Power BI erişebildiğinizden emin olun. Power BI'dan, ücretsiz önizlemeye aboneliği alabilirsiniz [http://www.powerbi.com/](http://www.powerbi.com/).

2. Oturum [Power BI](http://www.powerbi.com/).

3. Sol bölmede aşağıdan tıklatın **Veri Al**.

4. Üzerinde **Veri Al** sayfasında **alma veya verilere bağlanın**, için **veritabanları**, tıklatın **almak**.

    ![Apache Spark BI için Power BI Veri Al](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "veri alma Power BI için Apache Spark BI")

5. Sonraki ekranda, tıklatın **Azure hdınsight'ta Spark** ve ardından **Bağlan**. İstendiğinde, kümesi URL'sini girin (`mysparkcluster.azurehdinsight.net`) ve kümeye bağlanmak için kimlik bilgileri.

    ![Apache Spark BI bağlanmak](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Apache Spark BI Bağlan")

    Bağlantı kurulduktan sonra Power BI veri hdınsight'ta Spark kümesinde alma başlatır.

6. Power BI verileri alır ve ekler bir **Spark** veri kümesi altında **veri kümeleri** başlığı. Verileri görselleştirmek için yeni bir çalışma sayfasını açmak için bu veri kümesi'ı tıklatın. Bu gibi durumlarda, çalışma sayfası aynı zamanda bir rapor olarak kaydedebilirsiniz. Bir çalışma alanından kaydetmek için **dosya** menüsünde tıklatın **kaydetmek**.

    ![Power BI panosundaki Apache Spark BI bölümünden](./media/apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Power BI panosuna Apache Spark BI kutucuğu")
7. Dikkat **alanları** sağ listeleri listesinde **hvac** daha önce oluşturduğunuz tablo. Not defterinde daha önce tanımlanan tablo alanları görmek için tabloyu genişletin.

      ![Apache Spark BI Panoda Tabloları Listele](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Apache Spark BI Panoda Tabloları Listele")

8. Hedef sıcaklık ve her derleme için gerçek sıcaklık arasındaki fark göstermek için bir görsel öğe oluşturun. Verilerinizi görselleştirmek için seçin **alan grafiği** (kırmızı kutu içinde gösterilmiştir). Sürükle ve bırak ekseni tanımlamak için **BuildingID** altında **eksen**, ve **ActualTemp**/**TargetTemp** alanları altında **değeri**.

    ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

9. Varsayılan olarak görselleştirme için toplam gösterir **ActualTemp** ve **TargetTemp**. Her iki alanlardan, açılan seçin **ortalama** gerçek ortalama ve hedef etme için iki bina almak için.

    ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

10. Veri görselleştirme benzer ekran görüntüsü olması gerekir. Araç ipuçları ile ilgili verileri almak için görselleştirme üzerinden imlecinizi taşıyın.

    ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

11. Tıklatın **kaydetmek** üstteki menüden ve bir rapor adı sağlayın. Ayrıca visual sabitleyebilirsiniz. Bir görsel öğe PIN, böylece bir bakışta son değer izleyebilirsiniz Panonuzda depolanır.

   Aynı veri kümesi için istediğiniz ve bunları, verilerin bir anlık görüntüsünü panoya Sabitle sayıda görsel öğeleri ekleyebilirsiniz. Ayrıca, hdınsight'ta Spark kümeleri için Power BI ile doğrudan bağlı bağlanın. Bu veri kümesi için yenileme zamanlamanız gerekmez için Power BI her zaman en güncel verileri kümenizi sahip olmasını sağlar.

## <a name="tableau"></a>Spark veri görselleştirme için Tableau Masaüstü'nü kullanın

> [!NOTE]
> Bu bölüm, yalnızca Azure Hdınsight'ta oluşturulan 1.5.2 Spark kümeleri için geçerlidir.
>
>

1. Yükleme [Tableau Masaüstü](http://www.tableau.com/products/desktop) bu Apache Spark BI öğretici çalıştırdığınız bilgisayarda.

2. Bu bilgisayarda ayrıca Microsoft Spark ODBC sürücüsü yüklü olduğundan emin olun. Sürücüsünden yükleyebilirsiniz [burada](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Tableau Masaüstü başlatın. Sol bölmede, bağlanmak üzere sunucu listesinden tıklatın **Spark SQL**. Sol bölmede varsayılan Spark SQL listelenmemişse tıklatın bulabilirsiniz **daha sunucuları**.
2. Spark SQL Bağlantısı iletişim kutusunda aşağıdaki ekran görüntüsünde gösterildiği gibi değerler sağlayın ve ardından **Tamam**.

    ![Apache Spark BI için bir kümeye Bağlan](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Apache Spark BI için bir kümeye Bağlan")

    Kimlik doğrulama açılan listeleri **Microsoft Azure Hdınsight hizmeti** yalnızca yüklediyseniz bir seçenek olarak [Microsoft Spark ODBC sürücüsü](http://go.microsoft.com/fwlink/?LinkId=616229) bilgisayarda.
3. Sonraki ekranda, gelen **şema** açılır menüsünde tıklatın **Bul** simgesine ve ardından **varsayılan**.

    ![Şema bulmak için Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Apache Spark BI Bul şeması")
4. İçin **tablo** alan, tıklatın **Bul** yeniden kümedeki kullanılabilir tüm Hive tablolarını listelemek için simge. Görmeniz gerekir **hvac** daha önce Not Defteri kullanarak oluşturduğunuz tablo.

    ![Tablo için Apache Spark BI Bul](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Apache Spark BI Bul tablosu")
5. Sürükleyin ve üst kutusuna sağ taraftaki tablo bırakın. Tableau verileri alır ve şema kırmızı kutu ile vurgulanan görüntüler.

    ![Ekleme tablolar için Tableau için Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "eklemek tablolar için Tableau için Apache Spark BI")
6. Tıklatın **Sheet1** sekmesi altındaki sol. Tüm binalar için gerçek etme ve ortalama hedef her tarihini gösteren bir görsel öğe olun. Sürükleme **tarih** ve **kimliği oluşturma** için **sütunları** ve **gerçek Temp**/**hedef Temp** için **satırları**. Altında **işaretleri**seçin **alanı** bir alan eşlemesini Spark veri görselleştirme için kullanabilirsiniz.

     ![Spark veri görselleştirme için alanlar ekleyin](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Spark veri görselleştirme için alanlar ekleyin")
7. Varsayılan olarak, gösterilen sıcaklık alanlar toplama olarak. Bunun yerine ortalama etme göstermek istiyorsanız, açılan listeden, aşağıda gösterildiği gibi bunu yapabilirsiniz.

    ![Sıcaklık Spark veri görselleştirme için ortalama ele](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "sıcaklık Spark veri görselleştirme için ortalama alın")

8. Ayrıca Süper-bir ısı Haritası hedef gerçek etme arasındaki farkı daha iyi bir fikir almak için diğer üzerinden uygulayabilir. Kırmızı bir daire vurgulanmış tanıtıcı şekli gördüğünüz kadar fareyi alt alan eşleme köşesine getirin. Harita diğer eşlemeye üstte sürükleyin ve kırmızı dikdörtgende vurgulanan şekli gördüğünüzde fare düğmesini bırakın.

    ![MAPS Spark veri görselleştirme için birleştirme](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "birleştirme eşler için Spark veri Görselleştirme")

     Veri görselleştirme ekran görüntüsünde gösterildiği gibi değiştirmeniz gerekir:

    ![Spark veri görselleştirme için tableau çıkış](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau çıktı için Spark veri Görselleştirme")
9. Tıklatın **kaydetmek** çalışma kaydetmek için. Panolar oluşturun ve bir veya daha fazla sayfa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Şu ana kadar bir küme oluşturmak, veri çerçevelerini sorgu verileri için Spark oluşturun ve sonra BI Araçları'ndan bu verilere erişmek nasıl öğrendiniz. Şimdi, küme kaynaklarını yönetmek ve bir Hdınsight Spark kümesinde çalışan işlerin hata ayıklamak yönergeler da bakabilirsiniz.

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

