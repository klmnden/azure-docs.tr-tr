---
title: Azure Data Factory tümleştirme çalışma zamanında izleme | Microsoft Docs
description: Farklı Azure Data Factory içinde Integration zamanının izlemek öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/23/2017
ms.author: douglasl
ms.openlocfilehash: 523d50623257d3944342cb174174e27bd4731248
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045254"
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Azure Data Factory bir tümleştirme çalışma zamanı izleme  
**Tümleştirme çalışma zamanı** olan farklı ağ ortamlar genelinde çeşitli veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan bilgi işlem altyapısı. Veri fabrikası tarafından sunulan tümleştirme çalışma zamanları üç tür vardır:

- Azure tümleştirme çalışma zamanı
- Kendinden konak tümleştirme çalışma zamanı
- Azure SSIS tümleştirmesi çalışma zamanı

Tümleştirme çalışma zamanı (IR) örneği durumunu almak için aşağıdaki PowerShell komutunu çalıştırın: 

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

Cmdlet Integration zamanının farklı türleri için farklı bilgiler döndürür. Bu makalede, her tür tümleştirmesi çalışma zamanı durumlarını ve özellikleri açıklanmaktadır.  

## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı
Bir Azure tümleştirme çalışma zamanı için işlem kaynağı tamamen Azure'da özellikler esnek yönetilir. Aşağıdaki tabloda tarafından döndürülen özellikleri için açıklamalar sağlanır **Get-AzureRmDataFactoryV2IntegrationRuntime** komutu:

### <a name="properties"></a>Özellikler
Aşağıdaki tabloda Azure tümleştirme çalışma zamanı için cmdlet tarafından döndürülen özelliklerin açıklamaları verilmiştir:

| Özellik | Açıklama |
-------- | ------------- | 
| Ad | Azure tümleştirme çalışma zamanı adı. |  
| Durum | Azure tümleştirme çalışma zamanı durumu. | 
| Konum | Azure Integration zamanının konumu. Bir Azure Integration zamanının konumu hakkında daha fazla ayrıntı için bkz: [tümleştirmesi çalışma zamanı giriş](concepts-integration-runtime.md). |
| DataFactoryName | Azure tümleştirme çalışma zamanı ait veri fabrikası adı. | 
| ResourceGroupName | Veri Fabrikası ait kaynak grubunun adı.  |
| Açıklama | Tümleştirme çalışma zamanı açıklaması.  |

### <a name="status"></a>Durum
Aşağıdaki tabloda olası durumlar bir Azure Integration zamanının sağlar:

| Durum | Yorumlar/senaryoları | 
| ------ | ------------------ |
| Çevrimiçi | Azure tümleştirme çalışma zamanı, çevrimiçi ve kullanılacak hazır olur. | 
| Çevrimdışı | Bir iç hata nedeniyle Azure tümleştirme çalışma zamanı çevrimdışıdır. |

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı
Bu bölümde, Get-AzureRmDataFactoryV2IntegrationRuntime cmdlet tarafından döndürülen özellikleri için açıklamalar sağlanır. 

> [!NOTE] 
> Geri döndürülen özelliklerin ve durum Genel kendini barındıran tümleştirmesi çalışma zamanı ve çalışma zamanında her düğüm hakkındaki bilgileri içerir.  

### <a name="properties"></a>Özellikler

Aşağıdaki tabloda izleme özellikleri için açıklamalar sağlanır **her düğüm**:

| Özellik | Açıklama | 
| -------- | ----------- | 
| Ad | Kendini barındıran tümleştirmesi çalışma zamanı ve ilişkili düğüm adı. Kendini barındıran tümleştirmesi çalışma zamanı yüklü olan bir şirket içi Windows makine düğümdür. |  
| Durum | Genel kendini barındıran tümleştirmesi çalışma zamanı ve her düğüm durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vs. Bu durumlar hakkında daha fazla bilgi için sonraki bölüme bakın. | 
| Sürüm | Kendini barındıran tümleştirmesi çalışma zamanı ve her düğüm sürümü. Kendini barındıran tümleştirmesi çalışma zamanı sürümü grubu düğüm çoğunluğu sürümüne göre belirlenir. Kendini barındıran tümleştirmesi çalışma zamanı kurulumunda farklı sürümleri ile düğüm varsa, yalnızca mantıksal olarak aynı sürüm numarasına sahip tümleştirme çalışma zamanı işlevi düzgün kendi kendini barındırır. Başkalarının sınırlı modda ve (yalnızca otomatik güncelleştirmeler başarısız olursa) el ile güncelleştirilmesi gerekir. | 
| Uygun bellek | Kendini barındıran tümleştirmesi çalışma zamanı düğüm kullanılabilir bellek. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. | 
| CPU kullanımı | Bir kendi kendini barındıran tümleştirmesi çalışma zamanı düğümün CPU kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. |
| Ağ (çıkış) | Kendini barındıran tümleştirmesi çalışma zamanı düğümünün ağ kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. | 
| (Çalışan / sınırlamak) eşzamanlı işleri | İşlerini veya her bir düğümde çalışan görevlerin sayısıdır. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değeri makine boyutuna göre tanımlanır. Eşzamanlı iş yürütme burada altında kullanılan CPU/bellek/ağ, ancak etkinlikler zaman aşımına uğrama Gelişmiş senaryolarda ölçeklendirin sınırı artırabilirsiniz. Bu özellik, tek bir düğüm kendini barındıran tümleştirmesi çalışma zamanı ile de kullanılabilir. |
| Rol | Dağıtıcı ve çalışan rolleri bir çok düğümlü kendini barındıran tümleştirme çalışma zamanında – iki tür vardır. Tüm düğümleri, tüm işleri yürütmek için kullanılabilmesi için başka bir deyişle, çalışanlardır. Görevler/işleri bulut hizmetlerinden çekmek ve bunları farklı çalışan düğümlerine gönderme için kullanılan tek bir dağıtıcı düğüm yok. Dağıtıcı düğüm ayrıca bir çalışan olandır. |

Kendini barındıran tümleştirmesi çalışma zamanı'nda iki veya daha fazla düğüm (senaryo genişletme) olduğunda özelliklerinin bazı ayarları daha anlamlı. 
  
### <a name="status-per-node"></a>Durum (her düğüm)
Aşağıdaki tabloda olası durumlar bir kendi kendini barındıran tümleştirmesi çalışma zamanı düğümün sağlar:

| Durum | Açıklama |
| ------ | ------------------ | 
| Çevrimiçi | Düğümü veri fabrikası hizmetine bağlı. |
| Çevrimdışı | Çevrimdışı düğümdür. |
| Yükseltiliyor | Düğüm, otomatik olarak güncelleştirilir. |
| Sınırlı | Bir bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, hizmet veri yolu bağlantı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir. |
| Devre dışı | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümdür. |

Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir.

### <a name="status-overall-self-hosted-integration-runtime"></a>Durum (genel kendini barındıran tümleştirmesi çalışma zamanı)
Aşağıdaki tabloda olası durumlar kendini barındıran Integration zamanının sağlar. Bu durum çalışma ait tüm düğümlerin durumları bağlıdır. 

| Durum | Açıklama |
| ------ | ----------- | 
| Kayıt gerekiyor | Hiçbir düğümü bu kendini barındıran tümleştirmesi çalışma zamanı henüz kayıtlı değil. |
| Çevrimiçi | Tüm düğümlerde çevrimiçidir. |
| Çevrimdışı | Hiçbir düğümü çevrimiçi değil. |
| Sınırlı | Tüm düğümleri bu kendini barındıran tümleştirme çalışma zamanında sağlıklı bir durumda. Bu durum bazı düğümler aşağı olabilecek bir uyarıdır. Bu durum, dağıtıcı/çalışan düğümünde bir kimlik bilgisi eşitleme sorunu nedeniyle olabilir. |

