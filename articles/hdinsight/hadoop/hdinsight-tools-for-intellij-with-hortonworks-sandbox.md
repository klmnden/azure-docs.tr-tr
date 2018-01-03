---
title: "Intellij Hortonworks Sandbox ile için Azure araç kullanma | Microsoft Docs"
description: "Korumalı alan Hortonworks ile Intellij için Hdınsight araçları Azure araç setindeki kullanmayı öğrenin."
keywords: "hadoop araçları hive sorgusu, ıntellij, hortonworks korumalı alan, ıntellij için azure Araç Seti"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 6bb29dc4e231bc859be620e56a606fbbfade102b
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Korumalı alan Hortonworks ile Intellij için Hdınsight araçları kullanma

Apache Scala uygulamaları geliştirmek ve şirket uygulamalarını test Intellij için Hdınsight araçlarını kullanmayı öğrenin [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) , bilgisayarınızda çalışan. 

[Intellij Idea](https://www.jetbrains.com/idea/) bilgisayar yazılımı geliştirmek için Java tümleşik geliştirme ortamı (IDE) değil. Geliştirme ve Hortonworks Sandbox uygulamalarınızı test etme sonra uygulamaları taşıyabilirsiniz [Azure Hdınsight](apache-hadoop-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

- Hortonworks veri Platformu (HDP) 2.4 Hortonworks yerel bilgisayarınızda çalışan Sandbox. HDP ayarlamak için bkz: [Hadoop ekosistemindeki bir Hadoop korumalı bir sanal makinede ile çalışmaya başlama](apache-hadoop-emulator-get-started.md). 
    > [!NOTE]
    > Intellij için Hdınsight araçları yalnızca HDP 2.4 ile test edilmiştir. HDP 2.4 almak için genişletme **Hortonworks Sandbox arşiv** üzerinde [Hortonworks korumalı alan site yüklemeleri](http://hortonworks.com/downloads/#sandbox).

- [Java Geliştirme Seti (JDK) 1.8 veya sonraki bir sürümü](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Intellij için Azure Araç Seti JDK gerektirir.

- [Intellij Idea community sürümü](https://www.jetbrains.com/idea/download) ile [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) eklenti ve [Intellij için Azure Araç Seti](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij) eklentisi. Intellij için Hdınsight araçları kullanılabilir Intellij için Azure araç seti bir parçası olarak. 

Eklentiler yüklemek için:

  1. Intellij Idea açın.
  2. Üzerinde **Hoş Geldiniz** sayfasında, **yapılandırma**ve ardından **eklentileri**.
  3. Sol alt köşedeki seçin **yükleme JetBrains eklentisi**.
  4. Aramak için arama işlevini kullanın **Scala**ve ardından **yükleme**.
  5. Yüklemeyi tamamlamak için seçin **yeniden Intellij Idea**.
  6. 4 ve 5 yüklemek için **Intellij için Azure Araç Seti**. Daha fazla bilgi için bkz: [Intellij için Azure Araç Seti yüklemek](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="create-a-spark-scala-application"></a>Spark Scala uygulaması oluşturma

Bu bölümde, Intellij Idea kullanarak bir örnek Scala projesi oluşturun. Proje göndermeden önce sonraki bölümde, Intellij Idea korumalı alan (öykünücüsü) Hortonworks bağlarsınız.

1. Intellij Idea bilgisayarınızda açın. İçinde **yeni proje** iletişim kutusunda, aşağıdaki adımları tamamlayın:

   1. Seçin **Hdınsight** > **(Scala) hdınsight'ta Spark**.
   2. İçinde **oluşturma aracını** dayalı senaryonuz üzerinde listesinde, aşağıdakilerden birini seçin:

    * **Maven**: Scala için Proje Oluşturma Sihirbazı'nı desteği.
    * **SBT**: bağımlılıkları yönetmek ve Scala projeyi oluşturmak için.

   ![Yeni Proje iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Seçin **sonraki**.
3. Sonraki **yeni proje** iletişim kutusunda, aşağıdaki adımları tamamlayın:

    1. İçinde **proje adı** kutusunda, bir proje adı girin.
    2. İçinde **proje konumu** kutusunda, bir proje konumu girin.
    3. Yanına **proje SDK** aşağı açılan listesinden, **yeni**seçin **JDK**, ve ardından Java JDK 1,7 veya sonraki bir sürümü klasörü belirtin. Seçin **Java 1.8** Spark 2.x kümesi için. Seçin **Java 1.7** Spark 1.x kümesi için. Varsayılan konumu C:\Program Files\Java\jdk1.8.x_xxx şeklindedir.
    4. İçinde **Spark sürüm** aşağı açılan listesinde, Scala Proje Oluşturma Sihirbazı'nı Spark SDK ve Scala SDK'sı için doğru sürümü tümleştirir. Spark kümesi sürüm 2.0 eskiyse, seçin **1.x Spark**. Aksi takdirde seçin **Spark2.x**. Bu örnek, Spark 1.6.2 (Scala 2.10.5) kullanır. İşaretlenen deposu kullandığınızdan emin olun **Scala 2.10.x**. Scala işaretlenmiş deposu kullanmayın 2.11.x.
    
    ![Proje Özellikleri Intellij Scala oluşturma](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)


4. **Son**’u seçin.
5. Varsa **proje** görünüm zaten açık, basın **Alt + 1** açın.
6. İçinde **Proje Gezgini**projenizi genişletin ve ardından **src**.
7. Sağ **src**, işaret **yeni**ve ardından **Scala sınıfı**.
8. İçinde **adı** kutusunda, bir ad girin. İçinde **türü** kutusunda **nesne**. Ardından, seçin **Tamam**.

    ![Yeni Scala Sınıf Oluştur iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. .Scala dosyasında aşağıdaki kodu yapıştırın:

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

10. Üzerinde **yapı** menüsünde, select **derleme projesi**. Derleme başarıyla tamamladığından emin olun.


## <a name="link-to-the-hortonworks-sandbox"></a>Bağlantı Hortonworks korumalı

Bir Hortonworks korumalı (öykünücüsü) bağlayabilirsiniz, var olan bir Intellij uygulamasını sahip olmanız gerekir.

Bir öykünücü bağlantı oluşturmak için:

1. Proje içinde Intellij açın.
2. Üzerinde **Görünüm** menüsünde, select **Windows Araçları**ve ardından **Azure Gezgini**.
3. Genişletme **Azure**, sağ **Hdınsight**ve ardından **bir öykünücü bağlantı**.
4. İçinde **bağlantı A yeni öykünücü** iletişim kutusunda, Hortonworks Sandbox kök hesabı için ayarladığınız parolayı girin. Ardından, aşağıdaki ekran görüntüsünde kullanılan benzer değerleri girin. Ardından, seçin **Tamam**. 

   ![Bağlantı bir yeni öykünücü iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Öykünücü yapılandırmak için seçin **Evet**.

Öykünücü başarıyla bağlandığında (Hortonworks Sandbox) öykünücüsü Hdınsight düğümünde listelenir.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Spark Scala uygulama Hortonworks korumalı gönderme

Intellij Idea öykünücüsünü bağladığınız sonra projenizi gönderebilirsiniz.

Bir öykünücü projeye göndermek için:

1. İçinde **Proje Gezgini**projeye sağ tıklayın ve ardından **Hdınsight Spark gönderme uygulamaya**.
2. Aşağıdaki adımları tamamlayın:

    1. İçinde **Spark kümesi (yalnızca Linux)** aşağı açılan listesinde, yerel Hortonworks korumalı alan seçin.
    2. İçinde **ana sınıf adı** kutusunda seçin veya ana sınıf adı girin. Bu öğretici için addır **GroupByTest**.

3. Seçin **gönderme**. İş gönderme günlüklerini Spark gönderimi araç penceresi gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Hdınsight araçları Intellij için Azure Araç Seti Spark Hdınsight Spark Linux kümesi için uygulamalar oluşturmak için kullanın](../spark/apache-spark-intellij-tool-plugin.md).

- Intellij için Hdınsight araçları hakkında bir video için bkz: [Spark geliştirme Intellij için Hdınsight araçları tanıtmak](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Bilgi edinmek için nasıl [SSH aracılığıyla Intellij için Hdınsight kümesinde Azure araç seti ile Spark uygulamalarında uzaktan hata ayıklama](../spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Bilgi edinmek için nasıl [Hdınsight araçları Intellij için Azure Araç Seti uzaktan bir Hdınsight Spark Linux kümesi üzerinde Spark uygulamalarında hata ayıklamasını kullanın](../spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Bilgi nasıl [kullanım Hdınsight Spark uygulamaları oluşturmak Eclipse için Azure Araç Seti araçlarında](../spark/apache-spark-eclipse-tool-plugin.md).

- Eclipse için Hdınsight araçları hakkında bir video için bkz: [Spark uygulamaları oluşturmak Eclipse için Hdınsight araçları kullanın](https://mix.office.com/watch/1rau2mopb6fha).
