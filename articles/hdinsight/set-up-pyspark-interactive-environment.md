---
title: "Azure Hdınsight araçları - Visual Studio kodunu PySpark etkileşimli ortamını ayarlama | Microsoft Docs"
description: "Azure Hdınsight araçları Visual Studio Code için oluşturma, sorgular ve komut dosyaları göndermek için nasıl kullanacağınızı öğrenin."
Keywords: "VScode, Azure Hdınsight araçları, Hive, Python, PySpark, Spark, Hdınsight, Hadoop, LLAP, etkileşimli Hive, etkileşimli sorgu"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: 
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 24839aadaee07b98ac5a6e6cfd14e44de54e7e7e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="set-up-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio kodunu PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımları çalıştırdığınızda, python paketlerini yükleme Göster **Hdınsight: PySpark etkileşimli**.


## <a name="set-up-pyspark-interactive-environment-on-macos-and-linux"></a>PySpark etkileşimli ortamında MacOS ve Linux ayarlama
Komutunu kullanmanız gerekir **pip3** için aşağıdaki adımları yazılmışsa, **python 3.x**.
1. Emin olun **Python** ve **PIP** yüklü.
 
    ![Python PIP sürümü](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter yükleyin
    ```
    sudo pip install jupyter
    ```
    +  Belki de aşağıdaki hata iletisini gelen Linux ve MacOS:

        ![error1](./media/set-up-pyspark-interactive-environment/error1.png)
        ```Resolve:
        sudo pip uninstall asyncio
        sudo pip install trollies
        ```

    + Libkrb5 dev(For Linux only) yüklemek, belki de aşağıdaki hata iletisini görüntüler:

        ![error2](./media/set-up-pyspark-interactive-environment/error2.png)
        ```Resolve:
        sudo apt-get install libkrb5-dev 
        ```

3. Sparkmagic yükleyin
   ```
   sudo pip install sparkmagic
   ```

4. Bu ipywidgets çalıştırarak doğru şekilde yüklendiğinden emin olun:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Sarmalayıcı tekrar yükleme](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Sarmalayıcı tekrar yükleyin. Çalıştırma **PIP Göster sparkmagic** ve bu sparkmagic yüklü yolunu gösterir. 

    ![sparkmagic konumu](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Konum ve Çalıştır gidin:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec yükleme](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Yükleme durumunu kontrol edin: 

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec listesi](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Kullanılabilir tekrar için: **python2** ve **pysparkkernel** karşılık **python 2.x**, **python3** ve  **pyspark3kernel** karşılık **python 3.x**. 

8. VScode yeniden başlatın ve yedekleme çalıştıran komut dosyası Düzenleyicisi **Hdınsight: PySpark etkileşimli**.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım
* Hdınsight için VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

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
* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın](hdinsight-connect-hive-zeppelin.md)
