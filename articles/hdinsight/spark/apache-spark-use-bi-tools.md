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
ms.date: 10/24/2017
ms.author: nitinme
ms.openlocfilehash: 3886923639be8a7bd8167f10db503d7ebf8c1657
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark Azure Hdınsight ile verileri görselleştirme araçlarını kullanarak BI

Azure hdınsight'ta Apache Spark kümesinde verileri görselleştirmek için Power BI ve Tableau kullanmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).
* Örnek veri kümesi. Yönergeler için bkz: [bir Hdınsight Spark kümesinde etkileşimli sorgular gerçekleştirme](apache-spark-load-data-run-query.md).
* Power BI: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) ve [Power BI deneme aboneliği](https://app.powerbi.com/signupredirect?pbi_source=web) (isteğe bağlı).
* Tableau: [Tableau Masaüstü](http://www.tableau.com/products/desktop) ve [Microsoft Spark ODBC sürücüsü](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Örnek verileri gözden geçirin

Oluşturduğunuz Jupyter not defteri [önceki öğretici](apache-spark-load-data-run-query.md) oluşturmak için kodu içeren bir `hvac` tablo. Bu tablo, tüm Hdınsight Spark kümeleri, CSV dosyası dayalı **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Şimdi Spark kümesinde veri görselleştirmeleri oluşturmadan önce gözden geçirin.

1. Beklenen tablolar var olduğunu doğrulayın. Not defterindeki boş hücreye aşağıdaki kod parçacığında ve tuşuna kopyalama **SHIFT + ENTER**.

        %%sql
        SHOW TABLES

    Aşağıda gösterilene benzer bir çıktı görürsünüz:

    ![Tabloları Spark Göster](./media/apache-spark-use-bi-tools/show-tables.png)

    Bu öğreticiye başlamadan önce dizüstü bilgisayar kapalı `hvactemptable` , çıktıda dahil edilmeyen şekilde temizlendi.
    Yalnızca meta depo içinde depolanan tabloları hive (belirttiği **False** altında **isTemporary** sütun) BI Araçları'ndan erişilebilir. Bu öğreticide, biz bağlanmak **hvac** oluşturduğumuz tablo.

2. Tablo beklenen verileri içerdiğini doğrulayın. Not defterindeki boş hücreye aşağıdaki kod parçacığında ve tuşuna kopyalama **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

    Aşağıda gösterilene benzer bir çıktı görürsünüz:

    ![Spark hvac tablodaki Göster](./media/apache-spark-use-bi-tools/select-limit.png)

3. Kaynakları serbest bırakmak için Not Defteri kapatın. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın.

## <a name="powerbi"></a>Power BI için Spark veri görselleştirme kullanın

Beklenen verilerin var olduğunu doğruladıktan, Power BI görselleştirmeleri, raporlar ve panolar bu verileri oluşturmak için kullanabilirsiniz. Bu makalede, biz statik verilerle basit bir örnek gösterir, ancak daha karmaşık akış örnekler görmek istiyorsanız lütfen açıklamaları bilgilendirin.

### <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
Power BI Desktop'ta kümeye bağlanmak, kümeden veri yüklemek ve verilere dayanan bir temel görselleştirme oluşturmak için Spark ile çalışırken ilk adım değildir.

> [!NOTE]
> Bu makalede gösterilen bağlayıcı şu anda önizlemede değil, ancak bunu kullanın ve elinizde aracılığıyla geribildirim sağlamak için öneririz [Power BI topluluk](https://community.powerbi.com/) site veya [Power BI fikirleri](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Power BI Desktop'ta üzerinde **giriş** sekmesini tıklatın, **Veri Al**, ardından **daha fazla**.

2. Arama `Spark`seçin **Azure Hdınsight Spark**, ardından **Bağlan**.

    ![Apache Spark BI'dan Power BI Veri Al](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "veri alma Power BI'a Apache Spark BI'dan")

3. Küme URL'nizi girin (biçiminde `mysparkcluster.azurehdinsight.net`), select **DirectQuery**, ardından **Tamam**.

    ![Apache Spark BI bağlanmak](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Apache Spark BI Bağlan")

    > [!NOTE]
    > Spark ile ya da bağlantı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesinin yenileme olmadan raporlarında yansıtılır. Veri içe aktarırsanız, değişiklikleri görmek için bu veri kümesi yenilemeniz gerekir. Hakkında daha fazla bilgi ve DirectQuery kullanıldığı durumlar için bkz: [kullanarak DirectQuery Power bı'da](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Hdınsight oturum açma hesabı bilgilerini girin **kullanıcı adı** ve **parola** (varsayılan hesaptır `admin`), ardından **Bağlan**.

    ![Spark küme kullanıcı adı ve parola](./media/apache-spark-use-bi-tools/user-password.png "Spark küme kullanıcı adı ve parola")

5. Seçin `hvac` tablo ve verilerin önizlemesini görmek için bekleyin. Ardından **yük**.

    ![Spark küme kullanıcı adı ve parola](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark küme kullanıcı adı ve parola")

    Power BI Desktop şimdi Spark küme ve yük verileri bağlanmak için gerekli tüm bilgilere sahip `hvac` tablo. Tablo ve sütunlarını görüntülenen **alanları** bölmesi.

    ![Apache Spark BI Panoda Tabloları Listele](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Apache Spark BI Panoda Tabloları Listele")

7. Hedef sıcaklık ve her derleme için gerçek sıcaklık arasındaki fark göstermek için bir görsel öğe oluşturun: 

    1. İçinde **GÖRSELLEŞTİRMELERİ** bölmesinde, **alan grafiği**. Sürükleme **BuildingID** alanı **eksen**, sürükleyin **ActualTemp** ve **TargetTemp** alanları **değeri**.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

    2. Varsayılan olarak görselleştirme için toplam gösterir **ActualTemp** ve **TargetTemp**. Alanlardan açılan her ikisini de seçin **ortalama** gerçek ortalama ve hedef etme için her bir yapı almak için.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

    3. Veri görselleştirme benzer ekran görüntüsü olması gerekir. Araç ipuçları ile ilgili verileri almak için görselleştirme üzerinden imlecinizi taşıyın.

        ![Apache Spark BI kullanarak veri görselleştirmeleri Spark oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "oluşturma Spark veri görselleştirmeleri Apache Spark BI kullanma")

11. Tıklatın **dosya** sonra **kaydetmek**ve bir ad girin `spark.pbix` dosyası için. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Rapor Power BI hizmetine (isteğe bağlı) yayımlayın
Power BI Desktop'ta artık tam olarak işlevsel bir rapora sahipsiniz ve orada durdurabilirsiniz, ancak çoğu kişi, raporlar ve panolar, kuruluşunuz genelindeki paylaşmak kolaylaştırır Power BI hizmetinde yararlanmak istiyor. 

Bu bölümde, veri kümesi ve oluşturduğunuz Power BI Desktop dosyasında yer alan rapor yayımlayın. Ardından, raporundan görselleştirme bir Panoda de sabitleyin. Panolar, genellikle bir rapordaki verilerin bir alt kümesini odaklanmak için kullanılır; Raporunuzda yalnızca bir görselleştirme sahip ancak adımları gitmek hala faydalıdır.

1. Power BI Desktop'ta üzerinde **giriş** sekmesini tıklatın, **Yayımla**.

    ![Power BI masaüstünden yayımlama](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Power BI masaüstünden yayımlama")

2. Veri kümenizi yayımlamak için bir çalışma alanı seçin ve ardından rapor **seçin**. Aşağıdaki görüntüde, varsayılan **çalışma Alanım** seçilir.

    ![Veri kümesini yayımlamak ve rapor için çalışma alanı seçin](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "veri kümesini yayımlamak ve rapor için Select çalışma") 

3. Power BI Desktop ile bir başarı iletisi döndükten sonra tıklatın **'spark.pbix' Power BI'da Aç**.

    ![Başarı yayımlama, kimlik bilgilerini girmek için tıklatın](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "başarı yayımlama, kimlik bilgilerini girmek için tıklatın") 

4. Power BI hizmetinde tıklatın **kimlik bilgilerini girin**.

    ![Power BI hizmetinde kimlik bilgilerini girin](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI hizmetinde kimlik bilgilerini girin")

5. Tıklatın **kimlik bilgilerini Düzenle**.

    ![Power BI hizmetinde kimlik bilgilerini Düzenle](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Power BI hizmetinde kimlik bilgilerini Düzenle")

6. Hdınsight oturum açma hesabı bilgilerini girin (genellikle varsayılan `admin` Power BI Desktop'ta kullanılan hesabı), ardından **oturum**.

    ![Spark kümesi oturum](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Spark küme için oturum açın")

7. Sol bölmede, Git **çalışma alanları** > **çalışma Alanım** > **RAPORLARI**, ardından **spark**.

    ![Raporu rapor sol bölmede altında listelenen](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "raporları sol bölmede altında listelenen raporu")

    Ayrıca görmelisiniz **spark** altında listelenen **veri KÜMELERİ** sol bölmede.

8. Power BI Desktop'ta oluşturulan visual hizmetinde kullanıma sunulmuştur. Bu görsel bir panoya sabitlemek için görsel getirin ve sabitleme simgesine tıklayın.

    ![Power BI hizmetinde raporda](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Power BI hizmetindeki raporu")

9. "Yeni Pano" seçin, bir ad girin `SparkDemo`, ardından **PIN**.

    ![Yeni panoya Sabitle](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "yeni panoya Sabitle")

10. Rapora tıklayın **panoya gitmek**. 

    ![Pano gidin](./media/apache-spark-use-bi-tools/apache-spark-bi-open-dashboard.png "panoya gidin")

Visual panosuna sabitlediğiniz - diğer görsellerin rapora ekleyin ve bunları aynı panoya Sabitle. Raporlar ve panolar hakkında daha fazla bilgi için bkz: [Power BI raporlarınız](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)ve [Power BI panoları](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

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

