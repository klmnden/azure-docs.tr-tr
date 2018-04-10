---
title: Azure Data Factory'deki tümleştirme çalışma zamanı | Microsoft Docs
description: Azure Data Factory'deki tümleştirme çalışma zamanı hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 97c2f68356a6a589f48224d297493509786ceff1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="integration-runtime-in-azure-data-factory"></a>Azure Data Factory'deki tümleştirme çalışma zamanı
Integration Runtime (IR), Azure Data Factory tarafından farklı ağ ortamlarında aşağıdaki veri tümleştirme özelliklerini sunmak için kullanılan işlem altyapısıdır:

- **Veri taşıma**: Ortak ağdaki veri depoları ile özel ağ (şirket içi veya sanal özel ağ) üzerindeki veri depoları arasında veri taşıyın. Yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme, performanslı ve ölçeklenebilir veri aktarımı desteği sunar.
- **Etkinlik dağıtma**: Azure HDInsight, Azure Machine Learning, Azure SQL Veritabanı, SQL Server ve diğer bazı işlem hizmetlerinde çalıştırılan dönüştürme etkinliklerinin dağıtılması ve izlenmesini de destekler.
- **SSIS paketi yürütme**: SQL Server Integration Services (SSIS) paketlerini yönetilen bir Azure işlem ortamında yerel olarak yürütün.


> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

Data Factory'de etkinlik, gerçekleştirilecek eylemi tanımlar. Bağlı hizmet, bir hedef veri deposunu veya işlem hizmetini tanımlar. Tümleştirme çalışma zamanı, etkinlik ile bağlı Hizmetler arasında köprü görevi görür.  Bağlı hizmet tarafından başvurulur ve etkinliği çalıştığı veya dağıtıldığı işlem ortamını sağlar.  Bu şekilde etkinlik hedef veri deposuna veya işlem hizmetine en yakın bölgeden en yüksek performansla gerçekleştirilirken güvenlik ve uyum gereksinimleri korunmuş olur.

## <a name="integration-runtime-types"></a>Tümleştirme çalışma zamanı türleri
Data Factory, üç farklı Integration Runtime türü sunar ve ihtiyacınız olan veri tümleştirme ve ağ ortamı özelliklerine uygun türü seçmeniz gerekir.  Bu üç tür şunlardır:

- Azure
- Kendinden konak
- Azure-SSIS

Aşağıdaki tabloda tümleştirme çalışma zamanı türlerinin her birinin sunduğu özellikler ve ağ desteği açıklanmaktadır:

IR türü | Ortak ağ | Özel ağ
------- | -------------- | ---------------
Azure | Veri taşıma<br/>Etkinlik dağıtma | &nbsp;
Kendinden konak | Veri taşıma<br/>Etkinlik dağıtma | Veri taşıma<br/>Etkinlik dağıtma
Azure-SSIS | SSIS paketi yürütme | SSIS paketi yürütme

Aşağıdaki şemada gelişmiş veri tümleştirme özellikleri ve ağ desteği sunmak için birlikte kullanılabilecek farklı tümleştirme çalışma zamanları gösterilmiştir:

![Farklı tümleştirme çalışma zamanı türleri](media\concepts-integration-runtime\different-integration-runtimes.png)


## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı
Azure tümleştirme çalışma zamanları şunları yapabilir:

- Bulut veri depoları arasında kopyalama etkinliği gerçekleştirme
- Ortak ağda şu dönüştürme etkinliklerini dağıtma: HDInsight Hive etkinliği, HDInsight Pig etkinliği, HDInsight MapReduce etkinliği, HDInsight Spark etkinliği, HDInsight Streaming etkinliği, Machine Learning Batch Execution etkinliği, Machine Learning Update Resource etkinlikleri, Stored Procedure etkinliği, Data Lake Analytics U-SQL etkinliği, .Net özel etkinliği, Web etkinliği, Lookup etkinliği ve Get Metadata etkinliği.

