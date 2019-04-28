---
title: Azure Data Factory’ye Giriş | Microsoft Docs
description: Verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmeti olan Azure Data Factory hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: overview
ms.date: 01/11/2018
ms.author: shlo
ms.openlocfilehash: 66ea269e2f29bfd39cdb81086391e0277474219d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61456385"
---
# <a name="introduction-to-azure-data-factory"></a>Azure Data Factory'ye giriş 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-introduction.md)
> * [Geçerli sürüm](introduction.md)

Büyük veri dünyasında ham ve düzensiz veriler genellikle ilişkisel, ilişkisel olmayan ve diğer depolama sistemlerinde depolanır. Ancak, ham veriler kendi başlarına analiz uzmanlarına, veri bilimcilerine veya iş karar mekanizmalarına anlamlı bilgiler sağlamak için uygun bağlama veya anlama sahip değildir. 

Büyük veri için gerekli olan, muazzam büyüklükteki bu ham veri depolarını eyleme dönüştürülebilen iş öngörüleri haline getirmek için düzenleyici ve faaliyete geçiren süreçler sağlayan bir hizmettir. Azure Data Factory, bu karmaşık karma ayıkla-dönüştür-yükle (ETL), ayıkla-yükle-dönüştür (ELT) ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir.

Örneğin, buluttaki oyunlar tarafından üretilmiş petabaytlarca oyun günlüğünü toplayan bir oyun şirketi düşünün. Şirket, bu günlükleri analiz ederek müşteri tercihleri, demografik verileri ve kullanım davranışı hakkında bilgi sahibi olmak istemektedir. Ayrıca yukarı satış ve çapraz satış fırsatlarını belirlemek, yeni cazip özellikler geliştirmek, işleri büyütmek ve müşterilerine daha iyi bir deneyim sunmayı amaçlamaktadır.

Bu günlükleri analiz etmek için, şirketin şirket içi veri deposunda bulunan müşteri bilgileri, oyun bilgileri ve pazarlama kampanyası bilgileri gibi başvuru verilerini kullanması gerekir. Şirket bu verileri şirket içi veri deposundan bir bulut veri deposunda sahip olduğu ek günlük verileriyle bir arada kullanmak istemektedir. 

Öngörüler elde etmek için, bulutta (Azure HDInsight) bir Spark kümesi kullanarak birleştirilmiş verileri işlemeyi ve bunlara ilişkin bir raporu kolayca oluşturmak üzere dönüştürülmüş verileri Azure SQL Veri Ambarı gibi bir bulut veri ambarında yayımlamayı planlamaktadır. Şirket bu iş akışını otomatik hale getirmeyi ve günlük düzende izleyip yönetmeyi istemektedir. Ayrıca bu iş akışının blob depolama kapsayıcısına dosya geldiğinde yürütülmesini istemektedir.

Azure Data Factory, bu tür veri senaryolarını çözen platformdur. *Bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir*. Azure Data Factory platformunu kullanarak farklı veri depolarından veri alabilen veri odaklı iş akışları (işlem hattı olarak adlandırılır) oluşturabilir ve zamanlayabilirsiniz. Bu platform Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyip dönüştürebilir. 

Ayrıca çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz. Sonuç olarak, Azure Data Factory sayesinde ham veriler daha iyi iş kararları için anlamlı veri depoları ve veri gölleri halinde düzenlenebilir.

