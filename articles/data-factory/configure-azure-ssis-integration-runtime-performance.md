---
title: Performansı Azure SSIS tümleştirmesi çalışma zamanı için yapılandırma | Microsoft Docs
description: Yüksek performans için Azure SSIS Integration zamanının özellikleri yapılandırmayı öğrenin
services: data-factory
ms.date: 01/10/2018
ms.topic: conceptual
ms.service: data-factory
ms.workload: data-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: dc61d9d5e1a2f2ccae09f77d80aca08ebfb59047
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299661"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Yüksek performans için Azure SSIS tümleştirmesi çalışma zamanı yapılandırma

Bu makalede, yüksek performanslı bir Azure SSIS tümleştirmesi çalışma zamanı (IR) yapılandırmak açıklar. Azure SSIS IR dağıtmak ve Azure'da SQL Server Integration Services (SSIS) paketleri çalıştırmak sağlar. Azure SSIS IR hakkında daha fazla bilgi için bkz: [tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime) makalesi. Dağıtma ve Azure üzerinde çalışan SSIS paketleri hakkında daha fazla bilgi için bkz: [yükseltme ve shift SQL Server Integration Services iş yükleri buluta](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> Bu makale performans sonuçları ve şirket içi SSIS geliştirme ekibi üyeleri tarafından yapılan test gelen gözlemleri içerir. Sonuçlarınızı farklılık gösterebilir. Maliyet ve performansı etkileyen yapılandırma ayarlarınızı Sonlandır önce kendi test yapabilirsiniz.

## <a name="properties-to-configure"></a>Özellikleri yapılandırmak için

