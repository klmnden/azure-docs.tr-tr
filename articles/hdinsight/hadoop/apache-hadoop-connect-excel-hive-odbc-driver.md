---
title: Excel - Azure Hdınsight Hive ODBC sürücüsü ile Hadoop'a bağlama | Microsoft Docs
description: Ayarlama ve Microsoft Hive ODBC sürücüsü Excel için sorgu verilere Hdınsight kümelerinde Microsoft Excel'den nasıl kullanılacağını öğrenin.
keywords: hadoop excel, hive excel, hive odbc
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jgao
ms.openlocfilehash: 26234ca17d833fef01ad5a6465824c99d84cc556
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Excel'i Microsoft Hive ODBC sürücüsü ile Azure Hdınsight hadoop bağlama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft'un büyük veri çözüm Microsoft Business Intelligence (BI) bileşenleri Azure Hdınsight tarafından dağıtılan Apache Hadoop kümeleri ile tümleşir. Excel'i Microsoft Hive açık veritabanı bağlantısı (ODBC) sürücüsü kullanarak hdınsight'ta Hadoop kümesindeki Hive veri ambarına bağlama yeteneği Bu tümleştirme örnektir.

Hdınsight kümesi ve Excel için Microsoft Power Query eklentisini kullanarak Excel'den diğer (Hdınsight olmayan) Hadoop kümeleri dahil olmak üzere diğer veri kaynakları ile ilişkili verileri bağlanmak mümkündür. Yükleme ve Power Query kullanma hakkında daha fazla bilgi için bkz: [Hdınsight Power Query ile bağlanma Excel'e][hdinsight-power-query].



**Önkoşullar**:

Bu makaleye başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight kümesi**. Oluşturmak için bkz: [Azure Hdınsight kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* **Bir iş istasyonu** Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 tek başına veya Office 2010 Professional Plus ile.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme
Microsoft Hive ODBC sürücüsünü yükleyip [Yükleme Merkezi'nden][hive-odbc-driver-download].

Bu sürücü Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 ve Windows Server 2012, 32 bit veya 64 bit sürümlerinde yüklenebilir. Sürücüyü Azure Hdınsight bağlantı sağlar. ODBC sürücüsü kullandığınız uygulama sürümüyle eşleşen sürümünü yükleyin. Bu öğretici için sürücü Office Excel'den kullanılır.

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma
Aşağıdaki adımlar bir Hive ODBC veri kaynağı oluşturma gösterir.

1. Windows 8 veya Windows 10, başlangıç ekranından açmak için Windows tuşuna basın ve ardından **veri kaynakları**.
2. Tıklatın **ODBC veri kaynakları (32 bit) ayarlamanız** veya **ODBC veri kaynakları (64 bit) ayarlamanız** Office sürümüne bağlı olarak. Windows 7 kullanıyorsanız seçin **ODBC veri kaynakları (32 bit)** veya **ODBC veri kaynakları (64 bit)** gelen **Yönetimsel Araçlar**. Göreceksiniz **ODBC Veri Kaynağı Yöneticisi** iletişim.
   
    ![OBDC Veri Kaynağı Yöneticisi](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "ODBC Veri Kaynağı Yöneticisi kullanan bir DSN'ye yapılandırın")

3. Kullanıcı DNS'den tıklatın **Ekle** açmak için **yeni veri kaynağı oluştur** Sihirbazı.
4. Seçin **Microsoft Hive ODBC sürücüsü**ve ardından **son**. Göreceksiniz **Microsoft Hive ODBC sürücüsü DNS Kurulumu** iletişim.
5. Aşağıdaki değerleri yazın veya seçin:
   
   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Host |&lt;HDInsightKümesiAdı>.azurehdinsight.net yazın. Örnek: HDIKumesi.azurehdinsight.net |
   |  Bağlantı noktası |<strong>443</strong> yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
   |  Veritabanı |<strong>Default</strong>’u kullanın. |
   |  Mekanizma |<strong>Azure HDInsight Service</strong>’i seçin |
   |  Kullanıcı adı |Hdınsight küme HTTP kullanıcının kullanıcı adı girin. Varsayılan kullanıcı adı <strong>admin</strong> şeklindedir. |
   |  Parola |Hdınsight küme kullanıcı parolasını girin. |
   
    </table>
   
    Tıkladığınızda dikkate etmeniz gereken bazı önemli parametre **Gelişmiş Seçenekler**:
   
   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel sorgu kullanma |Seçildiğinde, HiveQL TSQL dönüştürmek ODBC sürücüsü denemez. Yalnızca % 100 saf HiveQL ifadelerini gönderiliyor emin olduğunuzda kullanın. SQL Server veya Azure SQL veritabanına bağlanırken denetlenmeyen bırakmanız gerekir. |
   |  Satır blok alınarak |Çok sayıda kayıt getirilirken Bu parametre ayarlama en iyi performanslarını emin olmak için gerekli. |
   |  Varsayılan dize sütun uzunluğu, ikili sütun uzunluğu, ondalık sütun ölçeği |Veri türü uzunlukları ve Precision bilgisayarlar, verinin nasıl döndürüldüğüne etkileyebilir. Bunlar duyarlık ve/veya kesme kaybı nedeniyle döndürülecek yanlış bilgi neden. |

    ![Gelişmiş Seçenekleri](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN Gelişmiş yapılandırma seçenekleri")

1. Tıklatın **Test** veri kaynağı test etmek için. Veri kaynağının doğru şekilde yapılandırıldığında, gösterir *TESTLERİ başarıyla tamamlandı!*.
2. Tıklatın **Tamam** Test iletişim kutusunu kapatmak için. Yeni veri kaynağı üzerinde listelenen **ODBC Veri Kaynağı Yöneticisi**.
3. Tıklatın **Tamam** sihirbazdan çıkmak için.

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma
Aşağıdaki adımlar, önceki bölümde oluşturduğunuz ODBC veri kaynağını kullanan bir Excel çalışma kitabı Hive tablodan veri aktarmak için yol açıklamaktadır.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.
2. Gelen **veri** sekmesini tıklatın, **Veri Al**, tıklatın **diğer kaynaklardan**ve ardından **gelen ODBC** başlatmak için **Veri Bağlantı Sihirbazı**.
   
    ![Açık Veri Bağlantı Sihirbazı'nı](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "açık Veri Bağlantı Sihirbazı")
4. Son bölümünde oluşturduğunuz veri kaynağı adı seçin ve ardından **Tamam**.
5. (Varsayılan adı yönetici) Hadoop kullanıcı adını ve parolasını girin ve ardından **Bağlan**.
6. Gezgin üzerinde genişletin **HIVE**, genişletin **varsayılan**, tıklatın **hivesampletable**ve ardından **yük**. Verilerin Excel’e aktarılması birkaç saniye sürer.

    ![Hdınsight Hive ODBC Gezgini](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "açık Veri Bağlantı Sihirbazı")


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Hive ODBC sürücüsü Hdınsight hizmetinden Excel'e veri almak için nasıl kullanılacağı hakkında bilgi edindiniz. Benzer şekilde, SQL veritabanına Hdınsight hizmetinden veri alabilirsiniz. Bir Hdınsight hizmetine veri yüklemeye mümkündür. Daha fazla bilgi için bkz:

* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](apache-hadoop-connect-hive-power-bi.md).
* [Etkileşimli sorgu Hive verileri Azure hdınsight'ta Power BI ile görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./../hdinsight-upload-data.md).

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