### <a name="network-environment"></a>Ağ ortamı
Azure Integration Runtime, ortak ağda yer alan ve ortak erişime açık uç noktalara sahip olan veri depolarına ve işlem hizmetlerine bağlanmayı destekler. Azure Sanal Ağ ortamı için kendiliğinden konak tümleştirme çalışma zamanı kullanın.

### <a name="compute-resource-and-scaling"></a>İşlem kaynağı ve ölçeklendirme
Azure tümleştirme çalışma zamanı Azure'da tamamen yönetilebilen ve sunucusuz bir işlem sunar.  Altyapı sağlama, yazılım yükleme, düzeltme eki uygulama ve kapasite ölçeklendirme konularında endişe etmeniz gerekmez.  Ayrıca yalnızca gerçekten kullandığınız süre boyunca ödeme yaparsınız.

Azure tümleştirme çalışma zamanı verileri bulut veri depoları arasında güvenli, güvenilir ve yüksek performanslı bir şekilde taşınması için gerekli yerel işlemi sunar.  Kopyalama etkinliğinde kullanılacak veri taşıma birimi sayısını belirleyebilirsiniz. Bunu yaptığınızda Azure IR işlem boyutu esnek şekilde ölçeklendirilerek Azure Integration Runtime boyutunu el ile ayarlama ihtiyacını ortadan kaldırır.

Etkinlik dağıtma, etkinliği hedef işlem hizmetine yönlendiren basit bir işlemdir. Bu nedenle bu senaryo için işlem boyutu ölçeğini genişletmeniz gerekmez.

Azure IR oluşturma ve yapılandırma hakkında bilgi almak için nasıl yapılır kılavuzlarından Azure IR'yi oluşturma ve yapılandırma bölümüne bakın. 

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı
Kendinden konak IR şu özelliklere sahiptir:

- Bulut veri depoları ve özel ağdaki veri deposu arasında kopyalama etkinliği çalıştırma.
- Şu dönüştürme etkinliklerini Şirket İçi veya Azure Sanal Ağ içindeki işlem kaynaklarında dağıtma: HDInsight Hive etkinliği (BYOC), HDInsight Pig etkinliği (BYOC), HDInsight MapReduce etkinliği (BYOC), HDInsight Spark etkinliği (BYOC), HDInsight Streaming etkinliği (BYOC), Machine Learning Batch Execution etkinliği, Machine Learning Update Resource etkinlikleri, Stored Procedure etkinliği, Data Lake Analytics U-SQL etkinliği, .Net özel etkinliği, Lookup etkinliği ve Get Metadata etkinliği.

> [!NOTE] 
> SAP Hana ve MySQL gibi kendi sürücünü getir düzenine sahip veri depolarını desteklemek için kendinden konak tümleştirme çalışma zamanını kullanın.  Daha fazla bilgi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="network-environment"></a>Ağ ortamı
Ortak bulut ortamından ulaşılamayan özel ağ ortamında güvenli bir şekilde veri tümleştirmesi gerçekleştirmek istiyorsanız kurumsal güvenlik duvarının arkasına veya bir sanal özel ağ içine kendinden konak IR yükleyebilirsiniz.  Kendinden konak tümleştirme çalışma zamanı yalnızca açık internete giden HTTP tabanlı bağlantılar oluşturur.

### <a name="compute-resource-and-scaling"></a>İşlem kaynağı ve ölçeklendirme
Kendinden konak IR'nin şirket içindeki bir makineye veya özel ağ içindeki bir sanal makineye yüklenmesi gerekir. Şu anda kendinden konak IR yalnızca Windows işletim sistemlerinde çalışmaktadır.  

