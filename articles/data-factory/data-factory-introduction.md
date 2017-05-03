---
title: "Bir veri tümleştirme hizmeti olan Data Factory’ye giriş | Microsoft Belgeleri"
description: "Azure Data Factory’nin ne olduğunu öğrenin: verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmetidir."
keywords: "veri tümleştirme, bulut veri tümleştirme, azure data factory nedir"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/21/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 260208e7c7a08110eb3c885ef86ec4c18ff42fc9
ms.openlocfilehash: 40552b5d3cea5b04826c08e7b4b1d046a9fcefba
ms.lasthandoff: 04/23/2017


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Buluttaki bir veri tümleştirme hizmeti olan Azure Data Factory Hizmetine giriş
## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?
Büyük veri dünyasında, işletmede mevcut verilerden nasıl yararlanılır? Şirket içi veri kaynaklarından veya diğer dağınık veri kaynaklarından elde edilen başvuru verilerini kullanarak bulutta oluşturulan verileri zenginleştirmek mümkün mü? Şöyle ki, çok çeşitli veri kaynaklarından gelen verileri toplamak ve işlemek için bir platform gereklidir. Azure Data Factory, verilerin **taşınmasını** ve **dönüştürülmesini** düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Dağınık veri depolarından giriş verileri alabilen, verileri dönüştürebilen/işleyebilen ve çıkış verilerini diğer veri depolarında yayımlayabilen veri tümleştirme çözümleri oluşturabilirsiniz. 

