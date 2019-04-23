---
title: Azure SQL veri ambarı için kimlik doğrulaması | Microsoft Docs
description: Azure SQL veri ambarı'na, Azure Active Directory (AAD) veya SQL Server kimlik doğrulamasını kullanarak kimlik doğrulaması yapmayı öğrenin.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: a3bed9df5b62ce7f2f3df7046357dc4f2458575c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797044"
---
# <a name="authenticate-to-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için kimlik doğrulaması
Azure SQL veri ambarı'na, Azure Active Directory (AAD) veya SQL Server kimlik doğrulamasını kullanarak kimlik doğrulaması yapmayı öğrenin.

SQL veri ambarı'na bağlanmak için kimlik doğrulama amaçlarıyla güvenlik kimlik bilgilerini geçmesi gerekir. Bağlantı kurulduktan sonra belirli bağlantı ayarları, sorgu oturumu bir parçası olarak yapılandırılır.  

Güvenlik ve veri ambarınıza bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz. [güvenli bir veritabanında SQL veri ambarı][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL kimlik doğrulaması
SQL veri ambarı'na bağlanmak için aşağıdaki bilgileri sağlamanız gerekir:

* Tam SunucuAdı
* SQL kimlik doğrulaması belirtin
* Kullanıcı adı
* Parola
* Varsayılan veritabanı (isteğe bağlı)

Varsayılan olarak, bağlantınızı bağlanır *ana* veritabanı ve değil kullanıcı veritabanınıza. Kullanıcı veritabanınıza bağlanmak için ikisinden birini seçebilirsiniz:

* Varsayılan veritabanı sunucunuz ile SQL Server Nesne Gezgini SSDT, SSMS, ya da uygulama bağlantı dizenizi kaydederken belirtin. Örneğin, bir ODBC bağlantısı InitialCatalog parametresi içerir.
* Kullanıcı veritabanı oturum SSDT'de oluşturmadan önce vurgulayın.

> [!NOTE]
> Transact-SQL deyimini **kullanım Veritabanım;** bir bağlantı için veritabanı değiştirme için desteklenmiyor. SSDT ile SQL Data warehouse'a bağlanma konusunda yönergeler için bkz [Visual Studio ile sorgulama] [ Query with Visual Studio] makalesi.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) kimlik doğrulaması
[Azure Active Directory] [ What is Azure Active Directory] kimlik doğrulaması, bir mekanizma kimlikleri Azure Active Directory (Azure AD) kullanarak Microsoft Azure SQL veri ambarı'na bağlanma. Azure Active Directory kimlik doğrulaması ile veritabanı kullanıcılarının kimliklerini ve diğer Microsoft Hizmetleri tek bir merkezi konumda merkezi olarak yönetebilir. Merkezi kimlik yönetimi, SQL veri ambarı kullanıcıları yönetmek için tek bir yerde sağlar ve izin yönetimini kolaylaştırır. 

### <a name="benefits"></a>Avantajlar
Azure Active Directory avantajları şunlardır:

* SQL Server kimlik doğrulaması için bir alternatif sunar.
* Yardımcı olur, kullanıcı kimliklerini çoğalan veritabanı sunucuları arasında durdurun.
* Tek bir yerde parola dönüşü sağlar.
* Dış (AAD) grupları kullanarak veritabanı izinleri yönetin.
* Parolaları, tümleşik Windows kimlik doğrulaması ve diğer Azure Active Directory tarafından desteklenen kimlik doğrulama etkinleştirerek ortadan kaldırır.
* Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları yer alan kullanır.
* SQL veri ambarı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* Active Directory Evrensel kimlik doğrulaması aracılığıyla destekler multi-Factor authentication da dahil olmak üzere çeşitli araçları için [SQL Server Management Studio](../sql-database/sql-database-ssms-mfa-authentication.md) ve [SQL Server veri Araçları](https://docs.microsoft.com/sql/ssdt/azure-active-directory?toc=/azure/sql-data-warehouse/toc.json).

> [!NOTE]
> Azure Active Directory hala yeni ve bazı sınırlamalar vardır. Azure Active Directory ortamınız için uygun olmasını sağlamak için bkz: [Azure AD özellikleri ve sınırlamaları][Azure AD features and limitations], özellikle ek hususlar.
> 
> 

### <a name="configuration-steps"></a>Yapılandırma adımları
Azure Active Directory kimlik doğrulamasını yapılandırmak için aşağıdaki adımları izleyin.

1. Oluşturma ve Azure Active Directory doldurma
2. İsteğe bağlı: İlişkilendirme veya şu anda Azure aboneliğinizle ilişkili olan active directory değiştirme
3. Azure SQL veri ambarı için Azure Active Directory Yöneticisi oluşturun.
4. İstemci bilgisayarlarınızın yapılandırın
5. Azure AD kimlikleri için eşlenmiş veritabanında bağımsız veritabanı kullanıcılarını oluşturun.
6. Azure AD kimlikleri kullanarak data warehouse'unuza bağlanma

Şu anda Azure Active Directory kullanıcılarını SSDT nesne Gezgini'nde gösterilmez. Geçici çözüm olarak, kullanıcılar görüntülemek [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>Ayrıntılarını bulun
* Adımları yapılandırmak ve Azure Active Directory kimlik doğrulamasını kullanmak için Azure SQL veritabanı ve Azure SQL veri ambarı için neredeyse aynıdır. Konuyu ayrıntılı adımları [SQL veritabanı veya SQL veri ambarı Azure Active Directory kimlik doğrulamasını kullanarak bağlanma](../sql-database/sql-database-aad-authentication.md).
* Özel veritabanı rollerinizi oluşturmak ve rollere kullanıcıları ekleyebilirsiniz. Daha sonra rolleri için ayrıntılı izinler verin. Daha fazla bilgi için [veritabanı altyapısı izinleri ile çalışmaya başlama](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ve diğer uygulamalarla veri Ambarınızı sorgulamaya başlamak için bkz: [Visual Studio ile sorgulama][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]:../active-directory/fundamentals/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
