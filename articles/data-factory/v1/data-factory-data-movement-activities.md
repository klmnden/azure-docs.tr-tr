---
title: Kopyalama etkinliği'ni kullanarak veri taşıma | Microsoft Docs
description: "Data Factory işlem hatları veri taşıma hakkında bilgi edinin: Bulut depoları arasında ve bir şirket içi depolama ile bulut deposu arasında veri taşıma. Kopyalama etkinliği'ni kullanın."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: bfb15e717e3cb726aba782d9a9506330d7ea39fe
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839329"
---
# <a name="move-data-by-using-copy-activity"></a>Kopyalama etkinliği'ni kullanarak veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-data-movement-activities.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-overview.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de kopyalama etkinliği](../copy-activity-overview.md).

## <a name="overview"></a>Genel Bakış
Azure Data Factory'de şirket içi ve bulut arasında veri kopyalamak için kopyalama etkinliğini kullanabilirsiniz veri depoları. Veri kopyalandıktan sonra daha fazla dönüştürülür ve analiz edilebilir. Kopyalama etkinliği, dönüştürme ve iş zekası (BI) ve uygulama tüketimini analiz sonuçları yayımlamak için de kullanabilirsiniz.

![Kopyalama etkinliği rolü](media/data-factory-data-movement-activities/copy-activity.png)

