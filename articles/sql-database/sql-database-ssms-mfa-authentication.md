---
title: Azure SQL veritabanı ve Azure SQL veri ambarı ile AAD çok faktörlü kimlik doğrulaması kullanarak | Microsoft Docs
description: Azure SQL veritabanı ve Azure SQL veri ambarı SQL Server Management Studio (SSMS) gelen bağlantılar, Active Directory Evrensel kimlik doğrulaması kullanarak destekler.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
manager: craigg
ms.date: 10/08/2018
ms.openlocfilehash: ccb78e201b90dfc27f52523348e76da57087bcc8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59494910"
---
# <a name="using-multi-factor-aad-authentication-with-azure-sql-database-and-azure-sql-data-warehouse-ssms-support-for-mfa"></a>Azure SQL veritabanı ve Azure SQL veri ambarı'nı (MFA için SSMS desteği) ile AAD çok faktörlü kimlik doğrulaması kullanma
Azure SQL veritabanı ve Azure SQL veri ambarı SQL Server Management Studio (SSMS) kullanarak bağlantılar Destek *Active Directory Evrensel kimlik doğrulaması*. Bu makalede, çeşitli kimlik doğrulama seçenekleri ve evrensel kimlik doğrulaması kullanma ile ilgili sınırlamalar arasındaki farklar açıklanmaktadır. 

**En son SSMS'yi indirin** - istemci bilgisayarında, SSMS, en son sürümünü indirin [indirme SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). 


Bu makalede ele alınan tüm özellikler için en az Temmuz 2017, sürüm 17,2 kullanın.  En son bağlantı iletişim kutusu görünmelidir aşağıdaki görüntüye benzer:
 
  ![1mfa Evrensel bağlanma](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "kullanıcı adı kutusuna tamamlar.")  

## <a name="the-five-authentication-options"></a>Beş kimlik doğrulama seçenekleri  

Active Directory Evrensel kimlik doğrulaması iki etkileşimli olmayan kimlik doğrulama yöntemlerini destekler:
    - `Active Directory - Password` Kimlik doğrulaması
    - `Active Directory - Integrated` Kimlik doğrulaması

Birçok farklı uygulamalarda (ADO.NET, JDCB, ODC, vb.) kullanılabilecek iki etkileşimli olmayan kimlik doğrulama modeli de vardır. Bu iki yöntem açılan iletişim kutularında hiç sonuç: 
- `Active Directory - Password` 
- `Active Directory - Integrated` 

Etkileşimli yöntem aynı zamanda Azure multi-Factor Authentication (MFA) destekler şöyledir: 
- `Active Directory - Universal with MFA` 


Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Güçlü kimlik doğrulaması ile çeşitli kolay doğrulama seçenekleri (telefon araması, SMS mesajı, akıllı kart PIN ya da mobil uygulama bildirimi ile) teslim tercih ettikleri kullanıcıların yöntemi seçmesine izin verme. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir.

