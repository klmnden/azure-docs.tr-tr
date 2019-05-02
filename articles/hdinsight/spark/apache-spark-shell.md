---
title: Azure HDInsight içinde bir etkileşimli bir Spark Shell kullanma
description: Etkileşimli bir Spark Shell, Spark, bir seferde komutları çalıştırma ve sonuçları görmek için bir okuma-yürütme-yazdırma işlemi sunar.
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 9044ed3ad9cf9ffa2f54d130bb50b37df121b86f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696811"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Apache Spark Spark Kabuğu'ndan çalıştırma

Etkileşimli [Apache Spark](https://spark.apache.org/) kabuğu, Spark, bir seferde komutları çalıştırma ve sonuçları görmek için bir REPL (okuma-yürütme-Yazdır, döngü) ortamı sağlar. Bu işlem, geliştirme ve hata ayıklama için kullanışlıdır. Spark her biri, desteklenen diller için bir kabuk sağlar: Scala, Python ve R'dir

## <a name="get-to-an-apache-spark-shell-with-ssh"></a>Bir Apache Spark Shell SSH ile öğrenin

Birincil baş düğümüne SSH kullanarak kümeye'nın bir HDInsight üzerinde Apache Spark Shell erişmek:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

Kümeniz için tüm SSH komutunu Azure portalından alabilirsiniz:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. HDInsight Spark kümenize bölmesine gidin.
3. Güvenli Kabuk (SSH) seçin.

    ![Azure portalında HDInsight bölmesi](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Görüntülenen SSH komutu kopyalayın ve terminalinizde çalıştırın.

    ![Azure portalında HDInsight SSH bölmesi](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

HDInsight için bağlanmak için SSH kullanma hakkında daha fazla bilgi için bkz [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-an-apache-spark-shell"></a>Bir Apache Spark Shell çalıştırın

Spark Kabukları (spark-shell) Scala, Python (pyspark) ve R (sparkR) sağlar. HDInsight kümenizin baş düğüm, SSH oturumunda, aşağıdaki komutlardan birini girin:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

Artık Spark komutları uygun dilde girebilirsiniz.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession ve zamanda sparkcontext öğesini örnekleri

Spark Shell çalıştırdığınızda varsayılan olarak SparkSession ve zamanda sparkcontext öğesini örnekleri otomatik olarak sizin için oluşturulur.

SparkSession örneğine erişmek için enter `spark`. Zamanda sparkcontext öğesini örneğine erişmek için enter `sc`.

## <a name="important-shell-parameters"></a>Önemli Kabuk parametreleri

Spark Shell komutu (`spark-shell`, `pyspark`, veya `sparkR`) birçok komut satırı parametrelerini destekler. Parametreleri, tam listesini görmek için Spark Shell anahtarıyla başlatma `--help`. Bu parametrelerden bazıları yalnızca için dağıtılsa `spark-submit`, hangi Spark Shell sarmalar.

| anahtarı | açıklama | Örnek |
| --- | --- | --- |
| --master MASTER_URL | Ana URL'yi belirtir. HDInsight bu değer her zaman, `yarn`. | `--master yarn`|
| --jar'lar JAR_LIST | Sürücü hem de Yürütücü sınıf yolları üzerinde dahil etmek için yerel jar dosyaları dışındaki virgülle ayrılmış listesi. HDInsight bu liste, Azure Depolama'da veya Data Lake Storage varsayılan dosya sistemi yolları oluşur. | `--jars /path/to/examples.jar` |
| --MAVEN_COORDS paketleri | Sürücü hem de Yürütücü sınıf yolları üzerinde dahil etmek için maven koordinatları jar dosyaları dışındaki virgülle ayrılmış listesi. Herhangi bir ek uzak depolar ile belirtilen yerel maven deposu, ardından maven central arar `--repositories`. Koordinatları biçimi *GroupID*:*Artifactıd*:*sürüm*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py dosyaları listesi | Python için yalnızca .zip, .egg veya .py dosyaları üzerinde ise PYTHONPATH yerleştirmek için virgülle ayrılmış listesi. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure HDInsight üzerinde Apache Spark'a giriş](apache-spark-overview.md) genel bakış.
- Bkz: [Azure HDInsight Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md) SparkSQL ve Spark kümeleri ile çalışmak için.
- Bkz: [Apache Spark yapılandırılmış Akış nedir?](apache-spark-streaming-overview.md) Spark ile akış verilerini işleme uygulamaları yazmak için.
