---
title: Azure-SSIS tümleştirme çalışma zamanı için Azure Active Directory kimlik doğrulamasını etkinleştirme | Microsoft Docs
description: Bu makalede, Azure-SSIS tümleştirme çalışma zamanı oluşturmak Azure Data Factory için yönetilen kimliğe sahip Azure Active Directory kimlik doğrulamasının nasıl etkinleştirileceği açıklanır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 5/14/2019
author: swinarko
ms.author: sawinark
manager: craigg
ms.openlocfilehash: a67436f09d6e28db8d19679e446ac4cf98383709
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65593808"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için Azure Active Directory kimlik doğrulamasını etkinleştirme

Bu makalede, Azure Data Factory (ADF) için yönetilen kimlik ile Azure Active Directory (Azure AD) kimlik doğrulamasını etkinleştirme ve sırayla sağlayacak bir Azure-SSIS Integration Runtime (IR) oluşturmak için SQL kimlik doğrulaması yerine kullanma gösterilmektedir Azure SQL veritabanı sunucusu/yönetilen örnek sizin adınıza, SSIS Kataloğu veritabanını (SSISDB).

Yönetilen kimlik bilgilerinizi ADF için hakkında daha fazla bilgi için bkz. [Data Factory için yönetilen identiy](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
>-  Bu senaryoda, ADF yönetilen kimlik ile Azure AD kimlik doğrulamasını yalnızca oluşturulmasında kullanılan ve sonraki başlangıç işlemleri de barındıracak SSIS IR sağlamak açıp SSISDB için bağlanın. SSIS paketi yürütme için SSIS IR hala SSISDB SSISDB sağlama sırasında oluşturulan tam olarak yönetilen hesaplar SQL kimlik doğrulaması kullanarak bağlanır.
>-  SQL kimlik doğrulaması kullanarak SSIS IR, zaten oluşturduysanız, şu anda PowerShell aracılığıyla Azure AD kimlik doğrulamasını kullanacak şekilde yeniden yapılandırabilirsiniz değil ancak Azure portal/ADF uygulama bunu yapabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL veritabanı'nda Azure AD etkinleştir

Azure SQL veritabanı sunucusu, bir Azure AD kullanıcısının bulunduğu bir veritabanı oluşturulmasını destekler. İlk olarak bir üye olarak, ADF için yönetilen kimlik ile bir Azure AD grubu oluşturmak gerekir. Ardından, bir Azure AD kullanıcısı, Azure SQL veritabanı sunucunuz için Active Directory Yöneticisi olarak ayarlayın ve ardından SQL Server Management Studio (kullanıcının kullanarak SSMS üzerinde) bağlanmak gerekir. Son olarak, yönetilen kimlik bilgilerinizi ADF için sizin adınıza SSISDB oluşturmak için Azure-SSIS IR tarafından kullanılabilmesi için Azure AD grubunu temsil eden içerilen kullanıcı oluşturmak gerekir.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>Azure AD grubu yönetilen kimlik ile ADF için bir üyesi olarak oluşturma

Mevcut bir Azure AD grubunu kullanın veya Azure AD PowerShell kullanarak yeni bir tane oluşturun.

1.  Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü.

2.  Oturum `Connect-AzureAD`, bir grup oluşturmak için aşağıdaki cmdlet'i çalıştırın ve bir değişkene kaydedin:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Sonuç değişken değerini görüntülendiği aşağıdaki örnekteki gibi görünür:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Yönetilen kimlik bilgilerinizi ADF için grubuna ekleyin. Makale izleyebilirsiniz [Data Factory için yönetilen identiy](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) birincil yönetilen kimlik nesne Kimliğini almak için (örneğin 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, ancak yönetilen Identity Application kimliği bu amaç için kullanmayın).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Daha sonra Grup üyeliği de denetleyebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Azure SQL veritabanı sunucusu için Azure AD kimlik doğrulamasını yapılandırma

Yapabilecekleriniz [yapılandırma ve SQL ile Azure AD kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) aşağıdaki adımları kullanarak:

1.  Azure portalında **tüm hizmetleri** -> **SQL sunucuları** sol gezinti bölmesinden.

2.  Azure AD kimlik doğrulaması ile yapılandırılması, Azure SQL veritabanı sunucusu seçin.

3.  İçinde **ayarları** dikey penceresinde bölümünü **Active Directory Yöneticisi**.

4.  Komut çubuğunda **yönetici Ayarla**.

5.  Sunucu Yöneticisi yapılması ve ardından seçmek için bir Azure AD kullanıcı hesabı seçin **seçin.**

6.  Komut çubuğunda **kaydedin.**

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Azure SQL veritabanı sunucusunda Azure AD grubunu temsil eden içerilen kullanıcı oluşturma

Bu sonraki adım için ihtiyacınız [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1. SSMS'yi başlatın.

2. İçinde **sunucuya Bağlan** iletişim kutusunda, Azure SQL veritabanı sunucu adını girin **sunucu adı** alan.

3. İçinde **kimlik doğrulaması** alanın, Seç **Active Directory - MFA desteğiyle Evrensel** (diğer iki kullanabilirsiniz Active Directory kimlik doğrulama türlerini görmek [yapılandırma ve yönetme Azure AD kimlik doğrulaması ile SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4. İçinde **kullanıcı adı** alan, örneğin sunucu yöneticisi olarak ayarladığınız Azure AD hesabının adını testuser@xxxonline.com.

5. seçin **Connect** ve oturum açma işlemini tamamlayın.

6. İçinde **Nesne Gezgini**, genişletme **veritabanları** -> **sistem veritabanları** klasör.

7. Sağ **ana** seçin ve veritabanı **yeni sorgu**.

8. Sorgu penceresinde aşağıdaki T-SQL komutunu girin ve seçin **yürütme** araç.

   ```sql
   CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
   ```

   Kapsanan kullanıcı grubunu temsil etmek için oluşturma komutu başarıyla tamamlamanız gerekir.

9. Sorgu penceresine işaretini kaldırın, aşağıdaki T-SQL komutu girin ve seçin **yürütme** araç.

   ```sql
   ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
   ```

   Komutu, kapsanan kullanıcı veritabanını (SSISDB) oluşturma olanağı verme başarıyla tamamlanmalıdır.

10. SQL kimlik doğrulaması kullanarak, SSISDB oluşturuldu ve ona erişmek için Azure AD kimlik doğrulaması için Azure-SSIS IR kullanmaya geçmek istiyorsunuz, sağ **SSISDB** seçin ve veritabanı **yeni sorgu**.

11. Sorgu penceresinde aşağıdaki T-SQL komutunu girin ve seçin **yürütme** araç.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Kapsanan kullanıcı grubunu temsil etmek için oluşturma komutu başarıyla tamamlamanız gerekir.

12. Sorgu penceresine işaretini kaldırın, aşağıdaki T-SQL komutu girin ve seçin **yürütme** araç.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    Komutu, kapsanan kullanıcı SSISDB erişim olanağı verme başarıyla tamamlanmalıdır.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği Azure AD etkinleştir

Azure SQL veritabanı yönetilen örneği, ADF için yönetilen kimlikle bir veritabanına doğrudan oluşturulmasını destekler. Değil, yönetilen kimlik için bir Azure AD grubu, ADF katılın veya temsil eden yönetilen Örneğinize o grupta içerilen kullanıcı oluşturmak.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için Azure AD kimlik doğrulamasını yapılandırma

1.   Azure portalında **tüm hizmetleri** -> **SQL sunucuları** sol gezinti bölmesinden.

2.   Yönetilen Azure AD kimlik doğrulaması ile yapılandırılması örneğinizi seçin.

3.   İçinde **ayarları** dikey penceresinde bölümünü **Active Directory Yöneticisi**.

4.   Komut çubuğunda **yönetici Ayarla**.

5.   Sunucu Yöneticisi yapılması ve ardından seçmek için bir Azure AD kullanıcı hesabı seçin **seçin**.

6.   Komut çubuğunda **Kaydet**.

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>Yönetilen kimlik bilgilerinizi ADF için Azure SQL veritabanı yönetilen örneği'nın bir kullanıcı olarak ekleyin.

Bu sonraki adım için ihtiyacınız [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  SSMS'yi başlatın.

2.  Yönetilen SQL/Active Directory yönetici hesabınızı kullanarak sunucuya bağlanın.

3.  İçinde **Nesne Gezgini**, genişletme **veritabanları** -> **sistem veritabanları** klasör.

4.  Sağ **ana** seçin ve veritabanı **yeni sorgu**.

5.  Yönetilen kimlik bilgilerinizi ADF için alın. Makale izleyebilirsiniz [Data Factory için yönetilen identiy](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) için birincil yönetilen kimlik uygulama kimliği alma (ancak yönetilen kimlik nesne kimliği bu amaç için kullanmayın).

6.  Sorgu penceresinde, yönetilen kimlik bilgilerinizi ADF ikili türe dönüştürmek için aşağıdaki T-SQL betiğini yürütün:

    ```sql
    DECLARE @applicationId uniqueidentifier = '{your Managed Identity Application ID}'
    select CAST(@applicationId AS varbinary)
    ```
    
    Yönetilen kimlik olarak ikili, ADF görüntüleme komutu başarıyla tamamlanması.

7.  Sorgu penceresine temizleyin ve bir kullanıcı olarak, ADF için yönetilen kimlik eklemek için aşağıdaki T-SQL betiğini yürütün

    ```sql
    CREATE LOGIN [{a name for the managed identity}] FROM EXTERNAL PROVIDER with SID = {your Managed Identity Application ID as binary}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{the managed identity name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{the managed identity name}]
    ```
    
    Yönetilen kimlik ADF veritabanını (SSISDB) oluşturma olanağı verme komutu başarıyla tamamlanması.

8.  SQL kimlik doğrulaması kullanarak, SSISDB oluşturuldu ve ona erişmek için Azure AD kimlik doğrulaması için Azure-SSIS IR kullanmaya geçmek istiyorsunuz, sağ **SSISDB** seçin ve veritabanı **yeni sorgu**.

9.  Sorgu penceresinde aşağıdaki T-SQL komutunu girin ve seçin **yürütme** araç.

    ```sql
    CREATE USER [{the managed identity name}] FOR LOGIN [{the managed identity name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{the managed identity name}]
    ```

    Yönetilen kimlik bilgilerinizi ADF için SSISDB erişim olanağı verme komutu başarıyla tamamlanması.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Azure portal/ADF uygulamasında Azure-SSIS IR sağlama

Ne zaman sağladığınız Azure portal/ADF uygulamasında Azure-SSIS IR üzerinde **SQL ayarları** sayfasında **, ADF için kullanım AAD kimlik doğrulaması ile yönetilen kimlik** seçeneği. Aşağıdaki ekran görüntüsünde, SSISDB barındıran Azure SQL veritabanı sunucusu ile IR ayarlarını gösterir. IR ile yönetilen örneği, SSISDB barındırmak için **Katalog veritabanı hizmet katmanı** ve **izin Azure Hizmetleri için erişim** ayarları değil uygulanabilir, diğer ayarları aynı çalışırken.

Bir Azure-SSIS IR oluşturma hakkında daha fazla bilgi için bkz. [Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Azure-SSIS tümleştirme çalışma zamanı ayarları](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>PowerShell ile Azure-SSIS IR sağlama

PowerShell ile Azure-SSIS IR sağlamak için şunları yapın:

1.  Yükleme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) modülü.

2.  Betiğinizde, ayarlamayın `CatalogAdminCredential` parametresi. Örneğin:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
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

    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
