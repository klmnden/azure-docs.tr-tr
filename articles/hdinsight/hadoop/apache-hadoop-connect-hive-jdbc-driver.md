---
title: Apache Hive JDBC sürücüsü - Azure HDInsight ile sorgulama
description: HDInsight üzerindeki hadoop, Apache Hive sorguları göndermek için bir Java uygulamasında JDBC sürücüsü kullanın. Programlı bir şekilde ve SQuirrel SQL istemcisi bağlanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: hrasheed
ms.openlocfilehash: 56a2b89277cbf8866c1992a6738bd80106ef3313
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480015"
---
# <a name="query-apache-hive-through-the-jdbc-driver-in-hdinsight"></a>Apache Hive JDBC sürücüsü, HDInsight ile sorgulama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Azure HDInsight Apache hadoop, Apache Hive sorguları göndermek için bir Java uygulamasında JDBC sürücüsü kullanmayı öğrenin. Bu belgedeki bilgiler programlı bir şekilde ve SQuirreL SQL İstemcisi'na nasıl bağlanacağınız gösterilmiştir.

Hive JDBC arabirimi hakkında daha fazla bilgi için bkz. [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Önkoşullar

* Bir HDInsight Hadoop kümesi. Oluşturmak için bkz: [Azure HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* [Java Developer Kit (JDK) sürüm 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html) veya üzeri.
* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL JDBC istemci uygulamasıdır.

## <a name="jdbc-connection-string"></a>JDBC bağlantı dizesi

Azure'da bir HDInsight kümesi için JDBC bağlantı bağlantı noktası 443 üzerinden yapılır ve trafiği SSL kullanılarak güvenlik altına alınır. Ortak ağ geçidi arkasında kümeleri sit HiveServer2 gerçekten dinlediği bağlantı noktasını trafiği yönlendirir. Aşağıdaki bağlantı dizesi, HDInsight için kullanılacak biçimi gösterir:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

`CLUSTERNAME` değerini HDInsight kümenizin adıyla değiştirin.

## <a name="authentication"></a>Kimlik Doğrulaması

Bağlantıyı kurarken, Küme Ağ Geçidi kimlik doğrulaması için HDInsight Küme Yöneticisi adı ve parolası kullanmanız gerekir. SQuirreL SQL gibi JDBC istemcilerinden bağlanırken istemci ayarları'nda Yönetici adını ve parolasını girmeniz gerekir.

Bir Java uygulamasından bir bağlantısı kurulurken adını ve parolayı kullanmanız gerekir. Örneğin, aşağıdaki Java kod, bağlantı dizesi, yönetici adı ve parola kullanarak yeni bir bağlantı açar:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>SQuirreL SQL istemcisi ile bağlanma

SQuirreL SQL uzaktan HDInsight kümenizle Hive sorguları çalıştırmak için kullanılan bir JDBC istemcisidir. Aşağıdaki adımlarda SQuirreL SQL zaten yüklemiş olduğunuz varsayılır.

1. Kümenizden kopyalanacak belirli dosyaları içerecek bir dizin oluşturun.

2. Aşağıdaki betikte değiştirin `sshuser` küme SSH kullanıcı hesabı adı ile.  Değiştirin `CLUSTERNAME` ile HDInsight kümesi adı.  Bir komut satırından önceki adımda oluşturulan çalışma dizininize geçin ve sonra bir HDInsight kümesinden dosyaları kopyalamak için aşağıdaki komutu girin:

    ```cmd
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hadoop-client/{hadoop-auth.jar,hadoop-common.jar,lib/log4j-*.jar,lib/slf4j-*.jar} .

    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hive-client/lib/{commons-codec*.jar,commons-logging-*.jar,hive-*-1.2*.jar,httpclient-*.jar,httpcore-*.jar,libfb*.jar,libthrift-*.jar} .
    ```

3. SQuirreL SQL uygulamayı başlatın. Pencerenin soldan seçin **sürücüleri**.

    ![Pencerenin sol sekmede sürücüleri](./media/apache-hadoop-connect-hive-jdbc-driver/squirreldrivers.png)

4. Simgeleri en üstündeki gelen **sürücüleri** iletişim kutusunda **+** bir sürücüsü oluşturmak için simge.

    ![Sürücüleri simgeleri](./media/apache-hadoop-connect-hive-jdbc-driver/driversicons.png)

5. Sürücü Ekle iletişim kutusunda aşağıdaki bilgileri ekleyin:

    * **Ad**: Hive
    * **Örnek URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Yol'ekstra sınıf**: Kullanım **Ekle** jar dosyalarının tümü eklemek için daha önce indirilen
    * **Sınıf adı**: org.apache.hive.jdbc.HiveDriver

   ![sürücü iletişim kutusu Ekle](./media/apache-hadoop-connect-hive-jdbc-driver/adddriver.png)

   Seçin **Tamam** bu ayarları kaydedin.

6. SQuirreL SQL pencerenin sol tarafında seçin **diğer adlar**. Ardından **+** bağlantı diğer adı oluşturmak için simge.

    ![Yeni diğer ad ekleyin](./media/apache-hadoop-connect-hive-jdbc-driver/aliases.png)

7. İçin aşağıdaki değerleri kullanın **diğer ad Ekle** iletişim.

    * **Ad**: HDInsight üzerinde hive

    * **Sürücü**: Seçmek için açılan listeyi kullanın **Hive** sürücü

    * **URL**: `jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`

        **CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

    * **Kullanıcı adı**: HDInsight kümeniz için küme oturum açma hesabı adı. Varsayılan değer: `admin`.

    * **Parola**: Küme oturum açma hesabı için parola.

   ![diğer iletişim kutusu Ekle](./media/apache-hadoop-connect-hive-jdbc-driver/addalias.png)

    > [!IMPORTANT] 
    > Kullanım **Test** bağlantının çalıştığını doğrulamak için düğmeyi. Zaman **bağlanın: HDInsight üzerinde Hive** iletişim kutusu görüntülenirse, seçin **Connect** testi gerçekleştirmek için. Test başarılı olursa, gördüğünüz bir **bağlantı başarılı** iletişim. Bir hata oluşursa bkz [sorun giderme](#troubleshooting).

    Bağlantı diğer kaydetmek için kullanın **Tamam** düğme alttaki **diğer ad Ekle** iletişim.

8. Gelen **bağlanma** SQuirreL SQL üst kısmındaki açılan menüyü seçin **üzerinde HDInsight Hive**. Sorulduğunda, **Connect**.

    ![Bağlantı iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/connect.png)

9. Bağlantı kurulduktan sonra SQL sorgu iletişim kutusuna aşağıdaki sorguyu girin ve ardından **çalıştırma** simgesi (çalışan bir kişi). Sonuçları alan sorgu sonuçlarını göstermelidir.

    ```hql
    select * from hivesampletable limit 10;
    ```

    ![Sonuçları dahil olmak üzere sql sorgu iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Bir örnek Java uygulamasını Bağlan

HDInsight üzerinde bir Hive sorgusu için Java istemci kullanma örneği kullanılabilir [ https://github.com/Azure-Samples/hdinsight-java-hive-jdbc ](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Depo derlemek ve örnek çalıştırmak için yönergeleri izleyin.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Bir SQL bağlantı açılmaya çalışılırken beklenmeyen bir hata oluştu

**Belirtiler**: Sürüm 3.3 ya da daha büyük olan bir HDInsight kümesine bağlanırken beklenmeyen bir hata oluştu. bir hata alabilirsiniz. Bu hata için yığın izlemesi şu satır ile başlar:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Neden**: Bu hata eski bir sürüm commons codec.jar dosya ile SQuirreL dahil kaynaklanır.

**Çözüm**: Bu hatayı düzeltmek için aşağıdaki adımları kullanın:

1. SQuirreL çıkın ve ardından SQuirreL sisteminizde yüklü olduğu dizine gidin. SquirreL dizinde altında `lib` dizini mevcut commons codec.jar bir HDInsight kümesinden indirilen değiştirin.

2. SQuirreL yeniden başlatın. HDInsight üzerindeki hive'a bağlanma sırasında bu hata artık gerçekleşmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışmak için JDBC kullanmayı öğrendiniz, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri görselleştirme](apache-hadoop-connect-hive-power-bi.md).
* [Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için karşıya veri yükleme](../hdinsight-upload-data.md)
* [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
