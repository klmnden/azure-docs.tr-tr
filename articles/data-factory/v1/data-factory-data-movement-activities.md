---
title: Kopyalama etkinliği kullanarak veri taşıma | Microsoft Docs
description: 'Data Factory işlem hatlarını veri taşıma hakkında bilgi edinin: Bulut depoları arasında ve bir şirket içi depolama ve bulut deposu arasındaki veri geçişi. Kopyalama etkinliği kullanın.'
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6b13c70d86af195e50190083aa562811236cdd4b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054267"
---
# <a name="move-data-by-using-copy-activity"></a>Kopyalama etkinliği kullanarak veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-data-movement-activities.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-overview.md)

> [!NOTE]
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 kopyalama etkinliği](../copy-activity-overview.md).

## <a name="overview"></a>Genel Bakış
Azure Data Factory'de şirket içi ve bulut arasında veri kopyalamak için kopyalama etkinliği kullanabilirsiniz verileri depolar. Verileri kopyaladıktan sonra daha fazla dönüştürülen ve analiz edilebilir. Kopyalama etkinliği, dönüştürme ve iş zekası (BI) ve uygulama tüketimi için çözümleme sonuçlarını yayımlamak için de kullanabilirsiniz.

![Kopyalama etkinliği rolü](media/data-factory-data-movement-activities/copy-activity.png)

