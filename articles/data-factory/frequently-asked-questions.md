---
title: 'Azure Data Factory: Sık sorulan sorular | Microsoft Docs'
description: Azure Data Factory hakkında sık sorulan soruların yanıtlarını alın.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: ebe8745db06113d0508d86554bf031a4235c8e44
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045958"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory hakkında SSS
Bu makalede Azure Data Factory hakkında sık sorulan soruların yanıtlarını sağlar.  

## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir? 
Veri Fabrikası taşınmasını ve dönüştürülmesini veri otomatikleştiren bir tam olarak yönetilen, bulut tabanlı veri tümleştirme hizmetidir. Hammaddeleri tamamlanmış ürünlere dönüştürmek için donanım çalıştıran bir fabrikası gibi Azure Data Factory ham verileri toplayan ve kullanıma hazır bilgilere dönüştürür var olan hizmetleri düzenler. 

Azure Data Factory kullanarak, şirket içi ve bulut arasında veri taşımak için veri tabanlı iş akışları oluşturabilirsiniz verileri depolar. İşleyebilmesi için ve Azure Hdınsight, Azure Data Lake Analytics ve SQL Server Integration Services (SSIS) Tümleştirmesi çalışma zamanı gibi hizmetleri kullanarak dönüştürme veri işlem. 

Data Factory ile veri işleme Azure tabanlı bulut hizmeti veya SSIS, SQL Server ve Oracle gibi kendi kendini barındıran işlem ortamınızda yürütebilir. Gereksinim duyduğunuz bir eylem gerçekleştiren bir işlem hattını oluşturduktan sonra onu Çalıştır düzenli aralıklarla (örneğin, saatlik, günlük veya haftalık), zaman penceresi zamanlama veya tetikleyici ardışık bir olay örnekten zamanlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md).

### <a name="control-flows-and-scale"></a>Denetim akışı ve ölçeklendirme 
Modern veri ambarında desenleri ve farklı tümleştirme akışları desteklemek için veri fabrikası tam denetim akışı koşullu yürütme dahil olmak üzere örneklerinde programlama içeren esnek veri ardışık düzen modelleme veri ardışık düzenlerinde dallanma sağlar ve açıkça parametreleri içinde ve bu akışlar arasında geçirin. Denetim akışı etkinliği gönderme dış yürütme altyapılarını ve veri taşıma kopyalama etkinliği aracılığıyla ölçekte da dahil olmak üzere veri akışı özellikleri üzerinden veri dönüştürme de kapsar.

Veri fabrikası, veri tümleştirmesi için gerekli ve isteğe bağlı veya sürekli olarak bir zamanlamaya göre dağıtılan herhangi bir akış stil model özgürlüğü sağlar. Bu model sağlayan birkaç ortak akışları şunlardır:   

- Denetim akışı:
    - Bir işlem hattı içinde sırayla zinciri etkinlikler.
    - Bir işlem hattı'nda şube etkinlikler.
    - Parametreler
        - Ardışık düzen düzeyinde parametrelerini tanımlayın ve isteğe bağlı veya bir tetikleyiciden ardışık düzen çağırma sırasında bağımsız değişkenleri geçirin.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçirme
        - Durum da dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından kullanılabilir.
    - Döngü kapsayıcıları
        - For-each 
- Tetikleyici tabanlı akışları:
    - Ardışık düzen, isteğe bağlı veya duvar saati süresi tetiklenebilir.
- Delta akışları:
    - Parametreleri kullanın ve ilişkisel deposundan, hem şirket içinde veya bulutta lake verileri yüklemek için boyut veya başvuru tabloları taşırken, yüksek su işareti delta kopya tanımlayın. 