Kullanım **Get-AzureRmDataFactoryV2IntegrationRuntimeMetric** ayrıntılı içeren JSON yükü getirmek için cmdlet kendi kendini barındıran tümleştirmesi çalışma zamanı özelliklerini ve bunların anlık görüntü değerleri yürütme zamanı sırasında cmdlet'ini kullanın.

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName  | | ConvertTo-Json 
```

Örnek çıktı: (iki düğüm bu kendini barındıran tümleştirmesi çalışma zamanı ile ilişkili olduğunu varsayar)

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
Azure SSIS tümleştirmesi çalışma zamanı SSIS paketleri çalıştırmak için ayrılmış Azure sanal makineleri (veya düğümler) tam olarak yönetilen bir kümedir. Azure Data Factory diğer etkinlikleri çalışmaz. Sağlanan sonra özelliklerini sorgulamak ve onun düğümü/genel-özel durumları izleyin.

### <a name="properties"></a>Özellikler

| Özellik/durumu | Açıklama |
| --------------- | ----------- |
| CreateTime | Azure SSIS tümleştirmesi Çalışma Zamanı Modülü oluşturulduğu UTC saati. |
| Düğümler | Düğüm özel durumları (başlatma/kullanılabilir/geri dönüştürme/kullanılamaz) ve tıklatılabilir hataları ile Azure SSIS Integration zamanının ayrılan/kullanılabilir düğümler. |
| OtherErrors | Düğüm özel tıklatılabilir hatalarda Azure SSIS tümleştirmesi Çalışma Zamanı Modülü. |
| LastOperation | Son Başlatma/durdurma işlemi başarısız olursa tıklatılabilir hatalarla ile Azure SSIS tümleştirme çalışma zamanında sonucu. |
| Durum | Genel durumunu (ilk/başlangıç/başlangıç/durdurma/durduruldu), Azure SSIS tümleştirmesi çalışma zamanı. |
| Konum | Azure SSIS tümleştirmesi Çalışma Zamanı Modülü konumu. |
| NodeSize | Her düğüm, Azure SSIS Integration zamanının boyutu. |
| NodeCount | Azure SSIS tümleştirme çalışma zamanındaki düğüm sayısı. |
| MaxParallelExecutionsPerNode | Paralel yürütmeleri Azure SSIS tümleştirme çalışma zamanındaki düğüm başına sayısı. |
| CatalogServerEndpoint | Mevcut Azure SQL veritabanı/yönetilen örneği (Önizleme) sunucunuzun konak SSISDB için uç nokta. |
| CatalogAdminUserName | Mevcut Azure SQL veritabanı/yönetilen örneği (Önizleme) sunucunuzu yönetici kullanıcı adı. Data Factory Hizmeti'ne hazırlamak ve sizin adınıza SSISDB yönetmek için bu bilgileri kullanır. |
| CatalogAdminPassword | Mevcut Azure SQL veritabanı/yönetilen örneği (Önizleme) sunucunuzun yönetici parolası. |
| CatalogPricingTier | Mevcut Azure SQL veritabanı sunucunuz tarafından barındırılan SSISDB için fiyatlandırma katmanı.  Azure SQL örneğine yönetilen SSISDB barındırma (Önizleme) geçerli değil. |
| VNetId | Katılmak için sanal ağ kaynak kimliği, Azure SSIS tümleştirmesi çalışma zamanı. |
| Alt ağ | Katılmak, Azure SSIS tümleştirmesi çalışma zamanı için alt ağ adı. |
| Kimlik | Kaynak Kimliği, Azure SSIS Integration zamanının. |
| Tür | Türü (yönetilen/Self-Hosted), Azure SSIS tümleştirmesi çalışma zamanı. |
| ResourceGroupName | Veri Fabrikası ve Azure SSIS tümleştirmesi çalışma zamanı oluşturulduğu, Azure kaynak grubu adı. |
| DataFactoryName | Azure data factory'nizi adı. |
| Ad | Azure SSIS tümleştirmesi Çalışma Zamanı Modülü adı. |
| Açıklama | Azure SSIS tümleştirmesi Çalışma Zamanı Modülü açıklaması. |

  
### <a name="status-per-node"></a>Durum (her düğüm)

| Durum | Açıklama |
| ------ | ----------- | 
| Başlatılıyor | Bu düğüm hazırlanıyor. |
| Kullanılabilir | Bu düğüm SSIS paketleri dağıtmak/yürütmek için hazırdır. |
| Geri dönüştürme | Bu düğüm, onarıldı/yeniden başlatma olan ' dir. |
| Kullanılamaz | Bu düğüm SSIS paketleri dağıtmak/yürütmek hazır değil ve tıklatılabilir hataları/çözümlenmesi sorunları vardır. |

### <a name="status-overall-azure-ssis-integration-runtime"></a>Durum (genel Azure SSIS tümleştirmesi çalışma zamanı)

| Tüm durum | Açıklama | 
| -------------- | ----------- | 
| İlk | Düğümler, Azure SSIS Integration zamanının ayrılmış ve hazırlanan verilmemiş. | 
| Başlatılıyor | Ayrılmış ve hazırlanan Azure SSIS Integration zamanının düğümleri ve faturalama başlatıldı. |
| Başlatıldı | Azure SSIS Integration zamanının düğümleri ayrılmış ve hazır ve SSIS paketleri dağıtmak/yürütmek hazır. |
| Durduruluyor  | Düğümler, Azure SSIS Integration zamanının yayınlanmaktadır. |
| Durduruldu | Azure SSIS Integration zamanının düğümleri kullanıma ve faturalama durduruldu. |

Azure SSIS tümleştirmesi çalışma zamanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (Önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalında Azure SSIS IR sanal ağ katılması sanal ağı yapılandırmak için adımları sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Farklı şekillerde işlem hatlarını izleme için aşağıdaki makalelere bakın: 

- [Hızlı Başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-dot-net.md).
- [Data Factory işlem hatlarını izlemek için Azure İzleyicisi'ni kullanın](monitor-using-azure-monitor.md)