Yüksek kullanılabilirlik ve ölçeklenebilirlik için kendinden konak IR ölçeğini mantıksal örneği birden fazla şirket içi makineyle etkin-etkin modda ilişkilendirerek genişletebilirsiniz.  Daha fazla bilgi için nasıl yapılır kılavuzlarında kendinden konak IR oluşturma ve yapılandırma makalesine bakın.

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime
Var olan SSIS iş yükünü artırmak ve değiştirmek için Azure-SSIS IR oluşturarak SSIS paketlerini yerel ortamda yürütebilirsiniz.

### <a name="network-environment"></a>Ağ ortamı
Azure-SSIS IR ortak ağ veya özel ağ üzerinde sağlanabilir.  Şirket içi verilere erişim için Azure-SSIS IR'nin şirket içi ağınıza bağlı bir Sanal Ağa katılması gerekir.  

### <a name="compute-resource-and-scaling"></a>İşlem kaynağı ve ölçeklendirme
Azure-SSIS IR, SSIS paketlerinizi çalıştırmaya ayrılmış Azure sanal makinelerinin tam yönetilen bir kümesidir. Kendi Azure SQL Veritabanı veya Yönetilen Örneği (Önizleme) sunucunuzu kullanarak eklenecek SSIS projelerini/paketlerini (SSISDB) barındırmasını sağlayabilirsiniz. Düğüm boyutunu belirttikten sonra kümedeki düğüm sayısını belirtik ölçeğini genişleterek işlem gücünü artırabilirsiniz. Azure-SSIS Integration Runtime hizmetini gerekli olduğunda durdurup başlatarak çalıştırma maliyetlerini kontrol altına alabilirsiniz.

Daha fazla bilgi için nasıl yapılır kılavuzlarında Azure SSIS IR oluşturma ve yapılandırma makalesine bakın.  Oluşturduktan sonra var olan SSIS paketlerinizi çok az veya sıfır değişiklikle SQL Server Veri Araçları (SSDT) ve SQL Server Management Studio (SSMS) gibi bilinen araçları kullanarak şirket içi SSIS kullanır gibi dağıtabilir ve yönetebilirsiniz.

Azure-SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 

- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (Önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına (VNet) ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımları sunar. 

## <a name="determining-which-ir-to-use"></a>Kullanılacak IR'yi belirleme
Her dönüştürme etkinliğinde bir tümleştirme çalışma zamanını işaret eden hedef işlem Bağlı Hizmeti vardır. Bu tümleştirme çalışma zamanı örneği, dönüştürme etkinliğinin dağıtıldığı yerdir.

Kopyalama etkinliği için veri akışı yönünü tanımlamak üzere kaynak ve havuz bağlantılı hizmetleri gerektirir. Kopyalama işlemini gerçekleştirmek için kullanılacak olan tümleştirme çalışma zamanı örneğini belirlemek için aşağıdaki mantık kullanılır: 

- **İki bulut veri kaynağı arasında kopyalama yapma**: Hem kaynak hem de havuz bağlantılı hizmetler Azure IR kullandığında Kopyalama etkinliğini yürütmek için havuz bağlantılı Hizmet kullanılır.
- **Bir bulut veri kaynağından özel ağdaki veri kaynağına kopyalama**: Kaynak veya havuz bağlantılı hizmet noktaları kendinden konak IR birimine işaret ediyorsa kopyalama etkinliği kendinden konak Integration Runtime üzerinde yürütülür.
- **Özel ağ üzerindeki iki veri kaynağı arasında kopyalama**: Hem kaynak hem de havuz Bağlantılı Hizmetin aynı tümleştirme çalışma zamanı örneğine işaret etmesi gerekir ve kopyalama Etkinliğini yürütmek için bu tümleştirme çalışma zamanı kullanılır.

Aşağıdaki şemada iki kopyalama etkinliği örneği verilmiştir:

- Kopyalama etkinliği 1'in kaynağı kendinden konak IR A'ya başvuran bir SQL Server Bağlantılı Hizmettir ve havuzu Azure IR B'ye başvuran Azure Depolama Bağlantılı Hizmettir. Kopyalama etkinliği çalıştığında kendinden konak IR A üzerinde yürütülür.
- Kopyalama etkinliği 2'nin kaynağı Azure IR C'ye başvuran bir Azure SQL Veritabanı Bağlantılı Hizmettir ve havuzu Azure IR B'ye başvuran bir Azure Depolama Bağlantılı Hizmettir. Kopyalama etkinliği çalıştığında havuz Bağlantılı Hizmeti onun tümleştirme çalışma zamanını kullandığından Azure IR B üzerinde yürütülür.

![Kullanılacak IR](media/concepts-integration-runtime/which-integration-runtime-to-use.png)

## <a name="integration-runtime-location"></a>Tümleştirme çalışma zamanının konumu
Data Factory konumu, veri fabrikası meta verilerinin depolandığı ve işlem hattı tetiklemesinin başlatıldığı konumdur. Şu anda desteklenen Data Factory konumları: Doğu ABD, Doğu ABD 2, Güneydoğu Asya ve Batı Avrupa. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir. Bu davranış veri uyumluluğu, verimlilik ve düşük ağ kullanım maliyetleri için global ölçekte birden fazla bölgede bulunan IR aracılığıyla gerçekleştirilir.

IR Konumu arka uç işleminin konumunu tanımlar ve bu veri taşıma, etkinlik dağıtımı ve SSIS paket yürütme işlemlerinin gerçekleştirileceği konumdur. IR konumu veri fabrikasının ait olduğu konumdan farklı olabilir. Aşağıdaki şemada Data Factory konum ayarları ve tümleştirme çalışma zamanları gösterilmektedir:

![Tümleştirme çalışma zamanının konumu](media/concepts-integration-runtime/integration-runtime-location.png)

### <a name="azure-ir"></a>Azure IR
Data Factory, verileri taşımak için aynı coğrafyadaki havuza en yakın olan bölgeyi kullanır. Eşleme için aşağıdaki tabloya bakın:

Havuz veri deposu coğrafyası | Havuz veri deposu konumu | Azure Integration Runtime için kullanılan konum
-------------------------------| ----------------| ------------------
Amerika Birleşik Devletleri | Doğu ABD | Doğu ABD
&nbsp; | Doğu ABD 2 | Doğu ABD 2
&nbsp; | Orta ABD | Orta ABD
&nbsp; | Orta Kuzey ABD | Orta Kuzey ABD
&nbsp; | Orta Güney ABD | Orta Güney ABD
&nbsp; | Batı Orta ABD | Batı Orta ABD
&nbsp; | Batı ABD | Batı ABD
&nbsp; | Batı ABD 2 | Batı ABD 2
Kanada | Doğu Kanada | Orta Kanada
&nbsp; | Orta Kanada | Orta Kanada
Brezilya | Güney Brezilya | Güney Brezilya
Avrupa | Kuzey Avrupa | Kuzey Avrupa
&nbsp; | Batı Avrupa | Batı Avrupa
Birleşik Krallık | Birleşik Krallık Batı | Birleşik Krallık Güney
&nbsp; | Birleşik Krallık Güney | Birleşik Krallık Güney
Asya Pasifik | Güneydoğu Asya | Güneydoğu Asya
&nbsp; | Doğu Asya | Güneydoğu Asya
Avustralya | Avustralya Doğu | Avustralya Doğu
&nbsp; | Avustralya Güneydoğu | Avustralya Güneydoğu
Japonya | Japonya Doğu | Japonya Doğu
&nbsp; | Japonya Batı | Japonya Doğu
Kore | Kore Orta | Kore Orta
&nbsp; | Kore Güney | Kore Orta
Hindistan | Orta Hindistan | Orta Hindistan
&nbsp; | Batı Hindistan | Orta Hindistan
&nbsp; | Güney Hindistan | Orta Hindistan

Azure IR konumunu otomatik çözümleme olarak da ayarlayabilirsiniz. Bu durumda Data Factory, bağlantılı hizmet tanımına göre kullanılacak en iyi konumu belirlemeye çalışır.

> [!NOTE] 
> Hedef veri deposunun bölgesi listede yoksa veya tespit edilemezse uyumluluk nedeniyle etkinlik alternatif bölgelere gitmek yerine başarısız olur. Bu durumda kopyalama için kullanılacak alternatif Konumu açıkça belirtmeniz gerekir.
 
Aşağıdaki resimde Azure IR konumu otomatik çözümlemeye ayarlandığında geçerli olacak konum örneği gösterilmektedir. Bir kopyalama etkinliği yürütüldüğünde veri hedefinin konumu (bu örnekte Japonya Batı) tespit edilir.  Tabloya göre gerçek veri kopyalama işlemini gerçekleştirmek için Japonya Doğu konumundaki Azure IR kullanılır. Bir Spark etkinliği için HDInsight'a bağlanmak üzere aynı IR kullanıldığında Spark uygulaması gönderimi Data Factory konumundan (bu örnekte Doğu ABD) gerçekleştirilir ve Spark uygulamasının gerçek yürütülmesi HDInsight sunucusunun konumunda gerçekleşir. 

![Geçerli konum](media/concepts-integration-runtime/effective-location.png)

### <a name="self-hosted-ir"></a>Kendinden konak IR
Kendinden konak IR, mantıksal olarak Data Factory'ye kayıtlıdır ve işlevini desteklemek için kullanılan işlem sizin tarafınızdan sağlanır. Bu nedenle kendinden konak IR için açık bir konum özelliği yoktur. 

Kendinden konak IR veri taşıma işlemini gerçekleştirmek için kullanıldığında kaynaktan veri ayıklar ve hedefe yazar.

### <a name="azure-ssis-ir"></a>Azure-SSIS IR
Ayıklama, dönüştürme, yükleme (ETL) iş akışlarınızda yüksek performansa ulaşmak için doğru Azure-SSIS IR konumunu seçmek önemlidir.  Önizleme sürümünde altı konum (Doğu ABD, Doğu ABD 2, Orta ABD, Avustralya Doğu, Kuzey Avrupa ve Batı Avrupa) kullanılabilir.

- Azure-SSIS IR konumunun veri fabrikası konumu ile aynı olması gerekmez ancak SSISDB'nin barındırılacağı Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucusunun konumuyla aynı olmalıdır. Bu şekilde Azure-SSIS Integration Runtime biriminiz farklı konumlar arasında aşırı trafik oluşturmadan kolayca SSISDB öğesine erişebilir.
- SSISDB'yi barındırmak için var olan bir Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucunuz yoksa ancak şirket içi veri kaynaklarınız/hedeflerini varsa şirket içi ağınıza bağlı sanal ağ ile aynı konumda yeni bir Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucusu oluşturmanız gerekir.  Bu şekilde Azure-SSIS IR öğenizi yeni Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucunu kullanarak oluşturabilir ve tümünü aynı konumdaki sanal ağa ekleyerek farklı konumlar arasında veri taşıma sayısını en aza indirebilirsiniz.
- SSISDB'nin barındırıldığı var olan Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucunuzun konumu şirket içi ağınıza bağlı özel ağın konumuyla aynı değilse öncelikle var olan bir Azure SQL Veritabanı/Yönetilen Örnek (Önizleme) sunucusunu kullanarak Azure SSIS IR öğenizi oluşturup aynı konumdaki başka bir sanal ağa ekleyin ve ardından iki konumdaki sanal ağlar arasında bağlantı yapılandırması gerçekleştirin.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Kendinden konak tümleştirme çalışma zamanı oluşturma](create-self-hosted-integration-runtime.md)
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (Önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
