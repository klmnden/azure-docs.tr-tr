---
title: Azure Hdınsight araçları - Visual Studio kodunu PySpark etkileşimli ortamını ayarlama | Microsoft Docs
description: Visual Studio Code Azure Hdınsight araçları oluşturmak ve sorgular ve komut dosyaları göndermek için nasıl kullanılacağını öğrenin.
Keywords: VScode,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
manager: ''
editor: ''
tags: azure-portal
ms.assetid: ''
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 4ab7b95861fcd1ff75f8ac84e4f00aedb6e526f3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31407240"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio kodunu PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımlar çalıştırarak Python paketlerini yükleme gösterir **Hdınsight: PySpark etkileşimli**.


## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>PySpark etkileşimli ortamda macOS ve Linux ayarlama
Kullanıyorsanız, **python 3.x**, komut kullanmanıza gerek **pip3** aşağıdaki adımları için:

1. Emin olun **Python** ve **PIP** yüklenir.
 
    ![Python PIP sürümü](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter yükleyin.
    ```
    sudo pip install jupyter
    ```
   Linux ve macOS aşağıdaki hata iletisini görebilirsiniz:

   ![Hata 1](./media/set-up-pyspark-interactive-environment/error1.png)

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

4. Olduğundan emin olun **ipywidgets** aşağıdakini çalıştırarak düzgün yüklendiğinden:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Sarmalayıcı tekrar yükleme](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Sarmalayıcı tekrar yükleyin. Çalıştırma **PIP Göster sparkmagic**. Çıkış yolu gösterir **sparkmagic** yükleme. 

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

    Kullanılabilir tekrar için: 
    - **python2** ve **pysparkkernel** karşılık **python 2.x**. 
    - **python3** ve **pyspark3kernel** karşılık **python 3.x**. 

8. VS Code'u yeniden başlatın ve sonra çalışan bir komut dosyası Düzenleyicisi geri dönün **Hdınsight: PySpark etkileşimli**.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* VS Code'da Hdınsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Visual Studio kodunu Azure Hdınsight aracını kullanın](hdinsight-for-vscode.md)
* [Oluşturma ve Spark Scala uygulamaları göndermek amacıyla Intellij için Azure araç seti kullanın](spark/apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında SSH üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Spark uygulamalarında VPN üzerinden uzaktan hata ayıklamak için Azure araç Intellij için kullanma](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Hdınsight araçları Azure araç setini Eclipse için Spark uygulamaları oluşturmak için kullanın](spark/apache-spark-eclipse-tool-plugin.md)
* [Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight’ta Microsoft Power BI ile Hive verileri görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın ](hdinsight-connect-hive-zeppelin.md)
