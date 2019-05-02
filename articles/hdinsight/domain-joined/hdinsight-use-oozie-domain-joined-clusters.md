---
title: Kurumsal güvenlik paketi - Azure HDInsight ile güvenli Apache Oozie iş akışları
description: Azure HDInsight Kurumsal güvenlik paketi kullanarak güvenli Apache Oozie iş akışları. Bir Oozie iş akışının tanımlayın ve Oozie işi gönderme hakkında bilgi edinin.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 7d7fbf5d72654c26edf09ab27f024eaf39f8c387
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64709005"
---
# <a name="run-apache-oozie-in-hdinsight-hadoop-clusters-with-enterprise-security-package"></a>Apache Oozie HDInsight Hadoop kümeleri Kurumsal güvenlik paketi ile çalıştırın.

Apache Oozie, Apache Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile tümleştirilir ve aşağıdaki işler destekler:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Oozie, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanabilirsiniz.

## <a name="prerequisite"></a>Önkoşul

- Azure HDInsight Hadoop kümesi ile Kurumsal güvenlik paketi (ESP). Bkz: [yapılandırma HDInsight kümeleri ile ESP](./apache-domain-joined-configure-using-azure-adds.md).

    > [!NOTE]  
    > ESP olmayan kümelerinde Oozie kullanma hakkında ayrıntılı yönergeler için bkz. [Azure HDInsight Linux tabanlı kullanım Apache Oozie iş akışlarında](../hdinsight-use-oozie-linux-mac.md).

## <a name="connect-to-an-esp-cluster"></a>ESP bir kümeye bağlanma

