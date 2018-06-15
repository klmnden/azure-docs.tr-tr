---
title: Azure Active Directory kimlik doğrulaması için Azure SSIS tümleştirmesi çalışma zamanı etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Active Directory kimlik doğrulaması kullanan bağlantıları etkinleştirmek için Azure SSIS tümleştirmesi çalışma zamanı yapılandırmak açıklar.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: douglasl
ms.openlocfilehash: 5fce1a3b8370ce49a522f41749795362e1bf1f9b
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757286"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı için Azure Active Directory kimlik doğrulamasını etkinleştirin

Bu makalede Azure Data Factory hizmeti kimliği ile bir Azure SSIS IR oluşturulacağını gösterir. Azure Active Directory (Azure AD) kimlik doğrulaması ile yönetilen hizmet kimliği (MSI) Azure SSIS tümleştirmesi çalışma zamanı için bir Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için SQL kimlik doğrulaması yerine veri fabrikası MSI kullanmanıza olanak sağlar.

Veri Fabrikası MSI hakkında daha fazla bilgi için bkz: [Azure Data Factory hizmeti kimliği](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> SQL kimlik doğrulaması ile Azure SSIS tümleştirmesi çalışma zamanı zaten oluşturduysanız, IR şu anda PowerShell ile Azure AD kimlik doğrulaması kullanacak şekilde yapılandıramazsınız.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Azure AD'de bir grup oluşturun ve veri fabrikası MSI grubunun bir üyesi yapın

Varolan bir Azure AD grubunu kullanın veya Azure AD PowerShell kullanarak yeni bir tane oluşturun.

1.  Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü.

2.  Kullanarak oturum `Connect-AzureAD`, grubu oluşturmak için aşağıdaki komutu çalıştırın ve bir değişkene kaydedin:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Çıktı ayrıca değişkenin değerini inceler aşağıdaki gibi görünür:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Veri Fabrikası MSI grubuna ekleyin. İzleyebileceğiniz [Azure Data Factory hizmeti kimliği](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity) hizmet kimlik Kimliği almak için (örneğin, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ayrıca, Grup üyeliğini daha sonra inceleyebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL veritabanı üzerinde Azure AD etkinleştir

Azure SQL Database, Azure AD kullanıcısının bulunduğu bir veritabanı oluşturmayı destekler. Sonuç olarak, bir Azure AD kullanıcısının Active Directory Yöneticisi olarak ayarlayın ve ardından Azure AD kullanıcısının kullanarak SSMS için oturum açın. Ardından, sunucu üzerinde SQL Server Integration Services (SSIS) katalogunu oluşturmak IR etkinleştirmek için Azure AD grubu için kapsanan bir kullanıcı oluşturabilirsiniz.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Azure SQL veritabanı için Azure AD kimlik doğrulamasını etkinleştirin

Yapabilecekleriniz [SQL veritabanı için Azure AD kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) aşağıdaki adımları kullanarak:

1.  Azure portalında seçin **tüm hizmetleri** -> **SQL sunucuları** sol gezinti gelen.

2.  Azure AD kimlik doğrulaması için etkinleştirilecek SQL veritabanını seçin.

3.  İçinde **ayarları** seçin dikey bölümde **Active Directory Yöneticisi**.

4.  Komut çubuğunda seçin **ayarlamak yönetici**.

5.  Sunucu Yöneticisi yapılması ve daha sonra seçmek için bir Azure AD kullanıcı hesabını seçin **seçin.**

6.  Komut çubuğunda seçin **kaydedin.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Azure AD grubu temsil eden veritabanında içerilen kullanıcı oluşturmak

Bu sonraki adım için gereksinim duyduğunuz [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  SQL Server Management Studio’yu başlatın.

2.  İçinde **sunucuya Bağlan** iletişim kutusunda, SQL server adınızı girin **sunucu adı** alan.

3.  İçinde **kimlik doğrulaması** alan, select **Active Directory - Evrensel MFA desteğiyle**. (Diğer iki Active Directory kimlik doğrulama türlerini de kullanabilirsiniz. Bkz: [yapılandırma ve SQL Database, yönetilen örneği ile Azure Active Directory kimlik doğrulamasını yönetmek](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure).)

4.  İçinde **kullanıcı adı** alanında, Sunucu Yöneticisi - Örneğin, ayarladığınız Azure AD hesabının adını girin testuser@xxxonline.com.

5.  Seçin **bağlanmak**. Oturum açma işlemini tamamlayın.

6.  İçinde **Object Explorer**, genişletin **veritabanları** -> Sistem veritabanları klasör.

7.  Sağ-Seç **ana** veritabanı ve seçin **yeni sorgu**.

8.  Sorgu penceresinde, aşağıdaki satırı girin ve seçin **yürütme** araç çubuğunda:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Komut grubu için kapsanan kullanıcı oluşturma başarıyla tamamlanmalıdır.

9.  Sorgu penceresi temizleyin, aşağıdaki satırı girin ve seçin **yürütme** araç çubuğunda:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    Komut, başarıyla, kapsanan kullanıcı veritabanı oluşturma olanağı verme tamamlamanız gerekir.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği üzerinde Azure AD etkinleştir

Yönetilen Azure SQL veritabanı örneği AD yönetici dışında herhangi bir Azure AD kullanıcı ile bir veritabanı oluşturmayı desteklemiyor Sonuç olarak, Azure AD grubu Active Directory yönetici olarak ayarlamanız gerekir Kapsanan kullanıcı oluşturmanız gerekmez.

Yapabilecekleriniz [SQL veritabanı örneği yönetilen sunucu için Azure AD kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) aşağıdaki adımları kullanarak:

7.  Azure portalında seçin **tüm hizmetleri** -> **SQL sunucuları** sol gezinti gelen.

8.  Azure AD kimlik doğrulaması için etkin için SQL server seçin.

9.  İçinde **ayarları** seçin dikey bölümde **Active Directory Yöneticisi**.

10. Komut çubuğunda seçin **ayarlamak yönetici**.

11. Arama ve Azure AD grubunu (örneğin, SSISIrGroup) seçin ve Seç **seçin.**

12. Komut çubuğunda seçin **kaydedin.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Portalda Azure SSIS IR sağlama

Ne zaman sağlamanız, Azure portal ile Azure SSIS IR üzerinde **SQL ayarlarını** sayfasında, onay "kullanım AAD ADF MSI ile kimlik doğrulaması" seçeneği. (Aşağıdaki ekran görüntüsü IR Azure SQL Database için ayarları gösterir. Yönetilen örneğiyle IR "Katalog veritabanı hizmet katmanının" özelliği mevcut değil; diğer ayarları aynıdır.)

Bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma hakkında daha fazla bilgi için bkz: [Azure Data Factory'de bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

![Azure SSIS tümleştirmesi çalışma zamanı ayarları](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>PowerShell ile Azure SSIS IR sağlama

PowerShell ile Azure SSIS IR sağlamak için şunları yapın:

1.  Yükleme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) modülü.

2.  Komut dosyanızı ayarlamayın *CatalogAdminCredential* parametresi. Örneğin:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Type Managed `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -Description $AzureSSISDescription `
                                               -Edition $AzureSSISEdition `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