Multi-Factor Authentication açıklaması için bkz: [multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md).
Yapılandırma adımları için bkz. [SQL Server Management Studio için Azure SQL veritabanını Yapılandır multi-Factor authentication](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD etki alanı adı veya Kiracı kimliği parametresi   

İle başlayarak [SSMS sürümü 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), diğer Azure Active dizin Konuk kullanıcılar, geçerli Active Directory'den içeri aktarılan kullanıcılar Azure AD etki alanı adını belirtin veya bağlandıklarında Kiracı kimliği. Konuk kullanıcıları davet diğer Azure reklam, outlook.com, hotmail.com, live.com gibi Microsoft hesapları veya gmail.com gibi diğer hesap kullanıcıları dahil edin. Bu bilgiler sağlayan **Active Directory Evrensel MFA kimlik doğrulama ile** doğru kimlik doğrulama yetkisi tanımlamak için. Bu seçenek ayrıca outlook.com, hotmail.com, live.com veya MSA olmayan hesapları gibi Microsoft hesapları (MSA) desteklemek için gereklidir. Evrensel kimlik doğrulaması kullanarak kimlik doğrulaması isteyen bu tüm kullanıcılar kendi Azure AD etki alanı adı girin veya Kiracı kimliği. Bu parametre temsil geçerli Azure AD etki alanı adı/Kiracı kimliği Azure sunucusu ile bağlantılıdır. Örneğin, Azure sunucusu Azure AD etki alanı ile ilişkili ise `contosotest.onmicrosoft.com` burada kullanıcı `joe@contosodev.onmicrosoft.com` içeri aktarılan bir Azure AD etki alanı kullanıcı olarak barındırılan `contosodev.onmicrosoft.com`, bu kullanıcının kimliğini doğrulamak için gerekli etki alanı adı `contosotest.onmicrosoft.com`. Kullanıcı yerel bir kullanıcı Azure sunucusuna bağlı bir Azure ad ve bir MSA hesabı değil, etki alanı adı veya Kiracı Kimliği gereklidir. (SSMS sürümü 17,2 ile başlayan), parametre girmek için **veritabanına bağlan** iletişim kutusunda, iletişim kutusunda, tam seçerek **Active Directory - MFA ile Evrensel** kimlik doğrulaması tıklayın **Seçenekleri**tam **kullanıcı adı** kutusuna ve ardından **bağlantı özellikleri** sekmesi. Denetleme **AD etki alanı adı veya Kiracı kimliği** kutusuna ve etki alanı adı gibi kimlik doğrulama yetkisi sağlamak (**contosotest.onmicrosoft.com**) veya Kiracı kimliğinin bir GUID  
   ![mfa Kiracı ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-to-business-support"></a>Azure AD işletmeler arası destek   
Desteklenen konuk kullanıcıları Azure AD B2B senaryoları için azure AD kullanıcıları (bkz [Azure B2B işbirliği nedir](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) yalnızca geçerli Azure AD'de oluşturulan ve el ile eşlenmiş bir grubun üyelerinin bir parçası olarak SQL veritabanı ve SQL veri ambarına bağlanabilir Transact-SQL kullanarak `CREATE USER` deyimi içinde belirli bir veritabanı. Örneğin, varsa `steve@gmail.com` Azure AD'ye davet `contosotest` (Azure Ad etki alanı ile `contosotest.onmicrosoft.com`), gibi Azure AD grubunda `usergroup` içeren Azure AD'de oluşturulan `steve@gmail.com` üyesi. Ardından, bu grubun belirli bir veritabanı için (diğer bir deyişle, Veritabanım) Azure AD SQL Yöneticisi veya Azure AD DBO tarafından bir Transact-SQL yürüterek oluşturulmalıdır `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` deyimi. Veritabanı kullanıcısı oluşturduktan sonra ardından kullanıcı `steve@gmail.com` üzerinde oturum açabildiğinizden `MyDatabase` SSMS kimlik doğrulaması seçeneği kullanılarak `Active Directory – Universal with MFA support`. Usergroup varsayılan olarak, yalnızca connect iznine ve normal bir şekilde verilmesi gereken herhangi bir başka veri erişimi vardır. Kullanıcının Not `steve@gmail.com` Konuk kullanıcı kutuyu işaretleyin ve AD etki alanı adı ekleme gibi `contosotest.onmicrosoft.com` ssms'de **bağlantı özelliği** iletişim kutusu. **AD etki alanı adı veya Kiracı kimliği** seçeneği yalnızca evrensel MFA bağlantı seçenekleri için desteklenir, aksi takdirde, devre dışı.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>SQL veritabanı ve SQL veri ambarı için evrensel kimlik doğrulaması sınırlamaları
- SSMS ve SqlPackage.exe Active Directory Evrensel kimlik doğrulaması yoluyla MFA için şu anda etkin tek araçlardır.
- SSMS sürümü 17,2, MFA ile Evrensel kimlik doğrulaması kullanarak birden çok kullanıcı eş zamanlı erişimi destekler. Sürüm 17,0 ve 17,1, tek bir Azure Active Directory hesabına Evrensel kimlik doğrulaması ile SSMS örneği için bir oturum açma kısıtlı. Başka bir Azure AD hesabı olarak oturum açmak için SSMS başka bir örneğini kullanmanız gerekir. (Bu kısıtlama, Active Directory Evrensel kimlik doğrulaması sınırlıdır; farklı sunuculara Active Directory parola kimlik doğrulaması, Active Directory tümleşik kimlik doğrulaması veya SQL Server kimlik doğrulaması kullanarak oturum açın).
- SSMS, Nesne Gezgini, sorgu Düzenleyicisi'ni ve Query Store görselleştirme için Active Directory Evrensel kimlik doğrulamasını destekler.
- SSMS sürümü 17,2 dışarı aktarma/Extract/dağıtma veri veritabanı için DacFx sihirbaz desteği sağlar. Evrensel kimlik doğrulaması, diğer tüm kimlik doğrulama yöntemleri için yaptığı aynı şekilde DacFx Sihirbazı işlevleri kullanarak ilk kimlik doğrulaması iletişim kutusu üzerinden belirli bir kullanıcı kimlik doğrulaması gerçekleştikten sonra.
- SSMS Tablo Tasarımcısı Evrensel kimlik doğrulaması desteklemez.
- SSMS desteklenen bir sürümünü kullanmanız gerekir dışında Active Directory Evrensel kimlik doğrulaması için ek yazılım gereksinimi yoktur.  
- Evrensel kimlik doğrulaması için Active Directory Authentication Library (ADAL) sürümü, en son ADAL.dll 3.13.9 mevcut yayımlanmış sürümüne güncelleştirildi. Bkz: [Active Directory kimlik doğrulama kitaplığı 3.14.1](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).  


## <a name="next-steps"></a>Sonraki adımlar

- Yapılandırma adımları için bkz. [SQL Server Management Studio için Azure SQL veritabanını Yapılandır multi-Factor authentication](sql-database-ssms-mfa-authentication-configure.md).
- Diğerleri, veritabanınıza erişimi verin: [SQL veritabanı kimlik doğrulaması ve yetkilendirme: Erişim verme](sql-database-manage-logins.md)  
- Diğer güvenlik duvarı üzerinden bağlanabildiğinden emin olun: [Azure portalını kullanarak Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md)  
- [SQL Veritabanı veya SQL Veri Ambarı ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)  
- [Microsoft SQL Server veri katmanı uygulaması çerçevesi (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://docs.microsoft.com/sql/tools/sqlpackage)  
- [BACPAC dosyasını yeni bir Azure SQL Veritabanı’na içeri aktarma](../sql-database/sql-database-import.md)  
- [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md)  
- C# arabirimi [IUniversalAuthProvider arabirimi](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
- Kullanırken **Active Directory - Evrensel MFA ile** kimlik doğrulaması, ADAL izleme ve sonraki sürümlerinde kullanılabilir olan [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Varsayılan olarak, ADAL izlemeyi kullanarak devre dışı bırakabilirsiniz **Araçları**, **seçenekleri** menüsü altında **Azure Hizmetleri**, **Azure bulut**,  **ADAL çıkış penceresi izleme düzeyini**etkinleştirme çizgidir **çıkış** içinde **görünümü** menüsü. İzlemeler çıktı penceresinde seçerken kullanılabilir **Azure Active Directory seçeneği**.  
