---
title: "Azure Hdınsight Spark kümeleri üzerinde - çalıştırmak için Scala uygulaması oluşturma | Microsoft Docs"
description: "Apache Maven ile Scala yapı sistemi ve var olan bir Maven archetype Intellij Idea tarafından sağlanan Scala için yazılmış bir Spark uygulaması oluşturun."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: bf1b127c4a8e05517d7bd877b463a04bcc0990a0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="create-a-scala-maven-application-to-run-on-apache-spark-cluster-on-hdinsight"></a>Hdınsight'ta Apache Spark kümesinde çalıştırmayı Scala Maven uygulaması oluşturma

Intellij Idea ile Maven kullanarak Scala içinde yazılmış bir Spark uygulaması oluşturmayı öğrenin. Makaleyi Apache Maven derleme sistem olarak kullanır ve Intellij Idea tarafından sağlanan Scala için var olan bir Maven archetype ile başlatır.  Intellij Idea Scala uygulaması oluşturma, aşağıdaki adımları içerir:

* Maven derleme sistem olarak kullanın.
* Spark modülü bağımlılıkları çözümlemek için proje nesne modeli (POM) dosyasını güncelleştirin.
* Uygulamanızı Scala içinde yazın.
* Hdınsight Spark kümeleri gönderilebilir jar dosyasını oluşturun.
* Uygulamayı Livy kullanarak Spark kümesi üzerinde çalıştırın.

