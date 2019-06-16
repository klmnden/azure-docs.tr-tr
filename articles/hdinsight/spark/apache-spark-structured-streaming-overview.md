---
title: Spark yapılandırılmış akışı Azure HDInsight
description: HDInsight Spark kümelerinde Spark yapılandırılmış akış uygulamaları kullanma
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: 0e9d87e5b344b7091a2a0cf41d6f7fa3484dfcf3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64711332"
---
# <a name="overview-of-apache-spark-structured-streaming"></a>Apache Spark yapılandırılmış akışı genel bakış

[Apache Spark](https://spark.apache.org/) yapılandırılmış akış veri akışlarını işlemeye ölçeklenebilir, yüksek performanslı, hataya dayanıklı uygulamalar uygulama olanak sağlar. Yapılandırılmış akış, Spark SQL altyapısı kurulmuştur ve temel Spark SQL veri çerçevelerden yapıları artırır ve akış aynı şekilde, sorgular yazabilmesi veri kümelerini toplu işlem sorguları yazmalısınız.  

Yapılandırılmış akış uygulamaları HDInsight Spark kümelerinde çalıştırılır ve veri akış bağlanma [Apache Kafka](https://kafka.apache.org/), bir TCP yuva (hata ayıklama amacıyla için) Azure depolama veya Azure Data Lake Storage. Dış depolama Hizmetleri'ni kullanır, ikinci iki seçenek, depolama alanına eklenen yeni dosyalar için izleyebilir ve akış gibi bunların içeriğini işlemek etkinleştirin. 

Yapılandırılmış akış sırasında işlemleri seçimi, projeksiyon, toplama, Pencereleme ve akış veri çerçevesi ile başvuru veri çerçevelerini katılma gibi giriş verileri için geçerli bir uzun süre çalışan sorgu oluşturur. Ardından, özel kod (örneğin, SQL veritabanı veya Power BI) kullanarak dosya depolama (Azure depolama Blobları veya Data Lake depolama) veya herhangi bir veri deposu sonuçları gönderir. HDInsight içinde hata ayıklama için oluşturulan verileri görebilirsiniz. Bu nedenle yapılandırılmış akış çıkışı yerel olarak hata ayıklama için konsolu ve bir bellek içi tablo için de sağlar. 

![Stream Spark yapılandırılmış akışını HDInsight ile işleme](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]  
> Spark yapılandırılmış akış, Spark Streaming (DStreams) yerini alıyor. Yalnızca Bakım modunda DStreams ancak bundan sonra yapılandırılmış akış geliştirmeleri ve Bakım alır. Yapılandırılmış akış şu anda olarak özellik DStreams kaynakları için eksiksiz değildir ve şu havuzlar kullanıma hazır destekler, bu nedenle, gereksinimlerinize uygun Spark akış işleme seçeneği seçin değerlendirin. 

## <a name="streams-as-tables"></a>Akışları olarak tabloları

Spark yapılandırılmış akış derinlik sınırsız bir tablo olarak veri akışını gösteren, diğer bir deyişle, tablonun yeni veriler ulaştıkça büyümeye devam. Bu *giriş tablosu* uzun süre çalışan sorgu ve gönderilen sonuçları tarafından sürekli olarak işlenen bir *çıktı tablosu*:

![Yapılandırılmış akış kavramı](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

Yapılandırılmış akış, verileri bir anda sistemin ulaşır ve hemen bir giriş tablosu alınır. Bu giriş tablosu karşılayacağımızı (veri çerçevesi ve veri kümesi API'leri kullanılarak) sorguları yazmanız. Başka bir tablo, sorgu çıktısı verir *sonuçlar tablosu*. Sonuçlar tablosu bir dış veri deposu, ilişkisel bir veritabanı için veri çizdiğiniz Sorgunuzun sonuçlarını içerir. Girdi tablosundan işlenen veri, zamanlama tarafından denetlenir *tetikleyici aralığı*. Yapılandırılmış akış çalıştığında, ulaşır ulaşmaz verileri işlemek için varsayılan olarak, tetikleyici aralığı sıfırdır. Yapılandırılmış akış tamamlandıktan hemen sonra uygulamada, bunun anlamı önceki sorguyu çalıştırma işleme, herhangi bir yeni alınan veri karşı çalıştırma başka bir işlem başlatır. Veri akışı zamana bağlı toplu olarak işlenir böylece, bir zaman aralığında, çalıştırılacak tetikleyici yapılandırabilirsiniz. 

Veri tabloları, en son yenilikler verileri tek bir sorgu zaman içerebilir sonuçlarında işlendi (*ekleme modunda açıldıysa*), veya var. her zaman yeni veriler tablonun tüm çıktı verilerini içerir. Bu nedenle tablo tamamen yenilenmiş olabilir Akış sorgu başlamasından bu yana (*tam modda*).

### <a name="append-mode"></a>Ekleme modu

İçinde modu, yalnızca son sorgu çalıştırma sonuç tablosunda var olduğundan, sonuçları tabloya eklenen ve dış depolama birimine yazılan satır ekleyin. Örneğin, en basit sorgu yalnızca tüm veriler girdi tablosundan değiştirilmeden sonuçları tabloya kopyalar. Bir tetikleyici aralığı sona erdiğinde, her zaman, yeni veriler işlendikten ve bu yeni verileri temsil eden satırlar Sonuçlar tablosunda görünür. 

Burada bir thermostat gibi sıcaklık sensörünün telemetri işleme bir senaryo düşünün. Saat 00:01 ile bir sıcaklık okuma 95 derece 1 cihazı için bir olay ilk tetikleyici işlenen varsayılır. Sorgunun ilk tetikleyici sonuçlar tablosunda yalnızca saat 00:01 satırı görünür. Saat 00:02, başka bir olay geldiğinde Saat 00:02 satırı yalnızca yeni satırdır ve bu nedenle sonuçlar tablosu yalnızca bir satır içerir.

![Ekleme modunda yapılandırılmış akış](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Ne zaman kullanarak ekleme modunda, sorgunuzu (Bu ilgilendiği sütunların seçilmesi), uygulama projeksiyonlar (belirli koşullara uyan satırları çekme) filtreleme veya (statik arama tablosu verilerden verilerle deneyimlerinizi) birleştirme. Dış depolama birimine işaret eder, yalnızca yeni ilgili veri göndermeye ilişkin kolay modu yapar ekleyin.

### <a name="complete-mode"></a>Tam modda

Tam modunu kullanarak bu kez aynı senaryoyu göz önünde bulundurun. Tablo, yalnızca en son tetikleyici çalıştırması, ancak tüm çalıştırmalardan verileri içerir. Bu nedenle tam modda, tüm çıktı tablosu her tetikleyici yenilenir. Tam modda, sonuçları tabloya girdi tablosundan değiştirilmeden veri kopyalamak için kullanabilirsiniz. Tetiklenen her çalıştırma, önceki tüm satırları yanı sıra yeni sonucu satırları görünür. Çıktı sonuçları tablosu tüm sorgu başladı ve sonunda bellek taşmasına uğruyor toplanan veriler depolama sona erecek. Tam modda şekilde gelen verileri özetleyen toplama sorguları ile kullanılmak üzere tasarlanmıştır ve bu şekilde her tetikleyici sonuçlar tablosu ile yeni bir Özet da güncelleştirilir. 

Şu ana kadar beş saniye değerinde veri önceden işlenmiş vardır ve altıncı saniye veriyi işlemek için zaman olduğu varsayılır. Girdi tablosu, saat 00:01 ve saat 00:03 olayları vardır. Bu örnek sorgu amacı, beş saniyede cihazın ortalama sıcaklık vermektir. Bu sorgu yürütmesinin tüm her 5 saniyelik aralıklarda içinde kalan, Sıcaklığın ortalamasını alan ve bu zaman aralığında ortalama sıcaklık için bir satır üretir değerleri alan bir toplama geçerlidir. İlk 5 saniyelik pencerenin sonunda iki vardır: (00:01, 1, 95) ve (00:03, 1, 98). 00:00-00 penceresi için bunu: 05 toplama 96.5 derece cinsinden ortalama sıcaklık ile tanımlama grubu oluşturur. Sonraki 5 saniyelik penceresinde var. yalnızca bir veri noktası saat 00:06 sonuçta elde edilen ortalama sıcaklık 98 derece, bu nedenle Saat 00: 10'da tam modunu kullanarak sonuçları tablo satırları hem windows 00:00-00 sahip: 05-00:05-00: Sorgu tüm toplanan satırları çıkardığından 10 değil yalnızca yeni bir tane. Bu nedenle sonuçlar tablosu yeni windows eklendikçe devam etmektedir.    

![Yapılandırılmış akış tam modu](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Tam modda kullanarak tüm sorgular tablonun sınırlar büyümeye neden olur.  Önceki örnekte, yerine tarafından zaman penceresi, bunun yerine cihaz kimliğine göre ortalama Sıcaklığın ortalamasını göz önünde bulundurun Sonuç tablosunun bu CİHAZDAN alınan tüm veri noktaları arasında cihaz için ortalama sıcaklık ile sabit sayıda satır (cihaz başına bir adet) içerir. Yeni Sıcaklıkların alındıkça, sonuçlar tablosu ortalamalar tablosunda her zaman geçerli olacak şekilde güncelleştirilir. 

## <a name="components-of-a-spark-structured-streaming-application"></a>Spark yapılandırılmış akışı bir uygulamanın bileşenleri

Basit bir örnek sorgu topluluğu windows tarafından sıcaklık okumalar özetleyebilir. Bu durumda, verilerin Azure (HDInsight kümesi için varsayılan depolama alanı olarak bağlı) depolama JSON dosyalarında depolanır:

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Bu JSON dosyaları depolanan `temps` HDInsight küme kapsayıcı altındaki alt. 

### <a name="define-the-input-source"></a>Giriş kaynağı tanımlayın

İlk veri kaynağını ve bu kaynaktan gerekli herhangi bir ayarı açıklayan bir DataFrame yapılandırın. Bu örnek JSON dosyalarını Azure storage'da çizer ve bir şema için bunları okuma zaman uygular.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>Sorgu Uygula

Ardından, akış veri çerçevesi istenen işlemler içeren bir sorgusunu uygulayın. Bu durumda, bir toplama tüm satırları 1 saatlik pencerelere gruplandırır ve ardından bu 1 saatlik pencerede minimum, ortalama ve maksimum Sıcaklıkların hesaplar.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>Çıkış havuzu tanımlayın

Ardından, her tetikleyici aralıkta sonuçları tabloya eklenen satırlar için hedef tanımlayın. Bu örnekte yalnızca bir bellek içi tablo için tüm satırları çıkaran `temps` SparkSQL ile daha sonra sorgulayabilirsiniz. Tam çıkış modu, tüm windows için tüm satırların her zaman çıkış olmasını sağlar.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>Sorgu Başlat

Akış sorgu başlangıç ve sonlandırma sinyal alınana kadar çalıştırın. 

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>Sonuçları görüntüleme

Sorgu çalışırken aynı SparkSession içinde SparkSQL sorgu çalıştırabilirsiniz `temps` tablo sorgu sonuçları nerede depolanır. 

    select * from temps

Bu sorgu, aşağıdakine benzer bir sonuç verir:


| Pencere |  Min(Temp) | AVG(Temp) | max(Temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Spark yapılandırılmış Stream API hakkında daha fazla bilgi için giriş verilerinin yanı sıra kaynakları, operations ve çıkış havuzlarını, desteklediği için bkz: [Apache Spark yapılandırılmış akış Programlama Kılavuzu](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html).

## <a name="checkpointing-and-write-ahead-logs"></a>Denetim noktası oluşturma ve yazma tamamlanan günlükleri

Dayanıklılık ve hataya dayanıklılık sağlamak için yapılandırılmış akış dayanan *denetim noktası* bu emin olmak için işleme düğüm hataları olsa da, kesintisiz devam edebilirsiniz. HDInsight Spark kontrol noktalarını kalıcı depolama, Azure depolama ya da Data Lake Storage oluşturur. Bu kontrol noktalarının akış sorgu ilerleme bilgilerini depolar. Ayrıca, yapılandırılmış akış'ı kullanan bir *yazma önceden yazılan günlük* (WAL). WAL aldı, ancak henüz bir sorgu tarafından işlenen alınan verileri yakalar. Bir hata oluşur ve işleme WAL başlatılır, kaynaktan alınan herhangi bir olayı kaybolmaz.

## <a name="deploying-spark-streaming-applications"></a>Spark akış uygulamaları dağıtma

Genellikle bir JAR dosyasına yerel olarak Spark akışı bir uygulama oluşturun ve ardından HDInsight üzerinde Spark HDInsight kümenize bağlı varsayılan depolama alanı için JAR dosyasını kopyalayarak dağıtın. Uygulamanızla birlikte başlatabilirsiniz [Apache Livy](https://livy.incubator.apache.org/) REST API'leri kullanarak bir gönderme işlemi kümenizden kullanılabilir. Gönderinin gövdesi olan ana yöntem tanımlar ve akış uygulaması ve isteğe bağlı olarak kaynak gereksinimlerini (örneğin, yürütücüler, bellek ve çekirdek sayısı) işin çalışan sınıfın adı, JAR yol sağlayan bir JSON belgesini içerir. , ve uygulama kodunuzun herhangi bir yapılandırma ayarı gerektirir.

![Bir Spark akışı uygulamasını dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumunu, ayrıca bir LIVY uç noktasına karşı bir GET isteğiyle denetlenebilir. Son olarak, LIVY uç nokta karşı bir silme isteği göndererek çalışan bir uygulamayı sonlandırabilirsiniz. LIVY API hakkında daha fazla bilgi için bkz: [Apache LIVY ile uzak işler](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Apache Spark yapılandırılmış akış Programlama Kılavuzu](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Apache Spark işleri Apache LIVY ile uzaktan başlatma](apache-spark-livy-rest-interface.md)