![Diyagram: Bir veri tümleştirme hizmeti olan Data Factory’ye Genel Bakış](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Şekil1.** Çeşitli veri kaynaklarından veri alın, bu verileri hazırlayın, dönüştürün ve analiz edin, ardından kullanıma hazır verileri tüketim için yayımlayın.


## <a name="what-does-it-offer"></a>Neler sağlar? 
Geleneksel olarak, veri tümleştirme projeleri aşağıdaki resimde gösterildiği gibi kuruluş içindeki çeşitli veri kaynaklarından Çıkartma-Dönüştürme-Yükleme (ETL) süreçleri oluşturma, verileri Kurumsal Veri Ambarı’nın (EDW) hedef şemasına uyacak şekilde dönüştürme ve verileri EDW’ye yükleme işlemleri çerçevesinde gelişir. Ardından BI analiz çözümlerinin tek doğru kaynağı olarak EDW’ye erişilir.

![Geleneksel ETL](media/data-factory-introduction/traditional-etl.png)
**Geleneksel ETL**

Günümüzde kuruluşların veri manzarası aşağıdaki resimde gösterildiği gibi hacim, çeşitlilik ve karmaşıklık açısından katlanarak büyümeye devam etmektedir. Farklı biçimlerde ve hızlarda şirket içinden ve buluttan kaynaklanan verilerle her zamankinden daha çeşitli hale gelmiştir. Veri işleme işleminin coğrafi konumlar arasında gerçekleştirilmesi gerekir ve pahalı, tümleştirmesi ve bakımı zor olan açık kaynak yazılımları, ticari çözümler ve özel işleme hizmetlerinin bir bileşimini içerir. Günümüzün değişen büyük veri manzarasına uyum sağlamak için gereken çeviklik, geleneksel EDW ile modern bilgi üretim sistemine gereken özellikleri birleştirmek için bir fırsat sağlar. Azure Data Factory, kuruluşlara veri odaklı karar almaya yönelik olarak sağlanan tüm verilerden yararlanma gücü vermek için geleneksel EDW’ler ve değişen veri manzarası genelinde çalışma olanağı tanıyan birleştirme platformudur.

![Yeni büyük veri manzarası](media/data-factory-introduction/new-big-data-landscape.png)
**Yeni büyük veri manzarası**

Azure Data Factory hizmeti **veri işleme, depolama ve taşıma hizmetlerinden bilgi üretim kanalları oluşturma** ve güvenilen veri varlıklarını yönetmeye yönelik bir platform sağlayarak, kuruluşlara bu çeşitlilikten yararlanma gücü getirir.

Azure Data Factory hizmetiyle şunları yapabilirsiniz:
- **Çeşitli veri depolama ve işleme sistemleriyle kolayca çalışabilirsiniz**. 

    Kuruluşların dağınık kaynaklarda yer alan birbirinden farklı türlerde verileri vardır. Bilgi üretim sistemi oluşturmanın ilk adımı SaaS hizmetleri, dosya paylaşımları, FTP, web hizmetleri gibi tüm gerekli veri kaynaklarına ve işleme çalışmalarına bağlanmak ve daha sonraki işleme çalışmaları için gerektiğinde verileri merkezi bir konuma taşımaktır.

    Data Factory olmadığında, kuruluşların bu veri kaynaklarını ve işleme çalışmalarını tümleştirmek için özel veri taşıma bileşenleri oluşturması veya özel hizmetler yazması gerekir. Bu tür sistemler pahalıdır, tümleştirmesi ve bakımı zordur; ayrıca tümüyle yönetilen bir hizmetin sağlayabileceği kurumsal sınıf izleme ve uyarılarla denetimlerden yoksundur.

    Data Factory ile, veri işlem hattında Kopyalama Etkinliği’ni kullanarak hem şirket içinde hem de buluttaki kaynak veri depolarını daha fazla analiz için merkezi bir veri deposuna taşıyabilirsiniz. Örneğin, Azure Data Lake Store’da veri toplayabilir ve daha sonra Azure Data Lake Analytics işlem hizmetini kullanarak verileri dönüştürebilirsiniz. Öte yandan, verileri Azure Blob Depolama Alanı’ndan toplayıp daha sonra Azure HDInsight Hadoop kümesi kullanarak da dönüştürebilirsiniz.
- **Verileri güvenilir bilgilere dönüştürün**. 

    Veriler bulutta merkezi bir veri deposunda olduğunda, üretim ortamlarını güvenilir verilerle beslemek için, korunabilir ve denetlenebilir bir zamanlamayla, güvenilir bir şekilde dönüştürülmüş veriler oluşturmak üzere veri işlem hatlarını yazmak ve dağıtmak istersiniz. Azure Data Factory’de veri dönüştürme işlemi Hive, Pig, MapReduce, Azure Machine Learning Batch Execution ve özel C# etkinlikleri (Azure HDInsight Hadoop kümesinde, Azure Machine Learning VM’lerinde veya VM’lerin Azure Toplu İşlem havuzunda çalışan etkinlikler) gibi dönüştürme etkinlikleri tarafından gerçekleştirilir.
- **Veri işlem hattını tek bir yerden izleyin**.

    Farklı verileri içeren portföyde, depolama, işleme ve veri taşıma hizmetlerinizi güvenilir bir şekilde ve tam olarak görüntüleyebilmek önemlidir. Data Factory, uçtan uca veri işlem hattının durumunu hızlı bir şekilde değerlendirmenize, sorunları belirlemenize ve gerekiyorsa düzeltme eylemi uygulamanıza yardımcı olur. Verilerinizin kökenini ve kaynaklarınızdaki verilerin birbirleriyle aralarındaki ilişkilerini görsel olarak izleyin. İş yürütme, sistem durumu ve bağımlılıklara ilişkin hesaplama geçmişini tek bir panoda görüntüleyin.

## <a name="how-does-it-work"></a>Nasıl çalışır? 
Azure Data Factory’de üç bilgi üretim aşaması vardır:

![Üç bilgi üretim aşaması](media/data-factory-introduction/three-information-production-stages.png)

- **Bağlanma ve Toplama**: Bu aşamada, çeşitli veri kaynaklarındaki veriler tek bir yerde toplanır.
- **Dönüştürme ve Zenginleştirme**: Bu aşamada, toplanan veriler işlenir veya dönüştürülür.
- **Yayımlama**. Bu aşamada, veriler yayımlanarak BI araçları, analiz araçları ve diğer uygulamalar tarafından kullanılabilmeleri sağlanır.

## <a name="key-components"></a>Başlıca bileşenler
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory başlıca dört bileşenden oluşur ve bu bileşenler birlikte çalışarak veri akışınız için basit veya karmaşık veri hareketi veya dönüştürme düzenlemeleri oluşturabileceğiniz platformu sağlar.

### <a name="activity"></a>Etkinlik
Etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlayın. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için Kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri.

Her etkinliğin sıfır veya sıfırdan çok giriş veri kümesi olabilir ve her etkinlik bir veya birden çok çıkış veri kümesi oluşturabilir. 


### <a name="data-movement-activities"></a>Veri taşıma etkinlikleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

### <a name="data-transformation-activities"></a>Veri dönüştürme etkinlikleri
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız ya da kendi mantığınızı kullanarak verileri dönüştürmeniz gerekirse **özel bir .NET etkinliği** oluşturun. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

### <a name="pipeline"></a>İşlem hattı
İşlem hattı bir grup etkinliktir. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı Azure blobundan verileri alan ve ardından HDInsight kümesinde Hive sorgusu çalıştırarak günlük verilerini bölümlere ayıran bir grup etkinlik içerebilir. İşlem hattının avantajı, etkinliklerin her birini tek tek yönetmek yerine bir küme olarak yönetmenize olanak tanımasıdır. Örneğin, etkinlikleri bağımsız olarak dağıtmak ve zamanlamak yerine işlem hattını dağıtabilir ve zamanlayabilirsiniz.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. Örneğin Azure Blob veri kümesi, işlem hattının verileri okuması gereken blob kapsayıcısını ve Azure Blob Depolama klasörünü belirtir. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün: veri kümesi verilerin yapısını temsil ederken, bağlı hizmet de veri kaynağına yönelik bağlantıyı tanımlar.  Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

* Bir **veri deposunu**, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server, Oracle veritabanı, dosya paylaşımı veya bir Azure Blob Depolama hesabı. Desteklenen veri depolarının bir listesi için [Veri taşıma etkinlikleri](#data-movement-activities) bölümüne bakın.
* Etkinlik yürütülmesini barındırabilen **işlem kaynağını** temsil etmek için. Örneğin, HDInsightHive etkinliği bir HDInsight Hadoop kümesinde yürütülür. Desteklenen işlem ortamlarının listesi için [Veri dönüştürme etkinlikleri](#data-transformation-activities) bölümüne bakın.

### <a name="relationship-between-data-factory-entities"></a>Data Factory varlıkları arasındaki ilişki
![Diyagram: Bir bulut veri tümleştirme hizmeti olan Data Factory ile ilgili Temel Kavramlar](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Figure2.** Veri kümesi, Etkinlik, İşlem Hattı ve Bağlı hizmet arasındaki ilişkiler

## <a name="supported-regions"></a>Desteklenen bölgeler
Şu anda **Batı ABD**, **Doğu ABD** ve **Kuzey Avrupa** bölgelerinde veri fabrikaları oluşturabilirsiniz. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir.

Azure Data Factory’nin kendisi verileri depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde akışlar oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar.

Data Factory yalnızca **Batı ABD**, **Doğu ABD** **Kuzey Avrupa** bölgelerinde kullanılabilir olsa da, Data Factory’de veri taşımayı destekleyen hizmet birçok bölgede [küresel olarak](data-factory-data-movement-activities.md#global) kullanılabilmektedir. Veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamınızda yüklü bir [Veri Yönetimi Ağ Geçidi](data-factory-move-data-between-onprem-and-cloud.md) bunun yerine verileri taşır.

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgesinde çalıştığını varsayalım. Kuzey Avrupa’da bir Azure Data Factory örneği oluşturup geliştirebilir ve bunu Batı Avrupa’daki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory’nin işlem ortamınızda işi tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin çalıştırılma süresi değişmez.

Gelecekte Azure tarafından desteklenen Azure Data Factory’nin her coğrafi bölge için kullanılır olmasını planlamaktayız.

## <a name="next-steps"></a>Sonraki adımlar
Veri işlem hatları ile veri fabrikaları oluşturmayı öğrenmek için aşağıdaki öğreticilerde yer alan adım adım yönergeleri izleyin:

| Öğretici | Açıklama |
| --- | --- |
| [İki bulut veri deposu arasında veri taşıma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |Bu öğreticide, Blob depolama biriminden SQL veritabanına **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız. |
| [Hadoop kümesi kullanarak veri dönüştürme](data-factory-build-your-first-pipeline.md) |Bu öğreticide, bir Azure HDInsight (Hadoop) kümesinde Hive betiği çalıştırarak **veri işleyen** bir veri işlem hattı ile ilk Azure veri fabrikanızı oluşturacaksınız. |
| [Veri Yönetimi Ağ Geçidi'ni kullanarak verileri şirket içi veri deposu ile bulut veri deposu arasında taşıma](data-factory-move-data-between-onprem-and-cloud.md) |Bu öğreticide, **şirket içi** SQL Server veritabanından Azure blob’a **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız. |

