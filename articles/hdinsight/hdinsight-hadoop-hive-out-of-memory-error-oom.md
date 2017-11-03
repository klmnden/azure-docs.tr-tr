---
title: "Yetersiz bellek hatasını Azure hdınsight'ta Hive düzeltme | Microsoft Docs"
description: "Yetersiz bellek hatasını hdınsight'ta Hive düzeltin. Müşteri bir sorgu birçok büyük tabloları arasında bir senaryodur."
keywords: "yetersiz bellek hata, OOM, Hive ayarları"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 7bce3dff-9825-4fa0-a568-c52a9f7d1dad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/17/2017
ms.author: jgao
ms.openlocfilehash: da1247070ade11f78b505524f5e970e18eb16d10
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a>Yetersiz bellek hatasını Azure hdınsight'ta Hive Düzelt

Yetersiz bellek hatasını Hive düzeltme hakkında bilgi almak zaman büyük tabloları Hive bellek ayarlarını yapılandırarak işlem.

## <a name="run-hive-query-against-large-tables"></a>Hive sorgusu çalıştırma karşı büyük tabloları

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

Bu sorgunun bazı küçük farklar:

* T1 dize sütun türleri çok sayıda sahip tablo1 büyük bir tabloya diğer ad değil.
* Diğer tablolar değil, büyük ancak çok sayıda sütuna sahip.
* Tüm tabloları birbirlerine tablo1 ve diğerleri birden fazla sütunlu bazı durumlarda birleştirebilirsiniz.

Hive sorgusu 24 düğümde A3 Hdınsight kümesi tamamlanması 26 dakika sürdü. Müşterinin, aşağıdaki uyarı iletilerinin fark:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Tez yürütme altyapısı kullanarak. Aynı sorgu için 15 dakika çalıştıran ve şu hatayı oluşturdu:

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

Bizim desteği ve mühendislik ekipleri birlikte, bellek yetersiz hatası neden sorunlardan biri bulundu bir [Apache JIRA açıklanan sorunu bilinen](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

**Hive.auto.convert.join.noconditionaltask** hive-site.xml dosyasının ayarlandı **true**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
              If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
              specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
      </property>

Büyük olasılıkla harita birleştirme Java yığın alanı neden oldu bizim bellek hatası. Blog gönderisinde açıklandığı gibi [hdınsight'ta Hadoop Yarn bellek ayarlarını](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), Tez yürütme altyapısı öbek kullanıldığında kullanılan alanı gerçekte Tez kapsayıcıya aittir. Tez kapsayıcı bellek açıklayan aşağıdaki görüntüde bakın.

![Tez kapsayıcı bellek diyagramı: Hive yetersiz bellek hatası](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Blog postası da anlaşılacağı gibi aşağıdaki iki bellek ayarlarını öbek için kapsayıcı belleği tanımlayın: **hive.tez.container.size** ve **hive.tez.java.opts**. Bizim deneyimlerden kapsayıcı boyutu çok küçük yetersiz bellek özel durumu anlamına gelmez. Bu, Java öbek boyutu (hive.tez.java.opts) çok küçük anlamına gelir. Bellek yetersiz gördüğünüzde artırmak deneyebilirsiniz şekilde **hive.tez.java.opts**. Gerekirse artırmak gerekebilir **hive.tez.container.size**. **Java.opts** ayarı yaklaşık % 80'i olmalıdır **container.size**.

> [!NOTE]
> Ayar **hive.tez.java.opts** her zaman daha küçük olmalıdır **hive.tez.container.size**.
> 
> 

D12 makine 28 GB bellek olduğundan, bir kapsayıcı boyutu 10 GB (10240 MB) kullanın ve java.opts için % 80 atamak karar:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Yeni ayarlarla sorgu altında 10 dakika içinde başarıyla çalıştırıldı.

## <a name="next-steps"></a>Sonraki adımlar

OOM hata alma mutlaka kapsayıcı boyutu çok küçük anlamına gelmez. Bunun yerine, böylece yığın boyutu artar ve kapsayıcı bellek boyutu % 80'en az bellek ayarlarını yapılandırmanız gerekir. Hive sorguları iyileştirmek için bkz: [hdınsight'ta Hadoop için en iyi duruma getirme Hive sorguları](hdinsight-hadoop-optimize-hive-query.md).