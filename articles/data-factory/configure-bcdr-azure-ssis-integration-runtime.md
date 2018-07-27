---
title: Azure-SSIS tümleştirme çalışma zamanı iş sürekliliği ve olağanüstü durum kurtarma (BCDR) önerileri | Microsoft Docs
description: Bu makalede, Azure-SSIS tümleştirme çalışma zamanı iş sürekliliği ve olağanüstü durum kurtarma önerileri özetler.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/26/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 37347df2d543116085f52fed76c692b60fac2ad6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285861"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-recommendations-for-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı iş sürekliliği ve olağanüstü durum kurtarma (BCDR) önerileri

Olağanüstü durum kurtarma amacıyla, şu anda çalışıyor bölgedeki Azure-SSIS Integration runtime'ı durdurun ve yeniden başlatmak için başka bir bölgeye geçiş yapabilirsiniz. Kullanmanızı öneririz [Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md) bu amaç için.

## <a name="prerequisites"></a>Önkoşullar

- Sunucunun aynı anda bir kesinti olması durumunda, olağanüstü durum kurtarma için Azure SQL veritabanı sunucunuzun etkinleştirdiğinizden emin olun. Daha fazla bilgi için bkz. [Azure SQL veritabanı ile iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md).

- Geçerli bölgede bulunan bir sanal ağ'ı kullanıyorsanız, başka bir sanal ağ, Azure-SSIS tümleştirme çalışma zamanınızın bağlanmak için yeni bölgede kullanmanız gerekir. Daha fazla bilgi için bkz. [bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın](join-azure-ssis-integration-runtime-virtual-network.md).

- Özel kurulum kullanıyorsanız, bir kesinti sırasında erişilebilir olmaya devam eder, böylece özel kurulum betiğini ve ilişkili dosyalarını depolayan blob kapsayıcısı için başka bir SAS URI'si hazırlama gerekebilir. Daha fazla bilgi için bkz. [özel kurulum bir Azure-SSIS tümleştirme çalışma zamanını yapılandırma](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="steps"></a>Adımlar

Azure-SSIS IR durdurmak, IR için yeni bir bölgeye geçiş ve yeniden başlatmak için aşağıdaki adımları izleyin.

1. Özgün bölgede IR durdurun.

2. IR güncelleştirmek için PowerShell'de şu komuta çağrı yapın

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Bu PowerShell komut hakkında daha fazla bilgi için bkz. [Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)

3. IR yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR için diğer yapılandırma seçenekleri göz önünde bulundurun:

- [Yüksek performans için Azure-SSIS tümleştirme çalışma zamanı yapılandırma](configure-azure-ssis-integration-runtime-performance.md)

- [Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Azure-SSIS tümleştirme çalışma zamanı için Enterprise Edition sağlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