Daha fazla bilgi için bkz: [Öğreticisi: denetim akışı](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Verilerinizi ölçekli kodu boş işlem hatları ile dönüştürme
Yeni araç tarayıcı tabanlı deneyimi Kodsuz ardışık düzen geliştirme ve dağıtım modern, etkileşimli web tabanlı bir deneyim sağlar.

Görsel veri geliştiricileri ve veri mühendisleri için ADF Web kullanıcı arabirimini ardışık düzen oluşturmak için kullanacağı Kodsuz tasarım ortamıdır. Visual Studio Online Git ile tamamen tümleşiktir ve tümleştirme CI/CD ve hata ayıklama seçenekleri ile yinelemeli geliştirme sağlar.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Platform SDK'ları İleri düzey kullanıcılar için zengin arası
İleri düzey bir kullanıcı olduğunuz ve programa dayalı bir arabirim için baktığınızda, ADF V2 yazar, yönetmek için işlevden SDK'ları zengin bir dizi sağlar, sık kullanılan IDE'yi kullanılarak işlem hatlarını izleme
1.  Python SDK'sı
2.  PowerShell CLI
3.  C# SDK kullanıcı arabirimine belgelenen REST API'leri ADF V2 ile da kullanabilirsiniz

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Yinelemeli geliştirme ve visual araçlarını kullanarak hata ayıklama
Azure veri fabrikası (ADF) visual araçları yinelemeli geliştirme ve hata ayıklama yapmanıza olanak sağlar. Test çalışmasını tek satırlık bir kod yazmak zorunda kalmadan ardışık düzen tuvalde hata ayıklama özelliği kullanarak ve işlem hatlarınızı oluşturabilirsiniz. Ardışık Düzen tuvale çıktı penceresinde test çalışmalarınız sonuçlarını görüntüleyebilirsiniz. Test çalışması başarılı olduktan sonra daha fazla etkinlikleri, ardışık düzenine eklemek ve yinelemeli bir şekilde hata ayıklama devam edebilirsiniz. Devam eden bir kez test çalışmalarınız iptal edebilirsiniz. Hata ayıklama tıklatmadan önce veri fabrikası hizmetine Değişikliklerinizi yayımlamak için gerekli değildir. Bu, yeni eklemeleri ve değişiklikleri çalışma, geliştirme, veri fabrikası akışlarında güncelleştirmeden önce beklendiği gibi test, veya üretim ortamları emin olun istediğiniz senaryolarda faydalıdır. 

### <a name="deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma 
SSIS iş yüklerinizi taşımak istiyorsanız, bir veri fabrikası oluşturun ve bir Azure SSIS tümleştirmesi çalışma zamanı sağlayın. Azure SSIS tümleştirmesi çalışma zamanı Azure SSIS paketleri bulutta çalıştırmak için adanmış VM'nin (düğümler) tam olarak yönetilen bir kümedir. Adım adım yönergeler için bkz: [azure'a dağıtma SSIS paketleri](tutorial-create-azure-ssis-runtime-portal.md) Öğreticisi. 
 
### <a name="sdks"></a>SDK’lar
İleri düzey bir kullanıcı varsa ve bu programa dayalı bir arabirim için baktığınızda, ADF zengin bir SDK'ları sağlayan yazar, yönetme veya sık kullanılan IDE kullanılarak işlem hatlarını izlemek için kullanabilirsiniz. .NET, PowerShell, Python ve REST dil desteği içerir.

### <a name="monitoring"></a>İzleme
PowerShell, SDK veya tarayıcı kullanıcı arabirimi Visual izleme araçları aracılığıyla, veri fabrikaları izleyebilirsiniz. İzleme ve isteğe bağlı, dayalı tetikleyici ve verimli ve etkili bir şekilde özel akışları güdümlü saati yönetme. İptal varolan görevleri, bkz: hataları bir bakışta ayrıntılı hata iletileri almak ve geçiş veya geri ve İleri ekranları arasında gezinme bağlamı olmadan tüm bölmesinden bir tek cam hatalarını ayıklamanıza için detaya. 

### <a name="new-features-for-ssis-in-adf"></a>ADF SSIS için yeni özellikler
İlk genel Önizleme sürümü bu yana 2017, veri fabrikası SSIS için aşağıdaki özellikleri eklemiştir:

-   Üç için daha fazla yapılandırmaları/çeşitleri konak SSIS katalog projeleri/paketlerin (SSISDB) için Azure SQL veritabanı (DB), destekler:
-   Azure SQL DB VNet hizmet uç noktaları
-   Yönetilen örneği (mı)
-   Elastik Havuz
-   Azure Resource Manager, sanal gelecekte – kullanım dışı kalacaktır Klasik VNet üzerinde ağ (VNet) için destek bu olanak tanır, Azure SSIS tümleştirmesi çalışma zamanı (IR) için Azure SQL DB VNet hizmet uç noktaları/mı ile yapılandırılmış bir sanal ağa ekleme/katılma / Şirket içi veri erişimi bakın: https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network 
-   Destek, SSISDB için - bağlanmak için SQL kimlik doğrulaması üzerinde Azure Active Directory (AAD) kimlik doğrulaması için bu, AAD kimlik bilgilerinizi ADF yönetilen hizmet kimliği (MSI) ile kullanmanıza olanak sağlar
-   Azure karma Avantajı (AHB) seçeneğinden önemli maliyet tasarrufları kazanmak için kendi şirket içi SQL Server lisansınızı hale getirme için destek
-   Enterprise Edition, Azure SSIS olanak sağlayan IR için destek, Gelişmiş/premium özellikleri, ek bileşenleri/uzantılarını ve 3 taraf ekosistemi yükleyin, görmek için özel kurulum kullanın: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/ 
-   İlk sınıf SSIS paketi yürütme etkinlikleri ADF ardışık düzende çağırma/tetikleyici ve SSMS zamanlama sağlayan ADF SSIS, daha derin tümleştirme bakın: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>Tümleştirme çalışma zamanı nedir?
Tümleştirme çalışma zamanı tarafından Azure Data Factory çeşitli ağ ortamlar genelinde aşağıdaki veri tümleştirme özellikleri sağlamak için kullanılan bilgi işlem altyapısı şöyledir:

- **Veri taşıma**: veri taşıma için tümleştirme çalışma zamanı yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme ve kullanıcı ve ölçeklenebilir veri aktarımı için destek sağlayarak kaynak ve hedef veri depoları arasında verileri taşır.
- **Gönderme etkinlikleri**: Dönüşüm için tümleştirme çalışma zamanı sağlamak SSIS paketleri yerel olarak yürütülecek yeteneği.
- **SSIS paketleri yürütme**: yerel bir yönetilen Azure işlem ortamında SSIS paketleri yürütür. Integration Runtime ayrıca Azure HDInsight, Azure Machine Learning, Azure SQL Veritabanı, SQL Server ve diğer bazı işlem hizmetlerinde çalıştırılan dönüştürme etkinliklerinin dağıtılması ve izlenmesini de destekler.

Bir veya daha çok örnekleri Integration zamanının verileri taşımak ve dönüştürmek için gerektiği şekilde dağıtabilirsiniz. Tümleştirme çalışma zamanı, Azure ortak ağ veya özel bir ağda (şirket içi, Azure sanal ağ veya Amazon Web Hizmetleri sanal özel bulut [VPC]) çalıştırabilirsiniz. 

Daha fazla bilgi için bkz. [Azure Data Factory'de tümleştirme çalışma zamanı](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Tümleştirme çalışma zamanları sayısı sınırı nedir?
Veri fabrikasında olabilir tümleştirmesi çalışma zamanı örnekleri sayısının sabit sınırı yoktur. Ancak, tümleştirme çalışma abonelik başına SSIS paketi yürütme için kullanabileceği VM çekirdek sayısına bir sınır yoktur. Daha fazla bilgi için bkz: [Data Factory sınırlar](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Azure Data Factory en üst düzey kavramlarını nelerdir?
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory veri temelli iş akışlarını taşıyabilir ve veri dönüştürme için gerekli adımları içeren oluşturan bir platform olarak birlikte çalışan dört anahtar bileşenleri içerir.

### <a name="pipelines"></a>İşlem hatları
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. Ardışık etkinlikler bir iş birimine gerçekleştirmek için mantıksal bir gruplandırmasıdır. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı verileri Azure blob alma ve ardından verileri bölümlemek için bir Hdınsight kümesinde bir Hive sorgusu çalıştırmak etkinliklerin bir grubu içerebilir. Her etkinlik tek tek yönetmek zorunda kalmak yerine bir küme olarak etkinliklerini yönetmek için bir işlem hattı kullanabilirsiniz avantajdır. Sıralı olarak çalışması için bir ardışık düzende etkinlik zincir veya bağımsız olarak, paralel olarak işleyebilir.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, kullanabileceğiniz bir *kopyalama* veri bir veri deposundan başka bir veri deposuna kopyalamak için kopyalama etkinliği. Benzer şekilde, dönüştürme ya da verilerinizi çözümlemek için bir Azure Hdınsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliğini kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu bu şekilde düşünün: bağlı hizmet, veri kaynağına bağlantıyı tanımlar ve bir veri kümesi verilerin yapısını temsil eder. Örneğin, bir Azure Storage bağlı hizmeti Azure Storage hesabınıza bağlanmak için bağlantı dizesini belirtir. Ve bir Azure Blob veri kümesi blob kapsayıcısında ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory'de iki amaca sahiptir:

- Temsil etmek için bir *veri deposu* , içerir, ancak bir şirket içi SQL Server örneği, Oracle veritabanı örneği, bir dosya paylaşımı veya bir Azure Blob Depolama hesabı için sınırlı değildir. Desteklenen veri depoları listesi için bkz: [kopya etkinliği Azure Data factory'de](copy-activity-overview.md).
- Etkinlik yürütülmesini barındırabilen *işlem kaynağını* temsil etmek için. Örneğin, Hdınsight Hive etkinliği bir Hdınsight Hadoop kümesinde çalışır. Dönüştürme etkinlikleri ve desteklenen bilgi işlem ortamlarını listesi için bkz: [dönüştürme Azure Data Factory veri](transform-data.md).

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler ardışık düzen yürütmesi zaman koparılan belirlemek işleme birimleri temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır. 

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
Çalıştıran bir ardışık düzen ardışık düzen yürütmesi örneğidir. Ardışık düzeninde tanımlanan parametrelere bağımsız değişkenleri geçirme çalıştırmak bir ardışık düzen genellikle örneği oluşturur. El ile veya tetikleyici tanımındaki bağımsız değişkenler geçirebilirsiniz.

### <a name="parameters"></a>Parametreler
Anahtar-değer çiftlerinin bir salt okunur yapılandırmasında parametreleridir. Ardışık düzeninde parametrelerini tanımlayın ve yürütme sırasında bir çalışma bağlamında tanımlanan parametrelerin bağımsız değişkenleri geçirin. Çalışma bağlamı, tetikleyici veya el ile yürütme ardışık oluşturulur. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Bir veri türü kesin belirlenmiş bir parametre ve yeniden veya başvuru bir varlığın kümesidir. Veri kümesi bir etkinlik başvurusu yapabilir ve veri kümesi tanımında tanımlanan özellikler kullanmasını sağlayabilirsiniz.

Bağlı hizmet ayrıca bir veri deposu ya da bir bilgi işlem ortamı bağlantı bilgilerini içeren bir kesin türü belirtilmiş parametredir. Bu aynı zamanda yeniden kullanabilir veya başvuru bir varlıktır.

### <a name="control-flows"></a>Denetim akışı
Denetim akışı etkinlikleri sırayla dallanma, ardışık düzen düzeyinde tanımlayan parametreleri zincirleme dahil ardışık etkinlikleri düzenlemek ve geçirdiğiniz bağımsız değişkenler isteğe bağlı veya bir tetikleyiciden ardışık düzen çağırma. Denetim akışı özel durumu geçirme ve kapsayıcıları (diğer bir deyişle, için-her yineleyiciler) döngü da kapsar.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümesi ve bağlantılı Hizmetleri](concepts-datasets-linked-services.md)
- [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Veri Fabrikası için fiyatlandırma modeli nedir?
Fiyatlandırma ayrıntıları olan Azure Data Factory için bkz: [Data Factory fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Nasıl ı Data Factory hakkında bilgi içeren güncel kalabileceği?
Azure Data Factory hakkında en güncel bilgiler için aşağıdaki sitelere bakın:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Belge giriş sayfası](/azure/data-factory)
- [Ürün giriş sayfası](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Teknik derinlemesine bakış 

### <a name="how-can-i-schedule-a-pipeline"></a>Bir işlem hattı nasıl zamanlayabilir miyim? 
Bir işlem hattı zamanlamak için Zamanlayıcı tetikleyicisi veya zaman penceresi tetikleyici kullanabilirsiniz. Tetikleyici duvar saati takvim zamanlama kullanır ve düzenli aralıklarla veya (örneğin, haftalık 6 PM adresindeki Pazartesi ve Salı günleri 9 PM adresindeki üzerinde) takvim tabanlı yinelenen desenler kullanılarak işlem hatlarını zamanlamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Parametreleri bir ardışık düzen Çalıştır olarak geçiş yapabilir?
Evet, birinci sınıf, üst düzey bir kavram ADF olarak parametreleridir. Ardışık düzen düzeyinde parametrelerini tanımlayın ve bağımsız değişkenler isteğe bağlı veya bir tetikleyici kullanarak çalıştırmak ardışık düzen yürütmek olarak geçirin.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Ardışık Düzen parametrelerinin varsayılan değerleri tanımlayın? 
Evet. Ardışık düzenlerinde parametrelerinin varsayılan değerleri tanımlayabilirsiniz. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Ardışık düzeninde bir aktivite çalıştırmak bir ardışık düzene geçirilen bağımsız değişkenlerini tüketebileceği? 
Evet. Ardışık Düzen içindeki her etkinliği ardışık düzene geçirilen ve çalıştırmalarına parametre değeri tüketebileceği `@parameter` oluşturun. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Bir etkinlik çıkış özelliği başka bir etkinliğin tüketilebilir? 
Evet. Bir etkinlik çıkışı ile sonraki bir aktivite içinde kullanılabilecek `@activity` oluşturun.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Nasıl bir etkinlik çıkışı null değerleri kolaylıkla idare? 
Kullanabileceğiniz `@coalesce` null değerler kolaylıkla idare edecek ifadelerinde oluşturun. 

## <a name="next-steps"></a>Sonraki adımlar
Data factory oluşturmak adım adım yönergeler için aşağıdaki öğreticiler bakın:

- [Hızlı Başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-dot-net.md)
- [Öğretici: verileri bulutta kopyalayın.](tutorial-copy-data-dot-net.md)
