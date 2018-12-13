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
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: d2000e626166304e92556e3c965df175a27046ad
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321076"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için Azure Active Directory kimlik doğrulamasını etkinleştirme

Bu makalede Azure Data Factory hizmet kimliği ile bir Azure-SSIS IR oluşturma işlemini gösterir. Azure Active Directory (Azure AD) kimlik doğrulaması yönetilen kimlikle Azure veri Fabrikanızı yerine SQL kimlik doğrulaması için bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için kullanabilirsiniz.

Veri fabrikanızın yönetilen kimlik hakkında daha fazla bilgi için bkz. [Azure veri fabrikası hizmet kimliği](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> SQL kimlik doğrulaması ile bir Azure-SSIS tümleştirme çalışma zamanı zaten oluşturduysanız, şu anda PowerShell ile Azure AD kimlik doğrulaması kullanılacak IR'yi yapılandıramazsınız.

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL veritabanı'nda Azure AD etkinleştir

Azure SQL veritabanı, bir Azure AD kullanıcısının bulunduğu bir veritabanı oluşturulmasını destekler. Sonuç olarak, Active Directory Yöneticisi olarak bir Azure AD kullanıcısı ayarlayın ve ardından SQL Server Management Studio (Azure AD kullanıcısını kullanarak SSMS için) oturum açın. Ardından, sunucu üzerinde SQL Server Integration Services (SSIS) kataloğunu oluşturmak, IR etkinleştirmek Azure AD grubunun içerilen kullanıcı oluşturabilirsiniz.

### <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-data-factory-a-member-of-the-group"></a>Azure AD'de grup oluşturma ve veri fabrikanızın yönetilen kimlik grubunun bir üyesi olun

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

3.  Veri fabrikanızın yönetilen kimlik grubuna ekleyin. İzleyebileceğiniz [Azure veri fabrikası hizmet kimliği](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) asıl hizmet kimliği almak için (örneğin, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, ancak hizmet kimliği uygulama kimliği, bu amaç için kullanmayın).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ayrıca, Grup üyeliğini sonradan inceleyebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

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

MSI ile veritabanı oluşturma, doğrudan Azure SQL veritabanı yönetilen örneği destekler. Veri Fabrikası MSI bir AD grubuna katılın veya içerilen kullanıcı oluşturmak mı gerek yoktur.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için Azure AD kimlik doğrulamasını etkinleştirme

1.   Azure portalında **tüm hizmetleri** -> **SQL sunucuları** sol gezinti bölmesinden.

1.   Azure AD kimlik doğrulaması için etkin için SQL server'ı seçin.

1.   İçinde **ayarları** dikey penceresinde bölümünü **Active Directory Yöneticisi**.

1.   Komut çubuğunda **yönetici Ayarla**.

1.   Sunucu Yöneticisi yapılması ve ardından seçmek için bir Azure AD kullanıcı hesabı seçin **seçin**.

1.   Komut çubuğunda **Kaydet**.

### <a name="add-data-factory-msi-as-a-user-to-the-azure-sql-database-managed-instance"></a>Veri Fabrikası MSI Azure SQL veritabanı yönetilen örneği için bir kullanıcı olarak ekleyin.

1.  SQL Server Management Studio’yu başlatın.

2.  SQL yönetici hesabı veya Active Directory yönetici hesabı kullanarak oturum açın.

3.  Nesne Gezgini'nde veritabanları -> Sistem veritabanları klasör.

4.  Ana veritabanında sağ tıklayıp **yeni sorgu**.

5.  Makale izleyebilirsiniz [Azure veri fabrikası hizmet kimliği](data-factory-service-identity.md) asıl hizmeti kimliği uygulama kimliği almak için (Hizmet kimliği Tanımlayıcısı bu amaç için kullanmayın.)

6.  Sorgu penceresinde, hizmet kimliği uygulama kimliği ikili türe dönüştürmek için aşağıdaki betiği çalıştırın:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your service identity application id}
    select CAST(@applicationId AS varbinary)
    ```

7.  Değer sonuç penceresinden alabilirsiniz.

8.  Sorgu penceresine temizleyin ve aşağıdaki betiği çalıştırın:

    ```sql
    CREATE LOGIN [{MSI name}] FROM EXTERNAL PROVIDER with SID ={your service identity application id in binary type}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{MSI name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{MSI name}]
    ```

9.  Komut başarıyla tamamlar.

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
