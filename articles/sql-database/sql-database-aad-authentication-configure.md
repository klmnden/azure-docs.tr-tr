---
title: "Azure Active Directory kimlik doğrulaması - SQL yapılandırma | Microsoft Docs"
description: "SQL Database ve SQL Data Warehouse için Azure Active Directory kimlik doğrulaması kullanarak bağlanmak öğrenin."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 07/10/2017
ms.author: rickbyh
ms.openlocfilehash: f0c9578217beff22b4a322b363c7499943311d88
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulaması yönetme

Bu makalede oluşturun ve Azure AD doldurmak ve Azure SQL Database ve SQL Data Warehouse ile Azure AD kullanın gösterilmektedir. Genel bir bakış için bkz: [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Bir Azure Active Directory hesabı kullanarak bir Azure VM üzerinde çalışan SQL Server için bağlanması desteklenmiyor. Etki alanı Active Directory hesabı kullanın.

## <a name="create-and-populate-an-azure-ad"></a>Oluşturma ve Azure AD doldurma
Azure AD oluşturabilir ve kullanıcılar ve gruplar ile doldurabilirsiniz. Azure AD, ilk Azure AD olabilir yönetilen etki alanı. Azure AD, bir şirket içi Active Directory etki alanı birleştirildiyse Hizmetleri'nde Azure AD ile de olabilir.

Daha fazla bilgi edinmek için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/active-directory-aadconnect.md), [Kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure artık Windows Server Active Directory ile federasyonu destekliyor](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Azure AD dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Azure AD'yi Windows PowerShell kullanarak yönetme](/powershell/azure/overview?view=azureadps-2.0) ve [Karma Kimlik için gerekli bağlantı noktaları ve protokoller](../active-directory/active-directory-aadconnect-ports.md).

## <a name="optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>İsteğe bağlı: İlişkilendir ya da şu anda Azure aboneliğinizle ilişkili olan active directory değiştirme
Kuruluşunuz için Azure AD dizini veritabanınızı ilişkilendirmek için dizin veritabanını barındıran Azure aboneliği için güvenilen bir dizin oluşturun. Daha fazla bilgi için bkz. [Azure aboneliklerinin Azure AD ile ilişkisi](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Ek bilgi:** her Azure aboneliği Azure AD örneğiyle birlikte bir güven ilişkisi vardır. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Dizin, aboneliğinizde güvendiği görebilirsiniz **ayarları** adresindeki sekmesinde [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Aboneliğin bir dizinle arasındaki bu güven ilişkisi, bir aboneliğin daha çok abonelik alt kaynakları gibi olan, Azure'daki tüm diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili bu diğer kaynaklara erişim de durdurulur. Ancak dizin Azure içinde kalır, siz de başka bir aboneliği bu dizinle ilişkilendirebilir, dizin kullanıcılarını yönetmeye devam edebilirsiniz. Kaynaklar hakkında daha fazla bilgi için bkz: [azure'da kaynak erişimini anlama](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Aşağıdaki yordamlar belirli bir aboneliğe için ilişkili dizininin nasıl değiştirileceğini gösterir.
1. Bağlanma, [Klasik Azure portalı](https://manage.windowsazure.com/) Azure Abonelik Yöneticisi kullanarak.
2. Sol başlığında seçin **ayarları**.
3. Aboneliklerinizi ayarları ekranında görüntülenir. İstenen abonelik görünmüyorsa tıklatın **abonelikleri** en üstte açılan **FİLTRESİ tarafından dizin** kutusuna ve aboneliklerinizi içeren dizini seçin ve ardından tıklatın**UYGULA**.
   
    ![Abonelik seç][4]
4. İçinde **ayarları** alanında, aboneliğinizi tıklayın ve ardından **dizini Düzenle** sayfanın sonundaki.
   
    ![ad ayarları portalı][5]
5. İçinde **dizini Düzenle** kutusuna SQL Server veya SQL Data Warehouse ile ilişkili Azure Active Directory'yi seçin ve ardından sonraki oka tıklayın.
   
    ![Düzen dizini seçin][6]
6. İçinde **Onayla** dizin eşleme iletişim kutusunu Onayla "**tüm ortak Yöneticiler kaldırılır.**"
   
    ![Edit directory onaylayın][7]
7. Portal yeniden Denetle'yi tıklatın.

   > [!NOTE]
   > Ne zaman dizini, tüm ortak Yöneticiler, Azure AD kullanıcıları ve grupları, erişimi değiştirin ve yedeklenen dizin kaynak kullanıcıları kaldırılır ve artık bu abonelik veya kaynaklarına erişimi. Yalnızca, Hizmet Yöneticisi olarak, erişim için yeni dizinine göre ilkeleri yapılandırabilirsiniz. Bu değişiklik, tüm kaynaklara yaymak için önemli bir süre alabilir. Dizini değiştirme ayrıca SQL Database ve SQL Data Warehouse için Azure AD Yöneticisi değiştirir ve veritabanı erişimi var olan Azure AD kullanıcısı için izin verme. Azure AD yönetim olmalıdır (aşağıda açıklandığı gibi) sıfırlama ve yeni Azure AD kullanıcıları oluşturulmalıdır.
   >  

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Azure SQL server için Azure AD Yöneticisi oluşturma
(Bir SQL veritabanı ya da SQL veri ambarı barındıran) her Azure SQL sunucusu, tüm Azure SQL server'ın yönetici haklarına sahip tek sunucu yöneticisi hesabı ile başlar. İkinci bir SQL Server Yöneticisi, bir Azure AD hesabı olan oluşturulması gerekir. Bu asıl ana veritabanı bir kapsanan veritabanı kullanıcı olarak oluşturulur. Yönetici olarak, sunucu yönetici hesapları, üyesi **db_owner** her kullanıcı rolünde veritabanı ve her kullanıcı veritabanı olarak girin **dbo** kullanıcı. Sunucu yönetici hesapları hakkında daha fazla bilgi için bkz: [yönetme veritabanları ve oturum açma bilgileri Azure SQL veritabanında](sql-database-manage-logins.md).

Azure Active Directory coğrafi çoğaltma ile kullanırken, Azure Active Directory Yöneticisi hem birincil hem de ikincil sunucular için yapılandırılması gerekir. Bir sunucu bir Azure Active Directory yönetici sahip değilse daha sonra Azure Active Directory oturumları ve kullanıcıları bir "sunucu hatası bağlanılamıyor" alırlar.

> [!NOTE]
> (Azure SQL server yönetici hesabı dahil) bir Azure AD hesabında dayalı olmayan kullanıcılar önerilen veritabanı kullanıcıları Azure AD ile doğrulama izni olmadığı için Azure AD tabanlı kullanıcılar, oluşturamıyor.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Azure Active Directory yönetici Azure SQL sunucunuzun sağlama

Aşağıdaki iki yordamdan Azure portalında ve PowerShell kullanarak Azure SQL server için Azure Active Directory yönetici sağlama gösterir.

### <a name="azure-portal"></a>Azure portal
1. İçinde [Azure portal](https://portal.azure.com/), sağ üst köşede, açılan olası etkin dizinlerin bir liste için bağlantınızı tıklatın. Doğru Active Directory varsayılan olarak Azure AD seçin. Bu adım, her ikisi için aynı abonelik kullanılır emin Azure SQL server ile Active Directory ile abonelik ilişkisi bağlar. Azure AD ve SQL Server. (Azure SQL Azure SQL Database veya Azure SQL Data Warehouse barındırma sunucusu olması.)   
    ![ad seçin][8]   
    
2. Sol başlığında seçin **SQL sunucuları**seçin, **SQL server**ve ardından **SQL Server** dikey penceresinde tıklatın **Active Directory Yöneticisi** .   
3. İçinde **Active Directory Yöneticisi** dikey penceresinde tıklatın **ayarlamak yönetici**.   
    ![Active Directory'yi seçin](./media/sql-database-aad-authentication/select-active-directory.png)  
    
4. İçinde **yönetici ekleme** dikey penceresinde, bir kullanıcı ara seçin kullanıcı veya gruba bir yönetici olmanız ve ardından **seçin**. (Active Directory Yönetim dikey tüm üyeleri ve grupları, Active Directory gösterir. Kullanıcıları veya gri renkte grupları Azure AD Yöneticiler desteklenmediği için seçilemez. (Desteklenen admins listesini görmek **Azure AD özelliklerini ve sınırlamalarını** bölümünü [Azure Active Directory kimlik doğrulamasını kullan SQL Database veya SQL Data Warehouse ile kimlik doğrulaması için](sql-database-aad-authentication.md).) Rol tabanlı erişim denetimi (RBAC) yalnızca portalına uygular ve SQL Server'a dağıtılmaz.   
    ![yönetici seçin](./media/sql-database-aad-authentication/select-admin.png)  
    
5. Üstündeki **Active Directory Yöneticisi** dikey penceresinde tıklatın **KAYDETMEK**.   
    ![Yönetici Kaydet](./media/sql-database-aad-authentication/save-admin.png)   

Yönetici değiştirme işlemi birkaç dakika sürebilir. Yeni yönetici görünür sonra **Active Directory Yöneticisi** kutusu.

   > [!NOTE]
   > Azure AD yönetim ayarlarken, yeni yönetici adı (kullanıcı veya grup) zaten bir SQL Server kimlik doğrulama kullanıcı olarak sanal ana veritabanında var olamaz. Varsa, Azure AD yönetim kurulum başarısız olur; Bu oluşturma geri alınıyor ve böyle bir yöneticinin (ad) zaten belirten bulunmaktadır. Böyle bir SQL Server kimlik doğrulama kullanıcı Azure AD parçası olmadığından, Azure AD kimlik doğrulaması kullanarak sunucuya bağlanmak için tüm çaba başarısız olur.
   > 


Daha sonra en üstündeki bir yönetici kaldırmak için **Active Directory Yönetim** dikey penceresinde tıklatın **kaldırmak yönetici**ve ardından **kaydetmek**.

### <a name="powershell"></a>PowerShell
PowerShell cmdlet'lerini çalıştırmak için Azure PowerShell yüklenmiş ve çalışıyor olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Bir Azure AD yönetim sağlamak için aşağıdaki Azure PowerShell komutları yürütün:

* Add-AzureRmAccount
* Select-AzureRmSubscription

Sağlamak ve Azure AD yönetim yönetmek için cmdlet'ler kullanılır:

| Cmdlet adı | Açıklama |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Azure SQL server veya Azure SQL Data Warehouse için Azure Active Directory yönetici sağlar. (Geçerli abonelikten olması gerekir.) |
| [Remove-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Azure SQL server veya Azure SQL Data Warehouse için Azure Active Directory yönetici kaldırır. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |Azure SQL sunucusu veya Azure SQL Data Warehouse için yapılandırılmış bir Azure Active Directory Yöneticisi hakkında bilgi verir. |

Örneğin her bu komutları için daha fazla ayrıntı görmek için PowerShell komutu get-help kullanın ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Aşağıdaki komut dosyası Azure AD yönetici grubuna adlı hükümleri **DBA_Group** (nesne kimliği `40b79501-b343-44ed-9ce7-da4c8cc7353f`) için **demo_server** adlı bir kaynak grubunda sunucu **Grup-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

**DisplayName** giriş parametresi Azure AD görünen adı veya kullanıcı asıl adını kabul eder. Örneğin, ``DisplayName="John Smith"`` ve ``DisplayName="johns@contoso.com"``. Azure AD grupları yalnızca Azure AD görünen adı desteklenir.

> [!NOTE]
> Azure PowerShell komutunu ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` desteklenmeyen kullanıcılar için Azure AD yöneticileri sağlama engellemez. Desteklenmeyen bir kullanıcı sağlanabilir, ancak bir veritabanına bağlanamıyor. 
> 

Aşağıdaki örnek isteğe bağlı kullanır **objectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Azure AD **objectID** gerekli olduğunda **DisplayName** benzersiz değil. Alınacak **objectID** ve **DisplayName** değerleri, Klasik Azure portalı Active Directory bölümünü kullanın ve bir kullanıcının veya grubun özelliklerini görüntüleyin.
> 

Aşağıdaki örnek geçerli hakkında bilgi verir Azure SQL server için Azure AD yönetim:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Aşağıdaki örnek, Azure AD yönetici kaldırır:

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Azure Active Directory yönetici REST API'lerini kullanarak da sağlayabilirsiniz. Daha fazla bilgi için bkz: [Hizmet Yönetimi REST API Başvurusu ve Azure SQL veritabanları için Azure SQL veritabanı işlemleri için işlemler](https://msdn.microsoft.com/library/azure/dn505719.aspx)

### <a name="cli"></a>CLI  
Bir Azure AD yönetim aşağıdaki CLI komutları çağırarak de sağlayabilirsiniz:
| Komut | Açıklama |
| --- | --- |
|[az sql server ad-Yöneticisi oluşturma](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_create) |Azure SQL server veya Azure SQL Data Warehouse için Azure Active Directory yönetici sağlar. (Geçerli abonelikten olması gerekir.) |
|[az sql server yönetici ad silme](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_delete) |Azure SQL server veya Azure SQL Data Warehouse için Azure Active Directory yönetici kaldırır. |
|[az sql server yönetici ad listesi](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_list) |Azure SQL sunucusu veya Azure SQL Data Warehouse için yapılandırılmış bir Azure Active Directory Yöneticisi hakkında bilgi verir. |
|[az sql server ad Yönetim Güncelleştirme](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_update) |Bir Azure SQL sunucusu veya Azure SQL Data Warehouse için Active Directory Yöneticisi güncelleştirir. |

CLI komutları hakkında daha fazla bilgi için bkz: [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  


## <a name="configure-your-client-computers"></a>İstemci bilgisayarları yapılandırın
Uygulama veya kullanıcıların Azure SQL veritabanı veya Azure SQL Data Warehouse kullanarak Azure AD kimlikleri bağlanmak tüm istemci makinelerde aşağıdaki yazılımları yüklemeniz gerekir:

* .NET framework 4.6 veya sonrası gelen [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* SQL Server için Azure Active Directory kimlik doğrulama kitaplığı (**ADALSQL. DLL**) birden çok dilde kullanılabilir (x86 ve amd64) İndirme Merkezi'nden [Microsoft Active Directory kimlik doğrulama kitaplığı için Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

Tarafından bu gereksinimleri karşılayabilirsiniz:

* Ya da yükleme [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) veya [SQL Server veri araçları Visual Studio 2015 için](https://msdn.microsoft.com/library/mt204009.aspx) .NET Framework 4.6 gereksinimini karşılar.
* SSMS yükler x86 sürümü **ADALSQL. DLL**.
* SSDT amd64 sürümünü yükler **ADALSQL. DLL**.
* En son Visual Studio'dan [Visual Studio indirmeleri](https://www.visualstudio.com/downloads/download-visual-studio-vs) .NET Framework 4.6 gereksinimini karşılıyor ancak gerekli amd64 sürümü yüklemez **ADALSQL. DLL**.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Azure AD kimlikleri eşlenen veritabanınızdaki kapsanan veritabanı kullanıcıları oluşturun

Azure Active Directory kimlik doğrulaması kapsanan veritabanı kullanıcıları oluşturulacak veritabanı kullanıcıları gerektirir. Bir Azure AD kimliğine göre bir kapsanan veritabanı kullanıcısı olan bir oturum açma ana veritabanında sahip olmayan bir veritabanı kullanıcısı ve veritabanı ile ilişkili Azure AD dizininde bir kimliğine eşler. Azure AD kimlik bireysel bir kullanıcı hesabı veya bir grup olabilir. Kapsanan veritabanı kullanıcıları hakkında daha fazla bilgi için bkz: [bulunan veritabanı kullanıcıları yapma bilgisayarınızı veritabanı taşınabilir](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Veritabanı kullanıcıları (yöneticiler dışında) portal kullanarak oluşturulamıyor. SQL Server, SQL Database veya SQL Data Warehouse için RBAC rolleri yayılmaz. Azure RBAC rolleri Azure kaynaklarını yönetmek için kullanılır ve veritabanı izinleri için geçerli değildir. Örneğin, **SQL Server Katılımcısı** rolü, SQL Database veya SQL Data Warehouse bağlanmak için erişim vermez. Transact-SQL deyimlerini kullanarak veritabanına doğrudan erişim izni verilmesi gerekir.
>

Bir Azure AD tabanlı bulunan veritabanı (başka kullanıcı veritabanı sahibi Sunucu Yöneticisi) oluşturmak, bir Azure AD kimlik veritabanıyla olan bir kullanıcı olarak bağlanmak için en az **ALTER herhangi bir kullanıcı** izni. Ardından aşağıdaki Transact-SQL söz dizimini kullanın:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* bir Azure AD kullanıcısının kullanıcı asıl adını veya Azure AD grubundaki görünen adı olabilir.

**Örnekler:** kapsanan bir veritabanı oluşturmak için Azure AD temsil eden kullanıcı federe veya yönetilen etki alanı kullanıcısı:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Azure AD temsil eden veya etki alanı grubu federe bir kapsanan veritabanı kullanıcısı oluşturmak için bir güvenlik grubu görünen adını sağlayın:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

Bir Azure AD belirteci kullanarak bağlanan bir uygulama temsil eden bir kapsanan veritabanı kullanıcısı oluşturmak için:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  Bir kullanıcı, Azure aboneliğinizle ilişkili Azure Active Directory dışındaki bir Azure Active Directory'den doğrudan oluşturulamıyor. Ancak, ilişkili Active Directory'de (dış kullanıcılar olarak da bilinir) içeri aktarılan kullanıcılar olan diğer etkin dizinleri üyeleri Active Directory kiracısındaki bir Active Directory grubuna eklenebilir. Bu AD grubu için kapsanan veritabanı kullanıcı oluşturarak, dış Active Directory'den kullanıcıları SQL veritabanına erişebilir.   

Bulunan oluşturma hakkında daha fazla bilgi için veritabanı kullanıcıları Azure Active Directory kimliği temel, bakın [kullanıcı oluştur (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Azure SQL server için Azure Active Directory Yöneticisi kaldırma, herhangi bir Azure AD kimlik doğrulama kullanıcı sunucusuna bağlanmasını engeller. Gerekli olduğunda, kullanılamaz Azure AD kullanıcıları bırakılan el ile bir SQL veritabanı yöneticisi tarafından.   

>  [!NOTE]
>  Alırsanız, bir **bağlantı zaman aşımı süresi doldu**, ayarlamanız gerekebilir `TransparentNetworkIPResolution` yanlış bağlantı dizesi parametresi. Daha fazla bilgi için bkz: [bağlantı zaman aşımı .NET Framework 4.6.1 - TransparentNetworkIPResolution sorun](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).   

   
Bir veritabanı kullanıcısı oluşturduğunuzda, bu kullanıcının aldığı **BAĞLAN** izni ve bir üyesi olarak o veritabanına bağlanabilir **ortak** rol. Başlangıçta yalnızca kullanıcı tarafından kullanılabilir verilen izinler izinlerdir **ortak** rol ya da bir üyesi olan herhangi bir Azure AD gruba verilen tüm izinleri. Bir Azure AD tabanlı bulunan veritabanı kullanıcı sağlamak sonra başka bir kullanıcı türüne izni gibi kullanıcı ek, aynı şekilde izin verebilirsiniz. Genellikle veritabanı rolleri izinleri ve kullanıcıları rollere ekleyin. Daha fazla bilgi için bkz: [veritabanı altyapısı izni Temelleri](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Özel SQL veritabanı rolleri hakkında daha fazla bilgi için bkz: [yönetme veritabanları ve oturum açma bilgileri Azure SQL veritabanında](sql-database-manage-logins.md).
Yönetilen bir etki alanı bir dış kullanıcı olarak içeri aktarılan bir Federasyon etki alanı kullanıcı hesabı yönetilen etki alanı kimliği kullanmanız gerekir.

> [!NOTE]
> Azure AD kullanıcıları veritabanı meta verilerde E (EXTERNAL_USER) türüyle ve gruplar için türü X (EXTERNAL_GROUPS) ile işaretlenir. Daha fazla bilgi için bkz: [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Kullanıcı veritabanı veya veri ambarı SSMS veya SSDT kullanarak bağlanın  
Azure AD Yöneticisi düzgün ayarlandığından onaylamak için bağlanmak **ana** Azure AD yönetici hesabını kullanarak veritabanı.
Bir Azure AD tabanlı bulunan veritabanı (başka kullanıcı veritabanı sahibi Sunucu Yöneticisi) sağlamak için veritabanına erişimi olan bir Azure AD kimlik veritabanıyla bağlayın.

> [!IMPORTANT]
> Azure Active Directory kimlik doğrulaması için destek ile kullanılabilir [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx) Visual Studio 2015'te. SSMS Ağustos 2016 sürümü, Active Directory Evrensel yöneticilerin çok faktörlü kimlik doğrulaması kullanarak bir telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi gerektiren olanak sağlayan kimlik doğrulaması için destek de içerir.
 
## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>SSMS veya SSDT kullanarak bağlanmak için bir Azure AD kimlik kullanma  

Aşağıdaki yordamlar SQL Server Management Studio veya SQL Server veritabanı araçları kullanarak bir Azure AD kimlik ile bir SQL veritabanına bağlanma gösterir.

### <a name="active-directory-integrated-authentication"></a>Active Directory tümleşik kimlik doğrulaması

Bir Federasyon etki alanına ait Azure Active Directory kimlik bilgilerinizi kullanarak Windows'a oturum açarsanız, bu yöntemi kullanın.

1. Management Studio veya veri araçları başlatın ve **sunucuya Bağlan** (veya **veritabanı motoruna Bağlan**) iletişim kutusunda **kimlik doğrulaması** kutusunda  **Active Directory - tümleşik**. Parola gereklidir veya var olan kimlik bilgilerinizle bağlantı için sunulur çünkü girilebilir.   

    ![AD tümleşik kimlik doğrulaması seçin][11]
2. Tıklatın **seçenekleri** düğmesi ve **bağlantı özelliklerini** sayfasında **veritabanına bağlan** kutusuna, bağlanmak istediğiniz kullanıcı veritabanının adını yazın. ( **AD etki alanı adı veya Kiracı kimliği**"seçeneği için desteklenen yalnızca **MFA bağlantısıyla Evrensel** seçenekleri, aksi takdirde, griyse.)  

    ![Veritabanı adını seçin][13]

## <a name="active-directory-password-authentication"></a>Active Directory parola kimlik doğrulaması

Azure AD kullanarak bir Azure AD asıl adı ile bağlanma etki alanı yönetildiğinde bu yöntemi kullanın. Uzaktan çalışırken, örneğin etki alanı erişimi olmadan birleştirilmiş hesap için de kullanabilirsiniz.

Windows Azure ile Federasyon olmayan bir etki alanı kimlik bilgilerini kullanarak oturum açarsanız, ya da Azure AD kullanarak Azure AD kimlik doğrulaması kullanarak ilk veya istemci etki alanına göre bu yöntemi kullanın.

1. Management Studio veya veri araçları başlatın ve **sunucuya Bağlan** (veya **veritabanı motoruna Bağlan**) iletişim kutusunda **kimlik doğrulaması** kutusunda  **Active Directory - parola**.
2. İçinde **kullanıcı adı** biçiminde Azure Active Directory kullanıcı adınızı yazın  **username@domain.com** . Bir etki alanındaki bir hesabı Azure Active Directory ile birleştirmek veya bu Azure Active Directory'den bir hesabı olmalıdır.
3. İçinde **parola** kutusunda, Azure Active Directory hesabı için kullanıcı parolanızı yazın veya Federasyon etki alanı hesabı.

    ![AD parola kimlik doğrulaması][12]
4. Tıklatın **seçenekleri** düğmesi ve **bağlantı özelliklerini** sayfasında **veritabanına bağlan** kutusuna, bağlanmak istediğiniz kullanıcı veritabanının adını yazın. (Bkz. önceki seçenek grafikte.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Bir istemci uygulamadan bağlanmak için bir Azure AD kimlik kullanma

Aşağıdaki yordamlar bir istemci uygulamadan bir Azure AD kimlik olan bir SQL veritabanına bağlanma gösterir.

###  <a name="active-directory-integrated-authentication"></a>Active Directory tümleşik kimlik doğrulaması

Tümleşik Windows kimlik doğrulaması kullanmak için Azure Active Directory ile etki alanınızın Active Directory Federasyon gerekir. Veritabanına bağlanırken istemci uygulamanız (veya bir hizmeti), bir etki alanına katılmış makinede kullanıcının etki alanı kimlik bilgileri altında çalışmalıdır.

Tümleşik kimlik doğrulaması ve Azure AD kimlik kullanarak bir veritabanına bağlanmak için kimlik doğrulama anahtar sözcüğü veritabanı bağlantı dizesinde Active Directory tümleşik için ayarlanmalıdır. Aşağıdaki C# kod örneği ADO .NET kullanır.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Bağlantı dizesi anahtar sözcüğü ``Integrated Security=True`` Azure SQL veritabanına bağlanmak için desteklenmiyor. ODBC bağlantısı yaparken boşlukları kaldırın ve kimlik doğrulama 'ActiveDirectoryIntegrated' olarak ayarlanabilir gerekecektir.

### <a name="active-directory-password-authentication"></a>Active Directory parola kimlik doğrulaması

Tümleşik kimlik doğrulaması ve Azure AD kimlik kullanarak bir veritabanına bağlanmak için Active Directory parola kimlik doğrulaması anahtar sözcüğü ayarlanması gerekir. Bağlantı dizesi, kullanıcı kimliği/kullanıcı kimliği ve parola/PWD anahtar sözcükleri ve değerleri içermesi gerekir. Aşağıdaki C# kod örneği ADO .NET kullanır.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Kullanarak gösteri kod örnekleri adresinde Azure AD kimlik doğrulama yöntemleri hakkında daha fazla bilgi [Azure AD kimlik doğrulama GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD simgesi
Bu kimlik doğrulama yöntemini orta katman Hizmetleri Azure Active Directory (AAD gelen) bir belirteç elde ederek Azure SQL Database veya Azure SQL Data Warehouse bağlanmasına izin verir. Sertifika tabanlı kimlik doğrulaması dahil olmak üzere Gelişmiş senaryoları etkinleştirir. Azure AD belirteci kimlik doğrulaması kullanmak için dört temel adımları tamamlamanız gerekir:

1. Azure Active Directory ile uygulamanızı kaydetme ve kodunuz için istemci kimliği alın. 
2. Uygulamayı temsil eden bir veritabanı kullanıcısı oluşturmalıdır. (6. adımda daha önce tamamlandı.)
3. Bir sertifika istemci bilgisayar çalışır uygulama oluşturun.
4. Uygulamanız için bir anahtar olarak sertifika ekleyin.

Örnek bağlantı dizesi:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Daha fazla bilgi için bkz: [SQL Server güvenlik blogu](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/). Bir sertifika ekleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'de sertifika tabanlı kimlik doğrulaması kullanmaya başlama](../active-directory/active-directory-certificate-based-authentication-get-started.md).

### <a name="sqlcmd"></a>sqlcmd

Aşağıdaki deyimleri bağlanmak kullanılabilir sqlcmd 13,1 sürümü kullanılarak [Yükleme Merkezi'nden](http://go.microsoft.com/fwlink/?LinkID=825643).

```
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

