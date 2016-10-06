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
    ms.date="09/22/2016" 
    ms.author="spelluru"/>


# Buluttaki bir veri tümleştirme hizmeti olan Azure Data Factory Hizmetine giriş

## Azure Data Factory nedir? 
Data Factory, verilerin **taşınmasını** ve **dönüştürülmesini** düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory hizmetini kullanarak Çeşitli veri depolarından veri alabilen, verileri dönüştürebilen/işleyebilen ve sonuç verilerini veri depolarında yayımlayabilen veri tümleştirme çözümleri oluşturabilirsiniz. 

Data Factory hizmeti verileri taşıyıp dönüştüren ve ardından işlem hattını belirli bir zamanlamaya (saatlik, günlük, haftalık, vb.) göre çalıştıran veri işlem hatları oluşturmanıza imkan tanır. Ayrıca, veri işlem hatlarınız arasındaki çizgileri ve bağımlılıkları gösteren ve sorunları kolayca saptamak ve izleme uyarılarını ayarlamak üzere tek bir birleşik görünümden tüm veri işlem hatlarınızı izleyen zengin görsel öğeler sağlar.

![Diyagram: Bir veri tümleştirme hizmeti olan Data Factory’ye Genel Bakış](./media/data-factory-introduction/what-is-azure-data-factory.png)
**Şekil 1.** Çeşitli veri kaynaklarından veri alın, bu verileri hazırlayın, dönüştürün ve analiz edin, ardından kullanıma hazır verileri tüketim için yayımlayın.

## İşlem hatları ve etkinlikler
Bir Data Factory çözümünde bir veya daha fazla **işlem hattı** oluşturursunuz. İşlem hattı mantıksal bir etkinlik gruplandırmasıdır. Bunlar, görevi birlikte gerçekleştiren bir birimde etkinlikleri gruplandırmak için kullanılır. 

**Etkinlikler** verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için Kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri. 
  
## Veri taşıma etkinlikleri 
[AZURE.INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 

## Veri dönüştürme etkinlikleri
[AZURE.INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız ya da kendi mantığınızı kullanarak verileri dönüştürmeniz gerekirse **özel bir .NET etkinliği** oluşturun. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

## Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bilgileri tanımlar. (Örnekler: Azure Storage, şirket içi SQL Server, Azure HDInsight). Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

- Bir **veri deposunu**, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server, Oracle veritabanı, dosya paylaşımı veya bir Azure Blob Depolama hesabı. Desteklenen veri depolarının bir listesi için [Veri taşıma etkinlikleri](data-factory-data-movement-activities.md) bölümüne bakın. 
- Etkinlik yürütülmesini barındırabilen **işlem kaynağını** temsil etmek için. Örneğin, HDInsightHive etkinliği bir HDInsight Hadoop kümesinde yürütülür. Desteklenen işlem ortamlarının listesi için [Veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) bölümüne bakın. 

## Veri kümeleri 
Bağlı hizmetler veri depolarını bir Azure data factory’ye bağlar. Veri kümeleri, veri depolarındaki veri yapılarını temsil eder. Örneğin, Azure Storage bağlı hizmeti Data Factory’nin bir Azure Storage hesabına bağlanması için bağlantı bilgileri sağlar. Azure Blob veri kümesi, işlem hattının verileri okuması gereken blob kapsayıcısını ve Azure Blob Depolama klasörünü belirtir. Benzer şekilde, Azure SQL bağlı hizmeti bir Azure SQL veritabanına ilişkin bağlantı bilgilerini sağlar ve Azure SQL veri kümesi verileri içeren tabloyu belirtir.   

## Data Factory varlıkları arasındaki ilişki
Data Factory’de girdi ve çıktı verilerini, olayları işlemeyi, istenen veri akışını yürütecek gerekli zamanlamayı ve kaynakları tanımlamak için birkaç önemli varlık vardır.

![Diyagram: Bir bulut veri tümleştirme hizmeti olan Data Factory - Temel Kavramlar](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Şekil 2.** Veri kümesi, Etkinlik, İşlem Hattı ve Bağlı hizmet arasındaki ilişkiler

Bağlı hizmetler, veri kümeleri, etkinlikler ve işlem hatlarından oluşan dört basit kavramla başlamaya hazırsınız! [İlk işlem hattınızı oluşturabilirsiniz](data-factory-build-your-first-pipeline.md). 

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


<!--HONumber=Sep16_HO4-->


