---
title: 'Öğretici: HDInsight’ta IntelliJ kullanarak Spark için bir Scala Maven uygulaması oluşturma | Microsoft Docs'
description: Derleme sistemi olarak Apache Maven ile Scala’da ve Scala için IntelliJ IDEA tarafından sağlanan mevcut bir Maven mimari türü ile yazılmış bir Spark uygulaması oluşturun.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: c72f513c7134c556afa5fa5d0b94c17b1142be54
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-create-a-scala-maven-application-for-spark-in-hdinsight-using-intellij"></a>Öğretici: HDInsight’ta IntelliJ kullanarak Spark için bir Scala Maven uygulaması oluşturma

Bu öğreticide, IntelliJ IDEA ile Maven kullanılarak Scala’da yazılmış bir Spark uygulaması oluşturmayı öğreneceksiniz. Makale, derleme sistemi olarak Apache Maven kullanmakta ve Scala için IntelliJ IDEA tarafından sağlanan mevcut bir Maven mimari türü ile başlamaktadır.  IntelliJ IDEA’da Scala uygulaması oluşturma işlemi aşağıdaki adımları içerir:

* Derleme sistemi olarak Maven kullanma.
* Spark modülü bağımlılıklarını çözümlemek için Proje Nesne Modeli (POM) dosyasını güncelleştirme.
* Uygulamanızı Scala’da yazma.
* HDInsight Spark kümelerine gönderilebilen bir jar dosyası oluşturma.
* Livy kullanarak uygulamayı Spark kümesi üzerinde çalıştırma.

> [!NOTE]
> HDInsight ayrıca uygulamaları oluşturma ve Linux üzerindeki bir HDInsight Spark kümesine gönderme işlemini kolaylaştıran IntelliJ IDEA eklenti aracını sağlar. Daha fazla bilgi için bkz. [Spark uygulamaları oluşturmak ve göndermek üzere IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md).
> 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * IntelliJ kullanarak bir Scala Maven uygulaması geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).


## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Oracle Java Development kit. [Buradan](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) yükleyebilirsiniz.
* Bir Java IDE. Bu makalede IntelliJ IDEA 18.1.1 kullanılmaktadır. [Buradan](https://www.jetbrains.com/idea/download/) yükleyebilirsiniz.

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme
Scala eklentisini yüklemek için aşağıdaki adımları kullanın:

1. IntelliJ IDEA’yı açın.
2. Karşılama ekranında **Yapılandır**’ı ve sonra **Eklentiler**’i seçin.
   
    ![Scala eklentisini etkinleştirme](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
3. Sol alt köşeden **JetBrains eklentisini yükle**’yi seçin. 
4. **JetBrains Eklentilerine Göz At** iletişim kutusunda **Scala** araması yapıp **Yükle**’yi seçin.
   
    ![Scala eklentisini yükleme](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
5. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.

## <a name="create-a-standalone-scala-project"></a>Tek başına Scala projesi oluşturma
1. IntelliJ IDEA’yı açın.
2. Yeni bir proje oluşturmak için **Dosya** menüsünden **Yeni > Proje**’yi seçin.
3. Yeni proje iletişim kutusunda aşağıdaki seçimleri yapın:
   
    ![Maven projesi oluşturma](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * Proje türü olarak **Maven**’i seçin.
   * Bir **Proje SDK’sı** belirtin. **Yeni**’yi seçin ve Java yükleme dizinine gidin (genellikle `C:\Program Files\Java\jdk1.8.0_66` dizinidir).
   * **Mimari türünden oluştur** seçeneğini belirleyin.
   * Mimari türleri listesinden **org.scala-tools.archetypes:scala-archetype-simple** öğesini seçin. Bu mimari türü, Scala programını yazmak için doğru dizin yapısını oluşturur ve gerekli varsayılan bağımlılıkları indirir.
4. **İleri**’yi seçin.
5. **GroupId**, **ArtifactId** ve **Version** için ilgili değerleri sağlayın. Bu öğreticide aşağıdaki değerler kullanılır:

    - GroupId: com.microsoft.spark.example
    - ArtifactId: SparkSimpleApp
6. **İleri**’yi seçin.
7. Ayarları doğrulayıp **İleri**’yi seçin.
8. Proje adını ve konumunu doğrulayıp **Son**’u seçin.
9. Sol bölmede **src > test > scala > com > microsoft > spark > örnek** öğesini seçin, **MySpec**’e sağ tıklayın ve sonra **Sil**’i seçin. Uygulama için bu dosya gerekli değildir.
  
10. Sonraki adımlarda pom.xml dosyasını, Spark Scala uygulamasına ilişkin bağımlılıkları tanımlamak için güncelleştireceksiniz. Bu bağımlılıkların otomatik olarak indirilip çözümlenmesi için Maven’i uygun şekilde yapılandırmanız gerekir.
   
    ![Maven’i otomatik yüklemeler için yapılandırma](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. **Dosya** menüsünden **Ayarlar**’ı seçin.
   2. **Ayarlar** iletişim kutusunda **Derleme, Yürütme, Dağıtım** > **Derleme Araçları** > **Maven** > **İçeri Aktarma** öğesine gidin.
   3. **Maven projelerini otomatik olarak içeri aktar** seçeneğini işaretleyin.
   4. **Uygula**’yı ve sonra **Tamam**’ı seçin.
11. Sol bölmede **src > ana > scala > com.microsoft.spark.example** öğesini seçin ve sonra **Uygulama**’ya çift tıklayarak App.scala dosyasını açın.

12. Var olan örnek kodu aşağıdaki kodla değiştirin ve değişiklikleri kaydedin. Bu kod, HVAC.csv dosyasından (tüm HDInsight Spark kümelerinde mevcuttur) verileri okur, altıncı sütunda yalnızca bir basamağı olan satırları alır ve çıktıyı kümenin varsayılan depolama kapsayıcısı altındaki **/HVACOut** dosyasına yazar.

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
13. Sol bölmede **pom.xml** dosyasına çift tıklayın.
   
   1. `<project>\<properties>` içinde aşağıdaki segmentleri ekleyin:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. `<project>\<dependencies>` içinde aşağıdaki segmentleri ekleyin:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      pom.xml dosyasında yapılan değişiklikleri kaydedin.
10. .jar dosyasını oluşturun. IntelliJ IDEA, proje yapıtı olarak JAR dosyası oluşturmayı sağlar. Aşağıdaki adımları uygulayın.
    
    1. **Dosya** menüsünden **Proje Yapısı**’nı seçin.
    2. **Proje Yapısı** iletişim kutusunda **Yapıtlar**’ı ve sonra artı simgesini seçin. Açılan iletişim kutusundan **JAR** dosyasını ve sonra **Bağımlılıkları olan modüllerden** öğesini seçin.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-1.png)
    3. **Modüllerden JAR Oluştur** iletişim kutusunda **Ana Sınıf**’ın karşısındaki üç noktayı (![üç nokta](./media/apache-spark-create-standalone-application/ellipsis.png)) seçin.
    4. **Ana Sınıf Seç** iletişim kutusunda, varsayılan olarak görünen sınıfı ve ardından **Tamam**’ı seçin.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-2.png)
    5. **Modüllerden JAR Oluştur** iletişim kutusunda **Hedef JAR dosyasına ayıkla** seçeneğinin işaretli olduğundan emin olun ve sonra **Tamam**’ı seçin.  Bu ayar, tüm bağımlılıklarla tek bir JAR oluşturur.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-3.png)
    6. Çıktı düzeni sekmesinde, Maven projesine eklenen tüm jar dosyaları listelenir. Scala uygulamasının doğrudan bağımlılığı olmayan jar dosyalarını seçip silebilirsiniz. Burada oluşturduğunuz uygulama için sonuncu (**SparkSimpleApp derleme çıktısı**) hariç tüm jar dosyalarını kaldırabilirsiniz. Silinecek jar dosyalarını ve sonra **Sil** simgesini seçin.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Projenin derlendiği veya güncelleştirildiği her durumda jar dosyasının oluşturulmasını sağlayan **Proje derlemesine ekle** kutusunun seçili olduğundan emin olun. **Uygula**’yı ve sonra **Tamam**’ı seçin.
    7. Jar dosyasını oluşturmak için **Derleme** menüsünden **Yapıtları Derle**’yi seçin. Çıktı jar dosyası, **\out\artifacts** altında oluşturulur.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>Uygulamayı Spark kümesi üzerinde çalıştırma
Uygulamayı kümede çalıştırmak için aşağıdaki yaklaşımları kullanabilirsiniz:

* **Uygulama jar dosyasını kümeyle ilişkili olan Azure depolama blobuna kopyalayın**. Bunu yapmak için, bir komut satırı yardımcı programı olan [**AzCopy**](../../storage/common/storage-use-azcopy.md)’yi kullanabilirsiniz. Verileri karşıya yüklemek için kullanabileceğiniz çok sayıda başka istemci de mevcuttur. Bunlar hakkında daha fazla bilgi için bkz. [HDInsight'ta Hadoop işleri için veri yükleme](../hdinsight-upload-data.md).
* Spark kümesine **uzaktan bir uygulama işi göndermek için Livy kullanın**. HDInsight üzerinde Spark kümeleri, Spark işlerini uzaktan göndermek için REST uç noktalarını kullanıma sunan Livy’yi içerir. Daha fazla bilgi için bkz. [HDInsight üzerinde Spark kümeleriyle Livy kullanarak Spark işlerini uzaktan gönderme](apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Sonraki adım

Bu makalede, bir Spark scala uygulaması oluşturmayı öğrendiniz. Bu uygulamayı Livy kullanarak bir HDInsight Spark kümesinde çalıştırmayı öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](./apache-spark-livy-rest-interface.md)

