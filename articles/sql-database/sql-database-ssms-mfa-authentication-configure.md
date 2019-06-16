---
title: Çok faktörlü kimlik doğrulama - Azure SQL yapılandırma | Microsoft Docs
description: SQL veritabanı ve SQL veri ambarı için multi-factored Authentication SSMS ile kullanmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: b12508fbd0aec3bc6c697c920cd8ff94e3ac3396
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67119167"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma

Bu konuda, SQL Server Management Studio ile Azure Active Directory'ye multi factor authentication (MFA) kullanmayı gösterir. Azure AD MFA, SSMS veya SqlPackage.exe Azure'a bağlanılırken kullanılabilir [SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md). Azure SQL veritabanı çok faktörlü kimlik doğrulaması genel bakış için bkz. [Evrensel kimlik doğrulaması ile SQL veritabanı ve SQL veri ambarı'nı (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

## <a name="configuration-steps"></a>Yapılandırma adımları

1. **Azure Active Directory'yi yapılandırma** - daha fazla bilgi için bkz: [Azure AD dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md), [ Kendi etki alanı adınızı Azure AD'ye ekleme](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure artık Windows Server Active Directory ile Federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), ve [Windows PowerShell'i kullanarak Azure AD'yi yönetme](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Mfa'yı yapılandırma** - adım adım yönergeler için bkz: [Azure multi-Factor Authentication nedir?](../active-directory/authentication/multi-factor-authentication.md), [koşullu erişim (MFA) ile Azure SQL veritabanı ve veri ambarı](sql-database-conditional-access.md). (Tam koşullu erişim, bir Premium Azure Active Directory (Azure AD) gerektirir. Sınırlı MFA standart bir Azure AD ile kullanılabilir.)
3. **SQL veritabanı veya SQL veri ambarı için Azure AD kimlik doğrulaması yapılandırma** - adım adım yönergeler için bkz: [SQL veritabanı veya SQL veri ambarı Azure Active Directory kimlik doğrulamasını kullanarak bağlanma](sql-database-aad-authentication.md).
4. **SSMS'yi indirin** - istemci bilgisayarda, en son SSMS'yi indirin [indirme SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Tüm özellikler için bu konudaki en az Temmuz 2017, sürüm 17,2 kullanın.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Evrensel kimlik doğrulaması ile SSMS kullanarak bağlanma

Aşağıdaki adımlarda, en son SSMS kullanarak SQL veritabanı veya SQL veri ambarınıza bağlanmak gösterilmektedir.

1. Evrensel kimlik doğrulaması kullanarak bağlanmak için **sunucuya Bağlan** iletişim kutusunda **Active Directory - MFA desteğiyle Evrensel**. (Görürseniz **Active Directory Evrensel kimlik doğrulaması** SSMS'nin en son sürümüne bağımlı değildir.)  
   ![1mfa-universal-connect][1]  
2. Tamamlamak **kullanıcı adı** kutusu biçimde Azure Active Directory kimlik bilgileriyle `user_name@domain.com`.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Konuk kullanıcı olarak bağlanılıyorsa tıklatmalısınız **seçenekleri**ve **bağlantı özelliği** iletişim kutusu, tam **AD etki alanı adı veya Kiracı kimliği** kutusu. Daha fazla bilgi için [Evrensel kimlik doğrulaması ile SQL veritabanı ve SQL veri ambarı'nı (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md).
   ![mfa Kiracı ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. SQL veritabanı ve SQL veri ambarı için her zamanki şekilde tıklatmalısınız **seçenekleri** ve veritabanı belirtin **seçenekleri** iletişim kutusu. (Bağlı olan kullanıcı Konuk kullanıcı ise (yani joe@outlook.com), onay kutusunu işaretleyin ve geçerli bir AD etki alanı adı ekleyebilir veya seçenekleri bir parçası olarak Kiracı kimliği. Bkz: [Evrensel kimlik doğrulaması ile SQL veritabanı ve SQL veri ambarı'nı (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md). Ardından **Bağlan**’a tıklayın.  
5. Zaman **hesabınızda oturum** iletişim kutusu görüntülenirse, hesap ve parola, Azure Active Directory kimlik bilgilerinizi sağlayın. Parolasız bir kullanıcı Azure AD ile Federasyon etki alanının parçası ise gereklidir.  
   ![2mfa oturum açın][2]  

   > [!NOTE]
   > Evrensel kimlik doğrulaması için MFA gerektirmeyen bir hesap, bu noktada bağlanın. MFA gerektirme, kullanıcılar için aşağıdaki adımları uygulayın:
   >  
   
6. İki MFA Kurulum iletişim kutusu görünebilir. Bu bir defalık işlem ayarı MFA yönetici bağlıdır ve bu nedenle isteğe bağlı olabilir. MFA etkin bir etki alanı için bu adımı bazen önceden tanımlanmış olabilir (örneğin, etki alanı, kullanıcıların bir akıllı kart ve PIN gerektirir).  
   ![3mfa-setup][3]  
7. İkinci olası bir kez iletişim kutusunda, kimlik doğrulama yöntemi ayrıntılarını seçmenizi sağlar. Olası seçeneklerin yöneticiniz tarafından yapılandırılır.  
   ![4mfa-verify-1][4]  
8. Azure Active Directory onaylayan bilgileri size gönderir. Doğrulama kodunu aldığınızda buraya girin **doğrulama kodunu girin** ve'ı tıklatın **oturum**.  
   ![5mfa-verify-2][5]  

Doğrulama tamamlandığında, normal olarak geçerli kimlik bilgilerini ve Güvenlik Duvarı erişimi pek fazla SSMS bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Evrensel kimlik doğrulaması ile Azure SQL veritabanı çok faktörlü kimlik doğrulaması genel bakış için bkz. [SQL veritabanı ve SQL veri ambarı'nı (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md).  
- Diğerleri, veritabanınıza erişimi verin: [SQL veritabanı kimlik doğrulaması ve yetkilendirme: Erişim verme](sql-database-manage-logins.md)  
- Diğer güvenlik duvarı üzerinden bağlanabildiğinden emin olun: [Azure portalını kullanarak Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md)  
- Kullanırken **Active Directory - Evrensel MFA ile** kimlik doğrulaması, ADAL izleme ve sonraki sürümlerinde kullanılabilir olan [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Varsayılan olarak, ADAL izlemeyi kullanarak devre dışı bırakabilirsiniz **Araçları**, **seçenekleri** menüsü altında **Azure Hizmetleri**, **Azure bulut**,  **ADAL çıkış penceresi izleme düzeyini**etkinleştirme çizgidir **çıkış** içinde **görünümü** menüsü. İzlemeler çıktı penceresinde seçerken kullanılabilir **Azure Active Directory seçeneği**.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

