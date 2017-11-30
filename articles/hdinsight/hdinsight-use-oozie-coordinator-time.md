---
title: "Hdınsight'ta Hadoop Oozie Düzenleyicisi zamana dayalı kullanın | Microsoft Docs"
description: "Hdınsight, büyük veri hizmeti zamana dayalı Hadoop Oozie düzenleyicisi kullanın. Oozie iş akışları ve düzenleyiciler tanımlayın ve işleri gönderme hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 0fa8e3630610913d909a75bf76236d120c8f1a2b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>İş akışları tanımlamak ve işleri koordine etmek için hdınsight'ta Hadoop ile zamana dayalı Oozie düzenleyicisi kullanın
Bu makalede, iş akışları ve düzenleyiciler nasıl tanımlanacağı ve zamana dayalı Düzenleyicisi işleri tetiklemek nasıl öğreneceksiniz. Geçtikleri faydalıdır [Hdınsight ile kullanım Oozie] [ hdinsight-use-oozie] önce bu makaleyi okuyun. Oozie ek olarak, Azure Data Factory kullanarak işleri de zamanlayabilirsiniz. Azure Data Factory öğrenmek için bkz: [kullanım Pig ve Hive Data Factory ile](../data-factory/transform-data.md).

> [!NOTE]
> Bu makalede Windows tabanlı Hdınsight kümesi gerektirir. Oozie, Linux tabanlı bir kümede zaman tabanlı işleri de dahil olmak üzere kullanma hakkında bilgi için bkz: [tanımlamak ve Linux tabanlı Hdınsight üzerinde bir iş akışını çalıştırmak için Hadoop ile kullanım Oozie](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Oozie nedir
Apache Oozie, Hadoop işlerini yöneten bir iş akışı/koordinasyon sistemidir. Bu Hadoop yığını ile tümleşiktir ve Apache MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Aşağıdaki resimde, gerçekleştireceksiniz iş akışı gösterilmiştir:

![İş akışı diyagramı][img-workflow-diagram]

İş akışı iki eylemleri içerir:

1. Hive eylem her günlük düzeyi türü log4j günlük dosyasında oluşumları saymak için HiveQL betiğini çalıştırır. Her log4j günlük türünün ve önem örneğin göstermek için [günlük düzeyi] alan içeren bir dizi alanlarının oluşur:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Hive betiği çıkış benzer:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma][hdinsight-use-hive].
2. Sqoop eylem HiveQL eylem çıkışı bir Azure SQL veritabanındaki bir tablo dışa aktarır. Sqoop hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanım Sqoop][hdinsight-use-sqoop].

> [!NOTE]
> Hdınsight kümelerinde desteklenen Oozie sürümleri için bkz: [Hdınsight tarafından sağlanan küme sürümlerindeki yenilikler nelerdir?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure PowerShell içeren bir iş istasyonu**.

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışıdır** ve 1 Ocak 2017 tarihine kadar kaldırılacaktır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

* **Hdınsight kümesi**. Hdınsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight kümeleri oluşturma][hdinsight-provision], veya [Hdınsight kullanmaya başlama][hdinsight-get-started]. Öğreticiyi incelemek için aşağıdaki veriler gerekir:

    <table border = "1">
    <tr><th>Küme özelliği</th><th>Windows PowerShell değişken adı</th><th>Değer</th><th>Açıklama</th></tr>
    <tr><td>Hdınsight küme adı</td><td>$clusterName</td><td></td><td>Bu öğretici çalışacağı Hdınsight kümesi.</td></tr>
    <tr><td>Hdınsight küme kullanıcı</td><td>$clusterUsername</td><td></td><td>Hdınsight küme kullanıcı adı. </td></tr>
    <tr><td>Hdınsight küme kullanıcı parolası </td><td>$clusterPassword</td><td></td><td>Hdınsight küme kullanıcı parolası.</td></tr>
    <tr><td>Azure depolama hesabı adı</td><td>$storageAccountName</td><td></td><td>Bir Azure depolama hesabı Hdınsight küme için kullanılabilir. Bu öğretici için küme sağlama işlemi sırasında belirtilen varsayılan depolama hesabı kullanın.</td></tr>
    <tr><td>Azure Blob kapsayıcı adı</td><td>$containerName</td><td></td><td>Bu örnekte, varsayılan Hdınsight küme dosya sistemi için kullanılan Azure Blob Depolama kapsayıcısını kullanın. Varsayılan olarak, Hdınsight kümesi ile aynı ada sahip.</td></tr>
    </table>

