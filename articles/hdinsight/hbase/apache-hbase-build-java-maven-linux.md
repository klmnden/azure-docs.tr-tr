---
title: Azure HDInsight için HBase Java istemci oluşturmak için Apache Maven kullanma
description: Apache Maven bir Java tabanlı Apache HBase uygulaması oluşturmayı ve ardından Azure HDInsight üzerinde HBase dağıtmak için kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 04/16/2019
ms.openlocfilehash: a4c601e81390efa3bb53a6f07225bb6e939bc9bb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114419"
---
# <a name="build-java-applications-for-apache-hbase"></a>Apache HBase için Java uygulamaları oluşturun

Oluşturmayı bir [Apache HBase](https://hbase.apache.org/) Java uygulaması. Ardından, Azure HDInsight üzerinde HBase ile uygulamayı kullanın.

Bu adımları belge kullanım [Apache Maven](https://maven.apache.org/) ve projeyi oluşturmak için. Maven, yazılım proje yönetimi ve yazılım, belgelere ve Java projeleri için raporlar oluşturmanıza olanak sağlayan kavramayı Aracı ' dir.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde Apache HBase kümesi. Bkz: [Apache HBase ile çalışmaya başlama](./apache-hbase-tutorial-get-started-linux.md).

* [Java Developer Kit (JDK) 8 sürümünü](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* PowerShell kullanarak, ihtiyacınız olacak [AZ modül](https://docs.microsoft.com/powershell/azure/overview).

* Bir metin düzenleyici. Bu makalede, Microsoft Notepad kullanır.

> [!IMPORTANT]  
> Azure PowerShell cmdlet'lerini [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) ve [Get-AzHDInsightJobOutput](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightjoboutput) şu anda ne zaman iş değil [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) depolama hesabında etkin .

## <a name="test-environment"></a>Test ortamı
Windows 10 çalıştıran bir bilgisayar için bu makalede kullanılan ortamı oluştu.  Komutları komut isteminde yürütüldü ve çeşitli dosyaların Notepad ile düzenlendi. Uygun şekilde değiştirin, ortamınız için.

Bir komut isteminden çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

1. Adlı bir Maven projesi oluşturmak için aşağıdaki komutu girin **hbaseapp**:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    cd hbaseapp
    mkdir conf
    ```

    Bu komut, adlı bir dizin oluşturur. `hbaseapp` geçerli konumunda, temel bir Maven projesi içerir. İkinci komut, çalışma dizinini değiştirir `hbaseapp`. Üçüncü komut yeni bir dizin oluşturur `conf`, doğrulayacak daha sonra. `hbaseapp` Dizini aşağıdaki öğeleri içerir:

    * `pom.xml`:  Proje nesne modeli ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) projeyi oluşturmak için kullanılan bilgiler ve yapılandırma ayrıntılarını içerir.
    * `src\main\java\com\microsoft\examples`: Uygulama kodunuza içerir.
    * `src\test\java\com\microsoft\examples`: Uygulamanız için testleri içerir.

2. Oluşturulan örnek kodu kaldırın. Oluşturulan test ve uygulama dosyalarını silin `AppTest.java`, ve `App.java` aşağıdaki komutları girerek:

    ```cmd
    DEL src\main\java\com\microsoft\examples\App.java
    DEL src\test\java\com\microsoft\examples\AppTest.java
    ```

## <a name="update-the-project-object-model"></a>Proje nesne modeli güncelleştirme

Pom.xml dosyasını tam başvuru için bkz: https://maven.apache.org/pom.html.  Açık `pom.xml` aşağıdaki komutu girerek:

```cmd
notepad pom.xml
```

### <a name="add-dependencies"></a>Bağımlılıkları ekleyin

İçinde `pom.xml`, aşağıdaki metni ekleyin `<dependencies>` bölümü:

```xml
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.phoenix</groupId>
    <artifactId>phoenix-core</artifactId>
    <version>4.14.1-HBase-1.1</version>
</dependency>
```  

Bu bölümde, proje gerektiğini belirtir **hbase istemci** ve **phoenix çekirdek** bileşenleri. Bu bağımlılıklar, derleme zamanında varsayılan Maven deposundan yüklenir. Kullanabileceğiniz [Maven merkezi depo arama](https://search.maven.org/artifact/org.apache.hbase/hbase-client/1.1.2/jar) bu bağımlılık hakkında daha fazla bilgi edinmek için.

> [!IMPORTANT]  
> Hbase istemcisinin sürüm numarası, Apache HBase, HDInsight kümenizle sağlanan sürümünü eşleşmesi gerekir. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.

| HDInsight küme sürümü | Apache HBase sürümü kullanmak için |
| --- | --- |
| 3.6 | 1.1.2 |
| 4.0 | 2.0.0 |

HDInsight sürümleri ve bileşenleri hakkında daha fazla bilgi için bkz. [HDInsight ile kullanılabilen farklı Apache Hadoop bileşenlerinin nelerdir](../hdinsight-component-versioning.md).

### <a name="build-configuration"></a>Derleme yapılandırması

Maven eklentileri proje derleme aşamalarında özelleştirmenizi sağlar. Bu bölümde, eklentiler, kaynaklar ve diğer yapı yapılandırma seçenekleri eklemek için kullanılır.

Aşağıdaki kodu ekleyin `pom.xml` dosyasını kaydedin ve dosyayı kapatın. Bu metin içinde olmalıdır `<project>...</project>` etiketleri dosyasında, örneğin, arasında `</dependencies>` ve `</project>`.

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
                <version>3.8.0</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
        </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
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

### <a name="download-the-hbase-sitexml"></a>Hbase-site.xml indirin

HBase kümesi için HBase yapılandırma kopyalamak için aşağıdaki komutu kullanın `conf` dizin. Değiştirin `CLUSTERNAME` , HDInsight ile küme adı ve ardından komutu girin:

```cmd
scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
```

## <a name="create-the-application"></a>Uygulama oluşturma

### <a name="implement-a-createtable-class"></a>CreateTable sınıf uygulama

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `CreateTable.java`. Seçin **Evet** istemine yeni bir dosya oluşturun.

```cmd
notepad src\main\java\com\microsoft\examples\CreateTable.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın. Dosyayı kaydedip kapatın.

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

Bu kodu `CreateTable` adlı bir tablo oluşturur sınıfı `people` ve önceden tanımlanmış bazı kullanıcılar ile doldurabilirsiniz.

### <a name="implement-a-searchbyemail-class"></a>SearchByEmail sınıf uygulama

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `SearchByEmail.java`. Seçin **Evet** istemine yeni bir dosya oluşturun.

```cmd
notepad src\main\java\com\microsoft\examples\SearchByEmail.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın. Dosyayı kaydedip kapatın.

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

`SearchByEmail` Sınıfı kullanılabilir sorgulamak için e-posta adresine göre satır. Normal ifade filtresi kullandığından, sınıf kullanırken bir dize veya bir normal ifade sağlayabilir.

### <a name="implement-a-deletetable-class"></a>DeleteTable sınıf uygulama

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `DeleteTable.java`. Seçin **Evet** istemine yeni bir dosya oluşturun.

```cmd
notepad src\main\java\com\microsoft\examples\DeleteTable.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın. Dosyayı kaydedip kapatın.

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

`DeleteTable` Sınıfı, bu örnekte, devre dışı bırakma ve tarafından oluşturulan tabloyu bırakarak oluşturduğunuz HBase tablolarını temizler `CreateTable` sınıfı.

## <a name="build-and-package-the-application"></a>Derleme ve uygulamayı paketlemeyi

1. Gelen `hbaseapp` dizin, uygulamayı içeren JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

    ```cmd
    mvn clean package
    ```

    Bu komut, yapılar ve .jar dosyasını uygulamasına paketler.

2. Komut tamamlandığında, `hbaseapp/target` dizin adlı dosyayı içeren `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]  
   > `hbaseapp-1.0-SNAPSHOT.jar` Uber jar dosyasıdır. Bu uygulamayı çalıştırmak için gereken tüm bağımlılıkları içerir.

## <a name="upload-the-jar-and-run-jobs-ssh"></a>Jar dosyasını karşıya yükleme ve işlerinizi (SSH)

Aşağıdaki adımları kullanın `scp` HDInsight kümesinde, Apache HBase, birincil baş düğüme JAR kopyalanacak. `ssh` Komutu ardından kümeye bağlanın ve doğrudan baş düğümünde örneği çalıştırmak için kullanılır.

1. Jar dosyasını kümeye yükleyin. Değiştirin `CLUSTERNAME` , HDInsight ile küme adı ve ardından aşağıdaki komutu girin:

    ```cmd
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

2. HBase kümeye bağlanın. Değiştirin `CLUSTERNAME` , HDInsight ile küme adı ve ardından aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

 3. Java uygulaması kullanarak bir HBase tablosu oluşturmak için aşağıdaki komutu, Open ssh bağlantı kullanın:

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

Azure PowerShell aşağıdaki adımları kullanın [AZ modül](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) varsayılan depolama alanı, Apache HBase kümesi için JAR yükleyebilirsiniz. HDInsight cmdlet'leri, daha sonra örnekleri uzaktan çalıştırmak için kullanılır.

1. Yükleme ve yapılandırma AZ modül oluşturduktan sonra adlı bir dosya `hbase-runner.psm1`. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

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
            Connect-AzAccount
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

2. Kaydet `hbase-runner.psm1` dosyası `hbaseapp` dizin.

3. Modüller, Azure PowerShell ile kaydedin. Yeni bir Azure PowerShell penceresi açın ve aşağıdaki komutu değiştirerek düzenleyin `CLUSTERNAME` değerini kümenizin adıyla. Ardından aşağıdaki komutları girin:

    ```powershell
    cd C:\HDI\hbaseapp
    $myCluster = "CLUSTERNAME"
    Import-Module .\hbase-runner.psm1
    ```

4. Karşıya yüklemek için aşağıdaki komutu kullanın `hbaseapp-1.0-SNAPSHOT.jar` kümenize.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName $myCluster
    ```

    İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin. Komut yükler `hbaseapp-1.0-SNAPSHOT.jar` için `example/jars` kümeniz için birincil depolama konumu.

5. Kullanarak tablo oluşturmak için `hbaseapp`, aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName $myCluster
    ```

    İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin.

    Bu komut, adlı bir tablo oluşturur. **kişiler** HDInsight kümenizdeki HBase içinde. Bu komut, konsol penceresinde herhangi bir çıktı göstermez.

6. Tablodaki girişleri aramak için aşağıdaki komutu kullanın:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName $myCluster -emailRegex contoso.com
    ```

    İstendiğinde, küme oturum açma (Yönetici) adını ve parolasını girin.

    Bu komut `SearchByEmail` herhangi bir satır için aranacak sınıfı burada `contactinformation` sütun ailesi ve `email` sütun içeren dize `contoso.com`. Aşağıdaki sonuçlar almanız gerekir:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kullanarak **fabrikam.com** için `-emailRegex` değeri döndürür, kullanıcılar **fabrikam.com** e-posta alanına. Arama terimi olarak normal ifadeler de kullanabilirsiniz. Örneğin, **^ r** e-posta 'r' harfi ile başlayan adresler döndürür.

7. Tablo silmek için aşağıdaki komutu kullanın:

    ```PowerShell
    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName $myCluster
    ```

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Sonuçları ya da başlangıç HBaseExample kullanırken, beklenmeyen sonuçlar

Kullanım `-showErr` işi çalıştırılırken üretilen standart hata (STDERR) görüntülemek için parametre.

## <a name="next-steps"></a>Sonraki adımlar

[SQuirreL SQL, Apache HBase ile kullanmayı öğrenin](apache-hbase-phoenix-squirrel-linux.md)
