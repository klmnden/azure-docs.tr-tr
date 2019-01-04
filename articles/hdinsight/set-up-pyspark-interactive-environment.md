---
title: Azure HDInsight araçları - Visual Studio Code için PySpark etkileşimli ortamını ayarlama
description: Oluşturmak ve sorgular ve betikleri göndermek amacıyla Visual Studio Code için Azure HDInsight Araçları'nı kullanmayı öğrenin.
keywords: VScode, Azure HDInsight araçları, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: bf47915ba93a4a3a7dec338395cfe0ce6aa3cdf6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993850"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code için PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımları çalıştırarak Python paketlerini yükleme Göster **HDInsight: PySpark etkileşimli**.

## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>MacOS ve Linux'ta PySpark etkileşimli ortamını ayarlama
Kullanıyorsanız **python 3.x**, komutunu kullanmanız gerekir **pip3** için aşağıdaki adımları:

1. Emin **Python** ve **pip** yüklenir.
 
    ![Python pip sürümünü](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter yükleyin.
    ```
    sudo pip install jupyter
    ```
   Linux ve Macos'ta aşağıdaki hata iletisini görebilirsiniz:

   ![Hata: 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Yükleme **libkrb5 geliştirme** (için yalnızca Linux). Aşağıdaki hata iletisini görebilirsiniz:

   ![Hata 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Yükleme **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Emin olun **ipywidgets** aşağıdakini çalıştırarak düzgün şekilde yüklenir:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Sarmalayıcı çekirdekler yükleyin](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Sarmalayıcı çekirdekler yükleyin. Çalıştırma **pip Göster sparkmagic**. Çıktı yolu gösterir **sparkmagic** yükleme. 

    ![sparkmagic konumu](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Konuma gidin ve ardından çalıştırın:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec yükleme](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Yükleme durumunu kontrol edin.

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec listesi](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    İçin kullanılabilir çekirdekler: 
    - **python2** ve **pysparkkernel** karşılık **python 2.x**. 
    - **python3** ve **pyspark3kernel** karşılık **python 3.x**. 

8. VS Code'u yeniden başlatın ve sonra çalışan bir betik Düzenleyicisi dönün **HDInsight: PySpark etkileşimli**.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* VS Code için HDInsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Visual Studio Code için Azure HDInsight aracını kullanın](hdinsight-for-vscode.md)
* [Oluşturmak ve Apache Spark Scala uygulamaları göndermek amacıyla Intellij için Azure Araç Seti'ni kullanma](spark/apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında SSH üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Apache Spark uygulamalar VPN üzerinden uzaktan hata ayıklama için Intellij için Azure araç takımı kullanın](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](spark/apache-spark-eclipse-tool-plugin.md)
* [Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri Görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma ](hdinsight-connect-hive-zeppelin.md)