Kopyalama etkinliği desteklenir güvenli tarafından güvenilir ve ölçeklenebilir ve [genel olarak kullanılabilir hizmet](#global). Bu makalede, veri taşıma veri fabrikası ve kopyalama etkinliği hakkında ayrıntılar sağlar.

İlk olarak, iki bulut veri depoları arasında ve bir şirket içi veri depolama ve bulut veri deposu arasındaki veri geçişinin nasıl gerçekleştirildiğini görelim.

> [!NOTE]
> Aktiviteler hakkında genel bilgi edinmek için [anlama işlem hatlarının ve etkinliklerin](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>İki bulut veri depoları arasında veri kopyalama
Kaynak ve havuz veri depolarına bulutta olduğunda kopyalama etkinliği verileri kaynak havuzuna kopyalamak için aşağıdaki aşamaları geçer. Kopyalama etkinliği'nın temelini oluşturan hizmeti:

1. Veri kaynağına veri deposundan okur.
2. Serileştirme/seri durumdan çıkarma, sıkıştırma/açma, sütun eşleme gerçekleştirir ve dönüştürme yazın. Girdi veri kümesi, çıktı veri kümesi ve kopyalama etkinliği yapılandırmalarını temel alan bu işlemleri yapar.
3. Hedef veri deposuna verileri yazar.

Hizmet veri taşımayı gerçekleştirmek için en iyi bölge otomatik olarak seçer. Bu genellikle bir havuz veri deposuna yakın bölgedir.

![Bulut Bulut kopyalama](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Bir şirket içi veri depolama ve bir bulut veri deposu arasında veri kopyalama
Güvenli bir şekilde bir şirket içi veri deposu ile bulut veri deposu arasında verileri taşımak için veri yönetimi ağ geçidi, şirket içi makineye yükleyin. Veri Yönetimi ağ geçidi karma veri hareketlerini ve işleme sağlayan bir aracıdır. Aynı makinede kendisi verileri depolamak ya da ayrı bir makinede veri deposuna erişim sahip olarak yükleyebilirsiniz.

Bu senaryoda, veri yönetimi ağ geçidi serileştirme/seri durumdan çıkarma, sıkıştırma/açma sütun eşleme gerçekleştirir ve dönüştürme yazın. Verileri Azure Data Factory hizmeti aracılığıyla geçmez. Bunun yerine, veri yönetimi ağ geçidi doğrudan hedef depolama verileri yazar.

![Şirket içi-için-bulut kopyalama](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Bkz: [şirket içi ve bulut arasında veri taşıma veri depolarına](data-factory-move-data-between-onprem-and-cloud.md) bir giriş ve gözden geçirme. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) bu aracı hakkında ayrıntılı bilgi için.

Veri Yönetimi ağ geçidi kullanarak Azure Iaas sanal makineleri (VM'ler) üzerinde barındırılan veri başlangıç/bitiş desteklenen veri depoları da taşıyabilirsiniz. Bu durumda, veri yönetimi ağ geçidi aynı VM'de kendisi verileri depolamak veya ayrı bir VM'ye veri deposuna erişimi olan yükleyebilirsiniz.

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimler
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

> [!NOTE] 
> Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız gerekirse Data Factory’de **özel bir etkinliği** kendi veri kopyalama/taşıma mantığınızla kullanın. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> \* taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri
Kopyalama etkinliği kullanabilirsiniz **olarak dosyaları kopyalama-olan** iki dosya tabanlı veri depoları arasında atlayabilirsiniz [Biçim bölümü](data-factory-create-datasets.md) girdi ve çıktı veri kümesi tanımlarında. Veriler, tüm serileştirme/seri durumdan çıkarma verimli bir şekilde kopyalanır.

Kopyalama etkinliği ayrıca okur ve belirtilen biçimleri dosyalarında Yazar: **metin, JSON, Avro, ORC ve Parquet**ve sıkıştırma codec **GZIP, Deflate, Bzıp2 ve ZipDeflate** desteklenir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) ayrıntılarla.

Örneğin, aşağıdaki kopyalama etkinliklerin yapabilirsiniz:

* Şirket içi SQL Server veri kopyalama ve Azure Data Lake Store'a ORC biçiminde yazın.
* Dosyaları metin (CSV) biçiminde şirket içi dosya sisteminden kopyalama ve Azure Blob Avro biçiminde yazın.
* Şirket içi dosya sisteminden daraltılmış dosyaları kopyalayın ve Azure Data Lake Store'a kara açın.
* Verileri Azure Blob'tan GZip sıkıştırılmış metin (CSV) biçiminde kopyalayın ve Azure SQL veritabanına yazma.

## <a name="global"></a>Genel olarak kullanılabilir veri taşıma
Azure Data Factory yalnızca Batı ABD, Doğu ABD ve Kuzey Avrupa bölgelerde kullanılabilir. Ancak, kopyalama etkinliği'nın temelini oluşturan genel olarak aşağıdaki bölgeler ve coğrafi kullanılabilir hizmetidir. Genel olarak kullanılabilir topoloji genellikle çapraz bölge atlama önler verimli veri taşıma sağlar. Bkz: [bölgeye göre Hizmetleri](https://azure.microsoft.com/regions/#services) Data Factory ve veri taşıma bir bölgede kullanılabilirliği.

### <a name="copy-data-between-cloud-data-stores"></a>Bulut veri depoları arasında veri kopyalama
Kaynak ve havuz veri depolarına bulutta, veri fabrikası Hizmet dağıtımının verileri taşımak için aynı Coğrafya havuzunda yakın bölgede kullanır. Eşleme için aşağıdaki tabloya bakın:

| Coğrafya hedef veri depoları | Hedef veri deposuna bölgesi | Veri taşıma için kullanılan bölge |
|:--- |:--- |:--- |
| Amerika Birleşik Devletleri | Doğu ABD | Doğu ABD |
| &nbsp; | Doğu ABD 2 | Doğu ABD 2 |
| &nbsp; | Orta ABD | Orta ABD |
| &nbsp; | Orta Kuzey ABD | Orta Kuzey ABD |
| &nbsp; | Orta Güney ABD | Orta Güney ABD |
| &nbsp; | Batı Orta ABD | Batı Orta ABD |
| &nbsp; | Batı ABD | Batı ABD |
| &nbsp; | Batı ABD 2 | Batı ABD 2 |
| Kanada | Doğu Kanada | Orta Kanada |
| &nbsp; | Orta Kanada | Orta Kanada |
| Brezilya | Güney Brezilya | Güney Brezilya |
| Avrupa | Kuzey Avrupa | Kuzey Avrupa |
| &nbsp; | Batı Avrupa | Batı Avrupa |
| Birleşik Krallık | Birleşik Krallık Batı | Birleşik Krallık Güney |
| &nbsp; | Birleşik Krallık Güney | Birleşik Krallık Güney |
| Asya Pasifik | Güneydoğu Asya | Güneydoğu Asya |
| &nbsp; | Doğu Asya | Güneydoğu Asya |
| Avustralya | Avustralya Doğu | Avustralya Doğu |
| &nbsp; | Avustralya Güneydoğu | Avustralya Güneydoğu |
| Hindistan | Orta Hindistan | Orta Hindistan |
| &nbsp; | Batı Hindistan | Orta Hindistan |
| &nbsp; | Güney Hindistan | Orta Hindistan |
| Japonya | Japonya Doğu | Japonya Doğu |
| &nbsp; | Japonya Batı | Japonya Doğu |
| Kore | Kore Orta | Kore Orta |
| &nbsp; | Kore Güney | Kore Orta |

Alternatif olarak, açıkça belirterek kopyalama işlemini gerçekleştirmek için kullanılacak Data Factory hizmetinin bölge belirtebilir `executionLocation` kopyalama etkinliği'nin altındaki özelliğini `typeProperties`. Bu özellik için desteklenen değerler listelenmiştir yukarıda **bölge veri taşıma için kullanılan** sütun. Verilerinizi bu bölge kablo üzerinden kopyalama sırasında gider unutmayın. Örneğin, arasında kopyalanacak belirleyebileceğiniz Azure içinde Kore depolar, `"executionLocation": "Japan East"` Japonya bölge yönlendir için (bkz [JSON örnek](#by-using-json-scripts) başvuru olarak).

> [!NOTE]
> Hedef veri deposuna bölge önceki listede yok veya belirlenemeyen, varsayılan olarak kopyalama etkinliği alternatif bir bölge giderek yerine sürece başarısız olursa `executionLocation` belirtilir. Desteklenen bölge listesi zamanla genişletilir.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Bir şirket içi veri depolama ve bir bulut veri deposu arasında veri kopyalama
Ne zaman veri kopyalanır şirket içi (ya da Azure sanal makineleri/Iaas) arasında ve bulut depoları, [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) bir şirket içi makineye veya sanal makineye veri taşımayı gerçekleştirir. Kullanmadığınız sürece veri bulut hizmeti aracılığıyla akış değil [kopyalama hazırlanan](data-factory-copy-activity-performance.md#staged-copy) yeteneği. Bu durumda, havuz veri deposuna yazılmadan önce verileri hazırlama Azure Blob Depolama akar.

## <a name="create-a-pipeline-with-copy-activity"></a>Kopyalama etkinliği ile işlem hattı oluşturma
Çeşitli şekillerde kopyalama etkinliği ile işlem hattı oluşturabilirsiniz:

### <a name="by-using-the-copy-wizard"></a>Kopyalama Sihirbazı'nı kullanma
Data Factory Kopyalama Sihirbazı'nı kopyalama etkinliği ile işlem hattı oluşturmanıza yardımcı olur. Bu ardışık düzen hedeflere desteklenen kaynaklardan veri kopyalamanızı sağlar *JSON yazma olmadan* bağlı hizmetler, veri kümeleri ve işlem hatları için tanımlar. Bkz: [Data Factory Kopyalama Sihirbazı](data-factory-copy-wizard.md) sihirbaz hakkındaki ayrıntılar için.  

### <a name="by-using-json-scripts"></a>JSON komut dosyalarını kullanarak
Bir ardışık düzeni için JSON tanımını (bir kopyalama etkinliği'ı kullanarak) oluşturmak için Azure portal, Visual Studio veya Azure PowerShell Data Factory düzenleyici kullanabilirsiniz. Ardından, bu veri fabrikasında ardışık düzen oluşturmak için dağıtabilirsiniz. Bkz [Öğreticisi: Azure Data Factory işlem hattı kullanım kopyalama etkinliği](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bir öğretici hakkında adım adım yönergeler için.    

JSON özellikleri (örneğin, ad, açıklama, giriş ve çıkış tabloları ve ilkeleri) etkinlikleri tüm türleri için kullanılabilir. Kullanılabilir özellikler `typeProperties` etkinlik bölümünü her etkinlik türü ile değişir.

Kopya etkinliği için `typeProperties` bölüm kaynakları türlerine bağlı olarak değişir ve şu havuzlar. Kaynak/havuz içindeki tıklatın [desteklenen kaynakları ve havuzlarını](#supported-data-stores-and-formats) kopyalama etkinliği için bu veri deposu destekleyen türü özellikleri hakkında bilgi edinmek için bölüm.

Bir örnek JSON tanımı aşağıda verilmiştir:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
Etkinlik çalıştığında çıkış veri kümesinde tanımlanan zamanlaması belirler (örneğin: **günlük**, sıklığı olarak **gün**ve aralığı olarak **1**). Etkinliğin bir giriş veri kümesinden alınan verileri kopyalar (**kaynak**) bir çıkış veri kümesi için (**havuz**).

Kopya etkinliği için birden fazla girdi veri kümesi belirtebilirsiniz. Bunlar, etkinlik çalıştırılmadan önce bağımlılıkları doğrulamak için kullanılır. Ancak, yalnızca ilk veri kümesini verileri hedef veri kümesi için kopyalanır. Daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Performans ve ayar
Bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md), Azure Data factory'de veri taşımayı (kopyalama etkinliği) performansını etkileyen önemli faktör açıklar. Ayrıca, iç test sırasında gözlemlenen performans listeler ve kopyalama etkinliği performansını iyileştirmek için çeşitli yollar ele alınmaktadır.

## <a name="fault-tolerance"></a>Hataya dayanıklılık
Varsayılan olarak, veri ve return hatası kopyalama kopyalama etkinliği durdurulacak zaman uyumsuz veri kaynağı ve havuz; arasında karşılaşırsanız atla ve uyumlu olmayan satırlar ve yalnızca kopya oturum açıkça yapılandırabilirsiniz ancak kopya oluşturmak için bu uyumlu veri başarılı oldu. Bkz: [kopyalama etkinliği hataya dayanıklılık](data-factory-copy-activity-fault-tolerance.md) üzerinde daha ayrıntılı bilgi.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Bkz: [güvenlik konuları](data-factory-data-movement-security-considerations.md), verilerinizi güvenli hale getirmek için Azure Data Factory veri taşıma hizmetleri kullanan güvenlik altyapısı açıklar.

## <a name="scheduling-and-sequential-copy"></a>Zamanlama ve sıralı kopyalama
Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme şeklini Data Factory hakkında ayrıntılı bilgi. Birden çok kopyalama işlemleri birbiri ardından sıralı/sıralı bir şekilde çalıştırmak mümkündür. Bkz: [sırayla kopyalamak](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) bölümü.

## <a name="type-conversions"></a>Tür dönüştürmeleri
Farklı veri depolarına farklı yerel tür sistemleri sahiptir. Kopyalama etkinliği, aşağıdaki iki aşamalı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün.
2. Bir .NET türünden bir yerel havuz türüne dönüştürün.

Yerel tür sistemden bir veri deposu için bir .NET türü için ilgili veri deposu makalesinde eşlemedir. (Belirli bağlantıya tıklayın [desteklenen veri depoları](#supported-data-stores) tablo). Bu eşlemeler, böylece kopyalama etkinliği sağ dönüşümleri gerçekleştirir, tablolarınızı oluşturulurken uygun türlerini belirlemek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kopyalama etkinliği hakkında daha fazla bilgi edinmek için bkz: [kopyalama verileri Azure Blob depolama alanından Azure SQL veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Bir bulut veri deposuna bir şirket içi veri deposundan veri taşıma hakkında bilgi edinmek için [veri depolarına buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).