* **Bir Azure SQL veritabanı**. İş istasyonunuzu erişime izin verecek şekilde SQL veritabanı sunucusu için bir güvenlik duvarını yapılandırmanız gerekir. Bir Azure SQL veritabanı oluşturma ve Güvenlik Duvarı'nı yapılandırma hakkında yönergeler için bkz: [Azure SQL veritabanını kullanmaya başlama][sqldatabase-get-started]. Bu makalede, Bu öğretici için gereksinim duyduğunuz Azure SQL veritabanı tablosu oluşturmak için bir Windows PowerShell komut dosyası sağlar.

    <table border = "1">
    <tr><th>SQL veritabanı özelliği</th><th>Windows PowerShell değişken adı</th><th>Değer</th><th>Açıklama</th></tr>
    <tr><td>SQL veritabanı sunucusu adı</td><td>$sqlDatabaseServer</td><td></td><td>SQL veritabanı sunucusuna Sqoop verileri dışa aktarır. </td></tr>
    <tr><td>SQL veritabanı oturum açma adı</td><td>$sqlDatabaseLogin</td><td></td><td>SQL veritabanı oturum açma adı.</td></tr>
    <tr><td>SQL veritabanı oturum açma parolası</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL veritabanı oturum açma parolası.</td></tr>
    <tr><td>SQL veritabanı adı</td><td>$sqlDatabaseName</td><td></td><td>Azure SQL veritabanı Sqoop verileri dışa aktarır. </td></tr>
    </table>

  > [!NOTE]
  > Varsayılan olarak, Azure Hdınsight gibi Azure hizmetlerinden bağlantıları bir Azure SQL veritabanı sağlar. Bu güvenlik duvarı ayarı devre dışıysa, Azure Portalı'ndan etkinleştirmeniz gerekir. Bir SQL veritabanı oluşturma ve güvenlik duvarı kuralları yapılandırma hakkında daha fazla yönerge için bkz: [oluşturma ve yapılandırma SQL veritabanı][sqldatabase-get-started].

> [!NOTE]
> Doldurma tablolardaki değerleri. Bu öğreticide olmaya yardımcı olacaktır.

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie iş akışı ve ilgili HiveQL betiğini tanımlayın
Oozie iş akışı tanımları hPDL (bir XML işlem tanım dili) yazılır. Varsayılan iş akışı dosya adı *workflow.xml*.  İş akışı dosyasını yerel olarak kaydedin ve ardından daha sonra Bu öğreticide Azure PowerShell kullanarak Hdınsight kümesine dağıtın.

İş akışı Hive eylemde HiveQL komut dosyasını çağırır. Bu komut dosyasını üç HiveQL ifadelerini içerir:

1. **DROP TABLE deyimi** varsa log4j Hive tablosu siler.
2. **CREATE TABLE deyimi** log4j günlük dosyasının; konumuna işaret eden bir log4j Hive dış tablosu oluşturur
3. **Log4j günlük dosyasının konumunu**. Alan sınırlayıcı ",". Varsayılan satır ayırıcı "\n" dir. Hive dış tablo durumunda, birden çok kez Oozie iş akışını çalıştırmak istediğiniz veri dosyasındaki özgün konumundan kaldırılmasını önlemek için kullanılır.
4. **INSERT üzerine deyimi** sayıları her günlük düzeyi türü log4j Hive tablosu ve oluşumlarını kaydeder çıkışı bir Azure Blob depolama konumuna.

> [!NOTE]
> Bilinen bir Hive yolu sorun yoktur. Bu sorunla Oozie iş gönderirken çalışır. Sorunu düzeltmek için yönergeleri TechNet Wiki'de bulunabilir: [Hdınsight Hive hata: yeniden adlandırılamıyor][technetwiki-hive-error].

**İş akışı tarafından çağrılacak HiveQL komut dosyasını tanımlamak için**

1. Aşağıdaki içerik ile bir metin dosyası oluşturun:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Komut dosyasında kullanılan üç değişkenleri şunlardır:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     İş akışı tanımı dosyası (Bu öğreticide workflow.xml) bu değerleri bu HiveQL betiğini çalışma zamanında geçer.
2. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\useooziewf.hql** ANSI (ASCII) kodlama kullanılarak. (Bu seçenek, metin düzenleyici sağlamıyorsa Notepad kullanın.) Bu komut dosyası Hdınsight kümesine daha sonra öğreticide dağıtılır.

**Bir iş akışı tanımlamak için**

