---
title: "Azure hdınsight'ta Power BI ile büyük veri Görselleştirme | Microsoft Docs"
description: "Azure Hdınsight tarafından işlenen Hive görselleştirmek için Microsoft Power BI kullanmayı öğrenin."
keywords: "hdınsight, hadoop, hive, etkileşimli sorgu, etkileşimli hive, LLAP, odbc"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jgao
ms.openlocfilehash: 390342eb08ae970fa760b414674b1a6783404d80
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="visualize-hive-data-with-microsoft-power-bi-in-azure-hdinsight"></a>Microsoft Power BI'ı Azure hdınsight'ta Hive verileri görselleştirin

Azure Hdınsight için Microsoft Power BI bağlanmak ve Hive verileri görselleştirmek öğrenin. Şu anda Power BI yalnızca Hdınsight ODBC bağlantısı destekler. Bu öğreticide, verileri Power BI hivesampletable Hive tablodan yükleme. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir world harita kullanım verilerini çizimi:

![Hdınsight Power BI haritası raporu](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Bilgiler de yeni geçerlidir [etkileşimli sorgu](../interactive-query/apache-interactive-query-get-started.md) küme türü.

## <a name="prerequisites"></a>Ön koşullar
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

## <a name="visualize-date"></a>Tarih Görselleştirme

Son yordamdan devam edin.

1. Görselleştirmeleri bölmesinden seçin **harita**.  Dünya simgesi değil.

    ![Hdınsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Alanları bölmesinden seçin **Ülke** ve **devicemake**. Haritada çizilmiş veri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight Power BI kullanarak verileri Görselleştir öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./../hdinsight-upload-data.md).
