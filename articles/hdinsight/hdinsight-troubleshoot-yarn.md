---
title: "YARN Azure Hdınsight kullanarak sorun giderme | Microsoft Docs"
description: "Apache Hadoop YARN ve Azure Hdınsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın."
keywords: "Azure Hdınsight, YARN, SSS, sorun giderme kılavuzu, sık sorulan sorular"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: arijitt
ms.openlocfilehash: fbcb4807aa7f6a3d6227cd630c77714c4d2834b3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>YARN Azure Hdınsight kullanarak sorun giderme

Apache Ambari, Apache Hadoop YARN yükü ile çalışırken, üst sorunları ve bunların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Bir kümede nasıl yeni bir YARN sıra oluşturulsun mu?


### <a name="resolution-steps"></a>Çözüm adımları 

Yeni bir YARN kuyruk oluşturmak için Ambari aşağıdaki adımları kullanın ve kapasite ayırma tüm sıraları arasında dengeleyin. 

Bu örnekte, iki mevcut sıraları (**varsayılan** ve **thriftsvr**) hem de yeni sıra (spark) % 50 kapasite verir % 25 kapasite için % 50 kapasiteden değiştirilir.
| Kuyruk | Kapasite | Maksimum kapasite |
| --- | --- | --- | --- |
| varsayılan | %25 | 50% |
| thrftsvr | %25 | 50% |
| Spark | 50% | 50% |

1. Seçin **Ambari görünümleri** simgesi ve Kılavuz düzeni seçin. Ardından, **YARN sıra yöneticisi**.

    ![Ambari görünümleri simgesini seçin](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Seçin **varsayılan** sırası.

    ![Varsayılan sıra seçin](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. İçin **varsayılan** kuyruk, değiştirme **kapasite** %50 %25. İçin **thriftsvr** kuyruk, değiştirme **kapasite** % 25.

    ![Varsayılan ve thriftsvr sıralar % 25 kapasite değiştirme](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Yeni bir kuyruk oluşturmak için seçin **ekleme sırası**.

    ![Select kuyruk Ekle](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. Yeni kuyruk adı.

    ![Spark kuyruk adı](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Bırakın **kapasite** % 50 ve ardından değerlerinde **Eylemler** düğmesi.

    ![Eylemler düğmesini seçin](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Seçin **kaydedin ve yenileyin sıraları**.

    ![Kaydet'i seçin ve Kuyruklar Yenile](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Bu değişiklikler hemen YARN Zamanlayıcı UI görünür.

### <a name="additional-reading"></a>Ek kaynaklar

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Bir kümeden nasıl YARN günlüklerini karşıdan?


### <a name="resolution-steps"></a>Çözüm adımları 

1. Bir güvenli Kabuk (SSH) istemcisi kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz: [ek okuma](#additional-reading-2).

2. YARN uygulamalarının çalışmakta olan tüm uygulama kimlikleri listelemek için aşağıdaki komutu çalıştırın:

    ```apache
    yarn top
    ```
    Kimlikler listelenen **APPLİCATİONID** sütun. Günlükleri indirebilirsiniz **APPLİCATİONID** sütun.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. Tüm uygulama yöneticileri için YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Bu komut amlogs.txt adlı bir günlük dosyası oluşturur. 

4. Yalnızca en son uygulama şablonu için YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Bu komut latestamlogs.txt adlı bir günlük dosyası oluşturur. 

4. İlk iki uygulama yöneticileri için YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Bu komut first2amlogs.txt adlı bir günlük dosyası oluşturur. 

5. Tüm YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Bu komut logs.txt adlı bir günlük dosyası oluşturur. 

6. Belirli bir kapsayıcıya YARN kapsayıcı günlüğü indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Bu komut containerlogs.txt adlı bir günlük dosyası oluşturur.

### <a name="additional-reading-2"></a>Ek kaynaklar

- [SSH kullanarak Hdınsight (Hadoop) bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN kavramları ve uygulamalar](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)


### <a name="see-also"></a>Ayrıca Bkz.
[Azure Hdınsight kullanarak sorun giderme](hdinsight-troubleshoot-guide.md)







