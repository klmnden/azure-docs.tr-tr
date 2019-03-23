---
title: HDInsight, Hadoop Oozie Düzenleyici zamana bağlı kullanın
description: HDInsight, büyük veri hizmeti zaman tabanlı Hadoop Oozie düzenleyicisi kullanın. Oozie ile iş akışlarını ve düzenleyicileri tanımlayın ve işlerini gönderme hakkında bilgi edinin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/04/2017
ROBOTS: NOINDEX
ms.openlocfilehash: bf1c7360157b3b4af467cf53634056d70b91ebd2
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361345"
---
# <a name="use-time-based-apache-oozie-coordinator-with-apache-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>İş akışları tanımlamak ve işlerini koordine etmek için HDInsight, Apache Hadoop ile zamana dayalı Apache Oozie düzenleyicisini kullanma
Bu makalede, zamana dayalı Düzenleyicisi işlerini nasıl tetikleyeceğinizi yanı sıra iş akışlarını ve düzenleyicileri nasıl tanımlanacağını öğreneceksiniz. Git yararlıdır [HDInsight ile kullanmak Apache Oozie] [ hdinsight-use-oozie] önce bu makaleyi okuyun. Oozie ek olarak, Azure Data Factory kullanarak işleri zamanlayabilirsiniz. Azure Data Factory bilgi edinmek için [kullanım Apache Pig ve Apache Hive ile veri fabrikası](../data-factory/transform-data.md).

> [!NOTE]  
> Bu makale, bir Windows tabanlı HDInsight kümesi gerekir. Linux tabanlı bir kümede zaman tabanlı işleri de dahil olmak üzere, Oozie kullanma hakkında bilgi için bkz: [tanımlamak ve Linux tabanlı HDInsight üzerinde bir iş akışı çalıştırmak için Hadoop ile Oozie kullanma](hdinsight-use-oozie-linux-mac.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-is-oozie"></a>Oozie nedir
Apache Oozie, Hadoop işlerini yöneten bir iş akışı/koordinasyon sistemidir. Bu Hadoop yığını ile tümleştirilir ve Apache Hadoop MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Aşağıdaki görüntüde, uygulama iş akışı gösterilmektedir:

![İş akışı diyagramı][img-workflow-diagram]

İş akışı iki eylemleri içerir:

1. Bir Hive eylem log4j günlük dosyasını bir Apache, her günlük düzeyi türünün sayısını için HiveQL betiğini çalıştırır. Örnek türünün ve önem göstermek için bir [günlük düzeyi] alan içeren bir dizi alanlarının her log4j günlük oluşur:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Benzer şekilde Hive betik çıktısı:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma Apache][hdinsight-use-hive].
2. Sqoop eylem HiveQL eylem çıkışındaki bir Azure SQL veritabanı tablosuna dışarı aktarır. Sqoop hakkında daha fazla bilgi için bkz: [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop].

