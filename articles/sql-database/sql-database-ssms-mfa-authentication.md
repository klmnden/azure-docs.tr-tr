---
title: "Çok faktörlü kimlik doğrulama - Azure SQL | Microsoft Docs"
description: "Azure SQL Database ve Azure SQL Data Warehouse Active Directory Evrensel kimlik doğrulaması kullanarak SQL Server Management Studio (SSMS) gelen bağlantıları destekler."
services: sql-database
documentationcenter: 
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 09/29/2017
ms.author: mireks
ms.openlocfilehash: 3a950ac837b543cbcbfd4ae43cf4b9ebeb537cd5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>SQL Database ve SQL Data Warehouse (MFA desteği SSMS) ile Evrensel kimlik doğrulaması
Azure SQL Database ve Azure SQL Data Warehouse desteği SQL Server Management Studio (SSMS) kullanarak bağlantıları *Active Directory Evrensel kimlik doğrulaması*. 
**En son SSMS karşıdan** - istemci bilgisayarda SSMS, en son sürümünü indirin [karşıdan SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Tüm özellikler için bu konudaki, en az Temmuz 2017, sürüm 17,2 kullanın.  En son bağlantısı iletişim kutusunda, aşağıdaki gibi görünür: ![1mfa Evrensel bağlanmak](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "kullanıcı adı kutusunu tamamlar.")  

## <a name="the-five-authentication-options"></a>Beş kimlik doğrulama seçenekleri  
- Active Directory Evrensel kimlik doğrulamasını destekleyen iki etkileşimli olmayan kimlik doğrulama yöntemleri (`Active Directory - Password` kimlik doğrulama ve `Active Directory - Integrated` kimlik doğrulaması). Etkileşimli olmayan `Active Directory - Password` ve `Active Directory - Integrated` birçok farklı uygulamalarda (ADO.NET, JDBC, ODBC, vb.) kimlik doğrulama yöntemleri kullanılabilir. Bu iki yöntem hiçbir zaman açılır iletişim kutularında neden.

- `Active Directory - Universal with MFA` kimlik doğrulama yöntemidir de destekleyen bir etkileşimli *Azure çok faktörlü kimlik doğrulaması* (MFA). Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Kolay doğrulama seçeneklerini (telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi) aralıklı güçlü kimlik doğrulaması sunar tercih kullanıcıların yöntemini seçmenize olanak tanır. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir.

Çok faktörlü kimlik doğrulaması açıklaması için bkz: [çok faktörlü kimlik doğrulaması](../multi-factor-authentication/multi-factor-authentication.md).
Yapılandırma adımları için bkz: [SQL Server Management Studio Azure SQL veritabanını yapılandırma çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD etki alanı adı veya Kiracı kimliği parametresi   

İle başlayarak [SSMS sürümü 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), diğer Azure Active dizinlerde Konuk kullanıcılar olarak geçerli Active Directory'den içeri aktarılan kullanıcılar Azure AD etki alanı adı sağlayın veya bağlandıklarında kimlik Kiracı. Konuk kullanıcılar diğer Azure reklam, outlook.com, hotmail.com, live.com gibi Microsoft hesapları ya da gmail.com gibi diğer hesapları davet kullanıcıları içerir. Bu bilgiler verir **Active Directory Evrensel MFA kimlik doğrulaması ile** doğru kimlik doğrulama yetkisi tanımlamak için. Bu seçenek ayrıca outlook.com, hotmail.com, live.com veya MSA olmayan hesapları gibi Microsoft hesaplarını (MSA) desteklemek için gereklidir. Evrensel kimlik doğrulama kullanarak kimlik doğrulaması isteyen tüm bu kullanıcıların kendi Azure AD etki alanı adı girin veya Kiracı kimliği Bu parametre temsil geçerli Azure AD etki alanı adı/Kiracı kimliği Azure sunucusu ile bağlanır. Örneğin, Azure sunucusu Azure AD etki alanı ile ilişkili ise `contosotest.onmicrosoft.com` burada kullanıcı `joe@contosodev.onmicrosoft.com` Azure AD etki alanından alınan bir kullanıcı olarak barındırılan `contosodev.onmicrosoft.com`, etki alanı adı bu kullanıcının kimliğini doğrulamak için gerekli olan `contosotest.onmicrosoft.com`. Kullanıcının yerel bir kullanıcı Azure sunucusuna bağlı Azure ad ve bir MSA hesabı değil, etki alanı adı veya Kiracı Kimliği gereklidir. Parametre (SSMS sürümü 17,2 başlayarak), girmek için **veritabanına bağlan** iletişim kutusunda, tam iletişim kutusu seçme **Active Directory - Evrensel MFA ile** kimlik doğrulaması, tıklatın **Seçenekler**tam **kullanıcı adı** kutusuna ve ardından **bağlantı özelliklerini** sekmesi. Denetleme **AD etki alanı adı veya Kiracı kimliği** kutusunda ve etki alanı adı gibi kimlik doğrulama yetkisi sağlayın (**contosotest.onmicrosoft.com**) veya Kiracı kimliğini GUID  
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-to-business-support"></a>Azure AD işletmeler için destek   
Konuk kullanıcı olarak Azure AD B2B senaryolarını desteklenen azure AD kullanıcıları (bkz [Azure B2B işbirliği nedir](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) SQL Database ve SQL Data Warehouse için yalnızca geçerli Azure AD'de oluşturulan ve el ile eşleştirilmiş bir grubun üyeleri bir parçası olarak bağlanabilir Transact-SQL kullanarak `CREATE USER` verilen bir veritabanı deyiminde. Örneğin, varsa `steve@gmail.com` Azure AD ile davet `contosotest` (Azure Ad etki alanı ile `contosotest.onmicrosoft.com`), gibi Azure AD grup `usergroup` içeren Azure AD'de oluşturulan `steve@gmail.com` üyesi. Ardından, bu grubun belirli bir veritabanı (yani Veritabanım) için Azure AD SQL yönetici ya da Azure AD DBO tarafından bir Transact-SQL yürüterek oluşturulmalıdır `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` deyimi. Veritabanı kullanıcısı oluşturduktan sonra daha sonra kullanıcı `steve@gmail.com` oturum açabileceğiniz `MyDatabase` SSMS kimlik doğrulaması seçeneği kullanılarak `Active Directory – Universal with MFA support`. Usergroup varsayılan olarak, yalnızca connect izni ve normal şekilde verilmesi gerekir herhangi başka bir veri erişim vardır. Lütfen bu kullanıcının Not `steve@gmail.com` Konuk kullanıcı kutuyu işaretleyin ve AD etki alanı adı ekleme gibi `contosotest.onmicrosoft.com` SSMS içinde **bağlantı özelliği** iletişim kutusu. **AD etki alanı adı veya Kiracı kimliği** seçeneği MFA bağlantı seçenekleri ile Evrensel için desteklenen yalnızca, aksi takdirde, devre dışı.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>SQL Database ve SQL Data Warehouse için evrensel kimlik doğrulama kısıtlamaları
- SSMS ve SqlPackage.exe Active Directory Evrensel kimlik doğrulaması üzerinden MFA için şu anda etkin yalnızca araçlardır.
- SSMS sürümü 17,2, çok kullanıcılı eş zamanlı erişim MFA ile Evrensel kimlik doğrulaması kullanarak destekler. Sürüm 17,0 ve 17,1, Evrensel kimlik doğrulaması için tek bir Azure Active Directory hesabı kullanan SSMS öğesinin bir örneği için bir oturum açma kısıtlı. Başka bir Azure AD hesabı olarak oturum açmak için başka bir örneği SSMS kullanmanız gerekir. (Bu kısıtlama, Active Directory Evrensel kimlik doğrulaması için sınırlıdır; farklı sunuculara Active Directory parola kimlik doğrulaması, Active Directory tümleşik kimlik doğrulaması veya SQL Server kimlik doğrulaması kullanarak oturum açabilir).
- SSMS Nesne Gezgini, sorgu Düzenleyicisi'ni ve sorgu deposu görselleştirme için Active Directory Evrensel kimlik doğrulamasını destekler.
- SSMS sürümü 17,2 Extract/dışa aktarma/dağıtma veri veritabanı için DacFx Sihirbazı desteği sağlar. Belirli bir kullanıcı evrensel kimlik doğrulama, diğer tüm kimlik doğrulama yöntemleri için yaptığı biçimde DacFx Sihirbazı işlevleri kullanarak ilk kimlik doğrulaması iletişim kutusu üzerinden doğrulandıktan sonra.
- SSMS Tablo Tasarımcısı Evrensel kimlik doğrulamasını desteklemez.
- SSMS desteklenen bir sürümünü kullanmanız gerekir ancak bu Active Directory Evrensel kimlik doğrulaması için ek yazılım gereksinimi yoktur.  
- Evrensel kimlik doğrulaması için Active Directory Authentication Library (ADAL) sürüm en son ADAL.dll 3.13.9 kullanılabilir yayımlanmış sürümü için güncelleştirildi. Bkz: [Active Directory Authentication Library 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).  


## <a name="next-steps"></a>Sonraki adımlar

- Yapılandırma adımları için bkz: [SQL Server Management Studio Azure SQL veritabanını yapılandırma çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md).
- Başkalarının, veritabanına erişim izni vermek: [SQL veritabanı kimlik doğrulaması ve yetkilendirme: erişim verme](sql-database-manage-logins.md)  
- Diğer güvenlik duvarı üzerinden bağlanabilir emin olun: [Azure portalını kullanarak bir Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md)  
- [Yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulaması yönetme](sql-database-aad-authentication-configure.md)  
- [Microsoft SQL Server veri katmanı uygulaması çerçevesi (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Yeni bir Azure SQL veritabanı için bir BACPAC dosyasını içeri aktarın](../sql-database/sql-database-import.md)  
- [Bir Azure SQL veritabanı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md)  
- C# arabirimi [IUniversalAuthProvider arabirimi](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
- Kullanırken **Active Directory - Evrensel MFA ile** kimlik doğrulaması, ADAL izleme kullanılabilir başlayarak olduğunu [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Varsayılan olarak, ADAL izlemeyi kullanarak devre dışı bırakabilirsiniz **Araçları**, **seçenekleri** menüsü altında **Azure Hizmetleri**, **Azure bulut**,  **ADAL çıkış penceresi izleme düzeyini**, izlenen etkinleştirerek **çıkış** içinde **Görünüm** menüsü. İzlemeler çıktı penceresinde seçerken kullanılabilir **Azure Active Directory seçeneği**.  
