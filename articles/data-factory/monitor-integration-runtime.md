---
title: Azure Data factory'deki tümleştirme çalışma zamanını izleme | Microsoft Docs
description: Azure Data factory'de tümleştirme çalışma zamanı farklı türde izlemeyi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/25/2018
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: b62cbe75730da8c5764839d41887deb7e6cd0e90
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122652"
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Azure Data factory'deki tümleştirme çalışma zamanı izleme  
**Integration runtime** , farklı ağ ortamlarında çeşitli veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır. Tümleştirme çalışma zamanları Data Factory tarafından sunulan üç tür vardır:

- Azure tümleştirme çalışma zamanı
- Kendinden konak tümleştirme çalışma zamanı
- Azure SSIS tümleştirmesi çalışma zamanı

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Integration runtime (IR) örneğini durumunu almak için aşağıdaki PowerShell komutunu çalıştırın: 

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

Cmdlet, Integration runtime'nın farklı türleri için farklı bilgi döndürür. Bu makalede, her tür tümleştirme çalışma zamanı durumlarını ve özellikler açıklanmaktadır.  

## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı
Azure tümleştirme çalışma zamanı için işlem kaynağı, esnek bir şekilde Azure'da tam olarak yönetilir. Aşağıdaki tabloda tarafından döndürülen özellikleri için açıklamalar verilmiştir **Get-AzDataFactoryV2IntegrationRuntime** komutu:

### <a name="properties"></a>Özellikler
Azure tümleştirme çalışma zamanı için cmdlet tarafından döndürülen özelliklerin açıklamaları aşağıdaki tabloda verilmiştir:

| Özellik | Açıklama |
-------- | ------------- | 
| Ad | Azure tümleştirme çalışma zamanı adı. |  
| Eyalet | Azure tümleştirme çalışma zamanı durumu. | 
| Location | Azure tümleştirme çalışma zamanının konumu. Bir Azure Integration runtime'nın konumu hakkında daha fazla ayrıntı için bkz: [tümleştirme çalışma zamanı giriş](concepts-integration-runtime.md). |
| DataFactoryName | Azure tümleştirme çalışma zamanı ait data factory adı. | 
| ResourceGroupName | Veri fabrikasına ait kaynak grubunun adı.  |
| Açıklama | Integration runtime'nın açıklaması.  |

### <a name="status"></a>Durum
Aşağıdaki tabloda, bir Azure Integration runtime'nın olası durumlar sağlar:

| Durum | Yorumlar/senaryoları | 
| ------ | ------------------ |
| Çevrimiçi | Azure tümleştirme çalışma zamanı, çevrimiçi ve kullanılmaya hazır durumda. | 
| Çevrimdışı | Azure tümleştirme çalışma zamanının bir iç hata nedeniyle çevrimdışı kalır. |

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı
Bu bölümde Get-AzDataFactoryV2IntegrationRuntime cmdlet'i tarafından döndürülen özellikleri için açıklamalar sağlanır. 

> [!NOTE] 
> Geri döndürülen özelliklerin ve durumu genel şirket içinde barındırılan tümleştirme çalışma zamanı ve çalışma zamanında her bir düğüm hakkında bilgi içerir.  

### <a name="properties"></a>Özellikler

Aşağıdaki tabloda, izleme özellikleri için açıklamalar verilmiştir **her düğüm**:

| Özellik | Açıklama | 
| -------- | ----------- | 
| Ad | Şirket içinde barındırılan tümleştirme çalışma zamanı ve onunla ilişkili düğümü adı. Şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olan bir şirket içi Windows makine düğümüdür. |  
| Durum | Genel şirket içinde barındırılan tümleştirme çalışma zamanını ve her düğüm durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vb. Bu durumlar hakkında daha fazla bilgi için sonraki bölüme bakın. | 
| Version | Şirket içinde barındırılan tümleştirme çalışma zamanı ve her düğüm sürümü. Şirket içinde barındırılan tümleştirme çalışma zamanı sürümü grubunda düğüm çoğunluğu sürümüne göre belirlenir. Şirket içinde barındırılan tümleştirme çalışma zamanı kurulumu ile ilgili farklı sürümlerini düğüm varsa, yalnızca mantıksal olarak aynı sürüm numarası ile tümleştirme çalışma zamanı işlevi düzgün bir şekilde şirket içinde barındırılan. Başkalarının sınırlı modundaki ve (yalnızca otomatik güncelleştirme başarısız olursa) el ile güncelleştirilmesi gerekir. | 
| Kullanılabilir bellek | Şirket içinde barındırılan Integration runtime düğümü kullanılabilir bellek. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. | 
| CPU kullanımı | Şirket içinde barındırılan Integration runtime düğümü, CPU kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. |
| Ağ (daraltma/genişletme) | Şirket içinde barındırılan Integration runtime düğümü, ağ kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. | 
| (Çalışan / sınırlama) eşzamanlı işleri | **Çalışan**. İşleri veya her bir düğümde çalışan görevler sayısı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. <br/><br/>**Sınırı**. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değer, makine boyutuna bağlı olarak tanımlanır. Sınır CPU, bellek veya ağ altında kullanılan olsa bile çıkış etkinliklerini zamanlama sırasında eş zamanlı iş yürütme Gelişmiş senaryolarda ölçeğini artırabilirsiniz. Bu özellik, bir tek düğümlü şirket içinde barındırılan tümleştirme çalışma zamanı ile de kullanılabilir. |
| Rol | Dağıtıcı ve çalışan rolleri bir çok düğümlü şirket içinde barındırılan tümleştirme çalışma zamanı – iki tür vardır. Tüm düğümleri, yani tüm işleri yürütmek için kullanılabilirler çalışanlardır. Görevler/işleri bulut hizmetlerinden çekme ve bunları farklı çalışan düğümlerine dağıtmak için kullanılan tek bir dağıtıcı düğüm yok. Dağıtıcı ayrıca bir çalışan düğümü düğümüdür. |

Şirket içinde barındırılan tümleştirme çalışma zamanı'nda iki veya daha fazla düğüm olduğunda bazı ayarları özelliklerinin daha anlamlı (diğer bir deyişle, senaryo ölçeklendirme).

#### <a name="concurrent-jobs-limit"></a>Eşzamanlı iş sınırı

Sınır eşzamanlı iş varsayılan değeri makine boyutuna göre. Bu değer hesaplamak için kullanılan faktörleri RAM miktarını ve makinenin CPU çekirdeği sayısını bağlıdır. Bu nedenle daha fazla çekirdek ve daha fazla bellek, daha yüksek eş zamanlı işlerinin varsayılan sınırlayın.

Düğüm sayısını artırarak ölçeği. Eşzamanlı iş sınırı düğüm sayısını artırdığınızda, kullanılabilir tüm düğümlerden eş zamanlı iş sınırı değerlerinin toplamıdır.  Bir düğüm, en fazla on iki eşzamanlı iş çalıştırmanıza olanak tanır, örneğin, sonra üç daha benzer düğüm eklemeyi, en fazla 48 eşzamanlı işler (4 x 12) çalıştırmanıza olanak tanır. Her bir düğümde düşük kaynak kullanımını varsayılan değerlerle gördüğünüzde eşzamanlı iş sınırı artırmak öneririz.

