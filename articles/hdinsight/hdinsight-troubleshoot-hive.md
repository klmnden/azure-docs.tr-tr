---
title: "Azure Hdınsight ile Hive giderme | Microsoft Docs"
description: "Apache Hive ve Azure Hdınsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın."
keywords: "Azure Hdınsight, Hive, SSS, sorun giderme kılavuzu, sık sorulan sorular"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: 53e9685458190efe6a586504721b8e7baadaed60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Hive Azure Hdınsight kullanarak sorun giderme

Üst sorular ve bunların çözümleri hakkında Apache Ambari, Apache Hive yükü ile çalışırken öğrenin.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Nasıl bir Hive meta depo dışarı aktarma ve başka bir kümede alma


### <a name="resolution-steps"></a>Çözüm adımları

1. Bir güvenli Kabuk (SSH) istemcisi kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz: [ek okuma](#additional-reading-end).

2. Meta depo vermek istediğiniz Hdınsight kümesinde aşağıdaki komutu çalıştırın:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Bu komut, allatables.sql adlı bir dosya oluşturur.

3. Yeni Hdınsight kümesine dosya alltables.sql kopyalayın ve ardından şu komutu çalıştırın:

  ```apache
  hive -f alltables.sql
  ```

Çözüm adımları kodda, veri yolu yeni kümede veri yolları eski kümedeki ile aynı olduğunu varsayar. Veri yolları farklıysa, el ile oluşturulan alltables.sql dosya değişiklikleri yansıtacak şekilde düzenleyebilirsiniz.

### <a name="additional-reading"></a>Ek kaynaklar

- [SSH kullanarak bir Hdınsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Bir kümede nasıl Hive günlüklerini bulun

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz: **ek okuma**.

2. Hive istemci günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. Hive meta depo günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. Hiveserver günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>Ek kaynaklar

- [SSH kullanarak bir Hdınsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Bir kümede belirli yapılandırmalarla Hive kabuğunu nasıl başlatma

### <a name="resolution-steps"></a>Çözüm adımları

1. Hive kabuğunu başlattığınızda yapılandırma anahtar-değer çifti belirtin. Daha fazla bilgi için bkz: [ek okuma](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. Tüm etkin yapılandırmaları Hive kabuğunu üzerinde listelemek için aşağıdaki komutu kullanın:

  ```apache
  hive> set;
  ```

  Örneğin, konsolda etkin hata ayıklama günlüğünü ile Hive kabuğunu başlatmak için aşağıdaki komutu kullanın:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>Ek kaynaklar

- [Hive yapılandırma özellikleri](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Küme kritik yolunda Tez DAG verileri nasıl çözümlemek


### <a name="resolution-steps"></a>Çözüm adımları
 
1. Bir küme kritik grafikte bir Apache Tez yönlendirilmiş Çevrimsiz grafik (DAG) çözümlemek için SSH kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz: [ek okuma](#additional-reading-end).

2. Bir komut isteminde aşağıdaki komutu çalıştırın:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. Tez DAG çözümlemek için kullanılan diğer çözümleyiciler listelemek için aşağıdaki komutu kullanın:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  İlk bağımsız değişkeni olarak bir örnek program sağlamanız gerekir.

  Geçerli program adları şunlardır:
    - **ContainerReuseAnalyzer**: yazdırma bir kapsayıcı yeniden ayrıntıları
    - **CriticalPath**: bir dag'nin kritik yolu bulunamadı
    - **LocalityAnalyzer**: yazdırma bir yere göre ayrıntıları
    - **ShuffleTimeAnalyzer**: karışık saati ayrıntıları bir çözümleme
    - **SkewAnalyzer**: bir eğme ayrıntıları Çözümle
    - **SlowNodeAnalyzer**: yazdırma bir düğüm ayrıntıları
    - **SlowTaskIdentifier**: bir yazdırma yavaş görev ayrıntıları
    - **SlowestVertexAnalyzer**: yazdırma bir yavaş köşe ayrıntıları
    - **SpillAnalyzer**: bir Karmadaki ayrıntılarını yazdırma
    - **TaskConcurrencyAnalyzer**: bir görev eşzamanlılık ayrıntılarını yazdırma
    - **VertexLevelCriticalPathAnalyzer**: bir DAG köşe düzeyinde kritik yolu bulunamadı


### <a name="additional-reading"></a>Ek kaynaklar

- [SSH kullanarak bir Hdınsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Bir kümeden nasıl Tez DAG veri karşıdan


#### <a name="resolution-steps"></a>Çözüm adımları

Tez DAG verileri toplamak için iki yolu vardır:

- Komut satırından:
 
    SSH kullanarak Hdınsight kümesine bağlanın. Komut isteminde aşağıdaki komutu çalıştırın:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Ambari Tez görünümünü kullanın:
   
  1. Ambarı'na gidin. 
  2. (Altında döşeme simgesine sağ üst köşesinde) Tez görünümüne gidin. 
  3. Görüntülemek istediğiniz DAG seçin.
  4. Seçin **karşıdan veri**.

### <a name="additional-reading-end"></a>Ek kaynaklar

[SSH kullanarak bir Hdınsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)






