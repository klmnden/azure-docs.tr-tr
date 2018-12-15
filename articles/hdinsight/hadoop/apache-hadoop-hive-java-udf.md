---
title: Java kullanıcı tanımlı işlev (UDF Apache Hive, HDInsight - Azure)
description: Apache Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnek, bir küçük metin dizesi tablosunu UDF dönüştürür.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 6e31062e3965c9ed5a0b24e822f0ec14234cec1c
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435081"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Bir Java kullanma UDF ile Apache Hive HDInsight

Apache Hive ile çalışan bir Java tabanlı kullanıcı tanımlı işlev (UDF) oluşturmayı öğrenin. Bu örnekte Java UDF bir metin dizesi tablosunu tümü küçük harf karakterlere dönüştürür.

## <a name="requirements"></a>Gereksinimler

* Bir HDInsight kümesi 

    > [!IMPORTANT]
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

    Bu belgedeki çoğu adımlar hem Windows ve Linux tabanlı kümelerde çalışır. Ancak, Linux tabanlı kümeler için derlenmiş UDF kümeye karşıya yüklemek ve çalıştırmak için kullanılan adımlarla özgüdür. Windows tabanlı kümeler ile kullanılabilecek bilgilere bağlantılar sağlanır.

* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 veya üzeri (veya OpenJDK gibi eşdeğeri)

* [Apache Maven](https://maven.apache.org/)

* Bir metin düzenleyicisi veya Java IDE

    > [!IMPORTANT]
    > Bir Windows istemcisi üzerinde Python dosyaları oluşturursanız, bir satır sonu olarak LF kullanan bir düzenleyici kullanmanız gerekir. Düzenleyici LF veya CRLF kullanıp emin değilseniz bkz [sorun giderme](#troubleshooting) kaldırmayı CR karakteri adımlar bölümüne.

## <a name="create-an-example-java-udf"></a>Örnek Java UDF oluşturma 

1. Bir komut satırından yeni bir Maven projesi oluşturmak için aşağıdakileri kullanın:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > PowerShell kullanıyorsanız, parametreleri tırnak yerleştirmeniz gerekir. Örneğin, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Bu komut adlı bir dizin oluşturur **exampleudf**, Maven projesi içerir.

2. Proje oluşturulduktan sonra Sil **exampleudf/src/test** projenin bir parçası oluşturulduğu dizin.

3. Açık **exampleudf/pom.xml**ve varolan `<dependencies>` şu XML ile giriş:

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

    Bu girişler, projeyi oluşturmak nasıl tanımlayın. Özellikle, proje kullanan Java ve nasıl bir uberjar Küme dağıtımı için derleme sürümü.

    Değişikliklerinizi yaptıktan sonra dosyayı kaydedin.

4. Yeniden adlandırma **exampleudf/src/main/java/com/microsoft/examples/App.java** için **ExampleUDF.java**ve ardından dosyayı düzenleyicinizde açın.

5. Öğesinin içeriğini değiştirin **ExampleUDF.java** aşağıdaki ile dosya ve ardından dosyayı kaydedin.

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

1. Derleme ve UDF paketlemek için aşağıdaki komutu kullanın:

    ```bash
    mvn compile package
    ```

    Bu komut, yapılar ve paketler halinde UDF `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` dosya.

2. Kullanım `scp` HDInsight kümesine dosya kopyalamak için komutu.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Değiştirin `myuser` kümenizin SSH kullanıcı hesabı ile. Değiştirin `mycluster` küme adı ile. SSH hesabının güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir sertifika kullanıyorsanız, kullanmanız gerekebilir `-i` parametresini kullanarak özel anahtar dosyası belirtin.

3. SSH kullanarak kümeye bağlanın.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. SSH oturumundan, HDInsight depolama alanına jar dosyasını kopyalayın.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>UDF kovanından kullanın

1. SSH oturumundan Beeline istemcisini başlatmak için aşağıdakileri kullanın.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Bu komut varsayılan değer olarak kullanılan varsayar **yönetici** kümeniz için oturum açma hesabı için.

2. Sonra ulaşırsınız `jdbc:hive2://localhost:10001/>` isteminde, UDF için Hive ekleyin ve bir işlev olarak göstermek için aşağıdakileri girin.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > Bu örnek, Azure depolama kümesi için varsayılan depolama alanı olduğunu varsayar. Bunun yerine, küme Data Lake Storage kullanıyorsa, Değiştir `wasb:///` değerini `adl:///`.

3. UDF küçük dizeleri bir tablodan alınan değerlerini dönüştürmek için kullanın.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Bu sorgu istediğiniz cihaz platformunu (Android, Windows, iOS, vb.) tablosundan seçer, düşük durumda ve bunları görüntülemek için bir dizeye dönüştürün. Çıktı aşağıdaki metne benzer görünür:

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

Hive ile çalışmak diğer yolları için bkz. [HDInsight ile Hive kullanma Apache](hdinsight-use-hive.md).

Hive User-Defined işlevleri hakkında daha fazla bilgi için bkz. [Apache Hive işleçleri ve kullanıcı tanımlı işlevler](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Apache.org Hive wiki bölümü.
