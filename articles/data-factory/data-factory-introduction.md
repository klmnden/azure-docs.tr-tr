<properties 
    pageTitle="Bir veri tümleştirme hizmeti olan Data Factory’ye giriş | Microsoft Azure" 
    description="Azure Data Factory’nin ne olduğunu öğrenin: verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmetidir." 
    keywords="veri tümleştirme, bulut veri tümleştirme, azure data factory nedir"
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
    ms.date="09/08/2016" 
    ms.author="spelluru"/>


# Buluttaki bir veri tümleştirme hizmeti olan Azure Data Factory Hizmetine giriş

## Azure Data Factory nedir? 
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Tam da, ham maddeleri alıp, bunları tamamlanmış ürünlere dönüştürmek için donanım çalıştıran üretim fabrikası gibi Data Factory de ham verileri toplayan var olan hizmetleri düzenler ve bunları kullanıma hazır bilgilere dönüştürür. 

Data Factory, verilerinizi almak, hazırlamak, dönüştürmek, analiz etmek ve yayımlamak için şirket içinde, bulut veri kaynaklarında ve SaaS’de çalışır. Data Factory’yi, büyük verilere ait bilgi işlem gereksinimlerinize yönelik olarak [Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) ve [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) gibi veri kullanan hizmetlerinizi dönüştürmek üzere yönetilen veri akışı işlem hatlarına yerleştirilecek hizmetler oluşturmak ve analiz çözümlerinizi [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) ile faaliyete geçirmek için kullanın. Tablo izleme görünümünün ötesine geçin, veri işlem hatlarınız arasında çizgi ve bağımlılığı hemen görüntülemek için Data Factory’nin zengin görselleştirmelerini kullanın. Sorunları kolayca saptamak ve izleme uyarıları ayarlamak için birleşik tek bir görünümden veri akışı işlem hatlarınızın tümünü izleyin.

