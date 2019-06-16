---
title: Azure Active Directory kimlik doğrulama - SQL yapılandırma | Microsoft Docs
description: SQL veritabanı yönetilen örneği ve SQL veri ambarı için Azure AD yapılandırdıktan sonra Azure Active Directory kimlik - kullanarak bağlanmayı öğreneceksiniz.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: abb4a43176026fca5a80409ade13af1f8f96d9f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60390588"
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql"></a>Yapılandırma ve Azure Active Directory kimlik doğrulaması SQL ile yönetme

Bu makalede, oluşturma ve Azure AD doldurun ve ardından Azure ile Azure AD kullanma işlemini göstermektedir [SQL veritabanı](sql-database-technical-overview.md), [yönetilen örneği](sql-database-managed-instance.md), ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md). Genel bakış için bkz. [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md).

> [!NOTE]
> Bu makale, Azure SQL server ve Azure SQL sunucusu üzerinde oluşturulmuş olan hem SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.
> [!IMPORTANT]  
> Bir Azure Active Directory hesabı kullanarak bir Azure sanal makinesinde çalışan SQL Server için bağlanması desteklenmiyor. Bunun yerine bir etki alanı Active Directory hesabı kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

## <a name="create-and-populate-an-azure-ad"></a>Oluşturma ve Azure AD'yi doldurma

Azure AD'yi oluşturun ve kullanıcılar ve gruplar ile doldurun. Azure AD, ilk Azure AD olabilir yönetilen etki alanı. Azure AD, bir şirket içi Active Directory etki alanı Federasyon Hizmetleri Azure AD ile de olabilir.