Bir yapılandırma betiğini aşağıdaki bölümü Azure SSIS tümleştirmesi çalışma zamanı oluşturduğunuzda, sizin yapılandırabileceğiniz özellikleri gösterir. Tam PowerShell komut dosyası ve bir açıklama için bkz: [Azure SQL Server Integration Services dağıtma paketlere](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (Preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** tümleştirmesi çalışma zamanı çalışan düğümüne konumudur. Çalışan düğümüne Azure SQL veritabanı (SSISDB) SSIS katalog veritabanına sabit bir bağlantı tutar. Ayarlama **AzureSSISLocation** SSISDB barındıran SQL veritabanı sunucusu olarak aynı konuma olanak sağlayan mümkün olduğunca verimli bir şekilde çalışması için tümleştirme çalışma zamanı.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
Azure SSIS IR dahil olmak üzere Azure Data Factory v2 genel önizlemesi, aşağıdaki seçenekleri destekler:
-   Standart\_A4\_v2
-   Standart\_A8\_v2
-   Standart\_D1\_v2
-   Standart\_D2\_v2
-   Standart\_D3\_v2
-   Standart\_D4\_v2.

Resmi olmayan şirket içi SSIS mühendislik ekibi tarafından testinde, D serisinin A series SSIS paketi yürütme için daha uygun gibi görünüyor.

-   D serisinin performans/fiyat oranı A series yüksektir.
-   Verimlilik D serisinin aynı fiyat üzerinden bir seri daha yüksektir.

### <a name="configure-for-execution-speed"></a>Yürütme hız için yapılandırma
Çalıştırmak için çok sayıda paket yok ve hızlı bir şekilde çalıştırmak için paketler istiyorsanız, bilgileri aşağıdaki grafikte senaryonuz için uygun bir sanal makine türü seçmek için kullanın.

Bu verileri tek bir paket yürütme tek çalışan düğümünde temsil eder. Paket adı ve son adı sütunlarla 10 milyon kayıtları Azure Blob depolama alanından yükler, bir tam ad sütunu oluşturur ve Azure Blob depolama alanına 20 karakterden uzun tam ada sahip kayıtları yazar.

![SSIS tümleştirmesi çalışma zamanı paketi yürütme hızı](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Genel üretilen işi için yapılandırma

Paketleri çalıştırmak için çok sayıda varsa ve sizin için en genel üretilen işi hakkında önemli bilgileri aşağıdaki grafikte senaryonuz için uygun bir sanal makine türü seçmek için kullanın.

![SSIS tümleştirmesi çalışma zamanı en fazla genel üretilen işi](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** Integration zamanının ölçeklenebilirlik ayarlar. Üretilen iş Integration zamanının orantılıdır **AzureSSISNodeNumber**. Ayarlama **AzureSSISNodeNumber** küçük bir değere ilk Integration zamanının verimlilik izleyebilir, ardından senaryonuz için değer ayarlayın. Çalışan düğüm sayısı yapılandırmak için bkz: [Azure SSIS tümleştirmesi çalışma zamanı yönetmek](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Paketleri çalıştırmak için güçlü çalışan düğüme zaten kullanırken artırma **AzureSSISMaxParallelExecutionsPerNode** Integration zamanının genel üretilen işi artırabilir. Düğüm başına 1-4 paralel yürütmeleri Standard_D1_v2 düğümleri için desteklenir. Tüm diğer türleri düğümleri için düğüm başına 1-8 paralel yürütmeleri desteklenir.
Paketinizi ve çalışan düğümleri için aşağıdaki yapılandırmaları maliyetini göre uygun değeri tahmin edebilirsiniz. Daha fazla bilgi için bkz: [genel amaçlı sanal makine boyutlarını](../virtual-machines/windows/sizes-general.md).

| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standart\_D1\_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 2/2x500                         | 2 / 750                                        |
| Standart\_D2\_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 4/4x500                         | 2 / 1500                                       |
| Standart\_D3\_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 8/8x500                         | 4 / 3000                                       |
| Standart\_D4\_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 16/16x500                       | 8 / 6000                                       |
| Standart\_A4\_v2 | 4    | 8           | 40                     | 4000/80/40                                             | 8/8x500                         | 4 / 1000                                       |
| Standart\_A8\_v2 | 8    | 16          | 80                     | 8000/160/80                                            | 16/16x500                       | 8 / 2000                                       |

Doğru değeri ayarlamak için yönergeleri işte **AzureSSISMaxParallelExecutionsPerNode** özelliği: 

1. İlk küçük bir değere ayarlayın.
2. Genel üretilen işi geliştirilmiş olup olmadığını denetlemek için bir küçük miktarda artırın.
3. Genel üretilen işi en büyük değer ulaştığında değeri artırmak durdurun.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** bir Azure SQL veritabanı SSIS Katalog veritabanı (SSISDB) için fiyatlandırma katmanı. Bu ayar, en fazla yürütme günlüğü yüklemeye çalışan IR örnek, bir paket yürütme kuyruğuna hızını ve hızını sayısı etkiler.

-   Hakkında hızlı sırası paket yürütme ve yürütme günlüğü yüklemek için önemli değil, en düşük veritabanı fiyatlandırma katmanı seçebilirsiniz. Temel fiyatlandırma ile Azure SQL veritabanı 8 çalışanları tümleştirmesi çalışma zamanı örneği destekler.

-   Çalışan sayısı 8'den fazla ya da çekirdek sayısı 50'den fazla temel'den daha güçlü bir veritabanı seçin. Aksi takdirde sıkışıklık tümleştirmesi çalışma zamanı örneğinin veritabanı hale ve genel performansı olumsuz yönde etkilenebilir.

Veritabanı fiyatlandırma katmanı göre de ayarlayabilirsiniz [veritabanı işlem birimi](../sql-database/sql-database-what-is-a-dtu.md) (DTU) kullanım bilgilerini kullanılabilir Azure portalındaki.

## <a name="design-for-high-performance"></a>Yüksek performans tasarımı
Azure üzerinde çalışmak için bir SSIS paketi tasarlarken, şirket içi yürütme için paket tasarlama farklıdır. Aynı pakette birden çok bağımsız görevleri birleştirme yerine bunları Azure SSIS IR daha verimli yürütme için çeşitli paketler halinde ayırın Her paket için bir paket yürütme oluşturun, böylece birbirine için bitmesini beklemek zorunda değilsiniz. Bu yaklaşım ölçeklenebilirlik sağlamak Azure SSIS Integration zamanının avantaj ve genel üretilen işi artıran.

## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS tümleştirmesi çalışma zamanı hakkında daha fazla bilgi edinin. Bkz: [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime).
