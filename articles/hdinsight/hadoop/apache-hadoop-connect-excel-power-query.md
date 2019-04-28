---
title: Excel'i Power Query - Azure HDInsight ile Apache hadoop'a bağlama
description: İş Zekası bileşenleri avantajlarından yararlanın ve HDInsight üzerinde Hadoop depolanan verilere erişmek için Excel için Power Query kullanın hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: df7bb39120dfe4c45a4749065f77649bc51d0356
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122158"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Excel'i Power Query kullanarak Apache Hadoop'a bağlama
Temel özelliklerinden biri, Microsoft büyük veri çözümü, Azure HDInsight, Apache Hadoop kümelerini Microsoft iş zekası (BI) bileşenleriyle tümleştirmedir. Excel için Excel eklentisi, Microsoft Power Query kullanarak Hadoop kümenizle ilişkili verileri içeren Azure depolama hesabına bağlanma olanağı buna birincil bir örnektir. Bu makalede, ayarlama ve HDInsight ile yönetilen Hadoop kümesi ile ilişkili verileri sorgulamak için Power Query kullanma konusunda yol göstermektedir.

### <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir HDInsight kümesi**. Bir yapılandırmak için bkz. [Azure HDInsight ile çalışmaya başlama] [hdınsight-get-started].
* **Bir iş istasyonu** Windows 7, Windows Server 2008 R2 veya sonraki bir işletim sistemini çalıştıran.
* **Office 2016, Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 tek başına veya Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Power Query yükleme
Power Query, çıkış veya, bir HDInsight kümesi üzerinde çalışan bir Hadoop işi tarafından üretilmiş olan verileri içeri aktarabilirsiniz.

Excel 2016'daki Al ve Dönüştür bölümünde veri Şerit halinde Power Query tümleştirilmiştir. Excel için Microsoft Power Query eski Excel sürümleri için indirme [Microsoft Download Center] [ powerquery-download] ve yükleyin.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight verileri Excel'e aktarmak
Excel için Power Query eklentisini Excel'e burada BI araçları gibi incelemek için PowerPivot ve Power Map kullanılabilir analiz edin ve verileri sunmak HDInsight kümenizden veri almak kolaylaştırır.

**Bir HDInsight kümesinden verileri içeri aktarmak için**

1. Excel'i açın.
2. Yeni bir boş çalışma kitabı oluşturun.
3. Excel sürüme göre aşağıdaki adımları gerçekleştirin:

   - Excel 2016

     - Tıklayın **veri** menüsünde tıklayın **Veri Al** gelen **Al ve Dönüştür** Şerit'a tıklayın **Azure**ve ardından**Azure HDInsight(HDFS) gelen**.

       ![HDI.PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

   - Excel 2013/2010

     - Tıklayın **Power Query** menüsünde tıklatın **Azure**ve ardından **gelen Microsoft Azure HDInsight**.
   
       ![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
       **Not:** Görmüyorsanız **Power Query** menü, Git **dosya** > **seçenekleri** > **eklentileri**, seçin **COM eklentileri** açılır listeden **Yönet** sayfanın alt kısmındaki kutusu. Seçin **Git...**  düğmesine tıklayın ve Excel eklentisi için Power Query onay kutusunun seçili olduğunu doğrulayın.
       
       **Not:** Power Query de sayesinde tıklayarak verileri HDFS içeri **diğer kaynaklardan**.
4. İçin **hesap adı**kümenizle ilişkili Azure Blob Depolama hesabı adını girin ve ardından **Tamam**. Bu hesap, varsayılan depolama hesabını ya da bağlantılı bir depolama hesabı olabilir.  Biçim *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. İçin **hesap anahtarı**, Blob Depolama hesabı anahtarını girin ve ardından **Kaydet**. (Bu deposuna erişim hesabı bilgilerini yalnızca uygulamayı ilk zaman girmeniz gerekir.)
6. İçinde **Gezgin** sorgu Düzenleyicisi'nin, sol bölmede, Blob Depolama kapsayıcısı adı'na çift tıklayın. Varsayılan olarak, kapsayıcı adını küme adıyla aynı addır.
7. Bulun **HiveSampleData.txt** içinde **adı** sütun (klasör yolu **../hive/warehouse/hivesampletable/ambar**) ve ardından **ikili** sol tarafındaki HiveSampleData.txt. HiveSampleData.txt kümeyle birlikte gelir. İsteğe bağlı olarak, kendi dosyanızı kullanabilirsiniz.
   
    ![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. İsterseniz, sütun adlarını yeniden adlandırabilirsiniz. Hazır olduğunuzda tıklayın **Kapat ve Yükle**.  Çalışma kitabınızda veri yüklendi:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Power Query veri HDInsight Excel'e almak için nasıl kullanılacağını öğrendiniz. Benzer şekilde, Azure SQL veritabanı'na HDInsight veri alabilirsiniz. HDInsight ile verileri yüklemek mümkündür. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri görselleştirme](apache-hadoop-connect-hive-power-bi.md).
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).

[image-hdi-powerquery-hdi-source]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: https://go.microsoft.com/fwlink/?LinkID=286689
