---
title: HDInsight etki alanına katılmış kümeleri Hadoop Oozie iş akışları
description: Kullanım Hadoop Oozie Linux tabanlı HDInsight etki alanına katılmış Kurumsal güvenlik paketi. Bir Oozie iş akışının tanımlayın ve Oozie işi gönderme hakkında bilgi edinin.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 9e5b7303830f2064f764d2de023b4a3ff9b0ea9f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450430"
---
#<a name="run-apache-oozie-in-domain-joined-hdinsight-hadoop-clusters"></a>Apache Oozie etki alanına katılmış HDInsight Hadoop kümelerinde çalıştırma
Oozie, Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile tümleştirilir ve aşağıdaki işler destekler:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Oozie, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanabilirsiniz.

##<a name="prerequisites"></a>Ön koşullar:
- Bir etki alanına katılmış HDInsight Hadoop kümesi. Bkz: [etki alanına katılmış HDInsight kümeleri](./apache-domain-joined-configure-using-azure-adds.md)

    > [!NOTE]
    > Etki alanı olmayan üzerinde Oozie kullanma katılmış ayrıntılı yönergeleri görmek için kümeleri bakın [bu](../hdinsight-use-oozie-linux-mac.md)

##<a name="connecting-to-domain-joined-cluster"></a>Bağlanan etki alanına katılmış kümeye
SSH hakkında daha fazla bilgi için bkz. [kimlik doğrulama: etki alanına katılmış HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
- SSH kullanarak HDInsight kümesine bağlanma:
     ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```
Kerberos kimlik doğrulaması başarılı olup olmadığını doğrulamak için `klist` komutu. Aksi takdirde, kullanın `kinit` Kerberos kimlik doğrulaması'nı başlatmak için.

- HDInsight ağ geçidi, Azure Data Lake Store (ADLS) erişmek için gereken oAuth belirteci kaydetmek için oturum açın
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Durum yanıt kodu _200 Tamam_ başarılı kayıt gösterir. Kullanıcı adı ve parola için alınan (401) yetkisiz bir yanıt olup olmadığını kontrol edin.

## <a name="define-the-workflow"></a>İş akışı tanımlama
Oozie iş akışı tanımları Hadoop işlem tanımı bir XML işlem tanımı dili olan dilinde (hPDL) yazılır. İş akışını tanımlamak için aşağıdaki adımları kullanın:

-   Etki alanı kullanıcının çalışma alanını ayarlama:
 ```bash
hdfs dfs -mkdir /user/<DomainUser>
cd /home/<DomainUserPath>
cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
tar -xvf oozie-examples.tar.gz
hdfs dfs -put examples /user/<DomainUser>/
 ```
Değiştirin `DomainUser` ile etki alanı kullanıcı adı. Değiştirin `DomainUserPath` ile etki alanı kullanıcısı için giriş dizin yolu. Değiştirin `ClusterVersion` küme HDP sürümünüzün ile.

-   Yeni bir dosya oluşturup aşağıdaki deyimi kullanın:
 ```bash
nano workflow.xml
 ```

- Nano Düzenleyici açıldıktan sonra aşağıdaki XML dosyanın içeriği girin:
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
Değiştirin `clustername` ile kümesinin adı. 

Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

İş akışı, iki bölüme ayrılır:
*   Kimlik bilgisi bölümü: Kimlik bilgisi bölümü oozie eylemleri kimlik doğrulaması için kullanılacak kimlik bilgilerini alır:

    Bu örnekte, Hive eylemler için kimlik doğrulaması kullanılır. Daha fazla bilgi için bkz. [bu]( https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

    Oozie eylemleri Hadoop Hizmetleri erişmek için kullanıcı bürünülecek kimlik bilgisi hizmet sağlar.

*   Eylem bölümü: Üç eylem burada map reduce, hive server 2 eylem ve hive sunucusu 1 eylem vardır:

    Map-reduce çalıştırmalar için oozie paketinden bir örnek bir harita-toplanan sözcük sayısını çıkarır, azaltın.

    Hive ve hive server 2 sunucu 1 eylemleri, HDInsight ile sağlanan hivesample tablosunda basit bir sorgu yürütür.

    Hive eylemleri anahtar sözcüğü kullanılarak kimlik doğrulaması için kimlik bilgileri bölümünde tanımlanan kimlik bilgilerini kullanan `cred` eylem öğesindeki

- Kopyalamak için aşağıdaki komutu kullanın `workflow.xml` dosyasını `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

    Değiştirin `domainuser` etki alanı adınızı içeren.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Oozie iş için özellikler dosyasını tanımlama

1.  Proje özellikleri için yeni bir dosya oluşturup aşağıdaki deyimi kullanın:
     ```bash
    nano job.properties
     ```

2.   Nano Düzenleyici açıldıktan sonra aşağıdaki XML dosyasının içeriği kullanın:

    ```bash
        nameNode=adl://home
        jobTracker=headnodehost:8050
        queueName=default
        examplesRoot=examples
        oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
        hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
        hiveScript2=${nameNode}/user/${user.name}/countrowshive1.hql
        oozie.use.system.libpath=true
        user.name=[domainuser]
        jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
        jdbcURL=[jdbcurlvalue]
        hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
        hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
    ```
    

   * Değiştirin `domainuser` etki alanı adınızı içeren.
   * Değiştirin `ClusterShortName` kümenin shortname ile. Clustername ise https://sechadoopcontoso.azurehdisnight.net, `clustershortname` kümenin ilk altı harf olduğundan: sechad
   * Değiştirin `jdbcurlvalue` yapılandırmadan hive JDBC URL. Örneğin, jdbc:hive2: / / headnodehost:10001 /; transportMode = http.
    
   * Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

   * Bu özellikler dosyası yerel olarak oozie işleri çalıştırırken mevcut olması gerekir

## <a name="creating-custom-hive-scripts-for-the-oozie-job"></a>Özel hive betiklerini Oozie iş oluşturma
Hive sunucusu 1 ve 2 hive server 2 hive betiklerini aşağıdaki gibi oluşturulabilir:
-   Hive Server 1'dosyası:
1.  Hive sunucusu 1 eylem için bir dosya oluşturup aşağıdaki deyimi kullanın:
    ```bash
    nano countrowshive1.xml
    ```

2.  Betik oluşturma
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Dosyayı kaydetmek için Hdfs
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

-   Hive Server 2 dosyası:
1.  Aşağıdaki deyim, oluşturmak ve bir alan için hive server 2 eylemi düzenlemek için kullanın:
    ```bash
    nano countrowshive2.xml
    ```

2.  Betik oluşturma
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Dosyayı kaydetmek için Hdfs
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submission-of-oozie-jobs"></a>Oozie işlerinin gönderimi
Etki alanına katılmış kümeleri için oozie işlerini gönderme, etki alanına katılmış kümeleri oozie işleri göndermek için benzerdir.

Daha fazla bilgi için lütfen bkz. [gönderin ve iş yönetimi](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-oozie-job-submission"></a>Oozie iş gönderme sonuçları
Oozie işleri adına kullanıcı çalıştırıldığından Yarn hem Ranger günlükleri göster başkasının kimliğine bürünülerek gerçekleştirilen bir kullanıcı olarak yürütülmekte olan işleri denetim. Oozie iş CLI çıktısı gibi aşağıda görünür:


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

*    Ranger denetim için hive server 2 eylemleri gösterir oozie kullanıcı adına bir eylemi yürütmede günlüğe kaydeder. Ranger ve Yarn görünümdür yalnızca Küme Yöneticisi için görünür

## <a name="configuration-of-user-authorization-in-oozie"></a>Kullanıcı yetkilendirme Oozie yapılandırması
Oozie kendisi tarafından durdurma, kullanıcıların başka kullanıcının işlerini sonlandırma engelleyebilirsiniz bir kullanıcı kimlik doğrulaması yapılandırması vardır. Bu ayarlayarak etkin `oozie.service.AuthorizationService.security.enabled` için `true`. 

Bunun hakkında daha fazla ayrıntı Oozie belgeleri bölümü - olarak bulunabilir [kullanıcı yetkilendirme Yapılandırması]( https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html):

Hive server 1 Ranger eklentisi kullanılabilir/desteklenen olduğu gibi bileşenler için yalnızca parçalı hdfs yetkilendirme mümkündür. İnce tanecikli yetkilendirme yalnızca ranger eklentileri kullanılabilir.

## <a name="oozie-web-ui"></a>Oozie web kullanıcı Arabirimi
Oozie web kullanıcı Arabirimi, küme üzerinde Oozie işlerin durumunu web tabanlı bir görünüm sağlar. Etki alanına katılmış kümeleri için ihtiyacınız:

1. Ekleme bir [kenar düğümüne](../hdinsight-apps-use-edge-node.md) ve etkinleştirme [SSH Kerberos kimlik doğrulaması](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. İzleyin [Oozie web kullanıcı Arabirimi](../hdinsight-use-oozie-linux-mac.md) kenar düğümüne SSH tüneli etkinleştirmek ve web kullanıcı Arabirimine erişip adımları.

## <a name="next-steps"></a>Sonraki adımlar
* [Tanımlamak ve Linux tabanlı Azure HDInsight üzerinde bir iş akışı çalıştırmak için Hadoop ile Oozie kullanma](../hdinsight-use-oozie-linux-mac.md)
* [Zaman tabanlı Oozie Düzenleyici kullanma](../hdinsight-use-oozie-coordinator-time.md)
* [Etki alanına katılmış HDInsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
