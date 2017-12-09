---
title: "Excel'i Power Query - Azure Hdınsight ile hadoop'a bağlama | Microsoft Docs"
description: "Hdınsight'ta Hadoop depolanan verilere erişmek için Excel için Power Query kullanın ve business Intelligence bileşenleri yararlanabilir öğrenin."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: jgao
ms.openlocfilehash: 1a2bb4c56484540f8b5de5fb61ca5b5f611e99c4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="connect-excel-to-hadoop-by-using-power-query"></a>Excel için Hadoop Power Query kullanarak bağlan
Bir anahtar Microsoft büyük veri çözüm Microsoft iş zekası (BI) bileşenleri Azure hdınsight'ta Hadoop kümeleri ile tümleştirilmesi özelliğidir. Excel için Excel eklenti Microsoft Power Query kullanarak Hadoop kümenizle ilişkili verileri içeren Azure depolama hesabı bağlantı özelliği buna birincil bir örnektir. Bu makalede, ayarlayın ve Hdınsight ile yönetilen bir Hadoop kümesine ilişkilendirilmiş sorgu verileri için Power Query nasıl kullanılacağını açıklanmaktadır.

### <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight kümesi**. Bir yapılandırmak için bkz. [Azure Hdınsight kullanmaya başlama] [hdinsight-get-started].
* **Bir iş istasyonu** Windows 7, Windows Server 2008 R2 veya sonraki bir işletim sistemi çalıştıran.
* **Office 2016, Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 tek başına veya Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Power Query yükleme
Power Query, çıktı veya bir Hdınsight kümesi üzerinde çalışan bir Hadoop iş tarafından oluşturuldu verileri içeri aktarabilirsiniz.

Excel 2016'da, Power Query veri Şerit Al & dönüştürme bölümü altında içine tümleştirilmiştir. Excel için Microsoft Power Query eski Excel sürümleri için karşıdan [Microsoft Download Center] [ powerquery-download] ve yükleyin.

## <a name="import-hdinsight-data-into-excel"></a>Hdınsight verileri Excel'e aktarmak
Excel için Power Query Eklentisi verilerini Hdınsight kümenize burada BI araçları gibi PowerPivot ve güç harita incelemek için kullanılabilir çözümlemek ve verileri sunmak Excel'e aktarmak kolay hale getirir.

**Bir Hdınsight kümesinden veri almak için**

1. Excel'i açın.
2. Yeni bir boş çalışma kitabı oluşturun.
3. Excel sürümlerine göre aşağıdaki adımları gerçekleştirin:

    - Excel 2016

        - ' I tıklatın **veri** menüsünde tıklatın **Veri Al** gelen **Al & veri dönüştürme** Şerit'ye tıklayın **Azure**ve ardından**Azure HDInsight(HDFS) gelen**.

        ![HDI. PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Excel 2013/2010

        - Tıklatın **Power Query** menüsünde tıklatın **Azure**ve ardından **Microsoft Azure Hdınsight'den**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Not:** görmüyorsanız **Power Query** menüsünde, Git **dosya** > **seçenekleri** > **eklentileri**seçip **COM eklentileri** açılan gelen **Yönet** sayfanın sonundaki kutusu. Seçin **Git...**  düğmesine tıklayın ve Excel eklentisi için Power Query için onay kutusunun seçili olduğunu doğrulayın.
       
        **Not:** Power Query de verir tıklayarak HDFS verileri almak **diğer kaynaklardan**.
4. İçin **hesap adı**kümenizle ilişkilendirilmiş Azure Blob Depolama hesabı adı girin ve ardından **Tamam**. Bu hesap olabilir [varsayılan depolama hesabı](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account) ya da bağlantılı depolama hesabı.  Biçim *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. İçin **hesap anahtarı**, Blob Depolama hesabı anahtarı girin ve ardından **kaydetmek**. (Bu deposuna erişim hesabı bilgileri yalnızca ilk girmeleri gerekir.)
6. İçinde **Gezgini** sorgu Düzenleyicisi'nin, sol bölmesinde, Blob storage kapsayıcısı adı öğesini çift tıklatın. Varsayılan olarak, küme adı adıyla aynı kapsayıcı adıdır.
7. Bulun **HiveSampleData.txt** içinde **adı** sütun (klasör yolu **../ hive / / hivesampletable/ambar**) ve ardından **ikili** sol tarafındaki HiveSampleData.txt. HiveSampleData.txt tüm kümeyle birlikte gelir. İsteğe bağlı olarak, kendi dosyasını kullanabilirsiniz.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. İsterseniz, sütun adlarının yeniden adlandırabilirsiniz. Hazır olduğunuzda tıklatın **Kapat & yük**.  Verilerin çalışma kitabınızda yüklendi:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight'ta Excel'e veri almak için Power Query kullanın öğrendiniz. Benzer şekilde, Hdınsight'ta Azure SQL veritabanına veri alabilir. Hdınsight'a verileri yüklemek mümkündür. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](apache-hadoop-connect-hive-power-bi.md).
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./../hdinsight-upload-data.md).

[image-hdi-powerquery-hdi-source]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