1. Aşağıdaki içerik ile bir metin dosyası oluşturun:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    İş akışında tanımlanan iki eylemler vardır. Başlangıç için eylem *RunHiveScript*. Eylem çalıştırıyorsa *Tamam*, bir sonraki eylem *RunSqoopExport*.

    RunHiveScript birkaç değişkeni yok. Azure PowerShell kullanarak Oozie iş istasyonunuzdan gönderdiğinizde değerler geçer.

    İş akışı değişkenleri

    <table border = "1">
    <tr><th>İş akışı değişkenleri</th><th>Açıklama</th></tr>
    <tr><td>${Jobtracker'a}</td><td>Hadoop işi İzleyicisi URL'sini belirtin. Kullanım <strong>jobtrackerhost:9010</strong> Hdınsight sürüm 3.0 ve 2.0 küme.</td></tr>
    <tr><td>${İş}</td><td>Hadoop adı düğümü URL'sini belirtin. Varsayılan dosya sistemi wasb kullanın: / / adres, örneğin, <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>İş için gönderilen sıra adı belirtir. Kullanım <strong>varsayılan</strong>.</td></tr>
    </table>

    Hive eylem değişkenleri

    <table border = "1">
    <tr><th>Eylem değişkeni yığını</th><th>Açıklama</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Create Table Hive komutu için kaynak dizini.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Çıkış klasörüne eklemek üzerine deyimi için.</td></tr>
    <tr><td>${hiveTableName}</td><td>Log4j veri dosyalarına başvuran Hive tablosu adı.</td></tr>
    </table>

    Sqoop eylem değişkenleri

    <table border = "1">
    <tr><th>Sqoop eylem değişkeni</th><th>Açıklama</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL veritabanı bağlantı dizesi.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Verileri dışarı burada için Azure SQL veritabanı tablosu.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Çıkış klasörüne Hive Ekle üzerine deyimi için. Bu Sqoop verme (verme-dir) için aynı klasörüdür.</td></tr>
    </table>

    Oozie iş akışı ve iş akışı eylemlerinin kullanma hakkında daha fazla bilgi için bkz: [Apache Oozie 4.0 belgelerine] [ apache-oozie-400] (için Hdınsight kümesi sürüm 3.0) veya [Apache Oozie 3.3.2 belgeleri ] [ apache-oozie-332] (için Hdınsight kümesi sürüm 2.1).

1. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\workflow.xml** ANSI (ASCII) kodlama kullanılarak. (Bu seçenek, metin düzenleyici sağlamıyorsa Notepad kullanın.)

**Düzenleyici tanımlamak için**

1. Aşağıdaki içerik ile bir metin dosyası oluşturun:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Tanım dosyasında kullanılan beş değişkenleri şunlardır:

   | Değişken | Açıklama |
   | --- | --- |
   | ${coordFrequency} |İş duraklatma süresi. Sıklık, her zaman dakika cinsinden ifade edilir. |
   | ${coordStart} |İş başlangıç zamanı. |
   | ${coordEnd} |İş bitiş saati. |
   | ${coordTimezone} |Oozie Düzenleyicisi işleri sabit bir saat diliminde hiçbir gün ışığından yararlanma saati (UTC kullanarak tipik olarak gösterilir) ile işler. Bu saat dilimi "Oozie işleme saat dilimi." olarak adlandırılır |
   | ${wfPath} |Workflow.xml yolu.  İş akışı dosyası adı varsayılan dosya adı (workflow.xml) değilse, belirtmeniz gerekir. |
2. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\coordinator.xml** ANSI (ASCII) kodlama kullanılarak. (Bu seçenek, metin düzenleyici sağlamıyorsa Notepad kullanın.)

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Oozie projeyi dağıtın ve öğretici hazırlama
Aşağıdakileri gerçekleştirmek üzere bir Azure PowerShell komut dosyası çalışır:

* Azure Blob depolama alanına HiveQL betiğini (useoozie.hql) kopyalama wasb:///tutorials/useoozie/useoozie.hql.
* Workflow.XML için wasb:///tutorials/useoozie/workflow.xml kopyalayın.
* Coordinator.XML için wasb:///tutorials/useoozie/coordinator.xml kopyalayın.
* Veri dosyasını kopyalayın (/ example/data/sample.log) wasb:///tutorials/useoozie/data/sample.log için.
* Sqoop verme verileri depolamak için bir Azure SQL veritabanı tablosu oluşturun. Tablo adı *log4jLogCount*.

**Hdınsight depolama anlama**

Hdınsight Azure Blob Depolama, veri depolaması için kullanır. wasb: / / Azure Blob depolamada Hadoop dağıtılmış dosya sistemi (HDFS) Microsoft uygulamasıdır. Daha fazla bilgi için bkz: [Azure Blob storage kullanma Hdınsight ile][hdinsight-storage].

Hdınsight kümesi sağladığınızda, Azure Blob Depolama hesabı ve bu hesaptan belirli bir kapsayıcıya tasarlanmış varsayılan dosya sistemi olarak gibi HDFS'de. Bu depolama hesabına ek olarak, ek depolama hesapları aynı Azure aboneliğinden veya farklı Azure aboneliklerinden sağlama işlemi sırasında ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz: [Hdınsight kümeleri hazırlama][hdinsight-provision]. Bu öğreticide kullanılan Azure PowerShell Betiği basitleştirmek için tüm dosyaları konumunda bulunan varsayılan dosya sistemi kapsayıcısında depolanır */öğreticileri/useoozie*. Varsayılan olarak, bu kapsayıcı Hdınsight küme adı ile aynı ada sahiptir.
Söz dizimi aşağıdaki gibidir:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> Yalnızca *wasb: / /* söz dizimi Hdınsight kümesi sürüm 3.0 desteklenir. Eski *asv: / /* söz dizimi Hdınsight 2.1 ve 1.6 kümeleri desteklenir, ancak Hdınsight 3.0 kümelerinde desteklenmez.
>
> Wasb: / / yolu olan bir sanal yol. Daha fazla bilgi için bkz: [Azure Blob storage kullanma Hdınsight ile][hdinsight-storage].

Varsayılan dosya sistemi kapsayıcısında depolanır bir dosya (workflow.xml örnek olarak kullanıyorum) aşağıdaki URI'ler birini kullanarak Hdınsight'ta erişilebilir:

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Dosyayı doğrudan depolama hesabından erişmek isterseniz, blob dosya adıdır:

    tutorials/useoozie/workflow.xml

**Hive iç ve dış tablolar anlama**

Hive iç ve dış tablolar hakkında bilmeniz gereken birkaç şey vardır:

* CREATE TABLE komutu bir iç tablosu olarak da bilinen yönetilen bir tablo oluşturur. Veri dosyası varsayılan kapsayıcısında bulunması gerekir.
* CREATE TABLE komutu veri dosyası /hive/ambarı/için taşır<TableName> varsayılan kapsayıcı klasöründe.
* Dış tablo oluşturma komut bir dış tablo oluşturur. Veri dosyasındaki varsayılan kapsayıcı bulunabilir.
* Dış Tablo Oluştur komutu veri dosyasını taşımaz.
* Dış tablo oluşturma komut konumu yan tümcesinde belirtilen klasör altındaki tüm alt izin vermez. Bu öğretici sample.log dosyasının bir kopyasını neden yapar nedenidir.

Daha fazla bilgi için bkz: [Hdınsight: Hive iç ve dış tablolar giriş][cindygross-hive-tables].

**Öğretici hazırlamak için**

1. Windows PowerShell ISE açın (Windows 8 Başlat ekranında, yazın **PowerShell_ISE**ve ardından **Windows PowerShell ISE**. Daha fazla bilgi için bkz: [Başlat Windows PowerShell Windows 8 ve Windows][powershell-start]).
2. Alt bölmede Azure aboneliğinize bağlanmak için aşağıdaki komutu çalıştırın:

    ```powershell
    Add-AzureAccount
    ```

    Azure hesabı kimlik bilgilerinizi girmeniz istenir. Bu yöntem bir abonelik bağlantısı ekleme zaman aşımına uğrayıp 12 saat sonra cmdlet'ini yeniden çalıştırmanız gerekir.

   > [!NOTE]
   > Birden çok Azure aboneliğiniz varsa ve varsayılan abonelik kullanmak için kullanmak istediğiniz değil <strong>Select-AzureSubscription</strong> cmdlet'ini bir abonelik seçin.

3. Betik bölmesine aşağıdaki betiği kopyalayın ve ilk altı değişkenleri ayarlayın:

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for the tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    ```

    Değişkenleri daha fazla açıklamaları için bkz: [Önkoşullar](#prerequisites) Bu öğretici bölümünde.

4. Aşağıdaki betik bölmesine komut dosyası Ekle:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create the log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log to example/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Tıklatın **komut dosyasını Çalıştır** veya basın **F5** komut dosyasını çalıştırmak için. Çıktı şuna benzeyecektir:

    ![Eğitmen hazırlık çıktı][img-preparation-output]

## <a name="run-the-oozie-project"></a>Oozie projesi çalıştırma
Azure PowerShell cmdlet'lerin Oozie işleri tanımlamak için şu anda sağlamaz. Kullanabileceğiniz **Invoke-RestMethod** cmdlet'ini Oozie web hizmetlerini çağırır. Oozie web hizmetleri API'si bir HTTP REST JSON API'dir. Oozie web hizmetleri API'si hakkında daha fazla bilgi için bkz: [Apache Oozie 4.0 belgelerine] [ apache-oozie-400] (için Hdınsight kümesi sürüm 3.0) veya [Apache Oozie 3.3.2 belgelerine] [ apache-oozie-332] (için Hdınsight kümesi sürüm 2.1).

**Oozie işi göndermek için**

1. Windows PowerShell ISE açın (Windows 8 Başlat ekranında, yazın **PowerShell_ISE**ve ardından **Windows PowerShell ISE**. Daha fazla bilgi için bkz: [Başlat Windows PowerShell Windows 8 ve Windows][powershell-start]).
2. Betik bölmesine aşağıdaki betiği kopyalayın ve bu ilk on dört değişkenleri ayarlayın (ancak, atla **$storageUri**).

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"  
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Değişkenleri daha fazla açıklamaları için bkz: [Önkoşullar](#prerequisites) Bu öğretici bölümünde.

    $coordstart ve $coordend başlangıç ve bitiş saati iş akışı ' dir. UTC/GMT zaman aşımına uğrar bulmak için "utc saati" aratıp arayın. $CoordFrequency, iş akışını çalıştırmak istediğiniz sıklıkta dakikadır.
3. Aşağıdaki komut dosyasına ekleyin. Bu bölümü Oozie yükünü tanımlar:

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > İş akışı gönderme yükü dosyasına göre en önemli fark değişkenidir **oozie.coord.application.path**. İş akışının gönderdiğinizde, kullandığınız **oozie.wf.application.path** yerine.

4. Aşağıdaki komut dosyasına ekleyin. Bu bölümü Oozie web hizmetinin durumunu denetler:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
        }
    }
    ```

5. Aşağıdaki komut dosyasına ekleyin. Bu bölümü Oozie işi oluşturur:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > İş akışının gönderdiğinizde, işi oluşturulduktan sonra işini başlatmak için çağrısı başka bir web hizmeti olmanız gerekir. Bu durumda, düzenleyici işi zaman tarafından tetiklenir. İş otomatik olarak başlatılacak.

6. Aşağıdaki komut dosyasına ekleyin. Bu bölümü Oozie iş durumunu denetler:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
        }
    }
    ```

7. (İsteğe bağlı) Aşağıdaki komut dosyasına ekleyin.

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. Aşağıdaki komut dosyasına ekleyin:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

Ek işlevler çalıştırmak istiyorsanız # işareti kaldırın.

9. Hdınsight kümesi sürüm 2.1 ise "https://$clusterName.azurehdinsight.net:443/oozie/v2/" "https://$clusterName.azurehdinsight.net:443/oozie/v1/" ile değiştirin. Hdınsight kümesi sürüm 2.1 değil destekler 2 web hizmetlerini desteklemiyor.
10. Tıklatın **komut dosyasını Çalıştır** veya basın **F5** komut dosyasını çalıştırmak için. Çıktı şuna benzeyecektir:

     ![İş akışı çıkış öğreticisini çalıştırma][img-runworkflow-output]
11. Dışarı aktarılan verileri görmek için SQL veritabanına bağlayın.

**İş hata günlüğü denetlemek için**

Bir iş akışı gidermek için Oozie günlük dosyası C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log küme headnode bulunabilir. RDP hakkında daha fazla bilgi için bkz: [Azure Portalı'nı kullanarak yönetme Hdınsight kümelerini][hdinsight-admin-portal].

**Öğreticiyi yeniden çalıştırmak için**

İş akışını yeniden çalıştırmak için aşağıdaki görevleri gerçekleştirmeniz gerekir:

* Hive betiği çıktı dosyasını silin.
* Log4jLogsCount tablodaki verileri silin.

Kullanabileceğiniz örnek Windows PowerShell betiğini şöyledir:

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete the Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Oozie iş akışı ve Oozie Düzenleyicisi nasıl tanımlanacağı ve Azure PowerShell kullanarak bir Oozie Düzenleyicisi işi çalıştırmak öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight kullanmaya başlama][hdinsight-get-started]
* [Hdınsight ile Azure Blob storage kullanma][hdinsight-storage]
* [Hdınsight Azure PowerShell kullanarak yönetme][hdinsight-admin-powershell]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Hdınsight ile Sqoop kullanma][hdinsight-use-sqoop]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [Hdınsight için Java MapReduce programlar geliştirmek][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
