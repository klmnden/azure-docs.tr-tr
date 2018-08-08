---
title: Excel - Azure HDInsight Hive ODBC sürücüsü ile Hadoop'a bağlama
description: Ayarlama ve Microsoft Hive ODBC sürücüsü kullanma Microsoft Excel'den Excel için HDInsight kümelerinde veri öğrenin.
keywords: hadoop, excel, hive excel, hive odbc
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: 4153504e7d0fb6dff4b8a675b301f54fb3588e46
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590876"
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Azure HDInsight hadoop Excel'i Microsoft Hive ODBC sürücüsü ile bağlama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft'un büyük veri çözümü Azure HDInsight tarafından dağıtılan bir Apache Hadoop kümelerini Microsoft Business Intelligence (BI) bileşenleriyle tümleşir. Bu tümleştirme özelliği Excel'i Microsoft Hive açık veritabanı bağlantısı (ODBC) sürücüsü kullanarak HDInsight Hadoop kümesinde Hive veri ambarının bağlama örneğidir.

Bir HDInsight kümesi ve Excel için Microsoft Power Query eklentisini kullanarak Excel'den diğer (HDInsight olmayan) Hadoop kümeleri gibi diğer veri kaynakları ile ilişkili verileri bağlanmak mümkündür. Yükleme ve Power Query kullanarak daha fazla bilgi için bkz: [Power Query ile HDInsight Excel'e bağlanma][hdinsight-power-query].



**Önkoşullar**:

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir HDInsight kümesi**. Oluşturmak için bkz: [Azure HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* **Bir iş istasyonu** Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 tek başına veya Office 2010 Professional Plus ile.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme
Microsoft Hive ODBC sürücüsünü yükleyip [İndirme Merkezi][hive-odbc-driver-download].

Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 ve Windows Server 2012, 32 bit veya 64 bit sürümlerinde bu sürücüye yüklenebilir. Sürücü, Azure HDInsight için bağlantı sağlar. ODBC sürücüsü kullandığınız uygulama sürümüyle eşleşen sürümünü yüklemeniz. Bu öğreticide, sürücü Office Excel'den kullanılır.

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma
Aşağıdaki adımlar bir Hive ODBC veri kaynağı oluşturma işlemini gösterir.

1. Windows 8 veya Windows 10, başlangıç ekranını açmak için Windows tuşuna basın ve ardından yazın **veri kaynakları**.
2. Tıklayın **ODBC veri kaynakları (32-bit) ayarlamanız** veya **ODBC veri kaynakları (64-bit) ayarlamanız** , Office sürümüne bağlı olarak. Windows 7 kullanıyorsanız seçin **ODBC veri kaynakları (32 bit)** veya **ODBC veri kaynakları (64-bit)** gelen **Yönetimsel Araçlar**. Göreceksiniz **ODBC Veri Kaynağı Yöneticisi** iletişim.
   
    ![Veri Kaynağı Yöneticisi OBDC](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "ODBC Veri Kaynağı Yöneticisi kullanan bir DSN'ye yapılandırın")

3. Kullanıcı DNS'den tıklayın **Ekle** açmak için **yeni veri kaynağı oluştur** Sihirbazı.
4. Seçin **Microsoft Hive ODBC sürücüsünü**ve ardından **son**. Göreceksiniz **Microsoft Hive ODBC sürücüsü DNS Kurulumu** iletişim.
5. Aşağıdaki değerleri yazın veya seçin:
   
   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Host |&lt;HDInsightKümesiAdı>.azurehdinsight.net yazın. Örnek: HDIKumesi.azurehdinsight.net |
   |  Bağlantı noktası |<strong>443</strong> yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
   |  Database |<strong>Default</strong>’u kullanın. |
   |  Mechanism |<strong>Azure HDInsight Service</strong>’i seçin |
   |  User Name |HDInsight küme HTTP kullanıcısı kullanıcı adı girin. Varsayılan kullanıcı adı <strong>admin</strong> şeklindedir. |
   |  Parola |HDInsight küme kullanıcı parolasını girin. |
   
    </table>
   
    Bazı önemli parametre tıkladığınızda dikkat etmeniz **Gelişmiş Seçenekler**:
   
   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel sorgu kullanın |Seçildiğinde, TSQL HiveQL dönüştürmek ODBC sürücüsü denemez. Yalnızca %100 saf HiveQL ifadelerini başlattığınızı emin olduğunuz kullanamaz. SQL Server veya Azure SQL veritabanına bağlanırken işaretli bırakmanız gerekir. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt getirme işlemi sırasında bu parametresi ayarlama işleminde en iyi performans sağlamak için gerekebilir. |
   |  Varsayılan sütun uzunluğu dize, ikili sütun uzunluğu, ondalık sütun ölçeği |Veri türü uzunlukları ve Precision bilgisayarlar, veriler nasıl döndürülür etkileyebilir. Bunlar, kesme ve/veya duyarlık kaybı nedeniyle döndürülecek yanlış bilgi neden. |

    ![Gelişmiş Seçenekleri](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN Gelişmiş yapılandırma seçenekleri")

1. Tıklayın **Test** veri kaynağı test etmek için. Veri kaynağının doğru şekilde yapılandırıldığında, bu gösterir *TESTLERİ başarıyla tamamlandı!*.
2. Tıklayın **Tamam** Test iletişim kutusunu kapatmak için. Yeni veri kaynağı üzerinde listelenen **ODBC Veri Kaynağı Yöneticisi**.
3. Tıklayın **Tamam** sihirbazdan çıkmak için.

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma
Aşağıdaki adımlar, önceki bölümde oluşturduğunuz ODBC veri kaynağı kullanarak bir Excel çalışma kitabına Hive tablosundaki verileri almak için biçimini tanımlar.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.
2. Gelen **veri** sekmesinde **Veri Al**, tıklayın **diğer kaynaklardan**ve ardından **gelen ODBC** başlatmak için **veri Bağlantı Sihirbazı**.
   
    ![Veri Bağlantı sihirbazını açın](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "veri bağlantı sihirbazını açın")
4. Son bölümde oluşturduğunuz veri kaynağı adı seçin ve ardından **Tamam**.
5. (Varsayılan ad Yöneticisi) Hadoop kullanıcı adını ve parolasını girin ve ardından **Connect**.
6. Gezgin üzerinde genişletin **HIVE**, genişletme **varsayılan**, tıklayın **hivesampletable**ve ardından **yük**. Verilerin Excel’e aktarılması birkaç saniye sürer.

    ![HDInsight Hive ODBC Gezgin](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "veri bağlantı sihirbazını açın")


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Hive ODBC sürücüsü Excel'e HDInsight hizmetten veri almak için nasıl kullanılacağını öğrendiniz. Benzer şekilde, SQL veritabanı'na HDInsight hizmetinden veri alabilirsiniz. Bir HDInsight hizmetinde Veril mümkündür. Daha fazla bilgi için bkz:

* [Microsoft Power BI'da Azure HDInsight ile Hive verileri görselleştirme](apache-hadoop-connect-hive-power-bi.md).
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure HDInsight Hive sorguları çalıştırmak için Zeppelin'i kullanma ](./../hdinsight-connect-hive-zeppelin.md).
* [Power Query kullanarak Excel'i Hadoop'a bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


