---
title: "Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın | Microsoft Docs"
description: "Hive sorguları çalıştırmak için Zeppelin kullanmayı öğrenin."
keywords: "hdınsight hadoop, hive, etkileşimli sorgu, LLAP"
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
ms.openlocfilehash: b44321619f2aa94a6d98624ab1ee35a598fb6fc8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın 

Hdınsight etkileşimli sorgu kümeleri etkileşimli Hive sorguları çalıştırmak için kullanabileceğiniz Zeppelin not defterlerini içerir. Bu makalede, Zeppelin Azure Hdınsight'ta Hive sorguları çalıştırmak için nasıl kullanılacağını öğrenin. 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede geçmeden önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight etkileşimli sorgu küme**. Bkz: [küme oluştur](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster) bir Hdınsight kümesi oluşturmak için.  Etkileşimli sorgu türü seçtiğinizden emin olun. 

## <a name="create-a-zeppelin-note"></a>Zeppelin not oluşturma

1. Aşağıdaki URL'ye gidin:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    **CLUSTERNAME** değerini kümenizin adıyla değiştirin.

2. Hadoop kullanıcı adını ve parolasını girin. Zeppelin sayfasından yeni bir not oluşturabilir veya varolan notları'nı açın. HiveSample bazı örnek Hive sorguları içerir.  

    ![Hdınsight etkileşimli sorgu zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Tıklatın **yeni not oluşturmak**.
4. Aşağıdaki değerleri yazın veya seçin:

    - Not ad: notu için bir ad girin.
    - Varsayılan yorumlayıcı: seçin **JDBC**.

5. Tıklatın **not oluşturmak**.
6. Aşağıdaki Hive sorgusunu çalıştırın:

        %jdbc(hive)
        show tables

    ![Hdınsight etkileşimli sorgu zeppelin sorgusu çalıştırır](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    **%Jdbc(hive)** ilk satırı deyiminde Hive JDBC yorumlayıcı kullanmak için Not Defteri söyler.

    Sorgu adı verilen bir Hive tablosu döndürme *hivesampletable*.

    Hivesampletable karşı çalıştırabilirsiniz iki daha fazla Hive sorguları verilmiştir. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Geleneksel Hive karşılaştırma, sorgu sonuçları geri gereken daha hızlı dönün.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight Power BI kullanarak verileri Görselleştir öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](./hdinsight-connect-hive-power-bi.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](./hdinsight-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](./hdinsight-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](./hdinsight-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./hdinsight-upload-data.md).
