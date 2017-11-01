---
title: "Azure Data Factory’ye Giriş | Microsoft Docs"
description: "Verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmeti olan Azure Data Factory hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2017
ms.author: shlo
ms.openlocfilehash: 9ed89261b7050bb41d49b827e02d24535983160f
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="introduction-to-azure-data-factory"></a>Azure Data Factory'ye giriş 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-introduction.md)
> * [Sürüm 2 - Önizleme](introduction.md)

Büyük veri dünyasında ham ve düzensiz veriler genellikle ilişkisel, ilişkisel olmayan ve diğer depolama sistemlerinde depolanır. Ancak, ham veriler kendi başlarına analiz uzmanlarına, veri bilimcilerine veya iş karar mekanizmalarına anlamlı bilgiler sağlamak için uygun bağlama veya anlama sahip değildir. 

Büyük veri için gerekli olan, muazzam büyüklükteki bu ham veri depolarını eyleme dönüştürülebilen iş öngörüleri haline getirmek için düzenleyici ve faaliyete geçiren süreçler sağlayan bir hizmettir. Azure Data Factory, bu karmaşık karma ayıkla-dönüştür-yükle (ETL), ayıkla-yükle-dönüştür (ELT) ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir.

Örneğin, buluttaki oyunlar tarafından üretilmiş petabaytlarca oyun günlüğünü toplayan bir oyun şirketi düşünün. Şirket, bu günlükleri analiz ederek müşteri tercihleri, demografik verileri ve kullanım davranışı hakkında bilgi sahibi olmak istemektedir. Ayrıca yukarı satış ve çapraz satış fırsatlarını belirlemek, yeni cazip özellikler geliştirmek, işleri büyütmek ve müşterilerine daha iyi bir deneyim sunmayı amaçlamaktadır.

Bu günlükleri analiz etmek için, şirketin şirket içi veri deposunda bulunan müşteri bilgileri, oyun bilgileri ve pazarlama kampanyası bilgileri gibi başvuru verilerini kullanması gerekir. Şirket bu verileri şirket içi veri deposundan bir bulut veri deposunda sahip olduğu ek günlük verileriyle bir arada kullanmak istemektedir. 

Öngörüler elde etmek için, bulutta (Azure HDInsight) bir Spark kümesi kullanarak birleştirilmiş verileri işlemeyi ve bunlara ilişkin bir raporu kolayca oluşturmak üzere dönüştürülmüş verileri Azure SQL Veri Ambarı gibi bir bulut veri ambarında yayımlamayı planlamaktadır. Şirket bu iş akışını otomatik hale getirmeyi ve günlük düzende izleyip yönetmeyi istemektedir. Ayrıca bu iş akışının blob depolama kapsayıcısına dosya geldiğinde yürütülmesini istemektedir.

Azure Data Factory, bu tür veri senaryolarını çözen platformdur. *Bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir*. Azure Data Factory platformunu kullanarak farklı veri depolarından veri alabilen veri odaklı iş akışları (işlem hattı olarak adlandırılır) oluşturabilir ve zamanlayabilirsiniz. Bu platform Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyip dönüştürebilir. 

Ayrıca çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz. Sonuç olarak, Azure Data Factory sayesinde ham veriler daha iyi iş kararları için anlamlı veri depoları ve veri gölleri halinde düzenlenebilir.