Azure portalında hesaplanan varsayılan değeri geçersiz kılabilirsiniz. Yazar seçin > bağlantılar > tümleştirme çalışma zamanları > Düzenle > düğümleri > düğüm başına eşzamanlı iş değerini değiştirin. PowerShell de kullanabilirsiniz [güncelleştirme Azdatafactoryv2integrationruntimenode](https://docs.microsoft.com/powershell/module/az.datafactory/update-Azdatafactoryv2integrationruntimenode#examples) komutu.
  
### <a name="status-per-node"></a>Durum (her düğüm)
Aşağıdaki tabloda, şirket içinde barındırılan tümleştirme çalışma zamanı düğümü olası durumlar sağlar:

| Durum | Açıklama |
| ------ | ------------------ | 
| Çevrimiçi | Düğüm bağlı Data Factory hizmetinde yayımlayın. |
| Çevrimdışı | Düğümü çevrimdışı durumda. |
| Yükseltme | Düğüm, otomatik olarak güncelleştirilir. |
| Sınırlı | Bir bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, service bus bağlantı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir. |
| Etkin değil | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümüdür. |

Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir.

### <a name="status-overall-self-hosted-integration-runtime"></a>Durum (genel şirket içinde barındırılan tümleştirme çalışma zamanı)
Aşağıdaki tabloda, şirket içinde barındırılan Integration runtime'nın olası durumlar sağlar. Bu durum çalışma zamanının ait tüm düğümlerin durumları bağlıdır. 

| Durum | Açıklama |
| ------ | ----------- | 
| Kayıt gerekiyor | Hiçbir düğüm bu şirket içinde barındırılan tümleştirme çalışma zamanı için henüz kayıtlı. |
| Çevrimiçi | Tüm düğümlerin çevrimiçi değil. |
| Çevrimdışı | Hiçbir düğümü çevrimiçi değil. |
| Sınırlı | Bu şirket içinde barındırılan tümleştirme çalışma zamanındaki tüm düğümleri iyi durumda olan. Bu durum bazı düğümler, çalışmıyor durumda bir uyarıdır. Bu durum, dağıtıcı/çalışan düğümüyle bir kimlik bilgisi eşitleme sorunu nedeniyle olabilir. |

Kullanım **Get-AzDataFactoryV2IntegrationRuntimeMetric** ayrıntılı içeren JSON yükü getirilecek cmdlet'i şirket içinde barındırılan tümleştirme çalışma zamanı özellikleri ve bunların anlık görüntü cmdlet'inin sırada değerleri.

```powershell
Get-AzDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName  | | ConvertTo-Json 
```

Örnek çıktı: (Bu şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili iki düğüm olduğunu varsayar)

```json
{
    "IntegrationRuntimeName":  "<Name of your integration runtime>",
    "ResourceGroupName":  "<Resource Group Name>",
    "DataFactoryName":  "<Data Factory Name>",
    "Nodes":  [
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        },
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        }

    ]
} 
```


## <a name="azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı
Azure-SSIS tümleştirme çalışma zamanı SSIS paketlerinizi çalıştırmaya ayrılmış Azure sanal makineleri (veya düğümler) tam olarak yönetilen bir kümesidir. Azure Data Factory diğer etkinlikleri çalıştırmaz. Sağlanan sonra özelliklerini sorgulama ve kendi genel/düğüm-özel durumları izleyin.

### <a name="properties"></a>Özellikler

| Özellik/durumu | Açıklama |
| --------------- | ----------- |
| CreateTime | Azure-SSIS tümleştirme çalışma zamanınızın oluşturulduğu UTC saati. |
| Düğümler | Azure-SSIS Integration runtime düğümü özel durumları (başlatma/kullanılabilir/geri dönüştürme/kullanılamıyor) ve işlem yapılabilir hataları ile ayrılan/kullanılabilir düğümleri. |
| OtherErrors | Eyleme dönüştürülebilir düğüme özgü olmayan hatalar, Azure-SSIS tümleştirme çalışma zamanı üzerinde. |
| LastOperation | Son Başlatma/durdurma işlemi başarısız olursa, işlem yapılabilir hataları sahip, Azure-SSIS tümleştirme çalışma zamanı sonucu. |
| Eyalet | Genel durumunu (ilk/başlangıç/başlangıç/durdurma/durduruldu), Azure-SSIS tümleştirme çalışma zamanı. |
| Location | Azure-SSIS tümleştirme çalışma zamanınızın konumu. |
| NodeSize | Azure-SSIS Integration runtime'nın her bir düğümün boyutu. |
| NodeCount | Azure-SSIS Integration runtime düğüm sayısı. |
| MaxParallelExecutionsPerNode | Azure-SSIS Integration runtime düğümü başına paralel yürütmelerinin sayısı. |
| CatalogServerEndpoint | SSISDB'yi barındırmak için var olan Azure SQL veritabanı/yönetilen örnek sunucunuzun uç nokta. |
| CatalogAdminUserName | Mevcut Azure SQL veritabanı/yönetilen örnek sunucunuza yönetici kullanıcı adı. Data Factory hizmeti, hazırlama ve sizin adınıza SSISDB yönetmek için bu bilgileri kullanır. |
| CatalogAdminPassword | Mevcut Azure SQL veritabanı/yönetilen örnek sunucunuza yönetici parolası. |
| CatalogPricingTier | Mevcut Azure SQL veritabanı sunucunuz tarafından barındırılan SSISDB için fiyatlandırma katmanı.  Azure SQL veritabanı yönetilen SSISDB barındırma örneği için geçerli değildir. |
| VNetId | Katılmak için sanal ağ kaynak kimliği, Azure-SSIS tümleştirme çalışma zamanı. |
| Alt ağ | Katılmak, Azure-SSIS tümleştirme çalışma zamanı için alt ağ adı. |
| Kimlik | Azure-SSIS tümleştirme çalışma zamanınızın kaynak kimliği. |
| Tür | Türü (yönetilen/Self-Hosted), Azure-SSIS tümleştirme çalışma zamanı. |
| ResourceGroupName | Data factory ve Azure-SSIS tümleştirme çalışma zamanı oluşturulduğu, Azure kaynak grubu adı. |
| DataFactoryName | Azure data factory'nizi adı. |
| Ad | Azure-SSIS tümleştirme çalışma zamanınızın adını. |
| Açıklama | Azure-SSIS Integration runtime'nın açıklaması. |

  
### <a name="status-per-node"></a>Durum (her düğüm)

| Durum | Açıklama |
| ------ | ----------- | 
| Başlatılıyor | Bu düğüm hazırlanıyor. |
| Kullanılabilir | Bu düğüm, SSIS paketlerini dağıtma/yürütme için hazırdır. |
| Geri dönüştürme | Onarılan/yeniden olan bu düğümüdür. |
| Kullanılamaz | Bu düğüm, SSIS paketlerini dağıtma/yürütme için hazır değil ve işlem yapılabilir hataları/çözümlenmesi sorunları vardır. |

### <a name="status-overall-azure-ssis-integration-runtime"></a>Durum (genel Azure-SSIS tümleştirme çalışma zamanı)

| Genel durum | Açıklama | 
| -------------- | ----------- | 
| İlk | Azure-SSIS Integration runtime düğümünde ayrılan ve hazırlanmış verilmemiş. | 
| Başlatılıyor | Ayrılmış ve hazırlanan, Azure-SSIS tümleştirme çalışma zamanı düğümleri ve faturalandırma başlatıldı. |
| Başlatıldı | Azure-SSIS Integration runtime düğümünde ayrılan ve hazır olan ve bunlar SSIS paketlerini dağıtma/yürütme hazır. |
| Durduruluyor  | Azure-SSIS Integration runtime düğümünde serbest bırakılır. |
| Durduruldu | Azure-SSIS Integration runtime düğümünde yayımlanmıştır ve faturalandırma durdurdu. |

### <a name="monitor-the-azure-ssis-integration-runtime-in-the-azure-portal"></a>Azure portalında Azure-SSIS tümleştirme çalışma zamanı izleme

Aşağıdaki ekran görüntüleri, izlemek ve örnek görüntülenen bilgileri olarak sağlamak için Azure-SSIS IR seçilecek gösterilmektedir.

![İzlemek için Azure-SSIS Integration runtime'ı seçin](media/monitor-integration-runtime/monitor-azure-ssis-ir-image1.png)

![Azure-SSIS tümleştirme çalışma zamanının hakkındaki bilgileri görüntüleme](media/monitor-integration-runtime/monitor-azure-ssis-ir-image2.png)

### <a name="monitor-the-azure-ssis-integration-runtime-with-powershell"></a>PowerShell ile Azure-SSIS tümleştirme çalışma zamanı izleme

Azure-SSIS IR'yi durumunu denetlemek için aşağıdaki örnekte olduğu gibi bir betik kullan

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Status
```

### <a name="more-info-about-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı hakkında daha fazla bilgi

Azure-SSIS tümleştirme çalışma zamanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure-SSIS IR'yi genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale öğreticiyi genişletip ve Azure SQL veritabanı yönetilen örneği kullanma ve IR'yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure-SSIS IR'nin sanal ağa katılmasını sanal ağı yapılandırmak için Azure portalını kullanma adımları sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
İşlem hatları farklı şekilde izlemek için aşağıdaki makalelere bakın: 

- [Hızlı Başlangıç: veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md).
- [Data Factory işlem hatlarını izlemek için Azure İzleyicisi'ni kullanın](monitor-using-azure-monitor.md)
