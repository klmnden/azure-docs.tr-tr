---
title: Azure HDInsight bellek hatası dışında bir Hive Düzelt
description: Yetersiz bellek hatasını HDInsight içinde bir Hive'ı düzeltin. Bir müşteri senaryosuna birçok büyük tablolar arasında bir sorgu gereklidir.
keywords: yetersiz bellek hata OOM, Hive ayarları
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 2e7328b95aecc8e644d7b9e2ec407a62551fff79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712795"
---
# <a name="fix-an-apache-hive-out-of-memory-error-in-azure-hdinsight"></a>Yetersiz bellek hatasını Azure HDInsight içinde bir Apache Hive Düzelt

Büyük tablolar Hive bellek ayarlarını yapılandırarak işlerken bir Apache Hive yetersiz bellek (OOM) hatasını düzeltme hakkında bilgi edinin.

## <a name="run-apache-hive-query-against-large-tables"></a>Apache Hive sorgusu çalıştırma karşı büyük tabloları

Bir müşteri bir Hive sorgusu çalıştıran:

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Bu sorgu, bazı küçük farklar:

* T1, çok sayıda dize sütun türleri olan TABLE1, büyük bir tabloya bir diğer addır.
* Diğer tablolar değil, büyük ancak çok sayıda sütun var.
* Tüm tabloları birden çok sütunda tablo1 ve diğerleri ile bazı durumlarda, birbiriyle katıldığınız.

Hive sorgusu 24 A3 HDInsight küme düğümünde tamamlanması 26 dakika sürdü. Müşteri, aşağıdaki uyarı iletilerini fark:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Apache Tez yürütme altyapısı kullanarak. Aynı sorgu 15 dakika boyunca çalıştırıldı ve ardından şu hatayı oluşturdu:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Hata, daha büyük bir sanal makine (örneğin, D12) kullanırken kalır.


## <a name="debug-the-out-of-memory-error"></a>Yetersiz bellek hatası hata ayıklama

Yetersiz bellek hatası neden sorunlardan biri olan desteğimiz ve mühendislik ekipleri birlikte bulunan bir [bilinen sorun Apache JIRA'da açıklanan](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesn't take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

**Hive.auto.convert.join.noconditionaltask** hive-site.xml dosyasının ayarlandı **true**:

```xml
<property>
    <name>hive.auto.convert.join.noconditionaltask</name>
    <value>true</value>
    <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
    </description>
</property>
```

Büyük olasılıkla harita birleşimi olan Java yığın alanı nedenini bizim bellek hatası. Blog gönderisinde açıklandığı gibi [HDInsight Hadoop Yarn bellek ayarlarını](https://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), Tez yürütme altyapısı, yığın kullanıldığında kullanılan alanı gerçekte Tez kapsayıcıya ait. Tez kapsayıcı bellek açıklayan aşağıdaki resme bakın.

![Tez kapsayıcı bellek diyagramı: Yetersiz bellek hatası hive](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Blog gönderisinde anlaşılacağı gibi aşağıdaki iki bellek ayarları öbek için kapsayıcı belleği tanımlayın: **hive.tez.container.size** ve **hive.tez.java.opts**. Deneyimlerimizden, yetersiz bellek özel durumu kapsayıcı boyutu çok küçükse gelmez. Java yığın boyutu (hive.tez.java.opts) çok küçük anlamına gelir. Bellek yetersiz gördüğünüzde artırmak yapabileceğiniz şekilde **hive.tez.java.opts**. Gerekirse artırmanız gerekebilir **hive.tez.container.size**. **Java.opts** ayarı yaklaşık % 80'i olmalıdır **container.size**.

> [!NOTE]  
> Ayar **hive.tez.java.opts** her zaman daha küçük olmalıdır **hive.tez.container.size**.
> 
> 

D12 makine, 28 GB belleğe sahip olduğundan, bir kapsayıcı boyutu 10 GB (10240 MB) kullanın ve % 80 için java.opts atamak verdik:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Sorgu, yeni ayarlarla 10 dakika içinde başarıyla çalıştı.

## <a name="next-steps"></a>Sonraki adımlar

OOM hata alınırken mutlaka kapsayıcı boyutu çok küçükse anlamına gelmez. Bunun yerine, böylece öbek boyutu artar ve kapsayıcı bellek boyutu % 80'en az bellek ayarlarını yapılandırmanız gerekir. Hive sorguları iyileştirmek için bkz: [HDInsight, Apache Hadoop için en iyi duruma getirme, Apache Hive sorguları](hdinsight-hadoop-optimize-hive-query.md).