Daha fazla bilgi edinmek için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md), [Kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure artık Windows Server Active Directory ile federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Azure AD dizininizi yönetme](../active-directory/fundamentals/active-directory-administer.md), [Azure AD'yi Windows PowerShell kullanarak yönetme](/powershell/azure/overview) ve [Karma Kimlik için gerekli bağlantı noktaları ve protokoller](../active-directory/hybrid/reference-connect-ports.md).

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Azure Active Directory'ye bir Azure aboneliği ekleme veya ilişkilendirme

1. Azure aboneliğinize Azure Active Directory dizin veritabanını barındıran Azure aboneliği için güvenilir bir dizin sağlayarak ilişkilendirin. Ayrıntılar için bkz [Azure aboneliklerinin Azure AD ile ilişkisi](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
2. Dizin değiştiricisini, etki alanı ile ilişkili aboneliğe geçmek için Azure Portalı'nda kullanın.

   **Ek bilgi:** Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Aboneliğin bir dizinle arasındaki bu güven ilişkisi, bir aboneliğin daha çok abonelik alt kaynakları gibi olan, Azure'daki tüm diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili bu diğer kaynaklara erişim de durdurulur. Ancak dizin Azure içinde kalır, siz de başka bir aboneliği bu dizinle ilişkilendirebilir, dizin kullanıcılarını yönetmeye devam edebilirsiniz. Kaynaklar hakkında daha fazla bilgi için bkz. [azure'da kaynak erişimini anlama](../active-directory/active-directory-b2b-admin-add-users.md). İlişki bakın güvenilen Bunun hakkında daha fazla bilgi edinmek için [veya Azure Active Directory'ye bir Azure aboneliği ekleme ilişkilendirmek nasıl](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Azure SQL server için Azure AD Yöneticisi oluşturma

(Bu bir SQL veritabanı veya SQL veri ambarı barındıran) her bir Azure SQL server yöneticisi olan tüm Azure SQL sunucusu tek bir sunucu yönetici hesabı ile başlar. İkinci bir SQL Server Yöneticisi, Azure AD hesabı oluşturulmalıdır. Bu asıl asıl veritabanında bağımsız veritabanı kullanıcısı olarak oluşturulur. Yönetici olarak, Sunucu Yöneticisi hesapları üyeleri **db_owner** her kullanıcı rolünde veritabanı ve her bir kullanıcı veritabanı olarak **dbo** kullanıcı. Sunucu yönetici hesapları hakkında daha fazla bilgi için bkz. [yönetme veritabanları ve Azure SQL veritabanı'nda oturum açma bilgileri](sql-database-manage-logins.md).

Azure Active Directory coğrafi çoğaltma ile kullanırken, Azure Active Directory Yöneticisi birincil ve ikincil sunucular için yapılandırılması gerekir. Bir sunucunun Azure Active Directory Yöneticisi yok, ardından Azure Active Directory oturum açma bilgileri ve kullanıcılar bir "sunucu hatası bağlanılamıyor" alırsınız.

> [!NOTE]
> (Azure SQL server yönetici hesabı dahil), bir Azure AD hesabı üzerinde dayalı olmayan kullanıcılar önerilen veritabanı kullanıcıları Azure AD ile doğrulamaya izin olmadığı için Azure AD tabanlı kullanıcılar oluşturamıyor.

## <a name="provision-an-azure-active-directory-administrator-for-your-managed-instance"></a>Yönetilen Örneğiniz için bir Azure Active Directory Yöneticisi sağlama

> [!IMPORTANT]
> Yalnızca bir yönetilen örnek sağlıyorsanız bu adımları izleyin. Bu işlem Azure AD'de yalnızca genel/şirket yöneticisi tarafından gerçekleştirilebilir. Aşağıdaki adımlar, dizinde farklı ayrıcalıklara sahip kullanıcılar için izinleri verme işlemi açıklamaktadır.

Yönetilen örneğiniz başarıyla güvenlik grubu üyeliği aracılığıyla kullanıcı kimlik doğrulaması veya yeni kullanıcı oluşturma gibi görevleri gerçekleştirmek için Azure AD okumak için izinler gerekiyor. Bunun işe yaraması için yönetilen Azure AD'ye okumak için örneği için izinleri vermeniz gerekir. Bunu yapmanın iki yolu vardır: Portal ve PowerShell. Her iki yöntem de şu adımları.

1. Azure portalında, sağ üst köşede, açılan bir liste olası etkin dizinlerinin için bağlantınızı seçin.
2. Doğru Active Directory, varsayılan Azure AD seçin.

   Bu adımı her ikisi için aynı abonelik kullanılır sağlamaktan yönetilen örneği ile Active Directory ile ilişkili aboneliği bağlanan Azure AD ve yönetilen örneği.
3. Yönetilen örnek'e gidin ve Azure AD tümleştirmesi için kullanmak istediğiniz bir tane seçin.

   ![aad](./media/sql-database-aad-authentication/aad.png)

4. Active Directory Yönetim sayfası üzerinde bir başlığı seçin ve geçerli kullanıcıya izin verin. Size Global/şirket Yöneticisi Azure AD'de oturum açtıysanız, Azure portalı veya PowerShell betiğiyle kullanarak yapabilirsiniz.

    ![portal izinleri verme](./media/sql-database-aad-authentication/grant-permissions.png)

    ```powershell
    # Gives Azure Active Directory read permission to a Service Principal representing the Managed Instance.
    # Can be executed only by a "Company Administrator" or "Global Administrator" type of user.

    $aadTenant = "<YourTenantId>" # Enter your tenant ID
    $managedInstanceName = "MyManagedInstance"

    # Get Azure AD role "Directory Users" and create if it doesn't exist
    $roleName = "Directory Readers"
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    if ($role -eq $null) {
        # Instantiate an instance of the role template
        $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
        Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
        $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    }

    # Get service principal for managed instance
    $roleMember = Get-AzureADServicePrincipal -SearchString $managedInstanceName
    $roleMember.Count
    if ($roleMember -eq $null)
    {
        Write-Output "Error: No Service Principals with name '$    ($managedInstanceName)', make sure that managedInstanceName parameter was     entered correctly."
        exit
    }
    if (-not ($roleMember.Count -eq 1))
    {
        Write-Output "Error: More than one service principal with name pattern '$    ($managedInstanceName)'"
        Write-Output "Dumping selected service principals...."
        $roleMember
        exit
    }

    # Check if service principal is already member of readers role
    $allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    $selDirReader = $allDirReaders | where{$_.ObjectId -match     $roleMember.ObjectId}

    if ($selDirReader -eq $null)
    {
        # Add principal to readers role
        Write-Output "Adding service principal '$($managedInstanceName)' to     'Directory Readers' role'..."
        Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId     $roleMember.ObjectId
        Write-Output "'$($managedInstanceName)' service principal added to     'Directory Readers' role'..."

        #Write-Output "Dumping service principal '$($managedInstanceName)':"
        #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
        #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
    }
    else
    {
        Write-Output "Service principal '$($managedInstanceName)' is already     member of 'Directory Readers' role'."
    }
    ```

5. Aşağıdaki bildirim işlemi başarıyla tamamlandıktan sonra sağ üst köşede gösterilir:

    ![başarılı](./media/sql-database-aad-authentication/success.png)

6. Şimdi, yönetilen Örneğiniz için Azure AD yöneticinizin seçebilirsiniz. Bunun için Active Directory Yönetim sayfasında seçin **yönetici Ayarla** komutu.

    ![Yönetici Ayarla](./media/sql-database-aad-authentication/set-admin.png)

7. AAD yönetici sayfasında, bir kullanıcı için arama yönetici olacak kullanıcı veya grup seçin ve ardından **seçin**.

   Active Directory yönetici sayfasına, tüm üyeleri ve Active Directory gruplarını gösterir. Azure AD yönetici olarak desteklenmediğinden, kullanıcılar veya gruplar gri renkte seçilemez. Desteklenen Yöneticiler grubuna üye listesini [Azure AD özellikler ve sınırlamalar](sql-database-aad-authentication.md#azure-ad-features-and-limitations). Rol tabanlı erişim denetimi (RBAC), yalnızca Azure portalında geçerlidir ve SQL Server'a yayılan değil.

    ![Yönetici Ekle](./media/sql-database-aad-authentication/add-admin.png)

8. Active Directory yönetici sayfanın üst kısmında seçin **Kaydet**.

    ![Kaydet](./media/sql-database-aad-authentication/save.png)

    Yönetici değiştirme işlemini birkaç dakika sürebilir. Ardından yeni yönetici Active Directory Yönetici kutusunda görüntülenir.

Yönetilen Örneğiniz için bir Azure AD Yöneticisi sağladıktan sonra Azure AD sunucusu ilkeleri (oturum açma bilgileri) oluşturmaya başlayabilirsiniz (**genel Önizleme**) ile <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> söz dizimi. Daha fazla bilgi için [yönetilen örnek genel bakış](sql-database-managed-instance.md#azure-active-directory-integration).

> [!TIP]
> Daha sonra yönetici, Active Directory yönetici sayfasının üst kısmındaki kaldırmak için seçin **yönetici kaldırma**ve ardından **Kaydet**.

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Azure SQL veritabanı sunucunuz için bir Azure Active Directory Yöneticisi sağlama

> [!IMPORTANT]
> Yalnızca bir Azure SQL veritabanı sunucusu veya veri ambarı sağlıyorsanız bu adımları izleyin.

Aşağıdaki iki yordamdan Azure portalında ve PowerShell kullanarak Azure SQL Sunucunuz için bir Azure Active Directory Yöneticisi sağlama gösterilmektedir.

### <a name="azure-portal"></a>Azure portal

1. İçinde [Azure portalında](https://portal.azure.com/), sağ üst köşede, açılan bir liste olası etkin dizinlerinin için bağlantınızı seçin. Doğru Active Directory, varsayılan Azure AD seçin. Bu adım aboneliği ilişkili Active Directory ile Azure SQL server'ı her ikisi için aynı abonelik kullanılır sağlamaktan bağlar. Azure AD ve SQL Server. (Azure SQL server veya Azure SQL veritabanı, hem de Azure SQL veri ambarı barındıran olması.) ![ad seçin][8]

2. Sol başlığında seçin **tüm hizmetleri**ve filtre türü içinde **SQL server**. Seçin **Sql sunucuları**.

    ![sqlservers.png](media/sql-database-aad-authentication/sqlservers.png)

    >[!NOTE]
    > Bu sayfada daha önce seçtiğiniz **SQL sunucuları**, seçebileceğiniz **yıldız** için adının yanındaki *sık kullanılan* kategori ekleyin **SQL sunucuları**sol gezinti çubuğu.

3. Üzerinde **SQL Server** sayfasında **Active Directory Yöneticisi**.
4. İçinde **Active Directory Yöneticisi** sayfasında **yönetici Ayarla**.  ![active Directory'yi seçin](./media/sql-database-aad-authentication/select-active-directory.png)  

5. İçinde **yönetici Ekle** sayfasında aramak için bir kullanıcı, kullanıcı seçin veya yönetici grubunu ve ardından **seçin**. (Active Directory Yönetim sayfasında tüm üyeleri ve Active Directory gruplarını gösterir. Azure AD yönetici olarak desteklenmediğinden, kullanıcılar veya gruplar gri renkte seçilemez. (Desteklenen Yöneticiler grubuna üye listesini **Azure AD özellikler ve sınırlamalar** bölümünü [kullanımı Azure SQL veritabanı veya SQL veri ambarı ile kimlik doğrulaması için Active Directory kimlik](sql-database-aad-authentication.md).) Rol tabanlı erişim denetimi (RBAC), yalnızca portalı için geçerlidir ve SQL Server'a dağıtılmaz.
    ![Yönetici Seç](./media/sql-database-aad-authentication/select-admin.png)  

6. Üst kısmındaki **Active Directory Yöneticisi** sayfasında **Kaydet**.
    ![Yönetici Kaydet](./media/sql-database-aad-authentication/save-admin.png)

Yönetici değiştirme işlemini birkaç dakika sürebilir. Yeni yönetici görünür sonra **Active Directory Yöneticisi** kutusu.

   > [!NOTE]
   > Azure AD Yöneticisi ayarlarken, yeni yönetici adı (kullanıcı veya grup) zaten bir SQL Server kimlik doğrulaması kullanıcısı olarak sanal ana veritabanında bulunamaz. Varsa, Azure AD yönetici kurulumu başarısız olur; Bu oluşturma geri alınıyor ve bu tür bir yönetici (ad) zaten belirten var. Böyle bir SQL Server kimlik doğrulaması kullanıcı Azure AD parçası olmadığından, Azure AD kimlik doğrulamasını kullanarak sunucuya bağlanmak için her çabayı başarısız olur.

Üst kısmında bir yönetici, daha sonra kaldırmak için **Active Directory Yöneticisi** sayfasında **yönetici kaldırma**ve ardından **Kaydet**.

### <a name="powershell"></a>PowerShell

PowerShell cmdlet'lerini çalıştırmak için Azure PowerShell yüklenmiş ve çalışıyor olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Bir Azure AD Yöneticisi sağlamak için aşağıdaki Azure PowerShell komutları yürütün:

- Connect AzAccount
- AzSubscription seçin

Sağlama ve Azure AD Yöneticisi yönetmek için cmdlet'ler kullanılır:

| Cmdlet adı | Açıklama |
| --- | --- |
| [Set-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlserveractivedirectoryadministrator) |Azure SQL server veya Azure SQL veri ambarı için Azure Active Directory yönetici sağlar. (Geçerli abonelikten olması gerekir.) |
| [Remove-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlserveractivedirectoryadministrator) |Azure SQL server veya Azure SQL veri ambarı için Azure Active Directory yönetici kaldırır. |
| [Get-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator) |Şu anda Azure SQL veri ambarı ve Azure SQL sunucusu için yapılandırılmış bir Azure Active Directory Yöneticisi hakkında bilgi döndürür. |

Daha fazla bilgi için bu komutların her örneğin görmek için get-help PowerShell komutunu kullanın ``get-help Set-AzSqlServerActiveDirectoryAdministrator``.

Aşağıdaki betiği bir Azure AD Yönetici grubu adlı hükümlerine **DBA_Group** (nesne kimliği `40b79501-b343-44ed-9ce7-da4c8cc7353f`) için **demo_server** adlı bir kaynak grubunda sunucu **Grup-23**:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

**DisplayName** giriş parametresi kabul eden Azure AD görünen adını veya kullanıcı asıl adı. Örneğin, ``DisplayName="John Smith"`` ve ``DisplayName="johns@contoso.com"``. Azure AD grupları yalnızca Azure AD için görünen ad desteklenir.

> [!NOTE]
> Azure PowerShell komutunu ```Set-AzSqlServerActiveDirectoryAdministrator``` desteklenmeyen kullanıcılar için Azure AD yönetici sağlamasından engellemez. Desteklenmeyen bir kullanıcı sağlanabilir ancak bir veritabanına bağlanamıyor.

Aşağıdaki örnek isteğe bağlı kullanır **objectID**:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Azure AD **objectID** gerekli olduğunda **DisplayName** benzersiz değil. Alınacak **objectID** ve **DisplayName** değerleri, Klasik Azure portalı Active Directory bölümünü kullanın ve bir kullanıcının veya grubun özelliklerini görüntüleyin.

Aşağıdaki örnek geçerli hakkında bilgi verir. Azure SQL server için Azure AD Yöneticisi:

```powershell
Get-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Aşağıdaki örnek, bir Azure AD Yöneticisi kaldırır:

```powershell
Remove-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Azure Active Directory Yöneticisi REST API'lerini kullanarak da sağlayabilirsiniz. Daha fazla bilgi için [Hizmet Yönetimi REST API Başvurusu ve Azure SQL veritabanı için Azure SQL veritabanı işlemleri için işlemler](https://docs.microsoft.com/rest/api/sql/)

### <a name="cli"></a>CLI  

Aşağıdaki CLI komutları çağırarak, bir Azure AD Yöneticisi ayrıca sağlayabilirsiniz:

| Komut | Açıklama |
| --- | --- |
|[az sql server ad Yöneticisi oluşturma](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-create) |Azure SQL server veya Azure SQL veri ambarı için Azure Active Directory yönetici sağlar. (Geçerli abonelikten olması gerekir.) |
|[az sql server ad Yöneticisi Sil](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-delete) |Azure SQL server veya Azure SQL veri ambarı için Azure Active Directory yönetici kaldırır. |
|[az sql server yönetici ad listesi](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) |Şu anda Azure SQL veri ambarı ve Azure SQL sunucusu için yapılandırılmış bir Azure Active Directory Yöneticisi hakkında bilgi döndürür. |
|[az sql server ad Yöneticisi güncelleştirme](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-update) |Bir Azure SQL server veya Azure SQL veri ambarı için Active Directory Yöneticisi güncelleştirir. |

CLI komutları hakkında daha fazla bilgi için bkz. [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  

## <a name="configure-your-client-computers"></a>İstemci bilgisayarlarınızın yapılandırın

Tüm istemci makinelerde, uygulamalara veya kullanıcılara Azure SQL veritabanı veya Azure AD kimlikleri kullanarak Azure SQL veri ambarı bağlanmak, aşağıdaki yazılımları yüklemeniz gerekir:

- .NET framework 4.6 veya sonraki bir sürümü [ https://msdn.microsoft.com/library/5a4x27ek.aspx ](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- SQL Server için Azure Active Directory Authentication Library (**ADALSQL. DLL**) birden çok dilde kullanılabilir (x86 ve amd64) indirme merkezinde [Microsoft Active Directory Authentication Library Microsoft SQL Server](https://www.microsoft.com/download/details.aspx?id=48742).

Tarafından bu gereksinimleri karşılayabilirsiniz:

- Ya da yükleme [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) veya [Visual Studio 2015 için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx) .NET Framework 4.6 gereksinimini karşılar.
- SSMS yükler x86 sürümünü **ADALSQL. DLL**.
- SSDT amd64 sürümünü yükler **ADALSQL. DLL**.
- En son Visual Studio'dan [Visual Studio indirmeleri](https://www.visualstudio.com/downloads/download-visual-studio-vs) .NET Framework 4.6 gereksinimini karşılar, ancak gerekli amd64 sürümünü yüklemez **ADALSQL. DLL**.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Azure AD kimlikleri için eşlenmiş veritabanında bağımsız veritabanı kullanıcılarını oluşturun.

>[!IMPORTANT]
>Yönetilen örnek, artık Azure AD sunucu sorumlusu (oturum açma bilgileri) destekler (**genel Önizleme**), oturum açma bilgileri, Azure AD kullanıcıları, grupları veya uygulamaları oluşturmanıza olanak sağlar. Azure AD sunucusu ilkeleri (oturum açma bilgileri), yönetilen Örneğinize bağımsız veritabanı kullanıcısı oluşturulacak veritabanı kullanıcıları gerek kalmadan kimlik doğrulama yeteneği sağlar. Daha fazla bilgi için [yönetilen örnek genel bakış](sql-database-managed-instance.md#azure-active-directory-integration). Azure AD sunucusu ilkeleri (oturum açma bilgileri) oluşturma sözdizimi için bkz <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>.

Azure Active Directory kimlik doğrulaması bağımsız veritabanı kullanıcılarını oluşturulacak veritabanı kullanıcıları gerektirir. Bir Azure AD kimliğine göre bir bağımsız veritabanı kullanıcısı olan bir oturum açma ana veritabanında sahip olmayan bir veritabanı kullanıcısı ve veritabanı ile ilişkili Azure AD dizininde bir kimliğe eşler. Azure AD kimlik, tek bir kullanıcı hesabı veya grup olabilir. Kapsanan veritabanı kullanıcıları hakkında daha fazla bilgi için bkz: [bağımsız veritabanı kullanıcıları-yapma veritabanınızı taşınabilir](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Azure portalını kullanarak veritabanı kullanıcıları (yöneticiler dışında) oluşturulamaz. SQL Server, SQL veritabanı veya SQL veri ambarı için RBAC rollerini yayılmaz. Azure RBAC rolleri Azure kaynaklarını yönetmek için kullanılır ve veritabanı izinleri geçerli değildir. Örneğin, **SQL Server Katılımcısı** rol SQL veritabanı veya SQL veri ambarınıza bağlanmak için erişim izni yok. Transact-SQL deyimlerini kullanarak doğrudan veritabanında erişim izni verilmesi gerekir.
> [!WARNING]
> İki nokta üst üste gibi özel karakterler `:` ya da ve işareti `&` kullanıcı adları T-SQL CREATE LOGIN ve kullanıcı oluşturma deyimleri desteklenmiyor dahil olduğunda.

Bir Azure AD tabanlı yer alan veritabanı kullanıcısı (veritabanı sahibi Sunucu Yöneticisi dışında) oluşturun, bir Azure AD kimlik ile veritabanına sahip bir kullanıcı olarak bağlanmak için en az **ALTER herhangi bir kullanıcı** izni. Ardından aşağıdaki Transact-SQL söz dizimini kullanın:

```sql
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* bir Azure AD kullanıcısının kullanıcı asıl adı veya bir Azure AD grubunun görünen adı olabilir.

**Örnekler:** Kapsanan bir veritabanı oluşturmak için temsil eden bir Azure AD kullanıcı federe veya yönetilen etki alanı kullanıcısı:

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Azure AD'yi temsil eden ya da federasyonla yönetilen etki alanı grubu bağımsız veritabanı kullanıcısı oluşturmak için bir güvenlik grubu görünen adını sağlayın:

```sql
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

Bir Azure AD belirtecini kullanarak bağlanan bir uygulama temsil eden bir bağımsız veritabanı kullanıcısı oluşturmak için:

```sql
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

> [!TIP]
> Bir kullanıcı, Azure aboneliğinizle ilişkili Azure Active Directory dışındaki bir Azure Active Directory'den doğrudan oluşturamazsınız. Ancak, ilişkili Active Directory'de (dış kullanıcı olarak bilinir) içeri aktarılan kullanıcıları diğer Active dizinleri üyeleri, Active Directory kiracısındaki bir Active Directory grubuna eklenebilir. Bu AD grubunun bağımsız veritabanı kullanıcısı oluşturarak, dış Active Directory'den kullanıcıları SQL veritabanına erişebilir.

Yer alan oluşturma hakkında daha fazla bilgi için kullanıcılar Azure Active Directory kimliği temel veritabanı, bkz: [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Azure SQL server için Azure Active Directory Yöneticisi kaldırılıyor, herhangi bir Azure AD kimlik doğrulaması kullanıcı sunucusuna bağlanmasını engeller. Gerekirse, kullanılamaz, Azure AD kullanıcıları bırakılan el ile bir SQL veritabanı yöneticisi tarafından.
> [!NOTE]
> Alırsanız bir **bağlantı zaman aşımı süresi doldu**, ayarlamanız gerekebilir `TransparentNetworkIPResolution` yanlış bağlantı dizesi parametresi. Daha fazla bilgi için [bağlantı zaman aşımı .NET Framework 4.6.1 - TransparentNetworkIPResolution sorun](https://blogs.msdn.microsoft.com/dataaccesstechnologies/20../../connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).

Bir veritabanı kullanıcısı oluşturduğunuzda, o kullanıcının aldığı **CONNECT** izni ve bir üyesi olarak o veritabanına bağlanabilir **genel** rol. Başlangıçta yalnızca kullanıcılar için uygun izinleri verilmiş tüm izinleri olan **genel** rol veya üyesi olduğunuz tüm Azure AD gruplarına verilen tüm izinleri. Bir Azure AD tabanlı yer alan veritabanı kullanıcısı sağladıktan sonra başka bir kullanıcı türüne izin vermesi gibi kullanıcı ek, aynı şekilde izinler verebilirsiniz. Genellikle veritabanı rollere izin verme ve rollerine kullanıcı ekleme. Daha fazla bilgi için [veritabanı altyapısı izni Temelleri](https://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Özel bir SQL veritabanı rolleri hakkında daha fazla bilgi için bkz. [yönetme veritabanları ve Azure SQL veritabanı'nda oturum açma bilgileri](sql-database-manage-logins.md).
Bir dış kullanıcı olarak yönetilen bir etki alanı içeri aktarılan bir Federasyon etki alanı kullanıcı hesabı, yönetilen etki alanı kimliği kullanmalıdır.

> [!NOTE]
> Azure AD kullanıcıları, veritabanı meta türüyle E (EXTERNAL_USER) ve gruplar için X (EXTERNAL_GROUPS) tür ile işaretlenir. Daha fazla bilgi için [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
>

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>SSMS veya SSDT kullanarak kullanıcı veritabanını veya veri ambarı'na bağlanma  

Azure AD Yöneticisi ayarlandığından doğrulamak için bağlanın **ana** Azure AD yönetici hesabını kullanarak veritabanı.
Bir Azure AD tabanlı yer alan veritabanı kullanıcısı (veritabanı sahibi olan sunucu yönetici dışında) sağlamak için veritabanına erişimi olan bir Azure AD kimlik ile veritabanına bağlanın.

> [!IMPORTANT]
> Azure Active Directory kimlik doğrulaması için destek, kullanılabilir [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx) Visual Studio 2015'te. SSMS Ağustos 2016 sürümü, Active Directory Evrensel yöneticilerin multi-Factor Authentication kullanarak bir telefon araması, SMS mesajı, akıllı kart PIN ya da mobil uygulama bildirimi ile gerektiren olanak sağlayan kimlik doğrulaması için destek de içerir.

## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>Bir Azure AD kimlik kullanarak SSMS veya SSDT kullanarak bağlanma

Aşağıdaki yordamlar SQL Server Management Studio veya SQL Server Veritabanı Araçları'nı kullanarak bir Azure AD kimlik ile bir SQL veritabanına bağlanmak nasıl gösterir.

### <a name="active-directory-integrated-authentication"></a>Active Directory tümleşik kimlik doğrulaması

İçin Windows Azure Active Directory kimlik bilgilerinizi kullanarak bir Federasyon etki alanından oturum açarsanız, bu yöntemi kullanın.

1. Management Studio veya veri Araçları'nı başlatın ve **sunucuya Bağlan** (veya **veritabanı altyapısına Bağlan**) iletişim kutusundaki **kimlik doğrulaması** kutusunda  **Active Directory - tümleşik**. Parola gerekli olmadığı veya var olan kimlik bilgilerinizle bağlantı için sunulur çünkü girilebilir.

    ![AD tümleşik kimlik doğrulaması seçin][11]
2. Seçin **seçenekleri** düğmesi ve **bağlantı özellikleri** sayfasında **veritabanına bağlan** kutusuna, bağlanmak istediğiniz kullanıcı veritabanının adını yazın. ( **AD etki alanı adı veya Kiracı kimliği**"seçeneği için desteklenen yalnızca **MFA bağlantıyla Evrensel** seçenekleri, aksi takdirde, gri.)  

    ![Veritabanı adını seçin][13]

## <a name="active-directory-password-authentication"></a>Active Directory parola kimlik doğrulaması

Azure AD kullanarak bir Azure AD sorumlusu adıyla bağlanma yönetilen etki alanı, bu yöntemi kullanın. Örneğin uzaktan çalışıyorsanız, etki alanına erişim olmadan Federasyon hesapları için de kullanabilirsiniz.

İçin SQL DB/DW yerel için Azure AD ile kimlik doğrulaması için bu yöntemi kullanın ya da federasyonla yönetilen Azure AD kullanıcılarının. Yerel kullanıcı açıkça Azure AD'de oluşturulmuş ve Azure AD ile birleştirildiyse bir Federasyon kullanıcısı bir Windows kullanıcı olsa da, etki alanı kullanıcı adı ve parola kullanarak kimlik doğrulaması gerçekleştirilen. (Kullanıcı ve parola kullanarak) İkinci yöntem, bir kullanıcı kendi windows kimlik bilgileri kullanmak istiyor, ancak (örneğin, bir uzaktan erişim'i kullanarak) etki alanı ile kendi yerel makinesine katılmamış kullanılabilir. Bu durumda, bir Windows kullanıcı, etki alanı hesabı ve parolasını belirtebilirsiniz ve SQL DB/federe kimlik bilgilerini kullanarak DW'ye kimlik doğrulamasından geçmesini sağlayabilirsiniz.

1. Management Studio veya veri Araçları'nı başlatın ve **sunucuya Bağlan** (veya **veritabanı altyapısına Bağlan**) iletişim kutusundaki **kimlik doğrulaması** kutusunda  **Active Directory - parola**.
2. İçinde **kullanıcı adı** biçiminde Azure Active Directory kullanıcı adınızı yazın **kullanıcıadı\@etkialanı.com**. Bir etki alanından bir hesabı Azure Active Directory ile federasyona eklemek veya kullanıcı adları hesabınız Azure Active Directory'den olmalıdır.
3. İçinde **parola** kutusuna, Azure Active Directory hesabının, kullanıcı parolayı yazın ya da Federasyon etki alanı hesabı.

    ![AD parola kimlik doğrulaması][12]
4. Seçin **seçenekleri** düğmesi ve **bağlantı özellikleri** sayfasında **veritabanına bağlan** kutusuna, bağlanmak istediğiniz kullanıcı veritabanının adını yazın. (Önceki seçenek grafikte bakın.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Bir Azure AD kimlik kullanarak bir istemci uygulamasından bağlanma

Aşağıdaki yordamlar bir istemci uygulamasından bir Azure AD kimlik ile bir SQL veritabanına bağlanmak nasıl gösterir.

### <a name="active-directory-integrated-authentication"></a>Active Directory tümleşik kimlik doğrulaması

Tümleşik Windows kimlik doğrulaması kullanmak için Azure Active Directory ile Active Directory etki alanının federasyona eklenmesi gerekir. Veritabanına bağlanırken istemci uygulamanızın (veya hizmet) bir kullanıcının etki alanı kimlik bilgileri altında etki alanına katılmış bir makinede çalıştırılması gerekir.

Tümleşik kimlik doğrulaması ve Azure AD kimliğini kullanarak bir veritabanına bağlanmak için kimlik doğrulama veritabanı bağlantı dizesi anahtar sözcük Active Directory tümleşik için ayarlanmalıdır. Aşağıdaki C# kod örneği, ADO .NET kullanır.

```C#
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Bağlantı dizesi anahtar kelimesi ``Integrated Security=True`` Azure SQL veritabanı'na bağlanmak için desteklenmiyor. Bir ODBC bağlantısı yapılırken alanları kaldırın ve kimlik doğrulama 'ActiveDirectoryIntegrated' olarak ayarlanabilir gerekecektir.

### <a name="active-directory-password-authentication"></a>Active Directory parola kimlik doğrulaması

Tümleşik kimlik doğrulaması ve Azure AD kimliğini kullanarak bir veritabanına bağlanmak için kimlik doğrulaması anahtar sözcüğü için Active Directory parolası ayarlamanız gerekir. Bağlantı dizesi, kullanıcı kimliği/kullanıcı kimliği ve parola/PWD anahtar sözcükleri ve değerler içermelidir. Aşağıdaki C# kod örneği, ADO .NET kullanır.

```C#
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Kullanılabilir tanıtım kod örnekleri kullanarak Azure AD kimlik doğrulama yöntemleri hakkında daha fazla bilgi [Azure AD kimlik doğrulaması GitHub Tanıtımı](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD belirteç

Bu kimlik doğrulama yöntemi, orta katman Hizmetleri, Azure Active Directory (AAD gelen) bir belirteç elde ederek Azure SQL veritabanı veya Azure SQL veri ambarı bağlanmasına izin verir. Bu, sertifika tabanlı kimlik doğrulaması gibi karmaşık senaryolar sağlar. Azure AD belirteç kimlik doğrulaması kullanmak için dört temel adımları tamamlamanız gerekir:

1. Uygulamanızı Azure Active Directory ile kaydedin ve istemci kimliği için kodunuzu alın.
2. Uygulamayı temsil eden bir veritabanı kullanıcısı oluşturun. (6. adımda daha önce tamamlandı.)
3. İstemci bilgisayar çalışır, bir sertifika uygulama oluşturun.
4. Uygulamanız için bir anahtar olarak sertifika ekleyin.

Örnek bağlantı dizesi:

```c#
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
conn.AccessToken = "Your JWT token"
conn.Open();
```

Daha fazla bilgi için [SQL Server güvenlik Blog](https://blogs.msdn.microsoft.com/sqlsecurity/20../../token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/). Bir sertifika ekleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'de sertifika tabanlı kimlik doğrulamasını kullanmaya başlama](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).

### <a name="sqlcmd"></a>sqlcmd

Aşağıdaki deyimleri bağlanmak edinebileceğiniz sqlcmd 13.1 sürümünü kullanarak [İndirme Merkezi](https://go.microsoft.com/fwlink/?LinkID=825643).

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanında erişim ve denetime genel bakış için bkz. [SQL Veritabanında erişim ve denetim](sql-database-control-access.md).
- SQL Veritabanındaki oturum açma bilgileri, kullanıcılar ve veritabanı rollerine genel bakış için bkz. [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md).
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png
