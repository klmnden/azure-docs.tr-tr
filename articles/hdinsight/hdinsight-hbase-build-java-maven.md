---
title: Windows tabanlı Azure HDInsight için HBase Java uygulaması derleme
description: Apache Maven bir Java tabanlı Apache HBase uygulaması oluşturmayı ve ardından bir Windows tabanlı Azure HDInsight kümesine dağıtmak için kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: c0ecfd3f148cecae713740ef37d4fe7a2e2f184f
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361821"
---
# <a name="use-apache-maven-to-build-java-applications-that-use-apache-hbase-with-windows-based-hdinsight-apache-hadoop"></a>Windows tabanlı HDInsight (Apache Hadoop) ile Apache HBase kullanan Java uygulamaları oluşturmak için Apache Maven kullanma
Öğrenin ve oluşturmak bir [Apache HBase](https://hbase.apache.org/) Apache Maven kullanarak Java uygulaması. Ardından uygulama, Azure HDInsight (Apache Hadoop) ile kullanın.

[Apache Maven](https://maven.apache.org/) yazılım, belgelere ve Java projeleri için raporlar oluşturmanıza olanak tanıyan bir yazılım proje yönetimi ve içeriği kavrama aracıdır. Bu makalede, sorgular, oluşturur ve bir Azure HDInsight kümesindeki bir HBase tablosu siler temel bir Java uygulaması oluşturmak için bunu kullanın öğrenin.

> [!IMPORTANT]  
> Bu belgedeki adımlarda Windows kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Gereksinimler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* [Java platformu JDK](https://aka.ms/azure-jdks) 7 veya üzeri
* [Apache Maven](https://maven.apache.org/)
* HBase ile bir Windows tabanlı HDInsight kümesi

    > [!NOTE]  
    > Bu belgedeki adımlarda HDInsight kümesi sürüm 3.2 ve 3.3 ile test edilmiştir. Örneklerde sağlanan varsayılan 3.3 HDInsight kümesi için değerler.

## <a name="create-the-project"></a>Proje oluşturma
1. Geliştirme ortamınızdaki komut satırından, dizinleri, bu gibi bir durumda projesini oluşturmak istediğiniz konuma değiştirin `cd code\hdinsight`.
2. Kullanım **mvn** proje için yapı iskelesini oluşturmak için Maven ile yüklenir, komutu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Bu komut bir dizin tarafından belirtilen ada sahip geçerli konumda oluşturur **Artifactıd** parametre (**hbaseapp** Bu örnekte.) Bu dizin, aşağıdaki öğeleri içerir:

   * **pom.xml**:  Proje nesne modeli ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) projeyi oluşturmak için kullanılan bilgiler ve yapılandırma ayrıntılarını içerir.
   * **src**: İçeren dizine **main\java\com\microsoft\examples** burada geliştirdiğiniz uygulama dizini.
3. Silme **src\test\java\com\microsoft\examples\apptest.java** Bu örnekte kullanılmadığından dosya.

## <a name="update-the-project-object-model"></a>Proje nesne modeli güncelleştirme
1. Düzen **pom.xml** dosya ve içine aşağıdaki kodu ekleyin `<dependencies>` bölümü:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Bu bölümde Maven proje gerektirdiğini belirten **hbase istemci** sürüm **1.1.2**. Bu bağımlılık, derleme zamanında varsayılan Maven deposundan yüklenir. Kullanabileceğiniz [Apache Maven merkezi depo arama](https://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) bu bağımlılık hakkında daha fazla bilgi edinmek için.

   > [!IMPORTANT]  
   > Sürüm numarası, HDInsight kümenizle sağlanan HBase sürümü aynı olmalıdır. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.
   >
   >

   | HDInsight küme sürümü | HBase sürümü kullanmak için |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    HDInsight sürümleri ve bileşenleri hakkında daha fazla bilgi için bkz. [HDInsight ile kullanılabilen farklı Apache Hadoop bileşenlerinin nelerdir](hdinsight-component-versioning.md).
2. Bir HDInsight 3.3 kümesi kullanıyorsanız, aşağıdaki da eklemelisiniz `<dependencies>` bölümü:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Bu bağımlılık Hbase sürümü tarafından kullanılan phoenix çekirdek bileşenleri yüklenecektir 1.1.x.
3. Aşağıdaki kodu ekleyin **pom.xml** dosya. Bu bölüm içinde olmalıdır `<project>...</project>` etiketleri dosyasında, örneğin, arasında `</dependencies>` ve `</project>`.

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
                    <source>1.7</source>
                    <target>1.7</target>
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

    `<resources>` Bölümü, bir kaynak yapılandırır (**conf\hbase-site.xml**), HBase için yapılandırma bilgilerini içerir.

   > [!NOTE]  
   > Ayrıca, kod aracılığıyla yapılandırma değerlerini ayarlayabilirsiniz. Bölümündeki yorumlara bakın **CreateTable** nasıl yapılacağı aşağıdaki örnek.
   >
   >

    Bu `<plugins>` bölümü yapılandırır [Apache Maven derleme eklentisini](https://maven.apache.org/plugins/maven-compiler-plugin/) ve [Apache Maven gölge eklentisi](https://maven.apache.org/plugins/maven-shade-plugin/). Eklenti derleyici topoloji derlemek için kullanılır. Eklenti gölge Maven tarafından oluşturulan JAR paketi lisans yinelenmesini önlemek için kullanılır. Bu nedeni, yinelenen lisans dosyaları üzerinde HDInsight kümesi çalışma zamanında hataya neden olmasıdır. Gölge maven plugin ile kullanarak `ApacheLicenseResourceTransformer` uygulama bu hatayı önler.

    Maven gölge eklentisi uber jar dosyasını (veya fat jar) üretir uygulamanın gerektirdiği tüm bağımlılıkları içerir.
4. **Pom.xml** dosyasını kaydedin.
5. Adlı yeni bir dizin oluşturma **conf** içinde **hbaseapp** dizin. İçinde **conf** dizin adlı bir dosya oluşturun **hbase-site.xml**. Dosyanın içeriğini aşağıdakileri kullanın:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     https://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Bu dosya, bir HDInsight kümesi için HBase Yapılandırması'nı yüklemek için kullanılır.

   > [!NOTE]  
   > Bir minimal hbase-site.xml dosyasının budur ve HDInsight kümesi için tam minimum ayarları içerir.

6. Kaydet **hbase-site.xml** dosya.

## <a name="create-the-application"></a>Uygulama oluşturma
1. Git **hbaseapp\src\main\java\com\microsoft\examples** dizin ve yeniden adlandırma app.java dosyasını **CreateTable.java**.
2. Açık **CreateTable.java** dosya ve mevcut içeriğini aşağıdaki kodla değiştirin:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    Bu **CreateTable** adlı bir tablo oluşturur ve sınıf **kişiler** ve önceden tanımlanmış bazı kullanıcılar ile doldurabilirsiniz.
3. Kaydet **CreateTable.java** dosya.
4. İçinde **hbaseapp\src\main\java\com\microsoft\examples** dizin adında yeni bir dosya oluşturun **SearchByEmail.java**. Bu dosyanın içeriği aşağıdaki kodu kullanın:

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

            // Create a new regex filter
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

    **SearchByEmail** sınıfı kullanılabilir sorgulamak için e-posta adresine göre satır. Normal ifade filtresi kullandığından, sınıf kullanırken bir dize veya bir normal ifade sağlayabilir.
5. Kaydet **SearchByEmail.java** dosya.
6. İçinde **hbaseapp\src\main\hava\com\microsoft\examples** dizin adında yeni bir dosya oluşturun **DeleteTable.java**. Bu dosyanın içeriği aşağıdaki kodu kullanın:

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

    Bu örnekte devre dışı bırakma ve tarafından oluşturulan tabloyu bırakarak temizleme işlemi için bu sınıf, **CreateTable** sınıfı.
7. Kaydet **DeleteTable.java** dosya.

## <a name="build-and-package-the-application"></a>Derleme ve uygulamayı paketlemeyi
1. Bir komut istemi açın ve dizinleri **hbaseapp** dizin.
2. Uygulamayı içeren JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

        mvn clean package

    Bu herhangi bir önceki derleme yapıtları temizler, zaten yüklü tüm bağımlılıkları indirir, sonra oluşturur ve uygulama paketleri.
3. Komut tamamlandığında, **hbaseapp\target** dizin adlı dosyayı içeren **hbaseapp 1.0 SNAPSHOT.jar**.

   > [!NOTE]  
   > **Hbaseapp 1.0 SNAPSHOT.jar** dosyasıdır bir uber uygulamayı çalıştırmak için gerekli tüm bağımlılıklarını içeren jar (fat bir jar denir).

## <a name="upload-the-jar-file-and-start-a-job"></a>JAR dosyasını karşıya yükleyin ve bir proje başlatın
HDInsight kümenize bir dosyayı karşıya yüklemek için birçok yolu vardır açıklandığı [HDInsight Apache Hadoop işleri için verileri karşıya yükleme](hdinsight-upload-data.md). Aşağıdaki adımlar, Azure PowerShell kullanırsınız.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Yükleme ve Azure PowerShell, yapılandırma oluşturduktan sonra adlı yeni bir dosya **hbase runner.psm1**. Bu dosyanın içeriği aşağıdakileri kullanın:

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

    Bu dosya iki modülü içerir:

   * **Ekleme HDInsightFile** - HDInsight için dosyaları karşıya yüklemek için kullanılır
   * **Başlangıç HBaseExample** - daha önce oluşturulan sınıflar çalıştırmak için kullanılır
2. Kaydet **hbase runner.psm1** dosya.
3. Yeni bir Azure PowerShell penceresi açın, dizinleri **hbaseapp** dizin ve ardından aşağıdaki komutu çalıştırın.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Yolun konumuyla değiştirin **hbase runner.psm1** daha önce oluşturulan dosya. Bu modül için bu Azure PowerShell oturumu kaydeder.
4. Karşıya yüklemek için aşağıdaki komutu kullanın **hbaseapp 1.0 SNAPSHOT.jar** HDInsight kümenize.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Değiştirin **hdınsightkümesiadı** HDInsight kümenizin adıyla. Komut yükler **hbaseapp 1.0 SNAPSHOT.jar** için **örnek/jar dosyaları dışındaki** HDInsight kümeniz için birincil depolama konumu.
5. Dosyalar karşıya yüklendikten sonra kullanarak tablo oluşturmak için aşağıdaki kodu kullanın **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Değiştirin **hdınsightkümesiadı** HDInsight kümenizin adıyla.

    Bu komut adlı yeni bir tablo oluşturur **kişiler** HDInsight kümenizdeki. Bu komut, konsol penceresinde herhangi bir çıktı göstermez.
6. Tablodaki girişleri aramak için aşağıdaki komutu kullanın:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Değiştirin **hdınsightkümesiadı** HDInsight kümenizin adıyla.

    Bu komut **SearchByEmail** herhangi bir satır için aranacak sınıfı burada **contactinformation** sütun ailesi ve **e-posta** sütun içeren dize **contoso.com**. Aşağıdaki sonuçlar almanız gerekir:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kullanarak **fabrikam.com** için `-emailRegex` değeri döndürür, kullanıcılar **fabrikam.com** e-posta alanına. Bu arama, normal bir ifade tabanlı filtresini kullanarak uygulandığından da normal ifadeler gibi girebilirsiniz **^ r**, hangi döndürür girişleri nerede e-posta ile başlayan 'r'.

## <a name="delete-the-table"></a>Tabloyu sil
Örnek ile işiniz bittiğinde, silmek için Azure PowerShell oturumunda aşağıdaki komutu kullanın **kişiler** Bu örnekte kullanılan tablo:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Değiştirin **hdınsightkümesiadı** HDInsight kümenizin adıyla.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Sonuçları ya da başlangıç HBaseExample kullanırken, beklenmeyen sonuçlar
Kullanım `-showErr` işi çalıştırılırken üretilen standart hata (STDERR) görüntülemek için parametre.