Üzerinde güvenli Kabuk (SSH) daha fazla bilgi için [SSH kullanarak HDInsight (Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. SSH kullanarak HDInsight kümesine bağlanma:  
   ```bash
   ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
   ```

2. Başarılı bir Kerberos kimlik doğrulamak için `klist` komutu. Aksi takdirde, kullanın `kinit` Kerberos kimlik doğrulaması'nı başlatmak için.

3. Azure Data Lake depolamaya erişmek için gereken OAuth belirteci kaydedilecek HDInsight ağ geçidi için oturum açın:   
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Durum yanıt kodu **200 Tamam** başarılı kayıt gösterir. Gibi 401 Yetkisiz bir yanıt alınmazsa kullanıcı adı ve parolayı kontrol edin.

## <a name="define-the-workflow"></a>İş akışı tanımlama
Oozie iş akışı tanımları, Apache Hadoop işlem tanım dili (hPDL) yazılır. hPDL XML işlem tanımı dilidir. İş akışını tanımlamak için aşağıdaki adımları uygulayın:

1. Bir etki alanı kullanıcısının çalışma ayarlayın:
   ```bash
   hdfs dfs -mkdir /user/<DomainUser>
   cd /home/<DomainUserPath>
   cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
   tar -xvf oozie-examples.tar.gz
   hdfs dfs -put examples /user/<DomainUser>/
   ```
   Değiştirin `DomainUser` ile etki alanı kullanıcı adı. 
   Değiştirin `DomainUserPath` ile etki alanı kullanıcısı için giriş dizin yolu. 
   Değiştirin `ClusterVersion` , küme Hortonworks Data Platform (HDP) sürümüne sahip.

2. Yeni bir dosya oluşturup aşağıdaki deyimi kullanın:
   ```bash
   nano workflow.xml
   ```

3. Nano Düzenleyici açıldıktan sonra aşağıdaki XML dosyanın içeriği girin:
   ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://hn0-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
   ```
4. Değiştirin `clustername` ile kümesinin adı. 

5. Dosyayı kaydetmek için Ctrl + X'i seçin. `Y` yazın. Ardından **Enter**.

    İş akışı, iki bölüme ayrılır:
   * **Kimlik bilgisi bölümü.** Bu bölümde, Oozie eylemleri kimlik doğrulaması için kullanılan kimlik bilgilerini alır:

     Bu örnek Hive eylemler için kimlik doğrulaması kullanır. Daha fazla bilgi için bkz. [eylem kimlik doğrulaması](https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

     Oozie eylemleri Hadoop Hizmetleri erişmek için kullanıcı bürünülecek kimlik bilgisi hizmet sağlar.

   * **Eylem bölümü.** Bu bölümde üç eylem vardır: map-reduce, Hive server 2 ve 1 Hive sunucusu:

     - Toplanan sözcük sayısını veren için Oozie paketinden bir örnek map-reduce eylemi çalıştığında harita azaltın.

     - Hive server 2 ve Hive sunucusu 1 eylemleri, HDInsight ile sağlanan örnek Hive tablosunda bir sorgu çalıştırın.

     Hive eylemleri anahtar sözcüğünü kullanarak kimlik doğrulaması için kimlik bilgileri bölümünde tanımlanan kimlik bilgilerini kullanan `cred` eylem öğesindeki.

6. Kopyalamak için aşağıdaki komutu kullanın `workflow.xml` dosyasını `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

7. Değiştirin `domainuser` etki alanı adınızı içeren.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Oozie iş için özellikler dosyasını tanımlama

1. Proje özellikleri için yeni bir dosya oluşturup aşağıdaki deyimi kullanın:

   ```bash
   nano job.properties
   ```

2. Nano Düzenleyici açıldıktan sonra aşağıdaki XML dosyasının içeriği kullanın:

   ```bash
       nameNode=adl://home
       jobTracker=headnodehost:8050
       queueName=default
       examplesRoot=examples
       oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
       hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
       hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
       oozie.use.system.libpath=true
       user.name=[domainuser]
       jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
       jdbcURL=[jdbcurlvalue]
       hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
       hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
   ```

   * Kullanım `adl://home` URI'sini `nameNode` , birincil küme depolama alanı olarak Azure Data Lake depolama Gen1 varsa özelliği. Azure Blob Depolama kullanıyorsanız, bu değişiklik `wasb://home`. Azure Data Lake depolama Gen2 kullanıyorsanız, bu değişiklik `abfs://home`.
   * Değiştirin `domainuser` etki alanı adınızı içeren.  
   * Değiştirin `ClusterShortName` kümenin kısa adı ile. Örneğin, küme adı https:// ise *[örnek bağlantı]* sechadoopcontoso.azurehdisnight.net, `clustershortname` kümenin ilk altı karakterdir: **sechad**.  
   * Değiştirin `jdbcurlvalue` Hive yapılandırmasından JDBC URL. Jdbc:hive2 örneğidir: / / headnodehost:10001 /; transportMode = http.      
   * Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

   Bu özellikler dosyası yerel olarak Oozie işleri çalıştırırken mevcut olması gerekir.

## <a name="create-custom-hive-scripts-for-oozie-jobs"></a>Özel Hive betiklerini Oozie işleri oluşturma

Hive server 1 ve Hive server 2 Aşağıdaki bölümlerde gösterildiği gibi iki Hive betik oluşturabilirsiniz.

### <a name="hive-server-1-file"></a>Hive server 1 dosyası

1.  Oluşturun ve Hive sunucusu 1 eylemler için bir dosyayı düzenleyin:
    ```bash
    nano countrowshive1.hql
    ```

2.  Komut dosyası oluşturun:
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Apache Hadoop dağıtılmış dosya sistemi (HDFS için) dosyayı kaydedin:
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

### <a name="hive-server-2-file"></a>Hive server 2 dosyası

1.  Oluşturma ve Hive server 2 eylemler için bir alanı düzenleme:
    ```bash
    nano countrowshive2.hql
    ```

2.  Komut dosyası oluşturun:
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  HDFS'ye dosyayı kaydedin:
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submit-oozie-jobs"></a>Oozie işlerini gönderme

ESP kümeleri için Oozie işlerini göndermenin ESP olmayan kümelerinde Oozie işleri gönderme gibi olur.

Daha fazla bilgi için [tanımlamak ve Linux tabanlı Azure HDInsight üzerinde bir iş akışı çalıştırmak için Apache Hadoop ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-an-oozie-job-submission"></a>Bir Oozie iş gönderme sonuçları
Oozie işleri, kullanıcı için çalıştırılır. Bu nedenle hem Apache Hadoop YARN ve Apache Ranger günlükleri göster başkasının kimliğine bürünülerek gerçekleştirilen bir kullanıcı olarak çalıştırılan işler denetim. Oozie iş komut satırı arabirimi çıktısı şu kod gibi görünür:



```bash
    Job ID : 0000015-180626011240801-oozie-oozi-W
    ------------------------------------------------------------------------------------------------
    Workflow Name : map-reduce-wf
    App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
    Status        : SUCCEEDED
    Run           : 0
    User          : alicetest
    Group         : -
    Created       : 2018-06-26 19:25 GMT
    Started       : 2018-06-26 19:25 GMT
    Last Modified : 2018-06-26 19:30 GMT
    Ended         : 2018-06-26 19:30 GMT
    CoordAction ID: -
    
    Actions
    ------------------------------------------------------------------------------------------------
    ID                      Status  Ext ID          ExtStatus   ErrCode
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
    -----------------------------------------------------------------------------------------------
```

Hive server 2 Eylemler Ranger denetim günlüklerini çalıştıran kullanıcı için eylem Oozie gösterir. Ranger ve YARN görünümleri yalnızca Küme Yöneticisi için görünür

## <a name="configure-user-authorization-in-oozie"></a>Kullanıcı yetkilendirmesi Oozie yapılandırın

Oozie kendisi tarafından kullanıcıların durdurma veya diğer kullanıcıların işlerini silme engelleyebilen bir kullanıcı yetkilendirme yapılandırması vardır. Bu yapılandırma etkinleştirmek için `oozie.service.AuthorizationService.security.enabled` için `true`. 

Daha fazla bilgi için [Apache Oozie yükleme ve yapılandırma](https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html).

Hive server burada eklenti Ranger desteklenen veya kullanılabilir değil 1 gibi bileşenleri için parçalı HDFS yetkilendirme mümkündür. Ayrıntılı yetkilendirme yalnızca Ranger eklentileri kullanılabilir.

## <a name="get-the-oozie-web-ui"></a>Get the Oozie web UI

Oozie web kullanıcı Arabirimi, küme üzerinde Oozie işlerin durumunu web tabanlı bir görünüm sağlar. Web kullanıcı Arabirimi almak için ESP kümeleri, aşağıdaki adımları uygulayın:

1. Ekleme bir [kenar düğümüne](../hdinsight-apps-use-edge-node.md) ve etkinleştirme [SSH Kerberos kimlik doğrulaması](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. İzleyin [Oozie web kullanıcı Arabirimi](../hdinsight-use-oozie-linux-mac.md) kenar düğümüne SSH tüneli etkinleştirmek ve web kullanıcı Arabirimine erişip adımları.

## <a name="next-steps"></a>Sonraki adımlar
* [Tanımlamak ve Linux tabanlı Azure HDInsight üzerinde bir iş akışı çalıştırmak için Apache Hadoop ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md).
* [SSH kullanarak HDInsight için (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
