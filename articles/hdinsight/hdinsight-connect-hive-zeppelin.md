---
title: Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın | Microsoft Docs
description: Hive sorguları çalıştırmak için Zeppelin kullanmayı öğrenin.
keywords: hdınsight hadoop, hive, etkileşimli sorgu, LLAP
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
ms.date: 05/14/2018
ms.author: jgao
ms.openlocfilehash: c8fe65d2eadaede1d99befbf76c4d06fab9598fc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202620"
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın 

Hdınsight etkileşimli sorgu kümeleri etkileşimli Hive sorguları çalıştırmak için kullanabileceğiniz Zeppelin not defterlerini içerir. Bu makalede, Zeppelin Azure Hdınsight'ta Hive sorguları çalıştırmak için nasıl kullanılacağını öğrenin. 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight etkileşimli sorgu küme**. Bkz: [küme oluştur](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) bir Hdınsight kümesi oluşturmak için.  Etkileşimli sorgu türü seçtiğinizden emin olun. 

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

* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Etkileşimli sorgu Hive verileri Azure hdınsight'ta Power BI ile görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](hdinsight-for-vscode.md).
* [Verileri Hdınsight'a yükleme](./hdinsight-upload-data.md).
