---
title: 'Hızlı Başlangıç: Azure HDInsight - Apache Zeppelin Apache Hive sorguları çalıştırma'
description: Bu hızlı başlangıçta, Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, LLAP etkileşimli sorgu
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: hrasheed
ms.openlocfilehash: 1642c64b0b14c2e290aad689399b59d896660a28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056687"
---
# <a name="quickstart-execute-apache-hive-queries-in-azure-hdinsight-with-apache-zeppelin"></a>Hızlı Başlangıç: Apache Zeppelin ile Azure HDInsight, Apache Hive sorguları yürütme

Bu hızlı başlangıçta, Apache Zeppelin çalıştırmak için kullanılacak öğrenin [Apache Hive](https://hive.apache.org/) sorgular, Azure HDInsight. HDInsight etkileşimli sorgu kümelerine dahil [Apache Zeppelin](https://zeppelin.apache.org/) Hive sorguları, etkileşimli olarak çalıştırmak için kullanabileceğiniz not defterleri.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bir HDInsight etkileşimli sorgu kümesi. Bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) bir HDInsight kümesi oluşturma.  Seçtiğinizden emin olun **etkileşimli sorgu** küme türü.

## <a name="create-an-apache-zeppelin-note"></a>Bir Apache Zeppelin Not oluşturun

1. Değiştirin `CLUSTERNAME` aşağıdaki URL'yi kümenizin adıyla `https://CLUSTERNAME.azurehdinsight.net/zeppelin`. Ardından bir web tarayıcısında URL'sini girin.

2. Küme oturum açma kullanıcı adı ve parolayı girin. Zeppelin sayfasından yeni bir not oluşturun veya var olan Notları'nı açın. **HiveSample** bazı örnek Hive sorguları içerir.  

    ![HDInsight etkileşimli sorgu zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)

3. Seçin **oluştur Yeni Not**.

4. Gelen **oluştur Yeni Not** iletişim, tür veya aşağıdaki değerleri seçin:

    - Not adı: Notu için bir ad girin.
    - Varsayılan yorumlayıcı: Seçin **jdbc** aşağı açılan listeden.

5. Seçin **oluşturmanız**.

6. Kod bölümünde aşağıdaki Hive sorgusunu girin ve sonra basın **Shift + Enter**:

    ```hive
    %jdbc(hive)
    show tables
    ```

    ![HDInsight etkileşimli sorgu zeppelin sorgusu çalıştırır.](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    **%Jdbc(hive)** deyiminin ilk satırı Hive JDBC yorumlayıcı kullanmak için Not defterini söyler.

    Sorgu adı verilen bir Hive tablosu döndürme **hivesampletable**.

    Karşı çalıştırabilirsiniz iki ek Hive sorguları şunlardır **hivesampletable**:

    ```hive
    %jdbc(hive)
    select * from hivesampletable limit 10

    %jdbc(hive)
    select ${group_name}, count(*) as total_count
    from hivesampletable
    group by ${group_name=market,market|deviceplatform|devicemake}
    limit ${total_count=10}
    ```

    Geleneksel Hive için karşılaştırma, sorgu sonuçları gereken daha hızlı dönün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanmayı öğrendiniz. Hive sorguları hakkında daha fazla bilgi edinmek için sonraki makalede, Visual Studio ile sorguları yürütüp yapmayı gösterir.

> [!div class="nextstepaction"]
> [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
