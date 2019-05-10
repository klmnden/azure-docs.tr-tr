---
title: Azure-SSIS tümleştirme çalışma zamanının performansını yapılandırma | Microsoft Docs
description: Azure-SSIS tümleştirme çalışma zamanı için yüksek performans özelliklerini yapılandırma hakkında bilgi edinin
services: data-factory
ms.date: 01/10/2018
ms.topic: conceptual
ms.service: data-factory
ms.workload: data-services
author: swinarko
ms.author: sawinark
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: 42c69653a002446552da998320a43730dfdaadf5
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232510"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Yüksek performans için Azure-SSIS tümleştirme çalışma zamanı yapılandırma

Bu makalede, yüksek performanslı bir Azure-SSIS Integration Runtime (IR) yapılandırma açıklanır. Azure-SSIS IR, dağıtmak ve SQL Server Integration Services (SSIS) paketlerini Azure'da çalıştırmak sağlar. Azure-SSIS IR hakkında daha fazla bilgi için bkz: [Integration runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime) makalesi. Dağıtımı ve SSIS paketlerini Azure'da çalıştırma hakkında daha fazla bilgi için bkz. [Lift and shift ile SQL Server Integration Services iş yüklerini buluta](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> Bu makalede, performans sonuçları ve test şirket içi SSIS geliştirme ekibi üyeleri tarafından yapılan gözlemler içerir. Sonuçlar farklılık gösterebilir. Hem maliyeti ve performansı etkileyen yapılandırma ayarlarınızı Sonlandır önce kendi testi gerçekleştirin.

## <a name="properties-to-configure"></a>Özellikleri yapılandırmak için

Bir yapılandırma betiği aşağıdaki bölümü, bir Azure-SSIS tümleştirme çalışma zamanını oluştururken yapılandırabileceğiniz özelliklerini gösterir. Tam PowerShell komut dosyası ve açıklaması için bkz. [dağıtma SQL Server Integration Services paketlerini azure'a](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to max(2 x number of cores, 8) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database with virtual network service endpoints or a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** Integration runtime çalışan düğümü konumudur. Çalışan düğümü, sabit bir bağlantı için bir Azure SQL veritabanı'nda SSIS Kataloğu veritabanını (SSISDB) tutar. Ayarlama **AzureSSISLocation** SSISDB barındıran SQL veritabanı sunucusuyla aynı konuma olanak tanıyan mümkün olduğunca verimli bir şekilde çalışması için tümleştirme çalışma zamanı.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
Data Factory, Azure-SSIS IR dahil olmak üzere aşağıdaki seçeneklerden destekler:
-   Standart\_A4\_v2
-   Standard\_A8\_v2
-   Standart\_D1\_v2
-   Standart\_D2\_v2
-   Standart\_D3\_v2
-   Standart\_D4\_v2
-   Standart\_D2\_v3
-   Standart\_D4\_v3
-   Standard\_D8\_v3
-   Standart\_D16\_v3
-   Standart\_D32\_v3
-   Standart\_D64\_v3
-   Standart\_E2\_v3
-   Standart\_E4\_v3
-   Standart\_E8\_v3
-   Standart\_E16\_v3
-   Standart\_E32\_v3
-   Standard\_E64\_v3

Terim ve kısaltmalarla şirket içi SSIS mühendislik ekibi tarafından testinde, D serisi A serisinden SSIS paketi yürütme için daha uygun gibi görünüyor.

-   D serisinin performansı/fiyat oranı A serisinden daha yüksektir ve v3 serisi performans/fiyat oranını dv2 serisi yüksektir.
-   D serisi için aktarım hızı A serisi ile aynı fiyattan daha yüksek ve aktarım hızı v3 serisinin dv2 serisi ile aynı fiyattan daha yüksek.
-   Azure-SSIS IR v2 serisi düğümler, özel kurulum için uygun değildir. Bu nedenle Lütfen v3 serisi düğümleri kullanın. V2 serisi düğümleri zaten kullanıyorsanız, lütfen mümkün olan en kısa sürede v3 serisi düğümleri kullanmaya geçin.
-   E serisi VM boyutları, diğer makinelere değerinden daha yüksek bellek CPU oranına sağlar bellek için iyileştirilmiş ' dir. Paketiniz çok miktarda bellek gerekiyorsa, E serisi VM seçme düşünebilirsiniz.

### <a name="configure-for-execution-speed"></a>Yürütme hızını için yapılandırma
Çalıştırmak için birçok paketi yok ve hızlı bir şekilde çalıştırmak için paketleri istiyorsanız bilgileri aşağıdaki tabloda senaryonuz için uygun bir sanal makine türünü seçmek için kullanın.

Bu veriler, tek bir çalışan düğümü üzerindeki bir tek bir paket yürütme temsil eder. Paket, 3 milyon kaydı ad ve son ad sütunu ile Azure Blob depolama alanından yükler, bir tam ad sütunu oluşturur ve Azure Blob Depolama'ya 20 karakterden uzun tam ada sahip kayıtları yazar.

![SSIS tümleştirme çalışma zamanı paketi yürütme hızını](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speedV2.png)

### <a name="configure-for-overall-throughput"></a>Toplam aktarım hızı için yapılandırma

Paketleri çalıştırmak için kullanabileceğiniz birçok seçenek mevcuttur ve genel üretilen işi hakkında en çok değer verdiğiniz bilgileri aşağıdaki tabloda senaryonuz için uygun bir sanal makine türünü seçmek için kullanın.

![SSIS tümleştirme çalışma zamanı en fazla toplam aktarım hızı](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughputV2.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** Integration runtime'nın ölçeklenebilirlik ayarlar. Integration runtime'nın aktarım hızı orantılıdır **AzureSSISNodeNumber**. Ayarlama **AzureSSISNodeNumber** küçük bir değere ilk Integration runtime'nın aktarım hızını izleyin, sonra senaryonuz için değeri ayarlayın. Çalışan düğümü sayısı yapılandırmak için bkz: [bir Azure-SSIS tümleştirme çalışma zamanını yönetme](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Paketleri çalıştırmak için zaten bir güçlü çalışan düğümü kullanırken, artan **AzureSSISMaxParallelExecutionsPerNode** Integration runtime'nın genel üretilen işi artırabilir. Standard_D1_v2 düğümlerde, düğüm başına 1-4 Paralel yürütme desteklenir. Tüm diğer türleri düğüm için düğüm başına 1-max(2 x number of cores, 8) paralel yürütme desteklenir. İsterseniz **AzureSSISMaxParallelExecutionsPerNode** biz desteklenen en büyük değer, bir destek bileti açabilirsiniz ve biz güncelleştirmek için Azure Powershell kullanma gereken en yüksek değer ve sonra artırabilir  **AzureSSISMaxParallelExecutionsPerNode**.
Paketinizi ve çalışan düğümleri için aşağıdaki yapılandırmaları maliyeti temel alarak uygun değeri tahmin edebilirsiniz. Daha fazla bilgi için [genel amaçlı sanal makine boyutları](../virtual-machines/windows/sizes-general.md).

| Boyutlandır             | vCPU | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standart\_D1\_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 2/2x500                         | 2 / 750                                        |
| Standart\_D2\_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 4/4x500                         | 2 / 1500                                       |
| Standart\_D3\_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 8/8x500                         | 4 / 3000                                       |
| Standart\_D4\_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 16/16x500                       | 8 / 6000                                       |
| Standart\_A4\_v2 | 4    | 8           | 40                     | 4000/80/40                                             | 8/8x500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000/160/80                                            | 16/16x500                       | 8 / 2000                                       |
| Standart\_D2\_v3 | 2    | 8           | 50                     | 3000/46/23                                             | 4 / 6 x 500                         | 2 / 1000                                       |
| Standart\_D4\_v3 | 4    | 16          | 100                    | 6000/93/46                                             | 8 / 12 x 500                        | 2 / 2000                                       |
| Standard\_D8\_v3 | 8    | 32          | 200                    | 12000/187/93                                           | 16 / 24 x 500                       | 4 / 4000                                       |
| Standart\_D16\_v3| 16   | 64          | 400                    | 24000/375/187                                          | 32 / 48 x 500                        | 8 / 8000                                       |
| Standart\_D32\_v3| 32   | 128         | 800                    | 48000/750/375                                          | 32 / 96 x 500                       | 8 / 16000                                      |
| Standart\_D64\_v3| 64   | 256         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192 x 500                      | 8 / 30000                                      |
| Standart\_E2\_v3 | 2    | 16          | 50                     | 3000/46/23                                             | 4 / 6 x 500                         | 2 / 1000                                       |
| Standart\_E4\_v3 | 4    | 32          | 100                    | 6000/93/46                                             | 8 / 12 x 500                        | 2 / 2000                                       |
| Standart\_E8\_v3 | 8    | 64          | 200                    | 12000/187/93                                           | 16 / 24 x 500                       | 4 / 4000                                       |
| Standart\_E16\_v3| 16   | 128         | 400                    | 24000/375/187                                          | 32 / 48 x 500                       | 8 / 8000                                       |
| Standart\_E32\_v3| 32   | 256         | 800                    | 48000/750/375                                          | 32 / 96 x 500                       | 8 / 16000                                      |
| Standard\_E64\_v3| 64   | 432         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192 x 500                      | 8 / 30000                                      |

Doğru değeri ayarlamak için yönergeler şunlardır **AzureSSISMaxParallelExecutionsPerNode** özelliği: 

1. Başta küçük bir değere ayarlayın.
2. Bu hizmetin genel performansını geliştirilmiş olup olmadığını denetlemek için küçük bir miktarda artırır.
3. Hizmetin genel performansını en büyük değere ulaştığında değeri artırmak durdurun.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** fiyatlandırma katmanı için bir Azure SQL veritabanı'nda SSIS Kataloğu veritabanını (SSISDB). Bu ayar, yürütme günlüğü yüklemek için çalışanların IR örneği, bir paket yürütme kuyruğuna hızını ve hızı maksimum sayısını etkiler.

-   Hakkında kuyruk paket yürütme ve yürütme günlüğü yüklemek için hızlı umursamaz, en düşük veritabanı fiyatlandırma katmanı seçebilirsiniz. Temel fiyatlandırma ile Azure SQL veritabanı 8 çalışan bir tümleştirme çalışma zamanı örneğini destekler.

-   8'den fazla çalışan sayısı veya çekirdek sayısı 50'den fazla olan temel değerinden daha güçlü bir veritabanı seçin. Aksi takdirde veritabanı tümleştirme çalışma zamanı örneğin performans sorunu haline gelir ve genel performansını olumsuz şekilde etkilenir.

-   Günlük düzeyi için ayrıntılı olarak ayarlanırsa, s3 gibi daha güçlü bir veritabanı seçin. Terim ve kısaltmalarla şirket içi test işlemlerimizi göre s3 fiyatlandırma katmanına SSIS paketi yürütme 2 düğüm, 128 paralel sayıları ve ayrıntılı günlük kaydı düzeyini destekler.

Veritabanı fiyatlandırma katmanı göre de ayarlayabilirsiniz [veritabanı işlem birimi](../sql-database/sql-database-what-is-a-dtu.md) (DTU) kullanım bilgilerini kullanılabilir Azure portalında.

## <a name="design-for-high-performance"></a>Yüksek performans tasarımı
Azure'da çalışan bir SSIS paketi tasarlama, şirket içi yürütme için bir paket tasarlama öğesinden farklıdır. Aynı paket içindeki birden fazla bağımsız görevi birleştirmek yerine, bunları Azure-SSIS IR'yi daha verimli yürütme için çeşitli paketler halinde ayırın Her paket için bir paket yürütme oluşturun, böylece bunlar birbirine için bitmesini beklemek zorunda değilsiniz. Bu yaklaşım, Azure-SSIS Integration runtime'nın ölçeklenebilirlik yararlanır ve hizmetin genel performansını artırır.

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS tümleştirme çalışma zamanı hakkında daha fazla bilgi edinin. Bkz: [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime).
