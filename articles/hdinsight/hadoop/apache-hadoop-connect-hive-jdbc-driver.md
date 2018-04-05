---
title: JDBC sürücüsü aracılığıyla - Azure Hdınsight Hive sorgusu | Microsoft Docs
description: Hdınsight'ta Hadoop Hive sorguları göndermek için bir Java uygulamasında JDBC sürücüsü kullanın. Program aracılığıyla ve SQuirrel SQL istemciden bağlanın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/02/2018
ms.author: larryfr
ms.openlocfilehash: 876d6169f1ecb2f9cdecc59f3f7c8d0a82a8fe7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a>Hdınsight'ta JDBC sürücüsü aracılığıyla Hive sorgusu

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Azure hdınsight'ta Hadoop Hive sorguları göndermek için bir Java uygulamasında JDBC sürücüsü kullanmayı öğrenin. Bu belgedeki bilgiler program aracılığıyla ve SQuirrel SQL istemciden nasıl bağlayacağınızı gösterir.

Hive JDBC arabirimi hakkında daha fazla bilgi için bkz: [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Önkoşullar

* Hdınsight kümesi Hadoop'ta.

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight 3.3 devre dışı bırakma](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL JDBC istemci uygulamasıdır.

* [Java Geliştirme Seti (JDK) sürüm 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) ya da daha yüksek.

* [Apache Maven](https://maven.apache.org). Maven bir projeyi derleme bu makaleyle ilişkili projesi tarafından kullanılan sistem Java projeleri için ' dir.

## <a name="jdbc-connection-string"></a>JDBC bağlantı dizesi

Azure Hdınsight kümesinde JDBC bağlantı üzerinde 443 yapılır ve trafiği güvenliğinin SSL ile. Arkasında kümeleri sit ortak ağ geçidi trafiği HiveServer2 gerçekten dinlediği bağlantı noktasına yönlendirir. Aşağıdaki bağlantı dizesini Hdınsight için kullanılacak biçimi gösterir:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

`CLUSTERNAME` değerini HDInsight kümenizin adıyla değiştirin.

## <a name="authentication"></a>Kimlik Doğrulaması

Bağlantıyı kurarken, Küme Ağ Geçidi kimlik doğrulaması yapmak için Hdınsight Küme Yönetici adı ve parola kullanmanız gerekir. SQuirreL SQL gibi JDBC istemcilerinden bağlanırken istemci ayarları'nda yönetici adı ve parola girmeniz gerekir.

Bir Java uygulamasında bir bağlantı kurulurken adını ve parolasını kullanmanız gerekir. Örneğin, aşağıdaki Java kod bağlantı dizesi, yönetici adı ve parola kullanarak yeni bir bağlantı açar:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>SQuirreL SQL istemci ile bağlanma

SQuirreL SQL uzaktan Hdınsight kümenizle Hive sorguları çalıştırmak için kullanılan bir JDBC istemcidir. Aşağıdaki adımlarda SQuirreL SQL zaten yüklemiş olduğunuz varsayılmaktadır.

1. Dosyaları içeren bir dizin oluşturun. Örneğin, `mkdir hivedriver`.

2. Bir komut satırından Hdınsight küme dosyaları kopyalamak için aşağıdaki komutları kullanın:

    ```bash
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/log4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/slf4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-*-1.2*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpclient-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpcore-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libthrift-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libfb*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-logging-*.jar .
    ```

    Değiştir `USERNAME` küme için SSH kullanıcı hesap adına sahip. Değiştir `CLUSTERNAME` Hdınsight küme adı ile.

3. SQuirreL SQL uygulaması başlatın. Pencerenin soldan seçin **sürücüleri**.

    ![Pencerenin sol sürücüler sekmesinde](./media/apache-hadoop-connect-hive-jdbc-driver/squirreldrivers.png)

4. Başındaki simgeler gelen **sürücüleri** iletişim kutusunda **+** bir sürücüsü oluşturmak için simge.

    ![Sürücüleri simgeler](./media/apache-hadoop-connect-hive-jdbc-driver/driversicons.png)

5. Sürücü Ekle iletişim kutusunda aşağıdaki bilgileri ekleyin:

    * **Ad**: yığını
    * **Örnek URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Ek sınıf yolu**: daha önce indirdiğiniz jar dosyalarını tüm eklemek için Ekle düğmesini kullanın
    * **Class Name**: org.apache.hive.jdbc.HiveDriver

   ![sürücü iletişim ekleyin](./media/apache-hadoop-connect-hive-jdbc-driver/adddriver.png)

   Tıklatın **Tamam** bu ayarları kaydetmek için.

6. SQuirreL SQL pencerenin sol tarafta seçin **diğer adlar**. Ardından **+** bağlantısı diğer adı oluşturmak için simge.

    ![Yeni diğer ad ekleyin](./media/apache-hadoop-connect-hive-jdbc-driver/aliases.png)

7. İçin aşağıdaki değerleri kullanın **diğer ad eklemek** iletişim.

    * **Ad**: Hdınsight'ta Hive

    * **Sürücü**: seçmek için açılır menüyü kullanın **Hive** sürücüsü

    * **URL**: `jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`

        **CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

    * **Kullanıcı adı**: Hdınsight kümeniz için küme oturum açma hesabının adı. Varsayılan değer `admin`.

    * **Parola**: küme oturum açma hesabının parolası.

 ![diğer iletişim ekleyin](./media/apache-hadoop-connect-hive-jdbc-driver/addalias.png)

    > [!IMPORTANT] 
    > Kullanım **Test** bağlantı çalıştığını doğrulamak için düğmesi. Zaman **bağlanın: Hdınsight'ta Hive** seçin iletişim kutusu görüntülenirse, **Bağlan** testi gerçekleştirmek için. Test başarılı olursa, gördüğünüz bir **bağlantı başarılı** iletişim. Bir hata oluşursa, bakınız [sorun giderme](#troubleshooting).

    Bağlantı diğer ad kaydetmek için kullanın **Tamam** alt kısmındaki düğmesi **diğer ad eklemek** iletişim.

8. Gelen **bağlanmak** SQuirreL SQL üstündeki açılan seçin **Hdınsight'ta Hive**. İstendiğinde, seçin **Bağlan**.

    ![Bağlantı iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/connect.png)

9. Bağlantı kurulduktan sonra aşağıdaki sorguyu SQL sorgusu iletişim kutusuna girin ve ardından **çalıştırmak** simgesi. Sonuç alanında sorgu sonuçlarını göstermesi gerekir.

        select * from hivesampletable limit 10;

    ![Sonuçları dahil olmak üzere sql sorgu iletişim kutusu](./media/apache-hadoop-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Bir örnek Java uygulaması Bağlan

Hdınsight'ta Hive sorgusu için Java İstemcisi'ni kullanarak bir örnek kullanılabilir [ https://github.com/Azure-Samples/hdinsight-java-hive-jdbc ](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Derleme ve çalıştırma örnek depo'ndaki yönergeleri izleyin.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>SQL bağlantısı açılmaya çalışılırken beklenmeyen bir hata oluştu

**Belirtiler**: sürüm 3.3 veya daha büyük olan bir Hdınsight kümesine bağlanırken beklenmeyen bir hata oluştu hata alabilirsiniz. Bu hata yığını izlemesi aşağıdaki satırları içeren başlar:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Neden**: Bu hata ile SQuirreL dahil daha eski bir sürüm commons codec.jar dosya kaynaklanır.

**Çözümleme**: Bu hatayı düzeltmek için aşağıdaki adımları kullanın:

1. Hdınsight kümenize commons codec jar dosyasını indirin.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. SQuirreL çıkmak ve SQuirreL, sisteminizde yüklü olduğu dizine gidin. SquirreL dizinindeki altında `lib` dizini, mevcut commons codec.jar biriyle indirilen Hdınsight küme değiştirin.

3. SQuirreL yeniden başlatın. Hata, artık hdınsight'ta Hive bağlanırken olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

JDBC Hive ile çalışmak için nasıl kullanılacağını öğrendiniz, Azure Hdınsight ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [Microsoft Power BI'ı Azure hdınsight'ta Hive görselleştirmek](apache-hadoop-connect-hive-power-bi.md).
* [Etkileşimli sorgu Hive verileri Azure hdınsight'ta Power BI ile görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin kullanın](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile Hdınsight bağlama](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Hadoop için bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure Hdınsight bağlanmak ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırmak](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio kodunu Azure Hdınsight aracını](../hdinsight-for-vscode.md).
* [Hdınsight'a verileri yükleme](../hdinsight-upload-data.md)
* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
