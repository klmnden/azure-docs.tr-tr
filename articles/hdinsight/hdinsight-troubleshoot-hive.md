---
title: Azure HDInsight'ı kullanarak Hive sorunlarını giderme
description: Apache Hive ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
keywords: Azure HDInsight, Hive, SSS, sorun giderme kılavuzu, sık sorulan sorular
services: hdinsight
ms.service: hdinsight
author: dharmeshkakadia
ms.author: dharmeshkakadia
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 832fab6c4f183ddad512c5e6e4309d70938a316b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600032"
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Hive sorunlarını giderme

Apache Ambari, Apache Hive yükü ile çalışırken en çok sorulan sorular ve bunların çözümleri hakkında bilgi edinin.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Nasıl bir Hive meta veri deposu dışarı aktarma ve bunu başka bir kümede de içeri?


### <a name="resolution-steps"></a>Çözüm adımları

1. Güvenli Kabuk (SSH) istemcisi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için [ek okuma](#additional-reading-end).

2. Meta veri deposu vermek istediğiniz HDInsight kümesinde aşağıdaki komutu çalıştırın:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Bu komut, allatables.sql adlı bir dosya oluşturur.

3. Yeni HDInsight kümesi için dosya alltables.sql kopyalayın ve ardından aşağıdaki komutu çalıştırın:

  ```apache
  hive -f alltables.sql
  ```

Çözüm adımları kodda veri yolu yeni kümedeki veri yolları eski kümedeki ile aynı olduğunu varsayar. Veri yolu farklı ise, oluşturulan alltables.sql dosya değişiklikleri yansıtacak şekilde el ile düzenleyebilirsiniz.

### <a name="additional-reading"></a>Ek okuma

- [Bir HDInsight kümesine SSH kullanarak bağlanın.](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hive günlükleri bir kümeye nasıl bulabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. HDInsight kümesine SSH kullanarak bağlanın. Daha fazla bilgi için **ek okuma**.

2. Hive istemci günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. Hive meta veri deposu günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. Hiveserver günlükleri görüntülemek için aşağıdaki komutu kullanın:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>Ek okuma

- [Bir HDInsight kümesine SSH kullanarak bağlanın.](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Nasıl bir kümede belirli yapılandırmalar ile Hive kabuğunu Başlat?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hive kabuğunu başlattığınızda bir yapılandırma anahtar-değer çifti belirtin. Daha fazla bilgi için [ek okuma](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. Tüm etkin yapılandırmaları üzerinde Hive kabuğunu listelemek için aşağıdaki komutu kullanın:

  ```apache
  hive> set;
  ```

  Örneğin, Hive kabuğunu konsolda etkin hata ayıklama günlüğü başlatmak için aşağıdaki komutu kullanın:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>Ek okuma

- [Hive yapılandırma özellikleri](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Tez DAG veri kümesi kritik yol üzerindeki nasıl analiz?


### <a name="resolution-steps"></a>Çözüm adımları
 
1. Küme kritik bir grafikte bir Apache Tez yönlendirilmiş Çevrimsiz graf (DAG) çözümlemek için HDInsight kümesine SSH kullanarak bağlanın. Daha fazla bilgi için [ek okuma](#additional-reading-end).

2. Bir komut isteminde aşağıdaki komutu çalıştırın:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. Tez DAG çözümlemek için kullanılan diğer Çözümleyicileri listelemek için aşağıdaki komutu kullanın:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  İlk bağımsız değişken olarak bir örnek program sağlamanız gerekir.

  Geçerli program adları şunlardır:
    - **ContainerReuseAnalyzer**: yazdırma DAG kapsayıcı yeniden ayrıntıları
    - **CriticalPath**: DAG kritik yolunu bulun
    - **LocalityAnalyzer**: yazdırma DAG ayrıntılarında yerleşim yeri
    - **ShuffleTimeAnalyzer**: dag'deki shuffle zaman ayrıntılarını analiz etme
    - **SkewAnalyzer**: dag'deki eğriltme ayrıntılarını analiz etme
    - **SlowNodeAnalyzer**: yazdırma bir DAG düğümüne ayrıntıları
    - **SlowTaskIdentifier**: DAG yazdırma yavaş görev ayrıntıları
    - **SlowestVertexAnalyzer**: en yavaş köşe ayrıntılarını dag'deki yazdırma
    - **SpillAnalyzer**: DAG yazdırma Saçılma ayrıntıları
    - **TaskConcurrencyAnalyzer**: DAG görev eşzamanlılık ayrıntılarında yazdırma
    - **VertexLevelCriticalPathAnalyzer**: kritik yol köşe düzeyinde DAG bulun


### <a name="additional-reading"></a>Ek okuma

- [Bir HDInsight kümesine SSH kullanarak bağlanın.](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Tez DAG veri bir kümeden nasıl indiririm?


#### <a name="resolution-steps"></a>Çözüm adımları

Tez DAG verileri toplamak için iki yolu vardır:

- Komut satırından:
 
    HDInsight kümesine SSH kullanarak bağlanın. Komut isteminde aşağıdaki komutu çalıştırın:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Ambari Tez görünümünü kullanın:
   
  1. Ambarı'na gidin. 
  2. (Kutucuklar simgesi sağ üst köşedeki altında) Tez görünümüne gidin. 
  3. Görüntülemek istediğiniz DAG seçin.
  4. Seçin **indirme veri**.

### <a name="additional-reading-end"></a>Ek okuma

[Bir HDInsight kümesine SSH kullanarak bağlanın.](hdinsight-hadoop-linux-use-ssh-unix.md)


### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](hdinsight-troubleshoot-guide.md)




