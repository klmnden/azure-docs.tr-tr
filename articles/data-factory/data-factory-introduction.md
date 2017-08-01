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
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 537bdee67ed9648c3cba2099553d847399609705
ms.contentlocale: tr-tr
ms.lasthandoff: 06/30/2017


---
# <a name="introduction-to-azure-data-factory"></a>Azure Data Factory'ye giriş 
## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?
Büyük veri dünyasında, işletmede mevcut verilerden nasıl yararlanılır? Şirket içi veri kaynaklarından veya diğer dağınık veri kaynaklarından elde edilen başvuru verilerini kullanarak bulutta oluşturulan verileri zenginleştirmek mümkün mü? Örneğin, bir oyun şirketi, oyunlar tarafından üretilen çok sayıda günlüğü bulutta toplamaktadır. Müşteri tercihleri, demografik bilgileri, kullanım davranışı, vb. konusunda fikir edinmek, yukarı satış ve çapraz satış fırsatlarını belirlemek, iş büyümesi sağlamak üzere yeni çekici özellikler geliştirmek ve müşterilere daha iyi bir deneyim sunmak üzere bu günlükleri analiz etmek istemektedir. 

Bu günlükleri analiz etmek için, şirketin şirket içi veri deposunda bulunan müşteri bilgileri, oyun bilgileri, pazarlama kampanyası bilgileri gibi başvuru verilerini kullanması gerekir. Bu nedenle, şirket bulut veri deposundan günlük verilerini ve şirket içi veri deposundan başvuru verilerini almak istemektedir. Ardından, bulutta (Azure HDInsight) Hadoop kullanarak verileri işleyip, sonuç verilerini Azure SQL Veri Ambarı gibi bir bulut veri ambarında veya SQL Server gibi bir şirket içi veri deposunda yayımlamak ister. Bu iş akışının haftada bir kez gerçekleştirilmesini ister. 

Gerekli olan şey, şirketin hem şirket içindeki hem de buluttaki veri depolarından veri alabilen bir iş akışı oluşturmasına, Hadoop gibi mevcut işlem hizmetlerini kullanarak verileri dönüştürmesine ya da işlemesine ve sonuçları BI uygulamalarının kullanması için şirket içi veya bulut veri deposunda yayımlamaya olanak tanıyan bir platformdur. 

