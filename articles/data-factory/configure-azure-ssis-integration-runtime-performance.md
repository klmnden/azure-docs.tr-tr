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
ms.openlocfilehash: 271da0a6ff443fcee28bc870821f4222b3018c91
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262248"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Yüksek performans için Azure-SSIS tümleştirme çalışma zamanı yapılandırma

Bu makalede, yüksek performanslı bir Azure-SSIS Integration Runtime (IR) yapılandırma açıklanır. Azure-SSIS IR, dağıtmak ve SQL Server Integration Services (SSIS) paketlerini Azure'da çalıştırmak sağlar. Azure-SSIS IR hakkında daha fazla bilgi için bkz: [Integration runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime) makalesi. Dağıtımı ve SSIS paketlerini Azure'da çalıştırma hakkında daha fazla bilgi için bkz. [Lift and shift ile SQL Server Integration Services iş yüklerini buluta](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> Bu makalede, performans sonuçları ve test şirket içi SSIS geliştirme ekibi üyeleri tarafından yapılan gözlemler içerir. Sonuçlar farklılık gösterebilir. Hem maliyeti ve performansı etkileyen yapılandırma ayarlarınızı Sonlandır önce kendi testi gerçekleştirin.

## <a name="properties-to-configure"></a>Özellikleri yapılandırmak için

Bir yapılandırma betiği aşağıdaki bölümü, bir Azure-SSIS tümleştirme çalışma zamanını oluştururken yapılandırabileceğiniz özelliklerini gösterir. Tam PowerShell komut dosyası ve açıklaması için bkz. [dağıtma SQL Server Integration Services paketlerini azure'a](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# Only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# Only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Database Managed Instance
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
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
-   Standart\_D4\_v2.

Terim ve kısaltmalarla şirket içi SSIS mühendislik ekibi tarafından testinde, D serisi A serisinden SSIS paketi yürütme için daha uygun gibi görünüyor.

-   D serisinin performansı/fiyat oranı, A serisi yüksektir.
-   D serisi için aktarım hızı, A serisi ile aynı fiyattan daha yüksektir.

### <a name="configure-for-execution-speed"></a>Yürütme hızını için yapılandırma
Çalıştırmak için birçok paketi yok ve hızlı bir şekilde çalıştırmak için paketleri istiyorsanız bilgileri aşağıdaki tabloda senaryonuz için uygun bir sanal makine türünü seçmek için kullanın.

Bu veriler, tek bir çalışan düğümü üzerindeki bir tek bir paket yürütme temsil eder. Paket adının ilk ve son adı sütunlarla 10 milyon kaydı Azure Blob depolama alanından yükler, bir tam ad sütunu oluşturur ve Azure Blob Depolama'ya 20 karakterden uzun tam ada sahip kayıtları yazar.

![SSIS tümleştirme çalışma zamanı paketi yürütme hızını](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Toplam aktarım hızı için yapılandırma

Paketleri çalıştırmak için kullanabileceğiniz birçok seçenek mevcuttur ve genel üretilen işi hakkında en çok değer verdiğiniz bilgileri aşağıdaki tabloda senaryonuz için uygun bir sanal makine türünü seçmek için kullanın.

![SSIS tümleştirme çalışma zamanı en fazla toplam aktarım hızı](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** Integration runtime'nın ölçeklenebilirlik ayarlar. Integration runtime'nın aktarım hızı orantılıdır **AzureSSISNodeNumber**. Ayarlama **AzureSSISNodeNumber** küçük bir değere ilk Integration runtime'nın aktarım hızını izleyin, sonra senaryonuz için değeri ayarlayın. Çalışan düğümü sayısı yapılandırmak için bkz: [bir Azure-SSIS tümleştirme çalışma zamanını yönetme](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Paketleri çalıştırmak için zaten bir güçlü çalışan düğümü kullanırken, artan **AzureSSISMaxParallelExecutionsPerNode** Integration runtime'nın genel üretilen işi artırabilir. Standard_D1_v2 düğümlerde, düğüm başına 1-4 Paralel yürütme desteklenir. Tüm diğer türleri düğüm için düğüm başına 1-8 Paralel yürütme desteklenir.
Paketinizi ve çalışan düğümleri için aşağıdaki yapılandırmaları maliyeti temel alarak uygun değeri tahmin edebilirsiniz. Daha fazla bilgi için [genel amaçlı sanal makine boyutları](../virtual-machines/windows/sizes-general.md).

| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standart\_D1\_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 2/2x500                         | 2 / 750                                        |
| Standart\_D2\_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 4/4x500                         | 2 / 1500                                       |
| Standart\_D3\_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 8/8x500                         | 4 / 3000                                       |
| Standart\_D4\_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 16/16x500                       | 8 / 6000                                       |
| Standart\_A4\_v2 | 4    | 8           | 40                     | 4000/80/40                                             | 8/8x500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000/160/80                                            | 16/16x500                       | 8 / 2000                                       |

Doğru değeri ayarlamak için yönergeler şunlardır **AzureSSISMaxParallelExecutionsPerNode** özelliği: 

1. Başta küçük bir değere ayarlayın.
2. Bu hizmetin genel performansını geliştirilmiş olup olmadığını denetlemek için küçük bir miktarda artırır.
3. Hizmetin genel performansını en büyük değere ulaştığında değeri artırmak durdurun.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** fiyatlandırma katmanı için bir Azure SQL veritabanı'nda SSIS Kataloğu veritabanını (SSISDB). Bu ayar, yürütme günlüğü yüklemek için çalışanların IR örneği, bir paket yürütme kuyruğuna hızını ve hızı maksimum sayısını etkiler.

-   Hakkında kuyruk paket yürütme ve yürütme günlüğü yüklemek için hızlı umursamaz, en düşük veritabanı fiyatlandırma katmanı seçebilirsiniz. Temel fiyatlandırma ile Azure SQL veritabanı 8 çalışan bir tümleştirme çalışma zamanı örneğini destekler.

-   8'den fazla çalışan sayısı veya çekirdek sayısı 50'den fazla olan temel değerinden daha güçlü bir veritabanı seçin. Aksi takdirde veritabanı tümleştirme çalışma zamanı örneğin performans sorunu haline gelir ve genel performansını olumsuz şekilde etkilenir.

Veritabanı fiyatlandırma katmanı göre de ayarlayabilirsiniz [veritabanı işlem birimi](../sql-database/sql-database-what-is-a-dtu.md) (DTU) kullanım bilgilerini kullanılabilir Azure portalında.

## <a name="design-for-high-performance"></a>Yüksek performans tasarımı
Azure'da çalışan bir SSIS paketi tasarlama, şirket içi yürütme için bir paket tasarlama öğesinden farklıdır. Aynı paket içindeki birden fazla bağımsız görevi birleştirmek yerine, bunları Azure-SSIS IR'yi daha verimli yürütme için çeşitli paketler halinde ayırın Her paket için bir paket yürütme oluşturun, böylece bunlar birbirine için bitmesini beklemek zorunda değilsiniz. Bu yaklaşım, Azure-SSIS Integration runtime'nın ölçeklenebilirlik yararlanır ve hizmetin genel performansını artırır.

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS tümleştirme çalışma zamanı hakkında daha fazla bilgi edinin. Bkz: [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime).
