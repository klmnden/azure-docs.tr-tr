---
title: "Çok faktörlü kimlik doğrulama - Azure SQL yapılandırma | Microsoft Docs"
description: "SQL Database ve SQL Data Warehouse için SSMS ile Multi-Factored kimlik doğrulaması kullanmayı öğrenin."
services: sql-database
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 09/27/2017
ms.author: mireks
ms.openlocfilehash: 7b74cd6b62686fd03d9f42316701f44daf99eed8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma

Bu konuda SQL Server Management Studio ile Azure Active Directory'ye multi factor authentication (MFA) kullanmayı gösterir. Azure AD MFA SSMS veya SqlPackage.exe Azure SQL Database ve Azure SQL Data Warehouse bağlanırken kullanılabilir.

Azure SQL veritabanı çok faktörlü kimlik doğrulamasına genel bakış için bkz: [Evrensel kimlik doğrulaması SQL Database ve SQL Data Warehouse (MFA desteği SSMS) ile](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Yapılandırma adımları

1. **Bir Azure Active Directory yapılandırma** - daha fazla bilgi için bkz: [Azure AD dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/active-directory-aadconnect.md), [ Kendi etki alanı adını Azure AD'ye ekleme](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure artık Windows Server Active Directory ile Federasyon destekler](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), ve [Yönet Windows PowerShell kullanarak Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **MFA yapılandırma** - adım adım yönergeler için bkz: [Azure multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md), [koşullu erişim (MFA) ile Azure SQL veritabanı ve veri ambarı](sql-database-conditional-access.md). (Bir Premium Azure Active Directory (Azure AD) tam koşullu erişim gerektirir. Sınırlı MFA standart bir Azure AD ile kullanılabilir.)
3. **SQL Database veya SQL Data Warehouse Azure AD kimlik doğrulaması için yapılandırma** - adım adım yönergeler için bkz: [SQL Database'e veya SQL veri ambarı tarafından kullanarak Azure Active Directory kimlik doğrulaması için bağlanma](sql-database-aad-authentication.md).
4. **SSMS karşıdan** - istemci bilgisayarda, en son SSMS indirin [karşıdan SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Tüm özellikler için bu konudaki, en az Temmuz 2017, sürüm 17,2 kullanın.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>SSMS ile Evrensel kimlik doğrulaması kullanarak bağlanma

Aşağıdaki adımlar en son SSMS kullanarak SQL veritabanına veya SQL Data Warehouse bağlanmak nasıl gösterir.

1. Evrensel kimlik doğrulaması kullanarak bağlanmak için **sunucuya Bağlan** iletişim kutusunda **Active Directory - Evrensel MFA desteğiyle**. (Görürseniz **Active Directory Evrensel kimlik doğrulaması** SSMS son sürümü değildir.)  
   ![1mfa-universal-connect][1]  
2. Tamamlamak **kullanıcı adı** biçimde Azure Active Directory kimlik kutusuyla `user_name@domain.com`.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Konuk kullanıcı olarak bağlanıyorsanız tıklatmalısınız **seçenekleri**ve **bağlantı özelliği** iletişim kutusu, tam **AD etki alanı adı veya Kiracı kimliği** kutusu. Daha fazla bilgi için bkz: [Evrensel kimlik doğrulaması SQL Database ve SQL Data Warehouse (MFA desteği SSMS) ile](sql-database-ssms-mfa-authentication.md).
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. SQL Database ve SQL Data Warehouse için her zamanki gibi tıklatmalısınız **seçenekleri** ve veritabanını belirtin **seçenekleri** iletişim kutusu. (Bağlı olan kullanıcı Konuk kullanıcı ise (yani joe@outlook.com), onay kutusunu işaretleyin ve geçerli AD etki alanı adı ekleyebilir veya seçenekleri bir parçası olarak kimliği Kiracı. Bkz: [Evrensel kimlik doğrulaması SQL Database ve SQL Data Warehouse (MFA desteği SSMS) ile]()(sql-veritabanı-ssms-mfa-authentication.md. Ardından **Bağlan**.  
5. Zaman **hesabınızda oturum** iletişim kutusu görüntülenirse, hesap ve parola, Azure Active Directory kimlik bilgilerinizi sağlayın. Parolasız bir kullanıcı Azure AD ile federe bir etki alanının parçası ise gereklidir.  
   ![2mfa-sign-in][2]  

   > [!NOTE]
   > MFA gerektirmeyen bir hesapla Evrensel kimlik doğrulaması için bu noktada bağlayın. MFA gerektirme kullanıcılar için aşağıdaki adımları uygulayın:
   >  
   
6. İki MFA Kurulum iletişim kutusu görünebilir. Bu bir kez işlem ayarı MFA yönetici bağlıdır ve bu nedenle isteğe bağlı olabilir. MFA etkinse etki alanı için bu adımı bazen önceden tanımlanmış (örneğin, etki alanı kullanıcılarının bir akıllı kart ve PIN kullanmasını gerektirir).  
   ![3mfa-setup][3]  
7. İkinci olası bir kez iletişim kutusu, kimlik doğrulama yöntemini ayrıntılarını seçmenize olanak sağlar. Olası seçenekler, yöneticiniz tarafından yapılandırılır.  
   ![4mfa doğrula 1][4]  
8. Azure Active Directory onaylayan bilgileri size gönderir. Doğrulama kodunu aldığınızda buraya girin **doğrulama kodunu girin** ve'ı tıklatın **oturum**.  
   ![5mfa doğrula 2][5]  

Doğrulama tamamlandıktan sonra normal olarak geçerli kimlik bilgileri ve güvenlik duvarı erişim pek fazla SSMS bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Evrensel kimlik doğrulaması ile Azure SQL veritabanı çok faktörlü kimlik doğrulamasına genel bakış için bkz: [SQL Database ve SQL Data Warehouse (MFA desteği SSMS)](sql-database-ssms-mfa-authentication.md).  
- Başkalarının, veritabanına erişim izni vermek: [SQL veritabanı kimlik doğrulaması ve yetkilendirme: erişim verme](sql-database-manage-logins.md)  
- Diğer güvenlik duvarı üzerinden bağlanabilir emin olun: [Azure portalını kullanarak bir Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md)  
- Kullanırken **Active Directory - Evrensel MFA ile** kimlik doğrulaması, ADAL izleme kullanılabilir başlayarak olduğunu [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Varsayılan olarak, ADAL izlemeyi kullanarak devre dışı bırakabilirsiniz **Araçları**, **seçenekleri** menüsü altında **Azure Hizmetleri**, **Azure bulut**,  **ADAL çıkış penceresi izleme düzeyini**, izlenen etkinleştirerek **çıkış** içinde **Görünüm** menüsü. İzlemeler çıktı penceresinde seçerken kullanılabilir **Azure Active Directory seçeneği**.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