![Data Factory'ye genel bakış](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory, bu tür senaryolara yönelik bir platformdur. **Bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir**. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.  

Bu, geleneksel bir Ayıklama-Dönüştürme-Yükleme (ETL) platformu yerine daha çok Ayıklama-Dönüştürme (EL) ve sonra Dönüştürme-Yükleme (TL) platformudur. Gerçekleştirilen dönüşümler; türetilmiş sütunlar ekleme, satır sayısını sayma, verileri sıralama, vb. dönüşümleri gerçekleştirmek yerine işlem hizmetlerini kullanarak verileri dönüştürmek/işlemek için gerçekleştirilir. 

Şu anda Azure Data Factory’de iş akışları tarafından tüketilen ve üretilen veriler, **zaman dilimli verilerdir** (saatlik, günlük, haftalık, vb.). Örneğin, bir işlem hattı günde bir kez giriş verilerini okuyabilir, verileri işleyebilir ve çıktı üretebilir. Bir iş akışını yalnızca bir kez de çalıştırabilirsiniz.  
  

## <a name="how-does-it-work"></a>Nasıl çalışır? 
Azure Data Factory’deki işlem hatları (veri odaklı iş akışları) genellikle aşağıdaki üç adımı gerçekleştirir:

![Azure Data Factory’nin üç aşaması](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Bağlanma ve toplama
Kuruluşların dağınık kaynaklarda yer alan çeşitli türlerde verileri vardır. Bilgi üretim sistemi oluşturmanın ilk adımı SaaS hizmetleri, dosya paylaşımları, FTP, web hizmetleri gibi tüm gerekli veri kaynaklarına ve işleme çalışmalarına bağlanmak ve daha sonraki işleme çalışmaları için gerektiğinde verileri merkezi bir konuma taşımaktır.

Data Factory olmadığında, kuruluşların bu veri kaynaklarını ve işleme çalışmalarını tümleştirmek için özel veri taşıma bileşenleri oluşturması veya özel hizmetler yazması gerekir. Bu tür sistemler pahalıdır, tümleştirmesi ve bakımı zordur; ayrıca tümüyle yönetilen bir hizmetin sağlayabileceği kurumsal sınıf izleme ve uyarılarla denetimlerden yoksundur.

Data Factory ile, veri işlem hattında Kopyalama Etkinliği’ni kullanarak hem şirket içinde hem de buluttaki kaynak veri depolarını daha fazla analiz için merkezi bir veri deposuna taşıyabilirsiniz. Örneğin, Azure Data Lake Store’da veri toplayabilir ve daha sonra Azure Data Lake Analytics işlem hizmetini kullanarak verileri dönüştürebilirsiniz. Öte yandan, verileri Azure Blob Depolama Alanı’ndan toplayıp daha sonra Azure HDInsight Hadoop kümesi kullanarak da dönüştürebilirsiniz.

### <a name="transform-and-enrich"></a>Dönüştürme ve zenginleştirme
Veriler buluttaki merkezi bir veri deposuna sunulduktan sonra, toplanan verilerin HDInsight Hadoop, Spark, Data Lake Analytics ve Machine Learning gibi işlem hizmetleri kullanılarak işlenmesi veya dönüştürülmesi gerekir. Üretim ortamlarının güvenilir verilerle beslenmesi için sürdürülebilir ve denetlenebilir bir zamanlamaya göre dönüştürülmüş verileri güvenilir bir şekilde üretmeniz gerekir. 

### <a name="publish"></a>Yayımlama 
Dönüştürülen verileri buluttan SQL Server gibi şirket içi kaynaklara aktarın veya iş zekası (BI) uygulamaları, analiz araçları ya da diğer uygulamalar tarafından kullanılmak üzere bulut depolama kaynaklarınızda saklayın.

## <a name="key-components"></a>Başlıca bileşenler
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory, üzerinde veri taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu sağlamak üzere birlikte çalışan başlıca dört bileşenden oluşur. 

### <a name="pipeline"></a>İşlem hattı
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı bir grup etkinliktir. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı Azure blobundan verileri alan ve ardından HDInsight kümesinde Hive sorgusu çalıştırarak verileri bölümlere ayıran bir grup etkinlik içerebilir. İşlem hattının avantajı, etkinliklerin her birini tek tek yönetmek yerine bir küme olarak yönetmenize olanak tanımasıdır. Örneğin, etkinlikleri bağımsız olarak dağıtmak ve zamanlamak yerine işlem hattını dağıtabilir ve zamanlayabilirsiniz. 

### <a name="activity"></a>Etkinlik
İşlem hattında bir veya daha fazla etkinlik olabilir. Etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlayın. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için Kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri.

### <a name="data-movement-activities"></a>Veri taşıma etkinlikleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

### <a name="data-transformation-activities"></a>Veri dönüştürme etkinlikleri
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

### <a name="custom-net-activities"></a>Özel .NET etkinlikleri
Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız ya da kendi mantığınızı kullanarak verileri dönüştürmeniz gerekirse **özel bir .NET etkinliği** oluşturun. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

### <a name="datasets"></a>Veri kümeleri
Bir etkinlik girdi olarak sıfır veya daha fazla veri kümesi ve çıktı olarak bir ya da daha fazla veri kümesi alır. Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. Örneğin Azure Blob veri kümesi, işlem hattının verileri okuması gereken blob kapsayıcısını ve Azure Blob Depolama klasörünü belirtir. Veya bir Azure SQL Tablosu veri kümesi, çıktı verilerinin etkinlik tarafından yazılacağı tabloyu belirtir. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün: bağlı bir hizmet, veri kaynağıyla bağlantıyı tanımlar ve veri kümesi verilerin yapısını temsil eder. Örneğin, Azure Depolama bağlı hizmeti Azure Depolama hesabına bağlanacak bağlantı dizesini belirtir. Ayrıca, bir Azure Blob veri kümesi blob kapsayıcıyı ve verileri içeren klasörü belirtir.   

Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

* Bir **veri deposunu**, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server, Oracle veritabanı, dosya paylaşımı veya bir Azure Blob Depolama hesabı. Desteklenen veri depolarının bir listesi için [Veri taşıma etkinlikleri](#data-movement-activities) bölümüne bakın.
* Etkinlik yürütülmesini barındırabilen **işlem kaynağını** temsil etmek için. Örneğin, HDInsightHive etkinliği bir HDInsight Hadoop kümesinde yürütülür. Desteklenen işlem ortamlarının listesi için [Veri dönüştürme etkinlikleri](#data-transformation-activities) bölümüne bakın.

### <a name="relationship-between-data-factory-entities"></a>Data Factory varlıkları arasındaki ilişki
![Diyagram: Bir bulut veri tümleştirme hizmeti olan Data Factory ile ilgili Temel Kavramlar](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Figure2.** Veri kümesi, Etkinlik, İşlem Hattı ve Bağlı hizmet arasındaki ilişkiler

## <a name="supported-regions"></a>Desteklenen bölgeler
Şu anda **Batı ABD**, **Doğu ABD** ve **Kuzey Avrupa** bölgelerinde veri fabrikaları oluşturabilirsiniz. Ancak, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla data factory başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir.

Azure Data Factory’nin kendisi verileri depolamaz. Veri hareketini [desteklenen veri depoları](#data-movement-activities) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](#data-transformation-activities) kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar.

Data Factory yalnızca **Batı ABD**, **Doğu ABD** **Kuzey Avrupa** bölgelerinde kullanılabilir olsa da, Data Factory’de veri taşımayı destekleyen hizmet birçok bölgede [küresel olarak](data-factory-data-movement-activities.md#global) kullanılabilmektedir. Veri deposunun güvenlik duvarı ardında kaldığı durumlarda şirket içi ortamınızda yüklü bir [Veri Yönetimi Ağ Geçidi](data-factory-move-data-between-onprem-and-cloud.md) bunun yerine verileri taşır.

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgesinde çalıştığını varsayalım. Kuzey Avrupa’da bir Azure Data Factory örneği oluşturup geliştirebilir ve bunu Batı Avrupa’daki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory’nin işlem ortamınızda işi tetiklemesi birkaç milisaniye alsa da, bilgi işlem ortamınızda işin çalıştırılma süresi değişmez.

## <a name="get-started-with-creating-a-pipeline"></a>İşlem hattı oluşturmaya başlama
Azure Data Factory'de veri işlem hatları oluşturmak için bu araç veya API'lerden birini kullanabilirsiniz: 

- Azure portalına
- Visual Studio
- PowerShell
- .NET API’si
- REST API
- Azure Resource Manager şablonu. 

Veri işlem hatları ile veri fabrikaları oluşturmayı öğrenmek için aşağıdaki öğreticilerde yer alan adım adım yönergeleri izleyin:

| Öğretici | Açıklama |
| --- | --- |
| [İki bulut veri deposu arasında veri taşıma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |Bu öğreticide, Blob depolama biriminden SQL veritabanına **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız. |
| [Hadoop kümesi kullanarak veri dönüştürme](data-factory-build-your-first-pipeline.md) |Bu öğreticide, bir Azure HDInsight (Hadoop) kümesinde Hive betiği çalıştırarak **veri işleyen** bir veri işlem hattı ile ilk Azure veri fabrikanızı oluşturacaksınız. |
| [Veri Yönetimi Ağ Geçidi'ni kullanarak verileri şirket içi veri deposu ile bulut veri deposu arasında taşıma](data-factory-move-data-between-onprem-and-cloud.md) |Bu öğreticide, **şirket içi** SQL Server veritabanından Azure blob’a **veri taşıyan** bir işlem hattı ile veri fabrikası oluşturacaksınız. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız. |

