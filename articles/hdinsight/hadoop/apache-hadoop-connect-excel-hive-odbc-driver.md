---
title: Apache Hadoop - Azure HDInsight Hive ODBC sürücüsü ile Excel'i bağlama
description: Ayarlama ve Microsoft Hive ODBC sürücüsü kullanma Microsoft Excel'den Excel için HDInsight kümelerinde veri öğrenin.
keywords: hadoop, excel, hive excel, hive odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: hrasheed
ms.openlocfilehash: 37fed781629351475b384ac843a588bdaf7b5b66
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718154"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Azure HDInsight, Apache Hadoop Excel'i Microsoft Hive ODBC sürücüsü ile bağlama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft'un büyük veri çözümü Azure HDInsight içinde dağıtılan bir Apache Hadoop kümelerini Microsoft Business Intelligence (BI) bileşenleriyle tümleşir. Bu tümleştirme özelliği Excel'i Microsoft Hive açık veritabanı bağlantısı (ODBC) sürücüsü kullanarak HDInsight Hadoop kümesinde Hive veri ambarının bağlama örneğidir.

Bir HDInsight kümesi ve Excel için Microsoft Power Query eklentisini kullanarak Excel'den diğer (HDInsight olmayan) Hadoop kümeleri gibi diğer veri kaynakları ile ilişkili verileri bağlanmak mümkündür. Yükleme ve Power Query kullanarak daha fazla bilgi için bkz: [Power Query ile HDInsight Excel'e bağlanma][hdinsight-power-query].


## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* Bir HDInsight Hadoop kümesi. Oluşturmak için bkz: [Azure HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* Office 2010 Professional Plus veya sonraki bir iş istasyonu veya Excel 2010 veya üzeri.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme
İndirme ve yükleme [Microsoft Hive ODBC sürücüsünü] [ hive-odbc-driver-download] burada kullanacağınız ODBC sürücüsü uygulama sürümüyle eşleşen sürümü.  Bu öğreticide, sürücü Office Excel için kullanılır.

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC veri kaynağı oluşturma
Aşağıdaki adımlar bir Hive ODBC veri kaynağı oluşturma işlemini gösterir.

1. Windows ' Başlat'a gidin > Windows Yönetim Araçları > ODBC veri kaynakları (32-bit)/(64-bit).  Bu açılır **ODBC Veri Kaynağı Yöneticisi** penceresi.
   
    ![Veri Kaynağı Yöneticisi OBDC](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "ODBC Veri Kaynağı Yöneticisi kullanan bir DSN'ye yapılandırın")

2. Gelen **Kullanıcı DSN** sekmesinde **Ekle** açmak için **yeni veri kaynağı oluştur** penceresi.

3. Seçin **Microsoft Hive ODBC sürücüsünü**ve ardından **son** açmak için **Microsoft Hive ODBC sürücüsü DSN Kurulum** penceresi.

4. Aşağıdaki değerleri yazın veya seçin:
   
   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Konaklarında |Girin &lt;Hdınsightkümesiadı&gt;. azurehdinsight.net. Örnek: HDIKumesi.azurehdinsight.net |
   |  Bağlantı noktası |<strong>443</strong> yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
   |  Database |Kullanım <strong>varsayılan</strong>. |
   |  Mechanism |<strong>Azure HDInsight Service</strong>’i seçin |
   |  User Name |HDInsight küme HTTP kullanıcısı kullanıcı adı girin. Varsayılan kullanıcı adı <strong>admin</strong> şeklindedir. |
   |  Parola |HDInsight küme kullanıcı parolasını girin. |

   
5. İsteğe bağlı: Seçin **Gelişmiş Seçenekler...**  
   
   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel sorgu kullanın |Seçildiğinde, TSQL HiveQL dönüştürmek ODBC sürücüsü denemez. Yalnızca %100 saf HiveQL ifadelerini başlattığınızı emin olduğunuz kullanamaz. SQL Server veya Azure SQL veritabanına bağlanırken işaretli bırakmanız gerekir. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt getirme işlemi sırasında bu parametresi ayarlama işleminde en iyi performans sağlamak için gerekebilir. |
   |  Varsayılan sütun uzunluğu dize, ikili sütun uzunluğu, ondalık sütun ölçeği |Veri türü uzunlukları ve Precision bilgisayarlar, veriler nasıl döndürülür etkileyebilir. Bunlar, kesme ve/veya duyarlık kaybı nedeniyle döndürülecek yanlış bilgi neden. |

    ![Gelişmiş Seçenekleri](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN Gelişmiş yapılandırma seçenekleri")

5. Seçin **Test** veri kaynağı test etmek için. Veri kaynağının doğru şekilde yapılandırıldığında, test sonuçlarını gösterir **başarılı!**.  

6. Seçin **Tamam** Test penceresini kapatın.  

7. Seçin **Tamam** kapatmak için **Microsoft Hive ODBC sürücüsü DSN Kurulum** penceresi.  

8. Seçin **Tamam** kapatmak için **ODBC Veri Kaynağı Yöneticisi** penceresi.  

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma
Aşağıdaki adımlar, önceki bölümde oluşturduğunuz ODBC veri kaynağı kullanarak bir Excel çalışma kitabına Hive tablosundaki verileri almak için biçimini tanımlar.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.

2. Gelen **veri** sekmesinde, gitmek **Veri Al** > **diğer kaynaklardan** > **gelen ODBC** başlatmak için **Gelen ODBC** penceresi.
   
    ![Veri Bağlantı sihirbazını açın](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "veri bağlantı sihirbazını açın")

3. Aşağı açılan listeden, son bölümde oluşturduğunuz veri kaynağı adı seçin ve ardından **Tamam**.

4. (Varsayılan ad Yöneticisi) Hadoop kullanıcı adını ve parolasını girin ve ardından **Connect** açmak için **Gezgin** penceresi.

5. Gelen **Gezgin**, gitmek **HIVE** > **varsayılan** > **hivesampletable**ve ardından seçin **Yük**. Verilerin Excel'e aktarılması birkaç dakika sürer.

    ![HDInsight Hive ODBC Gezgin](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "veri bağlantı sihirbazını açın")

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Hive ODBC sürücüsü Excel'e HDInsight hizmetten veri almak için nasıl kullanılacağını öğrendiniz. Benzer şekilde, SQL veritabanı'na HDInsight hizmetinden veri alabilirsiniz. Bir HDInsight hizmetinde Veril mümkündür. Daha fazla bilgi için bkz:

* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri görselleştirme](apache-hadoop-connect-hive-power-bi.md).
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: https://go.microsoft.com/fwlink/?LinkID=286698


