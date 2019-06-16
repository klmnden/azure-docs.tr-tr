---
title: Azure HDInsight araçları - Visual Studio Code için PySpark etkileşimli ortamını ayarlama
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
keywords: VScode, Azure HDInsight araçları, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 1/17/2019
ms.openlocfilehash: cd0eb03c80a2a379cc84144b0e86000e40753774
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066510"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code için PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımları VS code'da PySpark etkileşimli ortamını ayarlama işlemini göstermektedir.

Kullandığımız **python/pip** giriş yolda sanal ortam oluşturmak için komutu. Başka bir sürümünü kullanmak istiyorsanız, varsayılan sürümünü değiştirmek gereken **python/pip** el ile komutu. Daha fazla ayrıntı görmek [güncelleştirme alternatifleri](https://linux.die.net/man/8/update-alternatives).

1. Yükleme [Python](https://www.python.org/downloads/) ve [pip](https://pip.pypa.io/en/stable/installing/).
   
   + Python'dan yükleme [ https://www.python.org/downloads/ ](https://www.python.org/downloads/).
   + Instalovat modul pip gelen [ https://pip.pypa.io/en/stable/installing ](https://pip.pypa.io/en/stable/installing/). (Python yükleme yüklü değilse)
   + Python doğrulayın ve pip, aşağıdaki komutları kullanarak başarıyla yüklenir. (İsteğe bağlı)
 
        ![Python pip sürümünü](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > Python'ın MacOS varsayılan sürümü kullanmak yerine el ile yüklemeniz önerilir.


2. Yükleme **virtualenv** aşağıdaki komutu çalıştırarak.
   
   ```
   pip install virtualenv
   ```

3. Yalnızca Linux için hata iletisiyle karşılaşırsanız, aşağıdaki komutları çalıştırarak gerekli paketleri yükleyin.
   
    ![Python pip sürümünü](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)
       
   ```
   sudo apt-get install libkrb5-dev 
   ```

   ```
   sudo apt-get install python-dev
   ```

4. VS Code'u yeniden başlatın ve sonra çalışan bir betik Düzenleyicisi dönün **HDInsight: PySpark etkileşimli**.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* VS Code için HDInsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Visual Studio Code için Azure HDInsight aracını kullanın](hdinsight-for-vscode.md)
* [Oluşturmak ve Apache Spark Scala uygulamaları göndermek amacıyla Intellij için Azure Araç Seti'ni kullanma](spark/apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Apache Spark uygulamalar VPN üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri Görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight'da Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanma](./interactive-query/hdinsight-connect-hive-zeppelin.md)