Kopyalama etkinliği desteklenen bir güvenli tarafından güvenilir, ölçeklenebilir, ve [dünya çapında hizmet](#global). Bu makalede, veri taşıma Data Factory ve kopyalama etkinliği hakkında ayrıntılı bilgi sağlar.

İlk olarak, iki bulut veri depoları arasında ve bir şirket içi veri deposu ile bulut veri deposu arasında veri taşıma nasıl gerçekleştirildiğini görelim.

> [!NOTE]
> Etkinlikleri hakkında genel bilgi edinmek için [anlama işlem hatları ve etkinlikler](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>İki bulut veri depoları arasında veri kopyalama
Hem kaynak hem de havuz veri depolarına bulutta olduğunda, kopyalama etkinliği, verileri bir kaynaktan havuza kopyalanacak aşağıdaki aşamaları boyunca gider. Kopyalama etkinliği'ni destekleyen hizmet:

1. Kaynak veri deposundan veri okur.
2. Serileştirme/seri durumundan çıkarma, sıkıştırma/açma, sütun eşleme gerçekleştirir ve dönüştürme yazın. Bunu, giriş veri kümesi, çıktı veri kümesi ve kopyalama etkinliği yapılandırmalarına göre bu işlemleri yapar.
3. Verileri hedef veri deposuna yazar.

Hizmet, veri taşımayı gerçekleştirmek için en uygun bölgeyi otomatik olarak seçer. Bu genellikle bir havuz veri deposuna en yakın bölgedir.

![Bulut buluta kopyalama](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Bir şirket içi veri deposu ile bulut veri deposu arasında veri kopyalama
Güvenli bir şekilde verileri bir şirket içi veri deposu ile bulut veri deposu arasında taşımak için veri yönetimi ağ geçidi, şirket içi makinenize yükleyin. Veri Yönetimi ağ geçidi karma veri taşıma ve işleme sağlayan bir aracısıdır. Aynı makinede kendisini depolayabilir veya ayrı bir makineye erişim veri deposuna sahip olarak yükleyebilirsiniz.

Bu senaryoda, veri yönetimi ağ geçidi serileştirme/seri durumundan çıkarma, sıkıştırma/açma, sütun eşleme gerçekleştirir ve tür dönüştürme. Verileri Azure Data Factory hizmeti geçmez. Bunun yerine, veri yönetimi ağ geçidi hedef deposuna doğrudan verileri yazar.

![Şirket içi-buluta kopyalama](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Bkz: [şirket içi ile bulut arasında veri taşıma veri depoları](data-factory-move-data-between-onprem-and-cloud.md) bir giriş ve gözden geçirme. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) bu aracı hakkında ayrıntılı bilgi için.

Veri Yönetimi ağ geçidi'ni kullanarak Azure Iaas sanal makinelerinde (VM'ler) barındırılan veri / desteklenen veri depoları da taşıyabilirsiniz. Bu durumda, kendi veri deposu veya ayrı bir VM veri deposuna erişim sahip aynı VM'de veri yönetimi ağ geçidi yükleyebilirsiniz.

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimler
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

> [!NOTE] 
> Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız gerekirse Data Factory’de **özel bir etkinliği** kendi veri kopyalama/taşıma mantığınızla kullanın. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> \* taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri
Kopyalama etkinliği için kullanabileceğiniz **olarak dosya kopyalama-olan** iki dosya tabanlı veri depoları arasında atlayabilirsiniz [format bölümünde](data-factory-create-datasets.md) girdi ve çıktı veri kümesi tanımlarında. Verileri verimli bir şekilde tüm serileştirme/seri kaldırma kopyalanır.

Kopyalama etkinliği, ayrıca okur ve belirtilen biçimde dosyaları Yazar: **Metin, JSON, Avro, ORC ve Parquet**ve sıkıştırma codec **GZip, Deflate, Bzıp2 ve ZipDeflate** desteklenir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) ayrıntılarla.

Örneğin, aşağıdaki kopyalama etkinlikleri yapabilirsiniz:

* Şirket içi SQL Server verileri kopyalayın ve Azure Data Lake Store için ORC biçiminde yazmak.
* Dosyaları (CSV) metin biçiminde şirket içi dosya sisteminden kopyalama ve Azure Blob Avro biçiminde yazmak.
* Şirket içi dosya sisteminden sıkıştırılmış dosyaları kopyalayın ve ardından land Azure Data Lake Store için açılamadı.
* Verileri Azure Blobundan GZip sıkıştırılmış metni (CSV) biçiminde kopyalayın ve Azure SQL veritabanı'na yazın.

## <a name="global"></a>Dünya çapında veri taşıma
Azure Data Factory yalnızca Batı ABD, Doğu ABD ve Kuzey Avrupa bölgelerinde kullanılabilir. Ancak, kopyalama etkinliği'ni destekleyen hizmet genel olarak aşağıdaki bölgelere ve coğrafyalara kullanılabilir. Dünya çapında topolojisi genellikle bölgeler arası atlama önler verimli veri taşıma sağlar. Bkz: [bölgelere göre Hizmetler](https://azure.microsoft.com/regions/#services) Data Factory veri taşıma bir bölge ve kullanılabilirlik için.

### <a name="copy-data-between-cloud-data-stores"></a>Bulut veri depoları arasında veri kopyalama
Hem kaynak hem de havuz veri depolarına bulutta olduğunda, Data Factory hizmeti dağıtımı verileri taşımak için aynı coğrafyadaki havuza en yakın olan bölgeyi kullanır. Eşleme için aşağıdaki tabloya bakın:

| Hedef veri depolarını coğrafyası | Hedef veri deposunun bölgesi | Veri taşıma için kullanılan bölge |
|:--- |:--- |:--- |
| Amerika Birleşik Devletleri | East US | East US |
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
| Güney Kore | Kore Orta | Kore Orta |
| &nbsp; | Kore Güney | Kore Orta |

Alternatif olarak, açıkça belirterek kopyalama işlemini gerçekleştirmek için kullanılacak Data Factory hizmetinin bölgeyi belirtebilirsiniz `executionLocation` kopyalama etkinliği'nin altındaki özelliğini `typeProperties`. Bu özellik için desteklenen değerler listelenmiştir yukarıda **bölge veri taşıma için kullanılan** sütun. Verilerinizi bu bölge kopyalama sırasında kablo üzerinden geçer unutmayın. Örneğin arasında kopyalamak için belirtebileceğiniz depolayan Kore'de Azure `"executionLocation": "Japan East"` Japonya bölge üzerinden yönlendirmek için (bkz [JSON örneği](#by-using-json-scripts) başvuru olarak).

> [!NOTE]
> Hedef veri deposunun bölgesi, önceki listede değil veya tespit edilemezse varsayılan olarak kopyalama etkinliği farklı bir bölge gitmek yerine sürece başarısız olursa `executionLocation` belirtilir. Zaman içinde desteklenen bölge listesi genişletilecek.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Bir şirket içi veri deposu ile bulut veri deposu arasında veri kopyalama
Ne zaman veri şuraya kopyalanıyor şirket içinde (veya Azure sanal makineler/Iaas) arasında ve bulut depolarına [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) bir şirket içi veya sanal makinede veri hareketini gerçekleştirir. Veri Hizmeti, bulut üzerinden akmaz kullanılmadıkça [kopyalama aşamalı](data-factory-copy-activity-performance.md#staged-copy) yeteneği. Bu durumda, havuz veri deposuna yazılmadan önce verileri hazırlama Azure Blob Depolama akar.

## <a name="create-a-pipeline-with-copy-activity"></a>Kopyalama etkinliği ile işlem hattı oluşturma
Çeşitli şekillerde kopyalama etkinliği ile işlem hattı oluşturabilirsiniz:

### <a name="by-using-the-copy-wizard"></a>Kopyalama Sihirbazı'nı kullanarak
Data Factory Kopyalama Sihirbazı, kopyalama etkinliği ile işlem hattı oluşturmak için yardımcı olur. Bu işlem hattı veri desteklenen kaynaklardan hedeflere kopyalamanıza olanak sağlayan *JSON yazmadan* bağlı hizmetler, veri kümeleri ve işlem hatları için tanımlar. Bkz: [Data Factory Kopyalama Sihirbazı](data-factory-copy-wizard.md) Sihirbazı hakkında daha fazla ayrıntı için.  

### <a name="by-using-json-scripts"></a>JSON betikleri kullanarak
Data Factory Düzenleyicisi'nde Visual Studio ya da Azure PowerShell (kopyalama etkinliği kullanarak) bir işlem hattı için bir JSON tanımı oluşturmak için kullanabilirsiniz. Ardından, Data Factory'de işlem hattı oluşturmak için dağıtabilirsiniz. Bkz: [Öğreticisi: Bir Azure Data Factory işlem hattında kopyalama etkinliği kullanmak](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) adım adım yönergeleri içeren öğretici.    

JSON özellikleri (örneğin, ad, açıklama, girdi ve çıktı tabloları ve ilkeleri), tüm etkinlik türleri için kullanılabilir. Kullanılabilir özellikler `typeProperties` etkinlik bölümünü her etkinlik türü ile farklılık gösterir.

Kopyalama etkinliği için `typeProperties` bölüm kaynakları türlerine bağlı olarak değişir ve şu havuzlar. Bir kaynak/havuz tıklayın [desteklenen kaynaklar ve havuzlar](#supported-data-stores-and-formats) bölümü, veri deposu için kopyalama etkinliği destekleyen tür özellikleri hakkında bilgi edinin.

Örnek JSON tanımı aşağıda verilmiştir:

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
Etkinlik çalıştığında, çıkış veri kümesinde tanımlanan zamanlamaya belirler (örneğin: **günlük**, frekansı **gün**ve aralık olarak **1**). Etkinliğin bir giriş veri kümesinden veri kopyalar (**kaynak**) için bir çıktı veri kümesi (**havuz**).

Kopyalama etkinliği için birden fazla girdi veri kümesi belirtebilirsiniz. Bunlar, etkinlik çalıştırılmadan önce bağımlılıkları doğrulamak için kullanılır. Ancak, yalnızca ilk veri kümesindeki verileri hedef dataset nesnesine kopyalanır. Daha fazla bilgi için [zamanlama ve yürütme](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Performans ve ayar
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md), Azure Data factory'deki veri taşıma (kopyalama etkinliği) performansını etkileyen önemli faktörlerin açıklar. Ayrıca, iç test sırasında gözlemlenen performans listeler ve kopyalama etkinliği performansı iyileştirmek için çeşitli yollar ele alınmaktadır.

## <a name="fault-tolerance"></a>Hataya dayanıklılık
Varsayılan olarak, veri ve iadesi başarısız kopyalama kopyalama etkinliği durdurur zaman uyumsuz veri kaynağı ve havuz; arasında karşılaştığınız açıkça atlayıp uyumlu satırları ve yalnızca kopya günlüğünü yapılandırabilirsiniz; ancak bu uyumlu bir veri kopyalamayı yapmak için başarılı oldu. Bkz: [kopyalama etkinliği hataya dayanıklılık](data-factory-copy-activity-fault-tolerance.md) hakkında daha fazla bilgi.

## <a name="security-considerations"></a>Güvenlik konuları
Bkz: [güvenlik konuları](data-factory-data-movement-security-considerations.md), verilerinizin güvenliğini sağlamak için Azure Data factory'deki veri taşıma hizmetleri kullanan bir güvenlik altyapısı açıklar.

## <a name="scheduling-and-sequential-copy"></a>Zamanlama ve sıralı kopyalama
Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme işleyişi Data Factory hakkında ayrıntılı bilgi. Birden çok kopyalama işlemleri birbiri ardına sıralı/sıralı bir şekilde çalıştırmak mümkündür. Bkz: [sırayla kopyalamak](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) bölümü.

## <a name="type-conversions"></a>Tür dönüştürmeleri
Farklı veri depolarını farklı yerel tür sistemlerine sahip. Kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türleri için .NET türüne dönüştürün.
2. Bir .NET türünden bir yerel havuz türüne dönüştürün.

Bir .NET türü bir veri deposu için bir yerel tür sisteminden eşleme ilgili veri deposu makalesinde bir. (Desteklenen veri depoları tablosundaki belirli bağlantısına tıklayın). Bu eşlemeler, kopyalama etkinliği uygun dönüştürmeleri gerçekleştirir. böylece tablolarınızı oluşturulurken uygun türlerini belirlemek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kopyalama etkinliği hakkında daha fazla bilgi edinmek için bkz: [verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Bir şirket içi veri deposundan bir bulut veri deposuna veri taşıma hakkında bilgi edinmek için [bulut veri depoları şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).