![Data Factory'nin üstten görünümü](media/introduction/big-picture.png)

## <a name="how-does-it-work"></a>Nasıl çalışır?
Azure Data Factory’deki işlem hatları (veri odaklı iş akışları) genellikle aşağıdaki dört adımı gerçekleştirir:

![Veri temelli bir iş akışının dört adımı](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Bağlanma ve toplama

Kuruluşlar şirket içinde, bulutta bulunan yapılandırılmış, yapılandırılmamış veya yarı yapılandırılmış ve tümü farklı aralık ve hızlarda gelen farklı kaynaklarda bulunan çeşitli veri türlerine sahiptir. 

Bilgi üretim sistemi oluşturmanın ilk adımı hizmet olarak yazılım (SaaS) hizmetleri, veritabanları, dosya paylaşımları, FTP, web hizmetleri gibi tüm gerekli veri kaynaklarına ve işleme çalışmalarına bağlanmaktır. Sonraki adım ise takip eden işleme çalışmaları için gerektiğinde verileri merkezi bir konuma taşımaktır.

Data Factory olmadığında, kuruluşların bu veri kaynaklarını ve işleme çalışmalarını tümleştirmek için özel veri taşıma bileşenleri oluşturması veya özel hizmetler yazması gerekir. Bu tür sistemleri tümleştirmenin ve bakımını yapmanın maliyeti yüksektir. Buna ek olarak bu sistemlerde tamamen yönetilebilir bir hizmetin sunduğu kurumsal sınıf izleme, uyarı oluşturma ve denetim özellikleri mevcut değildir.

Data Factory ile, veri işlem hattında [Kopyalama Etkinliği](copy-activity-overview.md)’ni kullanarak hem şirket içinde hem de buluttaki kaynak veri depolarını daha fazla analiz için merkezi bir veri deposuna taşıyabilirsiniz. Örneğin, Azure Data Lake Store'da veri toplayabilir ve daha sonra Azure Data Lake Analytics işlem hizmetini kullanarak verileri dönüştürebilirsiniz. Verileri Azure Blob depolama alanından toplayıp daha sonra Azure HDInsight Hadoop kümesi kullanarak da dönüştürebilirsiniz.

### <a name="transform-and-enrich"></a>Dönüştürme ve zenginleştirme
Veriler buluttaki merkezi bir veri deposuna sunulduktan sonra toplanan verileri HDInsight Hadoop, Spark, Data Lake Analytics ve Machine Learning gibi işlem hizmetlerini kullanarak işleyin veya dönüştürün. Üretim ortamlarının güvenilir verilerle beslenmesi için sürdürülebilir ve denetlenebilir bir zamanlamaya göre dönüştürülmüş verileri güvenilir bir şekilde üretmeniz gerekir.

### <a name="publish"></a>Yayımlama
Ham veriler iş için kullanılabilir biçime getirildikten sonra, verileri Azure Veri Ambarı, Azure SQL Veritabanı, Azure CosmosDB'ye veya şirket kullanıcılarınızın iş zekası araçlarından işaret edebildiği herhangi bir analiz altyapısına yükleyebilirsiniz.

### <a name="monitor"></a>İzleme
Veri tümleştirme işlem hattınızı başarıyla oluşturup dağıtarak iyileştirilmiş verilerden iş değeri elde ettikten sonra, başarı ve hata oranları için zamanlanmış etkinlikleri ve işlem hatlarını izleyin. Azure Data Factory, Azure İzleyici, API, PowerShell, Azure İzleyici günlüklerine ve Azure portalındaki sistem durumu panelleri aracılığıyla işlem hattı izlemek için yerleşik destek sunmaktadır.

## <a name="top-level-concepts"></a>Üst düzey kavramlar
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory dört temel bileşenden oluşur. Bu bileşenler, üzerinde veri taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu sağlamak üzere birlikte çalışır.

### <a name="pipeline"></a>İşlem hattı
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir iş birimini gerçekleştirmeye yönelik mantıksal bir etkinlik grubudur. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı Azure blobundan verileri alan ve ardından HDInsight kümesinde Hive sorgusu çalıştırarak verileri bölümlere ayıran bir grup etkinlik içerebilir. 

İşlem hattının avantajı, etkinliklerin her birini tek tek yönetmek yerine bir küme olarak yönetmenize olanak tanımasıdır. Bir işlem hattındaki etkinlikler, sırayla çalışmak üzere birbirine zincirlenebilir veya paralel olarak birbirinden bağımsız çalışabilir.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory'ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün: bağlı bir hizmet, veri kaynağıyla bağlantıyı tanımlar ve veri kümesi verilerin yapısını temsil eder. Örneğin, Azure Depolama bağlı hizmeti Azure Depolama hesabına bağlanacak bağlantı dizesini belirtir. Ayrıca, bir Azure blob veri kümesi blob kapsayıcıyı ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

- Bir **veri deposunu**, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server veritabanı, Oracle veritabanı, dosya paylaşımı veya bir Azure blob depolama hesabı. Desteklenen veri depolarının listesi için [kopyalama etkinliği](copy-activity-overview.md) makalesine bakın.

- Etkinlik yürütülmesini barındırabilen **işlem kaynağını** temsil etmek için. Örneğin, HDInsightHive etkinliği bir HDInsight Hadoop kümesinde yürütülür. Dönüştürme etkinlikleri ve desteklenen işlem ortamlarının listesi için [veri dönüştürme](transform-data.md) makalesine bakın.

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler, bir işlem hattı çalıştırmasının başlatılması gereken zamanı belirleyen işlem birimini temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır.

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
İşlem hattı çalıştırması, işlem hattı yürütme örneğidir. İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. Bağımsız değişkenler el ile veya tetikleyici tanımı içinde geçirilebilir.

### <a name="parameters"></a>Parametreler
Parametreler salt okunur yapılandırmanın anahtar-değer çiftleridir.  Parametreler işlem hattında tanımlanır. Tanımlı parametrelerin bağımsız değişkenleri, bir tetikleyici tarafından oluşturulan çalıştırma bağlamı veya el ile yürütülen işlem hattından yürütme sırasında geçirilir. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Veri kümesi, türü kesin olarak belirtilmiş bir parametre ve yeniden kullanılabilir/başvurulabilir bir varlıktır. Bir etkinlik, veri kümelerine başvurabilir ve veri kümesi tanımında belirtilen özellikleri kullanabilir.

Bağlı hizmet de türü kesin olarak belirtilmiş ve veri deposu ya da işlem ortamı ile bağlantı bilgilerini içeren bir parametredir. Bu da yeniden kullanılabilir/başvurulabilir bir varlıktır.

### <a name="control-flow"></a>Denetim akışı
Denetim akışı, işlem hattı düzeyinde ve işlem hattı talep üzerine ya da bir tetikleyiciden çağrılırken geçirilen bağımsız değişkenlerde tanımlanabilen dizi, dallanma ve parametrelerdeki zincirleme etkinliklerini içeren işlem hattı etkinliklerinin düzenlenmesidir. Ayrıca özel durum geçirme ve döngü kapsayıcılarını, diğer bir deyişle For-each yineleyicilerini içerir.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md)
- [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Desteklenen bölgeler

Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir.

Azure Data Factory’nin kendisi verileri depolamaz. Veri taşımayı desteklenen veri depoları arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda işlem hizmetleri kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak iş akışlarını izlemenizi ve yönetmenizi de sağlar.

Data Factory yalnızca belirli bölgelerde kullanılabilir olsa da, Data Factory'de veri taşımayı destekleyen hizmet birçok bölgede küresel olarak kullanılabilmektedir. Bir veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamınızda yüklü bir Şirket İçinde Barındırılan Integration Runtime bunun yerine verileri taşır.

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgesinde çalıştığını varsayalım. Doğu ABD veya Doğu ABD 2’de bir Azure Data Factory örneği oluşturup geliştirebilir ve bunu Batı Avrupa’daki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory'nin işlem ortamınızda işi tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin çalıştırılma süresi değişmez.

## <a name="accessibility"></a>Erişilebilirlik

Azure Portal'da Data Factory kullanıcı deneyimi erişilebilirdir.

## <a name="compare-with-version-1"></a>Sürüm 1 ile karşılaştırma
Data Factory geçerli sürümü ile 1 numaralı sürümleri arasındaki farkların listesi için bkz. [Sürüm 1 ile karşılaştırma](compare-versions.md). 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki araçlardan/SDK’lardan birini kullanarak Data Factory işlem hattı oluşturmaya başlayın: 

- [Azure portalındaki Data Factory kullanıcı arabirimi](quickstart-create-data-factory-portal.md)
- [Azure portalındaki Veri Kopyalama aracı](quickstart-create-data-factory-copy-data-tool.md)
- [PowerShell](quickstart-create-data-factory-powershell.md)
- [.NET](quickstart-create-data-factory-dot-net.md)
- [Python](quickstart-create-data-factory-python.md)
- [REST](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager şablonu](quickstart-create-data-factory-resource-manager-template.md)
 
