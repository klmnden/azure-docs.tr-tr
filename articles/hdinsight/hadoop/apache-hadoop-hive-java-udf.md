---
title: Java kullanıcı tanımlı işlev (UDF Apache Hive, HDInsight - Azure)
description: Apache Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnek, bir küçük metin dizesi tablosunu UDF dönüştürür.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 03/21/2019
ms.date: 04/29/2019
ms.author: v-yiso
ms.openlocfilehash: b8417fe4c15259a7fd485254cf9edd2c8c082e92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60537641"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Bir Java kullanma UDF ile Apache Hive HDInsight

Apache Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnekte Java UDF bir metin dizesi tablosunu tümü küçük harf karakterlere dönüştürür.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight Hadoop kümesinde. Bkz: [Linux'ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).
* [Java Developer Kit (JDK) 8 sürümü](https://aka.ms/azure-jdks)
* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.
* [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kümeleri birincil depolama alanı için. Bu wasb olacaktır: / / abfs olan Azure depolama için: / / Azure Data Lake depolama Gen2'ye veya adl: / / Azure Data Lake depolama Gen1. Güvenli aktarım, Azure Depolama'da veya Data Lake depolama Gen2 için etkinse, URI wasbs olacaktır: / / ya da abfss: / /, sırasıyla ayrıca bakın [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).

* Bir metin düzenleyicisi veya Java IDE

    > [!IMPORTANT]  
    > Bir Windows istemcisi üzerinde Python dosyaları oluşturursanız, bir satır sonu olarak LF kullanan bir düzenleyici kullanmanız gerekir. Düzenleyici LF veya CRLF kullanıp emin değilseniz bkz [sorun giderme](#troubleshooting) kaldırmayı CR karakteri adımlar bölümüne.

## <a name="test-environment"></a>Test ortamı
Windows 10 çalıştıran bir bilgisayar için bu makalede kullanılan ortamı oluştu.  Komutları komut isteminde yürütüldü ve çeşitli dosyaların Notepad ile düzenlendi. Uygun şekilde değiştirin, ortamınız için.

Bir komut isteminden çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-an-example-java-udf"></a>Örnek Java UDF oluşturma

1. Aşağıdaki komutu girerek yeni bir Maven projesi oluşturun:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    Bu komut, adlı bir dizin oluşturur. `exampleudf`, Maven projesi içerir.

2. Proje oluşturulduktan sonra Sil `exampleudf/src/test` aşağıdaki komutu girerek projenin bir parçası oluşturulan dizini:

    ```cmd
    cd ExampleUDF
    rmdir /S /Q "src/test"
    ```

3. Açık `pom.xml` aşağıdaki komutu girerek:

    ```cmd
    notepad pom.xml
    ```

    Ardından varolan `<dependencies>` şu XML ile giriş:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    Bu giriş, Hadoop ve Hive ile HDInsight 3.6 dahil sürümünü belirtin. Hadoop ve Hive HDInsight ile sağlanan sürümleri hakkında bilgi bulabilirsiniz [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md) belge.

    Ekleme bir `<build>` önce bölümünde `</project>` dosyanın sonuna satır. Bu bölümde, aşağıdaki XML içermesi gerekir:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.6  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.1</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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

    Bu girişler, projeyi oluşturmak nasıl tanımlayın. Özellikle, proje kullanan Java ve nasıl bir uberjar Küme dağıtımı için derleme sürümü.

    Değişikliklerinizi yaptıktan sonra dosyayı kaydedin.

4. Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `ExampleUDF.java`:

    ```cmd
    notepad src/main/java/com/microsoft/examples/ExampleUDF.java
    ```

    Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın. Dosyayı kaydedip kapatın.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Bu kod, bir dize değeri kabul eder ve küçük bir sürüm dizesinin döndüren bir UDF uygular.

## <a name="build-and-install-the-udf"></a>Derleme ve UDF yükleyin

Aşağıdaki komutların yerini `sshuser` gerçek kullanıcı farklı olması durumunda. Değiştirin `mycluster` gerçek bir küme adı ile.

1. Derleme ve aşağıdaki komutu girerek UDF paketi:

    ```cmd
    mvn compile package
    ```

    Bu komut, yapılar ve paketler halinde UDF `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` dosya.

2. Kullanım `scp` komut dosyası, aşağıdaki komutu girerek HDInsight kümesine kopyalamak için:

    ```cmd
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight
    ```

3. Aşağıdaki komutu girerek SSH kullanarak kümeye bağlanın:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.cn
    ```

4. Açık SSH oturumundan, HDInsight depolama alanına jar dosyasını kopyalayın.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>UDF kovanından kullanın

1. Beeline istemci SSH oturumunda aşağıdaki komutu girerek başlayın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Bu komut varsayılan değer olarak kullanılan varsayar **yönetici** kümeniz için oturum açma hesabı için.

2. Sonra ulaşırsınız `jdbc:hive2://localhost:10001/>` isteminde, UDF için Hive ekleyin ve bir işlev olarak göstermek için aşağıdakileri girin.

    ```hiveql
    ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

3. UDF küçük dizeleri bir tablodan alınan değerlerini dönüştürmek için kullanın.

    ```hiveql
    SELECT tolower(state) AS ExampleUDF, state FROM hivesampletable LIMIT 10;
    ```

    Bu sorgu durumu tablodan seçer, düşük durumda ve değiştirilmemiş adının yanı sıra bunları görüntülemek için bir dizeye dönüştürün. Çıktı aşağıdaki metne benzer görünür:

        +---------------+---------------+--+
        |  exampleudf   |     state     |
        +---------------+---------------+--+
        | california    | California    |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | utah          | Utah          |
        | utah          | Utah          |
        | colorado      | Colorado      |
        +---------------+---------------+--+

## <a name="troubleshooting"></a>Sorun giderme

Hive işi çalıştırılırken aşağıdaki metne benzer bir hatayla karşılaşabilirsiniz:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Bu sorunun nedeni Python dosyasındaki satır sonlarını. CRLF sondaysa satır ancak Linux uygulamaları genellikle çok sayıda Windows düzenleyicileri varsayılan LF bekler.

HDInsight için dosyayı karşıya yüklemeden önce CR karakter kaldırmak için aşağıdaki PowerShell ifadeleri kullanabilirsiniz:

```PowerShell
# Set $original_file to the python file path
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışmak diğer yolları için bkz. [HDInsight ile Hive kullanma Apache](hdinsight-use-hive.md).

Hive User-Defined işlevleri hakkında daha fazla bilgi için bkz. [Apache Hive işleçleri ve kullanıcı tanımlı işlevler](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Apache.org Hive wiki bölümü.
