---
title: Enterprise Edition için Azure SSIS tümleştirmesi çalışma zamanı sağlama | Microsoft Docs
description: Bu makalede Azure SSIS tümleştirmesi çalışma zamanı ve onu sağlama için Enterprise Edition özelliklerini açıklar
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: 55f4fd18dbebe8a4c666c5512b9cad46ddf9f7d7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296865"
---
# <a name="enterprise-edition-of-the-azure-ssis-integration-runtime"></a>Azure SSIS Integration zamanının Enterprise Edition

Azure SSIS Integration zamanının Enterprise Edition aşağıdaki gelişmiş kullanın ve premium özellikleri sağlar:
-   Değişiklik verilerini yakalama (CDC) bileşenleri
-   Oracle ve Teradata SAP BW bağlayıcılar
-   SQL Server Analysis Services (SSAS) ve Azure Analysis Services (AAS) bağlayıcıları ve dönüştürmeler
-   Belirsiz gruplandırma ve benzer arama dönüşümleri
-   Terim ayıklama ve terimi araması dönüştürmeler

Bu özelliklerden bazıları duyduğunuz Azure SSIS IR özelleştirmek için ek bileşenleri yüklemek için Ek bileşenlerin nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Kurumsal özellikler

| **Kurumsal özellikler** | **Açıklamaları** |
|---|---|
| CDC bileşenleri | CDC kaynak, Denetim görev ve Bölümlendirici dönüştürme Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Oracle için bağlanmak için de CDC Tasarımcısı ve hizmet başka bir bilgisayara yüklemeniz gerekir. |
| Oracle bağlayıcılar | Oracle Bağlantı Yöneticisi, kaynak ve hedef Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Ayrıca Oracle Çağrı Arabirimi (OCI) sürücüsünü yüklemek ve gerekirse, Oracle aktarım ağ Tabaka (TNS) üzerinde Azure SSIS IR yapılandırmak gerekir Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Teradata bağlayıcılar | Teradata Bağlantı Yöneticisi, kaynak ve hedef yanı sıra Teradata paralel Transporter (birleştirilmiş TPT) API ve Teradata ODBC sürücüsü Azure SSIS IR Enterprise Edition'da yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| SAP BW bağlayıcılar | SAP BW Bağlantı Yöneticisi, kaynak ve hedef Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Ayrıca Azure SSIS IR SAP BW sürücü yüklemeniz gerekir Bu bağlayıcıların SAP BW 7.0 veya önceki sürümleri destekler. SAP BW veya diğer SAP ürünleri sonraki sürümleri için bağlanmak için satın alma ve Azure SSIS IR üçüncü taraf ISV SAP bağlayıcıları yükleyin Ek bileşenlerin nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Analiz Hizmetleri bileşenleri               | Veri araştırma modelinin eğitimi hedef, boyut işleme hedef ve bölüm işleme hedef yanı sıra veri madenciliği sorgu dönüşümü Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Tüm bu bileşenlerin SQL Server Analysis Services (SSAS) destekler, ancak yalnızca bölüm işleme hedef Azure Analysis Services (AAS) destekler. SSAS için bağlanmak için etmeniz [SSISDB içinde Windows kimlik doğrulaması kimlik bilgilerini yapılandırın](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Bu bileşenler ek olarak, Analysis Services yürütme DDL görev, Analysis Services görev işleme ve veri madenciliği sorgusu görevi de Azure SSIS IR standart/Enterprise Edition'da önceden yüklenmiş. |
| Belirsiz gruplandırma ve benzer arama dönüşümleri  | Belirsiz gruplandırma ve benzer arama dönüştürmeler Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Bu bileşen, başvuru verileri depolamak için SQL Server ve Azure SQL veritabanı destekler. |
| Terim ayıklama ve terimi araması dönüştürmeler | Terim ayıklama ve terimi araması dönüştürmeler Azure SSIS IR Enterprise Edition'da önceden yüklenmiş. Bu bileşen, başvuru verileri depolamak için SQL Server ve Azure SQL veritabanı destekler. |

## <a name="instructions"></a>Yönergeler

1.  İndirme ve yükleme [Azure PowerShell (sürüm 5.4 veya üstü)](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018).

2.  Sağlamak veya PowerShell ile Azure SSIS IR yeniden yapılandırmak, çalıştırmak `Set-AzureRmDataFactoryV2IntegrationRuntime` ile **Kurumsal** değeri olarak **Edition** Azure SSIS IR başlamadan önce parametresi Burada, örnek bir betik verilmiştir:

    ```powershell
    $MyAzureSsisIrEdition = "Enterprise"

    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                               -Name $MyAzureSsisIrName
                                               -ResourceGroupName $MyResourceGroupName
                                               -Edition $MyAzureSsisIrEdition

    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                                 -Name $MyAzureSsisIrName
                                                 -ResourceGroupName $MyResourceGroupName
    ```

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Geliştirmeyi ücretli veya Azure SSIS tümleştirmesi çalışma zamanı için özel bileşenler lisanslı](how-to-develop-azure-ssis-ir-licensed-components.md)
