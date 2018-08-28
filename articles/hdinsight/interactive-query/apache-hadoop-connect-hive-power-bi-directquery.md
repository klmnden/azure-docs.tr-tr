---
title: Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini Görselleştirme
description: Azure HDInsight tarafından işlenen etkileşimli sorgu Hive verilerini görselleştirmek için Microsoft Power BI'ı kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, etkileşimli sorgu, Interactive hive, LLAP, directquery
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/14/2018
ms.openlocfilehash: 1779151f3768542c08ea62d2c9109d4cd66a1e3f
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041053"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Doğrudan sorgu kullanarak Azure HDInsight Microsoft Power BI ile etkileşimli sorgu Hive verilerini Görselleştirme

Microsoft Power BI, Azure HDInsight etkileşimli sorgu kümelerine bağlanmak ve doğrudan sorgu kullanarak Hive verileri görselleştirme hakkında bilgi edinin. Bu öğreticide, verileri Power BI'a hivesampletable Hive tablosundaki yükleyin. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir dünya Haritası kullanım verileri Çiz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Yararlanabileceğiniz [Hive ODBC sürücüsünü](../hadoop/apache-hadoop-connect-hive-power-bi.md) Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için. Ancak etkileşimli Hive sorgu altyapısı göz önünde bulundurulduğunda BI iş yükleri için önerilmez. [HDInsight Interactive Query Bağlayıcısı](./apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Spark Bağlayıcısı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi Seçenekler.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight küme**. Küme bir Hive ile HDInsight kümesi ya da yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Bir kopyasından indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>HDInsight yük verileri

Hivesampletable Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

1. Power BI Desktop için oturum açın.
2. Tıklayın **giriş** sekmesinde **Veri Al** gelen **dış veri** Şerit ve ardından **daha fazla**.

    ![Açık veri HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Gelen **Veri Al** bölmesinde, türü **hdınsight** arama kutusuna. Görmüyorsanız **HDInsight etkileşimli sorgu (Beta)**, Power BI Desktop, en son sürüme güncelleştirmeniz gerekir.
4. Tıklayın **HDInsight etkileşimli sorgu (Beta)** ve ardından **Connect**.
5. Tıklayın **devam** kapatmak için **bağlayıcıyı Önizleme** Uyarısı iletişim kutusu.
6. Gelen **HDInsight etkileşimli sorgu**seçin veya aşağıdaki bilgileri girin:

    - **Sunucu**: etkileşimli sorgu kümesi adını girin, örneğin *myiqcluster.azurehdinsight.net*.
    - **Veritabanı**: Bu öğretici için girin **varsayılan**.
    - **Veri bağlantısı modu**: Bu öğretici için seçin **DirectQuery**.

    ![HDInsight etkileşimli sorgu power BI directquery bağlanma](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. **Tamam** düğmesine tıklayın.
8. HTTP kullanıcı kimlik bilgilerini girin ve ardından **Tamam**.  Varsayılan kullanıcı adı **yönetici**
9. Sol bölmeden **hivesampletale**ve ardından **yük**.

    ![HDInsight etkileşimli sorgu power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Verileri Görselleştirme

Son yordama devam edin.

1. Görsel öğeler bölmesinde seçin **harita**.  Dünya simgesi var.

    ![HDInsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. Alanlar bölmesinden seçin **Ülke** ve **devicemake**. Harita üzerinde çizilen verileri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Power BI'ı kullanarak HDInsight verilerini görselleştirmek öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power BI'ın ODBC kullanarak Azure HDInsight ile Hive verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Azure HDInsight Hive sorguları çalıştırmak için Zeppelin'i kullanma](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Power Query kullanarak Excel'i Hadoop'a bağlama](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).
