---
title: 'Öğretici: Power BI kullanarak Azure HDInsight Apache Spark verileri analiz etme '
description: Apache Spark verileri görselleştirmek için Microsoft Power BI kullanım depolanan HDInsight kümeleri
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: ed7beeadc0a550a28d1f936702aabeb45823b677
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127217"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Öğretici: HDInsight Power BI'ı kullanarak Apache Spark verileri analiz etme 

Nasıl kullanacağınızı öğrenin [Microsoft Power BI](https://powerbi.microsoft.com/) verileri görselleştirmek için bir [Apache Spark](https://spark.apache.org/) içinde küme [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Power BI kullanarak Spark verilerini görselleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **Makaleyi tamamlamak [Öğreticisi: Veri yükleme ve Azure HDInsight, Apache Spark kümesinde sorguları çalıştırma](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) ve [Power BI deneme aboneliği](https://app.powerbi.com/signupredirect?pbi_source=web) (isteğe bağlı).


## <a name="verify-the-data"></a>Verileri doğrulama

[Jupyter not defteri](https://jupyter.org/) oluşturduğunuz [önceki öğreticide](apache-spark-load-data-run-query.md) oluşturmak için kod içeren bir `hvac` tablo. Bu tablo, **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv** dizinindeki tüm HDInsight Spark kümelerinde bulunan CSV dosyasını temel alır. Verileri doğrulamak için aşağıdaki yordamı kullanın.

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

3. Not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Kaynakları serbest bırakmak için not defterini kapatın. 

## <a name="visualize-the-data"></a>Verileri görselleştirme

Bu bölümde Power BI kullanarak, Spark kümesi verilerinden görselleştirme, rapor ve panolar oluşturacaksınız. 

### <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
Spark ile çalışmanın ilk adımları, Power BI Desktop’ta kümeye bağlanmak, kümeden veri yüklemek ve bu verileri temel alarak basit bir görselleştirme oluşturmaktır.

> [!NOTE]  
> Bu makalede gösterilen bağlayıcı şu anda önizleme aşamasındadır. Geri bildirimlerinizi [Power BI Topluluğu](https://community.powerbi.com/) sitesi ya da [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas) sayfasından gönderin.

1. [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)’ı açın.
1. **Giriş** sekmesinde **Veri Al** ve sonra **Diğer**’e tıklayın.

    ![HDInsight Apache Spark’tan Power BI Desktop’a veri alma](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Apache Spark BI’dan Power BI’ya veri alma")


2. Girin `Spark` arama kutusunda **Azure HDInsight Spark**ve ardından **Connect**.

    ![Apache Spark BI’dan Power BI’ya veri alma](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Apache Spark BI’dan Power BI’ya veri alma")

3. Küme URL’nizi girin (`mysparkcluster.azurehdinsight.net` biçiminde), **DirectQuery** öğesini seçin ve sonra, **Tamam**’a tıklayın.

    Spark ile herhangi bir veri bağlantısı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesi yenilenmeden raporlara yansıtılır. Verileri içeri aktarırsanız, değişiklikleri görmek için veri kümesini yenilemeniz gerekir. DirectQuery’nin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Power BI’da DirectQuery kullanma](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. HDInsight oturum açma hesabı bilgilerini girin, ardından **Bağlan**’a tıklayın. Varsayılan hesap adı *admin*’dir.

5. `hvac` tablosunu seçin, verilerin önizlemesini görmeyi bekleyin ve sonra **Yükle**’ye tıklayın.

    ![Spark kümesi kullanıcı adı ve parolası](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark kümesi kullanıcı adı ve parolası")

    Power BI Desktop, Spark kümesine bağlanmak ve `hvac` tablosundan verileri yüklemek için gereken bilgilere sahiptir. Tablo ve sütunları, **Alanlar** bölmesinde gösterilir.  Aşağıdaki ekran görüntüsüne bakın:

6. Her bina için hedef sıcaklık ile gerçek sıcaklık arasındaki farkı görselleştirin: 

    1. **GÖRSELLEŞTİRMELER** bölmesinde **Alan Grafiği**’ni seçin. 
    2. **BuildingID** alanını **Eksen**’e, **ActualTemp** ve **TargetTemp** alanlarını ise **Değer**’e sürükleyin.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Diyagram şuna benzer:

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Varsayılan olarak görselleştirme, **ActualTemp** ve **TargetTemp** değerlerinin toplamını gösterir. Görselleştirmeler bölmesindeki **ActualTemp** ve **TragetTemp** değerlerinin yanında bulunan aşağı oka tıkladığınızda **Toplam** seçeneğinin işaretli olduğunu görebilirsiniz.

    3. Görselleştirmeler bölmesindeki **ActualTemp** ve **TragetTemp** değerlerinin yanında bulunan aşağı oklara tıklayın ve her bina için gerçek ve hedef sıcaklıkların ortalamasını almak üzere **Ortalama**’yı seçin.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

        Veri görselleştirmeniz, ekran görüntüsünde gösterilene benzer olmalıdır. İlgili verilere ilişkin araç ipuçları almak üzere imlecinizi görselleştirmenin üzerine getirin.

        ![Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Apache Spark BI kullanarak Spark veri görselleştirmeleri oluşturma")

7. **Dosya** ve ardından **Kaydet**’e tıklayın ve dosya için `BuildingTemperature.pbix` adını girin. 

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

<!--
## <a name="tableau"></a>Use Tableau Desktop 

> [!NOTE]
> This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.
>
>

1. Install [Tableau Desktop](https://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.

2. Make sure that computer also has Microsoft Spark ODBC driver installed. You can install the driver from [here](https://go.microsoft.com/fwlink/?LinkId=616229).

1. Launch Tableau Desktop. In the left pane, from the list of server to connect to, click **Spark SQL**. If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.
2. In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.

    ![Connect to a cluster for Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")

    The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](https://go.microsoft.com/fwlink/?LinkId=616229) on the computer.
3. On the next screen, from the **Schema** drop-down, click the **Find** icon, and then click **default**.

    ![Find schema for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")
4. For the **Table** field, click the **Find** icon again to list all the Hive tables available in the cluster. You should see the **hvac** table you created earlier using the notebook.

    ![Find table for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")
5. Drag and drop the table to the top box on the right. Tableau imports the data and displays the schema as highlighted by the red box.

    ![Add tables to Tableau for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables to Tableau for Apache Spark BI")
6. Click the **Sheet1** tab at the bottom left. Make a visualization that shows the average target and actual temperatures for all buildings for each date. Drag **Date** and **Building ID** to **Columns** and **Actual Temp**/**Target Temp** to **Rows**. Under **Marks**, select **Area** to use an area map for Spark data visualization.

     ![Add fields for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")
7. By default, the temperature fields are shown as aggregate. If you want to show the average temperatures instead, you can do so from the drop-down, as shown in the following screenshot:

    ![Take average of temperature for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")

8. You can also super-impose one temperature map over the other to get a better feel of difference between target and actual temperatures. Move the mouse to the corner of the lower area map until you see the handle shape highlighted in a red circle. Drag the map to the other map on the top and release the mouse when you see the shape highlighted in red rectangle.

    ![Merge maps for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")

     Your data visualization should change as shown in the screenshot:

    ![Tableau output for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")
9. Click **Save** to save the worksheet. You can create dashboards and add one or more sheets to it.
-->

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

- Apache Spark verileri Power bı'da görselleştirin.

Spark’a kaydettiğiniz verilerin Power BI gibi bir BI analiz aracına nasıl çekilebileceğini görmek için sonraki makaleye ilerleyin. 
> [!div class="nextstepaction"]
> [Bir Apache Spark akış işi çalıştırma](apache-spark-eventhub-streaming.md)

