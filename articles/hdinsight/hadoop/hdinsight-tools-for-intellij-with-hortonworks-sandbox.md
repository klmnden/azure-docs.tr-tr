---
title: Hortonworks korumalı alanı ile Intellij için Azure araç setini kullanma
description: Hortonworks korumalı alanı ile Intellij için Azure araç setindeki HDInsight araçlarını kullanmayı öğrenin.
keywords: hadoop araçları, hive sorgusu, ıntellij, hortonworks korumalı alanı, ıntellij için azure Araç Seti
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 6ae271fc464e2a5735ef95a428b3070066058ddc
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129238"
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Hortonworks korumalı alanı ile Intellij için HDInsight araçları kullanma

Apache Scala uygulamaları geliştirin ve uygulamalar üzerinde test etmek için Intellij için HDInsight Araçları'nı kullanmayı öğrenin [Hortonworks korumalı alanı](https://hortonworks.com/products/sandbox/) bilgisayarınızda çalışan. 

[Intellij Idea](https://www.jetbrains.com/idea/) bilgisayar yazılımı geliştirmek için Java tümleşik geliştirme ortamı (IDE) olan. Geliştirme ve test uygulamalarınızı Hortonworks korumalı alanı sonra uygulamaları taşıyabilirsiniz [Azure HDInsight](apache-hadoop-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

- Hortonworks Data Platform (HDP) 2.4 yerel bilgisayarınızda çalışan Hortonworks korumalı. HDP ayarlamak için bkz [Apache Hadoop ekosistemindeki bir sanal makinede bir Hadoop korumalı alanı ile çalışmaya başlama](apache-hadoop-emulator-get-started.md). 
    > [!NOTE]
    > Intellij için HDInsight araçları yalnızca HDP 2.4 ile test edilmiştir. HDP 2.4 almak için genişletin **Hortonworks Sandbox arşiv** üzerinde [Hortonworks korumalı alanı site yüklemeleri](https://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) 1.8 veya sonraki bir sürümü](https://aka.ms/azure-jdks). Intellij için Azure araç seti, JDK gerektirir.

- [Intellij Idea community edition](https://www.jetbrains.com/idea/download) ile [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) eklenti ve [Intellij için Azure Araç Seti](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij) eklenti. Intellij için HDInsight araçları, Intellij için Azure Araç Seti parçası olarak. 

Eklentileri yüklemek için:

  1. IntelliJ IDEA’yı açın.
  2. Üzerinde **Hoş Geldiniz** sayfasında **yapılandırma**ve ardından **eklentileri**.
  3. Sol alt köşedeki seçin **yükleme JetBrains eklentisi**.
  4. Aramak için arama işlevini **Scala**ve ardından **yükleme**.
  5. Yüklemeyi tamamlamak için seçin **yeniden Intellij Idea**.
  6. 4 ve 5 yüklemek için **Intellij için Azure Araç Seti**. Daha fazla bilgi için [Intellij için Azure Araç Seti'ni yükleme](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="create-an-apache-spark-scala-application"></a>Apache Spark Scala uygulama oluşturma

Bu bölümde, Intellij Idea'yı kullanarak bir örnek Scala projesi oluşturun. Proje göndermeden önce sonraki bölümde, Intellij Idea Hortonworks Sandbox (öykünücü) bağlayın.

1. Intellij Idea bilgisayarınızda açın. İçinde **yeni proje** iletişim kutusunda, aşağıdaki adımları tamamlayın:

   1. **HDInsight** > **HDInsight’ta Spark (Scala)** seçeneğini belirleyin.
   2. İçinde **derleme aracı** tabanlı senaryonuz üzerinde listesinde, aşağıdakilerden birini seçin:

      * **Maven**: Scala Proje Oluşturma Sihirbazı'nı desteği.
      * **SBT**: Bağımlılık yönetimi ve için Scala projesi oluşturma.

   ![Yeni Proje iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. **İleri**’yi seçin.
3. Sonraki **yeni proje** iletişim kutusunda, aşağıdaki adımları tamamlayın:

   1. İçinde **proje adı** kutusunda, bir proje adı girin.
   2. İçinde **proje konumu** kutusunda, bir proje konumu girin.
   3. Yanındaki **proje SDK'sı** aşağı açılan listesinden **yeni**seçin **JDK**, ve ardından Java JDK 1.7 veya sonraki bir sürümü için klasörü belirtin. Seçin **Java 1.8** Spark 2.x kümesi için. Seçin **Java 1.7** Spark 1.x kümesi için. Varsayılan konum C:\Program Files\Java\jdk1.8.x_xxx ' dir.
   4. İçinde **Spark sürümü** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı SDK Scala ve Spark SDK'sı için doğru sürüm tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, Spark 1.6.2 (Scala 2.10.5) kullanır. İşaretlenmiş depo kullandığınızdan emin olun **Scala 2.10.x**. Scala işaretlenmiş depo kullanmayın 2.11.x.
    
      ![Create IntelliJ Scala project properties](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)


4. **Son**’u seçin.
5. Varsa **proje** görünüm zaten açık değilse, basın **Alt + 1** açın.
6. İçinde **Proje Gezgini**projeyi genişletin ve ardından **src**.
7. Sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.
8. İçinde **adı** kutusunda, bir ad girin. İçinde **tür** kutusunda **nesne**. Sonra **Tamam**’ı seçin.

    ![Yeni Scala Sınıf Oluştur iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. .Scala dosyasında, aşağıdaki kodu yapıştırın:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache.
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. Üzerinde **derleme** menüsünde **derleme proje**. Derleme başarıyla tamamladığından emin olun.


## <a name="link-to-the-hortonworks-sandbox"></a>Hortonworks korumalı alanı bağlantısı

Hortonworks Sandbox (öykünücü) bağlayabilirsiniz önce var olan bir Intellij uygulamanızın olması gerekir.

Öykünücü olarak bağlamak için:

1. Intellij içinde projeyi açın.
2. Üzerinde **görünümü** menüsünde **araçları Windows**ve ardından **Azure Gezgini**.
3. Genişletin **Azure**, sağ **HDInsight**ve ardından **bir öykünücü bağlantı**.
4. İçinde **yeni bir öykünücü, bağlantı** iletişim kutusunda, Hortonworks korumalı alanı kök hesap için ayarladığınız parolayı girin. Ardından, aşağıdaki ekran görüntüsünde kullanılan benzer değerler girin. Sonra **Tamam**’ı seçin. 

   ![Bağlantıya yeni bir öykünücü iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Öykünücü yapılandırmak için seçin **Evet**.

Öykünücü başarıyla bağlandığında öykünücüsü (Hortonworks Sandbox) HDInsight düğümünde listelenir.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Hortonworks korumalı Scala Spark uygulaması gönderin

Intellij Idea için öykünücü bağladığınız sonra projenizi gönderebilirsiniz.

Bir projeye bir öykünücü göndermek için:

1. İçinde **Proje Gezgini**projeye sağ tıklayın ve ardından **HDInsight için Spark gönderme uygulaması**.
2. Aşağıdaki adımları tamamlayın:

    1. İçinde **Spark kümesi (yalnızca Linux)** aşağı açılan listesinde, yerel Hortonworks korumalı alanı seçin.
    2. İçinde **ana sınıf adı** kutusunda seçin veya ana sınıf adı girin. Bu öğreticide, addır **GroupByTest**.

3. Seçin **gönderme**. İş gönderme günlüklerini Spark gönderimi araç penceresinde gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için Intellij için Azure araç seti, HDInsight araçları kullanma](../spark/apache-spark-intellij-tool-plugin.md).

- Intellij için HDInsight araçları hakkında video için bkz. [geliştirme Apache Spark için Intellij için HDInsight araçları tanıtmak](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Bilgi edinmek için nasıl [SSH üzerinden Intellij için Azure araç seti ile bir HDInsight kümesi üzerinde Apache Spark uygulamaları uzaktan hata ayıklama](../spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Bilgi edinmek için nasıl [Apache Spark uygulamaları bir HDInsight Spark Linux kümesi üzerinde uzaktan hata ayıklama için Intellij için Azure araç seti, HDInsight araçları kullanma](../spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Bilgi edinmek için nasıl [Apache Spark uygulamaları oluşturmak Eclipse için Azure araç seti, HDInsight Araçları](../spark/apache-spark-eclipse-tool-plugin.md).

- Eclipse için HDInsight araçları hakkında video için bkz. [Spark uygulamaları oluşturmak Eclipse için HDInsight Araçları](https://mix.office.com/watch/1rau2mopb6fha).
