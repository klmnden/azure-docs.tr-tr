---
title: Azure-SSIS tümleştirme çalışma zamanı için Enterprise Edition sağlama | Microsoft Docs
description: Bu makalede Azure-SSIS tümleştirme çalışma zamanı ve onu sağlama için Enterprise Edition özellikleri açıklanmıştır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: c475a9c820e577a321491b87d806ed08ddff8693
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013887"
---
# <a name="provision-enterprise-edition-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için Enterprise Edition sağlama

Azure-SSIS Integration Runtime'nın Enterprise Edition aşağıdaki gelişmiş kullanın ve premium özellikleri sağlar:
-   Değişiklik verilerini yakalama (CDC) bileşenleri
-   Oracle ve Teradata SAP BW bağlayıcıları
-   SQL Server Analysis Services (SSAS) ve Azure Analysis Services (AAS) bağlayıcıları ve dönüştürmeler
-   Gruplandırma ve benzer arama dönüşümleri belirsiz
-   Terim ayıklama ve arama terimi dönüştürmeler

Bu özelliklerin bazıları Azure-SSIS IR'yi özelleştirmek için ek bileşenler yüklemeniz gerekir Ek bileşenlerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Kurumsal özellikler

| **Kurumsal özellikler** | **Açıklamaları** |
|---|---|
| CDC bileşenleri | Azure-SSIS IR Enterprise Edition'da önceden yüklenmiş CDC kaynak denetimi görev ve bölme dönüştürme. Oracle bağlanmak için de bu CDC Tasarımcısı ve hizmeti başka bir bilgisayara yüklemeniz gerekir. |
| Oracle bağlayıcılar | Oracle Bağlantı Yöneticisi, kaynak ve hedef Azure-SSIS IR Enterprise Edition'da önceden yüklenen. Ayrıca Oracle Çağrı Arabirimi (OCI) sürücüsünü yüklemek ve gerekirse, Oracle aktarım ağ Tabaka (TNS) üzerinde Azure-SSIS IR'yi yapılandırın gerekir Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Teradata bağlayıcılar | Azure-SSIS IR Enterprise sürümünde, Teradata Bağlantı Yöneticisi, kaynak ve hedef yanı sıra Teradata paralel Transporter (TPT) API ve Teradata ODBC sürücüsünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| SAP BW bağlayıcıları | SAP BW Bağlantı Yöneticisi, kaynak ve hedef Azure-SSIS IR Enterprise Edition'da önceden yüklenen. Ayrıca SAP BW sürücü Azure-SSIS IR'yi yüklemeniz gerekir Bu bağlayıcılar, SAP BW 7.0 veya önceki sürümleri destekler. SAP BW veya diğer SAP ürünleri daha sonraki sürümlere bağlanmak için satın alma ve Azure-SSIS IR'yi üçüncü taraf ISV SAP bağlayıcıları yükleyin Ek bileşenlerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Analysis Services bileşenleri               | Azure-SSIS IR Enterprise sürümünde, veri madenciliği modeli eğitimi hedef, işleme boyutu hedef ve bölüm işleme hedef yanı sıra veri madenciliği sorgu dönüştürme önceden yüklenen. SQL Server Analysis Services (SSAS) tüm bu bileşenlerin destekler, ancak yalnızca bölüm işleme hedef Azure Analysis Services (AAS) destekler. SSAS için bağlanmak için etmeniz [SSISDB içinde Windows kimlik doğrulaması kimlik bilgilerini yapılandırma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Bu bileşenlerin yanı sıra DDL görev yürütme Analiz Hizmetleri, Analiz Hizmetleri işleme görevi ve veri madenciliği sorgusu görevi de Azure-SSIS IR standart/Enterprise Edition'da önceden yüklenen. |
| Gruplandırma ve benzer arama dönüşümleri belirsiz  | Azure-SSIS IR Enterprise Edition'da belirsiz gruplandırma ve benzer arama dönüştürmeleri önceden yüklenen. Bu bileşenler, başvuru verileri depolamak için SQL Server hem de Azure SQL veritabanı destekler. |
| Terim ayıklama ve arama terimi dönüştürmeler | Azure-SSIS IR Enterprise Edition'da terimi ayıklama ve arama terimi dönüştürmeleri önceden yüklenen. Bu bileşenler, başvuru verileri depolamak için SQL Server hem de Azure SQL veritabanı destekler. |

## <a name="instructions"></a>Yönergeler

1.  İndirme ve yükleme [Azure PowerShell (sürüm 5.4 veya sonrası)](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018).

2.  Sağlama veya PowerShell ile Azure-SSIS IR yeniden çalıştırma `Set-AzureRmDataFactoryV2IntegrationRuntime` ile **Kurumsal** değeri olarak **Edition** Azure-SSIS IR'yi başlamadan önce parametresi Örnek bir betik verilmiştir:

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

-   [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Geliştirme konusunda ücretli veya özel bileşenler Azure-SSIS tümleştirme çalışma zamanı için lisanslı](how-to-develop-azure-ssis-ir-licensed-components.md)
