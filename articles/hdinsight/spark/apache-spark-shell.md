---
title: Azure Hdınsight'ta bir etkileşimli Spark kabuğunu kullanın | Microsoft Docs
description: Etkileşimli bir Spark Kabuk Spark komutları tek bir seferde çalıştırma ve sonuçları görmek için bir okuma yürütme yazdırma işlemi sunar.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: maxluk
ms.openlocfilehash: d2b65980516a7ae1857711f2e58d9cd0a8e8ec9a
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="run-spark-from-the-spark-shell"></a>Spark Spark Kabuğu'ndan çalıştırın

Etkileşimli bir Spark Kabuk Spark komutları tek bir seferde çalıştırma ve sonuçları görmek için bir REPL (okuma yürütme yazdırma döngü) ortamı sağlar. Bu işlem, geliştirme ve hata ayıklama için yararlıdır. Spark her desteklenen diller için bir kabuk sağlar: Scala, Python ve r

## <a name="get-to-a-spark-shell-with-ssh"></a>SSH Spark kabuğunda öğrenin

Hdınsight'ta Spark Kabuk birincil baş düğümüne SSH kullanarak kümeye bağlanarak erişebilirsiniz:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

Azure portalından, kümeniz için tam SSH komutu alabilirsiniz:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Hdınsight Spark kümenizin bölmesine gidin.
3. Güvenli Kabuk (SSH) seçin.

    ![Azure portalında Hdınsight bölmesi](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Görüntülenen SSH komutu kopyalayın ve terminalinizde çalıştırın.

    ![Hdınsight SSH bölmesinde Azure portalında](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Hdınsight'a bağlanmak için SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-a-spark-shell"></a>Spark Shell çalıştırma

Spark Scala (spark-shell), Python (pyspark) ve R (sparkR) Kabukları sağlar. Hdınsight kümesi baş düğümüne adresindeki SSH oturumunda, aşağıdaki komutlardan birini girin:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

Şimdi, uygun dili Spark komutları girebilirsiniz.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession ve SparkContext örnekleri

Spark Kabuk çalıştırdığınızda, varsayılan olarak SparkSession ve SparkContext örnekleri otomatik olarak sizin için oluşturulur.

SparkSession örneğine erişmek için girmek `spark`. SparkContext örneğine erişmek için girmek `sc`.

## <a name="important-shell-parameters"></a>Önemli Kabuk parametreleri

Spark Kabuk komut (`spark-shell`, `pyspark`, veya `sparkR`) birçok komut satırı parametreleri destekler. Tam parametrelerin listesini görmek için Spark Kabuk anahtarıyla başlatma `--help`. Bu parametrelerden bazıları yalnızca için uygulanabilir olduğunu unutmayın `spark-submit`, hangi Spark Kabuk sarmalar.

| geçiş | açıklama | Örnek |
| --- | --- | --- |
| --Ana MASTER_URL | Ana URL'yi belirtir. Hdınsight'ta, bu değer her zaman kullanılabilir `yarn`. | `--master yarn`|
| --jar'lar JAR_LIST | Sürücü ve yürütücü sınıf yolları üzerinde dahil etmek için yerel Kavanoz virgülle ayrılmış listesi. Hdınsight'ta, bu liste, Azure depolama veya Data Lake Store varsayılan dosya sistemi yolları kümesinden oluşur. | `--jars /path/to/examples.jar` |
| --paketleri MAVEN_COORDS | Sürücü ve yürütücü sınıf yolları üzerinde dahil etmek için maven koordinatlarını Kavanoz virgülle ayrılmış listesi. Tüm ek uzak depoları ile belirtilen sonra yerel maven depodaki sonra maven Orta arar `--repositories`. Koordinatları biçimi *GroupID*:*Artifactıd*:*sürüm*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py dosyaları listesi | Python için yalnızca üzerinde PYTHONPATH yerleştirmek için .zip, .egg veya .py dosyaları virgülle ayrılmış listesi. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure hdınsight'ta Spark giriş](apache-spark-overview.md) bir genel bakış.
- Bkz: [Azure Hdınsight'ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md) Spark kümeleri ve SparkSQL çalışmak için.
- Bkz: [Spark yapılandırılmış Akış nedir?](apache-spark-streaming-overview.md) Spark ile akış veri işleyen uygulamalar yazmak için.

