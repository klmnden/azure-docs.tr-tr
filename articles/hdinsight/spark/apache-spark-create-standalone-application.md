---
title: 'Öğretici: Intellij kullanarak Azure HDInsight içindeki Spark Scala Maven uygulama oluşturma'
description: Derleme sistemi olarak Apache Maven ile Scala’da ve Scala için IntelliJ IDEA tarafından sağlanan mevcut bir Maven mimari türü ile yazılmış bir Spark uygulaması oluşturun.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 01/30/2019
ms.openlocfilehash: 2d431659e46465bf16f6e597f3a49f7008432bb5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124484"
---
# <a name="tutorial-create-a-scala-maven-application-for-apache-spark-in-hdinsight-using-intellij"></a>Öğretici: Intellij kullanarak HDInsight, Apache Spark Scala Maven uygulama oluşturma

Bu öğreticide, şunların nasıl oluşturulacağı bir [Apache Spark](https://spark.apache.org/) yazılmış uygulama [Scala](https://www.scala-lang.org/) kullanarak [Apache Maven](https://maven.apache.org/) Intellij Idea ile. Makale, derleme sistemi olarak Apache Maven kullanmakta ve Scala için IntelliJ IDEA tarafından sağlanan mevcut bir Maven mimari türü ile başlamaktadır.  IntelliJ IDEA’da Scala uygulaması oluşturma işlemi aşağıdaki adımları içerir:

* Derleme sistemi olarak Maven kullanma.
* Spark modülü bağımlılıklarını çözümlemek için Proje Nesne Modeli (POM) dosyasını güncelleştirme.
* Uygulamanızı Scala’da yazma.
* HDInsight Spark kümelerine gönderilebilen bir jar dosyası oluşturma.
* Livy kullanarak uygulamayı Spark kümesi üzerinde çalıştırma.

> [!NOTE]  
> HDInsight ayrıca uygulamaları oluşturma ve Linux üzerindeki bir HDInsight Spark kümesine gönderme işlemini kolaylaştıran IntelliJ IDEA eklenti aracını sağlar. Daha fazla bilgi için [oluşturmak ve Apache Spark uygulamaları göndermek amacıyla Intellij Idea için kullanım HDInsight araçları eklentisi](apache-spark-intellij-tool-plugin.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * IntelliJ kullanarak bir Scala Maven uygulaması geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* [Oracle Java Development kit](https://www.azul.com/downloads/azure-only/zulu/).  Bu öğreticide, Java Sürüm 8.0.202 kullanılır.
* Bir Java IDE. Bu makalede [Intellij Idea topluluk ver.  2018.3.4](https://www.jetbrains.com/idea/download/).
* Intellij için Azure Araç Seti.  Bkz: [Intellij için Azure araç setini yükleme](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable).

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme
Scala eklentisini yüklemek için aşağıdaki adımları gerçekleştirin:

1. IntelliJ IDEA’yı açın.

2. Hoş Geldiniz ekranında gidin **yapılandırma** > **eklentileri** açmak için **eklentileri** penceresi.
   
    ![Scala eklentisini etkinleştirme](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)

3. Seçin **yükleme** yeni pencerede öne çıkan Scala eklentisi için.  
 
    ![Scala eklentisini yükleme](./media/apache-spark-create-standalone-application/install-scala-plugin.png)

4. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.


## <a name="use-intellij-to-create-application"></a>Uygulama oluşturmak için IntelliJ kullanma

1. Intellij Idea başlatın ve **yeni proje oluştur** açmak için **yeni proje** penceresi.

2. Seçin **Azure Spark/HDInsight** sol bölmeden.

3. Seçin **Spark proje (Scala)** ana penceresinde.

4. Gelen **derleme aracı** aşağı açılan listesinde, aşağıdakilerden birini seçin:
      * **Maven** Scala Proje Oluşturma Sihirbazı'nı desteği.
      * **SBT** bağımlılıkları yönetmek ve için Scala projesi oluşturma.

   ![Yeni Proje iletişim kutusu](./media/apache-spark-create-standalone-application/create-hdi-scala-app.png)

5. **İleri**’yi seçin.

6. İçinde **yeni proje** penceresinde aşağıdaki bilgileri sağlayın:  

  	|  Özellik   | Açıklama   |  
  	| ----- | ----- |  
  	|Proje adı| Bir ad girin.|  
  	|Proje&nbsp;konumu| Projenizi kaydetmek istediğiniz konumu girin.|
  	|Proje SDK'sı| Bu, ilk fikir kullanımınız boş olacaktır.  Seçin **yeni...**  ve, JDK için gidin.|
  	|Spark sürümü|Oluşturma Sihirbazı'nı, Spark SDK'sı ve Scala SDK'sı için doğru sürüm tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnekte **Spark 2.3.0 (Scala 2.11.8)**.|

    ![Spark SDK’sını seçme](./media/apache-spark-create-standalone-application/hdi-new-project.png)

7. **Son**’u seçin.

## <a name="create-a-standalone-scala-project"></a>Tek başına Scala projesi oluşturma

1. Intellij Idea başlatın ve **yeni proje oluştur** açmak için **yeni proje** penceresi.

2. Seçin **Maven** sol bölmeden.

3. Bir **Proje SDK’sı** belirtin. Boş bırakılırsa seçin **yeni...**  ve Java yükleme dizinine gidin.

4. Seçin **archetype Oluştur** onay kutusu.  

5. Mimari türleri listesinden **org.scala-tools.archetypes:scala-archetype-simple** öğesini seçin. Bu archetype doğru dizin yapısı oluşturur ve Scala program yazmak için gerekli varsayılan bağımlılıkları indirir.

    ![Maven projesi oluşturma](./media/apache-spark-create-standalone-application/create-maven-project.png)

6. **İleri**’yi seçin.

7. **GroupId**, **ArtifactId** ve **Version** için ilgili değerleri sağlayın. Bu öğreticide aşağıdaki değerler kullanılır:

    - **GroupID:** com.microsoft.spark.example
    - **ArtifactId:** SparkSimpleApp

8. **İleri**’yi seçin.

9. Ayarları doğrulayıp **İleri**’yi seçin.

10. Proje adını ve konumunu doğrulayıp **Son**’u seçin.  Projeyi içeri aktarmak için birkaç dakika sürer.

11. Proje aldı sonra sol bölmeden gidin **SparkSimpleApp** > **src** > **test**  >  **scala** > **com** > **microsoft** > **spark**  >  **örnek**.  Sağ **MySpec**ve ardından **Sil...** . Uygulama için bu dosya gerekli değildir.  Seçin **Tamam** iletişim kutusunda.
  
12. Sonraki adımlarda, güncelleştirme **pom.xml** Scala Spark uygulaması bağımlılıklarını tanımlamak için. Bu bağımlılıkların otomatik olarak indirilip çözümlenmesi için Maven’i uygun şekilde yapılandırmanız gerekir.

13. Gelen **dosya** menüsünde **ayarları** açmak için **ayarları** penceresi.

14. Gelen **ayarları** penceresinde gidin **derleme, yürütme, dağıtım** > **derleme Araçları** > **Maven**  >  **Alma**.

15. Seçin **Import Maven projelerinde otomatik olarak** onay kutusu.

16. **Uygula**’yı ve sonra **Tamam**’ı seçin.  Ardından Proje penceresine döndürülür.
   
    ![Maven’i otomatik yüklemeler için yapılandırma](./media/apache-spark-create-standalone-application/configure-maven.png)
   

17. Sol bölmeden gidin **src** > **ana** > **scala** > **com.microsoft.spark.example**ve çift tıklatarak **uygulama** App.scala açın.

18. Var olan örnek kodu aşağıdaki kodla değiştirin ve değişiklikleri kaydedin. Bu kod, HVAC.csv dosyasından (tüm HDInsight Spark kümelerinde mevcuttur) verileri okur, altıncı sütunda yalnızca bir basamağı olan satırları alır ve çıktıyı kümenin varsayılan depolama kapsayıcısı altındaki **/HVACOut** dosyasına yazar.

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
19. Sol bölmede **pom.xml** dosyasına çift tıklayın.  
   
20. `<project>\<properties>` içinde aşağıdaki segmentleri ekleyin:
      
          <scala.version>2.11.8</scala.version>
          <scala.compat.version>2.11.8</scala.compat.version>
          <scala.binary.version>2.11</scala.binary.version>

21. `<project>\<dependencies>` içinde aşağıdaki segmentleri ekleyin:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>2.3.0</version>
           </dependency>
      
    pom.xml dosyasında yapılan değişiklikleri kaydedin.

22. .jar dosyasını oluşturun. IntelliJ IDEA, proje yapıtı olarak JAR dosyası oluşturmayı sağlar. Aşağıdaki adımları uygulayın.
    
    1. Gelen **dosya** menüsünde **Proje yapısı...** .

    2. Gelen **Proje yapısı** penceresinde gidin **Yapıtları** > **artı simgesini +** > **JAR**  >  **Bağımlılıkları olan modüllerden...** .
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-1.png)

    3. İçinde **oluşturma JAR modüllerden** penceresinde klasör simgesini **ana sınıfı** metin kutusu.

    4. İçinde **ana sınıfı seçin** penceresi, varsayılan olarak görüntülenen sınıfı seçin ve ardından **Tamam**.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-2.png)

    5. İçinde **oluşturma JAR modüllerden** penceresinde olun **hedef jar dosyasını ayıklayın** seçeneği seçili ve ardından **Tamam**.  Bu ayar, tüm bağımlılıklarla tek bir JAR oluşturur.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/create-jar-3.png)

    6. **Çıkış Düzen** Maven projenin bir parçası olarak dahil edilen tüm jar dosyaları dışındaki sekmesinde listelenir. Scala uygulamasının doğrudan bağımlılığı olmayan jar dosyalarını seçip silebilirsiniz. Burada oluşturduğunuz uygulama için sonuncu (**SparkSimpleApp derleme çıktısı**) hariç tüm jar dosyalarını kaldırabilirsiniz. Jar dosyaları dışındaki silin ve ardından eksi sembolünün seçin **-**.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Emin olun **eklemek Proje yapı** onay kutusu seçilidir ve bu proje oluşturulan veya güncelleştirilen her zaman jar oluşturulmasını sağlar. seçin **Uygula** ardından **Tamam**.

    7. Jar oluşturmak için gidin **derleme** > **derleme yapıtlarını** > **yapı**. Projenin, yaklaşık 30 saniye içinde derlenecek.  Çıktı jar dosyası, **\out\artifacts** altında oluşturulur.
       
        ![JAR oluşturma](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-apache-spark-cluster"></a>Apache Spark kümesinde uygulamayı çalıştırın
Uygulamayı kümede çalıştırmak için aşağıdaki yaklaşımları kullanabilirsiniz:

* **Uygulama jar dosyasını kümeyle ilişkili olan Azure depolama blobuna kopyalayın**. Bunu yapmak için, bir komut satırı yardımcı programı olan [**AzCopy**](../../storage/common/storage-use-azcopy.md)’yi kullanabilirsiniz. Verileri karşıya yüklemek için kullanabileceğiniz çok sayıda başka istemci de mevcuttur. Onları hakkında daha fazla bulabilirsiniz [HDInsight Apache Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md).

* **Bir uygulama işi uzaktan göndermek için Apache Livy kullanın** Spark kümesine. HDInsight üzerinde Spark kümeleri, Spark işlerini uzaktan göndermek için REST uç noktalarını kullanıma sunan Livy’yi içerir. Daha fazla bilgi için [kümeleri HDInsight üzerinde Apache Livy kullanarak Spark ile uzaktan gönderme Apache Spark işleri](apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Sonraki adım

Bu makalede, bir Apache Spark scala uygulamasının nasıl oluşturulacağını öğrendiniz. Bu uygulamayı Livy kullanarak bir HDInsight Spark kümesinde çalıştırmayı öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](./apache-spark-livy-rest-interface.md)