> [!NOTE]
> Hdınsight oluşturma ve Linux'ta Hdınsight Spark kümesinde uygulamalar gönderme işlemini kolaylaştırmak için bir Intellij Idea eklentisi aracı da sağlar. Daha fazla bilgi için bkz: [oluşturmak ve Spark uygulamaları göndermek amacıyla Intellij Idea için Hdınsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin.md).
> 
> 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).
* Oracle Java Geliştirme Seti. Şuradan yükleyebilirsiniz [burada](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Bir Java IDE. Bu makalede Intellij Idea 15.0.1 kullanır. Şuradan yükleyebilirsiniz [burada](https://www.jetbrains.com/idea/download/).

## <a name="install-scala-plugin-for-intellij-idea"></a>Intellij Idea için Scala eklentisini yükleme
Intellij Idea yükleme değil Scala eklentisini etkinleştirmek için komut istemine değil, Intellij Idea başlatın ve eklentisini yüklemek için aşağıdaki adımları gidin:

1. Intellij Idea başlatın ve Hoş Geldiniz ekranında tıklatın **yapılandırma** ve ardından **eklentileri**.
   
    ![Scala eklentisini etkinleştirme](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
2. Sonraki ekranda, tıklatın **yükleme JetBrains eklentisi** sol alt köşesinde gelen. İçinde **Gözat JetBrains eklentileri** açar, Scala için arama yapın ve ardından iletişim kutusunu **yükleme**.
   
    ![Scala eklentisini yükleme](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
3. Eklentisi başarıyla yüklendikten sonra tıklatın **yeniden Intellij Idea düğmesi** IDE yeniden başlatmak için.

## <a name="create-a-standalone-scala-project"></a>Bir tek başına Scala projesi oluşturma
1. Intellij Idea başlatın ve yeni bir proje oluşturun. Yeni Proje iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **sonraki**.
   
    ![Bir Maven projesi oluşturun](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * Seçin **Maven** proje türü olarak.
   * Belirtin bir **proje SDK**. Yeni'yi tıklatın ve genellikle Java yükleme dizinine gidin `C:\Program Files\Java\jdk1.8.0_66`.
   * Seçin **archetype Oluştur** seçeneği.
   * Mimarisi türleri listesinden seçin **org.scala tools.archetypes:scala archetype basit**. Bu doğru dizin yapısını oluşturun ve Scala program yazmak için gerekli varsayılan bağımlılıkları indirin.
2. İlgili değerlerini sağlamanız **GroupID**, **Artifactıd**, ve **sürüm**. **İleri**’ye tıklayın.
3. Burada belirttiğiniz Maven giriş dizini ve diğer kullanıcı ayarları, bir sonraki iletişim kutusunda, Varsayılanları kabul edin ve **sonraki**.
4. Son iletişim kutusunda, bir proje adı ve konum belirtin ve ardından **son**.
5. Silme **MySpec.Scala** adresindeki dosya **src\test\scala\com\microsoft\spark\example**. Bu uygulama için gerekli değildir.
6. Gerekirse, varsayılan kaynak ve test dosyalarını yeniden adlandırın. Intellij Idea sol bölmeden gidin **src\main\scala\com.microsoft.spark.example**. Sağ **App.scala**, tıklatın **yeniden düzenlemeniz**, dosyayı yeniden adlandır, tıklayın ve iletişim kutusunda, uygulama için yeni bir ad sağlayın ve ardından **yeniden düzenlemeniz**.
   
    ![Dosyaları yeniden adlandırma](./media/apache-spark-create-standalone-application/rename-scala-files.png)  
7. Sonraki adımlarda Spark Scala uygulamayla ilgili bağımlılıkları tanımlamak için pom.xml güncelleştirir. Bu bağımlılıkları indirilir ve otomatik olarak çözülen Maven uygun şekilde yapılandırmanız gerekir.
   
    ![Maven otomatik yüklemeler için yapılandırma](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. Gelen **dosya** menüsünde tıklatın **ayarları**.
   2. İçinde **ayarları** iletişim kutusunda, gitmek **derleme, yürütme, dağıtımı** > **derleme Araçları** > **Maven**  >  **Alma**.
   3. Seçeneğini **Import Maven projeleri otomatik olarak**.
   4. Tıklatın **Uygula**ve ardından **Tamam**.
8. Uygulama kodunuz içerecek şekilde Scala kaynak dosyasını güncelleştirin. Açın ve var olan örnek kodu aşağıdaki kodla değiştirin ve değişiklikleri kaydedin. Bu kod (tüm Hdınsight Spark kümeleri üzerinde kullanılabilir) HVAC.csv verileri okur, yalnızca bir rakam altıncı sütununda satırları alır ve çıktıyı Yazar **/HVACOut** varsayılan depolama kapsayıcısını altında Küme.
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. Pom.xml güncelleştirin.
   
   1. İçinde `<project>\<properties>` aşağıdakileri ekleyin:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. İçinde `<project>\<dependencies>` aşağıdakileri ekleyin:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Pom.xml için değişiklikleri kaydedin.
10. .Jar dosyasını oluşturun. Intellij Idea JAR oluşturulmasını projesinin bir yapı olarak sağlar. Aşağıdaki adımları gerçekleştirin.
    
    1. Gelen **dosya** menüsünde tıklatın **proje yapısını**.
    2. İçinde **proje yapısını** iletişim kutusu, tıklatın **yapıları** artı simgesine tıklayın. Açılan iletişim kutusundan tıklatın **JAR**ve ardından **bağımlılıkları olan modüller gelen**.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-1.png)
    3. İçinde **oluşturma JAR modüllerden** iletişim kutusunda, üç nokta işaretine (![üç nokta](./media/apache-spark-create-standalone-application/ellipsis.png) ) karşı **ana sınıfı**.
    4. İçinde **ana sınıftan** iletişim kutusunda, varsayılan olarak görünür sınıfı seçin ve ardından **Tamam**.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-2.png)
    5. İçinde **oluşturma JAR modüllerden** iletişim kutusunda, olduğundan emin olun seçeneği **hedef JAR ayıklamak** seçilir ve ardından **Tamam**. Bu, tüm bağımlılıkları olan tek bir JAR oluşturur.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-3.png)
    6. Çıkış düzeni sekmesi Maven projenin bir parçası olarak dahil tüm Kavanoz listeler. Seçin ve üretileceği olanları Sil Scala uygulama doğrudan hiçbir bağımlılık içeriyor. Biz burada oluşturma uygulama için tüm sonuncu kaldırabilirsiniz (**SparkSimpleApp derleme çıktı**). Silin ve ardından Kavanoz seçin **silmek** simgesi.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Emin olun **olun yapı** kutusu seçiliyse, proje oluşturulmuş veya güncelleştirilmiş her zaman jar oluşturulur sağlar. Tıklatın **uygulamak** ve ardından **Tamam**.
    7. Menü çubuğundaki **yapı**ve ardından **olun proje**. Tıklatarak **derleme yapıları** jar oluşturmak için. Çıktı jar altında oluşturulan **\out\artifacts**.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>Spark kümesinde uygulamayı çalıştırın
Küme üzerinde uygulamayı çalıştırmak için aşağıdakileri yapmanız gerekir:

* **Uygulama jar Azure storage blobunuza kopyalamak** kümesi ile ilişkili. Kullanabileceğiniz [ **AzCopy**](../../storage/common/storage-use-azcopy.md), bunu yapmak için bir komut satırı yardımcı programı. Verileri yüklemek için kullanabileceğiniz diğer istemciler de çok vardır. Onları hakkında daha fazla bilgiyi [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md).
* **Bir uygulama işi uzaktan göndermek için Livy kullanın** Spark küme. Hdınsight'ta Spark kümeleri Spark işleri uzaktan göndermek için REST uç noktalarını kullanıma sunar Livy içerir. Daha fazla bilgi için bkz: [Livy kullanarak Spark ile uzaktan kümeleri Hdınsight'ta Spark gönderme işleri](apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Sonraki adım

Bu makalede bir Spark scala uygulamasının nasıl oluşturulacağını öğrendiniz. Livy kullanarak bir Hdınsight Spark kümesinde bu uygulamayı çalıştırmak öğrenmek için sonraki makalede ilerleyin.

> [!div class="nextstepaction"]
>[Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

