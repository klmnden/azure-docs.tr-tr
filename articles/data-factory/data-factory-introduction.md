<properties 
    pageTitle="Azure Data Factory'ye giriş" 
    description="Güvenli bilgiler oluşturan işlem hatlarını oluşturmak için veri işleme, veri depolama ve veri taşıma hizmetlerini birleştirecek Azure Data Factory hizmeti hakkında bilgi edinin." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="04/26/2016" 
    ms.author="spelluru"/>

# Azure Data Factory Hizmeti’ne giriş

## Genel Bakış
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Tam da, ham maddeleri alıp, bunları tamamlanmış ürünlere dönüştürmek için donanım çalıştıran üretim fabrikası gibi Data Factory de ham verileri toplayan var olan hizmetleri düzenler ve bunları kullanıma hazır bilgilere dönüştürür. 

Data Factory, verilerinizi almak, hazırlamak, dönüştürmek, analiz etmek ve yayımlamak için şirket içinde, bulut veri kaynaklarında ve SaaS’de çalışır.  Data Factory’yi, gereksinimleri hesaplayan büyük verilerinize yönelik, [Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) ve [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) gibi hizmetleri kullanarak verilerinizi dönüştürecek yönetilen veri akışı işlem hatlarında birleştirmek için kullanın; [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) ile de analiz çözümleriniz çalıştırılır.  Tablo izleme görünümünün ötesine geçin, veri işlem hatlarınız arasında çizgi ve bağımlılığı hemen görüntülemek için Data Factory’nin zengin görselleştirmelerini kullanın. Sorunları kolayca saptamak ve izleme uyarılarını ayarlamak için tek benzersiz bir görünümden veri akışı işlem hatlarınızın tümünü izleyin.

![Genel Bakış](./media/data-factory-introduction/data-factory-overview.png)

**Şekil1.** Verileri şirket içi pek çok kaynaktan toplayın, bunları alın ve hazırlayın, bir dizi işlemle bunları düzenleyin ve analiz edin, son olarak da kullanıma hazır verileri tüketim için yayımlayın.

Farklı şekillerdeki ve boyutlardaki verileri toplamanız, bunları dönüştürmeniz ve derin öngörüyü genişletmek amacıyla bunları yayımlamanız gerektiğinde, tümü de güvenilir bir zamanlamayla Data Factory’yi her istediğinizde kullanabilirsiniz. Data Factory, kendi analiz işlem hattı gereksinimleri için farklı endüstriler arasında pek çok senaryoyla ilgili çok uygun veri akışı işlem hatlarını oluşturmak amacıyla kullanılır.  Çevrimiçi perakendeciler, müşterinin göz atma davranışlarına dayanan kişiselleştirilmiş [ürün önerilerini](data-factory-product-reco-usecase.md) oluşturmak için bunları kullanır. Oyun stüdyoları [pazarlama kampanyalarının etkisini](data-factory-customer-profiling-usecase.md) anlamak için bunları kullanır. [Müşteri Örnek Olay İncelemeleri](data-factory-customer-case-studies.md)’ni gözden geçirerek müşterilerimizin Data Factory’yi nasıl ve neden kullandığını doğrudan kendilerinden öğrenin. 

> [AZURE.VIDEO azure-data-factory-overview]

## Önemli Kavramlar

Azure Data Factory’de girdi ve çıktı verilerini, olayları işlemeyi, istenen veri akışını yürütecek gerekli zamanlamayı ve kaynakları tanımlamak için birkaç önemli varlık vardır.

![Önemli Kavramlar](./media/data-factory-introduction/key-concepts.png)

**Şekil 2.** Veri kümesi, Etkinlik, İşlem Hattı ve Bağlı hizmet arasındaki ilişkiler


