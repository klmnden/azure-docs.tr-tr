---
title: "Spark yapılandırılmış Azure Hdınsight'ta akış | Microsoft Docs"
description: "Hdınsight Spark kümeleri üzerinde Spark yapılandırılmış akış uygulamaları kullanma"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: aa56c1e2f1f506be51f449a1cf10b4f0bc57a152
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="overview-of-spark-structured-streaming"></a>Yapılandırılmış Spark akış genel bakış

Spark yapılandırılmış akışı, veri akışlarını işlemeye ölçeklenebilir, yüksek verimlilik, hataya dayanıklı uygulamalarını uygulamak etkinleştirir. Yapılandırılmış akış Spark SQL altyapısı inşa edilmiş ve Spark SQL veri çerçevelerini alanından yapıları üzerine artırır ve akış aynı şekilde, sorgular yazmak için veri kümelerini toplu sorgular yazarsınız.  

Yapılandırılmış akış uygulamaları Hdınsight Spark kümelerinde çalıştırmanız ve veriler Kafka, bir TCP yuva (için hata ayıklama amacıyla), Azure Storage veya Azure Data Lake Store akış bağlanın. Harici depolama Hizmetleri'ni kullanır, sonraki iki seçenek, depolama alanına eklenen yeni dosyalar için izleme ve akışla alındı sanki içeriklerini işlemek etkinleştirin. 

Yapılandırılmış akış sırasında giriş verilere seçimi, yansıtma, toplama, Pencereleme ve akış DataFrame başvuru DataFrames ile birleştirme gibi işlemleri uygulamak uzun süre çalışan sorgu oluşturur. Ardından, sonuçlar dosya depolama (Azure Storage Bloblarında veya Data Lake Store) veya herhangi bir veri deposu için özel kod (örneğin, SQL Database veya Power BI) kullanarak çıktı. Hdınsight'ta hata ayıklama için oluşturulan verileri görebilmek için yapılandırılmış akış çıkış yerel olarak hata ayıklama için konsolu ve bir bellek içi tablo de sağlar. 

![Akış Hdınsight ile yapılandırılmış Spark akış işleme ](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]
> Spark yapılandırılmış akış Spark akış (DStreams) değiştirmektir. DStreams yalnızca bakım modunda olabilirken ileride, yapılandırılmış akış geliştirme ve Bakım, alır. Yapılandırılmış akış şu anda olarak özellik DStreams kaynakları için tamamlama değil ve kutudan çıktığında desteklediğini iç havuzlar, böylece gereksinimlerinize uygun Spark akış işleme seçeneği değerlendirin. 

## <a name="streams-as-tables"></a>Tabloları olarak akışlar

Spark yapılandırılmış akış veri akışı derinlemesine sınırsız olan bir tablo olarak temsil eder, diğer bir deyişle, tablonun yeni veri ulaştığında büyümeye devam eder. Bu *giriş tablosu* sürekli uzun süre çalışan sorgu ve gönderilen sonuçları tarafından işlenen bir *çıktı tablosu*:

