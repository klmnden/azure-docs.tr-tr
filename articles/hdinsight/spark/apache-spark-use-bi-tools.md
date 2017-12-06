---
title: "Spark Azure Hdınsight'ta veri görselleştirme araçları kullanarak BI | Microsoft Docs"
description: "Hdınsight kümelerinde Apache Spark BI'ı kullanarak analiz için verileri görselleştirme araçlarını kullanın"
keywords: "Apache spark BI, spark BI, spark veri görselleştirme, spark iş zekası"
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 18f495864befafd26e7adafb5c01612222d2cfdf
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark Azure Hdınsight ile verileri görselleştirme araçlarını kullanarak BI

Nasıl kullanacağınızı öğrenin [Microsoft Power BI](http://powerbi.microsoft.com) ve [Tableau](http://www.tableau.com) Azure hdınsight'ta Apache Spark kümesinde verileri görselleştirmek için.

## <a name="prerequisites"></a>Ön koşullar

* **Tam [hdınsight'ta Spark kümeleri üzerinde etkileşimli sorgular gerçekleştirme](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) ve [Power BI deneme aboneliği](https://app.powerbi.com/signupredirect?pbi_source=web) (isteğe bağlı).
* **Tableau**: [Tableau Masaüstü](http://www.tableau.com/products/desktop) ve [Microsoft Spark ODBC sürücüsü](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Verileri doğrulama

Oluşturduğunuz Jupyter not defteri [önceki öğretici](apache-spark-load-data-run-query.md) oluşturmak için kodu içeren bir `hvac` tablo. Bu tablo, tüm Hdınsight Spark kümeleri, CSV dosyası dayalı **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Verileri doğrulamak için aşağıdaki yordamı kullanın.

1. Jupyter not defteri aşağıdaki kodu yapıştırın ve sonra basın **SHIFT + ENTER**. Kod doğrular tabloları varlığını.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Çıktı şuna benzer:

    ![Tabloları Spark Göster](./media/apache-spark-use-bi-tools/show-tables.png)

    Bu öğreticiye başlamadan önce dizüstü bilgisayar kapalı `hvactemptable` , çıktıda dahil edilmeyen şekilde temizlendi.
    Yalnızca meta depo içinde depolanan tabloları Hive (belirttiği **False** altında **isTemporary** sütun) BI Araçları'ndan erişilebilir. Bu öğreticide, bağlandığınız **hvac** oluşturduğunuz tablo.

2. Boş bir hücreye aşağıdaki kodu yapıştırın ve sonra basın **SHIFT + ENTER**. Kod tablodaki verileri doğrular.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Çıktı şuna benzer:

    ![Spark hvac tablodaki Göster](./media/apache-spark-use-bi-tools/select-limit.png)

3. Gelen **dosya** dizüstü menüsünde **Kapat ve Durdur**. Kaynakları serbest bırakmak için Not Defteri kapatın. 















## <a name="powerbi"></a>Power BI kullanın

Bu bölümde, Spark küme verilerden görselleştirmeleri, raporları ve panoları oluşturmak için Power BI kullanın. 

### <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
Power BI Desktop'ta kümeye bağlanmak, kümeden veri yüklemek ve verilere dayanan bir temel görselleştirme oluşturmak için Spark ile çalışırken ilk adım değildir.

> [!NOTE]
> Bu makalede gösterilen bağlayıcı şu anda önizlemede değil. Sahip aracılığıyla geribildirim sağlamak [Power BI topluluk](https://community.powerbi.com/) site veya [Power BI fikirleri](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Açık [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Gelen **giriş** sekmesini tıklatın, **Veri Al**, ardından **daha fazla**.

    ![Power BI Desktop'a Hdınsight Apache Spark'tan Veri Al](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "veri alma Power BI'a Apache Spark BI'dan")


2. Girin `Spark` arama kutusunda **Azure Hdınsight Spark (Beta)**ve ardından **Bağlan**.

    ![Apache Spark BI'dan Power BI Veri Al](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "veri alma Power BI'a Apache Spark BI'dan")

3. Küme URL'nizi girin (biçiminde `mysparkcluster.azurehdinsight.net`), select **DirectQuery**ve ardından **Tamam**.

    Spark ile ya da veri bağlantı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesinin yenileme olmadan raporlarında yansıtılır. Veri içe aktarırsanız, değişiklikleri görmek için bu veri kümesi yenilemeniz gerekir. Hakkında daha fazla bilgi ve DirectQuery kullanıldığı durumlar için bkz: [kullanarak DirectQuery Power bı'da](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Hdınsight oturum açma hesabı bilgilerini girin ve ardından **Bağlan**. Varsayılan hesap adı *yönetici*.

5. Seçin `hvac` tablo, verilerin bir önizlemesini görmek için bekleyin ve ardından **yük**.

    ![Spark küme kullanıcı adı ve parola](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark küme kullanıcı adı ve parola")

    Power BI Desktop sahip Spark küme ve yük verileri bağlanmak için gereken bilgileri `hvac` tablo. Tablo ve sütunlarını görüntülenen **alanları** bölmesi.  Aşağıdaki ekran görüntüsüne bakın.

6. Hedef sıcaklık ve her derleme için gerçek sıcaklık arasındaki fark görselleştirin: 

    1. İçinde **GÖRSELLEŞTİRMELERİ** bölmesinde, **alan grafiği**. 
    2. Sürükleme **BuildingID** alanı **eksen**, sürükleyin **ActualTemp** ve **TargetTemp** alanları **değeri**.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

        Diyagram şuna benzer:

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

        Varsayılan olarak görselleştirme için toplam gösterir **ActualTemp** ve **TargetTemp**. Yanındaki aşağı oka tıklayın **ActualTemp** ve **TragetTemp** görselleştirmeleri bölmesinde gördüğünüz **toplam** seçilir.

    3. Aşağı ok tıklayın **ActualTemp** ve **TragetTemp** görselleştirmeleri bölmesinde seçin **ortalama** gerçek ortalama ve hedef etme her biri için almak için Yapı.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

        Veri görselleştirme benzer ekran görüntüsü olacaktır. Araç ipuçları ile ilgili verileri almak için görselleştirme üzerinden imlecinizi taşıyın.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

7. Tıklatın **dosya** sonra **kaydetmek**ve bir ad girin `BuildingTemperature.pbix` dosyası için. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Rapor Power BI hizmetine (isteğe bağlı) yayımlayın

Power BI hizmetinde raporlar ve panolar, kuruluşunuzda paylaşmanıza olanak tanır. Bu bölümde, ilk veri kümesini ve raporu yayımlayın. Ardından, bir Pano rapora sabitleyin. Panolar, genellikle bir rapordaki verilerin bir alt kümesini odaklanmak için kullanılır; Raporunuzda yalnızca bir görselleştirme sahip ancak adımları gitmek hala faydalıdır.

1. Power BI Desktop açın.
2. Gelen **giriş** sekmesini tıklatın, **Yayımla**.

    ![Power BI masaüstünden yayımlama](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Power BI masaüstünden yayımlama")

2. Veri kümenizi yayımlamak için bir çalışma alanı seçin ve ardından rapor **seçin**. Aşağıdaki görüntüde, varsayılan **çalışma Alanım** seçilir.

    ![Veri kümesini yayımlamak ve rapor için çalışma alanı seçin](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "veri kümesini yayımlamak ve rapor için Select çalışma") 

3. Yayımlama başarılı sonra tıklayın **Power bı'da Aç 'BuildingTemperature.pbix'**.

    ![Başarı yayımlama, kimlik bilgilerini girmek için tıklatın](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "başarı yayımlama, kimlik bilgilerini girmek için tıklatın") 

4. Power BI hizmetinde tıklatın **kimlik bilgilerini girin**.

    ![Power BI hizmetinde kimlik bilgilerini girin](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI hizmetinde kimlik bilgilerini girin")

5. Tıklatın **kimlik bilgilerini Düzenle**.

    ![Power BI hizmetinde kimlik bilgilerini Düzenle](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Power BI hizmetinde kimlik bilgilerini Düzenle")

6. Hdınsight oturum açma hesabı bilgilerini girin ve ardından **oturum**. Varsayılan hesap adı *yönetici*.

    ![Spark kümesi oturum](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Spark küme için oturum açın")

7. Sol bölmede, Git **çalışma alanları** > **çalışma Alanım** > **RAPORLARI**, ardından **BuildingTemperature**.

    ![Raporu rapor sol bölmede altında listelenen](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "raporları sol bölmede altında listelenen raporu")

    Ayrıca görmelisiniz **BuildingTemperature** altında listelenen **veri KÜMELERİ** sol bölmede.

    Power BI Desktop'ta oluşturulan visual Power BI hizmetinde kullanıma sunulmuştur. 

8. İmlecinizi görselleştirme getirin ve ardından sağ üst köşesinde PIN simgesine tıklayın.

    ![Power BI hizmetinde raporda](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Power BI hizmetindeki raporu")

9. "Yeni Pano" seçin, bir ad girin `Building temperature`, ardından **PIN**.

    ![Yeni panoya Sabitle](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "yeni panoya Sabitle")

10. Rapora tıklayın **panoya gitmek**. 

Visual panosuna sabitlediğiniz - diğer görsellerin rapora ekleyin ve bunları aynı panoya Sabitle. Raporlar ve panolar hakkında daha fazla bilgi için bkz: [Power BI raporlarınız](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)ve [Power BI panoları](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="tableau"></a>Tableau Masaüstü'nü kullanın 

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
7. Varsayılan olarak, gösterilen sıcaklık alanlar toplama olarak. Bunun yerine ortalama etme göstermek istiyorsanız, açılan listeden, aşağıdaki ekran görüntüsünde gösterildiği gibi bunu yapabilirsiniz.

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