![Diyagram: Bir veri tümleştirme hizmeti olan Data Factory’ye Genel Bakış](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Şekil1.** Şirket içi pek çok kaynaktan veri toplayın, bu verileri alın, hazırlayın, dönüştürün ve analiz edin, son olarak da kullanıma hazır verileri tüketim için yayımlayın.

Farklı şekillerdeki ve boyutlardaki verileri toplamanız, bunları dönüştürmeniz ve derin öngörüyü genişletmek amacıyla bunları yayımlamanız gerektiğinde, tümü de güvenilir bir zamanlamayla Data Factory’yi her istediğinizde kullanabilirsiniz. Data Factory, kendi analiz işlem hattı gereksinimleri için farklı endüstriler arasında pek çok senaryoyla ilgili çok uygun veri akışı işlem hatlarını oluşturmak amacıyla kullanılır. Çevrimiçi perakendeciler, müşterinin göz atma davranışlarına dayanan kişiselleştirilmiş [ürün önerilerini](data-factory-product-reco-usecase.md) oluşturmak için bunları kullanır. Oyun stüdyoları [pazarlama kampanyalarının etkisini](data-factory-customer-profiling-usecase.md) anlamak için bunları kullanır. [Müşteri Örnek Olay İncelemeleri](data-factory-customer-case-studies.md)’ni gözden geçirerek müşterilerimizin Data Factory’yi nasıl ve neden kullandığını doğrudan kendilerinden öğrenin. 

> [AZURE.VIDEO azure-data-factory-overview]

## Önemli Kavramlar

Data Factory’de girdi ve çıktı verilerini, olayları işlemeyi, istenen veri akışını yürütecek gerekli zamanlamayı ve kaynakları tanımlamak için birkaç önemli varlık vardır.

![Diyagram: Bir bulut veri tümleştirme hizmeti olan Data Factory - Temel Kavramlar](./media/data-factory-introduction/data-integration-service-key-concepts.png)

**Şekil 2.** Veri kümesi, Etkinlik, İşlem Hattı ve Bağlı hizmet arasındaki ilişkiler

### İşlem hatları
[İşlem Hatları](data-factory-create-pipelines.md) Etkinliklerin mantıksal bir gruplandırmasıdır. Bunlar, görevi birlikte gerçekleştiren bir birimde etkinlikleri gruplandırmak için kullanılır. Örneğin, birkaç dönüştürme Etkinliği dizisi günlük dosyası verilerini temizlemek için gerekebilir. Bu dizide, düzenlenmesi ve otomatikleştirilmesi gereken karmaşık zamanlama ve bağımsızlıklar olabilir. Bu etkinliklerin tümü “CleanLogFiles” adlı tek bir İşlem Hattı’nda gruplandırılır. Böylece, her etkinliği ayrı ayrı bağımsız olarak yönetmek yerine "CleanLogFiles" tek bir birim halinde dağıtılır, zamanlanır veya silinir.

### Etkinlikler
Etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlayın. Her etkinlik girdi olarak sıfır veya daha fazla [veri kümesi](data-factory-create-datasets.md) alır ve bir veya daha fazla veri kümesini çıktı olarak üretir. Etkinlik, Azure Data Factory’de bir düzenleme birimidir. Örneğin, verilerin bir veri kümesinden başkasına kopyalanmasını düzenlemek için [Kopyalama etkinliğini](data-factory-data-movement-activities.md) kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir [Hive etkinliği](data-factory-data-transformation-activities.md) kullanabilirsiniz. Azure Data Factory veri dönüştürmenin, analizinin ve veri taşıma etkinliklerinin geniş bir yelpazesini sağlar. 

### Veri kümeleri
[Veri kümeleri](data-factory-create-datasets.md), Etkinliğini girdisi veya çıktısı olarak kullanmak istediğiniz verilere adlandırılan başvurular/yönlendirmelerdir. Veri kümeleri veri yapılarını tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarında tanımlar.

### Bağlı hizmet
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bilgileri tanımlar. Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

- Bir veri deposunu, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server, Oracle DB, Dosya paylaşımı veya bir Azure Blob Depolama hesabı. Daha önce bahsedildiği gibi Veri Kümeleri, Bağlı hizmet aracılığıyla Data Factory’ye bağlanan veri depoları içindeki veri yapılarını temsil eder.
- Etkinlik yürütülmesini barındırabilen işlem kaynağını temsil etmek için. Örneğin, "HDInsightHive Etkinliği" HDInsight Hadoop kümesinde yürütülür.

Veri kümelerinin, etkinliklerin, işlem hatlarının ve bağlı hizmetlerin dört basit kavramıyla artık başlamaya hazırsınız! [İlk işlem hattınızı](data-factory-build-your-first-pipeline.md) derleyebilir veya [Data Factory Örnekleri](data-factory-samples.md) makalemizdeki yönergeleri uygulayarak hazır bir örneği dağıtabilirsiniz. 

## Desteklenen bölgeler
Şu anda **Batı ABD**, **Doğu ABD** ve **Kuzey Avrupa** bölgelerinde veri fabrikaları oluşturabilirsiniz. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir. 

Azure Data Factory’nin kendisi verileri depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde akışlar oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar. 

Azure Data Factory yalnızca **Batı ABD**, **Doğu ABD** **Kuzey Avrupa** bölgelerinde kullanılabilir olsa da, Data Factory’de veri taşımayı destekleyen hizmet birçok bölgede [küresel olarak](data-factory-data-movement-activities.md#global) kullanılabilmektedir. Veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamınızda yüklü bir [Veri Yönetimi Ağ Geçidi](data-factory-move-data-between-onprem-and-cloud.md) bunun yerine verileri taşır. 

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgesinde çalıştığını varsayalım. Kuzey Avrupa’da bir Azure Data Factory örneği oluşturup geliştirebilir ve bunu Batı Avrupa’daki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory’nin işlem ortamınızda işi tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin çalıştırılma süresi değişmez.

Gelecekte Azure tarafından desteklenen Azure Data Factory’nin her coğrafi bölge için kullanılır olmasını planlamaktayız.
  
## Sonraki adımlar
Veri işlem hatları ile veri fabrikaları oluşturmayı öğrenmek için aşağıdaki öğreticilerde yer alan adım adım yönergeleri izleyin. 

Öğretici | Açıklama
-------- | -----------
[Hadoop kümesi kullanarak veri işleyen bir veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) | Bu öğreticide, bir Azure HDInsight (Hadoop) kümesinde Hive betiği çalıştırarak **veri işleyen** bir veri işlem hattı ile ilk Azure veri fabrikanızı oluşturacaksınız. |
[Verileri iki bulut veri deposu arasında taşımak için veri işlem hattı oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Bu öğreticide, Blob depolama biriminden SQL veritabanına **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız.
[Veri Yönetimi Ağ Geçidi kullanarak verileri şirket içi veri deposu ile bulut veri deposu arasında taşımak üzere veri işlem hattı oluşturma](data-factory-move-data-between-onprem-and-cloud.md) | Bu öğreticide, **şirket içi** SQL Server veritabanından Azure blob’a **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız. 


<!--HONumber=Sep16_HO3-->


