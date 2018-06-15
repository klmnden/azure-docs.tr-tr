---
title: Etkileşimli sorgu Hive verileri Azure hdınsight'ta Power BI ile Görselleştirme | Microsoft Docs
description: Azure Hdınsight tarafından işlenen etkileşimli sorgu Hive görselleştirmek için Microsoft Power BI kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, etkileşimli sorgu, etkileşimli hive, LLAP, directquery
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
ms.openlocfilehash: b8da1f17b9e477caf9031cf94ee14f3a181e247e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31407971"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Microsoft Power BI'ın Azure Hdınsight'ta doğrudan sorgu kullanarak etkileşimli sorgu Hive verileri görselleştirin

Microsoft Power BI Azure Hdınsight etkileşimli sorgu kümelerine bağlanmak ve Hive verileri doğrudan sorgu kullanarak görselleştirmek öğrenin. Bu öğreticide, verileri Power BI hivesampletable Hive tablodan yükleme. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir world harita kullanım verilerini çizimi:

![Hdınsight Power BI haritası raporu](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Yararlanabileceğiniz [Hive ODBC sürücüsü](../hadoop/apache-hadoop-connect-hive-power-bi.md) Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için. Ancak Hive sorgu altyapısı etkileşimli olmayan yapısını verilen BI iş yükleri için önerilmez. [Hdınsight etkileşimli sorgu bağlayıcı](./apache-hadoop-connect-hive-power-bi-directquery.md) ve [Hdınsight Spark bağlayıcı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi seçimlerdir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight kümesi**. Küme Hive bir Hdınsight kümesini veya yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Bir kopyasından indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Hdınsight'ta veri yükleme

Hivesampletable Hive tablosu tüm Hdınsight kümeleri ile birlikte gelir.

1. Power BI Desktop için oturum açın.
2. Tıklatın **giriş** sekmesini tıklatın, **Veri Al** gelen **dış veri** Şerit ve ardından **daha fazla**.

    ![Hdınsight Power BI açık veri](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Gelen **Veri Al** bölmesi, türü **hdınsight** arama kutusuna. Görmüyorsanız, **Hdınsight etkileşimli sorgu (Beta)**, Power BI Desktop en son sürüme güncelleştirmeniz gerekir.
4. Tıklatın **Hdınsight etkileşimli sorgu (Beta)** ve ardından **Bağlan**.
5. Tıklatın **devam** kapatmak için **Önizleme bağlayıcı** uyarı iletişim kutusu.
6. Gelen **Hdınsight etkileşimli sorgu**aşağıdaki bilgileri girin veya seçin:

    - **Sunucu**: etkileşimli sorgu küme adını girin, örneğin *myiqcluster.azurehdinsight.net*.
    - **Veritabanı**: Bu öğreticide, girin **varsayılan**.
    - **Veri bağlantısı modu**: Bu öğreticide, seçin **DirectQuery**.

    ![Hdınsight etkileşimli sorgu power BI directquery Bağlan](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. **Tamam**’a tıklayın.
8. HTTP kullanıcı kimlik bilgilerini girin ve ardından **Tamam**.  Varsayılan kullanıcı adı **yönetici**
9. Sol bölmeden seçin **hivesampletale**ve ardından **yük**.

    ![Hdınsight etkileşimli sorgu power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Verileri görselleştirin

Son yordamdan devam edin.

1. Görselleştirmeleri bölmesinden seçin **harita**.  Dünya simgesi değil.

    ![Hdınsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. Alanları bölmesinden seçin **Ülke** ve **devicemake**. Haritada çizilmiş veri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight Power BI kullanarak verileri Görselleştir öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power BI'ın Azure Hdınsight'ta ODBC kullanarak Hive görselleştirmek](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./../hdinsight-upload-data.md).