![Data Factory'nin üstten görünümü](media/introduction/big-picture.png)

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1'e giriş](v1/data-factory-introduction.md).

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
Veri tümleştirme işlem hattınızı başarıyla oluşturup dağıtarak iyileştirilmiş verilerden iş değeri elde ettikten sonra, başarı ve hata oranları için zamanlanmış etkinlikleri ve işlem hatlarını izleyin. Azure Data Factory; Azure İzleyici, API, PowerShell, Microsoft Operations Management Suite ve Azure portalındaki sistem durumu panelleri aracılığıyla işlem hattı izlemek için yerleşik destek sunmaktadır.

## <a name="whats-different-in-version-2"></a>Sürüm 2’nin farkları nelerdir?
Azure Data Factory sürüm 2, özgün Azure Data Factory veri taşıma ve dönüştürme hizmetinin üzerine kurulmuştur ve daha fazla bulut öncelikli veri tümleştirme senaryosunu kapsar. Azure Data Factory sürüm 2 aşağıdaki özellikleri sunar:

- Denetim akışı ve ölçeklendirme
- Azure'da SQL Server Integration Services (SSIS) paketlerini dağıtma ve çalıştırma

Sürüm 1 yayınından sonra müşterilerin veri taşıma ve bulutta, şirket içinde ve bulut VM'lerde işleme gerektiren karmaşık karma veri tümleştirme senaryoları tasarlamaya ihtiyaç duyduğunu fark ettik. Bu gereksinimler, verileri güvenli sanal ağ ortamlarında aktarma ve işlemenin yanı sıra talep üzerine işleme gücü ile ölçek artırma gereksinimini doğurdu.

Veri işlem hatları bir iş analizi stratejisinin kritik bir parçası haline geldiğinden, bu kritik veri etkinliklerinin artımlı veri yüklerini ve olayla tetiklenen yürütmeleri desteklemek için esnek zamanlama gerektirdiğini gördük. Son olarak, bu işlemlerin karmaşıklıkları arttıkça, hizmetin dallanma, döngü ve koşullu işleme gibi yaygın iş akışı paradigmalarını destekleme gereksinimi artmıştır.

Sürüm 2 ile var olan SSIS paketlerini de buluta geçirebilirsiniz. Yeni "Integration Runtime" (IR) özelliğini kullanarak SSIS hizmetini ADF içinde yönetilen bir Azure hizmeti haline getirebilirsiniz. Sürüm 2’de bir SSIS IR’yi çalıştırarak, SSIS paketlerini bulutta yürütebilir, yönetebilir, izleyebilir ve oluşturabilirsiniz.

### <a name="control-flow-and-scale"></a>Denetim akışı ve ölçeklendirme 
Data Factory, modern veri ambarında çeşitli tümleştirme akışlarını ve desenlerini desteklemek için artık zaman dizisi verilerine bağlı olmayan yeni bir esnek veri işlem hattı modelini etkinleştirmiştir. Bu sürümle birlikte, bir veri işlem hattının denetim akışında koşulları ve dallanmayı modelleyebilir ve bu akışların içinde ve arasında parametreleri açıkça geçirebilirsiniz.

Artık veri tümleştirme için gereken ve talep üzerine ya da bir saat zamanlamasına göre tekrarlanarak gönderilen herhangi bir akış stilini modelleme özgürlüğüne sahipsiniz. Daha önce mümkün olmayıp şu anda etkin olan birkaç yaygın akış şunlardır:   

- Denetim akışı:
    - Bir işlem hattı içindeki sıralı zincirleme etkinlikleri
    - Bir işlem hattı içindeki etkinliklerin dallanması
    - Parametreler
        - Parametreler işlem hattı düzeyinde tanımlanabilir ve işlem hattı talep üzerine ya da bir tetikleyici ile çağrılırken bağımsız değişkenler geçirilebilir.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçirme
        - Durum da dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından kullanılabilir.
    - Döngü kapsayıcıları
        - For-each 
- Tetikleyici temelli akışlar
    - İşlem hatları talep üzerine veya duvar saati zamanlamasına göre tetiklenebilir.
- Delta akışlar
    - Verileri göle yüklemek için şirket içinde veya bulutta bulunan bir ilişkisel depodan boyut ya da başvuru tablolarını taşırken delta kopya için parametreleri kullanın ve yüksek filigranınızı tanımlayın. 

Daha fazla bilgi için bkz. [Data Factory işlem hattında dallanma ve zincirleme etkinlikleri](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma 
SSIS iş yüklerinizi taşımak istiyorsanız, data factory sürüm 2 oluşturabilir ve bir Azure SSIS Tümleştirmesi Çalışma Zamanı (IR) sağlayabilirsiniz. Azure-SSIS IR, bulutta SSIS paketlerinizi çalıştırmaya ayrılmış Azure VM'lerin (düğümler) tam yönetilen bir kümesidir. Adım adım yönergeler için bkz. [SQL Server Integration Services paketlerini Azure'a dağıtma](tutorial-deploy-ssis-packages-azure.md). 
 

### <a name="sdks"></a>SDK’lar
İleri düzey bir kullanıcıysanız ve bir programlama arabirimi arıyorsanız, sürüm 2 sık kullandığınız IDE'yi kullanarak işlem hattı oluşturmak, yönetmek ve izlemek için kullanılabilen zengin bir SDK kümesi sağlar.

- *.NET SDK*: .NET SDK, sürüm 2 için güncelleştirilmiştir. 
- *PowerShell*: PowerShell cmdlet'leri sürüm 2 için güncelleştirilmiştir. Sürüm 2 cmdlet'lerinin adında **DataFactoryV2** vardır. Örneğin: Get-AzureRmDataFactoryV2. 
- *Python SDK*: Bu SDK sürüm 2 içindir.
- *REST API*: REST API, sürüm 2 için güncelleştirilmiştir.  

Sürüm 2 için güncelleştirilmiş olan SDK'lar sürüm 1 istemcileriyle uyumlu değildir. 

### <a name="monitoring"></a>İzleme
Şu anda sürüm 2 veri fabrikalarının yalnızca SDK ile izlenmesini desteklemektedir. Portal henüz sürüm 2 veri fabrikalarını izleme desteği sunmamaktadır. 

## <a name="load-the-data-into-a-lake"></a>Verileri göle yükleme
Data Factory, verileri karma ve heterojen ortamlardan Azure'a yüklemenizi sağlayan 30'dan fazla bağlayıcıya sahiptir. Dahili testlerin en son performans sonuçları ve ayarlama önerileri için bkz. [Performans ve ayar kılavuzu](copy-activity-performance.md). 

Ayrıca kısa süre önce özel bir ağ ortamına yüklediğiniz şirket içinde barındırılan Tümleştirme Çalışma Zamanı için yüksek kullanılabilirlik ve ölçeklenebilirlik özelliklerini etkinleştirdik. Bu daha gelişmiş kullanılabilirlik ve ölçeklenebilirlik özellikleri için geniş kapsamlı 1. katman kurumsal müşteri gereksinimlerini hedeflemektedir.

## <a name="top-level-concepts-in-version-2"></a>Sürüm 2’deki üst düzey kavramlar
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
Tetikleyiciler, bir işlem hattı çalıştırmasının başlatılması gereken zamanı belirleyen işlem birimini temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır. Önizleme için duvar saati zamanlayıcı tetikleyicisini destekliyoruz. 


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

Şu anda Doğu ABD ve Doğu ABD 2 bölgelerinde veri fabrikaları oluşturabilirsiniz. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir.

Azure Data Factory’nin kendisi verileri depolamaz. Veri taşımayı desteklenen veri depoları arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda işlem hizmetleri kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak iş akışlarını izlemenizi ve yönetmenizi de sağlar.

Data Factory yalnızca Doğu ABD ve Doğu ABD 2 bölgelerinde kullanılabilir olsa da, Data Factory'de veri taşımayı destekleyen hizmet birçok bölgede küresel olarak kullanılabilmektedir. Veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamınızda yüklü bir Veri Yönetimi Ağ Geçidi bunun yerine verileri taşır.

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgesinde çalıştığını varsayalım. Kuzey Avrupa’da bir Azure Data Factory örneği oluşturup geliştirebilir ve bunu Batı Avrupa’daki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory'nin işlem ortamınızda işi tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin çalıştırılma süresi değişmez.

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikası oluşturma hakkında bilgi edinmek için şu Hızlı Başlangıçlarda verilen adım adım yönergeleri izleyin: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST API](quickstart-create-data-factory-rest-api.md) ve Azure portalı. 
