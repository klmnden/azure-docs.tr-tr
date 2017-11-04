---
title: "Java HBase istemci - Azure Hdınsight | Microsoft Docs"
description: "Apache Maven bir Java tabanlı Apache HBase uygulaması oluşturma ve ardından Azure hdınsight'ta HBase dağıtmak için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: larryfr
ms.openlocfilehash: 07f6d49b6d5c0a5b77ef5a7a7395a37e79419955
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="build-java-applications-for-apache-hbase"></a>Apache HBase için Java uygulamaları derleme

Oluşturmayı öğrenin bir [Apache HBase](http://hbase.apache.org/) Java uygulaması. Daha sonra Azure hdınsight'ta HBase ile uygulama kullanın.

Bu adımlarda belge kullanımı [Maven](http://maven.apache.org/) ve projeyi oluşturmak için. Maven yazılım proje yönetimi ve yazılım, belge ve Java projeleri için raporlar oluşturmanıza olanak sağlayan kavrama Aracı ' dir.

> [!NOTE]
> Bu belgede yer alan adımlar, en son Hdınsight 3.6 test edilmiş.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Gereksinimler

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 veya üzeri.

    > [!NOTE]
    > Hdınsight 3.5 ve büyük Java 8 gerektirir. Hdınsight'ın önceki sürümlerini Java 7 gerektirir.

* [Maven](http://maven.apache.org/)

* [HBase ile Linux tabanlı Azure Hdınsight kümesi](apache-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > Bu belgede yer alan adımlar 3.4 ve 3.5 Hdınsight küme sürümleri ile test edilmiştir. Örneklerde sağlanan varsayılan bir Hdınsight 3.5 küme için değerlerdir.

## <a name="create-the-project"></a>Proje oluşturma

1. Geliştirme ortamınızı komut satırından dizinleri, bu gibi bir durumda projesi oluşturmak istediğiniz konuma değiştirin `cd code\hbase`.

2. Kullanım **mvn** proje için askılamayı oluşturmak için Maven ile yüklü komutu.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > PowerShell kullanıyorsanız, almalısınız `-D` çift tırnak işareti parametreleri.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Bu komut, aynı ada sahip bir dizin oluşturur. **Artifactıd** parametre (**hbaseapp** Bu örnekte.) Bu dizin, aşağıdaki öğeleri içerir:

   * **pom.xml**: Proje nesne modeli ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) Projeyi derlemek için kullanılan bilgileri ve yapılandırma ayrıntılarını içerir.
   * **src**: içeren dizine **main/java/com/microsoft/örnekler** Yazar burada uygulama dizini.

3. Silme `src/test/java/com/microsoft/examples/apptest.java` dosya. Değil Bu örnekte, kullanılabilir.

## <a name="update-the-project-object-model"></a>Güncelleştirme projesi nesne modeli

1. Düzen `pom.xml` dosya ve aşağıdaki kodu ekleyin `<dependencies>` bölümü:

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    Bu bölümde proje gerektiğini belirtir **hbase istemci** ve **phoenix çekirdek** bileşenleri. Derleme zamanında varsayılan Maven depodan bu bağımlılıkları indirilir. Kullanabileceğiniz [Maven merkezi deposu arama](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) bu bağımlılığı hakkında daha fazla bilgi edinmek için.

   > [!IMPORTANT]
   > Hbase istemcisinin sürüm numarası Hdınsight kümenizle sağlanan HBase sürümü aynı olmalıdır. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.

   | Hdınsight küme sürümü | HBase sürümü kullanmak için |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4, 3.5 ve 3.6 |1.1.2 |

    Hdınsight sürümleri ve bileşenleri hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanılabilir farklı Hadoop bileşenleri nelerdir](../hdinsight-component-versioning.md).

3. Aşağıdaki kodu ekleyin **pom.xml** dosya. Bu metin içinde olmalıdır `<project>...</project>` etiketleri dosyasında, örneğin, arasında `</dependencies>` ve `</project>`.

   ```xml
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
        <resource>
            <directory>${basedir}/conf</directory>
            <filtering>false</filtering>
            <includes>
            <include>hbase-site.xml</include>
            </includes>
        </resource>
        </resources>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
            </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
            </configuration>
            <executions>
            <execution>
                <phase>package</phase>
                <goals>
                <goal>shade</goal>
                </goals>
            </execution>
            </executions>
        </plugin>
        </plugins>
    </build>
   ```

    Bu bölümde bir kaynak yapılandırır (`conf/hbase-site.xml`) HBase için yapılandırma bilgilerini içerir.

   > [!NOTE]
   > Kod aracılığıyla yapılandırma değerlerini de ayarlayabilirsiniz. Yer alan yorumlara bakın `CreateTable` örnek.

    Bu bölümde ayrıca yapılandırır [Maven derleyici eklentisi](http://maven.apache.org/plugins/maven-compiler-plugin/) ve [Maven gölge eklentisi](http://maven.apache.org/plugins/maven-shade-plugin/). Eklenti derleyici topoloji derlemek için kullanılır. Eklenti gölge lisans çoğaltma Maven tarafından oluşturulmuş JAR paketindeki önlemek için kullanılır. Bu eklenti, Hdınsight kümesinde çalışma zamanında "yinelenen lisans dosyaları" hatayı önlemek için kullanılır. Gölge maven eklentisi ile kullanarak `ApacheLicenseResourceTransformer` uygulama hata engeller.

    Maven gölge eklentisi Ayrıca uygulama tarafından istenen tüm bağımlılıkları içeren bir uber jar üretir.

4. Kaydet `pom.xml` dosya.

5. Adlı bir dizin oluşturun `conf` içinde `hbaseapp` dizin. Bu dizin için HBase bağlanmak için yapılandırma bilgilerini tutmak için kullanılır.

6. HBase yapılandırma HBase kümeye kopyalamak için aşağıdaki komutu kullanın `conf` dizin. Değiştir `USERNAME` SSH oturum açma adı. Değiştir `CLUSTERNAME` Hdınsight küme adıyla:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Kullanma hakkında daha fazla bilgi için `ssh` ve `scp`, bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-application"></a>Uygulama oluşturma

1. Git `hbaseapp/src/main/java/com/microsoft/examples` dizin ve yeniden adlandırma app.java dosyasını `CreateTable.java`.

2. Açık `CreateTable.java` dosya ve var olan içeriği aşağıdaki metinle değiştirin:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;
    import org.apache.hadoop.hbase.HTableDescriptor;
    import org.apache.hadoop.hbase.TableName;
    import org.apache.hadoop.hbase.HColumnDescriptor;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Put;
    import org.apache.hadoop.hbase.util.Bytes;

    public class CreateTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Example of setting zookeeper values for HDInsight
        // in code instead of an hbase-site.xml file
        //
        // config.set("hbase.zookeeper.quorum",
        //            "zookeepernode0,zookeepernode1,zookeepernode2");
        //config.set("hbase.zookeeper.property.clientPort", "2181");
        //config.set("hbase.cluster.distributed", "true");
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create the table...
        HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
        // ... with two column families
        tableDescriptor.addFamily(new HColumnDescriptor("name"));
        tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
        admin.createTable(tableDescriptor);

        // define some people
        String[][] people = {
            { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
            { "2", "Franklin", "Holtz", "franklin@contoso.com" },
            { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
            { "4", "Rae", "Schroeder", "rae@contoso.com" },
            { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
            { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

        HTable table = new HTable(config, "people");

        // Add each person to the table
        //   Use the `name` column family for the name
        //   Use the `contactinfo` column family for the email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close the table
        table.flushCommits();
        table.close();
        }
    }
   ```

    Bu kodu **CreateTable** adlı bir tablo oluşturur sınıfı **kişiler** ve önceden tanımlanmış bazı kullanıcılar ile doldurabilirsiniz.

3. Kaydet `CreateTable.java` dosya.

4. İçinde `hbaseapp/src/main/java/com/microsoft/examples` dizin adlı bir dosya oluşturun `SearchByEmail.java`. Aşağıdaki metni bu dosyanın içeriğini kullanın:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Scan;
    import org.apache.hadoop.hbase.client.ResultScanner;
    import org.apache.hadoop.hbase.client.Result;
    import org.apache.hadoop.hbase.filter.RegexStringComparator;
    import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
    import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
    import org.apache.hadoop.hbase.util.Bytes;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class SearchByEmail {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Use GenericOptionsParser to get only the parameters to the class
        // and not all the parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open the table
        HTable table = new HTable(config, "people");

        // Define the family and qualifiers to be used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach the regex filter to a filter
        //   for the email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set the filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get the results
        ResultScanner results = table.getScanner(scan);
        // Iterate over results and print  values
        for (Result result : results ) {
            String id = new String(result.getRow());
            byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
            String firstName = new String(firstNameObj);
            byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
            String lastName = new String(lastNameObj);
            System.out.println(firstName + " " + lastName + " - ID: " + id);
            byte[] emailObj = result.getValue(contactFamily, emailQualifier);
            String email = new String(emailObj);
            System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
        }
        results.close();
        table.close();
        }
    }
   ```

    **SearchByEmail** sınıfı kullanılabilir e-posta adresine göre satırlar için sorgulanamıyor. Normal ifade filtresi kullandığından, sınıf kullanırken bir dize veya normal bir ifade belirtebilirsiniz.

5. Kaydet `SearchByEmail.java` dosya.

6. İçinde `hbaseapp/src/main/hava/com/microsoft/examples` dizin adlı bir dosya oluşturun `DeleteTable.java`. Aşağıdaki metni bu dosyanın içeriğini kullanın:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete the table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    Bu sınıf devre dışı bırakma ve tarafından oluşturulan tablo bırakarak bu örnekte, oluşturduğunuz HBase tablolarını temizler `CreateTable` sınıfı.

7. Kaydet `DeleteTable.java` dosya.

## <a name="build-and-package-the-application"></a>Yapı ve uygulama paketi

1. Gelen `hbaseapp` dizin, uygulamayı içeren JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    mvn clean package
    ```

    Bu komut, oluşturur ve .jar dosyasına uygulama paketleri.

2. Komut tamamlandığında `hbaseapp/target` dizini içeren adlı bir dosya `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > `hbaseapp-1.0-SNAPSHOT.jar` Uber jar bir dosyadır. Uygulamayı çalıştırmak için gerekli olan tüm bağımlılıkları içerir.


## <a name="upload-the-jar-and-run-jobs-ssh"></a>JAR karşıya yükleme ve işleri (SSH) çalıştırın

Aşağıdaki adımları kullanın `scp` , HBase Hdınsight kümesinde birincil baş düğümüne JAR kopyalamak için. `ssh` Komutu kümeye bağlanın ve doğrudan baş düğümünde örneği çalıştırmak için kullanılan sonra.

1. Kümeye jar karşıya yüklemek için aşağıdaki komutu kullanın:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Değiştir `USERNAME` SSH oturum açma adı. Değiştir `CLUSTERNAME` ile Hdınsight kümenizin adıdır.

2. HBase kümeye bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Değiştir `USERNAME` SSH oturum açma adı. Değiştir `CLUSTERNAME` ile Hdınsight kümenizin adıdır.

3. Java uygulaması kullanarak bir HBase tablosu oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Bu komut adlı bir HBase tablosu oluşturur **kişiler**ve verileri ile doldurur.

4. Tabloda depolanan e-posta adreslerini aramak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    Aşağıdaki sonuçları alırsınız:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Tablo silmek için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>JAR karşıya yükleme ve işleri (PowerShell) çalıştırın

HBase kümesi için varsayılan depolama JAR yüklemek için Azure PowerShell aşağıdaki adımları kullanın. Hdınsight cmdlet'lerini sonra örnekler uzaktan çalıştırmak için kullanılır.

1. Yükleme ve Azure PowerShell, yapılandırma oluşturduktan sonra adlı bir dosya `hbase-runner.psm1`. Aşağıdaki metni bu dosyanın içeriğini kullanın:

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzureStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    Bu dosya iki modülleri içerir:

   * **Ekleme HDInsightFile** - kümeye dosyaları yüklemek için kullanılan
   * **Başlangıç HBaseExample** - daha önce oluşturduğunuz sınıfları çalıştırmak için kullanılan

2. Kaydet `hbase-runner.psm1` dosya.

3. Yeni bir Azure PowerShell penceresi açın, dizinleri değiştirmek `hbaseapp` dizin ve aşağıdaki komutu çalıştırın:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Konumuna yolunu değiştirmek `hbase-runner.psm1` daha önce oluşturduğunuz dosya. Bu komut, modül Azure PowerShell ile kaydeder.

4. Karşıya yüklemek için aşağıdaki komutu kullanın `hbaseapp-1.0-SNAPSHOT.jar` kümenize.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin. Komut yükler `hbaseapp-1.0-SNAPSHOT.jar` için `example/jars` kümeniz için birincil depolama konumu.

5. Kullanarak bir tablo oluşturmak için `hbaseapp`, aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin.

    Bu komut adlı bir tablo oluşturur **kişiler** HBase Hdınsight kümenizdeki içinde. Bu komut, konsol penceresinde herhangi bir çıktı göstermez.

6. Tablosunda giriş aramak için aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin.

    Bu komut kullanır `SearchByEmail` sınıfı için herhangi bir satır arama nerede `contactinformation` sütun ailesi ve `email` sütunu içeren dize `contoso.com`. Aşağıdaki sonuçları almanız gerekir:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kullanarak **fabrikam.com** için `-emailRegex` değeri döndürür kullanıcıları **fabrikam.com** e-posta alanında. Arama terimi olarak normal ifadeler de kullanabilirsiniz. Örneğin, **^ r** döndürür, 'r' harfle başlayan adreslerini e-posta.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Hiçbir sonuçları veya başlangıç HBaseExample kullanırken beklenmeyen sonuçlar

Kullanım `-showErr` işi çalıştırılırken üretilen standart hata (STDERR) görüntülemek için parametre.

## <a name="delete-the-table"></a>Tablo silme

Örneğiyle bittiğinde silmek için aşağıdakileri kullanın **kişiler** Bu örnekte kullanılan tablo:

__Gelen bir `ssh` oturum__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Azure PowerShell üzerinden__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Sonraki adımlar

[HBase ile SQuirreL SQL kullanmayı öğrenin](apache-hbase-phoenix-squirrel-linux.md)