> [!NOTE]  
> HDInsight kümelerinde desteklenen Oozie sürümleri için bkz: [HDInsight tarafından sağlanan küme sürümlerindeki yenilikler nelerdir?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure PowerShell içeren bir iş istasyonu**.

    > [!IMPORTANT]  
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışıdır** ve 1 Ocak 2017 tarihine kadar kaldırılacaktır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

* **Bir HDInsight kümesi**. HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [oluşturma HDInsight kümeleri][hdinsight-provision], veya [HDInsight ile çalışmaya başlama][hdinsight-get-started]. Bu öğreticiyi incelemek için aşağıdaki veriler ihtiyacınız olacak:

    |Küme özelliği|Windows PowerShell değişken adı|Değer|Açıklama|
    |---|---|---|---|
    |HDInsight küme adı|$clusterName||Bu öğreticide çalıştırılacağı HDInsight kümesi.|
    |HDInsight küme kullanıcı adı|$clusterUsername||HDInsight küme kullanıcı adı. |
    |HDInsight küme kullanıcı parolası |$clusterPassword||HDInsight küme kullanıcı parolası.|
    |Azure depolama hesabı adı|$storageAccountName||HDInsight kümesi için kullanılabilen bir Azure depolama hesabı. Bu öğretici için küme sağlama işlemi sırasında belirtilen varsayılan depolama hesabı kullanın.|
    |Azure Blob kapsayıcısı adı|$containerName||Bu örnekte, varsayılan HDInsight küme dosya sistemi için kullanılan Azure Blob Depolama kapsayıcısına kullanın. Varsayılan olarak, HDInsight kümesi ile aynı ada sahiptir.|


* **Bir Azure SQL veritabanı**. İstasyonunuzdan erişime izin vermek SQL veritabanı sunucusu için bir güvenlik duvarı kuralı yapılandırmanız gerekir. Bir Azure SQL veritabanı oluşturma ve güvenlik duvarını yapılandırma hakkında yönergeler için bkz: [Azure SQL veritabanı ile çalışmaya başlamak][sqldatabase-get-started]. Bu makalede, Bu öğretici için gereksinim duyduğunuz Azure SQL veritabanı tablosu oluşturmak için bir Windows PowerShell komut dosyası sağlar.

    |SQL veritabanı özelliği|Windows PowerShell değişken adı|Değer|Açıklama|
    |---|---|---|---|
    |SQL veritabanı sunucu adı|$sqlDatabaseServer||SQL veritabanı sunucusu Sqoop verileri dışarı aktarır. |
    |SQL veritabanı oturum açma adı|$sqlDatabaseLogin||SQL veritabanı oturum açma adı.|
    |SQL veritabanı oturum açma parolası|$sqlDatabaseLoginPassword||SQL veritabanı oturum açma parolası.|
    |SQL veritabanı adı|$sqlDatabaseName||Azure SQL veritabanı Sqoop verileri dışarı aktarır. |

  > [!NOTE]   
  > Varsayılan olarak bir Azure SQL veritabanı, Azure HDInsight gibi Azure hizmetlerinden bağlantılar sağlar. Bu güvenlik duvarı ayarı devre dışıysa, Azure portalından etkinleştirmeniz gerekir. SQL veritabanı oluşturma ve güvenlik duvarı kuralları yapılandırma hakkında daha fazla yönerge için bkz. [oluşturma ve SQL veritabanını Yapılandır][sqldatabase-get-started].

> [!NOTE]  
> Doldurma tablolardaki değerleri. Bu öğreticide giden yararlı olacaktır.

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie iş akışının ve ilgili HiveQL betiğini tanımlayın
Oozie iş akışı tanımları hPDL (bir XML işlem tanımı dili) yazılır. Varsayılan iş akışı dosya adı *workflow.xml*.  İş akışı dosyasını yerel olarak kaydedin ve ardından daha sonra Bu öğreticide Azure PowerShell kullanarak HDInsight kümesine dağıtın.

Hive iş akışı eylemi HiveQL komut dosyasını çağırır. Bu komut dosyası üç HiveQL ifadelerini içerir:

1. **DROP TABLE deyimi** log4j Hive tablosu varsa siler.
2. **CREATE TABLE deyimi** log4j günlük dosyasını; konumuna işaret eden bir log4j Hive dış tablo oluşturur
3. **Log4j günlük dosyasının konumunu**. Alan sınırlayıcı ",". Varsayılan satır sınırlayıcı "\n" dir. Dış bir Hive tablosu, Oozie iş akışının birden çok kez çalıştırmak istediğiniz durumunda özgün konumundan kaldırılıyor veri dosyası önlemek için kullanılır.
4. **INSERT üzerine deyimi** sayıları log4j Hive tablosu ve her günlük düzeyi türünün kaydeder çıkış için bir Azure Blob Depolama konumu.

> [!NOTE]  
> Bilinen bir Hive yolu sorun yoktur. Bu sorunla karşılaşırsanız, Oozie iş gönderirken çalıştırılır. Sorunu düzeltmek için yönergeleri için TechNet Wiki'de bulunabilir: [HDInsight Hive hata: Yeniden adlandırılamıyor][technetwiki-hive-error].

**İş akışı tarafından çağrılacak HiveQL betiğini dosyasını tanımlama**

1. Aşağıdaki içeriğe bir metin dosyası oluşturun:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Betikte kullanılan üç değişkenler vardır:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     İş akışı tanımı dosyası (Bu öğreticide workflow.xml) bu değerleri çalışma zamanında bu HiveQL betiğini geçer.
2. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\useooziewf.hql** (ASCII) ANSI kodlaması kullanarak. (Bu seçenek, metin düzenleyiciyi sağlamıyorsa not defteri kullanın.) Bu komut dosyası, öğreticinin ilerleyen bölümlerinde HDInsight kümesine dağıtılır.

**Bir iş akışı tanımlamak için**

1. Aşağıdaki içeriğe bir metin dosyası oluşturun:

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

    İş akışında tanımlanan iki eylemler vardır. Başlangıç için eylem *RunHiveScript*. Eylem çalıştırıyorsa *Tamam*, sonraki eylem *RunSqoopExport*.

    RunHiveScript birkaç değişkeni yok. Azure PowerShell kullanarak Oozie iş istasyonunuzdan gönderdiğinizde değerler geçer.

    İş akışı değişkenleri

    |İş akışı değişkenleri|Açıklama|
    |---|---|
    |${jobTracker}|Hadoop işi İzleyicisi URL'sini belirtin. Kullanım **jobtrackerhost:9010** sürüm 3.0 ve 2.0 üzerinde HDInsight kümesi.|
    |${nameNode}|Hadoop adı düğüm URL'sini belirtin. Varsayılan dosya sistemi wasb kullanın: / / adres, örneğin, *wasb: / /&lt;containerName&gt;\@&lt;storageAccountName&gt;. blob.core.windows.net*.|
    |${queueName}|İş için gönderilecek kuyruk adını belirtir. Kullanım **varsayılan**.|

    Hive eylem değişkenleri

    |Hive eylem değişkeni|Açıklama|
    |----|----|
    |${hiveDataFolder}|Hive tablosu oluşturma komutu için kaynak dizini.|
    |${hiveOutputFolder}|Çıkış klasörü üzerine INSERT deyimi için.|
    |${hiveTableName}|Log4j veri dosyalarına başvuran bir Hive tablosu adı.|

    Sqoop eylem değişkenleri

    |Sqoop eylem değişkeni|Açıklama|
    |---|---|
    |${sqlDatabaseConnectionString}|SQL veritabanı bağlantı dizesi.|
    |${sqlDatabaseTableName}|Verilerin nerede dışarı aktarılacak için Azure SQL veritabanı tablosu.|
    |${hiveOutputFolder}|Çıkış klasörü Ekle yığın üzerine deyimi için. Sqoop dışa aktar (dışarı aktarma-dir) aynı klasörde budur.|

    Oozie iş akışının ve iş akışı eylemlerini kullanma hakkında daha fazla bilgi için bkz. [Apache Oozie 4.0 belgeleri] [ apache-oozie-400] (için HDInsight kümesi sürüm 3.0) veya [Apache Oozie 3.3.2 belgeleri ] [ apache-oozie-332] (için HDInsight kümesi sürüm 2.1).

1. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\workflow.xml** (ASCII) ANSI kodlaması kullanarak. (Bu seçenek, metin düzenleyiciyi sağlamıyorsa not defteri kullanın.)

**Düzenleyici tanımlamak için**

1. Aşağıdaki içeriğe bir metin dosyası oluşturun:

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
   | ${coordFrequency} |İş duraklatma süresi. Her zaman sıklığını dakika cinsinden ifade edilir. |
   | ${coordStart} |İş başlangıç zamanı. |
   | ${coordEnd} |İşin bitiş saati. |
   | ${coordTimezone} |Oozie Düzenleyicisi işleri sabit bir saat diliminde (genellikle UTC saat kullanarak tarafından temsil edilen) hiçbir ışığından ile işler. Bu saat dilimini "Oozie işleme saat dilimi." olarak adlandırılır |
   | ${wfPath} |Workflow.xml yoludur.  İş akışı dosyası adı varsayılan dosya adı (workflow.xml) değilse, bunu belirtmeniz gerekir. |
2. Dosyayı Farklı Kaydet **C:\Tutorials\UseOozie\coordinator.xml** ANSI (ASCII) kodlama kullanılarak. (Bu seçenek, metin düzenleyiciyi sağlamıyorsa not defteri kullanın.)

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Oozie projeyi dağıtmak ve öğretici hazırlama
Aşağıdakileri gerçekleştirmek üzere bir Azure PowerShell Betiği çalışır:

* Azure Blob depolama alanına (useoozie.hql) HiveQL betiğini kopyalayın wasb:///tutorials/useoozie/useoozie.hql.
* Workflow.XML wasb:///tutorials/useoozie/workflow.xml için kopyalayın.
* Coordinator.XML wasb:///tutorials/useoozie/coordinator.xml için kopyalayın.
* Veri dosyasını kopyalayın (/ example/data/sample.log) wasb:///tutorials/useoozie/data/sample.log için.
* Sqoop dışarı aktarma verileri depolamak için bir Azure SQL veritabanı tablosu oluşturun. Tablo adı *log4jLogCount*.

**HDInsight depolama anlama**

HDInsight, veri depolama için Azure Blob Depolama kullanır. wasb: / / Azure Blob Depolama alanında Hadoop dağıtılmış dosya sistemi (HDFS) Microsoft uygulamasıdır. Daha fazla bilgi için [kullanımı Azure Blob Depolama, HDInsight ile][hdinsight-storage].

Bir HDInsight kümesi sağlama, bir Azure Blob Depolama hesabı ve bu hesaptan belirli bir kapsayıcı tasarlanmış varsayılan dosya sistemi olarak gibi HDFS'deki. Bu depolama hesabına ek olarak, ek depolama hesapları aynı Azure aboneliğinden veya farklı Azure aboneliklerinden gelen sağlama işlemi sırasında ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [sağlama HDInsight kümeleri][hdinsight-provision]. Bu öğreticide kullanılan Azure PowerShell Betiği basitleştirmek için tüm dosyaları konumundaki varsayılan dosya sistemi kapsayıcısında depolanan */öğreticiler/useoozie*. Varsayılan olarak, bu kapsayıcı HDInsight küme adı ile aynı ada sahiptir.
Söz dizimi aşağıdaki gibidir:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]  
> Yalnızca *wasb: / /* söz dizimi HDInsight kümesi sürüm 3.0 desteklenir. Eski *asv: / /* söz dizimi HDInsight 2.1 ve 1.6 kümeleri desteklenir, ancak HDInsight 3.0 kümelerinde desteklenmez.
>
> Wasb: / / yolu olan bir sanal yol. Daha fazla bilgi için [kullanımı Azure Blob Depolama, HDInsight ile][hdinsight-storage].

Varsayılan dosya sistemi kapsayıcısında depolanan bir dosya (workflow.xml örnek olarak kullanıyorum) aşağıdaki bir URI'leri birini kullanarak HDInsight ' erişilebilir:

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Dosya depolama hesabından doğrudan erişmek istiyorsanız, blob dosya adıdır:

    tutorials/useoozie/workflow.xml

**İç ve dış tablolar Hive'ı Anlama**

Hive iç ve dış tablolar hakkında bilmeniz gereken birkaç nokta vardır:

* CREATE TABLE komut, yönetilen bir tablo olarak da bilinen bir iç tablosu oluşturur. Veri dosyasındaki varsayılan kapsayıcıda yer almalıdır.
* CREATE TABLE komutu veri dosyası /hive/warehouse/dizinine taşınır<TableName> varsayılan kapsayıcısındaki.
* CREATE EXTERNAL TABLE komutu bir dış tablo oluşturur. Veri dosyasındaki varsayılan kapsayıcı yer alabilir.
* CREATE EXTERNAL TABLE komutu, veri dosyası taşımaz.
* CREATE EXTERNAL TABLE komutu konum yan tümcesinde belirtilen klasör altındaki tüm alt klasörlerde izin vermez. Neden öğretici sample.log dosyasına bir kopyasını getirir nedeni budur.

Daha fazla bilgi için [HDInsight: Apache Hive iç ve dış tablolar giriş][cindygross-hive-tables].

**Öğreticiye hazırlamak için**

1. Windows PowerShell ISE'yi açın (Windows 8 Başlat ekranında, yazın **PowerShell_ISE**ve ardından **Windows PowerShell ISE**. Daha fazla bilgi için [Windows 8 ve Windows üzerinde Windows PowerShell başlangıç][powershell-start]).
2. Alt bölmede Azure aboneliğinize bağlanmak için aşağıdaki komutu çalıştırın:

    ```powershell
    Add-AzureAccount
    ```

    Azure hesabı kimlik bilgilerinizi girmeniz istenir. Bir abonelik bağlantı ekleme, bu yöntem zaman aşımına uğrar ve 12 saatin ardından cmdlet'i yeniden çalıştırmanız gerekir.

   > [!NOTE]  
   > Birden çok Azure aboneliğiniz varsa varsayılan aboneliği kullanmak için kullanmak istediğiniz değilse <strong>Select-AzureSubscription</strong> cmdlet'ini bir abonelik seçin.

3. Betik bölmesine aşağıdaki betiği kopyalayın ve ardından ilk altı değişkenleri ayarlayın:

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

    Daha fazla değişkenlerin açıklamaları için bkz [önkoşulları](#prerequisites) Bu öğretici bölümünde.

4. Aşağıdaki betik bölmesinde betik Ekle:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-AzStoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
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

5. Tıklayın **betiğini Çalıştır** veya basın **F5** betiği çalıştırmak için. Çıktı şuna benzer olacaktır:

    ![Öğretici hazırlık çıkış][img-preparation-output]

## <a name="run-the-oozie-project"></a>Oozie projeyi Çalıştır
Azure PowerShell, şu anda Oozie işleri tanımlamak için tüm cmdlet'leri sağlamaz. Kullanabileceğiniz **Invoke-RestMethod** cmdlet'ini Oozie web hizmetlerini çağır. Oozie web servisleri API bir HTTP REST JSON API'dir. Oozie web hizmetleri API'si hakkında daha fazla bilgi için bkz. [Apache Oozie 4.0 belgeleri] [ apache-oozie-400] (için HDInsight kümesi sürüm 3.0) veya [Apache Oozie 3.3.2 belgeleri] [ apache-oozie-332] (için HDInsight kümesi sürüm 2.1).

**Oozie işi göndermek için**

1. Windows PowerShell ISE'yi açın (Windows 8 Başlat ekranında, yazın **PowerShell_ISE**ve ardından **Windows PowerShell ISE**. Daha fazla bilgi için [Windows 8 ve Windows üzerinde Windows PowerShell başlangıç][powershell-start]).
2. Betik bölmesine aşağıdaki betiği kopyalayın ve ardından ilk on dört değişkenleri (ancak atla **$storageUri**).

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

    Daha fazla değişkenlerin açıklamaları için bkz [önkoşulları](#prerequisites) Bu öğretici bölümünde.

    $coordstart ve $coordend başlangıç ve bitiş saati iş akışı ' dir. UTC/GMT zaman aşımına bulmak için "utc saati" bing.com adresindeki dizinlerde arayın. $CoordFrequency iş akışını çalıştırmak istediğiniz sıklıkta dakikadır.
3. Aşağıdaki betiği ekleyin. Bu bölüm, Oozie yükünü tanımlar:

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
   > İş akışı Gönderme Yükü dosyasını karşılaştırıldığında en önemli fark değişkendir **oozie.coord.application.path**. Bir iş akışı işi gönderdiğinizde, kullandığınız **oozie.wf.application.path** yerine.

4. Aşağıdaki betiği ekleyin. Bu bölüm, Oozie web hizmet durumunu denetler:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerStatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerStatus..."

        if($oozieServerStatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerStatus...cannot submit Oozie jobs. Check the server status and re-run the job."
        }
    }
    ```

5. Aşağıdaki betiği ekleyin. Bu bölüm, Oozie iş oluşturur:

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
   > Bir iş akışı işi gönderdiğinizde, başka bir web hizmeti işi oluşturulduktan sonra işi başlatmak için çağrısı yapmalısınız. Bu durumda, düzenleyici işi zaman tetiklenir. İş otomatik olarak başlatılacak.

6. Aşağıdaki betiği ekleyin. Bu bölüm, Oozie iş durumunu denetler:

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

7. (İsteğe bağlı) Aşağıdaki betiği ekleyin.

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

8. Aşağıdaki betiği ekleyin:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

Ek işlevler çalıştırmak istiyorsanız # işareti kaldırın.

1. HDInsight kümenizi sürüm 2.1 ise, "https://$clusterName.azurehdinsight.net:443/oozie/v2/" "https://$clusterName.azurehdinsight.net:443/oozie/v1/" ile değiştirin. HDInsight kümesi sürüm 2.1 değil web hizmetlerinin desteklediği sürüm 2 yapar.
1. Tıklayın **betiğini Çalıştır** veya basın **F5** betiği çalıştırmak için. Çıktı şuna benzer olacaktır:

    ![Öğretici, iş akışı çıkışı çalıştırma][img-runworkflow-output]
1. Dışarı aktarılan verileri görmek için SQL veritabanınıza bağlanın.

**İş hata günlüğünü kontrol etmek için**

Bir iş akışının sorunlarını gidermek için Oozie günlük dosyası C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log küme baş düğümüne bulunabilir. RDP hakkında daha fazla bilgi için bkz: [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](hdinsight-administer-use-portal-linux.md).

**Öğreticiyi yeniden çalıştırmak için**

İş akışını yeniden çalıştırmak için aşağıdaki görevleri gerçekleştirmeniz gerekir:

* Hive betiği çıkış dosyasını silin.
* Log4jLogsCount tablodaki verileri silin.

Aşağıda, kullanabileceğiniz örnek Windows PowerShell Betiği verilmiştir:

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
$storageaccountkey = get-AzStoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

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
Bu öğreticide, bir Oozie iş akışının ve Oozie düzenleyicisini nasıl tanımlandığını ve Azure PowerShell kullanarak bir Oozie Düzenleyicisi işi çalıştırmak öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight ile çalışmaya başlama][hdinsight-get-started]
* [HDInsight ile Azure Blob Depolama kullanma][hdinsight-storage]
* [HDInsight, Azure PowerShell kullanarak yönetme][hdinsight-admin-powershell]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]
* [HDInsight için Java MapReduce programları geliştirme][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

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

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell?view=powershell-6
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