### Etkinlikler
Etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlayın. Her etkinlik girdi olarak sıfır veya daha fazla [veri kümesi](data-factory-create-datasets.md) alır ve bir veya daha fazla veri kümesini çıktı olarak üretir. Etkinlik, Azure Data Factory’de bir düzenleme birimidir. Örneğin, verilerin bir veri kümesinden başkasına kopyalanmasını düzenlemek için [Kopyalama etkinliğini](data-factory-data-movement-activities.md) kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran [Hive etkinliğini](data-factory-data-transformation-activities.md) kullanabilirsiniz. Azure Data Factory veri dönüştürmenin, analizinin ve veri taşıma etkinliklerinin geniş bir yelpazesini sağlar. 

### İşlem hatları
[İşlem Hatları](data-factory-create-pipelines.md) Etkinliklerin mantıksal bir gruplandırmasıdır. Bunlar, görevi birlikte gerçekleştiren bir birimde etkinlikleri gruplandırmak için kullanılır. Örneğin, birkaç dönüştürme Etkinliği dizisi günlük dosyası verilerini temizlemek için gerekebilir.  Bu dizide, düzenlenmesi ve otomatikleştirilmesi gereken karmaşık zamanlama ve bağımsızlıklar olabilir. Bu etkinliklerin tümü “CleanLogFiles” adlı tek bir İşlem Hattı’nda gruplandırılır.  "CleanLogFiles" daha sonra tek tek etkinlikleri bağımsız olarak yönetme yerine tek bir birimde dağıtılır, zamanlanır veya silinir.

### Veri kümeleri
[Veri kümeleri](data-factory-create-datasets.md), Etkinliğini girdisi veya çıktısı olarak kullanmak istediğiniz verilere adlandırılan başvurular/yönlendirmelerdir. Veri kümeleri veri yapılarını tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarında tanımlar.

### Bağlı hizmet
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bilgileri tanımlar.  Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

- Bir veri deposunu, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server, Oracle DB, Dosya paylaşımı veya bir Azure Blob Storage hesabı.   Yukarıda da söz edildiği gibi Veri Kümeleri Bağlı hizmet aracılığıyla Data Factory’ye bağlanan veri depoları içindeki yapıları temsil eder.
- Etkinlik yürütülmesini barındırabilen işlem kaynağını temsil etmek için.  Örneğin, "HDInsightHive Etkinliği" HDInsight Hadoop kümesinde yürütülür.

Veri kümelerinin, etkinliklerin, işlem hatlarının ve bağlı hizmetlerin dört basit kavramıyla artık başlamaya hazırsınız!  [İlk işlem hattınızı](data-factory-build-your-first-pipeline.md) sıfırdan derleyebilir, [Data Factory Örnekleri](data-factory-samples.md) makalemizdeki yönergeleri uygulayarak da oluşturmaya hazır örnekleri dağıtabilirsiniz. 

## Desteklenen bölgeler
Data factory’leri **Batı ABD**, **Doğu ABD** ve **Kuzey Avrupa** bölgelerinde aynı anda oluşturabilirsiniz. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir. 

Azure Data Factory’nin kendisi verileri depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde akışlar oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar. 

Azure Data Factory yalnızca **Batı ABD**, **Kuzey Avrupa** ve **Kuzey Avrupa** bölgelerinde kullanılabilir olsa bile, Data Factory’de veri taşımayı destekleyen hizmet birçok bölgede[küresel olara](data-factory-data-movement-activities.md#global) kullanılabilir. Veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamda yüklü bir [Veri Yönetimi Ağ Geçidi](data-factory-move-data-between-onprem-and-cloud.md) bunun yerine verileri taşır. 

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa dışındaki bölgelerde çalıştığını var sayalım. Kuzey Avrupa’da bir Azure Data Factory örneğini oluşturabilir ve geliştirebilir ve bunu Batı Avrupa’da işlem ortamlarınızda işleri zamanlamak için kullanabilirsiniz. Data Factory hizmetinin işi işlem ortamınızda tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin yürütüldüğü süre değişmez.

Gelecekte Azure tarafından desteklenen Azure Data Factory’nin her coğrafi bölge için kullanılır olmasını planlamaktayız.
  




<!---HONumber=Jun16_HO2-->