![Kavram akış yapılandırılmış](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

Yapılandırılmış akış, veri sistemi ulaştığında ve hemen bir giriş tablosu alınan. Bu giriş tablosu karşı işlemleri (DataFrame ve veri kümesi API'leri kullanılarak) sorguları yazma. Başka bir tablo, sorgu çıktısı verir *sonuçlar tablosunu*. Sonuçları tablosu bir dış veri deposu, ilişkisel bir veritabanına veri çizin Sorgunuzun sonuçlarını içerir. Veriler girdi tablosundan zaman işlenir zamanlaması tarafından denetlenen *tetikleyici aralığı*. Varsayılan olarak, tetikleyici aralığı sıfır olduğundan, ulaşır ulaşmaz verileri işlemek yapılandırılmış akış çalışır. Yapılandırılmış akış yapılır hemen pratikte, bunun anlamı önceki Sorgu Çalıştır işleme, herhangi bir yeni alınan veri karşı çalıştırmak başka bir işlem başlatır. Veri akışı zamana dayalı toplu olarak işlenir tetikleyicinin bir aralıkta çalışabilmesi için yapılandırabilirsiniz. 

Veri tabloları tek sorgu en son yeni verileri zaman içerebilir sonuçlarında işlenmiş (*ekleme modu*), veya olduğundan her zaman yeni veri tablosu tüm çıktı verileri içerecek şekilde tablo tamamen yenilendi olabilir Akış sorgu başlamasından bu yana (*tam modu*).

### <a name="append-mode"></a>Ekleme modu

İçinde modu, yalnızca son sorgu çalıştırmanın sonuçlarını tabloda bulunan olduğundan sonuçları tabloya eklenen ve dış depolama birimine yazılan satırları ekleyin. Örneğin, en basit sorgu yalnızca tüm veriler girdi tablosundan değiştirilmemiş sonuçları tablosuna kopyalar. Bir tetikleyici aralığı sona erdiğinde, her seferinde yeni veriler işlenir ve bu yeni verileri temsil eden satırlar Sonuçlar tablosunda görünür. 

Burada bir thermostat gibi sıcaklık algılayıcıları telemetrisinden işleme bir senaryo düşünün. Saat 00:01 sıcaklık okuma 95 derece 1 aygıtla için bir olay ilk tetikleyici işlenen varsayalım. Sorgunun ilk tetikleyici yalnızca saat 00:01 satırla sonuçlar tablosunda görünür. Saat 00:02 başka bir olay geldiğinde, yalnızca yeni satır ile saat 00:02 satırıdır ve sonuçları tablo yalnızca bir satır böylece içerecektir.

![Ekleme modunda yapılandırılmış akış](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Ne zaman kullanarak ekleme modunda, sorgunuzu (hakkında cares sütun seçme), uygulama projeksiyonları (yalnızca belirli koşullara uyan satırları çekme) filtreleme veya (statik arama tablosu verilerle veri program.cs'ye) katılma. Bunu yalnızca ilgili yeni veri göndermek kolay dış depolama birimine işaret eder modu yapar ekleyin.

### <a name="complete-mode"></a>Tam modu

Aynı senaryo tam modunu kullanarak bu kez göz önünde bulundurun. Tablonun tüm çalışır ancak yalnızca çalıştırmak en son tetikleyici verileri içerecek şekilde tam modunda her tetikleyici tüm çıktı tablosu yenilenir. Tam modu girdi tablosundan sonuçları tabloya değiştirilmemiş verileri kopyalamak için kullanabilirsiniz. Her tetiklenen çalıştırılmasında yeni sonuç satırlarını yanı sıra önceki tüm satırları görüntülenir. Çıktı sonuçları tablosu tüm sorgu başlamıştır ve bellek yetersiz sonunda çalıştırılır toplanan verileri depolamak sona erer. Tam modu, bazı şekilde gelen özetlemek toplama sorguları kullanmaya yönelik ve vb. her tetikleyici sonuçlar tablosu ile yeni bir Özet da güncelleştirilir. 

Şu ana kadar beş saniyede eşitleyeceğini zaten işlenen veri vardır ve altıncı saniye verileri işlemek için zamanı varsayalım. Giriş tablosu saat 00:01 ve saat 00:03 için olayları vardır. Bu örnek sorgu amacı, beş saniyede ortalama sıcaklık cihazın vermektir. Bu sorgu uyarlamasını tüm her 5 saniyelik penceresi içinde kalan, sıcaklık ortalamasını alır ve bu aralığı içinde ortalama sıcaklık için bir satır üreten değerleri alan bir toplama geçerlidir. İlk 5 saniyelik penceresi sonunda iki başlığın vardır: (00:01, 1, 95) ve (00:03, 1, 98). Pencere 00:00-00 için bunu: 05 toplama 96.5 derece ortalama sıcaklık ile tanımlama grubu oluşturur. Sonraki 5 saniyelik penceresinde olmadığından yalnızca bir veri noktası saat 00:06 sonuçta elde edilen ortalama sıcaklık 98 derece döndürülür. Saat 00:10, tam modunu kullanarak sonuçları tablosunda hem de windows 00:00-00 olan satırlar: 05-00:05-00:10 sorgu tüm toplanan satırları çıkardığından değil yalnızca yenilerini. Bu nedenle sonuçlar tablosunu yeni windows eklendikçe büyümeye devam eder.    

![Tam modu akış yapılandırılmış](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Tam modunu kullanan tüm sorguları sınırları büyümeye tablo neden olur.  Önceki örnekte, yerine zaman penceresi tarafından sıcaklık ortalaması düşünün, bunun yerine bir cihaz kimliği ile ortalaması Sonuç tablosu bu aygıttan alınan tüm veri noktaları arasında cihaz için ortalama sıcaklığı ile sabit sayıda satır (bir cihaz başına) içerir. Yeni etme alındı olarak ortalamalar tablosunda her zaman geçerli; böylece sonuçları tablosu güncelleştirilir. 

## <a name="components-of-a-spark-structured-streaming-application"></a>Spark yapılandırılmış akış uygulamanın bileşenleri

Basit bir örnek sorgu topluluğu windows tarafından sıcaklık okumalar özetlemek. Bu durumda, verileri JSON dosyaları (Hdınsight kümesi için varsayılan depolama birimi olarak bağlı) Azure storage'da depolanır:

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Bu JSON dosyaları depolanmış `temps` Hdınsight küme kapsayıcısı altında alt klasörü. 

### <a name="define-the-input-source"></a>Giriş kaynağı tanımlayın

İlk veri kaynağı ve bu kaynak tarafından gerekli herhangi bir ayarı açıklayan bir DataFrame yapılandırın. Bu örnek Azure Storage JSON dosyalarından çizer ve bir şema bunlara okuma zaman uygular.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>Sorgu Uygula

Ardından, akış DataFrame karşı istenen işlemleri içeren bir sorgu uygulayın. Bu durumda, bir toplama tüm satırları 1 saatlik windows grupları ve o 1 saatlik penceresinde en az, ortalama ve en fazla etme hesaplar.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>Çıkış havuzunun tanımlayın

Ardından, sonuçlar tablosuna her tetikleyici aralıkta eklenen satırların hedef tanımlayın. Bu örnek yalnızca bir bellek içi tabloya tüm satırları çıkarır `temps` SparkSQL ile daha sonra sorgulayabilirsiniz. Tam çıkış modu, tüm windows için tüm satırların her çıktı olmasını sağlar.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>Sorgu Başlat

Akış sorgu başlatma ve sonlandırma sinyali alınana kadar çalıştırın. 

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>Sonuçları görüntüleme

Sorgu, aynı SparkSession içinde çalışırken bir SparkSQL sorgu çalıştırabilirsiniz `temps` sorgu sonuçlarını depolandığı tablo. 

    select * from temps

Bu sorgu, aşağıdakilere benzer sonuçlar verir:


| Pencere |  Min(Temp) | AVG(Temp) | max(Temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Spark yapılandırılmış akışı API'si hakkında daha fazla bilgi için giriş verilerinin yanı sıra kaynakları, işlemleri ve çıktı iç havuzlar, desteklediği için bkz: [Spark yapılandırılmış akış Programlama Kılavuzu](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html).

## <a name="checkpointing-and-write-ahead-logs"></a>Denetim noktası oluşturma ve yazma tamamlanan günlükleri

Dayanıklılık ve hataya dayanıklılık sağlamak üzere yapılandırılmış akış dayanan *denetim noktası oluşturma* Bu akış emin olmak için işleme düğümü hatalarıyla bile kesintisiz devam edebilirsiniz. Hdınsight'ta Spark Azure Storage veya Data Lake Store dayanıklı depolama denetim noktaları oluşturur. Bu kontrol noktalarının akış sorguyla ilgili ilerleme durumu bilgileri depolar. Ayrıca, yapılandırılmış akış kullanan bir *yazma tamamlanan günlük* (NİLEME). NİLEME aldı, ancak henüz bir sorgu tarafından işlenen alınan verileri yakalar. Bir hata oluşur ve işleme NİLEME yeniden kaynağından alınan olayları kayıp değildir.

## <a name="deploying-spark-streaming-applications"></a>Spark akış uygulamaları dağıtma

Genellikle bir Spark akış uygulamasına yerel olarak JAR dosyasını oluşturur ve bunu hdınsight'ta Spark Hdınsight kümenize bağlı varsayılan depolama JAR dosyasına kopyalayarak dağıtın. Uygulamanızı bir POST işlemini kullanarak kümenizden LIVY REST API'leri kullanılabilir başlatabilirsiniz. POSTANIN gövdesi, ana yöntem tanımlar ve akış uygulama ve isteğe bağlı olarak (örneğin, yürütücüler, bellek ve çekirdek sayısı) iş kaynak gereksinimlerini çalışır sınıfın adını, JAR yol sağlayan bir JSON belgesi içerir , ve uygulama kodunuz herhangi bir yapılandırma ayarı gerektirir.

![Spark akış uygulamasını dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumunu da LIVY uç noktası bir GET isteği ile denetlenebilir. Son olarak, çalışan bir uygulama LIVY uç noktası bir silme isteği göndererek sonlandırabilir. LIVY API'si hakkında daha fazla bilgi için bkz: [LIVY uzak işleriyle](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight'ta bir Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark akış programlama kılavuzu yapılandırılmış](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Spark işlerinin LIVY ile uzaktan başlatma](apache-spark-livy-rest-interface.md)