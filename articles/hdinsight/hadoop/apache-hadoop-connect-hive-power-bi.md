---
title: Azure hdınsight'ta Power BI ile büyük veri Görselleştirme | Microsoft Docs
description: Azure Hdınsight tarafından işlenen Hive görselleştirmek için Microsoft Power BI kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, etkileşimli sorgu, etkileşimli hive, LLAP, odbc
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: jgao
ms.openlocfilehash: 1c31f7247cc091d233f754dc19b996ee8234a2e9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="visualize-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Microsoft Power BI'ın Azure Hdınsight'ta ODBC kullanarak Hive verileri görselleştirin

Microsoft Power BI ODBC kullanarak Azure Hdınsight bağlamayı öğrenin ve Hive verileri görselleştirin. 

>[!IMPORTANT]
> Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için Hive ODBC sürücüsü yararlanabilirsiniz. Ancak Hive sorgu altyapısı etkileşimli olmayan yapısını verilen BI iş yükleri için önerilmez. [Hdınsight etkileşimli sorgu bağlayıcı](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) ve [Hdınsight Spark bağlayıcı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi seçimlerdir.

Bu öğreticide, verileri Power BI hivesampletable Hive tablodan yükleme. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir world harita kullanım verilerini çizimi:

![Hdınsight Power BI haritası raporu](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Bilgiler de yeni geçerlidir [etkileşimli sorgu](../interactive-query/apache-interactive-query-get-started.md) küme türü. Hdınsight etkileşimli doğrudan sorgu kullanarak sorgu bağlanmak için bkz: nasıl [Microsoft Power BI'ın Azure Hdınsight'ta doğrudan sorgu kullanarak verileri etkileşimli sorgu Hive görselleştirmek](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).



## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight kümesi**. Küme Hive Hdınsight kümesiyle ya da yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Bir kopyasından indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma

Bkz: [oluşturma Hive ODBC veri kaynağını](apache-hadoop-connect-excel-hive-odbc-driver.md#create-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Hdınsight'ta veri yükleme

Hivesampletable Hive tablosu tüm Hdınsight kümeleri ile birlikte gelir.

1. Power BI Desktop için oturum açın.
2. Tıklatın **giriş** sekmesini tıklatın, **Veri Al** gelen **dış veri** Şerit ve ardından **daha fazla**.

    ![Hdınsight Power BI açık veri](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Gelen **Veri Al** bölmesinde, tıklatın **diğer** soldan tıklatın **ODBC** sağdan ve ardından **Bağlan** alt.
4. Gelen **gelen ODBC** veri kaynağı adı son bölümünde oluşturduğunuz ve ardından bölmesinde, **Tamam**.
5. Gelen **Gezgini** bölmesini genişletin **ODBC -> HIVE varsayılan ->**seçin **hivesampletable**ve ardından **yük**.

## <a name="visualize-data"></a>Verileri görselleştirin

Son yordamdan devam edin.

1. Görselleştirmeleri bölmesinden seçin **harita**.  Dünya simgesi değil.

    ![Hdınsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Alanları bölmesinden seçin **Ülke** ve **devicemake**. Haritada çizilmiş veri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight Power BI kullanarak verileri Görselleştir öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./../hdinsight-upload-data.md).
