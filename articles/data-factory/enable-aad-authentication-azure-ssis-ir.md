---
title: Azure Active Directory kimlik doğrulamasını etkinleştirmek için Azure-SSIS tümleştirme çalışma zamanı | Microsoft Docs
description: Bu makalede, Azure Active Directory kimlik doğrulamasını kullanan bağlantıları etkinleştirmek için Azure-SSIS tümleştirme çalışma zamanı yapılandırma açıklanır.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: 3c829819748309ecbca248afe35cd59f54b202a6
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085419"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için Azure Active Directory kimlik doğrulamasını etkinleştirme

Bu makalede Azure Data Factory hizmet kimliği ile bir Azure-SSIS IR oluşturma işlemini gösterir. Azure Active Directory (Azure AD) kimlik doğrulaması yönetilen kimlikle Azure veri Fabrikanızı yerine SQL kimlik doğrulaması için bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için kullanabilirsiniz.

Yönetilen kimlik bilgilerinizi ADF için hakkında daha fazla bilgi için bkz. [Azure veri fabrikası hizmet kimliği](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> SQL kimlik doğrulaması ile bir Azure-SSIS tümleştirme çalışma zamanı zaten oluşturduysanız, şu anda PowerShell ile Azure AD kimlik doğrulaması kullanılacak IR'yi yapılandıramazsınız.

## <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-adf-a-member-of-the-group"></a>Azure AD'de grup oluşturma ve yönetilen kimlik bilgilerinizi ADF için grubunun bir üyesi olun.

Mevcut Azure AD grubunu kullanabilir veya Azure AD PowerShell kullanarak yeni bir grup oluşturabilirsiniz.

1.  Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü.

2.  Oturum `Connect-AzureAD`, grubu oluşturmak için aşağıdaki komutu çalıştırın ve bir değişkene kaydedin:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Çıktı, ayrıca değişkenin değerini inceler aşağıdaki örnekteki gibi görünür:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Yönetilen kimlik bilgilerinizi ADF için grubuna ekleyin. İzleyebileceğiniz [Azure veri fabrikası hizmet kimliği](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) asıl hizmet kimliği almak için (örneğin, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, ancak hizmet kimliği uygulama kimliği, bu amaç için kullanmayın).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ayrıca, Grup üyeliğini sonradan inceleyebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL veritabanı'nda Azure AD etkinleştir

Azure SQL veritabanı, bir Azure AD kullanıcısının bulunduğu bir veritabanı oluşturulmasını destekler. Sonuç olarak, Active Directory Yöneticisi olarak bir Azure AD kullanıcısını belirle ve SSMS kullanarak bir Azure AD kullanıcısı için oturum açın. Ardından, sunucu üzerinde SQL Server Integration Services (SSIS) kataloğunu oluşturmak, IR etkinleştirmek Azure AD grubunun içerilen kullanıcı oluşturabilirsiniz.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Azure SQL veritabanı için Azure AD kimlik doğrulamasını etkinleştirme

Yapabilecekleriniz [SQL veritabanı için Azure AD kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) aşağıdaki adımları kullanarak:

1.  Azure portalında **tüm hizmetleri** -> **SQL sunucuları** sol gezinti bölmesinden.

2.  Azure AD kimlik doğrulaması için etkinleştirilecek SQL veritabanını seçin.

3.  İçinde **ayarları** dikey penceresinde bölümünü **Active Directory Yöneticisi**.

4.  Komut çubuğunda **yönetici Ayarla**.

5.  Sunucu Yöneticisi yapılması ve ardından seçmek için bir Azure AD kullanıcı hesabı seçin **seçin.**

6.  Komut çubuğunda **kaydedin.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Azure AD grubunu temsil eden veritabanında bir içerilen kullanıcı oluşturma

Bu sonraki adım için ihtiyacınız [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  SQL Server Management Studio’yu başlatın.

2.  İçinde **sunucuya Bağlan** iletişim kutusunda SQL sunucu adınızı girin **sunucu adı** alan.

3.  İçinde **kimlik doğrulaması** alanın, Seç **Active Directory - MFA desteğiyle Evrensel**. (Diğer iki Active Directory kimlik doğrulama türleri de kullanabilirsiniz. Bkz: [yapılandırma ve SQL veritabanı yönetilen örneği ile Azure Active Directory kimlik doğrulamasını yönetmek](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  İçinde **kullanıcı adı** sunucu yöneticisi olarak - Örneğin, belirlediğiniz bir Azure AD hesabı adını girin testuser@xxxonline.com.

5.  seçin **Connect**. Oturum açma işlemini tamamlayın.

6.  İçinde **Nesne Gezgini**, genişletme **veritabanları** sistem veritabanları klasörünü ->.

7.  Sağ-Seç **ana** seçin ve veritabanı **yeni sorgu**.

8.  Sorgu penceresinde aşağıdaki satırı girin ve seçin **yürütme** araç çubuğunda:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Komutun başarıyla tamamlanması ve grup için içerilen kullanıcıyı oluşturması gerekir.

9.  Sorgu penceresine işaretini kaldırın, aşağıdaki satırı girin ve seçin **yürütme** araç çubuğunda:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    Komutu, kapsanan kullanıcı veritabanı oluşturma olanağı verme başarıyla tamamlanmalıdır.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği Azure AD etkinleştir

Azure SQL veritabanı yönetilen örneği AD Yöneticisi dışında tüm Azure AD kullanıcısı ile veritabanı oluşturma desteklemiyor Sonuç olarak, Azure AD grubu Active Directory Yöneticisi ayarlamanız gerekir İçerilen kullanıcı oluşturmanız gerekmez.

Yapabilecekleriniz [SQL veritabanı yönetilen örneği sunucusu için Azure AD kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) aşağıdaki adımları kullanarak:

7.  Azure portalında **tüm hizmetleri** -> **SQL sunucuları** sol gezinti bölmesinden.

8.  Azure AD kimlik doğrulaması için etkin için SQL server'ı seçin.

9.  İçinde **ayarları** dikey penceresinde bölümünü **Active Directory Yöneticisi**.

10. Komut çubuğunda **yönetici Ayarla**.

11. Arayın ve Azure AD grubu (örneğin, SSISIrGroup) seçip **seçin.**

12. Komut çubuğunda **kaydedin.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Portalda Azure-SSIS IR sağlamak

Ne zaman sağladığınız Azure portalı ile Azure-SSIS IR üzerinde **SQL ayarları** sayfasında "kullanımı AAD, ADF için yönetilen kimlik ile kimlik doğrulaması" seçeneği. (Aşağıdaki ekran görüntüsünde IR ile Azure SQL veritabanı ayarlarını gösterir. Yönetilen örneği ile IR için "Katalog veritabanı hizmet katmanı" özelliği kullanılabilir değil; diğer ayarları aynıdır.)

Bir Azure-SSIS tümleştirme çalışma zamanı oluşturma hakkında daha fazla bilgi için bkz. [Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Azure-SSIS tümleştirme çalışma zamanı ayarları](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>PowerShell ile Azure-SSIS IR sağlamak

PowerShell ile Azure-SSIS IR sağlamak için şunları yapın:

1.  Yükleme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) modülü.

2.  Betiğinizde, ayarlamayın *CatalogAdminCredential* parametresi. Örneğin:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
