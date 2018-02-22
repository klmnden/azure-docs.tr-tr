---
title: "Java kullanıcı tanımlı işlev (UDF hdınsight'ta - Azure Hive ile) | Microsoft Docs"
description: "Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnekte küçük metin dizelerini tablosu UDF dönüştürür."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/20/2018
ms.author: larryfr
ms.openlocfilehash: cc6b63f40dd4323ecdf0dfb75c7c8036cbc7b0b7
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Bir Java kullanma UDF hdınsight'ta Hive ile

Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnekte Java UDF metin dizelerini tablosu tüm küçük karakterlerine dönüştürür.

## <a name="requirements"></a>Gereksinimler

* Hdınsight kümesi 

    > [!IMPORTANT]
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

    Bu belgedeki çoğu adımlar her iki Windows ve Linux tabanlı kümelerde çalışır. Ancak, Linux tabanlı kümeler için derlenmiş UDF kümeye karşıya yüklemek ve çalıştırmak için kullanılan adımlarla özgüdür. Windows tabanlı kümeler ile kullanılabilir bilgilere bağlantılar sağlanmaktadır.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 veya sonraki (veya eşdeğeri, OpenJDK gibi bir)

* [Apache Maven](http://maven.apache.org/)

* Bir metin düzenleyicisi veya Java IDE

    > [!IMPORTANT]
    > Windows İstemcisi Python dosyalarda oluşturursanız, satır bitiş olarak LF kullanan bir Düzenleyicisi'ni kullanmanız gerekir. Düzenleyicinizi LF veya CRLF kullanıp kullanmadığını emin değilseniz bkz [sorun giderme](#troubleshooting) CR karakteri kaldırma adımları için bölüm.

## <a name="create-an-example-java-udf"></a>Java UDF örnek oluşturma 

1. Bir komut satırından yeni bir Maven projesi oluşturmak için aşağıdakileri kullanın:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > PowerShell kullanıyorsanız, parametreleri tırnak konulmalıdır. Örneğin, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Bu komut adlı bir dizin oluşturur **exampleudf**, Maven projesine içerir.

2. Proje oluşturulduktan sonra Sil **exampleudf/src/test** projenin bir parçası oluşturulan dizini.

3. Açık **exampleudf/pom.xml**ve varolan `<dependencies>` aşağıdaki XML girişi:

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

    Bu girişler Hadoop ve Hdınsight 3.5 ile birlikte dahil Hive sürümünü belirtin. Hadoop ve Hdınsight ile sağlanan Hive sürümleri hakkında bilgi bulabilirsiniz [Hdınsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md) belge.

    Ekleme bir `<build>` önce bölümünde `</project>` dosyasının son satırında. Bu bölümde aşağıdaki XML içermelidir:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.5  -->
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
                <version>2.3</version>
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

    Bu girişler Projeyi derlemek nasıl tanımlayın. Özellikle, sürümü proje kullanan Java ve Küme dağıtımı için bir uberjar oluşturma.

    Değişiklikler yapıldıktan sonra dosyayı kaydedin.

4. Yeniden Adlandır **exampleudf/src/main/java/com/microsoft/examples/App.java** için **ExampleUDF.java**ve düzenleyicinizde dosyasını açın.

5. Değiştir **ExampleUDF.java** dosya şununla ardından dosyayı kaydedin.

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

    Bu kod bir dize değeri kabul eder ve dize küçük harfli sürümünü döndüren bir UDF uygular.

## <a name="build-and-install-the-udf"></a>Derleme ve UDF yükleyin

1. Derleme ve UDF paket için aşağıdaki komutu kullanın:

    ```bash
    mvn compile package
    ```

    Bu komut oluşturur ve UDF içine paketler `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` dosya.

2. Kullanım `scp` dosyayı Hdınsight kümesine kopyalamak için komutu.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Değiştir `myuser` kümeniz için SSH kullanıcı hesabıyla. Değiştir `mycluster` küme adı ile. SSH hesabını güvenli hale getirmek için parola kullandıysanız parolayı girmeniz istenir. Bir sertifika kullanılması durumunda kullanmanız gerekebilir `-i` parametresini kullanarak özel anahtar dosyası belirtin.

3. SSH kullanarak kümeye bağlanın.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. SSH oturumundan jar dosyasını Hdınsight depolama alanına kopyalayın.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>UDF kovanından kullanın

1. SSH oturumundan Beeline istemci başlatmak için aşağıdakileri kullanın.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Bu komut varsayılan değer olarak kullanılan varsayar **yönetici** kümeniz için oturum açma hesabının.

2. Konumundaki gelmesini sonra `jdbc:hive2://localhost:10001/>` isteminde, UDF kovana ekleyin ve bir işlevi olarak kullanıma sunmak için aşağıdakileri girin.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > Bu örnek, Azure depolama kümesi için varsayılan depolama olduğunu varsayar. Bunun yerine, küme Data Lake Store kullanıyorsa, değiştirme `wasb:///` değeri `adl:///`.

3. UDF küçük dizelere tablodan alınan değerleri dönüştürmek için kullanın.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Bu sorgu cihaz Platformu (Android, Windows, iOS, vb.) tablosundan seçer, düşük durumda ve bunları görüntülemek için dize dönüştürün. Çıktı aşağıdakine benzer görünür:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışmak diğer yolları için bkz: [Hdınsight ile Hive kullanma](hdinsight-use-hive.md).

Hive User-Defined işlevler hakkında daha fazla bilgi için bkz: [Hive işleçler ve kullanıcı tanımlı işlevler](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Apache.org Hive wiki bölümü.
