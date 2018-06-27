---
title: Azure Hdınsight için Windows tabanlı bir Java HBase uygulaması oluşturma | Microsoft Docs
description: Apache Maven bir Java tabanlı Apache HBase uygulaması oluşturma ve bir Windows tabanlı Azure Hdınsight kümesine dağıtmak için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f4a4e02-45ab-40dd-842b-3ec034f256c9
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f5a8c33f2bd3c89b1049435f15d7bf2020248afa
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019332"
---
# <a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Windows tabanlı Hdınsight (Hadoop) ile HBase kullanan Java uygulamaları oluşturmak için Maven kullanın
Oluşturma ve derleme öğrenin bir [Apache HBase](http://hbase.apache.org/) Apache Maven kullanarak Java uygulaması. Daha sonra uygulamayı Azure Hdınsight (Hadoop) ile kullanın.

[Maven](http://maven.apache.org/) , yazılım, belge ve Java projeleri için raporlar oluşturmanıza olanak sağlayan bir yazılım proje yönetimi ve kavrama aracıdır. Bu makalede, sorguları, oluşturur ve Azure Hdınsight kümesinde bir HBase tablosu siler temel bir Java uygulaması oluşturmak üzere kullanmak nasıl öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Windows kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Gereksinimler
* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 veya üzeri
* [Maven](http://maven.apache.org/)
* HBase ile Windows tabanlı Hdınsight kümesi

    > [!NOTE]
    > Bu belgede yer alan adımlar Hdınsight kümesi sürüm 3.2 ve 3.3 ile test edilmiştir. Örneklerde sağlanan varsayılan bir Hdınsight 3.3 küme için değerlerdir.

## <a name="create-the-project"></a>Proje oluşturma
1. Geliştirme ortamınızı komut satırından dizinleri, bu gibi bir durumda projesi oluşturmak istediğiniz konuma değiştirin `cd code\hdinsight`.
2. Kullanım **mvn** proje için askılamayı oluşturmak için Maven ile yüklü komutu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Bu komut bir dizin tarafından belirtilen ada sahip geçerli konumda oluşturur **Artifactıd** parametre (**hbaseapp** Bu örnekte.) Bu dizin, aşağıdaki öğeleri içerir:

   * **pom.xml**: Proje nesne modeli ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) Projeyi derlemek için kullanılan bilgileri ve yapılandırma ayrıntılarını içerir.
   * **src**: içeren dizine **main\java\com\microsoft\examples** burada yazarın uygulama dizini.
3. Silme **src\test\java\com\microsoft\examples\apptest.java** Bu örnekte kullanılmadığı için dosya.

## <a name="update-the-project-object-model"></a>Güncelleştirme projesi nesne modeli
1. Düzen **pom.xml** dosya ve aşağıdaki kodu ekleyin `<dependencies>` bölümü:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Bu bölümde Maven projesi gerektirdiğini söyler **hbase istemci** sürüm **1.1.2**. Derleme zamanında bu bağımlılığı varsayılan Maven deposundan yüklenir. Kullanabileceğiniz [Maven merkezi deposu arama](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) bu bağımlılığı hakkında daha fazla bilgi edinmek için.

   > [!IMPORTANT]
   > Sürüm numarası Hdınsight kümenizle sağlanan HBase sürümü aynı olmalıdır. Doğru sürüm numarasını bulmak için aşağıdaki tabloyu kullanın.
   >
   >

   | Hdınsight küme sürümü | HBase sürümü kullanmak için |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    Hdınsight sürümleri ve bileşenleri hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanılabilir farklı Hadoop bileşenleri nelerdir](hdinsight-component-versioning.md).
2. Bir Hdınsight 3.3 kümesi kullanıyorsanız, aşağıdakiler için de eklemeniz gerekir `<dependencies>` bölümü:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Bu bağımlılık Hbase sürümü tarafından kullanılan phoenix çekirdek bileşenlerini yükler 1.1.x.
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

    `<resources>` Bölüm kaynak yapılandırır (**conf\hbase-site.xml**) HBase için yapılandırma bilgilerini içerir.

   > [!NOTE]
   > Kod aracılığıyla yapılandırma değerlerini de ayarlayabilirsiniz. Yer alan yorumlara bakın **CreateTable** bunun nasıl yapılacağı için aşağıdaki örneği.
   >
   >

    Bu `<plugins>` bölüm yapılandırır [Maven derleyici eklentisi](http://maven.apache.org/plugins/maven-compiler-plugin/) ve [Maven gölge eklentisi](http://maven.apache.org/plugins/maven-shade-plugin/). Eklenti derleyici topoloji derlemek için kullanılır. Eklenti gölge lisans çoğaltma Maven tarafından oluşturulmuş JAR paketindeki önlemek için kullanılır. Bu kullanılır yinelenen lisans dosyaları Hdınsight kümesinde çalışma zamanında bir hataya neden nedenidir. Gölge maven eklentisi ile kullanarak `ApacheLicenseResourceTransformer` uygulama bu hatayı önler.

    Maven gölge eklentisi ayrıca bir uber jar (veya fat jar) üreten uygulama tarafından istenen tüm bağımlılıkları içerir.
4. **Pom.xml** dosyasını kaydedin.
5. Adlı yeni bir dizin oluşturun **conf** içinde **hbaseapp** dizini. İçinde **conf** dizin adlı bir dosya oluşturun **hbase-site.xml**. Aşağıdaki dosyasının içeriği kullanın:

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
          *     http://www.apache.org/licenses/LICENSE-2.0
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

    Bu dosya, Hdınsight kümesi için HBase yapılandırmayı yüklemek için kullanılır.

   > [!NOTE]
   > Bu en az hbase-site.xml bir dosyadır ve Hdınsight kümesi için tam minimum ayarları içerir.

6. Kaydet **hbase-site.xml** dosya.

## <a name="create-the-application"></a>Uygulama oluşturma
1. Git **hbaseapp\src\main\java\com\microsoft\examples** dizin ve yeniden adlandırma app.java dosyasını **CreateTable.java**.
2. Açık **CreateTable.java** dosya ve varolan içeriğini aşağıdaki kodla değiştirin:

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

    Bu **CreateTable** adlı bir tablo oluşturacaksınız sınıfı **kişiler** ve önceden tanımlanmış bazı kullanıcılar ile doldurabilirsiniz.
3. Kaydet **CreateTable.java** dosya.
4. İçinde **hbaseapp\src\main\java\com\microsoft\examples** dizin adlı yeni bir dosya oluşturun **SearchByEmail.java**. Aşağıdaki kod bu dosyanın içeriğini kullanın:

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

    **SearchByEmail** sınıfı kullanılabilir e-posta adresine göre satırlar için sorgulanamıyor. Normal ifade filtresi kullandığından, sınıf kullanırken bir dize veya normal bir ifade belirtebilirsiniz.
5. Kaydet **SearchByEmail.java** dosya.
6. İçinde **hbaseapp\src\main\hava\com\microsoft\examples** dizin adlı yeni bir dosya oluşturun **DeleteTable.java**. Aşağıdaki kod bu dosyanın içeriğini kullanın:

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

    Bu örnek devre dışı bırakma ve tarafından oluşturulan tablo bırakarak temizleme işlemi için bu sınıf, **CreateTable** sınıfı.
7. Kaydet **DeleteTable.java** dosya.

## <a name="build-and-package-the-application"></a>Yapı ve uygulama paketi
1. Bir komut istemi açın ve dizinleri değiştirmek **hbaseapp** dizini.
2. Uygulamayı içeren JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

        mvn clean package

    Bu önceki yapı yapıtların temizler, zaten yüklü tüm bağımlılıkları indirir, ardından oluşturur ve uygulama paketleri.
3. Komut tamamlandığında **hbaseapp\target** dizini içeren adlı bir dosya **hbaseapp 1.0 SNAPSHOT.jar**.

   > [!NOTE]
   > **Hbaseapp 1.0 SNAPSHOT.jar** dosyasıdır bir uber uygulamayı çalıştırmak için gerekli olan tüm bağımlılıkları içeren jar (fat jar de denir).

## <a name="upload-the-jar-file-and-start-a-job"></a>JAR dosyasını karşıya yükleyin ve bir işi Başlat
Hdınsight kümenize bir dosyayı karşıya yüklemeyi birçok yolu vardır açıklandığı gibi [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](hdinsight-upload-data.md). Azure PowerShell aşağıdaki adımları kullanın.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Yükleme ve Azure PowerShell, yapılandırma oluşturduktan sonra adlı yeni bir dosya **hbase runner.psm1**. Bu dosyanın içeriğini aşağıdakileri kullanın:

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
                throw "No active Azure subscription found! If you have a subscription, use the Connect-AzureRmAccount cmdlet to login to your subscription."
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

    Bu dosya iki modülleri içerir:

   * **Ekleme HDInsightFile** - Hdınsight'a dosyaları yüklemek için kullanılan
   * **Başlangıç HBaseExample** - daha önce oluşturduğunuz sınıfları çalıştırmak için kullanılan
2. Kaydet **hbase runner.psm1** dosya.
3. Yeni bir Azure PowerShell penceresi açın, dizinleri değiştirmek **hbaseapp** dizin ve aşağıdaki komutu çalıştırın.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Konumuna yolunu değiştirmek **hbase runner.psm1** daha önce oluşturduğunuz dosya. Bu modül bu Azure PowerShell oturumu için kaydeder.
4. Karşıya yüklemek için aşağıdaki komutu kullanın **hbaseapp 1.0 SNAPSHOT.jar** Hdınsight kümenize.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Değiştir **hdinsightclustername** Hdınsight kümenizin adıyla. Komut yükler **hbaseapp 1.0 SNAPSHOT.jar** için **örnek/Kavanoz** Hdınsight kümeniz için birincil depolama konumu.
5. Dosyaları karşıya yükledikten sonra aşağıdaki kodu kullanarak bir tablo oluşturmak için kullanmak **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Değiştir **hdinsightclustername** Hdınsight kümenizin adıyla.

    Bu komut adlı yeni bir tablo oluşturur **kişiler** Hdınsight kümenizdeki. Bu komut, konsol penceresinde herhangi bir çıktı göstermez.
6. Tablosunda giriş aramak için aşağıdaki komutu kullanın:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Değiştir **hdinsightclustername** Hdınsight kümenizin adıyla.

    Bu komut kullanır **SearchByEmail** sınıfı için herhangi bir satır arama nerede **contactinformation** sütun ailesi ve **e-posta** sütun dizeiçeriyor**contoso.com**. Aşağıdaki sonuçları almanız gerekir:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kullanarak **fabrikam.com** için `-emailRegex` değeri döndürür kullanıcıları **fabrikam.com** e-posta alanında. Bu arama normal ifade tabanlı filtre kullanarak uygulandığından da normal ifadeler gibi girebilirsiniz **^ r**, e-posta 'r' harfle başladığı hangi döndürür girişleri.

## <a name="delete-the-table"></a>Tablo silme
Örneğiyle bittiğinde silmek için Azure PowerShell oturumunda aşağıdaki komutu kullanın **kişiler** Bu örnekte kullanılan tablo:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Değiştir **hdinsightclustername** Hdınsight kümenizin adıyla.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Hiçbir sonuçları veya başlangıç HBaseExample kullanırken beklenmeyen sonuçlar
Kullanım `-showErr` işi çalıştırılırken üretilen standart hata (STDERR) görüntülemek için parametre.
