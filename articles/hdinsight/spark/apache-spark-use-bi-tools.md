---
title: 'Öğretici: Power BI kullanarak Azure HDInsight Apache Spark verileri analiz etme '
description: Apache Spark verileri görselleştirmek için Microsoft Power BI kullanım depolanan HDInsight kümeleri
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/16/2019
ms.openlocfilehash: bf70abd2b3119a97af5ad1d4c56274f8e575fd3e
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65860965"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Öğretici: HDInsight Power BI'ı kullanarak Apache Spark verileri analiz etme

Bu öğreticide, şunların nasıl kullanılacağını [Microsoft Power BI](https://powerbi.microsoft.com/) bir Apache Spark kümesinde de verileri görselleştirmek için [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Power BI kullanarak Spark verilerini görselleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Makaleyi tamamlamak [Öğreticisi: Veri yükleme ve Azure HDInsight, Apache Spark kümesinde sorguları çalıştırma](./apache-spark-load-data-run-query.md).

* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

* İsteğe bağlı: [Power BI deneme aboneliği](https://app.powerbi.com/signupredirect?pbi_source=web).

## <a name="verify-the-data"></a>Verileri doğrulama

[Jupyter not defteri](https://jupyter.org/) oluşturduğunuz [önceki öğreticide](apache-spark-load-data-run-query.md) oluşturmak için kod içeren bir `hvac` tablo. Bu tablo, tüm HDInsight Spark kümeleri, CSV dosyası dayalı `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv`. Verileri doğrulamak için aşağıdaki yordamı kullanın.

1. Jupyter not defterinden aşağıdaki kodu yapıştırın ve sonra **SHIFT + ENTER** tuşuna basın. Kod, tabloların varlığını doğrular.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Çıkış aşağıdakine benzer olacaktır:

    ![Spark’ta tabloları gösterme](./media/apache-spark-use-bi-tools/show-tables.png)

    Bu öğreticiye başlamadan önce not defterini kapattıysanız, `hvactemptable` silinir ve bu nedenle çıktıya eklenmez.  Yalnızca meta veri deposunda depolanmış Hive tablolarına (**isTemporary** sütunu altında **False** ile gösterilir) BI araçlarından erişilebilir. Bu öğreticide, oluşturduğunuz **hvac** tablosuna bağlanacaksınız.

2. Aşağıdaki kodu boş bir hücreye yapıştırın ve sonra **SHIFT + ENTER** tuşlarına basın. Kod, tablodaki verileri doğrular.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Çıkış aşağıdakine benzer olacaktır:

    ![Spark içinde hvac tablosunun satırlarını gösterme](./media/apache-spark-use-bi-tools/select-limit.png)

3. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Kaynakları serbest bırakmak için not defterini kapatın.

## <a name="visualize-the-data"></a>Verileri görselleştirme

Bu bölümde Power BI kullanarak, Spark kümesi verilerinden görselleştirme, rapor ve panolar oluşturacaksınız.

### <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma

Spark ile çalışmanın ilk adımları, Power BI Desktop’ta kümeye bağlanmak, kümeden veri yüklemek ve bu verileri temel alarak basit bir görselleştirme oluşturmaktır.

> [!NOTE]  
> Bu makalede gösterilen bağlayıcı şu anda önizleme aşamasındadır. Geri bildirimlerinizi [Power BI Topluluğu](https://community.powerbi.com/) sitesi ya da [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas) sayfasından gönderin.

1. Power BI Desktop’ı açın. Başlangıç ekranı, açılırsa kapatın.

2. Gelen **giriş** sekmesinde, gitmek **Veri Al** > **daha...** .

    ![HDInsight Apache Spark’tan Power BI Desktop’a veri alma](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Apache Spark BI’dan Power BI’ya veri alma")

3. Girin `Spark` arama kutusunda **Azure HDInsight Spark**ve ardından **Connect**.

    ![Apache Spark BI’dan Power BI’ya veri alma](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Apache Spark BI’dan Power BI’ya veri alma")

4. Küme URL'nizi girin (biçiminde `mysparkcluster.azurehdinsight.net`) içinde **sunucu** metin kutusu.

5. Altında **veri bağlantısı modu**seçin **DirectQuery**. Sonra **Tamam**’ı seçin.

    Spark ile herhangi bir veri bağlantısı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesi yenilenmeden raporlara yansıtılır. Verileri içeri aktarırsanız, değişiklikleri görmek için veri kümesini yenilemeniz gerekir. DirectQuery’nin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Power BI’da DirectQuery kullanma](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/).

6. HDInsight oturum açma hesap bilgilerini girin ve ardından **Connect**. Varsayılan hesap adı *admin*’dir.

7. Seçin `hvac` tablo, verilerin bir önizlemesini görmek için bekleyin ve ardından **yük**.

    ![Spark kümesi kullanıcı adı ve parolası](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark kümesi kullanıcı adı ve parolası")

    Power BI Desktop, Spark kümesine bağlanmak ve `hvac` tablosundan verileri yüklemek için gereken bilgilere sahiptir. Tablo ve sütunları, **Alanlar** bölmesinde gösterilir.

8. Her bina için hedef sıcaklık ile gerçek sıcaklık arasındaki farkı görselleştirin:

    1. **GÖRSELLEŞTİRMELER** bölmesinde **Alan Grafiği**’ni seçin.

    2. **BuildingID** alanını **Eksen**’e, **ActualTemp** ve **TargetTemp** alanlarını ise **Değer**’e sürükleyin.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Diyagram şuna benzer:

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Varsayılan olarak görselleştirme, **ActualTemp** ve **TargetTemp** değerlerinin toplamını gösterir. Yanındaki aşağı oku seçerek **ActualTemp** ve **TragetTemp** görsel öğeler bölmesinde gördüğünüz **toplam** seçilir.

    3. Aşağı okları seçin **ActualTemp** ve **TragetTemp** görsel öğeler bölmesinde **ortalama** ortalama gerçek ve hedef Sıcaklıkların her biri için almak için Yapı.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Veri görselleştirmeniz, ekran görüntüsünde gösterilene benzer olmalıdır. İlgili verilere ilişkin araç ipuçları almak üzere imlecinizi görselleştirmenin üzerine getirin.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

9. Gidin **dosya** > **Kaydet**, bir ad girin `BuildingTemperature` dosya için seçip **Kaydet**.

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Raporu Power BI Hizmetinde yayımlama (isteğe bağlı)

Power BI hizmeti, raporları ve panoları kuruluşunuzda paylaşmanıza olanak tanır. Bu bölümde, ilk olarak veri kümesini ve raporu yayımlayacaksınız. Ardından, raporu bir panoya sabitleyeceksiniz. Panolar genellikle bir rapordaki veri alt kümesine odaklanmak için kullanılır. Raporunuzda yalnızca bir görselleştirme olmasına rağmen adımların üzerinden geçmek için yeterlidir.

1. Power BI Desktop’ı açın.
2. **Giriş** sekmesinden **Yayımla**’ya tıklayın.

    ![Power BI Desktop’tan yayımlama](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Power BI Desktop’tan yayımlama")

2. Veri kümenizin ve raporunuzun yayımlanacağı çalışma alanını seçin ve **Seç**’e tıklayın. Aşağıdaki görüntüde varsayılan **Çalışma Alanım** seçilidir.

    ![Veri kümesi ve rapor yayımlamak için çalışma alanı seçme](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Veri kümesi ve rapor yayımlamak için çalışma alanı seçme") 

3. Yayımlama başarılı olduktan sonra **'BuildingTemperature.pbix' dosyasını Power BI’da aç** öğesine tıklayın.

    ![Yayımlama başarılı, kimlik bilgilerinizi girmek için tıklayın](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Yayımlama başarılı, kimlik bilgilerinizi girmek için tıklayın") 

4. Power BI hizmetinde **Kimlik bilgilerini girin**’e tıklayın.

    ![Power BI hizmetinde kimlik bilgilerini girin](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI hizmetinde kimlik bilgilerini girin")

5. **Kimlik bilgilerini düzenle**’ye tıklayın.

    ![Power BI hizmetinde kimlik bilgilerini düzenle](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Power BI hizmetinde kimlik bilgilerini düzenle")

6. HDInsight oturum açma hesabı bilgilerini girin, ardından **Oturum Aç**’a tıklayın. Varsayılan hesap adı *admin*’dir.

    ![Spark kümesinde oturum açma](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Spark kümesinde oturum açma")

7. Sol bölmede, **Çalışma Alanları** > **Çalışma Alanım** > **RAPORLAR**’a gidin, ardından **BinaSıcaklığı**’na tıklayın.

    ![Sol bölmede raporlar altında listelenen rapor](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Sol bölmede raporlar altında listelenen rapor")

    **BinaSıcaklığı** değerinin sol bölmedeki **VERİ KÜMELERİ** altında da listelendiğini göreceksiniz.

    Power BI Desktop’ta oluşturduğunuz görsel artık Power BI hizmetinde kullanılabilir. 

8. İmlecinizi görselleştirmenin üzerine getirin ve ardından sağ üst köşedeki iğne simgesine tıklayın.

    ![Power BI hizmetinde rapor](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Power BI hizmetinde rapor")

9. "Yeni pano" öğesini seçin, `Building temperature` adını girin, ardından **Sabitle**’ye tıklayın.

    ![Yeni panoya sabitleme](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Yeni panoya sabitleme")

10. Raporda **Panoya git**’e tıklayın. 

Görseliniz panoya sabitlenir. Rapora başka görseller ekleyebilir ve bu görselleri aynı panoya sabitleyebilirsiniz. Raporlar ve panolar hakkında daha fazla bilgi için bkz: [Power BI raporlarında](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) ve [Power bı'da panolar](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici sayesinde nasıl kullanacağınızı öğrendiniz [Microsoft Power BI](https://powerbi.microsoft.com/) bir Apache Spark kümesinde de verileri görselleştirmek için [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/). Spark’a kaydettiğiniz verilerin Power BI gibi bir BI analiz aracına nasıl çekilebileceğini görmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Bir Apache Spark akış işi çalıştırma](apache-spark-eventhub-streaming.md)