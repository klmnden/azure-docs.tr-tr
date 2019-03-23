---
title: Apache Maven - Azure HDInsight'ı kullanarak Java HBase istemci oluşturun
description: Apache Maven bir Java tabanlı Apache HBase uygulaması oluşturmayı ve ardından Azure HDInsight üzerinde HBase dağıtmak için kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: fa831ad878d214515849787988ccb32f6c57ce20
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361770"
---
# <a name="build-java-applications-for-apache-hbase"></a>Apache HBase için Java uygulamaları oluşturun

Oluşturmayı bir [Apache HBase](https://hbase.apache.org/) Java uygulaması. Ardından, Azure HDInsight üzerinde HBase ile uygulamayı kullanın.

Bu adımları belge kullanım [Apache Maven](https://maven.apache.org/) ve projeyi oluşturmak için. Maven, yazılım proje yönetimi ve yazılım, belgelere ve Java projeleri için raporlar oluşturmanıza olanak sağlayan kavramayı Aracı ' dir.

> [!NOTE]  
> Bu belgedeki adımlarda HDInsight 3.6 ile birlikte en son test edilmiştir.

> [!IMPORTANT]  
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Gereksinimler

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* [Java platformu JDK](https://aka.ms/azure-jdks) 8 veya üzeri.

    > [!NOTE]  
    > Java 8 HDInsight 3.5 ve daha sonraki sürümleri gerektirir. HDInsight'ın önceki sürümlerinde, Java 7 gerektirir.

* [Apache Maven](https://maven.apache.org/)

* [Apache HBase ile bir Azure HDInsight Linux tabanlı küme](apache-hbase-tutorial-get-started-linux.md#create-apache-hbase-cluster)

## <a name="create-the-project"></a>Proje oluşturma

1. Geliştirme ortamınızdaki komut satırından, dizinleri, bu gibi bir durumda projesini oluşturmak istediğiniz konuma değiştirin `cd code\hbase`.

2. Kullanım **mvn** proje için yapı iskelesini oluşturmak için Maven ile yüklenir, komutu.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]  
    > PowerShell kullanıyorsanız, almalısınız `-D` çift tırnak parametreleri.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Bu komut aynı ada sahip bir dizin oluşturur. **Artifactıd** parametre (**hbaseapp** Bu örnekte.) Bu dizin, aşağıdaki öğeleri içerir:

   * **pom.xml**:  Proje nesne modeli ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) projeyi oluşturmak için kullanılan bilgiler ve yapılandırma ayrıntılarını içerir.
   * **src**: İçeren dizine **main/java/com/microsoft/örnekler** Yazar burada uygulama dizini.

3. Silme `src/test/java/com/microsoft/examples/apptest.java` dosya. Bu, bu örnekte kullanılmaz.

## <a name="update-the-project-object-model"></a>Proje nesne modeli güncelleştirme

1. Düzen `pom.xml` dosya ve içine aşağıdaki kodu ekleyin `<dependencies>` bölümü:

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

    Bu bölümde, proje gerektiğini belirtir **hbase istemci** ve **phoenix çekirdek** bileşenleri. Bu bağımlılıklar, derleme zamanında varsayılan Maven deposundan yüklenir. Kullanabileceğiniz [Maven merkezi depo arama](https://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) bu bağımlılık hakkında daha fazla bilgi edinmek için.

   > [!IMPORTANT]  
   > Hbase istemcisinin sürüm numarası, Apache HBase, HDInsight kümenizle sağlanan sürümünü eşleşmesi gerekir. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.

   | HDInsight küme sürümü | Apache HBase sürümü kullanmak için |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4, 3.5 ve 3.6 |1.1.2 |

    HDInsight sürümleri ve bileşenleri hakkında daha fazla bilgi için bkz. [HDInsight ile kullanılabilen farklı Apache Hadoop bileşenlerinin nelerdir](../hdinsight-component-versioning.md).

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

    Bu bölümde, bir kaynak yapılandırır (`conf/hbase-site.xml`), HBase için yapılandırma bilgilerini içerir.

   > [!NOTE]  
   > Ayrıca, kod aracılığıyla yapılandırma değerlerini ayarlayabilirsiniz. Bölümündeki yorumlara bakın `CreateTable` örnek.

    Bu bölümde ayrıca yapılandırır [Apache Maven derleme eklentisini](https://maven.apache.org/plugins/maven-compiler-plugin/) ve [Apache Maven gölge eklentisi](https://maven.apache.org/plugins/maven-shade-plugin/). Eklenti derleyici topoloji derlemek için kullanılır. Eklenti gölge Maven tarafından oluşturulan JAR paketi lisans yinelenmesini önlemek için kullanılır. Bu eklenti, HDInsight kümesi üzerinde çalışma zamanında bir "yinelenen lisans files" hatayı önlemek için kullanılır. Gölge maven plugin ile kullanarak `ApacheLicenseResourceTransformer` uygulama hatasını önler.

    Maven gölge eklentisi ayrıca uygulamanın gerektirdiği tüm bağımlılıklarını içeren bir uber jar üretir.

4. `pom.xml` dosyasını kaydedin.

5. Adlı bir dizin oluşturmak `conf` içinde `hbaseapp` dizin. Bu dizin için HBase bağlanmak için yapılandırma bilgilerini saklamak için kullanılır.

6. HBase kümesi için HBase yapılandırma kopyalamak için aşağıdaki komutu kullanın `conf` dizin. Değiştirin `USERNAME` , SSH oturum açma adı. Değiştirin `CLUSTERNAME` ile HDInsight kümenizin adıdır:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Kullanma hakkında daha fazla bilgi için `ssh` ve `scp`, bkz: [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-application"></a>Uygulama oluşturma

1. Git `hbaseapp/src/main/java/com/microsoft/examples` dizin ve yeniden adlandırma app.java dosyasını `CreateTable.java`.

2. Açık `CreateTable.java` dosya ve varolan içeriği aşağıdaki metinle değiştirin:

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

3. `CreateTable.java` dosyasını kaydedin.

4. İçinde `hbaseapp/src/main/java/com/microsoft/examples` dizin adlı bir dosya oluşturun `SearchByEmail.java`. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

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

    **SearchByEmail** sınıfı kullanılabilir sorgulamak için e-posta adresine göre satır. Normal ifade filtresi kullandığından, sınıf kullanırken bir dize veya bir normal ifade sağlayabilir.

5. `SearchByEmail.java` dosyasını kaydedin.

6. İçinde `hbaseapp/src/main/hava/com/microsoft/examples` dizin adlı bir dosya oluşturun `DeleteTable.java`. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

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

    Bu sınıf, bu örnekte, devre dışı bırakma ve tarafından oluşturulan tabloyu bırakarak oluşturduğunuz HBase tablolarını temizler `CreateTable` sınıfı.

7. `DeleteTable.java` dosyasını kaydedin.

## <a name="build-and-package-the-application"></a>Derleme ve uygulamayı paketlemeyi

1. Gelen `hbaseapp` dizin, uygulamayı içeren JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    mvn clean package
    ```

    Bu komut, yapılar ve .jar dosyasını uygulamasına paketler.

2. Komut tamamlandığında, `hbaseapp/target` dizin adlı dosyayı içeren `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]  
   > `hbaseapp-1.0-SNAPSHOT.jar` Uber jar dosyasıdır. Bu uygulamayı çalıştırmak için gereken tüm bağımlılıkları içerir.


## <a name="upload-the-jar-and-run-jobs-ssh"></a>Jar dosyasını karşıya yükleme ve işlerinizi (SSH)

Aşağıdaki adımları kullanın `scp` HDInsight kümesinde, Apache HBase, birincil baş düğüme JAR kopyalanacak. `ssh` Komutu ardından kümeye bağlanın ve doğrudan baş düğümünde örneği çalıştırmak için kullanılır.

1. Kümeye jar dosyasını karşıya yüklemek için aşağıdaki komutu kullanın:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Değiştirin `USERNAME` , SSH oturum açma adı. Değiştirin `CLUSTERNAME` ile HDInsight kümenizin adıdır.

2. HBase kümeye bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Değiştirin `USERNAME` , SSH oturum açma adı. Değiştirin `CLUSTERNAME` ile HDInsight kümenizin adıdır.

3. Java uygulaması kullanarak bir HBase tablosu oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Bu komut, adlandırılmış bir HBase tablosu oluşturur **kişiler**ve verilerle doldurur.

4. Tabloda depolanan bir e-posta adreslerini aramak için aşağıdaki komutu kullanın:

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

## <a name="upload-the-jar-and-run-jobs-powershell"></a>Jar dosyasını karşıya yükleme ve işlerinizi (PowerShell)

Aşağıdaki adımlar, varsayılan depolama alanı, Apache HBase kümesi için jar dosyasını yüklemek için Azure PowerShell kullanırsınız. HDInsight cmdlet'leri, daha sonra örnekleri uzaktan çalıştırmak için kullanılır.

1. Yükleme ve Azure PowerShell, yapılandırma oluşturduktan sonra adlı bir dosya `hbase-runner.psm1`. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

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
    $jobDefinition = New-AzHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzHDInsightJobOutput `
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
        Set-AzStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use the Connect-AzAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzHDInsightCluster -ClusterName $clusterName
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
        $storageAccountKey=(Get-AzStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
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

    Bu dosya iki modülü içerir:

   * **Ekleme HDInsightFile** - kümeye dosyaları karşıya yüklemek için kullanılır
   * **Başlangıç HBaseExample** - daha önce oluşturulan sınıflar çalıştırmak için kullanılır

2. `hbase-runner.psm1` dosyasını kaydedin.

3. Yeni bir Azure PowerShell penceresi açın, dizinleri `hbaseapp` dizin ve ardından aşağıdaki komutu çalıştırın:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Yolun konumuyla değiştirin `hbase-runner.psm1` daha önce oluşturulan dosya. Bu komut modülü, Azure PowerShell ile kaydeder.

4. Karşıya yüklemek için aşağıdaki komutu kullanın `hbaseapp-1.0-SNAPSHOT.jar` kümenize.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin. İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin. Komut yükler `hbaseapp-1.0-SNAPSHOT.jar` için `example/jars` kümeniz için birincil depolama konumu.

5. Kullanarak tablo oluşturmak için `hbaseapp`, aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin. İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin.

    Bu komut, adlı bir tablo oluşturur. **kişiler** HDInsight kümenizdeki HBase içinde. Bu komut, konsol penceresinde herhangi bir çıktı göstermez.

6. Tablodaki girişleri aramak için aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    `hdinsightclustername` değerini kümenizin adıyla değiştirin. İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin.

    Bu komut `SearchByEmail` herhangi bir satır için aranacak sınıfı burada `contactinformation` sütun ailesi ve `email` sütun içeren dize `contoso.com`. Aşağıdaki sonuçlar almanız gerekir:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kullanarak **fabrikam.com** için `-emailRegex` değeri döndürür, kullanıcılar **fabrikam.com** e-posta alanına. Arama terimi olarak normal ifadeler de kullanabilirsiniz. Örneğin, **^ r** e-posta 'r' harfi ile başlayan adresler döndürür.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Sonuçları ya da başlangıç HBaseExample kullanırken, beklenmeyen sonuçlar

Kullanım `-showErr` işi çalıştırılırken üretilen standart hata (STDERR) görüntülemek için parametre.

## <a name="delete-the-table"></a>Tabloyu sil

Örnek ile işiniz bittiğinde, aşağıdaki silmek için kullanın **kişiler** Bu örnekte kullanılan tablo:

__Gelen bir `ssh` oturumu__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Azure powershell'den__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Sonraki adımlar

[SQuirreL SQL, Apache HBase ile kullanmayı öğrenin](apache-hbase-phoenix-squirrel-linux.md)
